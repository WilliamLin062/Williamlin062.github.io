---
title: AspMvc路由至靜態Html
date: 2023-06-08 16:43:39
tags:
---

```csharp
 public ActionResult MyHtml(
 {
      var result = new FilePathResult($"~/Views/{htmlPageName}.html", "text/html");
      return result;
 }
```
