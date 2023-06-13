---
title: JASS_基本知識
date: 2023-06-13 11:03:17
tags:
  - WE
  - JASS
---

> 使用 Ctrl + F 快速搜尋

# 動機

### 將一些過去的經驗整理出來，有人來問時可以比較有系統的將知識傳遞給對方，近年資源也日漸稀少而且分散做個紀錄也方便大家。

### 完整的編輯器教學請去流連忘返的世界編輯器自學指南[](https://www.wasabistudio.ca/wikis/we/)

# JASS 是甚麼

### JASS 是暴雪開發的一款用於魔獸爭霸創建地圖的腳本語言，提供了非常豐富的 APId

# 為甚麼要使用 JASS

1. 所有 GUI 觸發器本質上都是 JASS，他只是多了視覺化的操作而已
2. 解放限制讓你能完整使用觸發器強大的功能
3. 可以把一些常用功能寫成函數在各個地方使用
4. 你會很常需要使用自訂函數，區域變數等功能使用 GUI 操作上會變得非常複雜
5. 你可以在 vs code 或其他文字編輯器中編輯、管理腳本，函數自動提示語法高量會讓你的生活輕鬆很多
6. 你不會因為崩潰損失未存檔的代碼還原只需要複製貼上

# 全域變數

### 定義方式

#### 觸發器介面中新增變數也是這種方式，腳本中所有 globals 區塊都會被移到腳本最上層，可以定義在任何地方這些變數在整個腳本中都可以讀取到

```
function Init_globals takes nothing returns nothing
  globals
    //在這邊定義全域變數
  endglobals
endfunction
```

# 區域變數

### 使用 local 可以定義只在這個函數存在的變數，有個小重點魔獸爭霸是不會自動回收沒使用的變數的，有繼承 handle 的物件都必須手動銷毀和指向 null，不然在大量執行後會導致記憶體外洩造成崩潰最常見的是點(location)沒有清理。

### 要更改變數資料要使用 set [變數名稱]=[值]

```
function Local takes nothing returns nothing
 local unit u= null
 set u = null
endfunction
```

# 呼叫其他函數(function)

### 在函數中要使用其他函數都必須使用 call，記得注意函數在腳本中的順序

```
function Local takes nothing returns nothing
    call functionName()
endfunction
```

# 觸發器(Trigger)

### 相信開始接觸 Jass 代表你對觸發器已經有基本的概念，觸發器在 GUI 中主要分為三個部分 事件、條件、動作，在編輯->轉化為自訂義腳本中可以發現在 JASS 中觸發器也是分成這三個部分，只是多了一個負責 InitTrigger 的部分。

### 使用 GUI 創建新的觸發實際上執行的工作是這些

1. 新增初始化 function ，在裡面創建新的觸發器(trigger)註冊條件(condition function)、動作(function)、事件(event)，分別是 TriggerAddCondition(trigName, Condition(function funcName) )，TriggerAddAction(trigName,function ActionFuncName)，TriggerRegisterEventFunction
2. 新增條件，動作函數(function)名稱是 Trig\_你的觸發器名稱\_Action\Condition
3. 放到 Jass 腳本中最底下 Init 中呼叫初始化，這邊有個重點 Jass 是自上而下逐行執行的所以放在上面的函數是讀不到下面的函數的

> 註冊是不用照順序的你要先註冊動作還是事件條件都可以

### 這是觸發做的回復魔力技能

```
function Trig_MeditationConditions takes nothing returns boolean
  return ((GetSpellAbilityId() == 'A011'))
endfunction

function Trig_MeditationActions takes nothing returns nothing
  local unit u = GetSpellAbilityUnit()
  local integer level = GetUnitAbilityLevel(u, 'A011')
  local real maxMana = GetUnitStateSwap(UNIT_STATE_MAX_MANA, u)
  local real Mana = GetUnitStateSwap(UNIT_STATE_MANA, u)
  local real recoverMana = Mana + maxMana *(0.2 +(0.05 * level))

  call SetUnitManaBJ( u, recoverMana )
  set u = null

endfunction

//===========================================================================
function InitTrig_Meditation takes nothing returns nothing
  set gg_trg_Meditation = CreateTrigger()

  call TriggerRegisterAnyUnitEventBJ( gg_trg_Meditation, EVENT_PLAYER_UNIT_SPELL_EFFECT )
  call TriggerAddCondition(gg_trg_Meditation, Condition(function Trig_MeditationConditions))
  call TriggerAddAction(gg_trg_Meditation, function Trig_MeditationActions)
endfunction
```
