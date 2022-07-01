---
title: Xamarin.Forms 创建移动应用程序的基础知识 1 - StackLayout
date: 2021-08-19 09:19:00
categories:
	- 学习笔记
tags: 
	- .NetCore
	- Xamarin
---

## 简介

演示如何在 StackLayout 中对齐控件。

1.  在 XAML 中创建 Xamarin.Forms StackLayout。
2.  指定 StackLayout 的方向。
3.  控制 StackLayout 内子视图的对齐和扩展。

<!-- more -->

## 创建 stacklayout

1.  打开已有项目 AwesomeApp。
2.  添加新项 StackLayoutPage.xaml：
    ![](https://img-blog.csdnimg.cn/20200305134444615.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjQ1NjQyMQ==,size_16,color_FFFFFF,t_70)
3.  编辑 StackLayoutPage.xaml：

```xml
<StackLayout>
	<Label Text="第一个Label。" />
	<Label Text="第二个Label。" />
	<Label Text="第三个Label。" />
</StackLayout>
```

> StackLayout 默认为垂直方向。 此外，Margin 属性表示 ContentPage 中 StackLayout 的外边距。

> \*_除 Margin 外，StackLayout 还可设置 Padding 和 Spacing。 Padding 指定 StackLayout 的内边距，Spacing 指定 StackLayout 中每个子元素之间的间隔大小。_

4.  编辑 App.xaml.cs：

```csharp
public App()
{
	InitializeComponent();
	MainPage = new StackLayoutPage();
}
```

5.  调试 Android 上的应用：
    ![](https://img-blog.csdnimg.cn/20200305135303284.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjQ1NjQyMQ==,size_16,color_FFFFFF,t_70)

## StackLayout 方向

1.  编辑 StackLayoutPage.xaml：

```xml
<StackLayout Margin="20,32" Orientation="Horizontal">
	<Label Text="第一个Label的显示内容。" />
	<Label Text="第二个Label的显示内容。" />
	<Label Text="第三个Label的显示内容。" />
</StackLayout>
```

> Orientation 属性：Horizontal 横向，Vertical 纵向

2.  调试 Android 上的应用：
    ![](https://img-blog.csdnimg.cn/20200305135351611.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjQ1NjQyMQ==,size_16,color_FFFFFF,t_70)

## 对齐方式和扩展选项

1.  编辑 StackLayoutPage.xaml：

```xml
<StackLayout Margin="20,35">
	<Label Text="Start"
		   HorizontalOptions="Start"
		   BackgroundColor="LightGray" />
	<Label Text="Center"
		   HorizontalOptions="Center"
		   BackgroundColor="LightGray" />
	<Label Text="End"
		   HorizontalOptions="End"
		   BackgroundColor="LightGray" />
	<Label Text="Fill"
		   HorizontalOptions="Fill"
		   BackgroundColor="LightGray" />
	<Label Text="StartAndExpand"
		   VerticalOptions="StartAndExpand"
		   BackgroundColor="LightGray" />
	<Label Text="CenterAndExpand"
		   VerticalOptions="CenterAndExpand"
		   BackgroundColor="LightGray" />
	<Label Text="EndAndExpand"
		   VerticalOptions="EndAndExpand"
		   BackgroundColor="LightGray" />
	<Label Text="FillAndExpand"
		   VerticalOptions="FillAndExpand"
		   BackgroundColor="LightGray" />
</StackLayout>
```

> 前四个 Label 设置 HorizontalOptions 横向配置 Start、Center、End 和 Fill 各元素的显示位置，后四个 Label 设置 VerticalOptions 纵向配置 StartAndExpand、CenterAndExpand、EndAndExpand 和 FillAndExpand 在剩余空间中的位置。

2.  调试 Android 上的应用：
    ![](https://img-blog.csdnimg.cn/20200305135430671.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjQ1NjQyMQ==,size_16,color_FFFFFF,t_70)
    > StackLayout 仅遵循与 StackLayout 方向相反的子元素位置配置。 因此，Vertical 方向的 HorizontalOptions 属性设置时有效

> StackLayout 只能按照其方向扩展元素位置。 因此，Vertical 方向的 StackLayout 可以扩展 Label 子元素。 这意味着，对于 Vertical 对齐方式，每个 Label 在 StackLayout 内占据相同的空间量。 但是，只有最后一个 Label（可将 VerticalOptions 属性设置为 FillAndExpand）具有不同的大小

> \*_以上代码中 StackLayout.Orientation 默认为 Vertical，故前四个 Label 配置 HorizontalOptions 位置有效，后四个 Label 配置 VerticalOptions 为 Expand 有效_

> \*_使用 StackLayout 中的所有空间时，Expand 不起作用_
