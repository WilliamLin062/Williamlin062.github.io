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

### CROS
