---
title: Kata-C-Sharp-MultiplesOf3or5
date: 2023-09-18 10:34:43
tags:
---
# 題目

找出1~輸入數字區間中3和5的倍數加起來返回

# 思路

循環一次數字區間檢查是不是3或5的倍數

# 程式碼

```csharp
public static class Kata
{
  public static int Solution(int value)
  {
    int ans = 0;
    if(value <= 0 ) return 0;
    for(int i = 1; i < value ; i++){
      if(i % 3 == 0 || i % 5 == 0){
        ans += i;
      }
    }
    return ans ;
  }
}
```

# 改進的地方
