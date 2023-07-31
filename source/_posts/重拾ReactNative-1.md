---
title: 重拾ReactNative(一)
date: 2023-07-14 15:13:44
tags:
  - ReactNative
---
# 前言

最近主管終於放棄 Xamarin 改使用 ReactNative 開發 APP，作為提出建議的人這個任務最後也落到我頭上來了，記憶中最後一次使用 RN 已經是三年多前的事情了很多東西都不同了，於是只好在開發的過程中慢慢重新學習 ReactNative 還有其他的套件

# 安裝

經過思考過後決定使用 expo，原因如下:

- 簡化開發流程
- 避免不必要的問題
- 速度還有方便性

其餘主要使用的套件有:

- React Navigation
- Redux
- React Native Paper
- React Native Render HTML
- React Native Vector Icon

另外為了避免 JavaScript 各種神奇的問題，還有挑戰自我這次使用 TypeScript 做為開發語言

# 過程中遇到的各種問題

### TypeScript 型別

TypeScript 作為 JavaScript 的超集提供了原本沒有的型別檢查功能，剛開始使用的時候在寫第一個 Component 的時候直接撞牆，這是一個類組建在 JavaScript 中是沒有問題的

```js
class App extends Component {
  constrostor(props) {}
  state: MyState = {
    count: 0,
  }
  render() {
    return (
      <div>
        {this.props.message} {this.state.count}
      </div>
    )
  }
}
```

但是在 TypeScript 中還要先定義他的參數、State 的型別不然 Ts 會報錯

### Redux Toolkit

需要一個東西來管理全域都會使用到的資料比如使用者資訊，有考慮過三種方法

1. 使用global
2. RN的Context
3. Redux

第一種感覺不是很好，畢竟資料一多一定會導致載入時間便很長而且難以維護，Context API 因為一開始都是用 Class Component寫的所以沒使用，第三種Redux 就是設定有點麻煩但是最後還是選他，但這次想試試看新的Redux toolkit

#### Redux Toolkit

> Redux **Toolkit**包旨在成為編寫[Redux](https://redux.js.org/)邏輯的標準方法。它最初是為了幫助解決有關 Redux 的三個常見問題而創建的：
>
> * “配置 Redux 存儲太複雜”
> * “我必須添加很多包才能讓 Redux 做任何有用的事情”
> * “Redux 需要太多樣板代碼”
>
> 我們無法解決每個用例，但我們可以嘗試提供一些工具來抽象設置過程並處理最常見的用例，並包含一些有用的實用程序，讓用戶簡化他們的操作應用程序代碼。

幾年前用Redux最大的問題就是有很多的配置還有套件要一起手動裝上 裝完Redux核心還要裝上Saga 、thunk... 裝完眼也花了還發現沒成功又要繼續翻文件 雖然最後還是弄好了但是安裝的使用體驗實在是很不好，官方並沒有提供如同[`create-react-app`](https://github.com/facebook/create-react-app) 這種一鍵安裝、開箱即用的工具，Redux Toolkit的出現解決了這三種問題讓配置過程簡單快速大大的減少使用上的挫敗感，而且 reducer action 也簡化成一個個slice 讓程式看起來更簡潔，缺點就是官方並沒有在文件說明怎麼跟Class component一起使用，網路上是說可以用connect但我是用 function component 將類組件包起來後將資料傳遞過去解決
