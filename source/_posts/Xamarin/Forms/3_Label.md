---
title: Xamarin.Forms 创建移动应用程序的基础知识 3 - Label
date: 2021-08-19 09:21:00
categories:
	- 学习笔记
tags: 
	- .NetCore
	- Xamarin
---

## 简介

演示如何在 Label 中显示文本。

1.  在 XAML 中创建 Xamarin.Forms Label。
2.  更改 Label 的样式。
3.  在一个 Label 中显示具有多种格式的文本。

<!-- more -->

## 创建 Label

1.  打开已有项目 AwesomeApp。
2.  添加新项 LabelPage.xaml：
    ![](https://img-blog.csdnimg.cn/20200305150539729.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjQ1NjQyMQ==,size_16,color_FFFFFF,t_70)
3.  编辑 LabelPage.xaml：

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:d="http://xamarin.com/schemas/2014/forms/design"
             xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
             mc:Ignorable="d"
             x:Class="AwesomeApp.LabelPage">
	<ContentPage.Content>
		<StackLayout>
			<Label Text="欢迎使用 Xamarin.Forms！"
                VerticalOptions="CenterAndExpand"
                HorizontalOptions="CenterAndExpand" />
		</StackLayout>
	</ContentPage.Content>
</ContentPage>
```

4.  编辑 App.xaml.cs：

```csharp
public App()
{
	InitializeComponent();
	MainPage = new LabelPage();
}
```

5.  调试 Android 上的应用：
    ![](https://img-blog.csdnimg.cn/20200305150842798.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjQ1NjQyMQ==,size_16,color_FFFFFF,t_70)

## 配置 Label 样式

1.  编辑 LabelPage.xaml：

```xml
<Label Text="欢迎使用 Xamarin.Forms！"
	   VerticalOptions="CenterAndExpand"
	   HorizontalOptions="CenterAndExpand"
	   TextColor="Blue"
       FontAttributes="Italic"
       FontSize="22"
       TextDecorations="Underline" />
```

> TextColor 设置文本的颜色，FontAttributes 设置字体为斜体，FontSize 设置字号，TextDecorations 应用下划线效果

2.  调试 Android 上的应用：
    ![](https://img-blog.csdnimg.cn/20200305151324276.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjQ1NjQyMQ==,size_16,color_FFFFFF,t_70)

## 自定义 Label 文本格式

1.  编辑 LabelPage.xaml：

```xml
<Label TextColor="Gray"
	   FontSize="20"
	   HorizontalOptions="Center"
	   VerticalOptions="CenterAndExpand">
	<Label.FormattedText>
		<FormattedString>
			<Span Text="欢迎 "
				  TextColor="Blue"/>
			<Span Text="使用 "
				  TextColor="Red"
				  FontAttributes="Italic" />
			<Span Text="Xamarin.Form！"
				  TextColor="Green"
				  TextDecorations="Underline" />
		</FormattedString>
	</Label.FormattedText>
</Label>
```

> FormattedText 属性是 FormattedString 类型，可以包含一个或多个 Span 实例

2.  调试 Android 上的应用：
    ![](https://img-blog.csdnimg.cn/20200305152247542.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjQ1NjQyMQ==,size_16,color_FFFFFF,t_70)
