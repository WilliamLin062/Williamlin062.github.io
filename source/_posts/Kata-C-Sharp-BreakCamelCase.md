---
title: Kata-C-Sharp-BreakCamelCase
date: 2023-09-18 10:31:52
tags:
---

# 題目

駝峰命名法切割

# 思路

建立一個StringBuilder在遍歷字串的時候加入至StringBuilder並判斷是否為大寫，是的話在前面插入空格

# 程式碼

```csharp
using System;
using System.Text;
public class Kata
{
  public static string BreakCamelCase(string str)
  {
    StringBuilder sb = new();
    foreach(char c in str ){

      if(Char.IsUpper(c)) sb.Append(' ');
      sb.Append(c);
    }
    return  sb.ToString();
  }
}
```

# 改進的地方
