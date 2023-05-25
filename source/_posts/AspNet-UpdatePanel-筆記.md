---
title: Asp.Net UpdatePanel筆記
date: 2023-05-25 17:07:20
tags: ASP.Net
---

# UpdatePanel

#### UpdatePanel 是 Asp.net WebForm 的一個控件，功能是讓區塊內的元素能實現 ajax 讓 post 時不再刷新整個頁面只單獨更新區塊內的控件，使用方式

```
<asp:UpdatePanel id="controlID" runat="server" UpdateMode="Always" class="">
       <ContentTemplate>
           //需要更新的物件放在這
       </ContentTemplate>
       <Triggers>
          //監聽的控件事件，事件觸發時會呼叫UpdatePanel的更新方法對Panel做更新
          <asp:AsyncPostBackTrigger ControlID="controlID" EventName="eventName" />
       </Triggers>
 </asp:UpdatePanel>
```

##### UpdatePanel 能獲取內部第一層的元素，如果使用 ItemListView 這種底下又有 Template 的控件，那必須 ItemTemplate 中再套一層 UpdatePanel
