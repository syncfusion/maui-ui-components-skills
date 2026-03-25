# Getting Started with Rich Text Editor

## Table of Contents
- [Overview](#overview)
- [Installation](#installation)
- [Adding to XAML](#adding-to-xaml)
- [Adding to C#](#adding-to-c)
- [Enabling the Toolbar](#enabling-the-toolbar)
- [Customizing Toolbar Items](#customizing-toolbar-items)
- [Initial Configuration](#initial-configuration)
- [Troubleshooting](#troubleshooting)

## Overview

The Syncfusion .NET MAUI Rich Text Editor (SfRichTextEditor) provides a WYSIWYG editing interface for creating richly formatted text. This guide walks you through installation, basic setup, and creating your first functional rich text editor with toolbar support.

**What you'll learn:**
- Installing the NuGet package
- Adding SfRichTextEditor to XAML and C# code
- Registering required namespaces
- Enabling and customizing the toolbar
- Basic configuration options

## Installation

### Step 1: Install NuGet Package

Install the `Syncfusion.Maui.RichTextEditor` package from NuGet:

**Using Package Manager Console:**
```powershell
Install-Package Syncfusion.Maui.RichTextEditor
```

**Using .NET CLI:**
```bash
dotnet add package Syncfusion.Maui.RichTextEditor
```

**Using Visual Studio NuGet Package Manager:**
1. Right-click your project in Solution Explorer
2. Select "Manage NuGet Packages"
3. Search for "Syncfusion.Maui.RichTextEditor"
4. Click "Install" on the latest stable version
5. Accept the license agreement

### Step 2: Verify Installation

After installation, verify that the package and its dependencies are properly restored:
- Check that `Syncfusion.Maui.RichTextEditor` appears in your project's Dependencies > Packages
- Ensure all dependencies (Syncfusion.Maui.Core, etc.) are also installed
- Build the project to verify no errors

### Step 3: Register the Syncfusion Handler

**CRITICAL:** The `Syncfusion.Maui.Core` NuGet is a dependent package for all Syncfusion controls. You MUST register the handler in `MauiProgram.cs`.

**File:** `MauiProgram.cs`

```csharp
using Syncfusion.Maui.Core.Hosting;

    public static class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            var builder = MauiApp.CreateBuilder();
            
            // Register Syncfusion Core - REQUIRED!
            builder.ConfigureSyncfusionCore();
            
            builder
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
**⚠️ Important:** Call `ConfigureSyncfusionCore()` before `Build()`. Failure to register the handler will result in runtime errors.

## Adding to XAML

### Step 1: Register Namespace

Add the Rich Text Editor namespace to your XAML page:

```xml
xmlns:rte="clr-namespace:Syncfusion.Maui.RichTextEditor;assembly=Syncfusion.Maui.RichTextEditor"
```

**Complete page example:**

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:rte="clr-namespace:Syncfusion.Maui.RichTextEditor;assembly=Syncfusion.Maui.RichTextEditor"
             x:Class="MyApp.MainPage">
    <Grid>
        <rte:SfRichTextEditor />
    </Grid>
</ContentPage>
```

### Step 2: Add SfRichTextEditor Control

Place the `SfRichTextEditor` control in your layout:

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:rte="clr-namespace:Syncfusion.Maui.RichTextEditor;assembly=Syncfusion.Maui.RichTextEditor"
             x:Class="MyApp.MainPage">
    <Grid>
        <rte:SfRichTextEditor x:Name="richTextEditor" />
    </Grid>
</ContentPage>
```

### Step 3: Basic Configuration in XAML

Add common properties for a functional editor:

```xml
<rte:SfRichTextEditor x:Name="richTextEditor"
                      ShowToolbar="True"
                      Placeholder="Start typing your content..."
                      EditorBackgroundColor="White"
                      BorderColor="LightGray"
                      BorderThickness="1" />
```

## Adding to C#

### Step 1: Import Namespace

Add the using directive at the top of your C# file:

```csharp
using Syncfusion.Maui.RichTextEditor;
```

### Step 2: Create and Configure Editor

Instantiate the `SfRichTextEditor` in your code:

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        
        SfRichTextEditor richTextEditor = new SfRichTextEditor();
        this.Content = richTextEditor;
    }
}
```

### Step 3: Apply Basic Configuration

Configure properties programmatically:

```csharp
public partial class MainPage : ContentPage
{
    private SfRichTextEditor richTextEditor;
    
    public MainPage()
    {
        InitializeComponent();
        
        richTextEditor = new SfRichTextEditor
        {
            ShowToolbar = true,
            Placeholder = "Start typing your content...",
            EditorBackgroundColor = Colors.White,
            BorderColor = Colors.LightGray,
            BorderThickness = 1,
            VerticalOptions = LayoutOptions.Fill,
            HorizontalOptions = LayoutOptions.Fill
        };
        
        this.Content = richTextEditor;
    }
}
```

## Enabling the Toolbar

The toolbar provides formatting controls and is essential for user interaction. Enable it using the `ShowToolbar` property.

### XAML Approach

```xml
<rte:SfRichTextEditor ShowToolbar="True" />
```

### C# Approach

```csharp
SfRichTextEditor richTextEditor = new SfRichTextEditor();
richTextEditor.ShowToolbar = true;
```

### Default Toolbar Behavior

When `ShowToolbar="True"`, the editor displays a default set of toolbar items:
- **Character formatting:** Bold, Italic, Underline, Strikethrough
- **Script formatting:** Subscript, Superscript
- **Text styling:** FontFamily, FontSize, TextColor, HighlightColor
- **Paragraph:** ParagraphFormat, Alignment
- **Lists:** NumberList, BulletList
- **Indentation:** IncreaseIndent, DecreaseIndent
- **Media:** Hyperlink, Image, Table
- **History:** Undo, Redo

### Toolbar Position

The toolbar position varies by platform:
- **Windows/macOS:** Top (default)
- **Android/iOS:** Bottom (default, for better thumb accessibility)

You can override this with the `ToolbarPosition` property (see toolbar.md for details).

## Customizing Toolbar Items

Control which toolbar items appear by populating the `ToolbarItems` collection.

### Minimal Toolbar Example

Show only essential text formatting:

```xml
<rte:SfRichTextEditor ShowToolbar="True">
    <rte:SfRichTextEditor.ToolbarItems>
        <rte:RichTextToolbarItem Type="Bold" />
        <rte:RichTextToolbarItem Type="Italic" />
        <rte:RichTextToolbarItem Type="Underline" />
    </rte:SfRichTextEditor.ToolbarItems>
</rte:SfRichTextEditor>
```

### Standard Toolbar with Separators

Group related items with separators:

```xml
<rte:SfRichTextEditor ShowToolbar="True">
    <rte:SfRichTextEditor.ToolbarItems>
        <!-- Character Formatting -->
        <rte:RichTextToolbarItem Type="Bold" />
        <rte:RichTextToolbarItem Type="Italic" />
        <rte:RichTextToolbarItem Type="Underline" />
        <rte:RichTextToolbarItem Type="Separator" />
        
        <!-- Lists -->
        <rte:RichTextToolbarItem Type="NumberList" />
        <rte:RichTextToolbarItem Type="BulletList" />
        <rte:RichTextToolbarItem Type="Separator" />
        
        <!-- Media -->
        <rte:RichTextToolbarItem Type="Hyperlink" />
        <rte:RichTextToolbarItem Type="Image" />
    </rte:SfRichTextEditor.ToolbarItems>
</rte:SfRichTextEditor>
```

### C# Toolbar Customization

```csharp
SfRichTextEditor richTextEditor = new SfRichTextEditor();
richTextEditor.ShowToolbar = true;

richTextEditor.ToolbarItems.Add(new RichTextToolbarItem() { Type = RichTextToolbarOptions.Bold });
richTextEditor.ToolbarItems.Add(new RichTextToolbarItem() { Type = RichTextToolbarOptions.Italic });
richTextEditor.ToolbarItems.Add(new RichTextToolbarItem() { Type = RichTextToolbarOptions.Underline });
richTextEditor.ToolbarItems.Add(new RichTextToolbarItem() { Type = RichTextToolbarOptions.Separator });
richTextEditor.ToolbarItems.Add(new RichTextToolbarItem() { Type = RichTextToolbarOptions.NumberList });
richTextEditor.ToolbarItems.Add(new RichTextToolbarItem() { Type = RichTextToolbarOptions.BulletList });
richTextEditor.ToolbarItems.Add(new RichTextToolbarItem() { Type = RichTextToolbarOptions.Separator });
richTextEditor.ToolbarItems.Add(new RichTextToolbarItem() { Type = RichTextToolbarOptions.Hyperlink });
richTextEditor.ToolbarItems.Add(new RichTextToolbarItem() { Type = RichTextToolbarOptions.Image });

this.Content = richTextEditor;
```

### Available Toolbar Item Types

All available `RichTextToolbarOptions` values:
- `Bold`, `Italic`, `Underline`, `Strikethrough`
- `SubScript`, `SuperScript`
- `FontFamily`, `FontSize`, `TextColor`, `HighlightColor`
- `ParagraphFormat`, `Alignment`
- `NumberList`, `BulletList`
- `IncreaseIndent`, `DecreaseIndent`
- `Hyperlink`, `Image`, `Table`
- `Undo`, `Redo`
- `Separator`

## Initial Configuration

### Complete Starter Template (XAML)

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:rte="clr-namespace:Syncfusion.Maui.RichTextEditor;assembly=Syncfusion.Maui.RichTextEditor"
             x:Class="MyApp.EditorPage">
    
    <Grid RowDefinitions="Auto,*">
        <!-- Header -->
        <Label Text="Rich Text Editor Demo" 
               FontSize="20" 
               FontAttributes="Bold"
               Margin="10" />
        
        <!-- Editor -->
        <rte:SfRichTextEditor x:Name="richTextEditor"
                              Grid.Row="1"
                              ShowToolbar="True"
                              Placeholder="Start typing..."
                              EditorBackgroundColor="White"
                              BorderColor="Gray"
                              BorderThickness="1"
                              DefaultFontFamily="Arial"
                              DefaultFontSize="14"
                              DefaultTextColor="Black">
            <rte:SfRichTextEditor.ToolbarItems>
                <rte:RichTextToolbarItem Type="Bold" />
                <rte:RichTextToolbarItem Type="Italic" />
                <rte:RichTextToolbarItem Type="Underline" />
                <rte:RichTextToolbarItem Type="Separator" />
                <rte:RichTextToolbarItem Type="FontFamily" />
                <rte:RichTextToolbarItem Type="FontSize" />
                <rte:RichTextToolbarItem Type="Separator" />
                <rte:RichTextToolbarItem Type="Alignment" />
                <rte:RichTextToolbarItem Type="NumberList" />
                <rte:RichTextToolbarItem Type="BulletList" />
                <rte:RichTextToolbarItem Type="Separator" />
                <rte:RichTextToolbarItem Type="Hyperlink" />
                <rte:RichTextToolbarItem Type="Image" />
            </rte:SfRichTextEditor.ToolbarItems>
        </rte:SfRichTextEditor>
    </Grid>
</ContentPage>
```

### Complete Starter Template (C#)

```csharp
using Syncfusion.Maui.RichTextEditor;

namespace MyApp
{
    public partial class EditorPage : ContentPage
    {
        private SfRichTextEditor richTextEditor;
        
        public EditorPage()
        {
            InitializeComponent();
            SetupEditor();
        }
        
        private void SetupEditor()
        {
            // Create editor
            richTextEditor = new SfRichTextEditor
            {
                ShowToolbar = true,
                Placeholder = "Start typing...",
                EditorBackgroundColor = Colors.White,
                BorderColor = Colors.Gray,
                BorderThickness = 1,
                DefaultFontFamily = "Arial",
                DefaultFontSize = 14,
                DefaultTextColor = Colors.Black
            };
            
            // Configure toolbar items
            richTextEditor.ToolbarItems.Add(new RichTextToolbarItem { Type = RichTextToolbarOptions.Bold });
            richTextEditor.ToolbarItems.Add(new RichTextToolbarItem { Type = RichTextToolbarOptions.Italic });
            richTextEditor.ToolbarItems.Add(new RichTextToolbarItem { Type = RichTextToolbarOptions.Underline });
            richTextEditor.ToolbarItems.Add(new RichTextToolbarItem { Type = RichTextToolbarOptions.Separator });
            richTextEditor.ToolbarItems.Add(new RichTextToolbarItem { Type = RichTextToolbarOptions.FontFamily });
            richTextEditor.ToolbarItems.Add(new RichTextToolbarItem { Type = RichTextToolbarOptions.FontSize });
            richTextEditor.ToolbarItems.Add(new RichTextToolbarItem { Type = RichTextToolbarOptions.Separator });
            richTextEditor.ToolbarItems.Add(new RichTextToolbarItem { Type = RichTextToolbarOptions.Alignment });
            richTextEditor.ToolbarItems.Add(new RichTextToolbarItem { Type = RichTextToolbarOptions.NumberList });
            richTextEditor.ToolbarItems.Add(new RichTextToolbarItem { Type = RichTextToolbarOptions.BulletList });
            richTextEditor.ToolbarItems.Add(new RichTextToolbarItem { Type = RichTextToolbarOptions.Separator });
            richTextEditor.ToolbarItems.Add(new RichTextToolbarItem { Type = RichTextToolbarOptions.Hyperlink });
            richTextEditor.ToolbarItems.Add(new RichTextToolbarItem { Type = RichTextToolbarOptions.Image });
            
            // Create layout
            var grid = new Grid
            {
                RowDefinitions =
                {
                    new RowDefinition { Height = GridLength.Auto },
                    new RowDefinition { Height = GridLength.Star }
                }
            };
            
            var header = new Label
            {
                Text = "Rich Text Editor Demo",
                FontSize = 20,
                FontAttributes = FontAttributes.Bold,
                Margin = 10
            };
            
            Grid.SetRow(header, 0);
            Grid.SetRow(richTextEditor, 1);
            
            grid.Children.Add(header);
            grid.Children.Add(richTextEditor);
            
            this.Content = grid;
        }
    }
}
```

## Troubleshooting

### Editor Not Appearing

**Problem:** The Rich Text Editor control doesn't show up on the page.

**Solutions:**
1. Verify the NuGet package is installed and restored
2. Check namespace registration in XAML: `xmlns:rte="clr-namespace:Syncfusion.Maui.RichTextEditor;assembly=Syncfusion.Maui.RichTextEditor"`
3. Ensure the control has appropriate layout sizing (set `VerticalOptions` and `HorizontalOptions`)
4. Check that parent container has available space (e.g., Grid, StackLayout)

### Toolbar Not Showing

**Problem:** Toolbar is not visible even with `ShowToolbar="True"`.

**Solutions:**
1. Verify `ShowToolbar` is set to `True` (it's `False` by default)
2. Check if custom `ToolbarItems` collection is empty (if specified)
3. Ensure sufficient height for the editor container
4. Test on different platforms (toolbar position varies)

### Build Errors After Installation

**Problem:** Build fails with assembly or namespace errors.

**Solutions:**
1. Clean and rebuild the solution
2. Verify all dependencies are installed (check NuGet package manager)
3. Ensure you're targeting compatible .NET MAUI version
4. Check for version conflicts between Syncfusion packages (use same version across all packages)
5. Restart Visual Studio or IDE

### License Warning

**Problem:** License warning message appears in the app.

**Solutions:**
1. Register Syncfusion license key in `MauiProgram.cs` before `builder.Build()`:
   ```csharp
   Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");
   ```
2. Obtain a license from Syncfusion (trial or commercial)
3. Community license is available for qualifying developers

## Next Steps

Now that you have a basic rich text editor working:
- Learn about [toolbar customization](toolbar.md) for complete control over available formatting options
- Explore [formatting and customization](formatting-and-customization.md) for programmatic text styling
- Implement [content management](content-management.md) to load/save HTML content
- Add [images and tables](images-and-tables.md) for richer content
- Wire up [events](events-and-interactions.md) for reactive UI and auto-save features
