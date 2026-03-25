# Getting Started with .NET MAUI Expander

## Table of Contents
- [Overview](#overview)
- [Step 1: Create a New .NET MAUI Project](#step-1-create-a-new-net-maui-project)
- [Step 2: Install NuGet Package](#step-2-install-nuget-package)
- [Step 3: Register the Handler](#step-3-register-the-handler)
- [Step 4: Add a Basic Expander](#step-4-add-a-basic-expander)
- [Step 5: Define Header and Content](#step-5-define-header-and-content)
- [Step 6: Running the Application](#step-6-running-the-application)
- [Complete Invoice Example](#complete-invoice-example)
- [Important Notes](#important-notes)

---

## Overview

This guide walks through setting up and configuring a Syncfusion .NET MAUI Expander (SfExpander) from scratch. The Expander is a layout control that allows users to expand or collapse content by tapping the header.

**What you'll learn:**
- Installing the Syncfusion.Maui.Expander NuGet package
- Registering the Syncfusion handler in MauiProgram.cs
- Creating a basic expander with XAML and C#
- Defining header and content views
- Building a complete invoice layout with multiple expanders

---

## Step 1: Create a New .NET MAUI Project

### Visual Studio

1. Open Visual Studio 2022
2. Go to **File > New > Project**
3. Search for ".NET MAUI App" template
4. Enter Project Name and Location
5. Select .NET 9.0 or later as the framework
6. Click **Create**

### JetBrains Rider

1. Go to **File > New Solution**
2. Select **.NET (C#)** and choose the **.NET MAUI App** template
3. Enter the Project Name, Solution Name, and Location
4. Select the .NET framework version
5. Click **Create**

---

## Step 2: Install NuGet Package

### Visual Studio

1. In **Solution Explorer**, right-click the project
2. Choose **Manage NuGet Packages**
3. Search for `Syncfusion.Maui.Expander`
4. Install the latest version

### JetBrains Rider

1. In **Solution Explorer**, right-click the project
2. Choose **Manage NuGet Packages**
3. Search for `Syncfusion.Maui.Expander`
4. Install the latest version
5. If dependencies aren't restored, open Terminal and run:
   ```bash
   dotnet restore
   ```

### Package Manager Console

```bash
Install-Package Syncfusion.Maui.Expander
```

### .NET CLI

```bash
dotnet add package Syncfusion.Maui.Expander
```

---

## Step 3: Register the Handler

The `Syncfusion.Maui.Core` NuGet is a dependent package for all Syncfusion controls. Register the handler in `MauiProgram.cs`:

```csharp
using Microsoft.Maui;
using Microsoft.Maui.Hosting;
using Microsoft.Maui.Controls.Hosting;
using Syncfusion.Maui.Core.Hosting;

namespace GettingStarted
{
    public class MauiProgram 
    {
        public static MauiApp CreateMauiApp()
        {
            var builder = MauiApp.CreateBuilder();
            builder
                .UseMauiApp<App>()
                .ConfigureFonts(fonts =>
                {
                    fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
                });

            // Register Syncfusion handler
            builder.ConfigureSyncfusionCore();
            
            return builder.Build();
        }
    }
}
```

**Key point:** The `ConfigureSyncfusionCore()` method must be called to initialize Syncfusion controls.

---

## Step 4: Add a Basic Expander

Import the namespace and initialize the control.

### XAML

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:syncfusion="clr-namespace:Syncfusion.Maui.Expander;assembly=Syncfusion.Maui.Expander"
             x:Class="GettingStarted.MainPage">
    
    <syncfusion:SfExpander />
    
</ContentPage>
```

### C#

```csharp
using Syncfusion.Maui.Expander;

public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        SfExpander expander = new SfExpander();
        this.Content = expander;
    }
}
```

---

## Step 5: Define Header and Content

The Expander consists of two main parts:
- **Header:** Always visible, tap to expand/collapse
- **Content:** Toggles visibility when header is tapped

**Important:** The `IsExpanded` property controls the initial state (default: `false`).

### XAML Example

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:syncfusion="clr-namespace:Syncfusion.Maui.Expander;assembly=Syncfusion.Maui.Expander"
             x:Class="GettingStarted.MainPage">
    
    <syncfusion:SfExpander AnimationDuration="200" IsExpanded="True">
        
        <syncfusion:SfExpander.Header>
            <Grid>
                <Grid.RowDefinitions>
                    <RowDefinition Height="48"/>
                </Grid.RowDefinitions>
                <Label Text="Invoice Date" 
                       FontSize="14" 
                       VerticalOptions="CenterAndExpand"/>
            </Grid>
        </syncfusion:SfExpander.Header>
        
        <syncfusion:SfExpander.Content>
            <Grid Padding="18,8,0,18">
                <Label Text="11:03 AM, 15 January 2019" 
                       FontSize="14" 
                       VerticalOptions="CenterAndExpand"/>
            </Grid>
        </syncfusion:SfExpander.Content>
        
    </syncfusion:SfExpander>
    
</ContentPage>
```

### C# Example

```csharp
using Syncfusion.Maui.Expander;

public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        
        var expander = new SfExpander
        {
            AnimationDuration = 200,
            IsExpanded = true
        };
        
        // Define Header
        var headerGrid = new Grid();
        headerGrid.RowDefinitions.Add(new RowDefinition { Height = 48 });
        headerGrid.Children.Add(new Label 
        { 
            Text = "Invoice Date", 
            FontSize = 14, 
            VerticalOptions = LayoutOptions.CenterAndExpand 
        });
        expander.Header = headerGrid;
        
        // Define Content
        var contentGrid = new Grid { Padding = new Thickness(18, 8, 0, 18) };
        contentGrid.Children.Add(new Label 
        { 
            Text = "11:03 AM, 15 January 2019", 
            FontSize = 14, 
            VerticalOptions = LayoutOptions.CenterAndExpand 
        });
        expander.Content = contentGrid;
        
        this.Content = expander;
    }
}
```

### ⚠️ Critical: Label as Direct Child

**Do NOT load Label as a direct child of Header or Content** - this will cause an exception.

```xml
<!-- ❌ WRONG - Will crash -->
<syncfusion:SfExpander.Header>
    <Label Text="Header"/>
</syncfusion:SfExpander.Header>

<!-- ✅ CORRECT - Wrap in container -->
<syncfusion:SfExpander.Header>
    <Grid>
        <Label Text="Header"/>
    </Grid>
</syncfusion:SfExpander.Header>
```

**Why:** The Expander expects a layout container (Grid, StackLayout, etc.) as the direct child, not individual controls.

---

## Step 6: Running the Application

Press **F5** in Visual Studio or Rider to build and run the application. The expander will display with the header and content you defined.

---

## Complete Invoice Example

A real-world example with multiple expanders showing invoice details:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:syncfusion="clr-namespace:Syncfusion.Maui.Expander;assembly=Syncfusion.Maui.Expander"
             x:Class="GettingStarted.MainPage">
    
    <ContentPage.Content>
        <ScrollView>
            <StackLayout HorizontalOptions="{OnPlatform MacCatalyst=Center,WinUI=Center}">
                
                <Label Text="Invoice: #FRU037020142097" 
                       Opacity="1.0" 
                       VerticalTextAlignment="Center" 
                       Margin="0,0,0,5" 
                       FontAttributes="Bold" 
                       VerticalOptions="Center" 
                       HorizontalOptions="CenterAndExpand"/>
                
                <!-- Invoice Date -->
                <Border StrokeShape="RoundRectangle 8,8,8,8" 
                        Margin="8,0,8,8" 
                        Stroke="#CAC4D0" 
                        StrokeThickness="1" 
                        WidthRequest="{OnPlatform MacCatalyst=460,WinUI=340}">
                    <syncfusion:SfExpander AnimationDuration="200" IsExpanded="True">
                        <syncfusion:SfExpander.Header>
                            <Grid>
                                <Grid.RowDefinitions>
                                    <RowDefinition Height="48"/>
                                </Grid.RowDefinitions>
                                <Grid.ColumnDefinitions>
                                    <ColumnDefinition Width="35"/>
                                    <ColumnDefinition Width="*"/>
                                </Grid.ColumnDefinitions>
                                <Label Text="&#xe703;" 
                                       FontSize="16" 
                                       Margin="14,2,2,2"
                                       FontFamily='{OnPlatform Android=AccordionFontIcons.ttf#,WinUI=AccordionFontIcons.ttf#AccordionFontIcons,MacCatalyst=AccordionFontIcons,iOS=AccordionFontIcons}'
                                       VerticalOptions="Center" 
                                       VerticalTextAlignment="Center"/>
                                <Label CharacterSpacing="0.25" 
                                       FontFamily="Roboto-Regular" 
                                       Text="Invoice Date" 
                                       FontSize="14" 
                                       Grid.Column="1" 
                                       VerticalOptions="CenterAndExpand"/>
                            </Grid>
                        </syncfusion:SfExpander.Header>
                        <syncfusion:SfExpander.Content>
                            <Grid Padding="18,8,0,18">
                                <Label CharacterSpacing="0.25" 
                                       FontFamily="Roboto-Regular" 
                                       Text="11:03 AM, 15 January 2019" 
                                       FontSize="14" 
                                       VerticalOptions="CenterAndExpand"/>
                            </Grid>
                        </syncfusion:SfExpander.Content>
                    </syncfusion:SfExpander>
                </Border>
                
                <!-- Items Section -->
                <Border StrokeShape="RoundRectangle 8,8,8,8" 
                        Margin="8,0,8,8" 
                        Stroke="#CAC4D0" 
                        StrokeThickness="1" 
                        WidthRequest="{OnPlatform MacCatalyst=460,WinUI=340}">
                    <syncfusion:SfExpander AnimationDuration="200" IsExpanded="False">
                        <syncfusion:SfExpander.Header>
                            <Grid>
                                <Grid.RowDefinitions>
                                    <RowDefinition Height="48"/>
                                </Grid.RowDefinitions>
                                <Grid.ColumnDefinitions>
                                    <ColumnDefinition Width="35"/>
                                    <ColumnDefinition Width="*"/>
                                </Grid.ColumnDefinitions>
                                <Label Text="&#xe701;" 
                                       FontSize="16" 
                                       Margin="14,2,2,2"
                                       FontFamily='{OnPlatform Android=AccordionFontIcons.ttf#,WinUI=AccordionFontIcons.ttf#AccordionFontIcons,MacCatalyst=AccordionFontIcons,iOS=AccordionFontIcons}'
                                       VerticalOptions="Center" 
                                       VerticalTextAlignment="Center"/>
                                <Label CharacterSpacing="0.25" 
                                       FontFamily="Roboto-Regular" 
                                       Text="Item(s)" 
                                       FontSize="14" 
                                       Grid.Column="1" 
                                       VerticalOptions="CenterAndExpand"/>
                            </Grid>
                        </syncfusion:SfExpander.Header>
                        <syncfusion:SfExpander.Content>
                            <Grid Padding="18,8,18,18">
                                <Grid.RowDefinitions>
                                    <RowDefinition Height="20"/>
                                    <RowDefinition Height="20"/>
                                    <RowDefinition Height="20"/>
                                </Grid.RowDefinitions>
                                <Grid.ColumnDefinitions>
                                    <ColumnDefinition Width="*"/>
                                    <ColumnDefinition Width="*"/>
                                </Grid.ColumnDefinitions>
                                <Label FontSize="14" Text="2018 Subaru Outback"/>
                                <Label FontSize="14" Text="All-Weather Mats" Grid.Row="1"/>
                                <Label FontSize="14" Text="Total Amount" Grid.Row="2"/>
                                <Label FontSize="14" HorizontalOptions="End" Text="$35,705.00" Grid.Column="1"/>
                                <Label FontSize="14" HorizontalOptions="End" Text="$101.00" Grid.Row="1" Grid.Column="1"/>
                                <Label FontSize="14" HorizontalOptions="End" Text="$36,220.00" Grid.Row="2" Grid.Column="1"/>
                            </Grid>
                        </syncfusion:SfExpander.Content>
                    </syncfusion:SfExpander>
                </Border>
                
            </StackLayout>
        </ScrollView>
    </ContentPage.Content>
</ContentPage>
```

---

## Important Notes

### IsExpanded Property

Controls initial expansion state:
- `IsExpanded="True"` - Content visible on load
- `IsExpanded="False"` - Content hidden on load (default)

### AnimationDuration Property

Controls expand/collapse animation speed in milliseconds:
- Default: 300ms
- Faster animations: 150-200ms
- Smoother animations: 400-600ms

### Container Requirement

Always wrap UI elements in a layout container (Grid, StackLayout, etc.) when placing them in Header or Content. Direct Label children will cause runtime exceptions.

### Platform-Specific Styling

Use `OnPlatform` markup extension for platform-specific values:
```xml
WidthRequest="{OnPlatform MacCatalyst=460,WinUI=340,Default=360}"
```

---

## Related Features

- **Animation and Events:** See `animation-events.md` for animation customization and event handling
- **Appearance:** See `appearance-styling.md` for header styling and Visual State Manager
- **Liquid Glass Effect:** See `liquid-glass-effect.md` for modern translucent designs

---

## Sample Project

View complete sample on GitHub: [Getting Started with .NET MAUI Expander](https://github.com/SyncfusionExamples/getting-started-with-.net-maui-expander)
