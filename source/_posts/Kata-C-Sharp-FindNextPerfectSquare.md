---
title: Kata-C-Sharp-FindNextPerfectSquare
date: 2023-09-15 11:08:27
tags:
---

# 題目

找到下一個平方數

比如:

`121 = 11 * 11 需要回傳 144 = 12 * 12`

# 思路

##### 1.找出平方根在加一返回

此題關鍵是需要超高精度平方根運算方式 c#內建 Sqrt 的 double 精度不足在 11 位數以上會有細微偏差導致失敗

找出平方根有幾種方法:

1. 牛顿迭帶法
2. 巴比倫方法

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
