---
title: Asp.NET WebForm前端傳遞資料到C#
date: 2023-05-23 14:43:01 
tags: Asp.NET
---
# 使用 HiddenField

##### 這是 ASP 的 HiddenField 控件

###### 屬性

- ID:ASP 的控件都會需要 ID 這會在 designer.aspx.cs 中使用 ID 名稱自動建立控件提供 aspx.cs 檔案操作控件
- runat:標示為伺服器端控件讓伺服器端可以直接訪問
- Value:可以填一些字串資料在裡面，hiddenField 會用 Post 方法回傳至伺服器端

```echarts
<asp:HiddenField ID="HiddenData" runat="server" Value="" />
```

##### 使用 JavaScript 填入資料

```javascript
//可以用ID訪問元素
var hiddenField = document.querySelector('#HiddenData')
```

```###javascript
//跟一般的元素屬性操作一樣，指定vlaue為想要的字串就可以了
hiddenField.value ="我很隱密"
```

##### 缺點

###### 不能放一些敏感的資訊比如 Token 之類的因為可以直接在前端看到並且修改，真的要傳遞資料還是乖乖 CallAPI
