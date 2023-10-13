---
title: Xamarin.Forms 创建移动应用程序的基础知识 11 - SQLite数据库
date: 2021-08-19 09:29:00
categories:
	- 学习笔记
tags: 
	- .NetCore
	- Xamarin
---

## 简介

演示 SQLite 数据库访问与数据操作。

1.  SQLite 类库引用。
2.  操作类与数据模型。
3.  数据操作。

<!-- more -->

## SQLite 类库引用

1.  管理 NuGet 包。
2.  搜索 sqlite-net-pcl。
3.  安装推荐版本。

## 操作类与数据模型

1.  添加 Note.cs 到 Models 文件夹。
2.  编辑 Note.cs：

```csharp
using System;
using SQLite;
namespace AwesomeApp.Models
{
	public class Note
	{
		[PrimaryKey, AutoIncrement]
		public int ID { get; set; }
		public string Name { get; set; }
		public string Text { get; set; }
		public DateTime Date { get; set; }
		public string DateString
		{
			get
			{
				return Date.ToString();
			}
		}
	}
}
```

> - PrimaryKey 属性标签表示为主键
> - AutoIncrement 属性标签表示为自增列
>
> 3.  添加 NoteDatabase.cs 到 Data 文件夹。
> 4.  编辑 NoteDatabase.cs：

```csharp
using AwesomeApp.Models;
using SQLite;
using System.Collections.Generic;
using System.Threading.Tasks;
namespace AwesomeApp.Data
{
	public class NoteDatabase
	{
		private readonly SQLiteAsyncConnection _database;
		public NoteDatabase(string dbPath)
		{
			_database = new SQLiteAsyncConnection(dbPath);
			_database.CreateTableAsync<Note>().Wait();
		}
		~NoteDatabase()
		{
			_database.CloseAsync();
		}
		public Task<List<Note>> GetNotesAsync()
		{
			return _database.Table<Note>().ToListAsync();
		}
		public Task<Note> GetNoteAsync(int id)
		{
			return _database.Table<Note>()
							.Where(i => i.ID == id)
							.FirstOrDefaultAsync();
		}
		public Task<int> SaveNoteAsync(Note note)
		{
			if (note.ID != 0)
			{
				return _database.UpdateAsync(note);
			}
			else
			{
				return _database.InsertAsync(note);
			}
		}
		public Task<int> DeleteNoteAsync(Note note)
		{
			return _database.DeleteAsync(note);
		}
	}
}
```

5.  编辑 App.xaml.cs：

```csharp
static NoteDatabase database;
public static NoteDatabase Database
{
	get
	{
		if (database == null)
		{
			database = new NoteDatabase(Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "Notes.db3"));
		}
		return database;
	}
}
……
public App()
{
	InitializeComponent();
	MainPage = new NavigationPage(new NotesPage());
}
```

## 数据操作

1. 添加 NotesPage.xaml。
2. 编辑 NotesPage.xaml：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
			 xmlns:local="clr-namespace:AwesomeApp;assembly=AwesomeApp"
             x:Class="AwesomeApp.NotesPage"
             Title="Notes">
	<ContentPage.ToolbarItems>
		<ToolbarItem Text="新建"
					 Order="Primary"
                     Clicked="OnNoteAddedClicked" />
	</ContentPage.ToolbarItems>
	<ContentPage.Content>
		<StackLayout Margin="{StaticResource PageMargin}">
			<Label x:Name="lblNull"
				   Text="暂无数据"
				   FontSize="32"
				   TextColor="Black"
				   HorizontalOptions="Center"
				   IsVisible="False" />
			<ListView x:Name="listView"
					  RowHeight="64"
					  IsPullToRefreshEnabled="True"
					  Refreshing="listView_Refreshing"
					  ItemSelected="OnListViewItemSelected">
				<ListView.ItemTemplate>
					<DataTemplate>
						<ViewCell>
							<ViewCell.ContextActions>
								<MenuItem x:Name="delMenuItem"
										  IconImageSource="{local:ImageResource Icons.delete.png}"
										  Text="删除"
										  CommandParameter="{Binding .}"
										  Clicked="MenuItem_OnDel_Clicked" />
								<MenuItem x:Name="moreMenuItem"
										  IconImageSource="{local:ImageResource Icons.more.png}"
										  Text="更多"
										  CommandParameter="{Binding .}"
										  Clicked="MenuItem_OnMore_Clicked" />
							</ViewCell.ContextActions>
							<StackLayout Margin="6,4">
								<Label Text="{Binding Name}"
									   FontSize="20"
									   TextColor="Black" />
								<Label Text="{Binding DateString}" />
							</StackLayout>
						</ViewCell>
					</DataTemplate>
				</ListView.ItemTemplate>
			</ListView>
		</StackLayout>
	</ContentPage.Content>
</ContentPage>
```

> ImageResource 嵌入资源处理类 ImageResourceExtension：

```csharp
[ContentProperty(nameof(Source))]
public class ImageResourceExtension : IMarkupExtension
{
	public string Source { get; set; }

	public object ProvideValue(IServiceProvider serviceProvider)
	{
		if (Source == null)
		{
			return null;
		}

		var type = typeof(ImageResourceExtension).GetTypeInfo().Assembly;

		ImageSource imageSource = ImageSource.FromResource(type.GetName().Name + "." + Source, type);

		return imageSource;
	}
}
```

3. 编辑 NotesPage.xaml.cs：

```csharp
using AwesomeApp.Models;
using System;
using System.Collections.Generic;
using System.Threading.Tasks;
using Xamarin.Forms;

namespace AwesomeApp
{
	public partial class NotesPage : ContentPage
	{
		public NotesPage()
		{
			InitializeComponent();
		}

		protected override async void OnAppearing()
		{
			base.OnAppearing();

			await bindListItems();
		}

		private async Task bindListItems()
		{
			List<Note> notes = await App.Database.GetNotesAsync();
			if (notes.Count == 0)
			{
				lblNull.IsVisible = true;
				listView.IsVisible = false;
			}
			else
			{
				lblNull.IsVisible = false;
				listView.IsVisible = true;
				listView.ItemsSource = notes.Count == 0 ? null : notes;
			}
		}

		private async void OnNoteAddedClicked(object sender, EventArgs e)
		{
			await Navigation.PushAsync(new NoteEntryPage
			{
				BindingContext = new Note()
			});
		}

		private async void OnListViewItemSelected(object sender, SelectedItemChangedEventArgs e)
		{
			if (e.SelectedItem != null)
			{
				await Navigation.PushAsync(new NoteEntryPage
				{
					BindingContext = e.SelectedItem as Note
				});
			}
		}

		private async void MenuItem_OnMore_Clicked(object sender, EventArgs e)
		{
			MenuItem menu = sender as MenuItem;
			if ((Note)menu.CommandParameter != null)
			{
				await Navigation.PushAsync(new NoteEntryPage
				{
					BindingContext = (Note)menu.CommandParameter as Note
				});
			}
		}

		private async void MenuItem_OnDel_Clicked(object sender, EventArgs e)
		{
			MenuItem menu = sender as MenuItem;
			if (true == await DisplayAlert("要删除 " + ((Note)menu.CommandParameter).Name + " 这条数据吗？", null, "删除", "取消"))
			{
				await App.Database.DeleteNoteAsync((Note)menu.CommandParameter);
				listView.BeginRefresh();
			}
		}

		private async void listView_Refreshing(object sender, EventArgs e)
		{
			await bindListItems();
			listView.EndRefresh();
		}
	}
}
```

4. 添加 NoteEntryPage.xaml。
5. 编辑 NoteEntryPage.xaml：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="AwesomeApp.NoteEntryPage"
             Title="Note Entry">
	<ContentPage.Resources>
		<Style TargetType="{x:Type Editor}">
			<Setter Property="BackgroundColor"
                    Value="{StaticResource AppBackgroundColor}" />
		</Style>

		<Style TargetType="Button"
               ApplyToDerivedTypes="True"
               CanCascade="True">
			<Setter Property="FontSize" Value="Medium" />
			<Setter Property="BackgroundColor" Value="#2196F3" />
			<Setter Property="TextColor" Value="White" />
			<Setter Property="CornerRadius" Value="5" />
		</Style>
	</ContentPage.Resources>
	<ContentPage.Content>
		<StackLayout Margin="{StaticResource PageMargin}">
			<Label Text="{Binding Data}" />
			<Entry x:Name="entName"
				   Placeholder="文件名"
				   Text="{Binding Name}" />
			<Editor x:Name="ediTtext"
					Placeholder="内容"
					AutoSize="TextChanges"
					MaxLength="200"
					Text="{Binding Text}" />
			<Grid>
				<Grid.ColumnDefinitions>
					<ColumnDefinition Width="*" />
					<ColumnDefinition Width="*" />
				</Grid.ColumnDefinitions>
				<Button Text="保存"
						Clicked="OnSaveButtonClicked" />
				<Button Grid.Column="1"
					    Text="取消"
					    Clicked="OnCancelButtonClicked"/>
			</Grid>
		</StackLayout>
	</ContentPage.Content>
</ContentPage>
```

6. 编辑 NoteEntryPage.xaml.cs：

```csharp
using AwesomeApp.Models;
using System;
using System.IO;
using System.Reflection;
using Xamarin.Forms;

namespace AwesomeApp
{
	public partial class NoteEntryPage : ContentPage
	{
		public NoteEntryPage()
		{
			InitializeComponent();
		}

		async void OnSaveButtonClicked(object sender, EventArgs e)
		{
			var note = (Note)BindingContext;
			note.Date = DateTime.UtcNow;

			await App.Database.SaveNoteAsync(note);

			await Navigation.PopAsync();
		}

		async void OnCancelButtonClicked(object sender, EventArgs e)
		{
			await Navigation.PopAsync();
		}
	}
}
```

7. 调试界面：
   ![](https://img-blog.csdnimg.cn/20200309171537312.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjQ1NjQyMQ==,size_16,color_FFFFFF,t_70)
   ![](https://img-blog.csdnimg.cn/20200309171553783.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjQ1NjQyMQ==,size_16,color_FFFFFF,t_70)
   ![](https://img-blog.csdnimg.cn/20200309171638665.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjQ1NjQyMQ==,size_16,color_FFFFFF,t_70)
   ![](https://img-blog.csdnimg.cn/20200309171656902.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjQ1NjQyMQ==,size_16,color_FFFFFF,t_70)
   ![](https://img-blog.csdnimg.cn/20200309171711780.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjQ1NjQyMQ==,size_16,color_FFFFFF,t_70)
