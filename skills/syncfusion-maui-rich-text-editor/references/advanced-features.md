# Advanced Features

## Table of Contents
- [Overview](#overview)
- [AutoSize Functionality](#autosize-functionality)
- [Liquid Glass Effect](#liquid-glass-effect)
- [Combining Advanced Features](#combining-advanced-features)
- [Best Practices](#best-practices)

## Overview

The Rich Text Editor includes advanced features for modern UI experiences:
- **AutoSize** - Dynamic height adjustment based on content
- **Liquid Glass Effect** - Modern Cupertino-style visual effects (iOS/macOS)

These features enhance usability and provide polished, platform-appropriate interfaces.

## AutoSize Functionality

### Overview

The `EnableAutoSize` property allows the editor to dynamically adjust its height to fit the content. This is particularly useful for comment sections, note-taking apps, and forms where the editor should grow/shrink with content.

### Property

- **EnableAutoSize** - Boolean property (default: `False`)

### XAML Configuration

```xml
<VerticalStackLayout>
    <rte:SfRichTextEditor x:Name="richTextEditor"
                          EnableAutoSize="True"
                          ShowToolbar="True" />
</VerticalStackLayout>
```

### C# Configuration

```csharp
VerticalStackLayout layout = new VerticalStackLayout();
SfRichTextEditor richTextEditor = new SfRichTextEditor
{
    EnableAutoSize = true,
    ShowToolbar = true
};
layout.Children.Add(richTextEditor);
this.Content = layout;
```

### Behavior

When `EnableAutoSize` is enabled:
- Editor height increases as content is added
- Editor height decreases as content is removed
- Height adjusts automatically without scrolling the editor itself
- Parent container should accommodate dynamic sizing

### Layout Considerations

**Recommended Parent Layouts:**
- `VerticalStackLayout` - Best for auto-sizing
- `ScrollView` with `VerticalStackLayout` - For scrollable pages
- `Grid` with `RowDefinition Height="Auto"` - For grid layouts

**XAML Example with ScrollView:**
```xml
<ScrollView>
    <VerticalStackLayout Spacing="10" Padding="10">
        <Label Text="Your Notes" FontSize="20" FontAttributes="Bold" />
        
        <rte:SfRichTextEditor x:Name="richTextEditor"
                              EnableAutoSize="True"
                              ShowToolbar="True"
                              Placeholder="Write your notes..." />
        
        <Button Text="Save" Clicked="OnSaveClicked" />
    </VerticalStackLayout>
</ScrollView>
```

### Use Cases

**Comment Section:**
```xml
<ScrollView>
    <VerticalStackLayout>
        <!-- Existing comments -->
        <Label Text="Add Comment" FontSize="16" FontAttributes="Bold" />
        
        <rte:SfRichTextEditor EnableAutoSize="True"
                              Placeholder="Write your comment..."
                              BorderColor="LightGray"
                              BorderThickness="1" />
        
        <Button Text="Post Comment" HorizontalOptions="End" />
    </VerticalStackLayout>
</ScrollView>
```

**Note-Taking App:**
```xml
<Grid RowDefinitions="Auto,*,Auto">
    <!-- Title -->
    <Entry Grid.Row="0" 
           Placeholder="Note Title" 
           FontSize="18" />
    
    <!-- Auto-sizing editor -->
    <ScrollView Grid.Row="1">
        <rte:SfRichTextEditor EnableAutoSize="True"
                              ShowToolbar="True"
                              Placeholder="Take notes..." />
    </ScrollView>
    
    <!-- Actions -->
    <HorizontalStackLayout Grid.Row="2" Spacing="10">
        <Button Text="Save" />
        <Button Text="Delete" />
    </HorizontalStackLayout>
</Grid>
```

**Email Composer:**
```csharp
public class EmailComposerPage : ContentPage
{
    public EmailComposerPage()
    {
        var scrollView = new ScrollView();
        var layout = new VerticalStackLayout { Spacing = 10, Padding = 10 };
        
        // To field
        layout.Children.Add(new Entry { Placeholder = "To" });
        
        // Subject field
        layout.Children.Add(new Entry { Placeholder = "Subject" });
        
        // Auto-sizing body editor
        var bodyEditor = new SfRichTextEditor
        {
            EnableAutoSize = true,
            ShowToolbar = true,
            Placeholder = "Compose email..."
        };
        layout.Children.Add(bodyEditor);
        
        // Send button
        layout.Children.Add(new Button { Text = "Send", HorizontalOptions = LayoutOptions.End });
        
        scrollView.Content = layout;
        Content = scrollView;
    }
}
```

**Feedback Form:**
```xml
<ScrollView>
    <VerticalStackLayout Spacing="15" Padding="20">
        <Label Text="Feedback Form" FontSize="24" FontAttributes="Bold" />
        
        <Entry Placeholder="Your Name" />
        <Entry Placeholder="Email" Keyboard="Email" />
        
        <Label Text="Your Feedback" FontSize="16" />
        <rte:SfRichTextEditor EnableAutoSize="True"
                              Placeholder="Share your thoughts..."
                              BorderColor="Gray"
                              BorderThickness="1"
                              MinimumHeightRequest="100" />
        
        <Button Text="Submit Feedback" />
    </VerticalStackLayout>
</ScrollView>
```

### Important Notes

**Don't Set HeightRequest:**
When using `EnableAutoSize`, avoid setting `HeightRequest` as it interferes with automatic sizing:

```xml
<!-- ✓ Good -->
<rte:SfRichTextEditor EnableAutoSize="True" />

<!-- ✗ Avoid -->
<rte:SfRichTextEditor EnableAutoSize="True" HeightRequest="300" />
```

**Use MinimumHeightRequest Instead:**
To set a minimum height:

```xml
<rte:SfRichTextEditor EnableAutoSize="True" 
                      MinimumHeightRequest="100" />
```

### When to Use AutoSize

**Use AutoSize when:**
- Editor should expand/contract with content
- Building comment sections or feedback forms
- Creating note-taking or messaging apps
- Editor is within a scrollable container
- Content amount is unpredictable

**Don't use AutoSize when:**
- Editor should maintain fixed height
- Building full-screen document editors
- Page layout requires fixed dimensions
- Performance is critical with very large content

## Liquid Glass Effect

### Overview

The Liquid Glass Effect provides a modern, sleek Cupertino-style visual aesthetic with smooth rounded corners, refined color palettes, and sophisticated visual treatments. This creates a polished, professional appearance for the Rich Text Editor on iOS and macOS.

### Platform Requirements

- **.NET 10** or later
- **iOS 26+** or **macOS 26+**

### Property

- **EnableLiquidGlassEffect** - Boolean property (default: `False`)

### What It Enhances

The Liquid Glass Effect applies to:
- **Toolbar:**
  - Font family picker
  - Font size picker
  - Text alignment options
  - Text style pickers
  - Insert link popup
  - Table selection
  - Table context menu popup
  - Inline toolbar for links
- **Editor:** Polished text editing area with modern aesthetics

### Basic Configuration (XAML)

```xml
<rte:SfRichTextEditor EnableLiquidGlassEffect="True"
                      EditorBackgroundColor="Transparent">
    <rte:SfRichTextEditor.ToolbarSettings>
        <rte:RichTextEditorToolbarSettings BackgroundColor="Transparent" />
    </rte:SfRichTextEditor.ToolbarSettings>
</rte:SfRichTextEditor>
```

### Basic Configuration (C#)

```csharp
SfRichTextEditor richTextEditor = new SfRichTextEditor
{
    EnableLiquidGlassEffect = true,
    EditorBackgroundColor = Colors.Transparent
};

richTextEditor.ToolbarSettings = new RichTextEditorToolbarSettings
{
    BackgroundColor = Colors.Transparent
};
```

### Optimal Configuration

For best visual appearance with a sleek, glassy output:

```xml
<rte:SfRichTextEditor EnableLiquidGlassEffect="True"
                      ShowToolbar="True"
                      EditorBackgroundColor="Transparent"
                      BorderThickness="0">
    <rte:SfRichTextEditor.ToolbarSettings>
        <rte:RichTextEditorToolbarSettings BackgroundColor="Transparent" />
    </rte:SfRichTextEditor.ToolbarSettings>
</rte:SfRichTextEditor>
```

### Customizing Corner Radius

Customize the toolbar and editor corner radius using Syncfusion theme keys:

**App.xaml:**
```xml
<Application xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:syncTheme="clr-namespace:Syncfusion.Maui.Themes;assembly=Syncfusion.Maui.Core"
             xmlns:sys="clr-namespace:System;assembly=mscorlib">
    <Application.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <syncTheme:SyncfusionThemeResourceDictionary VisualTheme="CupertinoLight"/>
                <ResourceDictionary>
                    <sys:Double x:Key="SfRichTextEditorToolbarCornerRadius">25</sys:Double>
                    <sys:Double x:Key="SfRichTextEditorToolbarSelectionCornerRadius">25</sys:Double>
                    <sys:Double x:Key="SfRichTextEditorCornerRadius">15</sys:Double>
                </ResourceDictionary>
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </Application.Resources>
</Application>
```

### Theme Keys

Available theme customization keys:

- **SfRichTextEditorToolbarCornerRadius** - Toolbar background corner radius
- **SfRichTextEditorToolbarSelectionCornerRadius** - Toolbar selection indicator corner radius
- **SfRichTextEditorCornerRadius** - Editor content area corner radius

### Complete Example

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:rte="clr-namespace:Syncfusion.Maui.RichTextEditor;assembly=Syncfusion.Maui.RichTextEditor"
             x:Class="MyApp.EditorPage"
             BackgroundColor="#F5F5F7">
    
    <Grid Padding="20">
        <rte:SfRichTextEditor x:Name="richTextEditor"
                              EnableLiquidGlassEffect="True"
                              ShowToolbar="True"
                              EditorBackgroundColor="Transparent"
                              BorderThickness="0"
                              Placeholder="Start typing...">
            <rte:SfRichTextEditor.ToolbarSettings>
                <rte:RichTextEditorToolbarSettings BackgroundColor="Transparent" />
            </rte:SfRichTextEditor.ToolbarSettings>
        </rte:SfRichTextEditor>
    </Grid>
</ContentPage>
```

### C# Complete Example

```csharp
using Syncfusion.Maui.RichTextEditor;

namespace MyApp
{
    public partial class EditorPage : ContentPage
    {
        public EditorPage()
        {
            InitializeComponent();
            SetupGlassEditor();
        }
        
        private void SetupGlassEditor()
        {
            // Set page background
            this.BackgroundColor = Color.FromRgb(245, 245, 247);
            
            // Create editor with liquid glass effect
            var richTextEditor = new SfRichTextEditor
            {
                EnableLiquidGlassEffect = true,
                ShowToolbar = true,
                EditorBackgroundColor = Colors.Transparent,
                BorderThickness = 0,
                Placeholder = "Start typing...",
                Margin = new Thickness(20)
            };
            
            richTextEditor.ToolbarSettings = new RichTextEditorToolbarSettings
            {
                BackgroundColor = Colors.Transparent
            };
            
            this.Content = richTextEditor;
        }
    }
}
```

### When to Use Liquid Glass Effect

**Use Liquid Glass Effect when:**
- Targeting iOS/macOS (26+)
- Building modern, minimalist UI
- App design follows Apple HIG
- Using Cupertino theme
- Want polished, professional appearance

**Don't use when:**
- Targeting Android/Windows primarily
- Need consistent cross-platform appearance
- Using Material Design theme
- Platform requirements not met (.NET 10, iOS/macOS 26+)

### Platform-Specific Styling

```csharp
#if IOS || MACCATALYST
    // Enable liquid glass on Apple platforms
    richTextEditor.EnableLiquidGlassEffect = true;
    richTextEditor.EditorBackgroundColor = Colors.Transparent;
    richTextEditor.ToolbarSettings.BackgroundColor = Colors.Transparent;
#else
    // Standard appearance on other platforms
    richTextEditor.EditorBackgroundColor = Colors.White;
    richTextEditor.ToolbarSettings.BackgroundColor = Colors.LightGray;
#endif
```

## Combining Advanced Features

### AutoSize + Liquid Glass Effect

Create a modern, adaptive editor:

```xml
<ScrollView BackgroundColor="#F5F5F7">
    <VerticalStackLayout Spacing="15" Padding="20">
        <Label Text="Beautiful Notes" 
               FontSize="28" 
               FontAttributes="Bold" />
        
        <rte:SfRichTextEditor EnableAutoSize="True"
                              EnableLiquidGlassEffect="True"
                              ShowToolbar="True"
                              EditorBackgroundColor="Transparent"
                              BorderThickness="0"
                              Placeholder="Write something beautiful..."
                              MinimumHeightRequest="150">
            <rte:SfRichTextEditor.ToolbarSettings>
                <rte:RichTextEditorToolbarSettings BackgroundColor="Transparent" />
            </rte:SfRichTextEditor.ToolbarSettings>
        </rte:SfRichTextEditor>
        
        <Button Text="Save Note" 
                BackgroundColor="#007AFF" 
                TextColor="White" />
    </VerticalStackLayout>
</ScrollView>
```

### C# Combined Configuration

```csharp
public class ModernNotesPage : ContentPage
{
    public ModernNotesPage()
    {
        BackgroundColor = Color.FromRgb(245, 245, 247);
        
        var scrollView = new ScrollView();
        var layout = new VerticalStackLayout { Spacing = 15, Padding = 20 };
        
        // Title
        layout.Children.Add(new Label
        {
            Text = "Beautiful Notes",
            FontSize = 28,
            FontAttributes = FontAttributes.Bold
        });
        
        // Editor with both features
        var editor = new SfRichTextEditor
        {
            EnableAutoSize = true,
            EnableLiquidGlassEffect = true,
            ShowToolbar = true,
            EditorBackgroundColor = Colors.Transparent,
            BorderThickness = 0,
            Placeholder = "Write something beautiful...",
            MinimumHeightRequest = 150
        };
        
        editor.ToolbarSettings = new RichTextEditorToolbarSettings
        {
            BackgroundColor = Colors.Transparent
        };
        
        layout.Children.Add(editor);
        
        // Save button
        layout.Children.Add(new Button
        {
            Text = "Save Note",
            BackgroundColor = Color.FromRgb(0, 122, 255),
            TextColor = Colors.White
        });
        
        scrollView.Content = layout;
        Content = scrollView;
    }
}
```

### Responsive Design Pattern

```csharp
public class ResponsiveEditorPage : ContentPage
{
    private SfRichTextEditor editor;
    
    public ResponsiveEditorPage()
    {
        SetupEditor();
        
        // Adjust based on device
        if (DeviceInfo.Idiom == DeviceIdiom.Phone)
        {
            SetupMobileEditor();
        }
        else if (DeviceInfo.Idiom == DeviceIdiom.Tablet)
        {
            SetupTabletEditor();
        }
        else
        {
            SetupDesktopEditor();
        }
    }
    
    private void SetupEditor()
    {
        editor = new SfRichTextEditor
        {
            ShowToolbar = true
        };
    }
    
    private void SetupMobileEditor()
    {
        // AutoSize for mobile (limited screen space)
        editor.EnableAutoSize = true;
        
#if IOS
        editor.EnableLiquidGlassEffect = true;
        editor.EditorBackgroundColor = Colors.Transparent;
        editor.ToolbarSettings = new RichTextEditorToolbarSettings
        {
            BackgroundColor = Colors.Transparent
        };
#endif
    }
    
    private void SetupTabletEditor()
    {
        // Fixed height for tablet
        editor.HeightRequest = 400;
        
#if IOS
        editor.EnableLiquidGlassEffect = true;
        editor.EditorBackgroundColor = Colors.Transparent;
        editor.ToolbarSettings = new RichTextEditorToolbarSettings
        {
            BackgroundColor = Colors.Transparent
        };
#endif
    }
    
    private void SetupDesktopEditor()
    {
        // Full height for desktop
        editor.VerticalOptions = LayoutOptions.Fill;
    }
}
```

## Best Practices

### 1. Test AutoSize with Various Content

```csharp
public async Task TestAutoSize()
{
    // Test with short content
    richTextEditor.Text = "Short text";
    await Task.Delay(100);
    
    // Test with long content
    richTextEditor.Text = string.Join("\n", Enumerable.Repeat("Long paragraph", 50));
    await Task.Delay(100);
    
    // Verify layout adjusts correctly
}
```

### 2. Set Minimum Height for AutoSize

```xml
<!-- Prevent editor from becoming too small -->
<rte:SfRichTextEditor EnableAutoSize="True"
                      MinimumHeightRequest="100" />
```

### 3. Use Appropriate Parent Layout

```xml
<!-- ✓ Good: VerticalStackLayout -->
<VerticalStackLayout>
    <rte:SfRichTextEditor EnableAutoSize="True" />
</VerticalStackLayout>

<!-- ✗ Avoid: Grid with fixed height -->
<Grid RowDefinitions="300">
    <rte:SfRichTextEditor EnableAutoSize="True" />
</Grid>
```

### 4. Liquid Glass Effect Platform Checks

```csharp
#if IOS || MACCATALYST
    if (DeviceInfo.Version.Major >= 26)
    {
        richTextEditor.EnableLiquidGlassEffect = true;
    }
#endif
```

### 5. Transparent Backgrounds for Glass Effect

```csharp
// Both editor and toolbar should be transparent
richTextEditor.EditorBackgroundColor = Colors.Transparent;
richTextEditor.ToolbarSettings.BackgroundColor = Colors.Transparent;

// Use page background for base color
this.BackgroundColor = Color.FromRgb(245, 245, 247);
```

### 6. Test on Target Platforms

- Test AutoSize on different screen sizes
- Verify Liquid Glass Effect on iOS/macOS 26+
- Check performance with large content
- Validate layout in portrait and landscape

### 7. Provide Fallback for Unsupported Platforms

```csharp
public void ConfigureEditor()
{
#if IOS || MACCATALYST
    if (DeviceInfo.Version.Major >= 26)
    {
        richTextEditor.EnableLiquidGlassEffect = true;
        richTextEditor.EditorBackgroundColor = Colors.Transparent;
        richTextEditor.ToolbarSettings.BackgroundColor = Colors.Transparent;
    }
    else
    {
        // Fallback styling for older iOS/macOS
        ApplyStandardStyling();
    }
#else
    // Standard styling for other platforms
    ApplyStandardStyling();
#endif
}

private void ApplyStandardStyling()
{
    richTextEditor.EditorBackgroundColor = Colors.White;
    richTextEditor.BorderColor = Colors.LightGray;
    richTextEditor.BorderThickness = 1;
}
```

### 8. Consider Performance

AutoSize with very large content may impact performance:

```csharp
private const int MaxAutoSizeContent = 10000; // characters

public void SetContent(string content)
{
    if (content.Length > MaxAutoSizeContent)
    {
        // Disable AutoSize for very large content
        richTextEditor.EnableAutoSize = false;
        richTextEditor.HeightRequest = 500;
    }
    else
    {
        richTextEditor.EnableAutoSize = true;
    }
    
    richTextEditor.Text = content;
}
```

## Next Steps

- Explore [content management](content-management.md) for loading/saving content
- Review [events and interactions](events-and-interactions.md) for tracking changes
- Learn about [formatting and customization](formatting-and-customization.md) for styling
