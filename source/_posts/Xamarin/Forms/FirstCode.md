---
title: Xamarin.Forms 创建移动应用程序的基础知识 - 生成第一个Xamarin应用
date: 2021-08-19 09:18:00
categories:
	- 学习笔记
tags: 
	- .NetCore
	- Xamarin
---

## 简介

生成第一个 Xamarin 应用

<!-- more -->

## 开发环境

Visual Studio 2019

## Windows 分步说明

1.  选择“文件”>“新建”>“项目...” ，或按“创建新项目...” 按钮：![](https://img-blog.csdnimg.cn/20200305094410752.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjQ1NjQyMQ==,size_16,color_FFFFFF,t_70)
2.  搜索“Xamarin”或从“项目类型” 菜单中选择“移动” 。 选择“移动应用(Xamarin.Forms)” 项目类型：![](https://img-blog.csdnimg.cn/20200305094544257.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjQ1NjQyMQ==,size_16,color_FFFFFF,t_70)
3.  选择项目名称 – 示例使用“AwesomeApp”：![](https://img-blog.csdnimg.cn/20200305094628686.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjQ1NjQyMQ==,size_16,color_FFFFFF,t_70)
4.  单击“空白” 项目类型，确保选择了“Android” 和“iOS” ：![](https://img-blog.csdnimg.cn/20200305094734459.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjQ1NjQyMQ==,size_16,color_FFFFFF,t_70)
5.  到 NuGet 包还原（状态栏中将出现“还原已完成”消息）。
6.  新 Visual Studio 2019 安装不会配置 Android 模拟器。 单击“调试” 按钮上的下拉箭头，然后选择“创建 Android Emulator” 以启动仿真器创建屏幕：
    ![](https://img-blog.csdnimg.cn/20200305092243262.png)
7.  在仿真器创建屏幕中，使用默认设置并单击“创建” 按钮：![](https://img-blog.csdnimg.cn/20200305094857438.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjQ1NjQyMQ==,size_16,color_FFFFFF,t_70)
8.  创建仿真器会使你返回“设备管理器”窗口。 单击“启动” 按钮以启动新仿真器：![](https://img-blog.csdnimg.cn/20200305094923593.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjQ1NjQyMQ==,size_16,color_FFFFFF,t_70)
9.  Visual Studio 2019 现在应在“调试” 按钮上显示新仿真器的名称：
    ![](https://img-blog.csdnimg.cn/20200305095008259.png)
10. 单击“调试” 按钮以生成应用程序并将其部署到 Android 仿真器：
    ![](https://img-blog.csdnimg.cn/20200305095224286.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjQ1NjQyMQ==,size_16,color_FFFFFF,t_70)

## 自定义应用程序

1.  编辑 MainPage.xaml：

```xml
<Label Text="欢迎使用Xamarin！" HorizontalOptions="Center" VerticalOptions="CenterAndExpand" />
<Button Text="点我" Clicked="Button_Clicked" />
```

2.  编辑 MainPage.xaml.cs：

```csharp
int count = 0;
void Button_Clicked(object sender, System.EventArgs e)
{
    count++;
    ((Button)sender).Text = $"你点击了 {count} 次。";
}
```

3.  调试 Android 上的应用：
    ![](https://img-blog.csdnimg.cn/20200305095949266.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjQ1NjQyMQ==,size_16,color_FFFFFF,t_70)

## 在 Visual Studio 2019 中生成 iOS 应用

{% video https://sec.ch9.ms/ch9/093f/7c34917b-e6a0-41b5-8f49-4253b0b9093f/XamariniOS_high.mp4 %}
