# Right-to-Left (RTL) Support in StepProgressBar

## Overview

The StepProgressBar fully supports Right-to-Left (RTL) languages such as Arabic, Hebrew, Persian, and Urdu. When RTL mode is enabled, the control automatically mirrors its layout to match the reading direction of these languages.

## Enabling RTL Mode

Set the `FlowDirection` property to `RightToLeft` to enable RTL layout.

### XAML Implementation

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:stepProgressBar="clr-namespace:Syncfusion.Maui.ProgressBar;assembly=Syncfusion.Maui.ProgressBar"
             x:Class="YourNamespace.MainPage">
    
    <stepProgressBar:SfStepProgressBar 
        FlowDirection="RightToLeft"
        ItemsSource="{Binding StepProgressItem}"/>
        
</ContentPage>
```

### C# Implementation

```csharp
using Syncfusion.Maui.ProgressBar;

public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        
        SfStepProgressBar stepProgressBar = new SfStepProgressBar()
        {
            FlowDirection = FlowDirection.RightToLeft,
            ItemsSource = viewModel.StepProgressItem
        };
        
        this.Content = stepProgressBar;
    }
}
```

## RTL Behavior

### Horizontal Orientation

In horizontal orientation with RTL enabled:
- Steps flow from **right to left**
- First step appears on the **right**
- Last step appears on the **left**
- Progress moves from right → left

**LTR (Default):**
```
[1] → [2] → [3] → [4] → [5]
```

**RTL:**
```
[5] ← [4] ← [3] ← [2] ← [1]
```

### Vertical Orientation

Vertical orientation is **not affected** by RTL. Steps always flow from top to bottom.

**Both LTR and RTL:**
```
[1]
 ↓
[2]
 ↓
[3]
 ↓
[4]
 ↓
[5]
```

However, label positioning within each step respects RTL:
- Text alignment changes (right-aligned in RTL)
- SecondaryText and PrimaryText positions may swap

## Complete RTL Example

### ViewModel with Arabic Text

```csharp
using System.Collections.ObjectModel;
using Syncfusion.Maui.ProgressBar;

public class RTLViewModel
{
    public ObservableCollection<StepProgressBarItem> StepProgressItem { get; set; }
    
    public RTLViewModel()
    {
        StepProgressItem = new ObservableCollection<StepProgressBarItem>();
        
        // Arabic text examples
        StepProgressItem.Add(new StepProgressBarItem() 
        { 
            PrimaryText = "السلة",          // Cart
            SecondaryText = "أضف العناصر"   // Add items
        });
        
        StepProgressItem.Add(new StepProgressBarItem() 
        { 
            PrimaryText = "العنوان",        // Address
            SecondaryText = "عنوان التسليم"  // Delivery address
        });
        
        StepProgressItem.Add(new StepProgressBarItem() 
        { 
            PrimaryText = "الدفع",          // Payment
            SecondaryText = "طريقة الدفع"    // Payment method
        });
        
        StepProgressItem.Add(new StepProgressBarItem() 
        { 
            PrimaryText = "التأكيد",        // Confirmation
            SecondaryText = "مراجعة الطلب"   // Review order
        });
    }
}
```

### XAML Page

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:stepProgressBar="clr-namespace:Syncfusion.Maui.ProgressBar;assembly=Syncfusion.Maui.ProgressBar"
             xmlns:local="clr-namespace:YourNamespace"
             x:Class="YourNamespace.RTLPage"
             FlowDirection="RightToLeft">
    
    <ContentPage.BindingContext>
        <local:RTLViewModel />
    </ContentPage.BindingContext>
    
    <Grid Padding="20">
        <stepProgressBar:SfStepProgressBar 
            x:Name="stepProgress"
            VerticalOptions="Center"
            HorizontalOptions="Center"
            Orientation="Horizontal"
            LabelPosition="Bottom"
            ActiveStepIndex="1"
            ActiveStepProgressValue="50"
            FlowDirection="RightToLeft"
            ItemsSource="{Binding StepProgressItem}"/>
    </Grid>
    
</ContentPage>
```

## Dynamic RTL Based on Culture

Automatically set RTL based on the current culture:

```csharp
public partial class AdaptiveRTLPage : ContentPage
{
    private SfStepProgressBar stepProgressBar;
    
    public AdaptiveRTLPage()
    {
        InitializeComponent();
        
        stepProgressBar = new SfStepProgressBar()
        {
            ItemsSource = viewModel.StepProgressItem,
            Orientation = StepProgressBarOrientation.Horizontal
        };
        
        // Set FlowDirection based on current culture
        SetFlowDirectionFromCulture();
        
        this.Content = stepProgressBar;
    }
    
    private void SetFlowDirectionFromCulture()
    {
        var currentCulture = System.Globalization.CultureInfo.CurrentUICulture;
        
        // List of RTL language codes
        string[] rtlLanguages = { "ar", "he", "fa", "ur", "yi" };
        
        bool isRTL = rtlLanguages.Any(lang => 
            currentCulture.TwoLetterISOLanguageName.Equals(lang, 
            StringComparison.OrdinalIgnoreCase));
        
        stepProgressBar.FlowDirection = isRTL ? FlowDirection.RightToLeft 
                                              : FlowDirection.LeftToRight;
        
        this.FlowDirection = isRTL ? FlowDirection.RightToLeft 
                                   : FlowDirection.LeftToRight;
    }
}
```

## RTL Language Support

Common RTL languages:

| Language | Code | Example Text |
|----------|------|--------------|
| **Arabic** | ar | العربية |
| **Hebrew** | he | עברית |
| **Persian (Farsi)** | fa | فارسی |
| **Urdu** | ur | اردو |
| **Yiddish** | yi | ייִדיש |
| **Pashto** | ps | پښتو |
| **Kurdish** | ku | کوردی |

## Best Practices

### Practice 1: Set FlowDirection at Page Level

Set RTL on the entire page for consistency:

```xml
<ContentPage xmlns="..."
             FlowDirection="RightToLeft">
    
    <stepProgressBar:SfStepProgressBar 
        FlowDirection="RightToLeft"
        ItemsSource="{Binding StepProgressItem}"/>
    
    <!-- Other controls automatically inherit RTL -->
    
</ContentPage>
```

### Practice 2: Test with Native Text

Always test with actual Arabic, Hebrew, or Persian text, not transliterated English:

**Good:**
```csharp
PrimaryText = "السلة"  // Actual Arabic
```

**Bad:**
```csharp
PrimaryText = "Alsalat"  // Transliteration - doesn't test RTL properly
```

### Practice 3: Verify Icon Directionality

Some icons need mirroring in RTL, others don't:

**Should mirror:**
- Arrows (→ becomes ←)
- "Next" buttons
- Navigation icons

**Should NOT mirror:**
- Checkmarks ✓
- Warning symbols ⚠
- Numbers and clocks

### Practice 4: Use Localization Resources

Store RTL text in resource files:

```csharp
// Resources/AppResources.resx
StepProgressItem.Add(new StepProgressBarItem() 
{ 
    PrimaryText = AppResources.Step1_Cart,
    SecondaryText = AppResources.Step1_Description
});
```

### Practice 5: Consider Both Orientations

Test both horizontal and vertical orientations with RTL:

```csharp
// Horizontal: Progress flows right-to-left
stepProgressBar.Orientation = StepProgressBarOrientation.Horizontal;
stepProgressBar.FlowDirection = FlowDirection.RightToLeft;

// Vertical: Steps flow top-to-bottom, but labels align right
stepProgressBar.Orientation = StepProgressBarOrientation.Vertical;
stepProgressBar.FlowDirection = FlowDirection.RightToLeft;
```

## Common Issues and Solutions

### Issue 1: Mixed LTR/RTL Content

**Problem:** English numbers or words mixed with Arabic text.

**Solution:** Use Unicode bidirectional markers:

```csharp
// Use \u200E (LRM - Left-to-Right Mark) for embedded LTR text
PrimaryText = "الخطوة \u200E123\u200E"  // "Step 123" in Arabic with number
```

### Issue 2: Icons Not Mirroring

**Problem:** Directional icons don't mirror in RTL.

**Solution:** Use platform-specific icon variants or flip programmatically:

```csharp
string iconPath = stepProgressBar.FlowDirection == FlowDirection.RightToLeft
    ? "arrow_left.png"
    : "arrow_right.png";
```

### Issue 3: Custom Templates Not Respecting RTL

**Problem:** Custom DataTemplates don't automatically mirror.

**Solution:** Set FlowDirection on template root:

```xml
<stepProgressBar:SfStepProgressBar.StepTemplate>
    <DataTemplate>
        <Grid FlowDirection="{Binding FlowDirection, Source={RelativeSource AncestorType={x:Type stepProgressBar:SfStepProgressBar}}}">
            <Image Source="icon.png"/>
            <!-- Content -->
        </Grid>
    </DataTemplate>
</stepProgressBar:SfStepProgressBar.StepTemplate>
```

## Testing RTL

### Visual Testing Checklist

- [ ] Steps flow right-to-left (horizontal)
- [ ] First step is on the right
- [ ] Progress bar fills from right to left
- [ ] Text is right-aligned
- [ ] Labels are in correct positions
- [ ] Tooltips appear in appropriate locations
- [ ] No layout breaking or overflow
- [ ] Icons (if any) mirror appropriately

### Device Testing

Test on actual devices with RTL system locale:

**iOS:**
Settings → General → Language & Region → iPhone Language → العربية

**Android:**
Settings → System → Languages & input → Languages → Add Arabic

### Automated Testing

```csharp
[Test]
public void TestRTLLayout()
{
    var stepProgressBar = new SfStepProgressBar
    {
        FlowDirection = FlowDirection.RightToLeft,
        ItemsSource = GetTestData()
    };
    
    Assert.AreEqual(FlowDirection.RightToLeft, stepProgressBar.FlowDirection);
    // Additional assertions for RTL behavior
}
```

## Resources

- [Unicode Bidirectional Algorithm](http://unicode.org/reports/tr9/)
- [.NET MAUI Localization](https://learn.microsoft.com/dotnet/maui/fundamentals/localization)
- [Material Design RTL Guidelines](https://material.io/design/usability/bidirectionality.html)
- [iOS RTL Support](https://developer.apple.com/library/archive/documentation/MacOSX/Conceptual/BPInternational/SupportingRight-To-LeftLanguages/SupportingRight-To-LeftLanguages.html)