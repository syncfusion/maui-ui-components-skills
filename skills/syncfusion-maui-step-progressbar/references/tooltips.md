# Tooltips in StepProgressBar

## Table of Contents
- [Overview](#overview)
- [Enable Tooltips](#enable-tooltips)
- [Setting Tooltip Text](#setting-tooltip-text)
- [Customize Tooltip Appearance](#customize-tooltip-appearance)
- [Custom Tooltip Templates](#custom-tooltip-templates)
- [Complete Examples](#complete-examples)
- [Best Practices](#best-practices)

## Overview

Tooltips provide additional context and information when users hover over or tap steps in the StepProgressBar. They're useful for displaying hints, instructions, timestamps, or status details without cluttering the main UI.

Tooltip features:
- Show/hide on hover or tap
- Customizable duration
- Custom styling (colors, fonts, borders)
- DataTemplate support for complex layouts
- Automatic positioning

## Enable Tooltips

Tooltips are **disabled by default**. Enable them using the `ShowToolTip` property.

### Basic Enable

**XAML:**
```xml
<stepProgressBar:SfStepProgressBar
    x:Name="stepProgress"
    Orientation="Horizontal"
    ShowToolTip="True"
    ItemsSource="{Binding StepProgressItem}">
</stepProgressBar:SfStepProgressBar>
```

**C#:**
```csharp
SfStepProgressBar stepProgressBar = new SfStepProgressBar();
ViewModel viewModel = new ViewModel();
stepProgressBar.ItemsSource = viewModel.StepProgressItem;
stepProgressBar.Orientation = StepProgressBarOrientation.Horizontal;
stepProgressBar.ShowToolTip = true;

this.Content = stepProgressBar;
```

**Important:** Tooltips only display when `ToolTipText` is provided for each step.

## Setting Tooltip Text

Add tooltip content using the `ToolTipText` property on `StepProgressBarItem`.

### ViewModel with Tooltip Text

```csharp
using System.Collections.ObjectModel;
using Syncfusion.Maui.ProgressBar;

public class ViewModel
{
    public ObservableCollection<StepProgressBarItem> StepProgressItem { get; set; }
    
    public ViewModel()
    {
        StepProgressItem = new ObservableCollection<StepProgressBarItem>();
        
        StepProgressItem.Add(new StepProgressBarItem() 
        { 
            PrimaryText = "Cart", 
            ToolTipText = "Add items to cart" 
        });
        
        StepProgressItem.Add(new StepProgressBarItem() 
        { 
            PrimaryText = "Address", 
            ToolTipText = "Add delivery address" 
        });
        
        StepProgressItem.Add(new StepProgressBarItem() 
        { 
            PrimaryText = "Payment", 
            ToolTipText = "Choose payment method" 
        });
        
        StepProgressItem.Add(new StepProgressBarItem() 
        { 
            PrimaryText = "Ordered", 
            ToolTipText = "Place your order" 
        });
    }
}
```

### XAML with Binding

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:stepProgressBar="clr-namespace:Syncfusion.Maui.ProgressBar;assembly=Syncfusion.Maui.ProgressBar"
             xmlns:local="clr-namespace:YourNamespace"
             x:Class="YourNamespace.MainPage">
    
    <ContentPage.BindingContext>
        <local:ViewModel />
    </ContentPage.BindingContext>
    
    <stepProgressBar:SfStepProgressBar
        x:Name="stepProgress"
        Orientation="Horizontal"
        ShowToolTip="True"
        ItemsSource="{Binding StepProgressItem}">
    </stepProgressBar:SfStepProgressBar>
    
</ContentPage>
```

Now when users hover or tap a step, the tooltip appears with the specified text.

## Customize Tooltip Appearance

Use `ToolTipSettings` to customize tooltip styling.

### Available Properties

| Property | Type | Description |
|----------|------|-------------|
| `Background` | `Brush` | Tooltip background color |
| `Stroke` | `Brush` | Tooltip border color |
| `Duration` | `TimeSpan` | How long tooltip remains visible |
| `TextStyle` | `StepTextStyle` | Text color, font, size, attributes |

### Basic Styling Example

**XAML:**
```xml
<stepProgressBar:SfStepProgressBar 
    x:Name="stepProgressBar"
    ItemsSource="{Binding StepProgressItem}"
    Orientation="Horizontal"
    ShowToolTip="True">
    
    <stepProgressBar:SfStepProgressBar.ToolTipSettings>
        <stepProgressBar:StepProgressBarToolTipSettings 
            Background="Blue"
            Stroke="Red"
            Duration="0:0:10">
            <stepProgressBar:StepProgressBarToolTipSettings.TextStyle>
                <stepProgressBar:StepTextStyle 
                    TextColor="White"
                    FontSize="14"
                    FontAttributes="Italic"/>
            </stepProgressBar:StepProgressBarToolTipSettings.TextStyle>
        </stepProgressBar:StepProgressBarToolTipSettings>
    </stepProgressBar:SfStepProgressBar.ToolTipSettings>
    
    <stepProgressBar:SfStepProgressBar.BindingContext>
        <local:ViewModel />
    </stepProgressBar:SfStepProgressBar.BindingContext>
</stepProgressBar:SfStepProgressBar>
```

**C#:**
```csharp
SfStepProgressBar stepProgressBar = new SfStepProgressBar();
ViewModel viewModel = new ViewModel();
stepProgressBar.ItemsSource = viewModel.StepProgressItem;
stepProgressBar.Orientation = StepProgressBarOrientation.Horizontal;
stepProgressBar.ShowToolTip = true;

stepProgressBar.ToolTipSettings = new StepProgressBarToolTipSettings() 
{ 
    Background = Brush.Blue, 
    Stroke = Brush.Red, 
    Duration = new TimeSpan(0, 0, 10),  // 10 seconds
    TextStyle = new StepTextStyle() 
    { 
        TextColor = Colors.White, 
        FontSize = 14, 
        FontAttributes = FontAttributes.Italic 
    } 
};

this.Content = stepProgressBar;
```

### Duration Examples

```csharp
// Show for 5 seconds
Duration = new TimeSpan(0, 0, 5)

// Show for 30 seconds
Duration = new TimeSpan(0, 0, 30)

// Show for 1.5 seconds
Duration = new TimeSpan(0, 0, 0, 1, 500)
```

**Recommendation:** 3-10 seconds for typical use. Longer durations for instructional content.

### Color Scheme Examples

**Professional Blue:**
```xml
<stepProgressBar:StepProgressBarToolTipSettings 
    Background="#2196F3"
    Stroke="#1976D2"
    Duration="0:0:5">
    <stepProgressBar:StepProgressBarToolTipSettings.TextStyle>
        <stepProgressBar:StepTextStyle 
            TextColor="White"
            FontSize="13"/>
    </stepProgressBar:StepProgressBarToolTipSettings.TextStyle>
</stepProgressBar:StepProgressBarToolTipSettings>
```

**Success Green:**
```xml
<stepProgressBar:StepProgressBarToolTipSettings 
    Background="#4CAF50"
    Stroke="#388E3C"
    Duration="0:0:5">
    <stepProgressBar:StepProgressBarToolTipSettings.TextStyle>
        <stepProgressBar:StepTextStyle 
            TextColor="White"
            FontSize="13"
            FontAttributes="Bold"/>
    </stepProgressBar:StepProgressBarToolTipSettings.TextStyle>
</stepProgressBar:StepProgressBarToolTipSettings>
```

**Dark Theme:**
```xml
<stepProgressBar:StepProgressBarToolTipSettings 
    Background="#212121"
    Stroke="#424242"
    Duration="0:0:7">
    <stepProgressBar:StepProgressBarToolTipSettings.TextStyle>
        <stepProgressBar:StepTextStyle 
            TextColor="#EEEEEE"
            FontSize="12"/>
    </stepProgressBar:StepProgressBarToolTipSettings.TextStyle>
</stepProgressBar:StepProgressBarToolTipSettings>
```

## Custom Tooltip Templates

For advanced layouts, use `ToolTipTemplate` with `DataTemplate`.

### Basic Custom Template

**XAML:**
```xml
<stepProgressBar:SfStepProgressBar 
    x:Name="stepProgressBar"
    ItemsSource="{Binding StepProgressItem}"
    Orientation="Horizontal"
    ShowToolTip="True">
    
    <stepProgressBar:SfStepProgressBar.ToolTipTemplate>
        <DataTemplate>
            <StackLayout Orientation="Horizontal">
                <Image Source="info.png" 
                       WidthRequest="20" 
                       HeightRequest="20"/>
                <Label Text="{Binding ToolTipText}" 
                       TextColor="White"
                       FontSize="Caption"
                       Padding="5,0,0,0"
                       VerticalOptions="Center"/>
            </StackLayout>
        </DataTemplate>
    </stepProgressBar:SfStepProgressBar.ToolTipTemplate>
    
    <stepProgressBar:SfStepProgressBar.BindingContext>
        <local:ViewModel />
    </stepProgressBar:SfStepProgressBar.BindingContext>
</stepProgressBar:SfStepProgressBar>
```

**C#:**
```csharp
SfStepProgressBar stepProgressBar = new SfStepProgressBar();
ViewModel viewModel = new ViewModel();
stepProgressBar.Orientation = StepProgressBarOrientation.Horizontal;
stepProgressBar.ShowToolTip = true;
stepProgressBar.ItemsSource = viewModel.StepProgressItem;

DataTemplate toolTipTemplate = new DataTemplate(() =>
{
    var stackLayout = new StackLayout();
    stackLayout.Orientation = StackOrientation.Horizontal;
    
    Image image = new Image
    {
        Source = "info.png",
        WidthRequest = 20,
        HeightRequest = 20
    };
    
    var label = new Label
    {
        TextColor = Colors.White,
        Padding = new Thickness(5, 0, 0, 0),
        VerticalOptions = LayoutOptions.Center
    };
    label.SetBinding(Label.TextProperty, "ToolTipText");
    
    stackLayout.Children.Add(image);
    stackLayout.Children.Add(label);
    
    return stackLayout;
});

stepProgressBar.ToolTipTemplate = toolTipTemplate;
this.Content = stepProgressBar;
```

### Advanced Template with Multiple Elements

**XAML:**
```xml
<stepProgressBar:SfStepProgressBar.ToolTipTemplate>
    <DataTemplate>
        <Border Background="#2196F3" 
                Stroke="#1976D2" 
                StrokeThickness="2"
                Padding="10"
                CornerRadius="5">
            <Grid RowDefinitions="Auto,Auto,Auto" 
                  RowSpacing="5">
                <!-- Icon -->
                <Image Grid.Row="0" 
                       Source="step_icon.png" 
                       WidthRequest="30" 
                       HeightRequest="30"
                       HorizontalOptions="Center"/>
                
                <!-- Title -->
                <Label Grid.Row="1" 
                       Text="{Binding PrimaryText}" 
                       FontSize="16"
                       FontAttributes="Bold"
                       TextColor="White"
                       HorizontalOptions="Center"/>
                
                <!-- Description -->
                <Label Grid.Row="2" 
                       Text="{Binding ToolTipText}" 
                       FontSize="12"
                       TextColor="#E3F2FD"
                       HorizontalOptions="Center"/>
            </Grid>
        </Border>
    </DataTemplate>
</stepProgressBar:SfStepProgressBar.ToolTipTemplate>
```

### Template with Status Indicator

```xml
<stepProgressBar:SfStepProgressBar.ToolTipTemplate>
    <DataTemplate>
        <Frame BackgroundColor="#263238" 
               BorderColor="#37474F" 
               CornerRadius="8"
               Padding="12"
               HasShadow="True">
            <StackLayout Spacing="8">
                <!-- Step Name -->
                <Label Text="{Binding PrimaryText}" 
                       FontSize="15"
                       FontAttributes="Bold"
                       TextColor="#ECEFF1"/>
                
                <!-- Tooltip Description -->
                <Label Text="{Binding ToolTipText}" 
                       FontSize="12"
                       TextColor="#B0BEC5"/>
                
                <!-- Status Badge -->
                <BoxView Color="#4CAF50" 
                         HeightRequest="3" 
                         HorizontalOptions="Start"
                         WidthRequest="40"/>
            </StackLayout>
        </Frame>
    </DataTemplate>
</stepProgressBar:SfStepProgressBar.ToolTipTemplate>
```

**Important:** When using `ToolTipTemplate`, the following `ToolTipSettings` properties are ignored:
- `Background`
- `Stroke`
- `TextStyle` (TextColor, FontSize, FontFamily, FontAttributes)

Only `Duration` remains applicable.

## Complete Examples

### Example 1: Order Tracking with Status Tooltips

```csharp
public class OrderViewModel
{
    public ObservableCollection<StepProgressBarItem> OrderSteps { get; set; }
    
    public OrderViewModel()
    {
        OrderSteps = new ObservableCollection<StepProgressBarItem>
        {
            new StepProgressBarItem() 
            { 
                PrimaryText = "Order Placed",
                SecondaryText = "Jan 15, 10:30 AM",
                ToolTipText = "Your order has been received and confirmed"
            },
            new StepProgressBarItem() 
            { 
                PrimaryText = "Processing",
                SecondaryText = "Jan 15, 2:15 PM",
                ToolTipText = "Items are being prepared for shipment"
            },
            new StepProgressBarItem() 
            { 
                PrimaryText = "Shipped",
                SecondaryText = "Jan 16, 9:00 AM",
                ToolTipText = "Package has been handed to carrier (Tracking: XYZ123)"
            },
            new StepProgressBarItem() 
            { 
                PrimaryText = "Out for Delivery",
                SecondaryText = "Expected Today",
                ToolTipText = "Package is on the way to your address"
            },
            new StepProgressBarItem() 
            { 
                PrimaryText = "Delivered",
                ToolTipText = "Package will be delivered to your doorstep"
            }
        };
    }
}
```

### Example 2: Form Wizard with Instructional Tooltips

```csharp
public class WizardViewModel
{
    public ObservableCollection<StepProgressBarItem> WizardSteps { get; set; }
    
    public WizardViewModel()
    {
        WizardSteps = new ObservableCollection<StepProgressBarItem>
        {
            new StepProgressBarItem() 
            { 
                PrimaryText = "Account",
                ToolTipText = "Create your account with email and password (8+ characters)"
            },
            new StepProgressBarItem() 
            { 
                PrimaryText = "Profile",
                ToolTipText = "Add your name, photo, and bio to personalize your profile"
            },
            new StepProgressBarItem() 
            { 
                PrimaryText = "Preferences",
                ToolTipText = "Set notification preferences and privacy settings"
            },
            new StepProgressBarItem() 
            { 
                PrimaryText = "Verification",
                ToolTipText = "Verify your email address to activate your account"
            }
        };
    }
}
```

## Best Practices

### Practice 1: Keep Tooltips Concise

**Good:** "Add items to your shopping cart"
**Bad:** "This is the first step where you need to add items from the catalog into your shopping cart before proceeding to the next step"

Aim for 3-10 words. Use clear, actionable language.

### Practice 2: Provide Value

Tooltips should add information not already visible:

**Good Use Cases:**
- Explain what action to take
- Show timestamps or IDs
- Display status details
- Provide keyboard shortcuts

**Poor Use Cases:**
- Repeating the step label
- Generic text like "Step 1"
- Obvious information

### Practice 3: Duration Guidelines

| Content Type | Recommended Duration |
|--------------|---------------------|
| Short hint (1-3 words) | 2-3 seconds |
| Standard instruction | 5-7 seconds |
| Detailed info | 10-15 seconds |
| Critical information | 15-20 seconds |

### Practice 4: Accessibility

Always provide `ToolTipText` even when using custom templates for screen reader support:

```csharp
new StepProgressBarItem() 
{ 
    PrimaryText = "Cart",
    ToolTipText = "Add items to cart"  // Important for accessibility
}
```

### Practice 5: Consistent Styling

Maintain consistent tooltip appearance across your app:

```csharp
// Create a reusable style
public static StepProgressBarToolTipSettings GetStandardTooltipSettings()
{
    return new StepProgressBarToolTipSettings()
    {
        Background = Brush.FromRgba("#2196F3"),
        Stroke = Brush.FromRgba("#1976D2"),
        Duration = new TimeSpan(0, 0, 5),
        TextStyle = new StepTextStyle()
        {
            TextColor = Colors.White,
            FontSize = 13
        }
    };
}

// Use in multiple places
stepProgressBar.ToolTipSettings = GetStandardTooltipSettings();
```

### Edge Case: Empty ToolTipText

If `ShowToolTip = true` but a step has no `ToolTipText`:
- No tooltip is displayed for that step
- Other steps with `ToolTipText` show tooltips normally

This allows selective tooltip usage within the same StepProgressBar.

### Edge Case: Long Tooltip Text

For very long tooltip content:
- Text may wrap to multiple lines (platform-dependent)
- Consider using custom template with ScrollView for very long content
- Or link to a details page instead

```xml
<stepProgressBar:SfStepProgressBar.ToolTipTemplate>
    <DataTemplate>
        <Frame MaximumWidthRequest="300">
            <ScrollView MaximumHeightRequest="200">
                <Label Text="{Binding ToolTipText}" TextColor="White"/>
            </ScrollView>
        </Frame>
    </DataTemplate>
</stepProgressBar:SfStepProgressBar.ToolTipTemplate>
```