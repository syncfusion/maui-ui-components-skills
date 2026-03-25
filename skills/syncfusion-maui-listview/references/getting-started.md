# Getting Started with .NET MAUI ListView

## Table of Contents
- [Overview](#overview)
- [Installation and Setup](#installation-and-setup)
- [Registering the Handler](#registering-the-handler)
- [Creating Your First ListView](#creating-your-first-listview)
- [Data Model and ViewModel](#data-model-and-viewmodel)
- [Binding Data to ListView](#binding-data-to-listview)
- [Defining Item Templates](#defining-item-templates)
- [Running the Application](#running-the-application)
- [Basic Customization](#basic-customization)
- [Next Steps](#next-steps)

## Overview

The Syncfusion .NET MAUI ListView (SfListView) is a powerful, data-driven list control that renders collections of data with rich customization options. It provides:

- **Optimized performance** with view reusing strategy
- **Flexible layouts** (linear and grid)
- **Data operations** (sorting, filtering, grouping)
- **Rich templating** for custom item appearance
- **Selection support** with multiple modes
- **Interactive features** (swipe, drag-drop, pull-to-refresh)

## Installation and Setup

### Step 1: Create a New .NET MAUI Project

**Visual Studio:**
1. Go to **File > New > Project**
2. Select **.NET MAUI App** template
3. Name your project (e.g., "MyListViewApp")
4. Choose location and click **Create**
5. Select .NET 9 or later framework

**Visual Studio Code:**
1. Press `Ctrl+Shift+P` (Windows/Linux) or `Cmd+Shift+P` (Mac)
2. Type **.NET: New Project**
3. Choose **.NET MAUI App**
4. Select location and name your project

**Command Line:**
```bash
dotnet new maui -n MyListViewApp
cd MyListViewApp
```

### Step 2: Install Syncfusion.Maui.ListView NuGet Package

**Visual Studio:**
1. Right-click project in **Solution Explorer**
2. Select **Manage NuGet Packages**
3. Search for `Syncfusion.Maui.ListView`
4. Install the latest version

**Visual Studio Code / Command Line:**
```bash
dotnet add package Syncfusion.Maui.ListView
```

**Package Manager Console:**
```powershell
Install-Package Syncfusion.Maui.ListView
```

### Step 3: Restore Dependencies

After installation, restore dependencies:
```bash
dotnet restore
```

## Registering the Handler

The Syncfusion.Maui.Core package (automatically installed as a dependency) requires handler registration.

**Open `MauiProgram.cs` and add:**

```csharp
using Microsoft.Maui.Controls.Hosting;
using Microsoft.Maui.Hosting;
using Syncfusion.Maui.Core.Hosting;  // Add this using

namespace MyListViewApp
{
    public static class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            var builder = MauiApp.CreateBuilder();
            builder
                .UseMauiApp<App>()
                .ConfigureFonts(fonts =>
                {
                    fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
                    fonts.AddFont("OpenSans-Semibold.ttf", "OpenSansSemibold");
                });

            // Register Syncfusion handler
            builder.ConfigureSyncfusionCore();
            
            return builder.Build();
        }
    }
}
```

**Important:** `ConfigureSyncfusionCore()` must be called before `Build()`.

## Creating Your First ListView

### Step 1: Add ListView to XAML

Open `MainPage.xaml` and add the ListView:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:syncfusion="clr-namespace:Syncfusion.Maui.ListView;assembly=Syncfusion.Maui.ListView"
             x:Class="MyListViewApp.MainPage">

    <syncfusion:SfListView x:Name="listView" />
    
</ContentPage>
```

### Step 2: Add ListView in Code-Behind

Alternatively, create ListView in C#:

```csharp
using Syncfusion.Maui.ListView;

namespace MyListViewApp
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            
            SfListView listView = new SfListView();
            this.Content = listView;
        }
    }
}
```

## Data Model and ViewModel

### Create Data Model

Create a new class `BookInfo.cs`:

```csharp
using System.ComponentModel;

namespace MyListViewApp.Models
{
    public class BookInfo : INotifyPropertyChanged
    {
        private string bookName;
        private string bookDescription;
        private string author;
        
        public string BookName
        {
            get => bookName;
            set
            {
                if (bookName != value)
                {
                    bookName = value;
                    OnPropertyChanged(nameof(BookName));
                }
            }
        }
        
        public string BookDescription
        {
            get => bookDescription;
            set
            {
                if (bookDescription != value)
                {
                    bookDescription = value;
                    OnPropertyChanged(nameof(BookDescription));
                }
            }
        }
        
        public string Author
        {
            get => author;
            set
            {
                if (author != value)
                {
                    author = value;
                    OnPropertyChanged(nameof(Author));
                }
            }
        }
        
        public event PropertyChangedEventHandler PropertyChanged;
        
        protected void OnPropertyChanged(string propertyName)
        {
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
        }
    }
}
```

**Why INotifyPropertyChanged?** It enables automatic UI updates when property values change at runtime.

### Create ViewModel

Create `BookInfoRepository.cs`:

```csharp
using System.Collections.ObjectModel;

namespace MyListViewApp.ViewModels
{
    public class BookInfoRepository
    {
        private ObservableCollection<BookInfo> bookInfo;
        
        public ObservableCollection<BookInfo> BookInfo
        {
            get => bookInfo;
            set => bookInfo = value;
        }
        
        public BookInfoRepository()
        {
            GenerateBookInfo();
        }
        
        private void GenerateBookInfo()
        {
            bookInfo = new ObservableCollection<BookInfo>
            {
                new BookInfo 
                { 
                    BookName = "Object-Oriented Programming in C#",
                    BookDescription = "Object-oriented programming is a programming paradigm based on the concept of objects",
                    Author = "John Smith"
                },
                new BookInfo 
                { 
                    BookName = "C# Code Contracts",
                    BookDescription = "Code Contracts provide a way to convey code assumptions",
                    Author = "Jane Doe"
                },
                new BookInfo 
                { 
                    BookName = "Machine Learning Using C#",
                    BookDescription = "Learn machine learning approaches with C#",
                    Author = "Mike Johnson"
                },
                new BookInfo 
                { 
                    BookName = "Neural Networks Using C#",
                    BookDescription = "Neural networks are an exciting field of software development",
                    Author = "Sarah Williams"
                },
                new BookInfo 
                { 
                    BookName = "Visual Studio Code",
                    BookDescription = "A powerful tool for editing code and end-to-end programming",
                    Author = "Chris Brown"
                }
            };
        }
    }
}
```

**Why ObservableCollection?** It automatically notifies the ListView when items are added, removed, or changed.

## Binding Data to ListView

### Method 1: XAML Binding

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:syncfusion="clr-namespace:Syncfusion.Maui.ListView;assembly=Syncfusion.Maui.ListView"
             xmlns:local="clr-namespace:MyListViewApp.ViewModels"
             x:Class="MyListViewApp.MainPage">
             
    <ContentPage.BindingContext>
        <local:BookInfoRepository />
    </ContentPage.BindingContext>
    
    <syncfusion:SfListView x:Name="listView"
                           ItemsSource="{Binding BookInfo}" />
    
</ContentPage>
```

### Method 2: Code-Behind Binding

```csharp
using MyListViewApp.ViewModels;

namespace MyListViewApp
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            
            BookInfoRepository viewModel = new BookInfoRepository();
            listView.ItemsSource = viewModel.BookInfo;
        }
    }
}
```

## Defining Item Templates

Without a template, ListView displays items using `ToString()`. Define an ItemTemplate for custom UI.

### Basic Item Template

```xml
<syncfusion:SfListView x:Name="listView"
                       ItemsSource="{Binding BookInfo}"
                       ItemSize="100">
    <syncfusion:SfListView.ItemTemplate>
        <DataTemplate>
            <Grid Padding="10">
                <Grid.RowDefinitions>
                    <RowDefinition Height="0.4*" />
                    <RowDefinition Height="0.6*" />
                </Grid.RowDefinitions>
                <Label Text="{Binding BookName}" 
                       FontAttributes="Bold" 
                       TextColor="Teal" 
                       FontSize="21" />
                <Label Grid.Row="1" 
                       Text="{Binding BookDescription}" 
                       TextColor="Gray" 
                       FontSize="15" />
            </Grid>
        </DataTemplate>
    </syncfusion:SfListView.ItemTemplate>
</syncfusion:SfListView>
```

### Advanced Item Template with Image

```xml
<syncfusion:SfListView.ItemTemplate>
    <DataTemplate>
        <Grid Padding="10" ColumnSpacing="10">
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="60" />
                <ColumnDefinition Width="*" />
            </Grid.ColumnDefinitions>
            
            <Image Source="{Binding BookCover}" 
                   Aspect="AspectFill"
                   HeightRequest="60"
                   WidthRequest="60" />
                   
            <StackLayout Grid.Column="1" VerticalOptions="Center">
                <Label Text="{Binding BookName}" 
                       FontAttributes="Bold" 
                       FontSize="16" />
                <Label Text="{Binding Author}" 
                       FontSize="12" 
                       TextColor="Gray" />
                <Label Text="{Binding BookDescription}" 
                       FontSize="11" 
                       TextColor="DarkGray"
                       LineBreakMode="TailTruncation" />
            </StackLayout>
        </Grid>
    </DataTemplate>
</syncfusion:SfListView.ItemTemplate>
```

## Running the Application

### Visual Studio
- Press **F5** or click **Debug > Start Debugging**
- Select target platform (Android, iOS, Windows, MacCatalyst)

### Visual Studio Code
- Open terminal: `Ctrl+`` 
- Run: `dotnet build -t:Run -f net8.0-android` (or -ios, -windows, -maccatalyst)

### Command Line
```bash
# Android
dotnet build -t:Run -f net8.0-android

# iOS (Mac only)
dotnet build -t:Run -f net8.0-ios

# Windows
dotnet build -t:Run -f net8.0-windows10.0.19041.0

# Mac Catalyst (Mac only)
dotnet build -t:Run -f net8.0-maccatalyst
```

## Basic Customization

### Setting Item Height

```xml
<syncfusion:SfListView ItemSize="80" />
```

### Changing Background Color

```xml
<syncfusion:SfListView BackgroundColor="WhiteSmoke" />
```

### Adding Item Spacing

```xml
<syncfusion:SfListView.ItemTemplate>
    <DataTemplate>
        <Frame Margin="10,5" Padding="10" CornerRadius="8" HasShadow="True">
            <!-- Your content here -->
        </Frame>
    </DataTemplate>
</syncfusion:SfListView.ItemTemplate>
```

## Next Steps

Now that you have a basic ListView running, explore these features:

1. **Layouts** → Learn about linear and grid layouts
2. **Data Operations** → Add sorting, filtering, and grouping
3. **Selection** → Enable single or multiple item selection
4. **Item Customization** → Create rich, custom item templates
5. **Interactive Features** → Add swipe actions and drag-drop
6. **Performance Optimization** → Optimize for large datasets

## Troubleshooting

**Issue:** "Handler not registered" error
**Solution:** Ensure `ConfigureSyncfusionCore()` is called in `MauiProgram.cs`

**Issue:** Items not displaying
**Solution:** Check that ItemsSource has data and ItemTemplate is defined

**Issue:** NuGet package errors
**Solution:** Run `dotnet restore` and rebuild the project

**Issue:** Hot Reload not working
**Solution:** Restart the application after adding Syncfusion packages
