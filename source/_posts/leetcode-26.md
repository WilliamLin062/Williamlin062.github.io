---
title: leetcode_26 RemoveDuplicates
date: 2023-06-29 10:09:02
tags:
 - C#
 - LeetCode
---
```csharp
public class Solution {
    public int RemoveDuplicates(int[] nums) {
            int uniqueCount =  nums.Distinct().Count(); //不重複數量
            int[] uniqueNumbers = new int[uniqueCount]; //建立陣列裝不重複數字
            Array.Copy( nums.Distinct().ToArray(), uniqueNumbers, uniqueCount);
            //複製nums的不重複元素至剛剛建立的陣列
            for (int i = 0; i < uniqueNumbers.Length; i++)
            {
                nums[i] = uniqueNumbers[i];//取代
            }

            return uniqueCount;
    }
}
```

因為題目說前面開頭k個數字跟他需要的數字一樣就可以所以不用多餘的數字
