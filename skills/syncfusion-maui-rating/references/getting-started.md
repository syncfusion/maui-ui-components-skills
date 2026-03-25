# Getting Started with .NET MAUI Rating

This guide walks through the complete setup and basic implementation of the Syncfusion .NET MAUI Rating (`SfRating`) control.

## Installation Steps

### Step 1: Create a New MAUI Project

**Visual Studio:**
1. Go to **File > New > Project**
2. Choose **.NET MAUI App** template
3. Name the project and choose a location
4. Click **Next**, select the .NET framework version, and click **Create**

**Visual Studio Code:**
1. Open Command Palette (**Ctrl+Shift+P**)
2. Type **.NET:New Project** and press Enter
3. Choose **.NET MAUI App** template
4. Select project location and enter project name
5. Choose **Create project**

**Command Line:**
```bash
dotnet new maui -n MyRatingApp
cd MyRatingApp
```

### Step 2: Install Syncfusion MAUI Inputs NuGet Package

**Visual Studio:**
1. In **Solution Explorer**, right-click the project
2. Choose **Manage NuGet Packages**
3. Search for `Syncfusion.Maui.Inputs`
4. Install the latest version
5. Ensure dependencies are restored

**Visual Studio Code / Command Line:**
```bash
dotnet add package Syncfusion.Maui.Inputs
dotnet restore
```

**Package Manager Console:**
```powershell
Install-Package Syncfusion.Maui.Inputs
```

### Step 3: Register the Syncfusion Handler

The `Syncfusion.Maui.Core` NuGet package is a required dependency for all Syncfusion .NET MAUI controls. Register the handler in `MauiProgram.cs`:

```csharp
using Microsoft.Maui;
using Microsoft.Maui.Hosting;
using Microsoft.Maui.Controls.Hosting;
using Syncfusion.Maui.Core.Hosting;

namespace MyRatingApp
{
    public static class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            var builder = MauiApp.CreateBuilder();
            builder
                .UseMauiApp<App>()
                .ConfigureSyncfusionCore()  // Register Syncfusion handler
                .ConfigureFonts(fonts =>
                {
                    fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
                    fonts.AddFont("OpenSans-Semibold.ttf", "OpenSansSemibold");
                });

            return builder.Build();
        }
    }
}
```

> **Important:** The `ConfigureSyncfusionCore()` method must be called to register all Syncfusion MAUI control handlers.

## Basic Implementation

### Step 4: Add Namespace

**In XAML:**
```xml
xmlns:rating="clr-namespace:Syncfusion.Maui.Inputs;assembly=Syncfusion.Maui.Inputs"
```

**In C#:**
```csharp
using Syncfusion.Maui.Inputs;
```

### Step 5: Add SfRating Control

**XAML Example:**
```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:rating="clr-namespace:Syncfusion.Maui.Inputs;assembly=Syncfusion.Maui.Inputs"
             x:Class="MyRatingApp.MainPage">
    
    <VerticalStackLayout Padding="20" Spacing="20">
        <Label Text="Rate this item:" FontSize="18" />
        <rating:SfRating x:Name="rating" />
    </VerticalStackLayout>
    
</ContentPage>
```

**C# Example:**
```csharp
using Syncfusion.Maui.Inputs;

public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        
        SfRating rating = new SfRating();
        this.Content = rating;
    }
}
```

## Configuration Properties

### Set Number of Rating Items

The `ItemCount` property determines how many rating items are displayed.

> **Default:** 5 items

**XAML:**
```xml
<rating:SfRating ItemCount="5" />
```

**C#:**
```csharp
SfRating rating = new SfRating();
rating.ItemCount = 5;
```

**Common ItemCount Values:**
- **3 items**: Simple preference (low/medium/high)
- **5 items**: Standard star rating (most common)
- **10 items**: Detailed scale rating

### Set Rating Value

The `Value` property sets the currently selected rating value.

> **Default:** 0 (no rating)

**XAML:**
```xml
<rating:SfRating Value="3" />
```

**C#:**
```csharp
SfRating rating = new SfRating();
rating.Value = 3;
```

**Example with ItemCount and Value:**
```xml
<!-- Display 5 stars with 3 selected -->
<rating:SfRating Value="3" ItemCount="5" />
```

### Set Precision Mode

The `Precision` property controls rating accuracy levels.

> **Default:** `Standard` (full item selection)

**XAML:**
```xml
<rating:SfRating Precision="Standard" />
```

**C#:**
```csharp
SfRating rating = new SfRating();
rating.Precision = Precision.Standard;
```

**Available Precision Modes:**
- `Standard`: Full item rating (1, 2, 3, etc.)
- `Half`: Half-step rating (1.5, 2.5, 3.5, etc.)
- `Exact`: Precise decimal rating (1.25, 2.73, 3.89, etc.)

## Complete Example

Here's a complete working example combining all basic features:

**XAML:**
```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:rating="clr-namespace:Syncfusion.Maui.Inputs;assembly=Syncfusion.Maui.Inputs"
             x:Class="MyRatingApp.MainPage">
    
    <VerticalStackLayout Padding="30" Spacing="25">
        
        <!-- Standard 5-star rating -->
        <Label Text="Product Rating:" FontSize="18" FontAttributes="Bold" />
        <rating:SfRating x:Name="productRating" 
                         Value="4" 
                         ItemCount="5" 
                         Precision="Standard" />
        
        <!-- Current value display -->
        <Label Text="{Binding Source={x:Reference productRating}, Path=Value, StringFormat='Rating: {0} stars'}" 
               FontSize="16" />
        
    </VerticalStackLayout>
    
</ContentPage>
```

**C#:**
```csharp
using Syncfusion.Maui.Inputs;

public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        
        // Create rating control programmatically
        SfRating rating = new SfRating
        {
            Value = 4,
            ItemCount = 5,
            Precision = Precision.Standard
        };
        
        // Add to layout
        var layout = new VerticalStackLayout
        {
            Padding = 30,
            Spacing = 25
        };
        
        layout.Children.Add(new Label 
        { 
            Text = "Product Rating:", 
            FontSize = 18, 
            FontAttributes = FontAttributes.Bold 
        });
        
        layout.Children.Add(rating);
        
        layout.Children.Add(new Label 
        { 
            Text = $"Rating: {rating.Value} stars", 
            FontSize = 16 
        });
        
        this.Content = layout;
    }
}
```

## Verification

After implementation, verify that:
1. ✓ Rating control appears with default 5 stars
2. ✓ Clicking/tapping stars changes the selection
3. ✓ Value property reflects the selected rating
4. ✓ Control is interactive (can change rating by clicking)

## Next Steps

- **Precision Modes**: Learn about Standard, Half, and Exact precision → [precision-modes.md](precision-modes.md)
- **Rating Shapes**: Explore different shapes (Heart, Diamond, Custom) → [rating-shapes.md](rating-shapes.md)
- **Appearance Styling**: Customize colors, sizes, and strokes → [appearance-styling.md](appearance-styling.md)
- **Interactive Features**: Handle events and read-only mode → [interactive-features.md](interactive-features.md)

## Common Issues

### Control Not Appearing
- **Solution**: Verify `ConfigureSyncfusionCore()` is called in `MauiProgram.cs`
- **Solution**: Ensure NuGet package is installed and restored
- **Solution**: Check namespace declaration in XAML

### Build Errors
- **Solution**: Clean and rebuild the solution
- **Solution**: Verify .NET MAUI workload is installed: `dotnet workload install maui`
- **Solution**: Check NuGet package compatibility with .NET version

### Stars Not Interactive
- **Solution**: Ensure `IsReadOnly` property is `false` (default)
- **Solution**: Check that the control is properly initialized

## Additional Resources

- **Complete Sample**: [GitHub - SyncfusionExamples/maui-rating-samples](https://github.com/SyncfusionExamples/maui-rating-samples)
- **NuGet Package**: [Syncfusion.Maui.Inputs](https://www.nuget.org/packages/Syncfusion.Maui.Inputs)
- **Video Tutorial**: [Getting Started with .NET MAUI Rating](https://www.youtube.com/watch?v=yEJzdjPNjBs)
