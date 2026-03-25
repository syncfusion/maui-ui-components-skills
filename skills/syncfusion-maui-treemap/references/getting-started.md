# Getting Started with .NET MAUI TreeMap

This guide walks through the complete setup and initial implementation of the Syncfusion .NET MAUI TreeMap (SfTreeMap) control, from installation to creating your first working TreeMap.

## Table of Contents
- [Installation](#installation)
- [Handler Registration](#handler-registration)
- [Creating Data Models](#creating-data-models)
- [Setting Up ViewModel](#setting-up-viewmodel)
- [Adding TreeMap to Your Page](#adding-treemap-to-your-page)
- [Configuring Basic Properties](#configuring-basic-properties)
- [Adding Labels](#adding-labels)
- [Applying Basic Styling](#applying-basic-styling)
- [Complete Working Example](#complete-working-example)
- [Platform-Specific Considerations](#platform-specific-considerations)
- [Troubleshooting](#troubleshooting)

## Installation

### Step 1: Create a New .NET MAUI Project

**Visual Studio 2022:**
1. Go to **File > New > Project**
2. Search for ".NET MAUI App" template
3. Name your project (e.g., "TreeMapDemo")
4. Choose a location
5. Click **Next**
6. Select the .NET framework version (.NET 9 or later)
7. Click **Create**

**Visual Studio Code:**
1. Open command palette (`Ctrl+Shift+P`)
2. Type ".NET: New Project" and press Enter
3. Select ".NET MAUI App" template
4. Choose project location and enter project name

### Step 2: Install Syncfusion.Maui.TreeMap NuGet Package

**Using Visual Studio:**
1. Right-click on your project in **Solution Explorer**
2. Select **Manage NuGet Packages**
3. Go to the **Browse** tab
4. Search for `Syncfusion.Maui.TreeMap`
5. Select the package and click **Install**
6. Accept the license agreement

**Using Package Manager Console:**
```powershell
Install-Package Syncfusion.Maui.TreeMap
```

**Using .NET CLI:**
```bash
dotnet add package Syncfusion.Maui.TreeMap
```

**Using Visual Studio Code:**
Right-click on the project, select "Manage NuGet Packages", search for `Syncfusion.Maui.TreeMap`, and install.

## Handler Registration

The Syncfusion .NET MAUI TreeMap requires handler registration before use. This must be done in your `MauiProgram.cs` file.

### Step 1: Import the Namespace

Add the following using statement at the top of `MauiProgram.cs`:

```csharp
using Syncfusion.Maui.Core.Hosting;
```

### Step 2: Register the Syncfusion Core Handler

Modify the `CreateMauiApp` method to include `ConfigureSyncfusionCore()`:

```csharp
public static class MauiProgram
{
    public static MauiApp CreateMauiApp()
    {
        var builder = MauiApp.CreateBuilder();
        builder
            .ConfigureSyncfusionCore()  // IMPORTANT: Add this line
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

**⚠️ Important**: The `ConfigureSyncfusionCore()` call must come **before** `UseMauiApp<App>()`. This is critical for proper initialization.

## Creating Data Models

TreeMap requires a data model class to represent your hierarchical data. Here's how to create one:

### Step 1: Create a Data Model Class

Create a new class file named `PopulationData.cs`:

```csharp
namespace TreeMapDemo.Models
{
    public class PopulationData
    {
        public string Country { get; set; }
        public string Continent { get; set; }
        public int Population { get; set; }
    }
}
```

**Key Points:**
- Properties should have public getters and setters
- Use appropriate data types (string for text, int/double for numeric values)
- Property names will be used in binding paths

### Example for File System Data

```csharp
public class FileSystemItem
{
    public string Name { get; set; }
    public string Category { get; set; }
    public long SizeInBytes { get; set; }
    public string FileType { get; set; }
}
```

### Example for Sales Data

```csharp
public class SalesData
{
    public string Product { get; set; }
    public string Region { get; set; }
    public string Category { get; set; }
    public decimal Revenue { get; set; }
    public int Units { get; set; }
}
```

### AOT Compilation Note

**⚠️ Important for iOS/macOS AOT publishing**: Add the `[Preserve]` attribute to prevent your model from being stripped during ahead-of-time compilation:

```csharp
using System.Runtime.Serialization;

[Preserve(AllMembers = true)]
public class PopulationData
{
    public string Country { get; set; }
    public string Continent { get; set; }
    public int Population { get; set; }
}
```

## Setting Up ViewModel

Create a ViewModel to provide data to the TreeMap.

### Step 1: Create ViewModel Class

Create a new class file named `PopulationViewModel.cs`:

```csharp
using System.Collections.ObjectModel;

namespace TreeMapDemo.ViewModels
{
    public class PopulationViewModel
    {
        public ObservableCollection<PopulationData> PopulationDetails { get; set; }
        
        public PopulationViewModel()
        {
            PopulationDetails = new ObservableCollection<PopulationData>
            {
                new PopulationData { Continent = "North America", Country = "United States", Population = 339996564 },
                new PopulationData { Continent = "South America", Country = "Brazil", Population = 216422446 },
                new PopulationData { Continent = "North America", Country = "Mexico", Population = 128455567 },
                new PopulationData { Continent = "South America", Country = "Colombia", Population = 52085168 },
                new PopulationData { Continent = "South America", Country = "Argentina", Population = 45773884 },
                new PopulationData { Continent = "North America", Country = "Canada", Population = 38781292 },
                new PopulationData { Continent = "South America", Country = "Peru", Population = 34352719 },
                new PopulationData { Continent = "South America", Country = "Venezuela", Population = 28838499 },
                new PopulationData { Continent = "South America", Country = "Chile", Population = 19629590 },
                new PopulationData { Continent = "South America", Country = "Ecuador", Population = 18190484 },
                new PopulationData { Continent = "North America", Country = "Guatemala", Population = 18092026 },
                new PopulationData { Continent = "South America", Country = "Bolivia", Population = 12388571 },
                new PopulationData { Continent = "North America", Country = "Honduras", Population = 10593798 },
                new PopulationData { Continent = "North America", Country = "Nicaragua", Population = 7046311 },
                new PopulationData { Continent = "South America", Country = "Paraguay", Population = 6861524 },
                new PopulationData { Continent = "North America", Country = "El Salvador", Population = 6364943 },
                new PopulationData { Continent = "North America", Country = "Costa Rica", Population = 5212173 },
                new PopulationData { Continent = "South America", Country = "Uruguay", Population = 3423109 }
            };
        }
    }
}
```

**Key Points:**
- Use `ObservableCollection<T>` for automatic update notifications
- Initialize data in the constructor
- Property should be public with at least a getter

### Using INotifyPropertyChanged (Optional for Dynamic Data)

If your data changes at runtime and you need the TreeMap to update automatically:

```csharp
using System.ComponentModel;
using System.Collections.ObjectModel;

public class PopulationViewModel : INotifyPropertyChanged
{
    private ObservableCollection<PopulationData> _populationDetails;
    
    public ObservableCollection<PopulationData> PopulationDetails
    {
        get => _populationDetails;
        set
        {
            _populationDetails = value;
            OnPropertyChanged(nameof(PopulationDetails));
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected virtual void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

## Adding TreeMap to Your Page

Now add the TreeMap control to your XAML page.

### Step 1: Add XML Namespace

Open your `MainPage.xaml` and add the TreeMap namespace declaration:

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:treemap="clr-namespace:Syncfusion.Maui.TreeMap;assembly=Syncfusion.Maui.TreeMap"
             xmlns:local="clr-namespace:TreeMapDemo.ViewModels"
             x:Class="TreeMapDemo.MainPage">
```

### Step 2: Initialize the TreeMap Control

Add the TreeMap within your page layout:

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:treemap="clr-namespace:Syncfusion.Maui.TreeMap;assembly=Syncfusion.Maui.TreeMap"
             xmlns:local="clr-namespace:TreeMapDemo.ViewModels"
             x:Class="TreeMapDemo.MainPage">
    
    <treemap:SfTreeMap x:Name="treeMap"
                       DataSource="{Binding PopulationDetails}"
                       PrimaryValuePath="Population">
        <treemap:SfTreeMap.BindingContext>
            <local:PopulationViewModel />
        </treemap:SfTreeMap.BindingContext>
    </treemap:SfTreeMap>
</ContentPage>
```

### Alternative: Code-Behind Approach in C#

If you prefer C#, modify your `MainPage.xaml.cs`:

```csharp
using Syncfusion.Maui.TreeMap;
using TreeMapDemo.ViewModels;

namespace TreeMapDemo;

public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        
        // Create ViewModel
        var viewModel = new PopulationViewModel();
        
        // Create TreeMap
        SfTreeMap treeMap = new SfTreeMap
        {
            DataSource = viewModel.PopulationDetails,
            PrimaryValuePath = "Population"
        };
        
        // Set as page content
        this.Content = treeMap;
    }
}
```

## Configuring Basic Properties

### DataSource Property

The `DataSource` property binds your data collection to the TreeMap:

```xml
<treemap:SfTreeMap DataSource="{Binding PopulationDetails}" ... />
```

Or in C#:
```csharp
treeMap.DataSource = viewModel.PopulationDetails;
```

### PrimaryValuePath Property

The `PrimaryValuePath` specifies which property determines the size of each rectangle:

```xml
<treemap:SfTreeMap PrimaryValuePath="Population" ... />
```

**Important**: This property name must exactly match a numeric property in your data model.

### RangeColorValuePath Property (Optional)

When using range-based coloring with legends, specify which property determines the color:

```xml
<treemap:SfTreeMap RangeColorValuePath="Population" ... />
```

This is only needed when using `TreeMapRangeBrushSettings` with legends.

## Adding Labels

To display text labels on TreeMap items, use the `LeafItemSettings` property:

### XAML Approach

```xml
<treemap:SfTreeMap DataSource="{Binding PopulationDetails}"
                   PrimaryValuePath="Population">
    <treemap:SfTreeMap.LeafItemSettings>
        <treemap:TreeMapLeafItemSettings LabelPath="Country" />
    </treemap:SfTreeMap.LeafItemSettings>
</treemap:SfTreeMap>
```

### C# Approach

```csharp
treeMap.LeafItemSettings = new TreeMapLeafItemSettings 
{ 
    LabelPath = "Country" 
};
```

The `LabelPath` property specifies which data property to display as text on each rectangle.

## Applying Basic Styling

### Using UniformBrushSettings (Single Color)

Apply a uniform color to all leaf items:

```xml
<treemap:SfTreeMap DataSource="{Binding PopulationDetails}"
                   PrimaryValuePath="Population">
    <treemap:SfTreeMap.LeafItemSettings>
        <treemap:TreeMapLeafItemSettings LabelPath="Country" />
    </treemap:SfTreeMap.LeafItemSettings>
    
    <treemap:SfTreeMap.LeafItemBrushSettings>
        <treemap:TreeMapUniformBrushSettings Brush="Orange" />
    </treemap:SfTreeMap.LeafItemBrushSettings>
</treemap:SfTreeMap>
```

In C#:
```csharp
treeMap.LeafItemBrushSettings = new TreeMapUniformBrushSettings 
{ 
    Brush = Brush.Orange 
};
```

### Using Color from Hex Code

```xml
<treemap:TreeMapUniformBrushSettings Brush="#FF6B35" />
```

Or in C#:
```csharp
treeMap.LeafItemBrushSettings = new TreeMapUniformBrushSettings 
{ 
    Brush = new SolidColorBrush(Color.FromArgb("#FF6B35")) 
};
```

## Complete Working Example

Here's a complete, copy-paste-ready example:

### PopulationData.cs
```csharp
namespace TreeMapDemo.Models
{
    public class PopulationData
    {
        public string Country { get; set; }
        public string Continent { get; set; }
        public int Population { get; set; }
    }
}
```

### PopulationViewModel.cs
```csharp
using System.Collections.ObjectModel;
using TreeMapDemo.Models;

namespace TreeMapDemo.ViewModels
{
    public class PopulationViewModel
    {
        public ObservableCollection<PopulationData> PopulationDetails { get; set; }
        
        public PopulationViewModel()
        {
            PopulationDetails = new ObservableCollection<PopulationData>
            {
                new PopulationData { Continent = "North America", Country = "USA", Population = 339996564 },
                new PopulationData { Continent = "South America", Country = "Brazil", Population = 216422446 },
                new PopulationData { Continent = "North America", Country = "Mexico", Population = 128455567 },
                new PopulationData { Continent = "South America", Country = "Colombia", Population = 52085168 },
                new PopulationData { Continent = "North America", Country = "Canada", Population = 38781292 }
            };
        }
    }
}
```

### MainPage.xaml
```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:treemap="clr-namespace:Syncfusion.Maui.TreeMap;assembly=Syncfusion.Maui.TreeMap"
             xmlns:local="clr-namespace:TreeMapDemo.ViewModels"
             x:Class="TreeMapDemo.MainPage">
    
    <treemap:SfTreeMap x:Name="treeMap"
                       DataSource="{Binding PopulationDetails}"
                       PrimaryValuePath="Population"
                       ShowToolTip="True">
        <treemap:SfTreeMap.BindingContext>
            <local:PopulationViewModel />
        </treemap:SfTreeMap.BindingContext>
        
        <treemap:SfTreeMap.LeafItemSettings>
            <treemap:TreeMapLeafItemSettings LabelPath="Country" />
        </treemap:SfTreeMap.LeafItemSettings>
        
        <treemap:SfTreeMap.LeafItemBrushSettings>
            <treemap:TreeMapUniformBrushSettings Brush="Orange" />
        </treemap:SfTreeMap.LeafItemBrushSettings>
    </treemap:SfTreeMap>
</ContentPage>
```

### MauiProgram.cs
```csharp
using Microsoft.Extensions.Logging;
using Syncfusion.Maui.Core.Hosting;

namespace TreeMapDemo;

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

#if DEBUG
        builder.Logging.AddDebug();
#endif

        return builder.Build();
    }
}
```

## Platform-Specific Considerations

### Windows
- No special configuration required
- Runs on Windows 10 version 1809 or later

### Android
- Minimum SDK: API 21 (Android 5.0)
- Target SDK: API 33 or later
- No additional permissions required for TreeMap

### iOS
- Minimum version: iOS 11.0
- For AOT compilation, ensure `[Preserve(AllMembers = true)]` is on data models

### macOS
- Minimum version: macOS 10.15
- For AOT compilation, ensure `[Preserve(AllMembers = true)]` is on data models

## Troubleshooting

### Issue 1: TreeMap Not Displaying

**Symptoms**: Empty screen or blank area where TreeMap should be

**Solutions**:
1. Verify `ConfigureSyncfusionCore()` is called in `MauiProgram.cs` **before** `UseMauiApp<App>()`
2. Check that the NuGet package is installed correctly
3. Ensure `DataSource` is not null or empty
4. Verify the namespace declaration in XAML is correct

### Issue 2: "PrimaryValuePath Not Found" Error

**Symptoms**: Runtime exception about missing property

**Solutions**:
1. Ensure `PrimaryValuePath` exactly matches a property name in your data model (case-sensitive)
2. Verify the property is numeric (int, double, decimal, long, etc.)
3. Check that the property has a public getter

### Issue 3: Labels Not Showing

**Symptoms**: Rectangles appear but no text labels

**Solutions**:
1. Verify `LabelPath` is set in `LeafItemSettings`
2. Check that `LabelPath` matches a string property in your data model
3. Ensure the property has values (not null or empty)
4. Check if rectangles are too small to display labels (increase data values)

### Issue 4: Binding Not Working

**Symptoms**: Empty TreeMap even though ViewModel has data

**Solutions**:
1. Verify `BindingContext` is set correctly
2. Check the binding path matches the ViewModel property name
3. Ensure ViewModel property is public
4. For XAML, verify the namespace prefix matches the xmlns declaration

### Issue 5: AOT Compilation Fails on iOS/macOS

**Symptoms**: Build succeeds but app crashes on iOS/macOS in Release mode

**Solution**: Add `[Preserve(AllMembers = true)]` to your data model classes:
```csharp
using System.Runtime.Serialization;

[Preserve(AllMembers = true)]
public class PopulationData
{
    // ... properties
}
```

### Issue 6: TreeMap Takes Up Too Much/Too Little Space

**Solutions**:
1. Wrap TreeMap in a Grid or other layout container with explicit sizing
2. Use `HeightRequest` and `WidthRequest` properties
3. Set `HorizontalOptions` and `VerticalOptions` for layout behavior

Example:
```xml
<Grid>
    <treemap:SfTreeMap HeightRequest="500"
                       HorizontalOptions="FillAndExpand"
                       VerticalOptions="FillAndExpand"
                       ... />
</Grid>
```

### Issue 7: Performance Issues with Large Datasets

**Solutions**:
1. Limit the number of items displayed (e.g., top 100)
2. Use data aggregation to reduce leaf nodes
3. Consider pagination or filtering for very large datasets
4. Avoid unnecessary property change notifications in ViewModel

## Next Steps

Now that you have a basic TreeMap working, explore these advanced features:

- **Layouts**: Change the layout algorithm (see `layouts.md`)
- **Hierarchical Levels**: Add multi-level grouping (see `hierarchical-levels.md`)
- **Brush Settings**: Apply color schemes based on data ranges (see `brush-settings.md`)
- **Legends**: Add a legend to explain colors (see `legend.md`)
- **Tooltips**: Display additional data on hover (see `tooltip.md`)
- **Selection**: Enable user interaction (see `selection-interaction.md`)
- **Customization**: Customize appearance further (see `leaf-item-customization.md`)
- **Accessibility**: Ensure your TreeMap is accessible (see `accessibility-events.md`)
