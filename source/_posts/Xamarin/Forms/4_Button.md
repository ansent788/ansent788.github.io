---
title: Xamarin.Forms 创建移动应用程序的基础知识 4 - Button
date: 2021-08-19 09:22:00
categories:
	- 学习笔记
tags: 
	- .NetCore
	- Xamarin
---

## 简介

演示如何自定义 Button。

1.  在 XAML 中创建 Xamarin.Forms Button。
2.  Button 点击事件。
3.  更改 Button 的样式。

<!-- more -->

## 创建 Button

1.  打开已有项目 AwesomeApp。
2.  添加新项 ButtonPage.xaml。
3.  编辑 ButtonPage.xaml：

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:d="http://xamarin.com/schemas/2014/forms/design"
             xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
             mc:Ignorable="d"
             x:Class="AwesomeApp.ButtonPage">
	<ContentPage.Content>
		<StackLayout>
			<Button Text="欢迎使用 Xamarin.Forms！"
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
	MainPage = new ButtonPage();
}
```

5.  调试 Android 上的应用：
    ![](https://img-blog.csdnimg.cn/2020030515320694.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjQ1NjQyMQ==,size_16,color_FFFFFF,t_70)

## Button 点击事件

1.  编辑 ButtonPage.xaml：

```xml
<Button Text="欢迎使用 Xamarin.Forms！"
		x:Name="btnWel"
        VerticalOptions="CenterAndExpand"
        HorizontalOptions="CenterAndExpand"
		Clicked="OnButtonClicked"/>
```

> 除了 Clicked 事件，Button 还定义了 Pressed 和 Released 事件

2.  编辑 ButtonPage.xaml.cs：

```csharp
private void OnButtonClicked(object sender, EventArgs e)
{
	btnWel.Text = "你点击了我一下！";
}
```

3.  调试 Android 上的应用：
    ![](https://img-blog.csdnimg.cn/20200305153708933.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjQ1NjQyMQ==,size_16,color_FFFFFF,t_70)

## 更改 Button 的样式

1.  编辑 ButtonPage.xaml：

```xml
<Button Text="欢迎使用 Xamarin.Forms！"
		x:Name="btnWel"
        VerticalOptions="CenterAndExpand"
        HorizontalOptions="Center"
        TextColor="Black"
		FontSize="20"
        BackgroundColor="LightGray"
        BorderColor="Gray"
        BorderWidth="5"
        CornerRadius="5"
        WidthRequest="240"
        HeightRequest="120"
		Clicked="OnButtonClicked"/>
```

> BorderColor 设置边框颜色，BorderWidth 设置边框宽度，CornerRadius 设置圆角大小，WidthRequest 和 HeightRequest 设置 Button 大小

2.  调试 Android 上的应用：
    ![](https://img-blog.csdnimg.cn/20200305154412650.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjQ1NjQyMQ==,size_16,color_FFFFFF,t_70)
