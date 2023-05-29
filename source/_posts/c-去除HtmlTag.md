---
title: c#去除HtmlTag
date: 2023-05-26 16:19:30
tags:
---
# 使用正則表達式 RegularExpression

```csharp
 public static string StripHTML(string input)
 {
   return Regex.Replace(input, "<[a-zA-Z/].*?>", String.Empty);
 }
```
