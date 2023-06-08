---
title: React + Firebase 即時聊天系統
categories:
  - React
tags:
  - react
  - firebase
cover: https://source.unsplash.com/random/500×500/?code
---
## 使用的有

- FireBase realtime database , functions , Cloud FireStore
- Post man
- node 8 (因為免費專案只能用 node 8 更高的版本要升級帳戶才能使用 cloude function，node 8 只支援到 2021 年)
- express

###### 一開始是用 react native 做因為這原本是某個案子的 demo 有簽保密協議只好重寫成網頁版，網頁版正在慢慢補上所以先講 firebase 的部分就好，

###### firebase 在設計資料結構的時候要注意

- 結構不能太深 ，平面化
- 鍵值沒指定的話都是自動產生的亂數 document id，你要自己在文件內搞一個 id 來撈你要的資料 比如 uid，在建立使用者的時候會建立的一個獨一無二字串來把各個集合內的資料串起來

## Storage 的資料設計差不多是這樣

```json
"userHandle":{
    ...
    userID: "uid"
}
"roomID":{
     ...
     member:[0:uid , 1:uid]
}

```

###### 在建立使用者之後會在 user collection 裡面建立 user 資料，裡面會儲存使用者的資訊比如大頭貼，email 等等

###### roomID 是建立聊天室之後回傳的 doc ID，然後 room 裡面的 member 陣列記錄聊天室內人的 uid，

###### 後端 api 只需要有 uid 就可以撈出這些資料，整理一下丟回去

......後續慢慢補上先完成 web 板好了
