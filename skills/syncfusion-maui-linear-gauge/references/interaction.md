# Interaction

## Table of Contents
- [Overview](#overview)
- [Enabling Interaction](#enabling-interaction)
- [Drag Gestures](#drag-gestures)
- [Swipe Gestures](#swipe-gestures)
- [Pointer Events](#pointer-events)
  - [ValueChangeStarted](#valuechangestarted)
  - [ValueChanging](#valuechanging)
  - [ValueChangeCompleted](#valuechangecompleted)
- [Constraining Pointer Movement](#constraining-pointer-movement)
- [Interactive Pointer Examples](#interactive-pointer-examples)
- [Multi-Pointer Interaction](#multi-pointer-interaction)

## Overview

Linear Gauge supports interactive pointers that users can drag or swipe to change values. This is perfect for creating sliders, adjustable controls, and user input interfaces.

**Interactive Pointer Types:**
- Shape marker pointers
- Content marker pointers
- **Not supported:** Bar pointers (read-only)

**User Interactions:**
- **Drag** - Click/touch and drag pointer along scale
- **Swipe** - Quick flick gesture to move pointer
- **Events** - Real-time feedback during value changes

**Use Cases:**
- Volume and brightness controls
- Temperature adjusters
- Settings sliders
- Range selectors
- Interactive dashboards

## Enabling Interaction

Enable interaction by setting the `IsInteractive` property to `true`.

**XAML:**
```xml
<gauge:SfLinearGauge>
    <gauge:SfLinearGauge.MarkerPointers>
        <gauge:LinearShapePointer Value="50" 
                                 IsInteractive="True"
                                 ShapeType="Circle"
                                 Fill="#2196F3"
                                 ShapeHeight="25"
                                 ShapeWidth="25"/>
    </gauge:SfLinearGauge.MarkerPointers>
</gauge:SfLinearGauge>
```

**C#:**
```csharp
gauge.MarkerPointers.Add(new LinearShapePointer
{
    Value = 50,
    IsInteractive = true,
    ShapeType = LinearShapeType.Circle,
    Fill = new SolidColorBrush(Color.FromArgb("#2196F3")),
    ShapeHeight = 25,
    ShapeWidth = 25
});
```

**Default Behavior:**
- `IsInteractive = false` (read-only)
- When `true`, pointer responds to touch/mouse input
- Pointer stays within scale minimum/maximum bounds

## Drag Gestures

Users can click/touch and drag pointers to new values.

**Example: Basic Draggable Slider**

```xml
<VerticalStackLayout Spacing="20" Padding="20">
    
    <Label Text="Adjust Volume" FontSize="18" FontAttributes="Bold"/>
    
    <gauge:SfLinearGauge Minimum="0" Maximum="100">
        <gauge:SfLinearGauge.MarkerPointers>
            <gauge:LinearShapePointer Value="65" 
                                     IsInteractive="True"
                                     ShapeType="Circle"
                                     Fill="#2196F3"
                                     ShapeHeight="30"
                                     ShapeWidth="30"/>
        </gauge:SfLinearGauge.MarkerPointers>
    </gauge:SfLinearGauge>
    
    <Label x:Name="VolumeLabel" 
           Text="Volume: 65%" 
           HorizontalOptions="Center"/>
    
</VerticalStackLayout>
```

**Drag Behavior:**
- Pointer follows cursor/finger position
- Snaps to scale values
- Updates in real-time
- Respects scale bounds

## Swipe Gestures

Quick flick gestures move pointers along the scale.

**Example: Swipe-Enabled Control**

```csharp
SfLinearGauge swipeGauge = new SfLinearGauge
{
    Minimum = 0,
    Maximum = 10
};

swipeGauge.MarkerPointers.Add(new LinearShapePointer
{
    Value = 5,
    IsInteractive = true,
    ShapeType = LinearShapeType.Diamond,
    Fill = new SolidColorBrush(Colors.Orange),
    ShapeHeight = 25,
    ShapeWidth = 25
});
```

**Swipe Behavior:**
- Quick gesture moves pointer
- Momentum-based movement
- Smooth deceleration
- Stops at scale bounds

## Pointer Events

Three events track pointer value changes:

### ValueChangeStarted

Fired when user begins dragging/touching pointer.

**XAML:**
```xml
<gauge:LinearShapePointer Value="50" 
                         IsInteractive="True"
                         ValueChangeStarted="OnValueChangeStarted"/>
```

**C#:**
```csharp
pointer.ValueChangeStarted += OnValueChangeStarted;

private void OnValueChangeStarted(object sender, ValueChangedEventArgs e)
{
    Console.WriteLine($"Started at: {e.Value}");
    // Capture initial value, show UI feedback, etc.
}
```

**Use Cases:**
- Capture initial value before change
- Show editing UI (highlight, borders)
- Disable other controls during drag
- Start logging user action

### ValueChanging

Fired continuously as pointer value changes during drag.

**XAML:**
```xml
<gauge:LinearShapePointer Value="50" 
                         IsInteractive="True"
                         ValueChanging="OnValueChanging"/>
```

**C#:**
```csharp
pointer.ValueChanging += OnValueChanging;

private void OnValueChanging(object sender, ValueChangingEventArgs e)
{
    Console.WriteLine($"Changing: {e.OldValue} → {e.NewValue}");
    
    // Update UI in real-time
    ValueLabel.Text = $"{e.NewValue:F1}";
    
    // Cancel change if needed
    if (e.NewValue > 80)
    {
        e.Cancel = true;  // Prevent exceeding 80
    }
}
```

**ValueChangingEventArgs Properties:**
- `OldValue` - Previous pointer value
- `NewValue` - New pointer value
- `Cancel` - Set to true to prevent change

**Use Cases:**
- Real-time value display
- Live updates to other UI elements
- Constrain values dynamically
- Validate input while dragging

### ValueChangeCompleted

Fired when user releases pointer after drag.

**XAML:**
```xml
<gauge:LinearShapePointer Value="50" 
                         IsInteractive="True"
                         ValueChangeCompleted="OnValueChangeCompleted"/>
```

**C#:**
```csharp
pointer.ValueChangeCompleted += OnValueChangeCompleted;

private void OnValueChangeCompleted(object sender, ValueChangedEventArgs e)
{
    Console.WriteLine($"Completed at: {e.Value}");
    
    // Save final value
    SaveUserPreference(e.Value);
    
    // Hide editing UI
    PointerBorder.IsVisible = false;
    
    // Trigger action based on final value
    ApplyVolumeChange(e.Value);
}
```

**Use Cases:**
- Save final value to database
- Trigger actions after adjustment
- Hide editing UI
- Log completed change
- Undo/redo support

## Constraining Pointer Movement

### Constrain to Specific Range

```csharp
pointer.ValueChanging += (s, e) =>
{
    // Keep between 20 and 80
    if (e.NewValue < 20 || e.NewValue > 80)
    {
        e.Cancel = true;
    }
};
```

### Snap to Increments

```csharp
pointer.ValueChanging += (s, e) =>
{
    // Snap to multiples of 5
    double snappedValue = Math.Round(e.NewValue / 5) * 5;
    
    if (snappedValue != e.NewValue)
    {
        e.Cancel = true;
        pointer.Value = snappedValue;
    }
};
```

### Prevent Negative Values

```csharp
pointer.ValueChanging += (s, e) =>
{
    if (e.NewValue < 0)
    {
        e.Cancel = true;
    }
};
```

### Require Minimum Change

```csharp
double lastSavedValue = 50;

pointer.ValueChanging += (s, e) =>
{
    // Require at least 5 unit change
    if (Math.Abs(e.NewValue - lastSavedValue) < 5)
    {
        // Allow dragging but don't save yet
    }
};

pointer.ValueChangeCompleted += (s, e) =>
{
    if (Math.Abs(e.Value - lastSavedValue) >= 5)
    {
        lastSavedValue = e.Value;
        SaveValue(e.Value);
    }
    else
    {
        // Revert to last saved
        pointer.Value = lastSavedValue;
    }
};
```

## Interactive Pointer Examples

### Example 1: Volume Control

```xml
<VerticalStackLayout Padding="20" Spacing="15">
    
    <HorizontalStackLayout Spacing="10">
        <Image Source="volume_icon.png" HeightRequest="24" WidthRequest="24"/>
        <Label x:Name="VolumeLabel" 
               Text="Volume: 50%" 
               FontSize="16"
               VerticalOptions="Center"/>
    </HorizontalStackLayout>
    
    <gauge:SfLinearGauge Minimum="0" 
                        Maximum="100"
                        ShowLabels="False"
                        ShowTicks="False">
        
        <!-- Background track -->
        <gauge:SfLinearGauge.LineStyle>
            <gauge:LinearLineStyle Thickness="8" 
                                  Fill="#E0E0E0"
                                  CornerStyle="BothCurve"/>
        </gauge:SfLinearGauge.LineStyle>
        
        <!-- Filled portion -->
        <gauge:SfLinearGauge.BarPointers>
            <gauge:BarPointer x:Name="VolumeBar" 
                             Value="50" 
                             PointerSize="8"
                             Fill="#2196F3"
                             CornerStyle="BothCurve"/>
        </gauge:SfLinearGauge.BarPointers>
        
        <!-- Interactive handle -->
        <gauge:SfLinearGauge.MarkerPointers>
            <gauge:LinearShapePointer x:Name="VolumePointer"
                                     Value="50" 
                                     IsInteractive="True"
                                     ShapeType="Circle"
                                     Fill="White"
                                     Stroke="#2196F3"
                                     StrokeThickness="3"
                                     ShapeHeight="24"
                                     ShapeWidth="24"
                                     ValueChanging="OnVolumeChanging"/>
        </gauge:SfLinearGauge.MarkerPointers>
        
    </gauge:SfLinearGauge>
    
</VerticalStackLayout>
```

```csharp
private void OnVolumeChanging(object sender, ValueChangingEventArgs e)
{
    // Update label
    VolumeLabel.Text = $"Volume: {e.NewValue:F0}%";
    
    // Sync bar pointer
    VolumeBar.Value = e.NewValue;
}
```

### Example 2: Temperature Adjuster with Min/Max

```csharp
SfLinearGauge tempGauge = new SfLinearGauge
{
    Minimum = 60,
    Maximum = 80,
    Interval = 5
};

Label tempLabel = new Label
{
    Text = "72°F",
    FontSize = 20,
    FontAttributes = FontAttributes.Bold,
    HorizontalOptions = LayoutOptions.Center
};

LinearShapePointer tempPointer = new LinearShapePointer
{
    Value = 72,
    IsInteractive = true,
    ShapeType = LinearShapeType.Circle,
    Fill = new SolidColorBrush(Colors.Orange),
    ShapeHeight = 30,
    ShapeWidth = 30
};

tempPointer.ValueChanging += (s, e) =>
{
    tempLabel.Text = $"{e.NewValue:F0}°F";
    
    // Change color based on temperature
    if (e.NewValue < 68)
        tempPointer.Fill = new SolidColorBrush(Colors.Blue);
    else if (e.NewValue > 75)
        tempPointer.Fill = new SolidColorBrush(Colors.Red);
    else
        tempPointer.Fill = new SolidColorBrush(Colors.Orange);
};

tempGauge.MarkerPointers.Add(tempPointer);
```

### Example 3: Snapping Slider

```csharp
LinearShapePointer snapPointer = new LinearShapePointer
{
    Value = 50,
    IsInteractive = true,
    ShapeType = LinearShapeType.Diamond,
    Fill = new SolidColorBrush(Colors.Purple),
    ShapeHeight = 25,
    ShapeWidth = 25
};

snapPointer.ValueChanging += (s, e) =>
{
    // Snap to multiples of 10
    double snapped = Math.Round(e.NewValue / 10) * 10;
    
    if (Math.Abs(snapped - e.NewValue) > 0.1)
    {
        e.Cancel = true;
        snapPointer.Value = snapped;
    }
};

snapPointer.ValueChangeCompleted += (s, e) =>
{
    Console.WriteLine($"Final snapped value: {e.Value}");
};
```

### Example 4: Range Selector (Two Pointers)

```csharp
SfLinearGauge rangeGauge = new SfLinearGauge
{
    Minimum = 0,
    Maximum = 100
};

// Min value pointer
LinearShapePointer minPointer = new LinearShapePointer
{
    Value = 20,
    IsInteractive = true,
    ShapeType = LinearShapeType.Triangle,
    Fill = new SolidColorBrush(Colors.Blue),
    ShapeHeight = 20,
    ShapeWidth = 20
};

// Max value pointer
LinearShapePointer maxPointer = new LinearShapePointer
{
    Value = 80,
    IsInteractive = true,
    ShapeType = LinearShapeType.InvertedTriangle,
    Fill = new SolidColorBrush(Colors.Red),
    ShapeHeight = 20,
    ShapeWidth = 20
};

// Prevent overlap
minPointer.ValueChanging += (s, e) =>
{
    if (e.NewValue >= maxPointer.Value)
    {
        e.Cancel = true;
    }
};

maxPointer.ValueChanging += (s, e) =>
{
    if (e.NewValue <= minPointer.Value)
    {
        e.Cancel = true;
    }
};

rangeGauge.MarkerPointers.Add(minPointer);
rangeGauge.MarkerPointers.Add(maxPointer);
```

### Example 5: With Visual Feedback

```xml
<gauge:LinearShapePointer x:Name="FeedbackPointer"
                         Value="50" 
                         IsInteractive="True"
                         ShapeType="Circle"
                         Fill="White"
                         Stroke="#2196F3"
                         StrokeThickness="2"
                         ShapeHeight="25"
                         ShapeWidth="25"
                         ValueChangeStarted="OnDragStart"
                         ValueChangeCompleted="OnDragEnd"/>
```

```csharp
private void OnDragStart(object sender, ValueChangedEventArgs e)
{
    // Visual feedback: increase size and add shadow
    FeedbackPointer.ShapeHeight = 35;
    FeedbackPointer.ShapeWidth = 35;
    FeedbackPointer.Shadow = new Shadow
    {
        Brush = Colors.Gray,
        Opacity = 0.5f,
        Radius = 10,
        Offset = new Point(2, 2)
    };
}

private void OnDragEnd(object sender, ValueChangedEventArgs e)
{
    // Reset to normal
    FeedbackPointer.ShapeHeight = 25;
    FeedbackPointer.ShapeWidth = 25;
    FeedbackPointer.Shadow = null;
}
```

## Multi-Pointer Interaction

Handle multiple interactive pointers on the same gauge.

**Example: Priority Levels**

```csharp
SfLinearGauge priorityGauge = new SfLinearGauge
{
    Minimum = 0,
    Maximum = 100
};

// Low priority threshold
LinearShapePointer lowPointer = new LinearShapePointer
{
    Value = 33,
    IsInteractive = true,
    ShapeType = LinearShapeType.Circle,
    Fill = new SolidColorBrush(Colors.Green),
    ShapeHeight = 20,
    ShapeWidth = 20
};

// Medium priority threshold
LinearShapePointer medPointer = new LinearShapePointer
{
    Value = 66,
    IsInteractive = true,
    ShapeType = LinearShapeType.Circle,
    Fill = new SolidColorBrush(Colors.Yellow),
    ShapeHeight = 20,
    ShapeWidth = 20
};

// Maintain order: low < medium
lowPointer.ValueChanging += (s, e) =>
{
    if (e.NewValue >= medPointer.Value - 5)
        e.Cancel = true;
};

medPointer.ValueChanging += (s, e) =>
{
    if (e.NewValue <= lowPointer.Value + 5)
        e.Cancel = true;
};

// Save on completion
EventHandler<ValueChangedEventArgs> saveThresholds = (s, e) =>
{
    SaveThresholds(lowPointer.Value, medPointer.Value);
};

lowPointer.ValueChangeCompleted += saveThresholds;
medPointer.ValueChangeCompleted += saveThresholds;

priorityGauge.MarkerPointers.Add(lowPointer);
priorityGauge.MarkerPointers.Add(medPointer);
```

**Best Practices for Multi-Pointer:**
- Prevent pointer overlap with ValueChanging events
- Maintain logical order (min < max)
- Use different shapes/colors for distinction
- Provide clear visual feedback
- Sync dependent pointers appropriately
