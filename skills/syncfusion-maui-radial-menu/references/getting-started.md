# Getting Started with .NET MAUI Radial Menu

This guide walks through setting up and configuring a Syncfusion .NET MAUI Radial Menu (SfRadialMenu) in your application.

## Step 1: Create a New .NET MAUI Project

### Using Visual Studio

1. Go to **File > New > Project**
2. Choose the **.NET MAUI App** template
3. Name the project and choose a location
4. Click **Next**
5. Select the .NET framework version and click **Create**

### Using .NET CLI

```bash
dotnet new maui -n MyRadialMenuApp
cd MyRadialMenuApp
```

## Step 2: Install the Syncfusion MAUI Radial Menu NuGet Package

### Using Visual Studio

1. In **Solution Explorer**, right-click the project
2. Choose **Manage NuGet Packages**
3. Search for `Syncfusion.Maui.RadialMenu`
4. Install the latest version
5. Ensure dependencies are installed correctly

### Using Package Manager Console

```powershell
Install-Package Syncfusion.Maui.RadialMenu
```

### Using .NET CLI

```bash
dotnet add package Syncfusion.Maui.RadialMenu
```

**Important:** The `Syncfusion.Maui.Core` NuGet package is automatically included as a dependency.

## Step 3: Register the Handler

The `Syncfusion.Maui.Core` package requires handler registration in your `MauiProgram.cs` file.

**Open `MauiProgram.cs` and add the following:**

```csharp
using Microsoft.Extensions.Logging;
using Syncfusion.Maui.Core.Hosting;

namespace RadialMenuGettingStarted
{
    public static class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            var builder = MauiApp.CreateBuilder();
            builder
                .UseMauiApp<App>()
                .ConfigureSyncfusionCore() // ⬅️ Register Syncfusion handler
                .ConfigureFonts(fonts =>
                {
                    fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
                });

            return builder.Build();
        }
    }
}
```

**Key point:** The `.ConfigureSyncfusionCore()` call is mandatory for all Syncfusion MAUI controls.

## Step 4: Add a Basic Radial Menu

### Import the Namespace

Add the RadialMenu namespace to your XAML or C# file.

**XAML:**
```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:radialMenu="clr-namespace:Syncfusion.Maui.RadialMenu;assembly=Syncfusion.Maui.RadialMenu"
             x:Class="RadialMenuGettingStarted.MainPage">
    
    <radialMenu:SfRadialMenu />
    
</ContentPage>
```

**C#:**
```csharp
using Syncfusion.Maui.RadialMenu;

namespace RadialMenuGettingStarted
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            
            SfRadialMenu radialMenu = new SfRadialMenu();
            this.Content = radialMenu;
        }
    }
}
```

## Step 5: Add Items to Radial Menu

Create a functional radial menu by adding items to it.

### XAML Implementation

```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:radialMenu="clr-namespace:Syncfusion.Maui.RadialMenu;assembly=Syncfusion.Maui.RadialMenu"
             x:Class="RadialMenuGettingStarted.MainPage">
    
    <radialMenu:SfRadialMenu x:Name="radialMenu" 
                             CenterButtonText="Edit"
                             CenterButtonFontSize="15">
        <radialMenu:SfRadialMenu.Items>
            <radialMenu:SfRadialMenuItem Text="Cut" FontSize="15"/>
            <radialMenu:SfRadialMenuItem Text="Copy" FontSize="15"/>
            <radialMenu:SfRadialMenuItem Text="Paste" FontSize="15"/>
            <radialMenu:SfRadialMenuItem Text="Crop" FontSize="15"/>
            <radialMenu:SfRadialMenuItem Text="Paint" FontSize="15"/>
        </radialMenu:SfRadialMenu.Items>
    </radialMenu:SfRadialMenu>
    
</ContentPage>
```

### C# Implementation

```csharp
using Syncfusion.Maui.RadialMenu;

namespace RadialMenuGettingStarted
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();

            SfRadialMenu radialMenu = new SfRadialMenu
            {
                CenterButtonText = "Edit",
                CenterButtonFontSize = 15
            };

            RadialMenuItemsCollection itemCollection = new RadialMenuItemsCollection
            {
                new SfRadialMenuItem { Text = "Cut", FontSize = 15 },
                new SfRadialMenuItem { Text = "Copy", FontSize = 15 },
                new SfRadialMenuItem { Text = "Paste", FontSize = 15 },
                new SfRadialMenuItem { Text = "Crop", FontSize = 15 },
                new SfRadialMenuItem { Text = "Paint", FontSize = 15 }
            };
            
            radialMenu.Items = itemCollection;
            this.Content = radialMenu;
        }
    }
}
```

## Understanding the Basic Structure

### RadialMenuItemsCollection vs ObservableCollection

**Always use `RadialMenuItemsCollection`** for the items list instead of `ObservableCollection`. This ensures proper rendering and interaction.

```csharp
// ✅ Correct
RadialMenuItemsCollection items = new RadialMenuItemsCollection();

// ❌ Incorrect
ObservableCollection<SfRadialMenuItem> items = new ObservableCollection<SfRadialMenuItem>();
```

For nested items within each RadialMenuItem, use `SubMenuItemsCollection`:

```csharp
SfRadialMenuItem parentItem = new SfRadialMenuItem
{
    Text = "Color",
    Items = new SubMenuItemsCollection
    {
        new SfRadialMenuItem { Text = "Red" },
        new SfRadialMenuItem { Text = "Blue" }
    }
};
```

### Key Properties for Getting Started

| Property | Description | Default |
|----------|-------------|---------|
| `CenterButtonText` | Text displayed on the center button | Empty |
| `CenterButtonFontSize` | Font size of center button text | 14 |
| `Items` | Collection of outer rim menu items | Empty |
| `Text` (MenuItem) | Text displayed on menu item | Empty |
| `FontSize` (MenuItem) | Font size of menu item text | 14 |

## CSS and Theme Imports

The RadialMenu control doesn't require explicit CSS imports like web components. All styling is handled through .NET MAUI properties.

## Running Your First Radial Menu

1. Build and run the project
2. Click/tap the center button to open the menu
3. Click/tap any item on the rim
4. The menu will close after selection

## Common Initial Setup Issues

### Handler Not Registered

**Error:** `Handler not found for view`

**Solution:** Ensure `.ConfigureSyncfusionCore()` is called in `MauiProgram.cs`.

### Namespace Not Found

**Error:** `The type or namespace name 'Syncfusion' could not be found`

**Solution:** 
1. Verify the NuGet package is installed
2. Rebuild the project
3. Check the using statement: `using Syncfusion.Maui.RadialMenu;`

### Items Not Showing

**Issue:** Center button appears but no items visible

**Causes:**
- Items collection not assigned to `radialMenu.Items`
- FontSize set to 0 or very small value
- Items added but menu not opened (set `IsOpen="true"` for testing)

**Solution:**
```csharp
// Ensure items are added and assigned
radialMenu.Items = itemCollection;

// Or for testing, keep menu open
radialMenu.IsOpen = true;
```

### Center Button Not Responding

**Issue:** Tapping center button doesn't open the menu

**Causes:**
- Handler not registered
- Content property not set correctly
- Overlapping UI elements

**Solution:**
```csharp
// Ensure RadialMenu is the content
this.Content = radialMenu;

// Or within a layout
Grid grid = new Grid();
grid.Children.Add(radialMenu);
this.Content = grid;
```

## Minimal Working Example

Here's a complete, copy-paste-ready example:

```csharp
using Microsoft.Maui.Controls;
using Syncfusion.Maui.RadialMenu;

namespace MyApp
{
    public class MainPage : ContentPage
    {
        public MainPage()
        {
            SfRadialMenu radialMenu = new SfRadialMenu
            {
                CenterButtonText = "Menu",
                CenterButtonFontSize = 16
            };

            radialMenu.Items = new RadialMenuItemsCollection
            {
                new SfRadialMenuItem { Text = "Cut", FontSize = 14 },
                new SfRadialMenuItem { Text = "Copy", FontSize = 14 },
                new SfRadialMenuItem { Text = "Paste", FontSize = 14 }
            };

            Content = radialMenu;
        }
    }
}
```

This creates a fully functional radial menu with three items that can be opened by tapping the center button.
