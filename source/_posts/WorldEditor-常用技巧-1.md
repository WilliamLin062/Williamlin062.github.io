---
title: WorldEditor常用技巧 1
date: 2023-06-13 08:28:57
tags: 
  - WE
  - JASS
---
# 前言

## WorldEditor是魔獸爭霸自帶的地圖編輯器，其功能強大讓魔獸爭霸這款有二十年歷史的遊戲依然活耀。

### 會寫這篇是因為中文參考資源日漸稀少，魔獸爭霸中文圈生態近年也是改變很多，中文玩家以及作者、翻譯者都流向中國大型平台，平台支援的各種API也讓地圖不像以前一樣流通，一般版本的編輯器資訊也就變得很稀少並且破碎讓新手難以入門，在這邊整理一些常見的技巧做個紀錄也供大家參考。

# 自訂資料

### 在單位編輯器中有一個欄位是自訂義資料，可以在裡面放一些數據在編輯器中讀取，可以用hashtable取代

# 假人(dummy、馬甲)

### 假人是非常廣泛用到的一種技巧，比如單位重生、自訂義投射物模型、多重施法、科技檢測等都會用到，他本身就只是一個普通單位通常會用苦工當模板並且使用以下設定

#### **普通技能**

* 中立無敵
* 蝗蟲
* 隱形

#### 其他設定

* 移動速度: 0
* 移動:無
* 碰撞體積: 0
* 視野: 0
* 攻擊: 無
* 不可逃跑
* 生命回復: -1

### 實際應用 自訂義投射物

#### 以下來源是我自己做地圖時寫出來的通用系統全使用Jass，只要設定好都是可以用的，不懂Jass的小夥伴不用擔心之後會寫一篇簡易的入門文章，以下會分兩個觸發器工作分別是設定創建投射物單位、以及讓他往目標移動

### 創建單位

#### CreateMissilesLoc(投射物擁有玩家、假人單位類型ID、速度、傷害範圍、開始位置、目標位置、拋物線參數、傷害、攻擊類型、傷害類型、目標特效) 回傳 單位

#### 這邊的工作很簡單，計算兩點角度還有創造假人然後暫停單位，暫停單位很重要你也不希望單位做出一些你不希望他做的事情，然後關閉移動路徑讓單位移動無視地形，添加德魯伊的變化技能之後移除，整理還有詳細說明一下

1. AngleBetweenPoints(startLoc, endLoc): 內建的功能函數，會計算兩個點之間的角度回傳是一個180到-180的real數字
2. CreateUnitAtLoc(player, UnitType, location , faceAngle ):在目標點創造單位，一定會用到的函數至於跟他長得差不多的親戚有什麼不同之後再介紹
3. PauseUnit(unit, true):暫停單位
4. UnitAddAbilityBJ( interger ablitedID, unit): 單位添加技能，這邊的重點在'Arav'這是翔風德魯伊變形成烏鴉型態的技能ID，添加這個技能再移除可以讓單位改變飛行高度，是一個BUG但是非常有用也就沒改了
5. SaveUnitHandle、SaveInterger ... :都是儲存值到HashTable
6. GroupAddUnit(group , unit):　添加單位到單位組

```
//==============================================================================
//指向導彈
function CreateMissilesLoc takes player p, unit attacker, integer UnitType, real speed, real radius, location start, location end , real earc , real damage, attacktype type1 , damagetype type2 , string targetLocEffect returns unit

  local real angle = AngleBetweenPoints(start, end)
  local unit vest = CreateUnitAtLoc(p, UnitType, start , angle )
  local integer handleId = GetHandleId(vest)

  call PauseUnit(vest, true)
  call SetUnitPathing( vest, false )
  call UnitAddAbilityBJ( 'Arav', vest)
  call UnitRemoveAbilityBJ( 'Arav', vest)


  //----------------------------------------------------------
  call SaveUnitHandle(Missiles_loc_table, handleId, 0, vest)
  call SaveReal(Missiles_loc_table, handleId, 1, damage)
  call SaveLocationHandle(Missiles_loc_table, handleId, 2, end)
  call SaveUnitHandle(Missiles_loc_table, handleId, 3, attacker)
  call SaveReal(Missiles_loc_table, handleId, 4, earc)
  call SaveReal(Missiles_loc_table, handleId, 5, radius)
  call SaveReal(Missiles_loc_table, handleId, 6, speed)
  call SaveStr(Missiles_loc_table, handleId, 7, targetLocEffect)
  call SaveLocationHandle(Missiles_loc_table, handleId, 8, start)

  call GroupAddUnit(Missiles_Unit_Group, vest)
 
  return vest
endfunction
```



### 移動還有命中

#### 這邊看起來很複雜實際上他只有做一個ForGroup跟投射物到目標後的後續動作，詳細說明一下

1. ForGroup(group, function functionHandle):GUI裡面對組做動作實際上在Jass中就是長這樣，會需要一個函數(function)去對組中的物件做操作，因為無法傳遞參數在操作函數中只能讀取到觸發器中事件響應物件，這造成一些資料讀取的困難但使用一些技巧還是可以克服的，這邊就使用unithandle將資料都存取在hashtable中，ForGroup時可以獲得unithandle讀取到我們想要的資料
2. DistanceBetweenPoints(loc1,loc2): 兩點間的距離，返回一個real
3. PolarProjectionBJ(loc, distance, angle): 極座標點位移，實用而且常用的函數，將點朝一個角度位移你要的距離

#### 整段做的動作是每0.05秒將投射物單位朝目標點位移，期間計算拋物線高度到目標點後殺死目標並且加入到單位組中實現後續對範圍內目標傷害的機制，這整個系統難點都是資料不好傳遞內建的傷害範圍目標是不分敵我所以無法使用，必須用這種方式來實現

```
function MissilesLoc_add_group takes nothing returns nothing
  local integer handleId = GetHandleId(Missiles_OnHitUnit)
  local unit u = GetEnumUnit()
  local unit attacker = LoadUnitHandle(Missiles_loc_table, handleId, 3)
  local integer target_handleId = GetHandleId(u)

  local real dmg = LoadReal(Missiles_loc_table, handleId, 1)

  call GroupAddUnit(Missiles_Damage_Group, u)
  call SaveUnitHandle(Missiles_Damage_table, target_handleId, 0, attacker)
  call SaveReal(Missiles_Damage_table, target_handleId, 1, dmg)
  
  set u = null
endfunction


function MissilesLoc_ForGroup takes nothing returns nothing
  local unit u = GetEnumUnit()
  local integer handleId = GetHandleId(u)
  local location target = LoadLocationHandle(Missiles_loc_table, handleId, 2)
  local location loc = GetUnitLoc(u)
  local location startLoc = MissilesGetStartLoc(handleId)
  local real angle = AngleBetweenPoints(loc, target)
  local real distance = DistanceBetweenPoints(loc, target)
  local real x = GetLocationX(loc)
  local real y = GetLocationY(loc)
  local real erac = LoadReal(Missiles_loc_table, handleId, 4)
  local real radius = LoadReal(Missiles_loc_table, handleId, 5)
  local real speed = LoadReal(Missiles_loc_table, handleId, 6)

  local string effectName = LoadStr(Missiles_loc_table, handleId, 7)
  //
  local location nextLoc = PolarProjectionBJ(loc, speed / 20, angle)

  local real hight = util_GetParabolaHeight(u, startLoc , target, erac)
  local group g = null

  local effect e = null
  if IsUnitDeadBJ(u) == true then 
    call FlushChildHashtable(Missiles_loc_table, handleId)
    call GroupRemoveUnit(Missiles_Unit_Group, u)
    call RemoveLocation(target)
  endif
  if IsUnitInRangeLoc(u, target, 80) then
    set g = GetUnitsInRangeOfLocMatching(radius, loc, Condition(function util_isEnemyfilter ))
    set e = AddSpecialEffectLoc( effectName, loc)
  
    set Missiles_OnHitUnit = u
    call ForGroup(g, function MissilesLoc_add_group )

    //---------------------------------------------------------
    call DestroyEffect(e)
    call KillUnit(u)
  else
    call SetUnitPositionLocFacingBJ( u, nextLoc , bj_UNIT_FACING )
  endif


  call SetUnitPositionLocFacingBJ( u, nextLoc , bj_UNIT_FACING )
  call SetUnitFlyHeight(u, hight  , 99999)
  //=====================================================================
  call RemoveLocation(loc)
  call RemoveLocation(nextLoc)
  call DestroyGroup(g)
  set loc = null
  set nextLoc = null
  set g = null
  set e = null
  set u = null
endfunction
function Trig_MissilesLocActionActions takes nothing returns nothing
 
  call ForGroup(Missiles_Unit_Group, function MissilesLoc_ForGroup )

endfunction 

//===========================================================================
function InitTrig_MissilesLocAction takes nothing returns nothing
  set gg_trg_MissilesLocAction = CreateTrigger()

  call TriggerRegisterTimerEventPeriodic( gg_trg_MissilesLocAction, 0.05 )
  call TriggerAddAction(gg_trg_MissilesLocAction, function Trig_MissilesLocActionActions)
endfunction

```
