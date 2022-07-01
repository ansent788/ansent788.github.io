---
title: Xamarin.Forms 创建移动应用程序的基础知识 9 - 弹出窗口
date: 2021-08-19 09:27:00
categories:
	- 学习笔记
tags: 
	- .NetCore
	- Xamarin
---

## 简介

演示如何在 Xamarin.Forms 中显示弹出式菜单。

1.  弹出提醒。
2.  模式对话框。

<!-- more -->

## 弹出提醒

1.  打开已有项目 AwesomeApp。
2.  添加新项 PopupsPage.xaml。
3.  编辑 PopupsPage.xaml：

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:d="http://xamarin.com/schemas/2014/forms/design"
             xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
             mc:Ignorable="d"
             x:Class="AwesomeApp.PopupsPage">
	<ContentPage.Content>
		<StackLayout Margin="20,32">
			<Button Text="弹出提醒1"
					Clicked="Button1_Clicked" />
			<Button Text="弹出提醒2"
					Clicked="Button2_Clicked" />
			<Grid>
				<Grid.ColumnDefinitions>
					<ColumnDefinition Width="Auto" />
					<ColumnDefinition Width="*" />
				</Grid.ColumnDefinitions>
				<Label Text="弹出提醒2确认"
					   VerticalOptions="Center" />
				<Switch Grid.Column="1"
						x:Name="swhBtn2OK"
						HorizontalOptions="Start" />
			</Grid>
		</StackLayout>
	</ContentPage.Content>
</ContentPage>
```

4.  编辑 PopupsPage.xaml：

```csharp
private async void Button1_Clicked(object sender, EventArgs e)
{
	await DisplayAlert("弹出框", "这里是提醒内容", "关闭");
}
private async void Button2_Clicked(object sender, EventArgs e)
{
	swhBtn2OK.IsToggled = await DisplayAlert("确认框", "这里是确认内容", "确认", "取消");
}
```

5.  编辑 App.xaml.cs：

```csharp
public App()
{
	InitializeComponent();
	MainPage = new PopupsPage();
}
```

5.  调试界面：
    ![](https://img-blog.csdnimg.cn/20200307151206169.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjQ1NjQyMQ==,size_16,color_FFFFFF,t_70)
    ![](https://img-blog.csdnimg.cn/20200307151226189.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjQ1NjQyMQ==,size_16,color_FFFFFF,t_70)

## 模式对话框

1.  编辑 PopupsPage.xaml：

```xml
<Button Text="模式对话框"
		Clicked="Button3_Clicked" />
<Label x:Name="lblBtn3Action"
	   Text="模式对话框返回值" />
```

2.  编辑 PopupsPage.xaml.cs：

```csharp
private async void Button3_Clicked(object sender, EventArgs e)
{
	lblBtn3Action.Text = await DisplayActionSheet("模式对话框", "关闭", "请选择一个Action", "Action1", "Action2", "Action3");
}
```

3.  调试界面：
    ![](https://img-blog.csdnimg.cn/20200307151046154.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjQ1NjQyMQ==,size_16,color_FFFFFF,t_70)
