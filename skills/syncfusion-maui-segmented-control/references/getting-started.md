# Getting Started with .NET MAUI Segmented Control

This guide covers the initial setup and basic implementation of the Syncfusion .NET MAUI Segmented Control (SfSegmentedControl).

## Installation

### Step 1: Install NuGet Package

Install the **Syncfusion.Maui.Buttons** package which contains the Segmented Control:

**Using Visual Studio:**
1. Right-click your project → **Manage NuGet Packages**
2. Search for `Syncfusion.Maui.Buttons`
3. Install the latest stable version

**Using .NET CLI:**
```bash
dotnet add package Syncfusion.Maui.Buttons
```

**Using Package Manager Console:**
```powershell
Install-Package Syncfusion.Maui.Buttons
```

The Syncfusion.Maui.Buttons package includes the SfSegmentedControl along with other button controls.

### Step 2: Register Syncfusion Handler

Register the Syncfusion Core handler in your `MauiProgram.cs` file. This is required for all Syncfusion .NET MAUI controls:

```csharp
using Syncfusion.Maui.Core.Hosting;

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
```

**Important:** Place `.ConfigureSyncfusionCore()` immediately after `.UseMauiApp<App>()` to ensure proper initialization.

## Basic Implementation

### XAML Approach

#### Step 1: Add Namespace

Add the Syncfusion.Maui.Buttons namespace to your ContentPage:

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:buttons="clr-namespace:Syncfusion.Maui.Buttons;assembly=Syncfusion.Maui.Buttons"
             x:Class="MyApp.MainPage">
    
</ContentPage>
```

#### Step 2: Add the Control

```xml
<ContentPage xmlns:buttons="clr-namespace:Syncfusion.Maui.Buttons;assembly=Syncfusion.Maui.Buttons">
    <StackLayout Padding="20" VerticalOptions="Center">
        <buttons:SfSegmentedControl x:Name="segmentedControl">
            <buttons:SfSegmentedControl.ItemsSource>
                <x:Array Type="{x:Type x:String}">
                    <x:String>Day</x:String>
                    <x:String>Week</x:String>
                    <x:String>Month</x:String>
                    <x:String>Year</x:String>
                </x:Array>
            </buttons:SfSegmentedControl.ItemsSource>
        </buttons:SfSegmentedControl>
    </StackLayout>
</ContentPage>
```

### C# Code-Behind Approach

```csharp
using Syncfusion.Maui.Buttons;

namespace MyApp;

public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        
        // Create segmented control
        var segmentedControl = new SfSegmentedControl
        {
            ItemsSource = new List<string> { "Day", "Week", "Month", "Year" }
        };
        
        // Add to layout
        var stackLayout = new StackLayout
        {
            Padding = 20,
            VerticalOptions = LayoutOptions.Center,
            Children = { segmentedControl }
        };
        
        Content = stackLayout;
    }
}
```

## IDE-Specific Setup

### Visual Studio 2022

1. **Create New Project:**
   - File → New → Project
   - Select **.NET MAUI App** template
   - Name your project and click **Create**

2. **Install NuGet Package:**
   - Solution Explorer → Right-click project → Manage NuGet Packages
   - Browse tab → Search "Syncfusion.Maui.Buttons"
   - Install latest version

3. **Register Handler:**
   - Open `MauiProgram.cs`
   - Add `using Syncfusion.Maui.Core.Hosting;`
   - Add `.ConfigureSyncfusionCore()` after `.UseMauiApp<App>()`

4. **Add Control:**
   - Open `MainPage.xaml`
   - Add namespace and control as shown above

### Visual Studio Code

1. **Create New Project:**
   ```bash
   dotnet new maui -n MyMauiApp
   cd MyMauiApp
   ```

2. **Install NuGet Package:**
   ```bash
   dotnet add package Syncfusion.Maui.Buttons
   dotnet restore
   ```

3. **Register Handler:**
   - Open `MauiProgram.cs`
   - Add handler registration as shown above

4. **Run Project:**
   ```bash
   dotnet build
   dotnet run
   ```

### JetBrains Rider

1. **Create New Project:**
   - File → New Solution
   - Select **.NET MAUI App** template
   - Configure project settings and click **Create**

2. **Install NuGet Package:**
   - Solution Explorer → Right-click project → Manage NuGet Packages
   - Search and install `Syncfusion.Maui.Buttons`
   - Alternatively, run in terminal: `dotnet add package Syncfusion.Maui.Buttons`

3. **Register Handler:**
   - Modify `MauiProgram.cs` as shown above

4. **Run Project:**
   - Select target platform (Android, iOS, Windows, MacCatalyst)
   - Click Run button or press Shift+F10

## Minimal Working Example

Here's a complete minimal example that demonstrates a working segmented control:

**MainPage.xaml:**
```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:buttons="clr-namespace:Syncfusion.Maui.Buttons;assembly=Syncfusion.Maui.Buttons"
             x:Class="MyApp.MainPage">
    
    <VerticalStackLayout Spacing="20" Padding="30" VerticalOptions="Center">
        
        <Label Text="Select a time period:"
               FontSize="18"
               HorizontalOptions="Center"/>
        
        <buttons:SfSegmentedControl x:Name="segmentedControl"
                                    SelectionChanged="OnSelectionChanged">
            <buttons:SfSegmentedControl.ItemsSource>
                <x:Array Type="{x:Type x:String}">
                    <x:String>Day</x:String>
                    <x:String>Week</x:String>
                    <x:String>Month</x:String>
                    <x:String>Year</x:String>
                </x:Array>
            </buttons:SfSegmentedControl.ItemsSource>
        </buttons:SfSegmentedControl>
        
        <Label x:Name="resultLabel"
               Text="Selected: Day"
               FontSize="16"
               HorizontalOptions="Center"/>
        
    </VerticalStackLayout>
</ContentPage>
```

**MainPage.xaml.cs:**
```csharp
using Syncfusion.Maui.Buttons;

namespace MyApp;

public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
    }
    
    private void OnSelectionChanged(object sender, SelectionChangedEventArgs e)
    {
        if (e.NewItem != null)
        {
            resultLabel.Text = $"Selected: {e.NewItem}";
        }
    }
}
```

## Common Setup Issues

### Issue: Control Not Rendering

**Cause:** Syncfusion handler not registered  
**Solution:** Ensure `.ConfigureSyncfusionCore()` is called in `MauiProgram.cs`

### Issue: License Error Message

**Cause:** No valid Syncfusion license  
**Solution:** 
- Register for a free trial at syncfusion.com
- Or add license key in `MauiProgram.cs` before building:
  ```csharp
  Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");
  ```

### Issue: Namespace Not Found

**Cause:** NuGet package not installed or not restored  
**Solution:** 
```bash
dotnet restore
# OR
dotnet clean
dotnet build
```

### Issue: ItemsSource Not Populating

**Cause:** ItemsSource expects IList type  
**Solution:** Ensure ItemsSource is a proper collection:
```csharp
// ✓ Correct
ItemsSource = new List<string> { "Item1", "Item2" }

// ✗ Incorrect
ItemsSource = "Item1,Item2"
```

## Next Steps

Once you have the basic control working:

1. **Populate Items:** Learn different ways to add segments ([populating-items.md](populating-items.md))
2. **Configure Selection:** Customize selection behavior and indicators ([selection.md](selection.md))
3. **Style Appearance:** Apply custom colors, borders, and styles ([customization.md](customization.md))
4. **Handle Events:** Respond to user interactions ([events.md](events.md))

## Quick Reference

**Required Package:** `Syncfusion.Maui.Buttons`  
**Namespace:** `Syncfusion.Maui.Buttons`  
**Control Class:** `SfSegmentedControl`  
**Required Setup:** `.ConfigureSyncfusionCore()` in MauiProgram.cs  
**Minimum ItemsSource:** At least 2 items for meaningful segmented control
