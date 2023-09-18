---
title: Kata-C-Sharp-DuplicateEncoder
date: 2023-09-14 09:29:11
tags:
---
# 題目

檢查輸入字串有沒有重複符號，如果出現次數超過一次用`)`表示沒有則使用`(`

比如:

```
"din"      =>  "((("
"recede"   =>  "()()()"
"Success"  =>  ")())())"
"(( @"     =>  "))((" 
```

# 思路

遍歷字串紀錄出現次數，再依題目規則返回新的字串

# 程式碼

```csharp
using System.Linq;
public class Kata
{
  public static string DuplicateEncode(string word)
  {
      string ans = "";
      foreach (char s in word)
      {
          int count = word.Where(w => char.ToLower(w) == char.ToLower(s)).Count();
          if (count > 1) ans += @")";
          else ans += @"(";
      }
      return ans;
  }
}
```

# 改進方法

使用字典紀錄出現次數而不是使用Linq每個循環都查一遍

使用String Builder 因為字串操作次數有可能會很大使用string會帶來額外的性能開銷
