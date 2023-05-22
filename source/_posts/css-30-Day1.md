---
title: Animation30 Day1
date: 2020-09-18 14:12:42
cover: https://source.unsplash.com/random
tags:
  - css
  - animation

---

## 用到了

- TweenMaxTweenMax
- TimelineMax
- ~~ScrollMagicScrollMagic~~ (有更好的被取代了)
- GSAP

## 動機

###### 最近突然覺得自己炫泡動畫功力真的該加強一下了，之前都不怎麼在意外觀只想要把功能弄出來 外觀能看就好 ，不過還是該學一下怎麼弄複雜的動畫還有視覺設計

# GreenSock

###### 這個真的非常好用，用了這個不用再管關鍵貞還有時間線可以專心在設計動畫，而且還有 Scroll Trigger ，這是最近出的功能之前都還要搭配另一個叫 Scroll Magic 的套件，總之非常推

#### 只需要使用 cdn 體驗一下就知道有多簡單

```
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.5.1/gsap.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.5.1/ScrollTrigger.min.js"></script>
```

#### JS

###### 官網提供最簡單的範例

```cs
//當box進入到畫面的時候往右邊滑

gsap.to('#box',{x:200})
```

###### 比較進階 可以自己設定很多屬性

```cs
// 設定觸發元件還有觸發位置，其他還提供非常多屬性可以設定 功能很多
let tl = gsap.timeline({scrollTrigger:{
trigger:"#div",
start:'top top',
scrub:1

}});


//從右側滑回來
tl.form('.box',{x:200})
```

---



# 總結

###### 一下就能上手感覺還有很多花樣可以研究，明天繼續!
