# Descriptions and Labels in StepProgressBar

## Table of Contents
- [Overview](#overview)
- [Text Labels](#text-labels)
- [Formatted Text](#formatted-text)
- [Label Spacing](#label-spacing)
- [Label Position](#label-position)
- [Image Customization](#image-customization)
- [Font Icons](#font-icons)
- [Complete Examples](#complete-examples)
- [Edge Cases and Tips](#edge-cases-and-tips)

## Overview

The StepProgressBar provides rich text description capabilities for each step. You can add primary and secondary text labels, apply formatted text styling, control label positioning, and even use images or font icons as step content. This guide covers all text and label customization options.

## Text Labels

Each step can display two text labels:
- **PrimaryText**: Main label (typically displayed on right/bottom of the step)
- **SecondaryText**: Secondary label (typically displayed on left/top of the step)

### Basic Text Usage

**XAML:**
```xml
<stepProgressBar:SfStepProgressBar
    x:Name="stepProgress"
    VerticalOptions="Center"
    HorizontalOptions="Center"
    Orientation="Vertical"
    LabelSpacing="12"
    ActiveStepIndex="3"
    ActiveStepProgressValue="50"
    ItemsSource="{Binding StepProgressItem}">
</stepProgressBar:SfStepProgressBar>

<ContentPage.BindingContext>
    <local:ViewModel />
</ContentPage.BindingContext>
```

**ViewModel:**
```csharp
public class ViewModel
{
    public ObservableCollection<StepProgressBarItem> StepProgressItem { get; set; }
    
    public ViewModel()
    {
        StepProgressItem = new ObservableCollection<StepProgressBarItem>();
        
        StepProgressItem.Add(new StepProgressBarItem() 
        { 
            PrimaryText = "Requirement Gathering", 
            SecondaryText = "Jan 10, 2024" 
        });
        
        StepProgressItem.Add(new StepProgressBarItem() 
        { 
            PrimaryText = "Design Phase", 
            SecondaryText = "Jan 20, 2024" 
        });
        
        StepProgressItem.Add(new StepProgressBarItem() 
        { 
            PrimaryText = "Development", 
            SecondaryText = "Feb 5, 2024" 
        });
        
        StepProgressItem.Add(new StepProgressBarItem() 
        { 
            PrimaryText = "Testing", 
            SecondaryText = "Feb 20, 2024" 
        });
        
        StepProgressItem.Add(new StepProgressBarItem() 
        { 
            PrimaryText = "Deployment", 
            SecondaryText = "Mar 5, 2024" 
        });
    }
}
```

**Use Case:** Order tracking, project timelines, registration steps with dates/status information.

## Formatted Text

For advanced text styling, use `PrimaryFormattedText` and `SecondaryFormattedText` properties. These allow multiple styles within a single label.

### Creating Formatted Text

**ViewModel with FormattedString:**
```csharp
public class ViewModel
{
    public ObservableCollection<StepProgressBarItem> StepProgressItem { get; set; }
    
    public ViewModel()
    {
        // Step 1: Introduction with formatted text
        FormattedString primaryFormattedText1 = new FormattedString();
        primaryFormattedText1.Spans.Add(new Span 
        { 
            Text = "Step 1: Introduction", 
            FontSize = 14, 
            FontAttributes = FontAttributes.Bold 
        });
        primaryFormattedText1.Spans.Add(new Span 
        { 
            Text = "\nWelcome to our", 
            FontSize = 12, 
            TextColor = Colors.Gray 
        });
        primaryFormattedText1.Spans.Add(new Span 
        { 
            Text = "\nplatform!", 
            FontSize = 12, 
            TextColor = Colors.Gray 
        });
        
        // Step 2: Registration with formatted text
        FormattedString primaryFormattedText2 = new FormattedString();
        primaryFormattedText2.Spans.Add(new Span 
        { 
            Text = "Step 2: Registration", 
            FontSize = 14, 
            FontAttributes = FontAttributes.Bold 
        });
        primaryFormattedText2.Spans.Add(new Span 
        { 
            Text = "\nCreate your account", 
            FontSize = 12, 
            TextColor = Colors.Gray 
        });
        primaryFormattedText2.Spans.Add(new Span 
        { 
            Text = "\nto access exclusive features", 
            FontSize = 12, 
            TextColor = Colors.Gray 
        });
        
        // Step 3: Profile Setup
        FormattedString primaryFormattedText3 = new FormattedString();
        primaryFormattedText3.Spans.Add(new Span 
        { 
            Text = "Step 3: Profile Setup", 
            FontSize = 14, 
            FontAttributes = FontAttributes.Bold 
        });
        primaryFormattedText3.Spans.Add(new Span 
        { 
            Text = "\nComplete your profile", 
            FontSize = 12, 
            TextColor = Colors.Gray 
        });
        primaryFormattedText3.Spans.Add(new Span 
        { 
            Text = "\nto personalize your experience", 
            FontSize = 12, 
            TextColor = Colors.Gray 
        });
        
        // Secondary formatted text
        FormattedString secondaryFormattedText1 = new FormattedString();
        secondaryFormattedText1.Spans.Add(new Span 
        { 
            Text = "Join us and explore!", 
            FontSize = 12, 
            TextColor = Colors.Gray 
        });
        
        FormattedString secondaryFormattedText2 = new FormattedString();
        secondaryFormattedText2.Spans.Add(new Span 
        { 
            Text = "Unlock all features", 
            FontSize = 12, 
            TextColor = Colors.Gray 
        });
        
        FormattedString secondaryFormattedText3 = new FormattedString();
        secondaryFormattedText3.Spans.Add(new Span 
        { 
            Text = "Personalize your journey", 
            FontSize = 12, 
            TextColor = Colors.Gray 
        });
        
        // Add to collection
        StepProgressItem = new ObservableCollection<StepProgressBarItem>();
        StepProgressItem.Add(new StepProgressBarItem() 
        { 
            PrimaryFormattedText = primaryFormattedText1, 
            SecondaryFormattedText = secondaryFormattedText1 
        });
        StepProgressItem.Add(new StepProgressBarItem() 
        { 
            PrimaryFormattedText = primaryFormattedText2, 
            SecondaryFormattedText = secondaryFormattedText2 
        });
        StepProgressItem.Add(new StepProgressBarItem() 
        { 
            PrimaryFormattedText = primaryFormattedText3, 
            SecondaryFormattedText = secondaryFormattedText3 
        });
    }
}
```

**XAML:**
```xml
<stepProgressBar:SfStepProgressBar
    x:Name="stepProgress"
    VerticalOptions="Center"
    HorizontalOptions="Center"
    Orientation="Vertical"
    LabelSpacing="12"
    ActiveStepIndex="1"
    ActiveStepProgressValue="50"
    ItemsSource="{Binding StepProgressItem}">
</stepProgressBar:SfStepProgressBar>
```

**Precedence Rule:** If both `PrimaryText` and `PrimaryFormattedText` are provided, `PrimaryFormattedText` takes precedence. Same applies for `SecondaryText` vs `SecondaryFormattedText`.

## Label Spacing

Control the space between the step indicator and its text labels using the `LabelSpacing` property.

**Default:** 5 pixels

**XAML Example:**
```xml
<stepProgressBar:SfStepProgressBar
    LabelSpacing="28"
    ItemsSource="{Binding StepProgressItem}">
</stepProgressBar:SfStepProgressBar>
```

**C# Example:**
```csharp
SfStepProgressBar stepProgressBar = new SfStepProgressBar()
{
    LabelSpacing = 28,
    ItemsSource = viewModel.StepProgressItem
};
```

**Recommendations:**
- **Compact layout:** LabelSpacing = 8-12
- **Standard layout:** LabelSpacing = 12-20 (default range)
- **Spacious layout:** LabelSpacing = 20-30
- **Avoid:** LabelSpacing > 40 (too much whitespace)

## Label Position

Control where labels appear relative to the step indicator using the `LabelPosition` property.

### Options

- **Start**: Labels before the step (left in horizontal, top in vertical)
- **End**: Labels after the step (right in horizontal, bottom in vertical)
- **Top**: Labels above the step (both orientations)
- **Bottom**: Labels below the step (both orientations)

### Default Values

- **Horizontal orientation:** `Bottom`
- **Vertical orientation:** `End`

### Horizontal Orientation Examples

**Bottom Position (Default):**
```xml
<stepProgressBar:SfStepProgressBar
    Orientation="Horizontal"
    LabelPosition="Bottom"
    ItemsSource="{Binding StepProgressItem}">
</stepProgressBar:SfStepProgressBar>
```

**Top Position:**
```xml
<stepProgressBar:SfStepProgressBar
    Orientation="Horizontal"
    LabelPosition="Top"
    ItemsSource="{Binding StepProgressItem}">
</stepProgressBar:SfStepProgressBar>
```

**Start Position:**
```xml
<stepProgressBar:SfStepProgressBar
    Orientation="Horizontal"
    LabelPosition="Start"
    ItemsSource="{Binding StepProgressItem}">
</stepProgressBar:SfStepProgressBar>
```

**End Position:**
```xml
<stepProgressBar:SfStepProgressBar
    Orientation="Horizontal"
    LabelPosition="End"
    ItemsSource="{Binding StepProgressItem}">
</stepProgressBar:SfStepProgressBar>
```

### Vertical Orientation Examples

**End Position (Default):**
```xml
<stepProgressBar:SfStepProgressBar
    Orientation="Vertical"
    LabelPosition="End"
    ItemsSource="{Binding StepProgressItem}">
</stepProgressBar:SfStepProgressBar>
```

**C# Example:**
```csharp
SfStepProgressBar stepProgressBar = new SfStepProgressBar()
{
    Orientation = StepProgressBarOrientation.Vertical,
    LabelPosition = LabelPosition.End,
    ItemsSource = viewModel.StepProgressItem
};
```

**Important:** `LabelPosition` is only effective when `PrimaryText`, `SecondaryText`, `PrimaryFormattedText`, or `SecondaryFormattedText` is provided.

## Image Customization

Add images to step indicators using the `ImageSource` property. This replaces the default content (numbers, dots, ticks).

### Using Image Files

**ViewModel with Image Paths:**
```csharp
public class ViewModel
{
    public ObservableCollection<StepProgressBarItem> StepProgressItem { get; set; }
    
    public ViewModel()
    {
        StepProgressItem = new ObservableCollection<StepProgressBarItem>();
        
        StepProgressItem.Add(new StepProgressBarItem() 
        { 
            ImageSource = "ordered.png",
            PrimaryText = "Ordered"
        });
        
        StepProgressItem.Add(new StepProgressBarItem() 
        { 
            ImageSource = "transport.png",
            PrimaryText = "In Transit"
        });
        
        StepProgressItem.Add(new StepProgressBarItem() 
        { 
            ImageSource = "payment.png",
            PrimaryText = "Payment Confirmed"
        });
        
        StepProgressItem.Add(new StepProgressBarItem() 
        { 
            ImageSource = "delivered.png",
            PrimaryText = "Delivered"
        });
    }
}
```

**XAML:**
```xml
<stepProgressBar:SfStepProgressBar
    x:Name="stepProgress"
    VerticalOptions="Center"
    HorizontalOptions="Center"
    Orientation="Horizontal"
    LabelSpacing="12"
    LabelPosition="Bottom"
    ActiveStepIndex="2"
    ActiveStepProgressValue="50"
    ItemsSource="{Binding StepProgressItem}">
</stepProgressBar:SfStepProgressBar>
```

**Image Requirements:**
- Place images in `Resources/Images` folder
- Recommended size: 32x32 to 64x64 pixels
- Supported formats: PNG, JPG, SVG
- Use transparent backgrounds for best results

## Font Icons

Use font icons (like Material Icons, Font Awesome) instead of image files for scalable, customizable step content.

### Using Font Icons

**ViewModel with FontImageSource:**
```csharp
public class ViewModel
{
    public ObservableCollection<StepProgressBarItem> StepProgressItem { get; set; }
    
    public ViewModel()
    {
        StepProgressItem = new ObservableCollection<StepProgressBarItem>();
        
        StepProgressItem.Add(new StepProgressBarItem() 
        { 
            ImageSource = new FontImageSource() 
            { 
                Glyph = "\ue760",  // Cart icon
                Size = 32, 
                FontFamily = "IconFont", 
                Color = Colors.White 
            },
            PrimaryText = "Cart"
        });
        
        StepProgressItem.Add(new StepProgressBarItem() 
        { 
            ImageSource = new FontImageSource() 
            { 
                Glyph = "\ue77f",  // Location icon
                Size = 32, 
                FontFamily = "IconFont", 
                Color = Colors.White 
            },
            PrimaryText = "Address"
        });
        
        StepProgressItem.Add(new StepProgressBarItem() 
        { 
            ImageSource = new FontImageSource() 
            { 
                Glyph = "\ue786",  // Payment icon
                Size = 32, 
                FontFamily = "IconFont", 
                Color = Colors.White 
            },
            PrimaryText = "Payment"
        });
    }
}
```

**Setup Font Family:**

1. Add your icon font file (e.g., `MaterialIcons.ttf`) to `Resources/Fonts`
2. Register in `MauiProgram.cs`:

```csharp
builder
    .UseMauiApp<App>()
    .ConfigureFonts(fonts =>
    {
        fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
        fonts.AddFont("MaterialIcons.ttf", "IconFont");
    });
```

**Benefits of Font Icons:**
- Scalable without quality loss
- Easy color customization
- Smaller file size than images
- Consistent styling across platforms

## Complete Examples

### Example 1: Order Tracking with Dates

```csharp
public class OrderTrackingViewModel
{
    public ObservableCollection<StepProgressBarItem> Steps { get; set; }
    
    public OrderTrackingViewModel()
    {
        Steps = new ObservableCollection<StepProgressBarItem>
        {
            new StepProgressBarItem() 
            { 
                PrimaryText = "Order Placed",
                SecondaryText = "Mar 15, 10:30 AM"
            },
            new StepProgressBarItem() 
            { 
                PrimaryText = "Confirmed",
                SecondaryText = "Mar 15, 11:15 AM"
            },
            new StepProgressBarItem() 
            { 
                PrimaryText = "Shipped",
                SecondaryText = "Mar 16, 9:00 AM"
            },
            new StepProgressBarItem() 
            { 
                PrimaryText = "Out for Delivery",
                SecondaryText = "Mar 17, 7:30 AM"
            },
            new StepProgressBarItem() 
            { 
                PrimaryText = "Delivered",
                SecondaryText = "Expected Today"
            }
        };
    }
}
```

### Example 2: Multi-Stage Registration

```csharp
public class RegistrationViewModel
{
    public ObservableCollection<StepProgressBarItem> RegistrationSteps { get; set; }
    
    public RegistrationViewModel()
    {
        RegistrationSteps = new ObservableCollection<StepProgressBarItem>
        {
            new StepProgressBarItem() 
            { 
                PrimaryText = "Account",
                SecondaryText = "Email & Password"
            },
            new StepProgressBarItem() 
            { 
                PrimaryText = "Profile",
                SecondaryText = "Personal Details"
            },
            new StepProgressBarItem() 
            { 
                PrimaryText = "Verification",
                SecondaryText = "Confirm Email"
            },
            new StepProgressBarItem() 
            { 
                PrimaryText = "Complete",
                SecondaryText = "All Set!"
            }
        };
    }
}
```

## Edge Cases and Tips

### Edge Case 1: Long Text Truncation

**Problem:** Long PrimaryText or SecondaryText gets truncated.

**Solution:** Use shorter text or enable text wrapping in custom templates. Consider using SecondaryText for additional details.

```csharp
// Instead of:
PrimaryText = "This is a very long step description that might get cut off"

// Use:
PrimaryText = "Long Step"
SecondaryText = "Additional details here"
```

### Edge Case 2: Both Text and FormattedText Provided

**Behavior:** If both `PrimaryText` and `PrimaryFormattedText` are set, only `PrimaryFormattedText` is displayed.

**Best Practice:** Choose one approach per step:
```csharp
// Option 1: Simple text
new StepProgressBarItem() { PrimaryText = "Step 1" }

// Option 2: Formatted text
new StepProgressBarItem() { PrimaryFormattedText = formattedString }

// Don't mix both for the same property
```

### Edge Case 3: Empty Labels

**Problem:** Steps with no text labels look disconnected.

**Solution:** Always provide at least PrimaryText for clarity. SecondaryText is optional.

```csharp
// Minimum recommended
new StepProgressBarItem() { PrimaryText = "Step Name" }
```

### Edge Case 4: Images Not Displaying

**Troubleshooting:**
1. Verify image path is correct
2. Check image is in `Resources/Images` folder
3. Ensure image build action is `MauiImage`
4. For font icons, verify font family is registered in MauiProgram.cs

### Tip: Dynamic Label Updates

Update labels dynamically by modifying the collection:

```csharp
// Update a specific step's text
StepProgressItem[2].PrimaryText = "Updated Step";
StepProgressItem[2].SecondaryText = "New timestamp";

// Notify UI of changes (if using MVVM with INotifyPropertyChanged)
OnPropertyChanged(nameof(StepProgressItem));
```

### Tip: Localization Support

For multi-language apps, use resource files:

```csharp
StepProgressItem.Add(new StepProgressBarItem() 
{ 
    PrimaryText = AppResources.Step1_Title,
    SecondaryText = AppResources.Step1_Description
});
```

### Tip: Accessibility

Always provide PrimaryText or SecondaryText for screen reader support, even when using images:

```csharp
new StepProgressBarItem() 
{ 
    ImageSource = "cart.png",
    PrimaryText = "Shopping Cart"  // Important for accessibility
}
```