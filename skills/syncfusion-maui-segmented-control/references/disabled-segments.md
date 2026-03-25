# Disabled Segments

This guide explains how to disable specific segments in the Segmented Control to prevent user interaction.

## Overview

Disabling segments is useful when certain options are:
- Temporarily unavailable
- Conditionally restricted based on user permissions
- Not applicable in the current context
- Pending prerequisite actions

**Key features:**
- Per-segment disabling via `IsEnabled` property
- Automatic visual feedback (grayed out appearance)
- Prevents selection and tap events
- Can be toggled dynamically at runtime

## Disabling Segments

### Setting IsEnabled on SfSegmentItem

Use the `IsEnabled` property to disable individual segments.

**C#:**
```csharp
var segmentedControl = new SfSegmentedControl
{
    ItemsSource = new List<SfSegmentItem>
    {
        new SfSegmentItem { Text = "Day", IsEnabled = true },      // Enabled
        new SfSegmentItem { Text = "Week", IsEnabled = true },     // Enabled
        new SfSegmentItem { Text = "Month", IsEnabled = false },   // Disabled
        new SfSegmentItem { Text = "Year", IsEnabled = false }     // Disabled
    }
};
```

### Default Behavior

- **Default value:** `IsEnabled = true` (all segments enabled)
- Disabled segments appear grayed out automatically
- Tapping a disabled segment has no effect
- SelectionChanged event does not fire for disabled segments

## Visual Feedback

### Default Disabled Appearance

When `IsEnabled = false`:
- Text color becomes lighter (gray)
- Background may appear muted
- Touch/hover effects disabled
- Selection indicator cannot be applied

### Platform-Specific Appearance

Disabled appearance may vary slightly by platform:
- **Android:** Gray text, reduced opacity
- **iOS:** Light gray text, no interaction feedback
- **Windows:** Gray text, system disabled style

**Note:** Exact appearance depends on platform theme and system settings.

## Dynamic Enabling/Disabling

Enable or disable segments at runtime based on conditions.

### Toggle Based on User Action

```csharp
public partial class MainPage : ContentPage
{
    private SfSegmentedControl segmentedControl;
    private List<SfSegmentItem> segments;

    public MainPage()
    {
        InitializeComponent();
        
        segments = new List<SfSegmentItem>
        {
            new SfSegmentItem { Text = "Day" },
            new SfSegmentItem { Text = "Week" },
            new SfSegmentItem { Text = "Month" },
            new SfSegmentItem { Text = "Year" }
        };

        segmentedControl = new SfSegmentedControl
        {
            ItemsSource = segments
        };

        Content = segmentedControl;
    }

    private void EnableAdvancedOptions()
    {
        // Enable previously disabled segments
        segments[2].IsEnabled = true;  // Enable "Month"
        segments[3].IsEnabled = true;  // Enable "Year"
    }

    private void DisableAdvancedOptions()
    {
        // Disable segments for basic users
        segments[2].IsEnabled = false;  // Disable "Month"
        segments[3].IsEnabled = false;  // Disable "Year"
    }
}
```

### Conditional Disabling Based on Permissions

```csharp
public void ConfigureSegmentsByUserRole(UserRole role)
{
    var segments = new List<SfSegmentItem>
    {
        new SfSegmentItem { Text = "Day", IsEnabled = true },
        new SfSegmentItem { Text = "Week", IsEnabled = true },
        new SfSegmentItem { Text = "Month", IsEnabled = role >= UserRole.Premium },
        new SfSegmentItem { Text = "Year", IsEnabled = role == UserRole.Admin }
    };

    segmentedControl.ItemsSource = segments;
}
```

### Disabling Based on Data Availability

```csharp
public void UpdateSegmentsBasedOnData(List<string> availablePeriods)
{
    var allPeriods = new[] { "Day", "Week", "Month", "Year" };
    
    var segments = allPeriods.Select(period => new SfSegmentItem
    {
        Text = period,
        IsEnabled = availablePeriods.Contains(period)
    }).ToList();

    segmentedControl.ItemsSource = segments;
}
```

## Common Use Cases

### Feature Availability

Disable segments for features not yet available or under development.

```csharp
var viewModes = new List<SfSegmentItem>
{
    new SfSegmentItem { Text = "List", IsEnabled = true },
    new SfSegmentItem { Text = "Grid", IsEnabled = true },
    new SfSegmentItem { Text = "Map", IsEnabled = false }  // Coming soon
};
```

### Subscription Tiers

Restrict options based on user subscription level.

```csharp
public void SetupReportPeriods(bool isPremiumUser)
{
    var periods = new List<SfSegmentItem>
    {
        new SfSegmentItem { Text = "Today", IsEnabled = true },
        new SfSegmentItem { Text = "Week", IsEnabled = true },
        new SfSegmentItem { Text = "Month", IsEnabled = isPremiumUser },
        new SfSegmentItem { Text = "Year", IsEnabled = isPremiumUser },
        new SfSegmentItem { Text = "Custom", IsEnabled = isPremiumUser }
    };

    reportPeriodControl.ItemsSource = periods;
}
```

### Data Availability

Disable options when underlying data is unavailable.

```csharp
public void UpdateChartPeriods(DateTime earliestDataDate)
{
    var now = DateTime.Now;
    
    var periods = new List<SfSegmentItem>
    {
        new SfSegmentItem 
        { 
            Text = "1D", 
            IsEnabled = earliestDataDate <= now.AddDays(-1) 
        },
        new SfSegmentItem 
        { 
            Text = "1W", 
            IsEnabled = earliestDataDate <= now.AddDays(-7) 
        },
        new SfSegmentItem 
        { 
            Text = "1M", 
            IsEnabled = earliestDataDate <= now.AddMonths(-1) 
        },
        new SfSegmentItem 
        { 
            Text = "1Y", 
            IsEnabled = earliestDataDate <= now.AddYears(-1) 
        }
    };

    chartPeriodControl.ItemsSource = periods;
}
```

### Workflow Steps

Disable future steps until current step is completed.

```csharp
public class WorkflowStepManager
{
    private List<SfSegmentItem> steps;
    private int currentStepIndex = 0;

    public void InitializeWorkflow()
    {
        steps = new List<SfSegmentItem>
        {
            new SfSegmentItem { Text = "Setup", IsEnabled = true },
            new SfSegmentItem { Text = "Configure", IsEnabled = false },
            new SfSegmentItem { Text = "Review", IsEnabled = false },
            new SfSegmentItem { Text = "Complete", IsEnabled = false }
        };

        workflowControl.ItemsSource = steps;
    }

    public void CompleteCurrentStep()
    {
        if (currentStepIndex < steps.Count - 1)
        {
            currentStepIndex++;
            steps[currentStepIndex].IsEnabled = true;
            workflowControl.SelectedIndex = currentStepIndex;
        }
    }
}
```

## Handling Disabled Segment Interactions

### Prevent Selection of Disabled Segments

Disabled segments cannot be selected programmatically or by user interaction:

```csharp
// This will not work if segment at index 2 is disabled
segmentedControl.SelectedIndex = 2;  // Ignored if disabled

// Check before setting selection
if (segments[2].IsEnabled)
{
    segmentedControl.SelectedIndex = 2;
}
```

### Show Explanation on Tap

Provide feedback when users tap disabled segments (requires custom implementation):

```csharp
// Using TapGestureRecognizer on custom template
segmentedControl.SegmentTapped += (sender, e) =>
{
    var segmentItem = e.SegmentItem;
    
    if (!segmentItem.IsEnabled)
    {
        // Show reason for disabled state
        DisplayAlert("Feature Unavailable", 
                    "Upgrade to Premium to access this feature.", 
                    "OK");
    }
};
```

## Best Practices

### Visual Clarity

- Ensure disabled segments are clearly distinguishable from enabled ones
- Use sufficient contrast (disabled should be obviously muted)
- Consider adding icons (lock icon for premium features)

### User Communication

- Provide tooltips or messages explaining why segments are disabled
- Offer upgrade paths for permission-based restrictions
- Show progress indicators for time-based restrictions

### Accessibility

- Screen readers should announce disabled state
- Ensure disabled segments are not in the tab order
- Provide alternative text explaining unavailability

### Performance

- Batch updates when enabling/disabling multiple segments
- Avoid frequent toggling that causes visual flickering

## Troubleshooting

### Disabled Segment Still Selectable

**Cause:** IsEnabled property not set or reverted  
**Solution:** Verify `IsEnabled = false` is set on SfSegmentItem, not control

### Visual State Not Updating

**Cause:** Platform rendering delay or cache  
**Solution:** Force refresh by recreating ItemsSource:
```csharp
var items = segmentedControl.ItemsSource;
segmentedControl.ItemsSource = null;
segmentedControl.ItemsSource = items;
```

### Cannot Select Any Segment

**Cause:** All segments disabled or control itself disabled  
**Solution:** Ensure at least one segment has `IsEnabled = true`

### IsEnabled Property Not Available

**Cause:** Using string array instead of SfSegmentItem objects  
**Solution:** Use `List<SfSegmentItem>` to access IsEnabled property

## Next Steps

- **Handle events:** See [events.md](events.md) for responding to segment interactions
- **Customize appearance:** See [customization.md](customization.md) for styling disabled segments
- **Selection features:** See [selection.md](selection.md) for selection modes
