---
title: Kata-C-Sharp-IsThisATriangle
date: 2023-09-18 10:32:50
tags:
---

# 題目

給三個長度判斷能不能組成三角形

# 思路

三角形的特性

1. 任意兩邊加其他會大於第三邊

只需要判斷有沒有滿足這個特性還有是否為負數長度

# 程式碼

```csharp
using System;
public class Triangle
{
    public static bool IsTriangle(int a, int b, int c)
    {
       int [] arr = {a,b,c};
       Array.Sort(arr);
       if( arr[0]+arr[1] <= arr[2])  return false;
       for(int i  = 0 ; i<arr.Length ; i++){
          if( arr[i] < 0)  return false;
        }
        return true;
    }
}
```

# 改進的地方

可以更簡短只需要在返回時判斷有沒有符合特性就好
