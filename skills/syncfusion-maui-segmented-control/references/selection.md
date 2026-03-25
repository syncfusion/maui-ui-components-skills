# Selection Features

## Table of Contents
- [Overview](#overview)
- [Programmatic Selection](#programmatic-selection)
- [Selection Indicator Placements](#selection-indicator-placements)
- [Selection Modes](#selection-modes)
- [Customizing Selected Segment Appearance](#customizing-selected-segment-appearance)
- [Ripple Effect Animation](#ripple-effect-animation)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

The Segmented Control provides comprehensive selection features to indicate the active segment and control user interaction behavior. This includes visual indicators, selection modes, and customization options for the selected state.

**Key Selection Features:**
- Programmatic selection via SelectedIndex
- Four indicator placement styles (Fill, Border, TopBorder, BottomBorder)
- Two selection modes (Single, SingleDeselect)
- Customizable colors, backgrounds, and borders for selected state
- Optional ripple effect animation
- Per-item selection styling

## Programmatic Selection

Set the default selected segment using the `SelectedIndex` property.

### Setting Initial Selection

**XAML:**
```xml
<buttons:SfSegmentedControl SelectedIndex="1">
    <buttons:SfSegmentedControl.ItemsSource>
        <x:Array Type="{x:Type x:String}">
            <x:String>Day</x:String>
            <x:String>Week</x:String>    <!-- Initially selected -->
            <x:String>Month</x:String>
            <x:String>Year</x:String>
        </x:Array>
    </buttons:SfSegmentedControl.ItemsSource>
</buttons:SfSegmentedControl>
```

**C#:**
```csharp
var segmentedControl = new SfSegmentedControl
{
    ItemsSource = new List<string> { "Day", "Week", "Month", "Year" },
    SelectedIndex = 1  // Select "Week" (zero-based index)
};
```

### Changing Selection Programmatically

```csharp
// Change to third segment
segmentedControl.SelectedIndex = 2;

// Get current selection
int currentIndex = segmentedControl.SelectedIndex;

// Clear selection (deselect all)
segmentedControl.SelectedIndex = -1;
```

**Index Range:** 
- Valid: `0` to `ItemsSource.Count - 1`
- No selection: `-1`
- Default: `0` (first segment)

## Selection Indicator Placements

The selection indicator visually highlights the selected segment. Choose from four placement styles.

### Fill (Background Fill)

The indicator fills the entire selected segment with a background color.

**XAML:**
```xml
<buttons:SfSegmentedControl>
    <buttons:SfSegmentedControl.SelectionIndicatorSettings>
        <buttons:SelectionIndicatorSettings 
            SelectionIndicatorPlacement="Fill"
            Background="#6200EE"/>
    </buttons:SfSegmentedControl.SelectionIndicatorSettings>
    <buttons:SfSegmentedControl.ItemsSource>
        <x:Array Type="{x:Type x:String}">
            <x:String>Day</x:String>
            <x:String>Week</x:String>
            <x:String>Month</x:String>
        </x:Array>
    </buttons:SfSegmentedControl.ItemsSource>
</buttons:SfSegmentedControl>
```

**C#:**
```csharp
segmentedControl.SelectionIndicatorSettings = new SelectionIndicatorSettings
{
    SelectionIndicatorPlacement = SelectionIndicatorPlacement.Fill,
    Background = Color.FromArgb("#6200EE")
};
```

**Use case:** Prominent selection with strong visual distinction, ideal for navigation or mode switchers.

### Border (Full Border)

Highlights the selected segment with a border around all edges.

**XAML:**
```xml
<buttons:SfSegmentedControl>
    <buttons:SfSegmentedControl.SelectionIndicatorSettings>
        <buttons:SelectionIndicatorSettings 
            SelectionIndicatorPlacement="Border"
            Stroke="#6200EE"
            StrokeThickness="2"/>
    </buttons:SfSegmentedControl.SelectionIndicatorSettings>
</buttons:SfSegmentedControl>
```

**C#:**
```csharp
segmentedControl.SelectionIndicatorSettings = new SelectionIndicatorSettings
{
    SelectionIndicatorPlacement = SelectionIndicatorPlacement.Border,
    Stroke = Color.FromArgb("#6200EE"),
    StrokeThickness = 2
};
```

**Use case:** Subtle selection indicator without obscuring segment content.

### TopBorder (Top Edge Only)

Places the indicator at the top edge of the selected segment.

**XAML:**
```xml
<buttons:SfSegmentedControl>
    <buttons:SfSegmentedControl.SelectionIndicatorSettings>
        <buttons:SelectionIndicatorSettings 
            SelectionIndicatorPlacement="TopBorder"
            Stroke="#6200EE"
            StrokeThickness="3"/>
    </buttons:SfSegmentedControl.SelectionIndicatorSettings>
</buttons:SfSegmentedControl>
```

**C#:**
```csharp
segmentedControl.SelectionIndicatorSettings = new SelectionIndicatorSettings
{
    SelectionIndicatorPlacement = SelectionIndicatorPlacement.TopBorder,
    Stroke = Color.FromArgb("#6200EE"),
    StrokeThickness = 3
};
```

**Use case:** Tab-like navigation where the indicator resembles a top highlight bar.

### BottomBorder (Bottom Edge Only)

Places the indicator at the bottom edge of the selected segment.

**XAML:**
```xml
<buttons:SfSegmentedControl>
    <buttons:SfSegmentedControl.SelectionIndicatorSettings>
        <buttons:SelectionIndicatorSettings 
            SelectionIndicatorPlacement="BottomBorder"
            Stroke="#6200EE"
            StrokeThickness="3"/>
    </buttons:SfSegmentedControl.SelectionIndicatorSettings>
</buttons:SfSegmentedControl>
```

**C#:**
```csharp
segmentedControl.SelectionIndicatorSettings = new SelectionIndicatorSettings
{
    SelectionIndicatorPlacement = SelectionIndicatorPlacement.BottomBorder,
    Stroke = Color.FromArgb("#6200EE"),
    StrokeThickness = 3
};
```

**Use case:** Material Design-style tabs with underline indicator.

## Selection Modes

Control how users can interact with segments and whether deselection is allowed.

### Single Selection (Default)

Allows selecting one segment at a time. Once a segment is selected, it remains selected until another segment is tapped.

**XAML:**
```xml
<buttons:SfSegmentedControl SelectionMode="Single">
    <buttons:SfSegmentedControl.ItemsSource>
        <x:Array Type="{x:Type x:String}">
            <x:String>Day</x:String>
            <x:String>Week</x:String>
            <x:String>Month</x:String>
        </x:Array>
    </buttons:SfSegmentedControl.ItemsSource>
</buttons:SfSegmentedControl>
```

**C#:**
```csharp
segmentedControl.SelectionMode = SegmentSelectionMode.Single;
```

**Behavior:**
- Tapping a segment selects it
- Tapping another segment switches selection
- Tapping the selected segment has no effect (remains selected)
- Always maintains one selected segment

**Use case:** Required choice scenarios like view modes or time periods.

### SingleDeselect

Allows deselecting the currently selected segment by tapping it again.

**XAML:**
```xml
<buttons:SfSegmentedControl SelectionMode="SingleDeselect">
    <buttons:SfSegmentedControl.ItemsSource>
        <x:Array Type="{x:Type x:String}">
            <x:String>Bold</x:String>
            <x:String>Italic</x:String>
            <x:String>Underline</x:String>
        </x:Array>
    </buttons:SfSegmentedControl.ItemsSource>
</buttons:SfSegmentedControl>
```

**C#:**
```csharp
segmentedControl.SelectionMode = SegmentSelectionMode.SingleDeselect;
```

**Behavior:**
- Tapping an unselected segment selects it
- Tapping the selected segment deselects it (SelectedIndex becomes -1)
- Can have zero selected segments

**Use case:** Optional filters or toggleable features where "none selected" is valid.

## Customizing Selected Segment Appearance

Customize the visual appearance of selected segments to match your app's design.

### Selected Background Color (Global)

Apply a background color to all selected segments.

**XAML:**
```xml
<buttons:SfSegmentedControl>
    <buttons:SfSegmentedControl.SelectionIndicatorSettings>
        <buttons:SelectionIndicatorSettings 
            SelectionIndicatorPlacement="Fill"
            Background="#1976D2"/>
    </buttons:SfSegmentedControl.SelectionIndicatorSettings>
</buttons:SfSegmentedControl>
```

**C#:**
```csharp
segmentedControl.SelectionIndicatorSettings = new SelectionIndicatorSettings
{
    SelectionIndicatorPlacement = SelectionIndicatorPlacement.Fill,
    Background = Color.FromArgb("#1976D2")
};
```

**Note:** `Background` property only applies when `SelectionIndicatorPlacement` is `Fill`.

### Selected Background Color (Per-Item)

Customize the selected background for individual segments.

**C#:**
```csharp
var segmentedControl = new SfSegmentedControl
{
    ItemsSource = new List<SfSegmentItem>
    {
        new SfSegmentItem 
        { 
            Text = "Low", 
            SelectedSegmentBackground = Colors.Green 
        },
        new SfSegmentItem 
        { 
            Text = "Medium", 
            SelectedSegmentBackground = Colors.Orange 
        },
        new SfSegmentItem 
        { 
            Text = "High", 
            SelectedSegmentBackground = Colors.Red 
        }
    },
    SelectionIndicatorSettings = new SelectionIndicatorSettings
    {
        SelectionIndicatorPlacement = SelectionIndicatorPlacement.Fill
    }
};
```

**Use case:** Priority indicators, status levels, or semantic colors (success/warning/danger).

### Selected Text Color (Global)

Change the text color for all selected segments.

**XAML:**
```xml
<buttons:SfSegmentedControl>
    <buttons:SfSegmentedControl.SelectionIndicatorSettings>
        <buttons:SelectionIndicatorSettings 
            TextColor="White"
            Background="#6200EE"/>
    </buttons:SfSegmentedControl.SelectionIndicatorSettings>
</buttons:SfSegmentedControl>
```

**C#:**
```csharp
segmentedControl.SelectionIndicatorSettings = new SelectionIndicatorSettings
{
    TextColor = Colors.White,
    Background = Color.FromArgb("#6200EE")
};
```

### Selected Text Color (Per-Item)

Customize text color for individual segments when selected.

**C#:**
```csharp
var segmentedControl = new SfSegmentedControl
{
    ItemsSource = new List<SfSegmentItem>
    {
        new SfSegmentItem 
        { 
            Text = "Active", 
            SelectedSegmentTextColor = Colors.Blue 
        },
        new SfSegmentItem 
        { 
            Text = "Inactive", 
            SelectedSegmentTextColor = Colors.Gray 
        }
    }
};
```

### Selected Border Color

Customize the border color for selected segments (applies to Border, TopBorder, BottomBorder placements).

**XAML:**
```xml
<buttons:SfSegmentedControl>
    <buttons:SfSegmentedControl.SelectionIndicatorSettings>
        <buttons:SelectionIndicatorSettings 
            SelectionIndicatorPlacement="Border"
            Stroke="#FF5722"
            StrokeThickness="2"/>
    </buttons:SfSegmentedControl.SelectionIndicatorSettings>
</buttons:SfSegmentedControl>
```

**C#:**
```csharp
segmentedControl.SelectionIndicatorSettings = new SelectionIndicatorSettings
{
    SelectionIndicatorPlacement = SelectionIndicatorPlacement.Border,
    Stroke = Color.FromArgb("#FF5722"),
    StrokeThickness = 2
};
```

**Note:** `Stroke` and `StrokeThickness` only apply when placement is `Border`, `TopBorder`, or `BottomBorder`.

### Selected Border Thickness

Control the thickness of the selection border.

**XAML:**
```xml
<buttons:SfSegmentedControl>
    <buttons:SfSegmentedControl.SelectionIndicatorSettings>
        <buttons:SelectionIndicatorSettings 
            SelectionIndicatorPlacement="BottomBorder"
            Stroke="#6200EE"
            StrokeThickness="4"/>
    </buttons:SfSegmentedControl.SelectionIndicatorSettings>
</buttons:SfSegmentedControl>
```

**Recommended values:**
- Subtle: 1-2 pixels
- Standard: 2-3 pixels
- Bold: 3-5 pixels

## Ripple Effect Animation

Enable or disable the tap ripple animation that provides visual feedback when a segment is tapped.

### Enabling Ripple Effect (Default)

**XAML:**
```xml
<buttons:SfSegmentedControl EnableRippleEffect="True">
    <!-- Items -->
</buttons:SfSegmentedControl>
```

**C#:**
```csharp
segmentedControl.EnableRippleEffect = true;  // Default
```

### Disabling Ripple Effect

**XAML:**
```xml
<buttons:SfSegmentedControl EnableRippleEffect="False">
    <!-- Items -->
</buttons:SfSegmentedControl>
```

**C#:**
```csharp
segmentedControl.EnableRippleEffect = false;
```

**When to disable:**
- Custom tap animations already implemented
- Minimalist design requiring no visual feedback
- Accessibility requirements preferring simpler interactions

**Note:** The ripple effect applies to both default segments and custom DataTemplate segments.

## Best Practices

### Indicator Visibility

- Ensure sufficient color contrast between selected and unselected states (WCAG AA: 3:1 minimum)
- Use Fill placement for critical selections that must be immediately obvious
- Use Border placements for subtle, secondary selections

### Selection Modes

- Use **Single** for mutually exclusive choices (view modes, sort orders)
- Use **SingleDeselect** for optional filters or toggleable features
- Always provide a sensible default selection for Single mode

### Color Combinations

Ensure text remains readable on selected backgrounds:

```csharp
// Good contrast
Background = Colors.DarkBlue
TextColor = Colors.White

// Poor contrast (avoid)
Background = Colors.LightGray
TextColor = Colors.White
```

### Responsive Design

Test selection indicators across different screen sizes and orientations to ensure they remain visible and proportional.

## Troubleshooting

### Selection Not Visible

**Cause:** Selection indicator color matches segment background  
**Solution:** Set contrasting `Background` or `Stroke` color in SelectionIndicatorSettings

### SelectedIndex Not Updating

**Cause:** Setting SelectedIndex to out-of-range value  
**Solution:** Ensure index is between 0 and ItemsSource.Count - 1

### Background Property Not Working

**Cause:** SelectionIndicatorPlacement is not set to Fill  
**Solution:** Set `SelectionIndicatorPlacement = SelectionIndicatorPlacement.Fill`

### Stroke Property Not Working

**Cause:** SelectionIndicatorPlacement is set to Fill  
**Solution:** Use Border, TopBorder, or BottomBorder placement for Stroke to apply

### Ripple Effect Not Showing

**Cause:** EnableRippleEffect is false or platform doesn't support ripple  
**Solution:** Set `EnableRippleEffect = true` and test on Android/iOS

## Next Steps

- **Customize appearance:** See [customization.md](customization.md) for overall styling
- **Configure layout:** See [layout.md](layout.md) for sizing and scrolling
- **Handle events:** See [events.md](events.md) for responding to selection changes
