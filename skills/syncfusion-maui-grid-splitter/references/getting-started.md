# Getting Started with .NET MAUI Grid Splitter

## Table of Contents
- [Prerequisites](#prerequisites)
- [Project Setup](#project-setup)
- [Basic Implementation](#basic-implementation)
- [Creating Your First Layout](#creating-your-first-layout)
- [Adding Content to Panes](#adding-content-to-panes)
- [Programmatic Pane Creation](#programmatic-pane-creation)

## Installation and Setup

### Step 1: Install NuGet Package

The SfGridSplitter component is included in the **Syncfusion.Maui.GridSplitter** NuGet package.

**Visual Studio Method:**
1. Right-click your project → **Manage NuGet Packages**
2. Search for `Syncfusion.Maui.GridSplitter`
3. Install the latest version

**Command Line Method:**

```bash
dotnet add package Syncfusion.Maui.GridSplitter
```

### Step 2: Register the Handler

In your `MauiProgram.cs` file, register the Syncfusion handler:

```csharp
using Syncfusion.Maui.Core.Hosting;

public static class MauiProgram
{
    public static MauiApp CreateMauiApp()
    {
        var builder = MauiApp.CreateBuilder();
        builder
            .ConfigureSyncfusionCore()
            .UseMauiApp<App>()
            .ConfigureFonts(fonts =>
            {
                fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
                fonts.AddFont("OpenSans-Semibold.ttf", "OpenSansSemibold");
            });

        return builder.Build();
    }
}
```

### Step 3: Add the Required Namespace

In your XAML files, add the GridSplitter namespace declaration:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage
    xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    xmlns:gridSplitter="clr-namespace:Syncfusion.Maui.GridSplitter;assembly=Syncfusion.Maui.GridSplitter"
    Title="My App">

</ContentPage>
```

In C# code-behind, add the using statement:

```csharp
using Syncfusion.Maui.GridSplitter;
```

---

## Creating Your First GridSplitter

### XAML Approach

The simplest way to create a GridSplitter is using XAML:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage
    xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    xmlns:gridSplitter="clr-namespace:Syncfusion.Maui.GridSplitter;assembly=Syncfusion.Maui.GridSplitter"
    Title="GridSplitter Demo">

    <ContentPage.Content>
        <gridSplitter:SfGridSplitter Orientation="Horizontal">

            <gridSplitter:SplitterPane Size="250">
                <VerticalStackLayout Padding="10">
                    <Label Text="Navigation" />
                </VerticalStackLayout>
            </gridSplitter:SplitterPane>

            <gridSplitter:SplitterPane Size="1*">
                <VerticalStackLayout Padding="10">
                    <Label Text="Content Area" />
                </VerticalStackLayout>
            </gridSplitter:SplitterPane>

        </gridSplitter:SfGridSplitter>
    </ContentPage.Content>

</ContentPage>
```

### C# Approach

Create  GridSplitter programmatically in *ode-behind:

```csharp
using Syncfusion.Maui.GridSplitter;

namespace GridSplitterDemo
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();

            var gridSplitter = new SfGridSplitter
            {
                Orientation = GridSplitterOrientation.Horizontal
            }
            ;
            var navigationPane = new SplitterPane
            {
                Size = "250",
                Content = new VerticalStackLayout
                {
                    Padding = 10,
                    Children =
                    {
                        new Label { Text = "Navigation" }
                    }
                }
            }
            ;

            var contentPane = new SplitterPane
            {
                Size = "1*",
                Content = new VerticalStackLayout
                {
                    Padding = 10,
                    Children =
                    {
                        new Label { Text = "Content Area" }
                    }
                }
            };

            gridSplitter.AddPane(navigationPane);
            gridSplitter.AddPane(contentPane);

            this.Content = gridSplitter;
        }
    }
}
```

---

## Adding Panes and Content

### Static Pane Configuration

You can add multiple panes directly in XAML:

```xaml
<gridSplitter:SfGridSplitter Orientation="Horizontal">

    <gridSplitter:SplitterPane Size="200">
        <Label Text="Filters"
               HorizontalTextAlignment="Center"
               VerticalTextAlignment="Center"/>
    </gridSplitter:SplitterPane>

    <gridSplitter:SplitterPane Size="2*">
        <Label Text="Dashboard Charts"
               HorizontalTextAlignment="Center"
               VerticalTextAlignment="Center"/>
    </gridSplitter:SplitterPane>

    <gridSplitter:SplitterPane Size="300">
        <Label Text="Details"
               HorizontalTextAlignment="Center"
               VerticalTextAlignment="Center"/>
    </gridSplitter:SplitterPane>

</gridSplitter:SfGridSplitter>
```

### Pane with Forms

Each pane can host any MAUI view or layout.

```xaml
<gridSplitter:SplitterPane Size="1*">
    <VerticalStackLayout Padding="20" Spacing="10">
        <Entry Placeholder="Name"/>
        <Entry Placeholder="Email"/>
        <Button Text="Submit"/>
    </VerticalStackLayout>
</gridSplitter:SplitterPane>
```

### Pane with Scrollable Content

```xaml
<gridSplitter:SplitterPane Size="1*">
    <ScrollView>
        <VerticalStackLayout Padding="10" Spacing="5">
            <Label Text="Item 1"/>
            <Label Text="Item 2"/>
            <Label Text="Item 3"/>
            <Label Text="Item 4"/>
            <Label Text="Item 5"/>
        </VerticalStackLayout>
    </ScrollView>
</gridSplitter:SplitterPane>
```

### Programmatic Pane Addition (C#)

```csharp
var gridSplitter = new SfGridSplitter
{
    Orientation = GridSplitterOrientation.Horizontal
};

for (int i = 1; i <= 3; i++)
{
    var pane = new SplitterPane
    {
        Size = "1*",
        Content = new Label
        {
            Text = $"Pane {i}",
            HorizontalTextAlignment = TextAlignment.Center,
            VerticalTextAlignment = TextAlignment.Center
        }
    };

    gridSplitter.AddPane(pane);
}

this.Content = gridSplitter;
```

## Dynamic Pane Creation

### Creating a Model

First, create a model class to represent pane data:

```csharp
public class PaneInfo
{
    public string Title { get; set; }

    public string Description { get; set; }
}
```

### Creating Pane Data

Create a collection that provides pane information:

```csharp
public class GridSplitterViewModel
{
    public List<PaneInfo> PaneItems { get; set; }

    public GridSplitterViewModel()
    {
        PaneItems = new List<PaneInfo>
        {
            new PaneInfo
                {
                Title = "Navigation",
                Description = "Menu links and navigation itims"
            },
            new PaneInfo
            {
                Title = "Dashboard",
                Description = "Main application content"
            },
            new PaneInfo
            {
                Title = "Details",
                Description = "Additional information panel"
                
            }
        };
    }
}
```

### Creating Panes Dynamically

```csharp
var viewModel = new GridSplitterViewModel();

var gridSplitter = new SfGridSplitter
{
    Orientation = GridSplitterOrientation.Horizontal
};
foreach (var item in viewModel.PaneItems)
{
    gridSplitter.AddPane(new SplitterPane
    {
        Size = "1*",
        Content = new VerticalStackLayout
        {
            Padding = 10,
            Children =
            {
                new Label
                {
                    Text = item.Title,
                    FontAttributes = FontAttributes.Bold
                },
                new Label
                {
                    Text = item.Description
                }
            }
        }
    });
}
```

### Reusable Pane Creation Method

```csharp
private SplitterPane CreatePane(string title, string description)
{
    return new SplitterPane
    {
        Size = "1*",
        Content = new VerticalStackLayout
        {
            Padding = 10,
            Children =
            {
                new Label
                {
                    Text = title,
                    FontAttributes = FontAttributes.Bold
                },
                new Label
                {
                    Text = description
                }
            }
        }
    };
}
```

---

## Orientation Modes

### Horizontal Orientation

Define panes side by side:

```xaml
<gridSplitter:SfGridSplitter Orientation="Horizontal">

    <gridSplitter:SplitterPane Size="250">
        <Label Text="Left Pane"/>
    </gridSplitter:SplitterPane>

    <gridSplitter:SplitterPane Size="1*">
        <Label Text="Right Pane"/>
    </gridSplitter:SplitterPane>

</gridSplitter:SfGridSplitter>
```

### Vertical Orientation

Define panes from top to bottom:

```xaml
<gridSplitter:SfGridSplitter Orientation="Vertical">

    <gridSplitter:SplitterPane Size="200">
        <Label Text="Top Pane"/>
    </gridSplitter:SplitterPane>

    <gridSplitter:SplitterPane Size="1*">
        <Label Text="Bottom Pane"/>
    </gridSplitter:SplitterPane>

</gridSplitter:SfGridSplitter>
```

### C# Orientation Configuration

```csharp
var gridSplitter = new SfGridSplitter
{
    Orientation = GridSplitterOrientation.Vertical
};
```

---

## Common Setup Issues

### Issue 1: GridSplitter Not Showing

**Problem:** GridSplitter appears blank or doesn't render.

**Solution:**
- Verify `ConfigureSyncfusionCore()` is called in `MauiProgram.cs`
- Ensure the namespace is correctly declared
- Verify at least two panes are added to the GridSplitter
- Check that the control has sufficient layout space

### Issue 2: Pane Content Not Displaying

**Problem:** Pane content does not appear.

**Solution:**
- Verify the `Content` property is assigned for each pane
- Ensure layouts such as `Grid`, `VerticalStackLayout`, or `ScrollView` are used correctly
- Check size and layout constraints

### Issue 3: Unable to Resize Panes

**Problem:** Dragging the splitter does not resize panes.

**Solution:**
- Verify multiple panes are available
- Ensure pane sizes are configured correctly
- Test with both fixed-size and star-sized panes
- Verify the GridSplitter is not placed inside a layout with conflicting size restrictions

### Issue 4: Layout Not Resizing Correctly

**Problem:** Pane sizes are not updated as expected.

**Solution:**
- Use valid size values such as `200`, `300`, or `1*`
- Ensure enough space is available for all panes
- Avoid conflicting WidthRequest and HeightRequest values
- Verify the correct Orientation is configured