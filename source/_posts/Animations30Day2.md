---
title: Animations30 Day2
date: 2020-09-21 15:42:47
cover: https://source.unsplash.com/random/800x500
tags:
  - pixijs
---

## 用到了

- Pixi.js
- GSAP

---

# PIXI.JS

##### pixi.js 是專門用來做 2D 動畫的渲染庫，速度快，體積小 有了他可以不用深入研究 webGL 而且瀏覽器相容性問題他都幫你處理好了。

# 起手式

##### 導入 cdn

```cs
<script src="https://cdnjs.cloudflare.com/ajax/libs/pixi.js/5.0.2/pixi.min.js"></script>
```

##### 然後開始

```cs
//首先我們要做的是建立一個新的PIXI容器
const app = new PIXI.Application({
    width:window.innerWidth,
    height:window.innerHeight,
//其他還有很多屬性
    view
    backgroundColor
    ...
})
// 把容器掛上去
doucument.body.appendChild(app.view)

//建立貼圖
// 先導入圖片
const textTrue = PIXI.Texture.from('./asset/example.png')

// 使用圖片創建紋理

let player = new PIXI.Sprite(textTrue)

//這時候應該你的圖片會顯示在動畫容器裡面
//設定一下圖片位置
player.x = app.view.width / 2
player.y = app.view.height / 2

//簡單的旋轉動畫
function animation (){
  player.rotation += 0.1
}
// ticker是用來更新畫面，PIXI都幫你處理好底下的一些邏輯，以往都要自己寫
app.ticker.add(animation)

//這時候他應該會開始旋轉
```

# 總結

##### 今天原本是打算用 gsap + pixi 做一個滾動的故事動畫可是要研究的東西比我想像中的還要多，PIXI 是一個很強大的動畫庫，而且很有趣
