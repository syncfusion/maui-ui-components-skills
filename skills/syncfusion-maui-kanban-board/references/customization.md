# Customization and Styling in .NET MAUI Kanban Board

## Table of Contents
- [Overview](#overview)
- [Liquid Glass Effect](#liquid-glass-effect)
- [Theme Integration](#theme-integration)
- [Visual Styling](#visual-styling)
- [Migration from Xamarin.Forms](#migration-from-xamarinforms)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)
- [Next Steps](#next-steps)

## Overview

This reference covers visual customization options for the SfKanban control:

- **Liquid Glass Effect** - Modern, translucent design
- **Theme Integration** - Consistent app theming
- **Visual Styling** - Colors, backgrounds, borders
- **Migration Guidance** - Transitioning from Xamarin

## Liquid Glass Effect

The Liquid Glass Effect provides a modern, translucent design with adaptive color tinting and light refraction, creating a sleek glass-like appearance.

### Prerequisites

Install the Syncfusion.Maui.Core package (required for `SfGlassEffectView`).

```bash
dotnet add package Syncfusion.Maui.Core
```

### Enabling Liquid Glass Effect

**Step 1: Wrap in SfGlassEffectView**

```xml
<ContentPage xmlns:core="clr-namespace:Syncfusion.Maui.Core;assembly=Syncfusion.Maui.Core"
             xmlns:kanban="clr-namespace:Syncfusion.Maui.Kanban;assembly=Syncfusion.Maui.Kanban">
    
    <Grid BackgroundColor="Transparent">
        <core:SfGlassEffectView EffectType="Clear"
                                CornerRadius="7">
            <kanban:SfKanban x:Name="kanban"
                             Background="Transparent"
                             AutoGenerateColumns="False"
                             ItemsSource="{Binding Cards}"
                             EnableLiquidGlassEffect="True">
                <kanban:SfKanban.Columns>
                    <kanban:KanbanColumn Title="To Do" Categories="Open" />
                    <kanban:KanbanColumn Title="In Progress" Categories="In Progress" />
                    <kanban:KanbanColumn Title="Done" Categories="Done" />
                </kanban:SfKanban.Columns>
            </kanban:SfKanban>
        </core:SfGlassEffectView>
    </Grid>
    
</ContentPage>
```

**Step 2: Enable on SfKanban**

```csharp
kanban.EnableLiquidGlassEffect = true;
kanban.Background = Colors.Transparent;
```

### Glass Effect Types

| EffectType | Description | Use For |
|------------|-------------|---------|
| `Clear` | Transparent with subtle blur | Modern, minimal designs |
| `Tinted` | Color-tinted glass | Brand-specific aesthetics |
| `Frosted` | Heavy blur effect | Privacy-focused UIs |

### Example: Full Liquid Glass Setup

```csharp
var glassView = new SfGlassEffectView
{
    EffectType = GlassEffectType.Clear,
    CornerRadius = 10,
    Content = new SfKanban
    {
        Background = Colors.Transparent,
        AutoGenerateColumns = false,
        EnableLiquidGlassEffect = true,
        ItemsSource = viewModel.Cards
    }
};

// Add columns
var kanban = glassView.Content as SfKanban;
kanban.Columns.Add(new KanbanColumn { Title = "To Do", Categories = new List<object> { "Open" } });
// ... more columns

this.Content = new Grid
{
    BackgroundColor = Colors.Transparent,
    Children = { glassView }
};
```

**When to use Liquid Glass Effect:**
- Modern, premium applications
- Applications with background imagery
- Translucent design requirements
- iOS/Material Design aesthetics

## Theme Integration

Integrate the kanban board with your application's theme system.

### Using Application Resources

**App.xaml:**
```xml
<Application.Resources>
    <ResourceDictionary>
        <!-- Kanban Theme Colors -->
        <Color x:Key="KanbanBackgroundColor">#F5F5F5</Color>
        <Color x:Key="KanbanColumnColor">#FFFFFF</Color>
        <Color x:Key="KanbanCardColor">#FAFAFA</Color>
        <Color x:Key="KanbanAccentColor">#2196F3</Color>
        
        <!-- Card Style -->
        <Style x:Key="KanbanCardBorderStyle" TargetType="Border">
            <Setter Property="Background" Value="{StaticResource KanbanCardColor}" />
            <Setter Property="Stroke" Value="{StaticResource KanbanAccentColor}" />
            <Setter Property="StrokeThickness" Value="1" />
            <Setter Property="Padding" Value="10" />
            <Setter Property="Margin" Value="5" />
        </Style>
    </ResourceDictionary>
</Application.Resources>
```

**Usage:**
```xml
<kanban:SfKanban Background="{StaticResource KanbanBackgroundColor}">
    <kanban:SfKanban.CardTemplate>
        <DataTemplate>
            <Border Style="{StaticResource KanbanCardBorderStyle}">
                <!-- Card content -->
            </Border>
        </DataTemplate>
    </kanban:SfKanban.CardTemplate>
</kanban:SfKanban>
```

### Dark/Light Theme Support

```csharp
public class ThemeService
{
    public void ApplyTheme(string themeName)
    {
        var resources = Application.Current.Resources;
        
        if (themeName == "Dark")
        {
            resources["KanbanBackgroundColor"] = Color.FromArgb("#1E1E1E");
            resources["KanbanColumnColor"] = Color.FromArgb("#252526");
            resources["KanbanCardColor"] = Color.FromArgb("#2D2D30");
            resources["KanbanAccentColor"] = Color.FromArgb("#007ACC");
        }
        else // Light
        {
            resources["KanbanBackgroundColor"] = Color.FromArgb("#F5F5F5");
            resources["KanbanColumnColor"] = Colors.White;
            resources["KanbanCardColor"] = Color.FromArgb("#FAFAFA");
            resources["KanbanAccentColor"] = Color.FromArgb("#2196F3");
        }
    }
}
```

### Responsive Theme Switching

```xml
<ContentPage>
    <StackLayout>
        <Switch x:Name="themeSwitch" 
                Toggled="OnThemeToggled" />
        <kanban:SfKanban x:Name="kanban" 
                         Background="{AppThemeBinding Light={StaticResource LightBackground},
                                                      Dark={StaticResource DarkBackground}}" />
    </StackLayout>
</ContentPage>
```

```csharp
private void OnThemeToggled(object sender, ToggledEventArgs e)
{
    Application.Current.UserAppTheme = e.Value
        ? AppTheme.Dark
        : AppTheme.Light;
}
```

## Visual Styling

### Column Background Colors

```xml
<kanban:SfKanban.Columns>
    <kanban:KanbanColumn Title="To Do" 
                         Categories="Open"
                         Background="#FFF3E0" />
    <kanban:KanbanColumn Title="In Progress" 
                         Categories="In Progress"
                         Background="#E3F2FD" />
    <kanban:KanbanColumn Title="Done" 
                         Categories="Done"
                         Background="#E8F5E9" />
</kanban:SfKanban.Columns>
```

### Card Styling with Custom Template

```xml
<kanban:SfKanban.CardTemplate>
    <DataTemplate>
        <Border Background="White"
                Stroke="#E0E0E0"
                StrokeThickness="1"
                Padding="12"
                Margin="8,4">
            <Border.Shadow>
                <Shadow Brush="Black" 
                        Opacity="0.1"
                        Radius="8"
                        Offset="0,2" />
            </Border.Shadow>
            
            <Grid RowDefinitions="Auto,Auto,Auto">
                <!-- Title with gradient background -->
                <Border Grid.Row="0" 
                        Padding="8,4"
                        Margin="0,0,0,8">
                    <Border.Background>
                        <LinearGradientBrush StartPoint="0,0" EndPoint="1,0">
                            <GradientStop Color="#2196F3" Offset="0" />
                            <GradientStop Color="#21CBF3" Offset="1" />
                        </LinearGradientBrush>
                    </Border.Background>
                    <Label Text="{Binding Title}"
                           TextColor="White"
                           FontAttributes="Bold" />
                </Border>
                
                <!-- Description -->
                <Label Grid.Row="1"
                       Text="{Binding Description}"
                       TextColor="#666"
                       LineBreakMode="WordWrap" />
                
                <!-- Tags with custom style -->
                <FlexLayout Grid.Row="2"
                            Wrap="Wrap"
                            Margin="0,8,0,0"
                            BindableLayout.ItemsSource="{Binding Tags}">
                    <BindableLayout.ItemTemplate>
                        <DataTemplate>
                            <Border Background="#E3F2FD"
                                    Padding="8,4"
                                    Margin="4,2"
                                    StrokeThickness="0">
                                <Border.StrokeShape>
                                    <RoundRectangle CornerRadius="12" />
                                </Border.StrokeShape>
                                <Label Text="{Binding .}"
                                       TextColor="#1976D2"
                                       FontSize="10" />
                            </Border>
                        </DataTemplate>
                    </BindableLayout.ItemTemplate>
                </FlexLayout>
            </Grid>
        </Border>
    </DataTemplate>
</kanban:SfKanban.CardTemplate>
```

### Custom Header Styling

```xml
<kanban:SfKanban.HeaderTemplate>
    <DataTemplate>
        <Border Background="#F5F5F5"
                Padding="12"
                Margin="4,0">
            <Grid ColumnDefinitions="*,Auto">
                <Label Grid.Column="0"
                       Text="{Binding Title}"
                       FontAttributes="Bold"
                       FontSize="16"
                       TextColor="#333" />
                <Border Grid.Column="1"
                        Background="#2196F3"
                        Padding="6,2"
                        StrokeThickness="0">
                    <Border.StrokeShape>
                        <RoundRectangle CornerRadius="10" />
                    </Border.StrokeShape>
                    <Label Text="{Binding ItemsCount}"
                           TextColor="White"
                           FontSize="12"
                           FontAttributes="Bold" />
                </Border>
            </Grid>
        </Border>
    </DataTemplate>
</kanban:SfKanban.HeaderTemplate>
```

## Migration from Xamarin.Forms

### Namespace Changes

**Xamarin:**
```csharp
using Syncfusion.SfKanban.XForms;
```

**MAUI:**
```csharp
using Syncfusion.Maui.Kanban;
```

### XAML Namespace

**Xamarin:**
```xml
xmlns:kanban="clr-namespace:Syncfusion.SfKanban.XForms;assembly=Syncfusion.SfKanban.XForms"
```

**MAUI:**
```xml
xmlns:kanban="clr-namespace:Syncfusion.Maui.Kanban;assembly=Syncfusion.Maui.Kanban"
```

### API Changes

| Xamarin API | MAUI API | Notes |
|-------------|----------|-------|
| `SfKanban` | `SfKanban` | Same name, different namespace |
| `KanbanModel` | `KanbanModel` | Same |
| `KanbanColumn` | `KanbanColumn` | Same |
| Properties | Properties | Mostly same, check migration docs |

### Property Updates

Most properties remain the same, but verify specific implementations:

**Xamarin:**
```xml
<kanban:SfKanban AutoGenerateColumns="False" />
```

**MAUI (Same):**
```xml
<kanban:SfKanban AutoGenerateColumns="False" />
```

### Handler Registration (MAUI Specific)

**Required in MauiProgram.cs:**
```csharp
using Syncfusion.Maui.Core.Hosting;

builder.ConfigureSyncfusionCore();
```

### Migration Checklist

- [ ] Update package from `Syncfusion.Xamarin.SfKanban` to `Syncfusion.Maui.Kanban`
- [ ] Update namespaces in C# files
- [ ] Update XAML namespace declarations
- [ ] Add `ConfigureSyncfusionCore()` to MauiProgram.cs
- [ ] Test all custom templates
- [ ] Verify event handlers work
- [ ] Check workflow configurations
- [ ] Test data binding
- [ ] Validate custom styling

## Best Practices

### Performance

1. **Use lightweight card templates** - Avoid complex layouts
2. **Limit tags per card** - 2-4 tags maximum
3. **Optimize images** - Use appropriate sizes
4. **Virtual scrolling** - Automatic for large datasets

### Visual Design

1. **Consistent colors** - Use theme resources
2. **Adequate spacing** - Padding and margins for readability
3. **Contrast** - Ensure text is readable
4. **Touch targets** - Minimum 44x44 pixels for interactive elements

### Theming

1. **Use resource dictionaries** - Centralize styles
2. **Support dark mode** - Test in both themes
3. **Brand consistency** - Match company colors
4. **Accessibility** - High contrast mode support

## Troubleshooting

### Issue: Liquid glass effect not visible

**Check:**
1. `EnableLiquidGlassEffect` is `true`
2. Background is `Transparent`
3. Wrapped in `SfGlassEffectView`
4. Syncfusion.Maui.Core package installed

### Issue: Theme not applying

**Solution:**
```csharp
// Force resource update
Application.Current.Resources["KanbanBackgroundColor"] = newColor;

// Recreate content
kanban.Background = (Color)Application.Current.Resources["KanbanBackgroundColor"];
```

### Issue: Custom template not displaying

**Check:**
1. `CardTemplate` is set
2. Bindings use correct property names
3. DataTemplate returns valid content

## Next Steps

- **Configure columns:** See [columns.md](columns.md)
- **Customize cards:** See [cards.md](cards.md)
- **Handle events:** See [events.md](events.md)
- **Implement workflows:** See [workflows.md](workflows.md)