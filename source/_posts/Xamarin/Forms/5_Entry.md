---
title: Xamarin.Forms 创建移动应用程序的基础知识 5 - Entry
date: 2021-08-19 09:23:00
categories:
	- 学习笔记
tags: 
	- .NetCore
	- Xamarin
---

## 简介

演示 Entry 属性 。

1.  在 XAML 中创建 Xamarin.Forms Entry 。
2.  响应 Entry 的文本更改。
3.  Entry 属性简介。

<!-- more -->

## 创建 Entry

1.  打开已有项目 AwesomeApp。
2.  添加新项 EntryPage.xaml。
3.  编辑 EntryPage.xaml：

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:d="http://xamarin.com/schemas/2014/forms/design"
             xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
             mc:Ignorable="d"
             x:Class="AwesomeApp.EntryPage">
	<ContentPage.Content>
		<StackLayout Margin="20,32">
			<Entry Placeholder="输入文本" />
		</StackLayout>
	</ContentPage.Content>
</ContentPage>
```

4.  编辑 App.xaml.cs：

```csharp
public App()
{
	InitializeComponent();
	MainPage = new EntryPage();
}
```

5.  调试 Android 上的应用：
    ![](https://img-blog.csdnimg.cn/20200305155717799.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjQ1NjQyMQ==,size_16,color_FFFFFF,t_70)

## 响应 Entry 的文本更改

1.  编辑 EntryPage.xaml：

```xml
<StackLayout Margin="20,32">
	<Entry Placeholder="输入文本"
		   TextChanged="OnEntryTextChanged"
		   Completed="OnEntryCompleted" />
	<Label Text="OldText"
		   TextColor="LightGray"/>
	<Label x:Name="labOld" />
	<Label Text="NewText"
		   TextColor="LightGray" />
	<Label x:Name="labNew" />
	<Label Text="CompletedText"
		   TextColor="LightGray" />
	<Label x:Name="labCompleted" />
</StackLayout>
```

> \*_使用返回或完成键完成 Entry 中的文本输入时，才会触发 Completed 事件_

2.  编辑 EntryPage.xaml.cs：

```csharp
public void OnEntryTextChanged(object sender, TextChangedEventArgs e)
{
	labOld.Text = e.OldTextValue;
	labNew.Text = e.NewTextValue;
}

public void OnEntryCompleted(object sender, EventArgs e)
{
	labCompleted.Text = ((Entry)sender).Text;
}
```

3.  调试 Android 上的应用：
    ![](https://img-blog.csdnimg.cn/20200305161009906.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjQ1NjQyMQ==,size_16,color_FFFFFF,t_70)

## Entry 属性简介

1.  编辑 EntryPage.xaml：

```xml
<Entry Placeholder="输入文本"
	   MaxLength="15"
	   IsSpellCheckEnabled="false"
	   IsTextPredictionEnabled="false"
	   IsPassword="true"
	   TextChanged="OnEntryTextChanged"
	   Completed="OnEntryCompleted" />
```

> - MaxLength 限制输入长度
> - IsSpellCheckEnabled 禁用拼写检查
> - IsTextPredictionEnabled 禁用文本预测和自动文本预测
> - IsPassword 对输入的字符进行掩码

2.  调试 Android 上的应用：
    ![](https://img-blog.csdnimg.cn/20200305161434688.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjQ1NjQyMQ==,size_16,color_FFFFFF,t_70)
