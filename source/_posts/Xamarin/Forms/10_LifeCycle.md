---
title: Xamarin.Forms 创建移动应用程序的基础知识 10 - 应用程序的生命周期
date: 2021-08-19 09:28:00
categories:
	- 学习笔记
tags: 
	- .NetCore
	- Xamarin
---

## 简介

演示应用程序的生命周期事件与各事件之间的数据处理。

1.  生命周期事件。
2.  生命周期数据处理。

<!-- more -->

## 生命周期事件

1.  打开已有项目 AwesomeApp。
2.  编辑 App.xaml.cs：

```csharp
protected override void OnStart()
{
	Console.WriteLine("OnStart");
}
protected override void OnSleep()
{
	Console.WriteLine("OnSleep");
}
protected override void OnResume()
{
	Console.WriteLine("OnResume");
}
```

3.  调试界面：
    > 应用程序起动时触发
    > ![Onstart](https://img-blog.csdnimg.cn/20200309114151711.png)

> 应用程序切换到后台时触发
> ![OnSleep](https://img-blog.csdnimg.cn/20200309114647300.png)

> 当前活动应用程序切换回本程序时触发![OnResume](https://img-blog.csdnimg.cn/20200309115207278.png)

## 生命周期数据处理

1.  编辑 App.xaml.cs：

```csharp
const string displayText = "displayText";
public string DisplayText { get; set; }
……
protected override void OnStart()
{
	Console.WriteLine("OnStart");
	if (Properties.ContainsKey(displayText))
	{
		DisplayText = (string)Properties[displayText];
	}
}
protected override void OnSleep()
{
	Console.WriteLine("OnSleep");
	Properties[displayText] = DisplayText;
}
```

2.  编辑 MainPage.xaml：

```xml
<Entry x:Name="entEvent"
	   Placeholder="这里显示生命周期内容"
	   Completed="entEvent_Completed" />
```

3.  编辑 MainPage.xaml.cs：

```csharp
protected override void OnAppearing()
{
	base.OnAppearing();
	entEvent.Text = (Application.Current as App).DisplayText;
}
private void entEvent_Completed(object sender, EventArgs e)
{
	(Application.Current as App).DisplayText = (sender as Entry).Text;
}
```

4.  调试界面：
    > 无输入内容、APP 永久属性字典中不包含 displayText 或 displayText 无值时显示
    > ![](https://img-blog.csdnimg.cn/20200309121724339.png)

> 应用程序切换时 APP 永久属性字典包含 displayText 并 displayText 有值时显示
> ![](https://img-blog.csdnimg.cn/20200309124017490.png)
