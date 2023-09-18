---
using System;
public static class Kata
{
  // return masked string
  public static string Maskify(string cc)
  {
    string ans ="";
    if(cc.Length < 5)　return cc;
    for(int  i  = cc.Length-1  ; i > -1 ; i--){
      if(i < cc.Length - 4){
          ans+="#";
      }else{
          ans+=cc[i];
      }
  
    }
  
    return Reverse(ans);
  }
  public static string Reverse( string s )
{
    char[] charArray = s.ToCharArray();
    Array.Reverse(charArray);
    return new string(charArray);
}
}
---
# 題目

把字串除了最後四個字元以外的字用#遮蔽

# 思路

從後面開始處裡字串並填入新的變數裡返回

# 程式碼

```csharp
using System;
public static class Kata
{
  // return masked string
  public static string Maskify(string cc)
  {
    string ans ="";
    if(cc.Length < 5)　return cc;
    for(int  i  = cc.Length-1  ; i > -1 ; i--){
      if(i < cc.Length - 4){
          ans+="#";
      }else{
          ans+=cc[i];
      }
  
    }
  
    return Reverse(ans);
  }
  public static string Reverse( string s )
{
    char[] charArray = s.ToCharArray();
    Array.Reverse(charArray);
    return new string(charArray);
}
}
```

# 改進的地方

使用Substring切下後四個字元在使用PadLeft填充#返回

```csharp
public static class Kata
{
  // return masked string
  public static string Maskify(string cc)
  {
    int len = cc.Length;
    if (len <=4)
      return cc;
  
    return cc.Substring(len-4).PadLeft(len, '#');
  }
}
```
