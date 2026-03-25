# Getting Started with .NET MAUI Slider

This guide covers the installation, and basic implementation of the Syncfusion .NET MAUI Slider (SfSlider) control.

## Installation Steps

### Step 1: Create a New .NET MAUI Project

**Visual Studio 2026:**
1. Go to **File > New > Project**
2. Choose **.NET MAUI App** template
3. Name the project and choose a location
4. Click **Next**, select .NET framework version, then **Create**

**Visual Studio Code:**
1. Press **Ctrl+Shift+P** (Cmd+Shift+P on Mac)
2. Type **.NET:New Project** and press Enter
3. Choose **.NET MAUI App** template
4. Select project location and enter project name
5. Choose **Create project**

**JetBrains Rider:**
1. Go to **File > New Solution**
2. Select **.NET (C#)** and choose **.NET MAUI App** template
3. Enter Project Name, Solution Name, and Location
4. Select .NET framework version and click **Create**

### Step 2: Install Syncfusion.Maui.Sliders NuGet Package

**Visual Studio:**
1. Right-click the project in **Solution Explorer**
2. Choose **Manage NuGet Packages**
3. Search for `Syncfusion.Maui.Sliders`
4. Install the latest version
5. Ensure dependencies are installed correctly

**Visual Studio Code / Terminal:**
```bash
dotnet add package Syncfusion.Maui.Sliders
dotnet restore
```

**JetBrains Rider:**
1. Right-click the project in **Solution Explorer**
2. Choose **Manage NuGet Packages**
3. Search for `Syncfusion.Maui.Sliders`
4. Install the latest version
5. If needed, run `dotnet restore` in Terminal

### Step 3: Register the Syncfusion Handler

The `Syncfusion.Maui.Core` package is a dependency for all Syncfusion .NET MAUI controls. Register the handler in **MauiProgram.cs**:

```csharp
using Microsoft.Maui;
using Microsoft.Maui.Hosting;
using Microsoft.Maui.Controls.Compatibility;
using Microsoft.Maui.Controls.Hosting;
using Microsoft.Maui.Controls.Xaml;
using Syncfusion.Maui.Core.Hosting;

namespace SliderGettingStarted
{
    public static class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            var builder = MauiApp.CreateBuilder();
            builder
                .UseMauiApp<App>()
                .ConfigureSyncfusionCore()  // ← Register Syncfusion handler
                .ConfigureFonts(fonts =>
                {
                    fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
                });

            return builder.Build();
        }
    }
}
```

**Important**: Without `ConfigureSyncfusionCore()`, the Slider control will not render correctly.

## Basic Slider Implementation

### Step 4: Add Namespace and Create Slider

**XAML Implementation:**

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sliders="clr-namespace:Syncfusion.Maui.Sliders;assembly=Syncfusion.Maui.Sliders"
             x:Class="SliderGettingStarted.MainPage">
    <Grid>
        <sliders:SfSlider />
    </Grid>
</ContentPage>
```

**C# Implementation:**

```csharp
using Syncfusion.Maui.Sliders;

namespace SliderGettingStarted
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            
            SfSlider slider = new SfSlider();
            Content = slider;
        }
    }
}
```

This creates a basic slider with default settings:
- Minimum: 0
- Maximum: 1
- Value: 0.5 (middle)
- Horizontal orientation

## Configuring Basic Properties

### Setting Minimum, Maximum, and Value

```xml
<sliders:SfSlider Minimum="0"
                  Maximum="100"
                  Value="50" />
```

```csharp
SfSlider slider = new SfSlider
{
    Minimum = 0,
    Maximum = 100,
    Value = 50
};
```

### Enabling Labels

Display labels at regular intervals:

```xml
<sliders:SfSlider Minimum="0"
                  Maximum="10"
                  Value="5"
                  ShowLabels="True"
                  Interval="2" />
```

```csharp
SfSlider slider = new SfSlider
{
    Minimum = 0,
    Maximum = 10,
    Value = 5,
    Interval = 2,
    ShowLabels = true
};
```

### Enabling Ticks

Display major and minor ticks:

```xml
<sliders:SfSlider Minimum="0"
                  Maximum="10"
                  Value="5"
                  Interval="2"
                  ShowTicks="True"
                  ShowLabels="True"
                  MinorTicksPerInterval="1" />
```

```csharp
SfSlider slider = new SfSlider
{
    Minimum = 0,
    Maximum = 10,
    Value = 5,
    Interval = 2,
    ShowLabels = true,
    ShowTicks = true,
    MinorTicksPerInterval = 1
};
```

## Orientation

Change the slider orientation from horizontal (default) to vertical:

```xml
<sliders:SfSlider Orientation="Vertical"
                  Minimum="0"
                  Maximum="10"
                  Value="5"
                  Interval="2"
                  ShowLabels="True"
                  ShowTicks="True"
                  MinorTicksPerInterval="1" />
```

```csharp
SfSlider slider = new SfSlider
{
    Orientation = SliderOrientation.Vertical,
    Minimum = 0,
    Maximum = 10,
    Value = 5,
    Interval = 2,
    ShowLabels = true,
    ShowTicks = true,
    MinorTicksPerInterval = 1
};
```

**Use Cases:**
- **Horizontal**: Volume, brightness, progress bars, price filters
- **Vertical**: Vertical volume controls, height adjustments, elevator controls

## Inverse the Slider

Reverse the slider so the minimum value is on the right (horizontal) or top (vertical):

```xml
<sliders:SfSlider Minimum="0"
                  Maximum="10"
                  Value="5"
                  IsInversed="True"
                  Interval="2"
                  ShowTicks="True"
                  ShowLabels="True"
                  MinorTicksPerInterval="1" />
```

```csharp
SfSlider slider = new SfSlider
{
    Minimum = 0,
    Maximum = 10,
    Value = 5,
    IsInversed = true,
    Interval = 2,
    ShowLabels = true,
    ShowTicks = true,
    MinorTicksPerInterval = 1
};
```

**Use Cases:**
- Right-to-left (RTL) language support
- Countdown timers
- Inverted scales (e.g., depth measurements)

## Formatting Labels

Add prefix or suffix to labels using NumberFormat:

```xml
<sliders:SfSlider Minimum="20"
                  Maximum="100"
                  Value="60"
                  Interval="20"
                  NumberFormat="$##"
                  ShowLabels="True"
                  ShowTicks="True"
                  MinorTicksPerInterval="1" />
```

```csharp
SfSlider slider = new SfSlider
{
    Minimum = 20,
    Maximum = 100,
    Value = 60,
    Interval = 20,
    NumberFormat = "$##",
    ShowLabels = true,
    ShowTicks = true,
    MinorTicksPerInterval = 1
};
```

**Common Number Formats:**
- Currency: `"$#"` or `"$0"`
- Percentage: `"0%"` or `"#'%'"`
- Decimals: `"0.00"` or `"0.##"`
- Custom units: `"0'kg'"`, `"0'°C'"`

## Complete Example

Here's a fully configured slider for a temperature control:

**XAML:**
```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sliders="clr-namespace:Syncfusion.Maui.Sliders;assembly=Syncfusion.Maui.Sliders"
             x:Class="MyApp.MainPage">
    <Grid Padding="20">
        <VerticalStackLayout Spacing="10">
            <Label Text="Temperature Control" 
                   FontSize="18" 
                   FontAttributes="Bold" />
            
            <sliders:SfSlider Minimum="10"
                              Maximum="30"
                              Value="22"
                              Interval="5"
                              NumberFormat="0'°C'"
                              ShowLabels="True"
                              ShowTicks="True"
                              ShowDividers="True"
                              MinorTicksPerInterval="1">
                <sliders:SfSlider.Tooltip>
                    <sliders:SliderTooltip NumberFormat="0.0'°C'" />
                </sliders:SfSlider.Tooltip>
            </sliders:SfSlider>
        </VerticalStackLayout>
    </Grid>
</ContentPage>
```

**C#:**
```csharp
using Syncfusion.Maui.Sliders;

public class MainPage : ContentPage
{
    public MainPage()
    {
        var stackLayout = new VerticalStackLayout
        {
            Spacing = 10
        };
        
        var label = new Label
        {
            Text = "Temperature Control",
            FontSize = 18,
            FontAttributes = FontAttributes.Bold
        };
        
        var slider = new SfSlider
        {
            Minimum = 10,
            Maximum = 30,
            Value = 22,
            Interval = 5,
            NumberFormat = "0'°C'",
            ShowLabels = true,
            ShowTicks = true,
            ShowDividers = true,
            MinorTicksPerInterval = 1,
            Tooltip = new SliderTooltip
            {
                NumberFormat = "0.0'°C'"
            }
        };
        
        stackLayout.Children.Add(label);
        stackLayout.Children.Add(slider);
        
        Content = new Grid
        {
            Padding = new Thickness(20),
            Children = { stackLayout }
        };
    }
}
```

## Troubleshooting

### Issue: Slider Not Visible

**Cause**: Handler not registered  
**Solution**: Ensure `ConfigureSyncfusionCore()` is called in `MauiProgram.cs`

### Issue: Labels Not Showing

**Cause**: ShowLabels is false or Interval not set  
**Solution**: 
```xml
<sliders:SfSlider ShowLabels="True" Interval="10" />
```
Or use `Interval="0"` for automatic interval calculation.

### Issue: Ticks Not Appearing

**Cause**: ShowTicks is false  
**Solution**:
```xml
<sliders:SfSlider ShowTicks="True" Interval="10" />
```

### Issue: Build Errors After Installation

**Cause**: NuGet packages not restored  
**Solution**: Run `dotnet restore` or rebuild the solution

## Next Steps

Now that you have a basic slider working:

1. **Customize the track**: See [track-and-values.md](track-and-values.md) for track styling
2. **Format labels**: See [labels-and-formatting.md](labels-and-formatting.md) for advanced formatting
3. **Add tooltips**: See [tooltip.md](tooltip.md) for tooltip configuration
4. **Handle events**: See [events-and-commands.md](events-and-commands.md) for value change handling
5. **Style the thumb**: See [thumb-selection-overlay.md](thumb-selection-overlay.md) for thumb customization

## Summary

Key points for getting started:

- Install `Syncfusion.Maui.Sliders` NuGet package
- Register handler with `ConfigureSyncfusionCore()` in MauiProgram.cs
- Add namespace: `xmlns:sliders="clr-namespace:Syncfusion.Maui.Sliders;assembly=Syncfusion.Maui.Sliders"`
- Basic properties: `Minimum`, `Maximum`, `Value`
- Enable features: `ShowLabels`, `ShowTicks`, `ShowDividers`
- Configure interval: `Interval` property (0 for auto)
- Format labels: `NumberFormat` property
- Change orientation: `Orientation` property (Horizontal/Vertical)
- Reverse layout: `IsInversed` property
