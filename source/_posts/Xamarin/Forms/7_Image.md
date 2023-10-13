---
title: Xamarin.Forms 创建移动应用程序的基础知识 7 - Image
date: 2021-08-19 09:25:00
categories:
	- 学习笔记
tags: 
	- .NetCore
	- Xamarin
---

## 简介

演示如何显示图像。

1.  在 XAML 中创建 Xamarin.Forms Image 。
2.  Image 基本属性。
3.  Image 图片引用。

<!-- more -->

## 创建 Image

1.  打开已有项目 AwesomeApp。
2.  添加新项 ImagePage.xaml。
3.  编辑 ImagePage.xaml：

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:d="http://xamarin.com/schemas/2014/forms/design"
             xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
             mc:Ignorable="d"
             x:Class="AwesomeApp.ImagePage">
	<ContentPage.Content>
		<StackLayout Margin="20,32">
			<Image Source="https://avatar.csdnimg.cn/F/3/D/1_weixin_42456421_1583375444.jpg"
               HeightRequest="300" />
		</StackLayout>
	</ContentPage.Content>
</ContentPage>
```

> Image.Source 属性是 ImageSource 类型，可以是文件、URI 或本地资源

> \*_Image 默认保持图像的纵横比_

4.  编辑 App.xaml.cs：

```csharp
public App()
{
	InitializeComponent();
	MainPage = new ImagePage();
}
```

5.  调试 Android 上的应用：
    ![](https://img-blog.csdnimg.cn/20200306144958616.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjQ1NjQyMQ==,size_16,color_FFFFFF,t_70)
    > \*_Image 自动将下载的图像缓存 24 小时_

## Image 基本属性

1.  编辑 ImagePage.xaml：

```xml
<Image Source="https://avatar.csdnimg.cn/F/3/D/1_weixin_42456421_1583375444.jpg"
       Aspect="Fill"
       HeightRequest="{OnPlatform iOS=300, Android=250}"
       WidthRequest="{OnPlatform iOS=300, Android=250}"
       HorizontalOptions="Center" />
```

> - Aspect 图像的缩放模式
> - OnPlatform 标记扩展可每个平台自定义值

2.  调试 Android 上的应用：
    ![](https://img-blog.csdnimg.cn/20200306151617294.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjQ1NjQyMQ==,size_16,color_FFFFFF,t_70)

## Image 图片引用

![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9jc2RuaW1nLmNuL2Nkbi9jb250ZW50LXRvb2xiYXIvY3Nkbi1sb2dvXy5wbmc?x-oss-process=image/format,png)

1.  本地图片引用：

- Android
  ![](https://img-blog.csdnimg.cn/2020030615274837.png)
  ![](https://img-blog.csdnimg.cn/20200306155233812.png)
- IOS
  ![](https://img-blog.csdnimg.cn/20200306152825162.png)
  ![](https://img-blog.csdnimg.cn/20200306152949849.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjQ1NjQyMQ==,size_16,color_FFFFFF,t_70)
  > 选择文件

![](https://img-blog.csdnimg.cn/20200306153057589.png)

> 重命名资源为文件名，保险与 Android 本地资源名称一致。

![](https://img-blog.csdnimg.cn/20200306155621329.png)

- 编辑 ImagePage.xaml：

```xml
<Image Source="csdn_logo"
       Aspect="Fill"
       HeightRequest="{OnPlatform iOS=88, Android=88}"
       WidthRequest="{OnPlatform iOS=180, Android=180}"
       HorizontalOptions="Center" />
```

> Source 本地资源 Android 引用直接使用文件名称 \*_使用 OnPlatform 标记可分别定义_

- 调试 Android 上的应用：
  ![](https://img-blog.csdnimg.cn/20200306153544175.png)

2.  嵌入资源引用：
    ![](https://img-blog.csdnimg.cn/20200306153912518.png)

- 编辑 ImagePage.xaml：

```xml
<Image x:Name="imgIcon" />
```

- 编辑 ImagePage.xaml.cs：

```csharp
public ImagePage()
{
	InitializeComponent();
	imgIcon.Source = ImageSource.FromResource("AwesomeApp.Icons.delete.png");
}
```

> 使用 ImageSource.FromResource 查找嵌入资源 \*_路径：资源所在程序集命名空间.文件夹.文件名.文件类型_

- 调试 Android 上的应用：
  ![](https://img-blog.csdnimg.cn/20200306155107769.png)
