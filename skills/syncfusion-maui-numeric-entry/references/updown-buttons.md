# UpDown Buttons in .NET MAUI Numeric Entry

This guide covers all features related to incrementing and decrementing values using up/down buttons, keyboard keys, and mouse scrolling.

## Table of Contents
- [Increase or Decrease Value](#increase-or-decrease-value)
- [UpDown Button Placement](#updown-button-placement)
- [UpDown Button Alignment](#updown-button-alignment)
- [UpDown Button Order](#updown-button-order)
- [UpDown Button Color](#updown-button-color)
- [UpDown Button Template](#updown-button-template)
- [Auto Reverse](#auto-reverse)

## Increase or Decrease Value

The NumericEntry value can be incremented or decremented using:
- **Keyboard Arrow Keys** (↑↓) - Small changes
- **Page Up/Page Down Keys** - Large changes
- **Mouse Scrolling** (desktop) - Small changes
- **Up/Down Buttons** (when visible) - Small changes

### SmallChange Property

Controls the increment/decrement amount for:
- Arrow keys (↑↓)
- Mouse scrolling
- Up/Down button clicks

```xml
<editors:SfNumericEntry WidthRequest="200"
                        Value="10"
                        SmallChange="5" />
```

```csharp
var numericEntry = new SfNumericEntry
{
    WidthRequest = 200,
    Value = 10,
    SmallChange = 5
};
```

**Default value:** `1`

**Behavior:**
- Press ↑ or click up button → Value increases by 5
- Press ↓ or click down button → Value decreases by 5
- Scroll mouse up → Value increases by 5
- Scroll mouse down → Value decreases by 5

### LargeChange Property

Controls the increment/decrement amount for:
- Page Up key
- Page Down key

```xml
<editors:SfNumericEntry WidthRequest="200"
                        Value="10"
                        SmallChange="5"
                        LargeChange="10" />
```

```csharp
var numericEntry = new SfNumericEntry
{
    WidthRequest = 200,
    Value = 10,
    SmallChange = 5,
    LargeChange = 10
};
```

**Default value:** `10`

**Behavior:**
- Press Page Up → Value increases by 10
- Press Page Down → Value decreases by 10

### Example: Fine and Coarse Control

```xml
<VerticalStackLayout Padding="20" Spacing="15">
    
    <!-- Price with $0.01 and $1 increments -->
    <StackLayout Spacing="5">
        <Label Text="Price (↑↓: $0.01, PageUp/Down: $1):" />
        <editors:SfNumericEntry WidthRequest="250"
                                CustomFormat="C2"
                                Value="19.99"
                                SmallChange="0.01"
                                LargeChange="1.00" />
    </StackLayout>
    
    <!-- Quantity with 1 and 10 increments -->
    <StackLayout Spacing="5">
        <Label Text="Quantity (↑↓: 1, PageUp/Down: 10):" />
        <editors:SfNumericEntry WidthRequest="250"
                                CustomFormat="N0"
                                Value="1"
                                SmallChange="1"
                                LargeChange="10" />
    </StackLayout>
    
    <!-- Temperature with 0.1 and 1 increments -->
    <StackLayout Spacing="5">
        <Label Text="Temperature (↑↓: 0.1°, PageUp/Down: 1°):" />
        <editors:SfNumericEntry WidthRequest="250"
                                CustomFormat="N1' °C'"
                                Value="20"
                                SmallChange="0.1"
                                LargeChange="1" />
    </StackLayout>
    
</VerticalStackLayout>
```

### Mouse Scrolling

Mouse scrolling is automatically enabled on desktop platforms.

**Behavior:**
- Scroll up → Value increases by `SmallChange`
- Scroll down → Value decreases by `SmallChange`
- Works when control has focus
- Respects `Minimum` and `Maximum` constraints

## UpDown Button Placement

The `UpDownPlacementMode` property controls the visibility and position of up/down buttons.

### Hidden (Default)

No buttons are displayed.

```xml
<editors:SfNumericEntry WidthRequest="200"
                        UpDownPlacementMode="Hidden" />
```

**Default value:** `Hidden`

**Use case:** Standard keyboard/mouse input only

### Inline

Buttons displayed **horizontally** beside the entry.

```xml
<editors:SfNumericEntry WidthRequest="200"
                        Value="360"
                        UpDownPlacementMode="Inline" />
```

```csharp
var numericEntry = new SfNumericEntry
{
    WidthRequest = 200,
    Value = 360,
    UpDownPlacementMode = NumericEntryUpDownPlacementMode.Inline
};
```

**Layout:**
```
┌────────────────────────────┐
│  [↑] [↓]  360  [Clear]    │ ← Buttons on left by default
└────────────────────────────┘
```

**Use case:** Desktop interfaces, wide layouts, horizontal space available

### InlineVertical

Buttons displayed **vertically** stacked.

```xml
<editors:SfNumericEntry WidthRequest="200"
                        Value="360"
                        UpDownPlacementMode="InlineVertical" />
```

```csharp
var numericEntry = new SfNumericEntry
{
    WidthRequest = 200,
    Value = 360,
    UpDownPlacementMode = NumericEntryUpDownPlacementMode.InlineVertical
};
```

**Layout:**
```
┌────────────────────────────┐
│  [↑]  360  [Clear]        │
│  [↓]                      │
└────────────────────────────┘
```

**Use case:** Mobile interfaces, compact layouts, narrow spaces

### Example: All Placement Modes

```xml
<VerticalStackLayout Padding="20" Spacing="20">
    
    <!-- Hidden -->
    <StackLayout Spacing="5">
        <Label Text="Hidden (No buttons):" />
        <editors:SfNumericEntry WidthRequest="250"
                                Value="100"
                                UpDownPlacementMode="Hidden" />
    </StackLayout>
    
    <!-- Inline -->
    <StackLayout Spacing="5">
        <Label Text="Inline (Horizontal):" />
        <editors:SfNumericEntry WidthRequest="250"
                                Value="100"
                                UpDownPlacementMode="Inline" />
    </StackLayout>
    
    <!-- InlineVertical -->
    <StackLayout Spacing="5">
        <Label Text="InlineVertical (Stacked):" />
        <editors:SfNumericEntry WidthRequest="250"
                                Value="100"
                                UpDownPlacementMode="InlineVertical" />
    </StackLayout>
    
</VerticalStackLayout>
```

## UpDown Button Alignment

The `UpDownButtonAlignment` property controls the horizontal position of buttons (when using `Inline` mode).

### Right Alignment (Default)

Buttons displayed on the **right** side.

```xml
<editors:SfNumericEntry Value="123"
                        WidthRequest="200"
                        UpDownPlacementMode="Inline"
                        UpDownButtonAlignment="Right" />
```

**Default value:** `Right`

**Layout:**
```
┌────────────────────────────┐
│  123  [Clear] [↑] [↓]     │
└────────────────────────────┘
```

### Left Alignment

Buttons displayed on the **left** side.

```xml
<editors:SfNumericEntry Value="123"
                        HorizontalTextAlignment="End"
                        WidthRequest="200"
                        UpDownPlacementMode="Inline"
                        UpDownButtonAlignment="Left" />
```

```csharp
var numericEntry = new SfNumericEntry
{
    Value = 123,
    HorizontalTextAlignment = TextAlignment.End,
    WidthRequest = 200,
    UpDownPlacementMode = NumericEntryUpDownPlacementMode.Inline,
    UpDownButtonAlignment = UpDownButtonAlignment.Left
};
```

**Layout:**
```
┌────────────────────────────┐
│  [↑] [↓]  123  [Clear]    │
└────────────────────────────┘
```

**Tip:** Combine with `HorizontalTextAlignment="End"` for right-aligned text

### Both Alignment

Buttons displayed on **both** sides.

```xml
<editors:SfNumericEntry Value="123"
                        HorizontalTextAlignment="Center"
                        WidthRequest="200"
                        UpDownPlacementMode="Inline"
                        UpDownButtonAlignment="Both" />
```

```csharp
var numericEntry = new SfNumericEntry
{
    Value = 123,
    HorizontalTextAlignment = TextAlignment.Center,
    WidthRequest = 200,
    UpDownPlacementMode = NumericEntryUpDownPlacementMode.Inline,
    UpDownButtonAlignment = UpDownButtonAlignment.Both
};
```

**Layout:**
```
┌────────────────────────────┐
│  [↓]  123  [Clear] [↑]    │
└────────────────────────────┘
```

**Tip:** Combine with `HorizontalTextAlignment="Center"` for centered text

### Example: All Alignments

```xml
<VerticalStackLayout Padding="20" Spacing="20">
    
    <!-- Right (Default) -->
    <StackLayout Spacing="5">
        <Label Text="Right Alignment (Default):" />
        <editors:SfNumericEntry WidthRequest="250"
                                Value="123"
                                UpDownPlacementMode="Inline"
                                UpDownButtonAlignment="Right" />
    </StackLayout>
    
    <!-- Left -->
    <StackLayout Spacing="5">
        <Label Text="Left Alignment:" />
        <editors:SfNumericEntry WidthRequest="250"
                                Value="123"
                                HorizontalTextAlignment="End"
                                UpDownPlacementMode="Inline"
                                UpDownButtonAlignment="Left" />
    </StackLayout>
    
    <!-- Both -->
    <StackLayout Spacing="5">
        <Label Text="Both Sides:" />
        <editors:SfNumericEntry WidthRequest="250"
                                Value="123"
                                HorizontalTextAlignment="Center"
                                UpDownPlacementMode="Inline"
                                UpDownButtonAlignment="Both" />
    </StackLayout>
    
</VerticalStackLayout>
```

## UpDown Button Order

The `UpDownOrder` property controls the order of up and down buttons.

### UpThenDown (Default)

Up button appears first (left or top), then down button.

```xml
<editors:SfNumericEntry Value="123"
                        WidthRequest="200"
                        UpDownOrder="UpThenDown"
                        UpDownPlacementMode="Inline"
                        UpDownButtonAlignment="Right" />
```

**Default value:** `UpThenDown`

**Inline layout:**
```
┌────────────────────────────┐
│  123  [Clear] [↑] [↓]     │
└────────────────────────────┘
```

**InlineVertical layout:**
```
┌────────────────────────────┐
│  [↑]  123  [Clear]        │
│  [↓]                      │
└────────────────────────────┘
```

### DownThenUp

Down button appears first (left or top), then up button.

```xml
<editors:SfNumericEntry Value="123"
                        WidthRequest="200"
                        UpDownOrder="DownThenUp"
                        UpDownPlacementMode="Inline"
                        UpDownButtonAlignment="Right" />
```

```csharp
var numericEntry = new SfNumericEntry
{
    Value = 123,
    WidthRequest = 200,
    UpDownOrder = UpDownOrder.DownThenUp,
    UpDownPlacementMode = NumericEntryUpDownPlacementMode.Inline,
    UpDownButtonAlignment = UpDownButtonAlignment.Right
};
```

**Inline layout:**
```
┌────────────────────────────┐
│  123  [Clear] [↓] [↑]     │
└────────────────────────────┘
```

**InlineVertical layout:**
```
┌────────────────────────────┐
│  [↓]  123  [Clear]        │
│  [↑]                      │
└────────────────────────────┘
```

### Example: Button Order

```xml
<VerticalStackLayout Padding="20" Spacing="20">
    
    <!-- UpThenDown (Default) -->
    <StackLayout Spacing="5">
        <Label Text="UpThenDown (↑ ↓):" />
        <editors:SfNumericEntry WidthRequest="250"
                                Value="100"
                                UpDownOrder="UpThenDown"
                                UpDownPlacementMode="Inline"
                                UpDownButtonAlignment="Right" />
    </StackLayout>
    
    <!-- DownThenUp -->
    <StackLayout Spacing="5">
        <Label Text="DownThenUp (↓ ↑):" />
        <editors:SfNumericEntry WidthRequest="250"
                                Value="100"
                                UpDownOrder="DownThenUp"
                                UpDownPlacementMode="Inline"
                                UpDownButtonAlignment="Right" />
    </StackLayout>
    
</VerticalStackLayout>
```

## UpDown Button Color

The `UpDownButtonColor` property customizes the color of button icons.

```xml
<editors:SfNumericEntry HeightRequest="50"
                        WidthRequest="200"
                        Value="360"
                        UpDownPlacementMode="Inline"
                        UpDownButtonColor="Blue" />
```

```csharp
var numericEntry = new SfNumericEntry
{
    HeightRequest = 50,
    WidthRequest = 200,
    Value = 360,
    UpDownPlacementMode = NumericEntryUpDownPlacementMode.Inline,
    UpDownButtonColor = Colors.Blue
};
```

### Example: Themed Buttons

```xml
<VerticalStackLayout Padding="20" Spacing="20">
    
    <!-- Blue Theme -->
    <StackLayout Spacing="5">
        <Label Text="Blue Theme:" />
        <editors:SfNumericEntry WidthRequest="250"
                                Value="100"
                                UpDownPlacementMode="Inline"
                                UpDownButtonColor="Blue" />
    </StackLayout>
    
    <!-- Green Theme -->
    <StackLayout Spacing="5">
        <Label Text="Green Theme:" />
        <editors:SfNumericEntry WidthRequest="250"
                                Value="100"
                                UpDownPlacementMode="Inline"
                                UpDownButtonColor="Green" />
    </StackLayout>
    
    <!-- Red Theme -->
    <StackLayout Spacing="5">
        <Label Text="Red Theme:" />
        <editors:SfNumericEntry WidthRequest="250"
                                Value="100"
                                UpDownPlacementMode="Inline"
                                UpDownButtonColor="Red" />
    </StackLayout>
    
</VerticalStackLayout>
```

## UpDown Button Template

Use `UpButtonTemplate` and `DownButtonTemplate` properties to completely customize button appearance.

### Custom Button Templates

```xml
<editors:SfNumericEntry x:Name="numericEntry"
                        WidthRequest="200"
                        HeightRequest="40"
                        VerticalOptions="Center"
                        UpDownPlacementMode="Inline"
                        Value="50">
    
    <!-- Custom Up Button -->
    <editors:SfNumericEntry.UpButtonTemplate>
        <DataTemplate>
            <Grid>
                <Label Padding="10,3,15,10"
                       FontFamily="FontIcons"
                       HorizontalOptions="Center"
                       Text="&#8593;"
                       TextColor="Green"
                       FontSize="20" />
            </Grid>
        </DataTemplate>
    </editors:SfNumericEntry.UpButtonTemplate>
    
    <!-- Custom Down Button -->
    <editors:SfNumericEntry.DownButtonTemplate>
        <DataTemplate>
            <Grid>
                <Label Padding="10,3,15,10"
                       Rotation="180"
                       FontFamily="FontIcons"
                       HorizontalOptions="Center"
                       Text="&#8593;"
                       TextColor="Red"
                       FontSize="20" />
            </Grid>
        </DataTemplate>
    </editors:SfNumericEntry.DownButtonTemplate>
    
</editors:SfNumericEntry>
```

### C# Custom Templates

```csharp
var numericEntry = new SfNumericEntry
{
    WidthRequest = 200,
    HeightRequest = 40,
    VerticalOptions = LayoutOptions.Center,
    UpDownPlacementMode = NumericEntryUpDownPlacementMode.Inline,
    Value = 50
};

// Up Button Template
var upButtonTemplate = new DataTemplate(() =>
{
    var grid = new Grid();
    var label = new Label
    {
        Padding = new Thickness(10, 3, 15, 10),
        FontFamily = "FontIcons",
        HorizontalOptions = LayoutOptions.Center,
        Text = "\u2191",  // Up arrow Unicode
        TextColor = Colors.Green,
        FontSize = 20
    };
    grid.Children.Add(label);
    return grid;
});

// Down Button Template
var downButtonTemplate = new DataTemplate(() =>
{
    var grid = new Grid();
    var label = new Label
    {
        Padding = new Thickness(10, 3, 15, 10),
        Rotation = 180,
        FontFamily = "FontIcons",
        HorizontalOptions = LayoutOptions.Center,
        Text = "\u2191",  // Up arrow rotated becomes down
        TextColor = Colors.Red,
        FontSize = 20
    };
    grid.Children.Add(label);
    return grid;
});

numericEntry.UpButtonTemplate = upButtonTemplate;
numericEntry.DownButtonTemplate = downButtonTemplate;
```

### Use Cases for Custom Templates

- Custom icons (plus/minus, chevrons, triangles)
- Branding consistency
- Theme integration
- Image buttons
- Animated buttons
- Accessibility enhancements

## Auto Reverse

The `AutoReverse` property makes the value wrap around when reaching `Minimum` or `Maximum`.

### Enable Auto Reverse

```xml
<editors:SfNumericEntry UpDownPlacementMode="Inline"
                        AutoReverse="True"
                        Minimum="0"
                        Maximum="10"
                        Value="0" />
```

```csharp
var numericEntry = new SfNumericEntry
{
    UpDownPlacementMode = NumericEntryUpDownPlacementMode.Inline,
    AutoReverse = true,
    Minimum = 0,
    Maximum = 10,
    Value = 0
};
```

**Default value:** `false`

**Behavior with AutoReverse=True:**
- Value at Maximum (10) → Click up button → Value becomes Minimum (0)
- Value at Minimum (0) → Click down button → Value becomes Maximum (10)

**Behavior with AutoReverse=False (Default):**
- Value at Maximum → Click up button → Value stays at Maximum
- Value at Minimum → Click down button → Value stays at Minimum

### Example: Hour Selector (0-23)

```xml
<StackLayout Spacing="5">
    <Label Text="Hour (0-23, wraps around):" />
    <editors:SfNumericEntry WidthRequest="250"
                            Value="0"
                            Minimum="0"
                            Maximum="23"
                            SmallChange="1"
                            AutoReverse="True"
                            UpDownPlacementMode="Inline"
                            CustomFormat="00' hours'" />
</StackLayout>
```

**Behavior:**
- At 23:00, click up → 00:00
- At 00:00, click down → 23:00

### Example: Minute/Second Selector (0-59)

```xml
<HorizontalStackLayout Spacing="10">
    
    <!-- Minutes -->
    <StackLayout Spacing="5">
        <Label Text="Minutes:" />
        <editors:SfNumericEntry WidthRequest="100"
                                Value="0"
                                Minimum="0"
                                Maximum="59"
                                SmallChange="1"
                                AutoReverse="True"
                                UpDownPlacementMode="Inline"
                                CustomFormat="00" />
    </StackLayout>
    
    <Label Text=":" VerticalOptions="Center" FontSize="24" />
    
    <!-- Seconds -->
    <StackLayout Spacing="5">
        <Label Text="Seconds:" />
        <editors:SfNumericEntry WidthRequest="100"
                                Value="0"
                                Minimum="0"
                                Maximum="59"
                                SmallChange="1"
                                AutoReverse="True"
                                UpDownPlacementMode="Inline"
                                CustomFormat="00" />
    </StackLayout>
    
</HorizontalStackLayout>
```

### Use Cases for Auto Reverse

- Time pickers (hours, minutes, seconds)
- Date pickers (days, months)
- Circular selections (angles 0-360°)
- Paginations with wraparound
- Cyclic values (week days, months)

## Complete UpDown Buttons Example

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:editors="clr-namespace:Syncfusion.Maui.Inputs;assembly=Syncfusion.Maui.Inputs"
             x:Class="NumericEntryApp.UpDownButtonsPage">
    
    <ScrollView>
        <VerticalStackLayout Padding="20" Spacing="20">
            
            <Label Text="UpDown Buttons Demo"
                   FontSize="20"
                   FontAttributes="Bold"
                   Margin="0,0,0,10" />
            
            <!-- Basic Inline Buttons -->
            <StackLayout Spacing="5">
                <Label Text="Basic Inline (SmallChange=1):" FontAttributes="Bold" />
                <editors:SfNumericEntry WidthRequest="250"
                                        Value="50"
                                        SmallChange="1"
                                        LargeChange="10"
                                        UpDownPlacementMode="Inline" />
            </StackLayout>
            
            <!-- Vertical Buttons -->
            <StackLayout Spacing="5">
                <Label Text="InlineVertical (Stacked):" FontAttributes="Bold" />
                <editors:SfNumericEntry WidthRequest="250"
                                        Value="50"
                                        SmallChange="1"
                                        UpDownPlacementMode="InlineVertical" />
            </StackLayout>
            
            <!-- Left Aligned -->
            <StackLayout Spacing="5">
                <Label Text="Left Aligned:" FontAttributes="Bold" />
                <editors:SfNumericEntry WidthRequest="250"
                                        Value="50"
                                        HorizontalTextAlignment="End"
                                        UpDownPlacementMode="Inline"
                                        UpDownButtonAlignment="Left" />
            </StackLayout>
            
            <!-- Both Sides -->
            <StackLayout Spacing="5">
                <Label Text="Both Sides (Centered text):" FontAttributes="Bold" />
                <editors:SfNumericEntry WidthRequest="250"
                                        Value="50"
                                        HorizontalTextAlignment="Center"
                                        UpDownPlacementMode="Inline"
                                        UpDownButtonAlignment="Both" />
            </StackLayout>
            
            <!-- Custom Color -->
            <StackLayout Spacing="5">
                <Label Text="Custom Color (Blue):" FontAttributes="Bold" />
                <editors:SfNumericEntry WidthRequest="250"
                                        Value="50"
                                        UpDownPlacementMode="Inline"
                                        UpDownButtonColor="Blue" />
            </StackLayout>
            
            <!-- Auto Reverse -->
            <StackLayout Spacing="5">
                <Label Text="Auto Reverse (0-10, wraps):" FontAttributes="Bold" />
                <editors:SfNumericEntry WidthRequest="250"
                                        Value="0"
                                        Minimum="0"
                                        Maximum="10"
                                        AutoReverse="True"
                                        UpDownPlacementMode="Inline"
                                        CustomFormat="N0" />
            </StackLayout>
            
            <!-- Time Picker -->
            <StackLayout Spacing="5">
                <Label Text="Time (Hour : Minute):" FontAttributes="Bold" />
                <HorizontalStackLayout Spacing="10">
                    <editors:SfNumericEntry WidthRequest="100"
                                            Value="12"
                                            Minimum="0"
                                            Maximum="23"
                                            AutoReverse="True"
                                            UpDownPlacementMode="Inline"
                                            CustomFormat="00" />
                    <Label Text=":" VerticalOptions="Center" FontSize="20" />
                    <editors:SfNumericEntry WidthRequest="100"
                                            Value="30"
                                            Minimum="0"
                                            Maximum="59"
                                            AutoReverse="True"
                                            UpDownPlacementMode="Inline"
                                            CustomFormat="00" />
                </HorizontalStackLayout>
            </StackLayout>
            
        </VerticalStackLayout>
    </ScrollView>
</ContentPage>
```

## Summary

This reference covered:
- ✅ Value changes with SmallChange and LargeChange
- ✅ Keyboard (arrows, Page Up/Down) and mouse scrolling
- ✅ UpDown button placement (Hidden, Inline, InlineVertical)
- ✅ Button alignment (Left, Right, Both)
- ✅ Button order (UpThenDown, DownThenUp)
- ✅ Button color customization
- ✅ Custom button templates
- ✅ Auto-reverse behavior at min/max limits

**Next:** Read [liquid-glass-effect.md](liquid-glass-effect.md) for modern translucent design (.NET 10+).
