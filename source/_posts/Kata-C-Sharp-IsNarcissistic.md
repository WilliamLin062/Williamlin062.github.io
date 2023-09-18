---
title: CodeWars C# Does my number look big in this?水仙花數(Narcissistic)
date: 2023-09-13 08:52:40
tags: 
    -CodeWars
    -C#
    -解題
    -Narcissistic
    -數學問題
---

# 題目

檢測輸入數字是否是一個`自戀數`

比如:

`153 = 1^3 + 5^3 + 3^3`

`1938 = 1^4 + 9^4 + 3^4 + 8^4`

# 思路

##### 1.將數字轉字串再遍歷一遍對數字處理 (暴力解法)

優點:簡單實現

可以改進的地方: 將數值儲存起來避免重複使用Pow計算、直接使用數學方法取得每個數字

# 程式碼

```csharp
public class Kata
        {
            public static bool Narcissistic(int value)
            {
                char[] valString = Convert.ToString(value).ToCharArray();
                int len = valString.Length;
                long sum = 0;
                foreach( char digitChar in valString){
                  int digitValue = digitChar - '0';
                  sum += (long)Math.Pow(digitValue,len);
                }
                return sum == value;
            }
        }
```

# 看到更好的寫法

```csharp
using System;
using System.Linq;

public class Kata
{
  public static bool Narcissistic(int value)
  {
    var str = value.ToString();
    return str.Sum(c => Math.Pow(Convert.ToInt16(c.ToString()), str.Length)) == value;
  }
}
```

