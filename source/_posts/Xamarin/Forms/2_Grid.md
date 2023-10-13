---
title: Xamarin.Forms 创建移动应用程序的基础知识 2 - Grid
date: 2021-08-19 09:20:00
categories:
	- 学习笔记
tags:
	- .NetCore
	- Xamarin
---

## 简介

演示如何在 Grid 中布局。

1.  在 XAML 中创建 Xamarin.Forms Grid。
2.  指定 Grid 的列和行。
3.  涉及 Grid 中多列或多行的内容。

<!-- more -->

## 创建 Grid

1.  打开已有项目 AwesomeApp。
2.  添加新项 GridPage.xaml：
    ![](https://img-blog.csdnimg.cn/20200305132648634.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjQ1NjQyMQ==,size_16,color_FFFFFF,t_70)
3.  编辑 GridPage.xaml：

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:d="http://xamarin.com/schemas/2014/forms/design"
             xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
             mc:Ignorable="d"
             x:Class="AwesomeApp.GridPage">
    <ContentPage.Content>
		<Grid Margin="20,35">
			<Label Text="设置Grid.Margin，可控制Grid外边距。" />
		</Grid>
	</ContentPage.Content>
</ContentPage>
```

> \*_除 Margin 属性外，还可在 Grid 上设置 Padding 属性。 Padding 指定 Grid 的内边距。_

4.  编辑 App.xaml.cs：

```csharp
public App()
{
	InitializeComponent();
	MainPage = new GridPage();
}
```

5.  调试 Android 上的应用：
    ![](https://img-blog.csdnimg.cn/20200305140418953.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjQ1NjQyMQ==,size_16,color_FFFFFF,t_70)

## 行列配置

1.  编辑 GridPage.xaml：

```xml
<Grid Margin="20,35,20,20">
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="0.5*" />
        <ColumnDefinition Width="0.5*" />
    </Grid.ColumnDefinitions>
    <Grid.RowDefinitions>
        <RowDefinition Height="50" />
        <RowDefinition Height="50" />
    </Grid.RowDefinitions>
    <Label Text="Column 0, Row 0" />
    <Label Grid.Column="1"
           Text="Column 1, Row 0" />
    <Label Grid.Row="1"
           Text="Column 0, Row 1" />
    <Label Grid.Column="1"
           Grid.Row="1"
           Text="Column 1, Row 1" />
</Grid>
```

2.  调试 Android 上的应用：
    ![](https://img-blog.csdnimg.cn/20200305143245643.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjQ1NjQyMQ==,size_16,color_FFFFFF,t_70)
    > 有效的宽度值和高度值为：
    >
    > - Auto 对列或行进行大小调整以适应内容。
    > - 比例值，将列和行的大小设置为剩余空间的比例。 以 \* 结尾表示比例值。
    > - 绝对值，使用特定的固定值调整列或行的大小。

> \*_可以使用 ColumnSpacing 和 RowSpacing 属性设置 Grid 中列和行之间的间距_

## 跨行跨列

1.  编辑 GridPage.xaml：

```xml
<Grid Margin="20,35,20,20">
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="0.5*" />
        <ColumnDefinition Width="0.5*" />
    </Grid.ColumnDefinitions>
    <Grid.RowDefinitions>
        <RowDefinition Height="50" />
        <RowDefinition Height="30" />
        <RowDefinition Height="30" />
    </Grid.RowDefinitions>
    <Label Grid.ColumnSpan="2"
           Text="This text uses the ColumnSpan property to span both columns." />
    <Label Grid.Row="1"
           Grid.RowSpan="2"
           Text="This text uses the RowSpan property to span two rows." />
</Grid>
```

> ColumnSpan 跨越多列，RowSpan 跨越多行

2.  调试 Android 上的应用：
    ![](https://img-blog.csdnimg.cn/20200305145400950.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjQ1NjQyMQ==,size_16,color_FFFFFF,t_70)
