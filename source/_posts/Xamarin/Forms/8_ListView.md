---
title: Xamarin.Forms 创建移动应用程序的基础知识 8 - ListView
date: 2021-08-19 09:26:00
categories:
	- 学习笔记
tags: 
	- .NetCore
	- Xamarin
---

## 简介

演示如何自定义 ListView。

1.  在 XAML 中创建 Xamarin.Forms ListView 。
2.  ListView 数据绑定。
3.  ListView 选择事件。
4.  ListView 自定义单元格。

<!-- more -->

## 创建 ListView

1.  打开已有项目 AwesomeApp。
2.  添加新项 ListViewPage.xaml。
3.  编辑 ListViewPage.xaml：

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:d="http://xamarin.com/schemas/2014/forms/design"
             xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
             mc:Ignorable="d"
             x:Class="AwesomeApp.ListViewPage">
	<ContentPage.Content>
		<StackLayout Margin="20,32">
			<ListView x:Name="list"
					  SeparatorColor="Gray">
				<ListView.ItemsSource>
					<x:Array Type="{x:Type x:String}">
						<x:String>Item1</x:String>
						<x:String>Item2</x:String>
						<x:String>Item3</x:String>
						<x:String>Item4</x:String>
						<x:String>Item5</x:String>
						<x:String>Item6</x:String>
						<x:String>Item7</x:String>
						<x:String>Item8</x:String>
						<x:String>Item9</x:String>
					</x:Array>
				</ListView.ItemsSource>
				<ListView.ItemTemplate>
					<DataTemplate>
						<TextCell Text="{Binding .}"
								  TextColor="Black" />
					</DataTemplate>
				</ListView.ItemTemplate>
			</ListView>
		</StackLayout>
	</ContentPage.Content>
</ContentPage>
```

> - ItemsSource 指定了一个字符串数组做为数据源
> - ItemTemplate 定义了一个文本单元格

4.  编辑 App.xaml.cs：

```csharp
public App()
{
	InitializeComponent();
	MainPage = new ListViewPage();
}
```

5.  调试界面：
    ![](https://img-blog.csdnimg.cn/20200306172716586.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjQ1NjQyMQ==,size_16,color_FFFFFF,t_70)

## 数据绑定

1.  编辑 ListViewPage.xaml：

```xml
<ListView x:Name="list"
		  SeparatorColor="Gray">
	<ListView.ItemTemplate>
		<DataTemplate>
			<ImageCell ImageSource="{Binding Image}"
					   Text="{Binding Title}"
					   TextColor="Black"
					   Detail="{Binding Detail}"/>
		</DataTemplate>
	</ListView.ItemTemplate>
</ListView>
```

> ItemTemplate 定义了一个图文单元格

2.  添加 ItemData 类

```csharp
public class ItemData
{
    public string Title { get; set; }
    public string Detail { get; set; }
    public string Image { get; set; }

    public override string ToString()
    {
        return Title;
    }
}
```

3.  编辑 ListViewPage.xaml.cs：

```csharp
public ListViewPage()
{
	InitializeComponent();

	List<ItemData> listData = new List<ItemData>();
	for (int i = 1; i < 10; i++)
	{
		listData.Add(new ItemData
		{
			Title = "Title" + i.ToString(),
			Detail = "Detail" + i.ToString(),
			Image = "csdn_logo"
		});
	}
	list.ItemsSource = listData;
}
```

> Image 这里使用的是本地资源，还可以根据需要使用 URL 或嵌入资源 4. 调试界面：
> ![](https://img-blog.csdnimg.cn/20200307091011181.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjQ1NjQyMQ==,size_16,color_FFFFFF,t_70)

## 选择事件

1.  编辑 ListViewPage.xaml：

```xml
<Label x:Name="lblSelectTitle"
	   Text="这里显示Selected事件的单元格绑定Title"/>
<Label x:Name="lblTappedTitle"
	   Text="这里显示Tapped事件的单元格绑定Title"/>
<ListView x:Name="list"
		  SeparatorColor="Gray"
		  ItemSelected="list_ItemSelected"
		  ItemTapped="list_ItemTapped">
```

2.  编辑 ListViewPage.xaml.cs：

```csharp
private int selectIndex = -1;
private void list_ItemSelected(object sender, SelectedItemChangedEventArgs e)
{
	if (selectIndex != e.SelectedItemIndex)
	{
		selectIndex = e.SelectedItemIndex;
		lblSelectTitle.Text = (e.SelectedItem as ItemData).Title;
	}
	else
	{
		lblSelectTitle.Text = (e.SelectedItem as ItemData).Detail;
	}
}
private int tapIndex = -1;
private void list_ItemTapped(object sender, ItemTappedEventArgs e)
{
	if (tapIndex != e.ItemIndex)
	{
		tapIndex = e.ItemIndex;
		lblTappedTitle.Text = (e.Item as ItemData).Title;
	}
	else
	{
		lblTappedTitle.Text = (e.Item as ItemData).Detail;
	}
}
```

> - ItemSelected 事件选择新项后触发
> - ItemTapped 事件每次点按后都会触发
>
> 3.  调试界面：
>     ![](https://img-blog.csdnimg.cn/20200307092528623.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjQ1NjQyMQ==,size_16,color_FFFFFF,t_70)

## 自定义单元格

1.  编辑 ListViewPage.xaml：

```xml
<ListView x:Name="list"
		  SeparatorColor="Gray"
		  HasUnevenRows="true"
		  ItemsSource="{Binding listData}"
		  ItemSelected="list_ItemSelected"
		  ItemTapped="list_ItemTapped">
	<ListView.ItemTemplate>
		<DataTemplate>
			<ViewCell>
				<Grid Padding="12">
					<Grid.RowDefinitions>
						<RowDefinition Height="Auto" />
						<RowDefinition Height="*" />
					</Grid.RowDefinitions>
					<Grid.ColumnDefinitions>
						<ColumnDefinition Width="Auto" />
						<ColumnDefinition Width="*" />
					</Grid.ColumnDefinitions>
					<Image Grid.RowSpan="2"
						   Margin="0,0,6,0"
						   Source="{Binding Image}"
						   HeightRequest="60"
						   WidthRequest="60"
						   BackgroundColor="LightGray"
						   VerticalOptions="Start" />
					<Label Grid.Column="1"
						   Text="{Binding Title}"
						   TextColor="Black"
						   FontAttributes="Bold" />
					<Label Grid.Row="1"
						   Grid.Column="1"
						   Text="{Binding Detail}"
						   TextColor="DimGray"
						   VerticalOptions="Start" />
				</Grid>
			</ViewCell>
		</DataTemplate>
	</ListView.ItemTemplate>
</ListView>
```

2.  调试界面：
    ![](https://img-blog.csdnimg.cn/20200307113326591.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjQ1NjQyMQ==,size_16,color_FFFFFF,t_70)
