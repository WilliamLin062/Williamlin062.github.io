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

# Xamarin 根元素屬性

## 命名空間 xmlns

### 根據官方文件說明

> _XAML 使用 xmlns XML 屬性進行命名空間聲明。本文介紹 XAML 命名空間語法，並演示如何聲明 XAML 命名空間以訪問類型。_

```xml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views ="clr-namespace:Test01"
       xmlns:local ="clr-namespace:Test01.View"
       x:Class="Test01.MainShell"
       Title="華信APP"
       >
```

## 說明

```csharp
xmlns="http://xamarin.com/schemas/2014/forms"
//第一個命名空間 將整個XAML命名空間應設為默認命名空間
//WPF程序集會將比如System.Windows System.Windows.Controls映射到這類命名空間
```

```####csharp
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
 //第二個命名空間會用xmlns:x ，將映射單獨的命名空間至x:前墜
```

```##csharp
xmlns:views ="clr-namespace:Test01" //映射到自定義類和程序集views
//clr-namespace:在程序集中聲明的 CLR 命名空間，其中包含要公開為元素的公共類型。
```

### 自訂命名空間 SDKSample

```csharp
namespace SDKSample {
    public class ExampleClass : ContentControl {
        public ExampleClass() {
        ...
        }
    }
}
```

### 在跟標記中使用帶前綴的引用 xmlns:custom 使 ExampleClass 在 UI 中進行實例化

```xml
<Page x:Class="WPFApplication1.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:custom="clr-namespace:SDKSample;assembly=SDKSampleLibrary">
  ...
  <custom:ExampleClass/>
...
</Page>
```

### 映射到當前程序集

`assemblyclr-namespace`如果引用的對像是在與引用自定義類的應用程序代碼相同的程序集中定義的，則可以省略。或者，這種情況的等效語法是指定`assembly=`，等號後面沒有字符串標記。

如果在同一程序集中定義自定義類，則不能將其用作頁面的根元素。部分類不需要映射；如果您打算在 XAML 中將它們引用為元素，則僅需要映射應用程序中頁面的分部類以外的類。

# Shell 應用程式

在 Xamarin 4.0 中新增的功能不同於一般的 Page, Shell 是一種基於 URI 的導航方式本身的功能只負責導航，大部分應用程式常見的導航方式都可以使用 Shell 實現，要使用必須在 App.cs 中將首頁設為 Shell 頁面

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

FlyoutDisplayOptions:設定有多少控件在 FlyoutItem 中時將標籤移至手風情選單中，莫認為 AsSingleItem

#### 範例

##### 以下 xaml 手風琴選單中會有

1. 首頁
2. 我的帳戶
3. 登入

##### 首頁中會有三個子頁面導航標籤

1. 專欄文章
2. 熱門文章
3. 全球焦點

##### 其他 Tab 以此類推

```csharp
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       x:Class="MyApp.AppShell">
   <FlyoutItem Title="首頁">
        <Tab Title="首頁">
            <ShellContent Title="專欄文章" ContentTemplate="{DataTemplate views:FrontPage}" />
            <ShellContent Title="熱門文章" ContentTemplate="{DataTemplate views:MessageList}" />
            <ShellContent Title="全球焦點" ContentTemplate="{DataTemplate views:Login}" />
        </Tab>
        <Tab Title="財富管理">
            <ShellContent Title="全權委託" ContentTemplate="{DataTemplate views:FrontPage}" />
            <ShellContent Title="AI人工智慧財" ContentTemplate="{DataTemplate views:Login}" />
        </Tab>
        <Tab Title="關於我們">
            <ShellContent Title="公司簡介" ContentTemplate="{DataTemplate views:FrontPage}" />
            <ShellContent Title="經營理念" ContentTemplate="{DataTemplate views:Login}" />
            <ShellContent Title="服務項目" ContentTemplate="{DataTemplate views:Login}" />
        </Tab>
    </FlyoutItem>
    <FlyoutItem  FlyoutDisplayOptions="AsMultipleItems">
        <ShellContent Title="我的帳戶" ContentTemplate="{DataTemplate views:FrontPage}" />
        <ShellContent Title="登入" ContentTemplate="{DataTemplate views:Login}" />
    </FlyoutItem>
</Shell>
```

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

過去自身 APP 開發都是使用 ReactNative 這種基於 Web 技術的混合式解決方案或者 Native,目前覺得大致上差異在使用人數上，xamarin 的參考有點少官方文件也沒寫很明白，學習上有點小卡
