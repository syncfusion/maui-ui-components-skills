# Getting Started with .NET MAUI Toolbar

Learn how to install, configure, and create your first Syncfusion .NET MAUI Toolbar (SfToolbar) control.

## Step 1: Create a New .NET MAUI Project

### Using Visual Studio:
1. Go to **File > New > Project**
2. Choose the **.NET MAUI App** template
3. Name your project (e.g., "ToolbarDemoApp")
4. Choose a location for your project
5. Click **Next**
6. Select the .NET framework version (.NET 9 or later)
7. Click **Create**

### Using .NET CLI:
```bash
dotnet new maui -n ToolbarDemoApp
cd ToolbarDemoApp
```

## Step 2: Install the Syncfusion MAUI Toolbar NuGet Package

### Using Visual Studio:
1. In **Solution Explorer**, right-click your project
2. Choose **Manage NuGet Packages**
3. Click the **Browse** tab
4. Search for `Syncfusion.Maui.Toolbar`
5. Select the package from the results
6. Click **Install** to add the latest version
7. Accept the license agreement when prompted
8. Ensure all dependencies are installed correctly

### Using Package Manager Console:
```powershell
Install-Package Syncfusion.Maui.Toolbar
```

### Using .NET CLI:
```bash
dotnet add package Syncfusion.Maui.Toolbar
```

**Verify Installation:**
Check your `.csproj` file to confirm the package reference:
```xml
<ItemGroup>
  <PackageReference Include="Syncfusion.Maui.Toolbar" Version="*" />
</ItemGroup>
```

## Step 3: Register the Syncfusion Core Handler

The `Syncfusion.Maui.Core` NuGet package is a dependency for all Syncfusion controls in .NET MAUI. You must register the Syncfusion core handler in your `MauiProgram.cs` file.

Open `MauiProgram.cs` and add the handler registration:

```csharp
using Syncfusion.Maui.Core.Hosting;

public static class MauiProgram
{
    public static MauiApp CreateMauiApp()
    {
        var builder = MauiApp.CreateBuilder();
        builder
            .ConfigureSyncfusionCore()  // Register Syncfusion handler
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

**Important:** The `ConfigureSyncfusionCore()` method must be called before `UseMauiApp<App>()`.

## Step 4: Configure Material Icons Font (Optional but Recommended)

To use Material icons in your toolbar, add the Material icons font to your project:

### Add the Font File:
1. Download or obtain `MauiMaterialAssets.ttf`
2. Place it in the `Resources/Fonts` folder of your project

### Register the Font:
Update `MauiProgram.cs` to include the font:

```csharp
builder.ConfigureFonts(fonts =>
{
    fonts.AddFont("MauiMaterialAssets.ttf", "MauiMaterialAssets");  // Add this line
    fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
    fonts.AddFont("OpenSans-Semibold.ttf", "OpenSansSemibold");
});
```

## Step 5: Add Your First Toolbar

Now you're ready to add a toolbar to your application.

### XAML Approach:

Open `MainPage.xaml` and add the toolbar namespace and control:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:toolbar="clr-namespace:Syncfusion.Maui.Toolbar;assembly=Syncfusion.Maui.Toolbar"
             x:Class="ToolbarDemoApp.MainPage">
    <Grid>
        <toolbar:SfToolbar x:Name="toolbar" HeightRequest="56">
            <!-- Toolbar items will be added here -->
        </toolbar:SfToolbar>
    </Grid>
</ContentPage>
```

### C# Code Approach:

Open `MainPage.xaml.cs` and create the toolbar programmatically:

```csharp
using Syncfusion.Maui.Toolbar;

namespace ToolbarDemoApp
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            
            // Create toolbar
            SfToolbar toolbar = new SfToolbar();
            toolbar.HeightRequest = 56;
            
            // Set as page content
            this.Content = toolbar;
        }
    }
}
```

## Step 6: Add Toolbar Items

Add items to your toolbar with icons and tooltips.

### XAML with Icons:

```xaml
<toolbar:SfToolbar x:Name="toolbar" HeightRequest="56">
    <toolbar:SfToolbar.Items>
        <toolbar:SfToolbarItem Name="Bold" ToolTipText="Bold">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE770;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        
        <toolbar:SfToolbarItem Name="Underline" ToolTipText="Underline">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE762;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        
        <toolbar:SfToolbarItem Name="Italic" ToolTipText="Italic">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE771;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        
        <toolbar:SfToolbarItem Name="AlignLeft" ToolTipText="Align Left">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE751;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        
        <toolbar:SfToolbarItem Name="AlignRight" ToolTipText="Align Right">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE753;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
    </toolbar:SfToolbar.Items>
</toolbar:SfToolbar>
```

### C# with Icons:

```csharp
using Syncfusion.Maui.Toolbar;
using System.Collections.ObjectModel;

namespace ToolbarDemoApp
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            
            SfToolbar toolbar = new SfToolbar();
            toolbar.HeightRequest = 56;
            
            // Create toolbar items collection
            ObservableCollection<BaseToolbarItem> itemCollection = new ObservableCollection<BaseToolbarItem>
            {
                new SfToolbarItem
                {
                    Name = "Bold",
                    ToolTipText = "Bold",
                    Icon = new FontImageSource 
                    { 
                        Glyph = "\uE770", 
                        FontFamily = "MauiMaterialAssets" 
                    }
                },
                new SfToolbarItem
                {
                    Name = "Underline",
                    ToolTipText = "Underline",
                    Icon = new FontImageSource 
                    { 
                        Glyph = "\uE762", 
                        FontFamily = "MauiMaterialAssets" 
                    }
                },
                new SfToolbarItem
                {
                    Name = "Italic",
                    ToolTipText = "Italic",
                    Icon = new FontImageSource 
                    { 
                        Glyph = "\uE771", 
                        FontFamily = "MauiMaterialAssets" 
                    }
                },
                new SfToolbarItem
                {
                    Name = "AlignLeft",
                    ToolTipText = "Align Left",
                    Icon = new FontImageSource 
                    { 
                        Glyph = "\uE751", 
                        FontFamily = "MauiMaterialAssets" 
                    }
                },
                new SfToolbarItem
                {
                    Name = "AlignRight",
                    ToolTipText = "Align Right",
                    Icon = new FontImageSource 
                    { 
                        Glyph = "\uE753", 
                        FontFamily = "MauiMaterialAssets" 
                    }
                }
            };
            
            toolbar.Items = itemCollection;
            this.Content = toolbar;
        }
    }
}
```

## Complete Working Example

Here's a complete text formatting toolbar example:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:toolbar="clr-namespace:Syncfusion.Maui.Toolbar;assembly=Syncfusion.Maui.Toolbar"
             x:Class="ToolbarDemoApp.MainPage">
    <Grid RowDefinitions="Auto,*">
        <!-- Toolbar at the top -->
        <toolbar:SfToolbar x:Name="toolbar" 
                           Grid.Row="0"
                           HeightRequest="56">
            <toolbar:SfToolbar.Items>
                <toolbar:SfToolbarItem Name="Bold" ToolTipText="Bold">
                    <toolbar:SfToolbarItem.Icon>
                        <FontImageSource Glyph="&#xE770;" FontFamily="MauiMaterialAssets" />
                    </toolbar:SfToolbarItem.Icon>
                </toolbar:SfToolbarItem>
                
                <toolbar:SfToolbarItem Name="Italic" ToolTipText="Italic">
                    <toolbar:SfToolbarItem.Icon>
                        <FontImageSource Glyph="&#xE771;" FontFamily="MauiMaterialAssets" />
                    </toolbar:SfToolbarItem.Icon>
                </toolbar:SfToolbarItem>
                
                <toolbar:SfToolbarItem Name="Underline" ToolTipText="Underline">
                    <toolbar:SfToolbarItem.Icon>
                        <FontImageSource Glyph="&#xE762;" FontFamily="MauiMaterialAssets" />
                    </toolbar:SfToolbarItem.Icon>
                </toolbar:SfToolbarItem>
                
                <toolbar:SeparatorToolbarItem />
                
                <toolbar:SfToolbarItem Name="AlignLeft" ToolTipText="Align Left">
                    <toolbar:SfToolbarItem.Icon>
                        <FontImageSource Glyph="&#xE751;" FontFamily="MauiMaterialAssets" />
                    </toolbar:SfToolbarItem.Icon>
                </toolbar:SfToolbarItem>
                
                <toolbar:SfToolbarItem Name="AlignCenter" ToolTipText="Align Center">
                    <toolbar:SfToolbarItem.Icon>
                        <FontImageSource Glyph="&#xE752;" FontFamily="MauiMaterialAssets" />
                    </toolbar:SfToolbarItem.Icon>
                </toolbar:SfToolbarItem>
                
                <toolbar:SfToolbarItem Name="AlignRight" ToolTipText="Align Right">
                    <toolbar:SfToolbarItem.Icon>
                        <FontImageSource Glyph="&#xE753;" FontFamily="MauiMaterialAssets" />
                    </toolbar:SfToolbarItem.Icon>
                </toolbar:SfToolbarItem>
                
                <toolbar:SfToolbarItem Name="AlignJustify" ToolTipText="Align Justify">
                    <toolbar:SfToolbarItem.Icon>
                        <FontImageSource Glyph="&#xE74F;" FontFamily="MauiMaterialAssets" />
                    </toolbar:SfToolbarItem.Icon>
                </toolbar:SfToolbarItem>
            </toolbar:SfToolbar.Items>
        </toolbar:SfToolbar>
        
        <!-- Content area -->
        <Editor Grid.Row="1" 
                Placeholder="Type something here..."
                Margin="10" />
    </Grid>
</ContentPage>
```

## Run Your Application

1. **Set the startup project** if you have multiple projects
2. **Select a target device** (Android, iOS, Windows, macOS)
3. **Press F5** or click the **Run** button
4. Your application should launch with the toolbar displayed

## Common Icon Glyphs

Here are commonly used Material icon glyphs for toolbar items:

| Icon | Glyph Code | Description |
|------|------------|-------------|
| Bold | `&#xE770;` | Bold text formatting |
| Italic | `&#xE771;` | Italic text formatting |
| Underline | `&#xE762;` | Underline text formatting |
| Align Left | `&#xE751;` | Left alignment |
| Align Center | `&#xE752;` | Center alignment |
| Align Right | `&#xE753;` | Right alignment |
| Align Justify | `&#xE74F;` | Justify alignment |
| Undo | `&#xE744;` | Undo action |
| Redo | `&#xE745;` | Redo action |

**Usage in XAML:**
```xaml
<FontImageSource Glyph="&#xE770;" FontFamily="MauiMaterialAssets" />
```

**Usage in C#:**
```csharp
Icon = new FontImageSource { Glyph = "\uE770", FontFamily = "MauiMaterialAssets" }
```

## Troubleshooting

### Icons Not Displaying
- **Issue:** Icons appear as empty boxes or don't show at all
- **Solution:** 
  - Verify the font file is in `Resources/Fonts`
  - Check font registration in `MauiProgram.cs`
  - Ensure `FontFamily` name matches the registered name exactly
  - Verify the `.csproj` includes the font with `MauiFont` build action

### Package Installation Fails
- **Issue:** NuGet package installation error
- **Solution:**
  - Clear NuGet cache: `dotnet nuget locals all --clear`
  - Check internet connection
  - Verify NuGet package source is configured
  - Try updating Visual Studio or .NET SDK

### Toolbar Not Visible
- **Issue:** Toolbar doesn't appear in the app
- **Solution:**
  - Check `HeightRequest` is set on the toolbar
  - Verify toolbar is added to the visual tree (parent container)
  - Ensure `ConfigureSyncfusionCore()` is called in `MauiProgram.cs`
  - Check if toolbar is covered by other UI elements

### Build Errors After Adding Package
- **Issue:** Build fails with assembly reference errors
- **Solution:**
  - Clean and rebuild the solution
  - Close Visual Studio and delete `bin` and `obj` folders
  - Restore NuGet packages
  - Update all Syncfusion packages to the same version

## Next Steps

Now that you have a basic toolbar working, explore these topics:

1. **Toolbar Items** - Learn about different item types (text, icons, custom views)
2. **Layout Options** - Configure horizontal/vertical orientation and overflow handling
3. **Selection Modes** - Implement single or multiple item selection
4. **Events** - Handle toolbar item clicks and interactions
5. **Customization** - Style your toolbar with colors, fonts, and themes

Refer to the other reference files in this skill for detailed guidance on each topic.
