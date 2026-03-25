# Getting Started with .NET MAUI Picker

This guide covers the installation, setup, and basic implementation of the Syncfusion .NET MAUI Picker (SfPicker) control.

## Table of Contents
- [Installation](#installation)
- [Handler Registration](#handler-registration)
- [Basic Implementation](#basic-implementation)
- [Icon Button Implementation](#icon-button-implementation)
- [Complete Examples](#complete-examples)

## Installation Steps

### Step 1: Create a New .NET MAUI Project

#### Visual Studio

1. Go to **File > New > Project**
2. Choose the **.NET MAUI App** template
3. Name the project and choose a location
4. Click **Next**
5. Select the .NET framework version
6. Click **Create**

### Step 2: Install the Syncfusion .NET MAUI Picker NuGet Package

#### Visual Studio / JetBrains Rider

1. In **Solution Explorer**, right-click the project
2. Choose **Manage NuGet Packages**
3. Search for **Syncfusion.Maui.Picker**
4. Install the latest version
5. Ensure all dependencies are installed correctly

#### Visual Studio Code

1. Press `Ctrl` + `` ` `` (backtick) to open the integrated terminal
2. Ensure you're in the project root directory
3. Run the command:
   ```bash
   dotnet add package Syncfusion.Maui.Picker
   ```
4. Run `dotnet restore` to ensure all dependencies are installed

### Step 3: Register the Syncfusion Handler

The **Syncfusion.Maui.Core** NuGet package is a dependent package for all Syncfusion controls. You must register the handler in **MauiProgram.cs**.

**MauiProgram.cs:**
```csharp
using Syncfusion.Maui.Core.Hosting;

namespace GettingStarted
{
    public static class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            var builder = MauiApp.CreateBuilder();

            // Register Syncfusion core handler
            builder.ConfigureSyncfusionCore();
            
            builder
                .UseMauiApp<App>()
                .ConfigureFonts(fonts =>
                {
                    fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
                });

            return builder.Build();
        }
    }
}
```

**Key Points:**
- Import `Syncfusion.Maui.Core.Hosting` namespace
- Call `builder.ConfigureSyncfusionCore()` before building the app
- This step is required for all Syncfusion .NET MAUI controls

## Basic Picker Implementation

### Step 4: Add .NET MAUI Picker Control

#### XAML Implementation

1. Import the `Syncfusion.Maui.Picker` namespace in your XAML file
2. Initialize the SfPicker control

**MainPage.xaml:**
```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:picker="clr-namespace:Syncfusion.Maui.Picker;assembly=Syncfusion.Maui.Picker"
             x:Class="GettingStarted.MainPage">

    <picker:SfPicker x:Name="picker" />

</ContentPage>
```

#### C# Implementation

**MainPage.xaml.cs:**
```csharp
using Syncfusion.Maui.Picker;

namespace GettingStarted
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            
            SfPicker picker = new SfPicker();
            this.Content = picker;
        }
    }
}
```

## Adding Header to the Picker

Set header text using the `PickerHeaderView` to provide context for the user.

**XAML:**
```xml
<picker:SfPicker x:Name="picker">
    <picker:SfPicker.HeaderView>
        <picker:PickerHeaderView Text="Select a color" Height="40" />
    </picker:SfPicker.HeaderView>
</picker:SfPicker>
```

**C#:**
```csharp
SfPicker picker = new SfPicker();
picker.HeaderView = new PickerHeaderView()
{
    Text = "Select a color",
    Height = 40,
};

this.Content = picker;
```

**Important:** Set the `Height` property to a value greater than 0 to display the header.

## Adding Picker Items

The Picker is a data-bound control. You need to create a data collection and bind it to the picker.

### Create a Data Source

**ItemInfo.cs:**
```csharp
using System.Collections.ObjectModel;

public class ItemInfo
{
    private ObservableCollection<object> dataSource;

    public ObservableCollection<object> DataSource
    {
        get { return dataSource; }
        set { dataSource = value; }
    }

    public ItemInfo()
    {
        dataSource = new ObservableCollection<object>()
        {
            "Pink", "Green", "Blue", "Yellow", 
            "Orange", "Purple", "SkyBlue", "PaleGreen"
        };
    }
}
```

### Bind the Data Collection to Picker

**XAML:**
```xml
<ContentPage xmlns:local="clr-namespace:GettingStarted">
    
    <picker:SfPicker x:Name="picker">
        <picker:SfPicker.HeaderView>
            <picker:PickerHeaderView Text="Select a color" Height="40" />
        </picker:SfPicker.HeaderView>
        
        <picker:SfPicker.Columns>
            <picker:PickerColumn ItemsSource="{Binding DataSource}" />
        </picker:SfPicker.Columns>
    </picker:SfPicker>

    <ContentPage.BindingContext>
        <local:ItemInfo />
    </ContentPage.BindingContext>

</ContentPage>
```

**C#:**
```csharp
ItemInfo itemInfo = new ItemInfo();

SfPicker picker = new SfPicker()
{
    Columns = new ObservableCollection<PickerColumn>()
    {
        new PickerColumn()
        {
            ItemsSource = itemInfo.DataSource,
        }
    }
};

this.Content = picker;
```

## Adding Footer with Validation Buttons

Add OK and Cancel buttons to the footer for user validation.

**XAML:**
```xml
<picker:SfPicker x:Name="picker">
    <picker:SfPicker.FooterView>
        <picker:PickerFooterView ShowOkButton="True" Height="40" />
    </picker:SfPicker.FooterView>
</picker:SfPicker>
```

**C#:**
```csharp
SfPicker picker = new SfPicker();
picker.FooterView = new PickerFooterView()
{  
    ShowOkButton = true,
    Height = 40,
};

this.Content = picker;
```

**Key Points:**
- Set `ShowOkButton` to `true` to display the OK button
- Set `Height` to a value greater than 0 to display the footer
- Both OK and Cancel buttons are available by default

## Adding Column Headers

Set column header text to identify what each column represents.

**XAML:**
```xml
<picker:SfPicker x:Name="picker">
    <picker:SfPicker.HeaderView>
        <picker:PickerHeaderView Text="Select a color" Height="40" />
    </picker:SfPicker.HeaderView>

    <picker:SfPicker.Columns>
        <picker:PickerColumn HeaderText="Colors" 
                             ItemsSource="{Binding DataSource}" />
    </picker:SfPicker.Columns>

    <picker:SfPicker.ColumnHeaderView>
        <picker:PickerColumnHeaderView Height="40" />
    </picker:SfPicker.ColumnHeaderView>
</picker:SfPicker>
```

**C#:**
```csharp
ItemInfo itemInfo = new ItemInfo();

SfPicker picker = new SfPicker()
{
    HeaderView = new PickerHeaderView()
    {
        Text = "Select a color",
        Height = 40,
    },

    Columns = new ObservableCollection<PickerColumn>()
    {
        new PickerColumn()
        {
            HeaderText = "Colors",
            ItemsSource = itemInfo.DataSource,
        }
    },

    ColumnHeaderView = new PickerColumnHeaderView()
    {
        Height = 40,
    },
};

this.Content = picker;
```

## Setting Height and Width

Control the picker dimensions using `HeightRequest` and `WidthRequest`.

**XAML:**
```xml
<picker:SfPicker x:Name="picker" 
                 HeightRequest="280" 
                 WidthRequest="300">
</picker:SfPicker>
```

**C#:**
```csharp
SfPicker picker = new SfPicker()
{
    HeightRequest = 280,
    WidthRequest = 300,
};

this.Content = picker;
```

## Complete Example

Here's a complete working example that combines all the basic features:

**MainPage.xaml:**
```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:picker="clr-namespace:Syncfusion.Maui.Picker;assembly=Syncfusion.Maui.Picker"
             xmlns:local="clr-namespace:GettingStarted"
             x:Class="GettingStarted.MainPage">
    
    <picker:SfPicker x:Name="picker"
                     HeightRequest="280"
                     WidthRequest="300">
        
        <picker:SfPicker.HeaderView>
            <picker:PickerHeaderView Text="Select a color" Height="40" />
        </picker:SfPicker.HeaderView>
        
        <picker:SfPicker.Columns>
            <picker:PickerColumn HeaderText="Colors" 
                                 ItemsSource="{Binding DataSource}" />
        </picker:SfPicker.Columns>
        
        <picker:SfPicker.ColumnHeaderView>
            <picker:PickerColumnHeaderView Height="40" />
        </picker:SfPicker.ColumnHeaderView>
        
        <picker:SfPicker.FooterView>
            <picker:PickerFooterView ShowOkButton="True" Height="40" />
        </picker:SfPicker.FooterView>
        
    </picker:SfPicker>
    
    <ContentPage.BindingContext>
        <local:ItemInfo />
    </ContentPage.BindingContext>
    
</ContentPage>
```

## Common Issues and Troubleshooting

### Issue: Picker Not Displaying

**Solution:**
- Verify that `ConfigureSyncfusionCore()` is called in `MauiProgram.cs`
- Ensure the NuGet package is properly installed
- Check that the namespace is correctly imported

### Issue: No Items Showing

**Solution:**
- Verify that `ItemsSource` is bound to a valid collection
- Check that the binding context is set correctly
- Ensure the data source is not empty

### Issue: Header/Footer Not Visible

**Solution:**
- Set `Height` property to a value greater than 0
- Verify that the view is properly initialized

## Next Steps

Now that you have a basic picker implementation:

- Explore different **picker modes** (Dialog, RelativeDialog) for different UI experiences
- Learn about **multi-column pickers** for complex selection scenarios
- Customize **appearance and styling** to match your app theme
- Implement **event handlers** for user interactions
- Add **accessibility features** for better user experience
