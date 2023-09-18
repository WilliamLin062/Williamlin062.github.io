---
title: Kata-C-Sharp-PlayingWithDigits
date: 2023-09-14 10:32:19
tags:
---
# 題目

檢測輸入數字是否滿足以下特性

比如:

`89 --> 81 + 92 = 89 * 1`

`695 --> 6² + 9³ + 5⁴= 1390 = 695 * 2`

```
digPow(89, 1) should return 1 since 8¹ + 9² = 89 = 89 * 1
digPow(92, 1) should return -1 since there is no k such as 9¹ + 2² equals 92 * k
digPow(695, 2) should return 2 since 6² + 9³ + 5⁴= 1390 = 695 * 2
digPow(46288, 3) should return 51 since 4³ + 6⁴+ 2⁵ + 8⁶ + 8⁷ = 2360688 = 46288 * 51
```

# 思路

將數字轉為字串後遍歷對各個數字做`a ^ p + b ^ (p+1) + c ^(p+2) + d ^ (p+3) + ...)` 計算後加總起來，檢測是不是n的倍數

# 程式碼

```csharp
using System;

public class DigPow {
	public static long digPow(int n, int p) {
    int numberLength = Convert.ToString(n).Length;
    int roughNumber = 0;
    string numbers = Convert.ToString(n);
    for (int i = 0, j = 0; i < numberLength; i++, j++)
        {
            int dig = Convert.ToInt32(numbers[i] - '0');
            roughNumber += (int)Math.Pow(dig, p + i);
        }

     return roughNumber % n == 0 ? roughNumber / n : -1;
	}
}
```

# 改進方法

使用數學方法取得各個位數不是使用字串轉換操作，減少轉換的性能開銷

可以使用字典紀錄數字減少一些重複計算的性能問題
