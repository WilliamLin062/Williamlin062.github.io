---
title: Kata-C-Sharp-ListFiltering
date: 2023-09-18 10:35:08
tags:
---

# 題目

過濾列表中不是整數的物件

# 思路

使用 Enumerable.OfType 過濾

# 程式碼

```csharp
using System.Collections;
using System.Collections.Generic;
using System.Linq;
public class ListFilterer
{
   public static IEnumerable<int> GetIntegersFromList(List<object> listOfItems)
   {
     return listOfItems.OfType<int>();
   }
}
```

# 改進的地方

使用自訂的迭帶器這樣在輸入很大的時候效率會好一點
