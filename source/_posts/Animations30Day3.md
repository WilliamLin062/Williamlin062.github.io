---
title: Animations30 Day3
date: 2020-09-23 10:10:31
cover: https://source.unsplash.com/random/500×500/?code
tags:
  - pixijs
---
## 整理一下目前我對pixi的了解

* container   (放圖形的容器可以有很多個，可以用來分組)
* stage         (是在Application底下的成員，根畫面容器，跟他的意思一樣就是舞台)
* loader       (預加載器 渲染需要先把素材轉換成WebGL GPU能使用的圖片，就是紋理)
* ticker         (Application的成員 ,用來更新畫面)
* application (建立PIXI動畫的class，會自動產生 ticker  ,renderer ,root container)

### 大概的藍圖

1. 載入緩存素材
2. 初始化
3. ticker更新
4. 其他邏輯

## 研究文件

待續
