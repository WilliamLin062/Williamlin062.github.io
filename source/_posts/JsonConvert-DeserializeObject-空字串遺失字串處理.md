---
title: JsonConvert.DeserializeObject()的空字串遺失字串處理
date: 2023-06-06 14:31:06
tags: C#
---

# 設定 JsonSerializerSettings

```csharp
           var settings = new JsonSerializerSettings
                {
                    NullValueHandling = NullValueHandling.Ignore,
                    MissingMemberHandling = MissingMemberHandling.Ignore
                };
```
