---
title: xamarin學習筆記
date: 2023-06-17 09:26:21
tags:
  - xamarin
  - C#
  - .Net
---

# 動機

### 因應公司有項目要使用 Xamarin 開發要做準備

# Shell 應用程式

### 不同於一般的 Page，Shell 是一種基於 URI 的導航方式本身的功能只負責導航，大部分應用程式常見的導航方式都可以使用 Shell 實現，要使用必須在 App.cs 中將首頁設為 Shell 頁面

## 繼承 Shell 的子類

```csharp
public partial class MainShell : Shell{
    public MainShell(){
           InitializeComponent();
    }
}
```

## Xaml

```xml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       x:Class="MyApp.AppShell">
   <!---控件放這--->
</Shell>
```

## App 設定入口頁面

```csharp
public partial class App : Application{
       public App()
        {
            InitializeComponent();
            MainPage = new MainShell();
        }
}
```

## 導航版面設計

#### Shell 的子控件分為三大類

1. TabBar 或 FlyoutItem: 底部標籤，側邊手風琴彈出視窗，兩者同時只能使用一個若是出現在同一個 Shell 會使用最上面那個為主另一個不顯示
2. Tab:用來幫 ShellContent 分組
3. ShellContent:每個導航選項卡的頁面，如果 Tab 外出現多個 ShellContent 則會出現在底部標籤列

#### FlyoutItem 屬性

##### FlyoutDisplayOptions:設定有多少控件在 FlyoutItem 中時將標籤移至手風情選單中，莫認為 AsSingleItem

# Application.Resources

### 程式碼

```xml
<?xml version="1.0" encoding="utf-8" ?>
<Application xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Test01.App">
    <Application.Resources>

    </Application.Resources>
</Application>
```

### 說明

# 心得

### 過去自身 APP 開發都是使用 ReactNative 這種基於 Web 技術的混合式解決方案或者 Native,目前覺得大致上差異在使用人數上，xamarin 的參考有點少官方文件也沒寫很明白，學習上有點小卡
