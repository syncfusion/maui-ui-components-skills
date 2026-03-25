# Thumbs and Overlays in .NET MAUI Range Slider

## Table of Contents
- [Overview](#overview)
- [Thumb Size](#thumb-size)
- [Custom Thumb Icons](#custom-thumb-icons)
- [Thumb Colors](#thumb-colors)
- [Thumb Stroke](#thumb-stroke)
- [Overlapping Thumb Stroke](#overlapping-thumb-stroke)
- [Thumb Overlay](#thumb-overlay)
- [Disabled Thumbs](#disabled-thumbs)

## Overview

The thumb is the draggable element that users interact with to select range values. The range slider has two thumbs (start and end). The thumb overlay provides visual feedback during interaction.

**Key elements**:
- **Thumb**: The circular handle users drag to change values
- **Thumb Overlay**: Visual feedback circle that appears when interacting with the thumb

## Thumb Size

Control thumb size using the `Radius` property in `ThumbStyle`.

### Default Size
- **Radius**: 10.0 (default)

### Setting Thumb Size

**XAML:**
```xaml
<sliders:SfRangeSlider>
    <sliders:SfRangeSlider.ThumbStyle>
        <sliders:SliderThumbStyle Radius="15" />
    </sliders:SfRangeSlider.ThumbStyle>
</sliders:SfRangeSlider>
```

**C#:**
```csharp
SfRangeSlider rangeSlider = new SfRangeSlider();
rangeSlider.ThumbStyle.Radius = 15;
```

### Larger Thumbs for Touch

For better touch accessibility:

```csharp
rangeSlider.ThumbStyle.Radius = 18;  // Larger, easier to tap
```

### Smaller Thumbs for Dense UI

```csharp
rangeSlider.ThumbStyle.Radius = 8;  // Compact appearance
```

## Custom Thumb Icons

Replace default circular thumbs with custom views using `StartThumbIcon` and `EndThumbIcon`.

### StartThumbIcon

Set custom view for the start (left/bottom) thumb:

**XAML:**
```xaml
<sliders:SfRangeSlider>
    <sliders:SfRangeSlider.StartThumbIcon>
        <Rectangle Fill="Blue" 
                   HeightRequest="35" 
                   WidthRequest="35" />
    </sliders:SfRangeSlider.StartThumbIcon>
</sliders:SfRangeSlider>
```

**C#:**
```csharp
rangeSlider.StartThumbIcon = new Microsoft.Maui.Controls.Shapes.Rectangle
{
    Fill = Brush.Blue,
    HeightRequest = 35,
    WidthRequest = 35
};
```

### EndThumbIcon

Set custom view for the end (right/top) thumb:

**XAML:**
```xaml
<sliders:SfRangeSlider>
    <sliders:SfRangeSlider.EndThumbIcon>
        <Rectangle Fill="Red" 
                   HeightRequest="35" 
                   WidthRequest="35" />
    </sliders:SfRangeSlider.EndThumbIcon>
</sliders:SfRangeSlider>
```

**C#:**
```csharp
rangeSlider.EndThumbIcon = new Microsoft.Maui.Controls.Shapes.Rectangle
{
    Fill = Brush.Red,
    HeightRequest = 35,
    WidthRequest = 35
};
```

### Custom Icons with Images

Use `Image` control for custom icons:

**XAML:**
```xaml
<sliders:SfRangeSlider>
    <sliders:SfRangeSlider.StartThumbIcon>
        <Image Source="thumb_left.png"
               HeightRequest="30"
               WidthRequest="30" />
    </sliders:SfRangeSlider.StartThumbIcon>
    
    <sliders:SfRangeSlider.EndThumbIcon>
        <Image Source="thumb_right.png"
               HeightRequest="30"
               WidthRequest="30" />
    </sliders:SfRangeSlider.EndThumbIcon>
</sliders:SfRangeSlider>
```

### Complex Custom Views

Use any .NET MAUI view:

```xaml
<sliders:SfRangeSlider>
    <sliders:SfRangeSlider.StartThumbIcon>
        <Grid HeightRequest="40" WidthRequest="40">
            <Ellipse Fill="White"
                     Stroke="Blue"
                     StrokeThickness="3" />
            <Label Text="A"
                   HorizontalOptions="Center"
                   VerticalOptions="Center"
                   FontAttributes="Bold" />
        </Grid>
    </sliders:SfRangeSlider.StartThumbIcon>
</sliders:SfRangeSlider>
```

## Thumb Colors

Customize thumb fill color using the `Fill` property in `ThumbStyle`.

### Setting Thumb Color

**XAML:**
```xaml
<sliders:SfRangeSlider>
    <sliders:SfRangeSlider.ThumbStyle>
        <sliders:SliderThumbStyle Fill="#FF5722" />
    </sliders:SfRangeSlider.ThumbStyle>
</sliders:SfRangeSlider>
```

**C#:**
```csharp
rangeSlider.ThumbStyle.Fill = new SolidColorBrush(Color.FromArgb("#FF5722"));
```

### Matching Theme Colors

```csharp
// Use app theme colors
rangeSlider.ThumbStyle.Fill = new SolidColorBrush(
    Application.Current.RequestedTheme == AppTheme.Dark 
        ? Colors.White 
        : Colors.Black
);
```

## Thumb Stroke

Add borders to thumbs using `Stroke` and `StrokeThickness` properties.

### Setting Thumb Stroke

**XAML:**
```xaml
<sliders:SfRangeSlider>
    <sliders:SfRangeSlider.ThumbStyle>
        <sliders:SliderThumbStyle Fill="White"
                                  Stroke="#2196F3"
                                  StrokeThickness="3" />
    </sliders:SfRangeSlider.ThumbStyle>
</sliders:SfRangeSlider>
```

**C#:**
```csharp
rangeSlider.ThumbStyle.Fill = new SolidColorBrush(Colors.White);
rangeSlider.ThumbStyle.Stroke = new SolidColorBrush(Color.FromArgb("#2196F3"));
rangeSlider.ThumbStyle.StrokeThickness = 3;
```

### Hollow Thumbs

Create hollow circles with transparent fill:

```csharp
rangeSlider.ThumbStyle.Fill = new SolidColorBrush(Colors.Transparent);
rangeSlider.ThumbStyle.Stroke = new SolidColorBrush(Colors.Blue);
rangeSlider.ThumbStyle.StrokeThickness = 4;
rangeSlider.ThumbStyle.Radius = 12;
```

## Overlapping Thumb Stroke

When both thumbs overlap (same value), customize the stroke color using `OverlapStroke`.

### Setting Overlap Stroke

**XAML:**
```xaml
<sliders:SfRangeSlider>
    <sliders:SfRangeSlider.ThumbStyle>
        <sliders:SliderThumbStyle Fill="White"
                                  Stroke="#2196F3"
                                  StrokeThickness="3"
                                  OverlapStroke="#FFD700" />
    </sliders:SfRangeSlider.ThumbStyle>
</sliders:SfRangeSlider>
```

**C#:**
```csharp
rangeSlider.ThumbStyle.OverlapStroke = new SolidColorBrush(Color.FromArgb("#FFD700"));
```

**Use case**: Provides visual feedback when users drag thumbs to the same position, preventing confusion about thumb positions.

## Thumb Overlay

The overlay is a semi-transparent circle that appears when the user interacts with a thumb, providing visual feedback.

### Overlay Size

Control overlay radius using the `Radius` property in `ThumbOverlayStyle`.

**Default**: 24.0

**XAML:**
```xaml
<sliders:SfRangeSlider>
    <sliders:SfRangeSlider.ThumbOverlayStyle>
        <sliders:SliderThumbOverlayStyle Radius="20" />
    </sliders:SfRangeSlider.ThumbOverlayStyle>
</sliders:SfRangeSlider>
```

**C#:**
```csharp
rangeSlider.ThumbOverlayStyle.Radius = 20;
```

### Overlay Color

Customize overlay color using the `Fill` property.

**XAML:**
```xaml
<sliders:SfRangeSlider>
    <sliders:SfRangeSlider.ThumbOverlayStyle>
        <sliders:SliderThumbOverlayStyle Fill="#332196F3" />
    </sliders:SfRangeSlider.ThumbOverlayStyle>
</sliders:SfRangeSlider>
```

**C#:**
```csharp
rangeSlider.ThumbOverlayStyle.Fill = new SolidColorBrush(Color.FromArgb("#332196F3"));
```

**Tip**: Use semi-transparent colors (with alpha channel) for subtle feedback.

### Complete Overlay Customization

```csharp
rangeSlider.ThumbOverlayStyle.Radius = 25;
rangeSlider.ThumbOverlayStyle.Fill = new SolidColorBrush(
    Color.FromRgba(33, 150, 243, 0.2)  // Blue with 20% opacity
);
```

## Disabled Thumbs

Customize thumb appearance for disabled state using Visual State Manager.

### Complete VSM Example

**XAML:**
```xaml
<ContentPage.Resources>
    <Style TargetType="sliders:SfRangeSlider">
        <Setter Property="Interval" Value="0.25" />
        <Setter Property="VisualStateManager.VisualStateGroups">
            <VisualStateGroupList>
                <VisualStateGroup>
                    <!-- Default (Enabled) State -->
                    <VisualState x:Name="Default">
                        <VisualState.Setters>
                            <Setter Property="ThumbStyle">
                                <Setter.Value>
                                    <sliders:SliderThumbStyle Radius="13"
                                                              Fill="#2196F3"
                                                              Stroke="White"
                                                              StrokeThickness="3" />
                                </Setter.Value>
                            </Setter>
                        </VisualState.Setters>
                    </VisualState>
                    
                    <!-- Disabled State -->
                    <VisualState x:Name="Disabled">
                        <VisualState.Setters>
                            <Setter Property="ThumbStyle">
                                <Setter.Value>
                                    <sliders:SliderThumbStyle Radius="13"
                                                              Fill="Gray"
                                                              Stroke="LightGray"
                                                              StrokeThickness="3" />
                                </Setter.Value>
                            </Setter>
                            <Setter Property="TrackStyle">
                                <Setter.Value>
                                    <sliders:SliderTrackStyle ActiveFill="Gray"
                                                              InactiveFill="LightGray" />
                                </Setter.Value>
                            </Setter>
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateGroupList>
        </Setter>
    </Style>
</ContentPage.Resources>

<VerticalStackLayout>
    <Label Text="Enabled" Padding="24,10" />
    <sliders:SfRangeSlider />
    
    <Label Text="Disabled" Padding="24,10" />
    <sliders:SfRangeSlider IsEnabled="False" />
</VerticalStackLayout>
```

**C#:**
```csharp
VerticalStackLayout stackLayout = new VerticalStackLayout();
SfRangeSlider enabledSlider = new SfRangeSlider();
SfRangeSlider disabledSlider = new SfRangeSlider { IsEnabled = false };

VisualStateGroupList visualStateGroupList = new VisualStateGroupList();
VisualStateGroup commonStateGroup = new VisualStateGroup();

// Default State
VisualState defaultState = new VisualState { Name = "Default" };
defaultState.Setters.Add(new Setter
{
    Property = SfRangeSlider.ThumbStyleProperty,
    Value = new SliderThumbStyle
    {
        Radius = 13,
        Fill = Color.FromArgb("#2196F3"),
        Stroke = Colors.White,
        StrokeThickness = 3
    }
});

// Disabled State
VisualState disabledState = new VisualState { Name = "Disabled" };
disabledState.Setters.Add(new Setter
{
    Property = SfRangeSlider.ThumbStyleProperty,
    Value = new SliderThumbStyle
    {
        Radius = 13,
        Fill = Colors.Gray,
        Stroke = Colors.LightGray,
        StrokeThickness = 3
    }
});
disabledState.Setters.Add(new Setter
{
    Property = SfRangeSlider.TrackStyleProperty,
    Value = new SliderTrackStyle
    {
        ActiveFill = Colors.Gray,
        InactiveFill = Colors.LightGray
    }
});

commonStateGroup.States.Add(defaultState);
commonStateGroup.States.Add(disabledState);
visualStateGroupList.Add(commonStateGroup);

VisualStateManager.SetVisualStateGroups(enabledSlider, visualStateGroupList);
VisualStateManager.SetVisualStateGroups(disabledSlider, visualStateGroupList);

stackLayout.Children.Add(new Label { Text = "Enabled", Padding = new Thickness(24, 10) });
stackLayout.Children.Add(enabledSlider);
stackLayout.Children.Add(new Label { Text = "Disabled", Padding = new Thickness(24, 10) });
stackLayout.Children.Add(disabledSlider);

this.Content = stackLayout;
```

## Common Scenarios

### Material Design Style Thumbs

```csharp
rangeSlider.ThumbStyle.Fill = new SolidColorBrush(Color.FromArgb("#2196F3"));
rangeSlider.ThumbStyle.Radius = 10;
rangeSlider.ThumbOverlayStyle.Fill = new SolidColorBrush(Color.FromRgba(33, 150, 243, 0.16));
rangeSlider.ThumbOverlayStyle.Radius = 24;
```

### iOS-Style Thumbs

```csharp
rangeSlider.ThumbStyle.Fill = new SolidColorBrush(Colors.White);
rangeSlider.ThumbStyle.Stroke = new SolidColorBrush(Colors.LightGray);
rangeSlider.ThumbStyle.StrokeThickness = 0.5;
rangeSlider.ThumbStyle.Radius = 14;
```

### High Contrast for Accessibility

```csharp
rangeSlider.ThumbStyle.Fill = new SolidColorBrush(Colors.White);
rangeSlider.ThumbStyle.Stroke = new SolidColorBrush(Colors.Black);
rangeSlider.ThumbStyle.StrokeThickness = 3;
rangeSlider.ThumbStyle.Radius = 15;
```

## Best Practices

1. **Minimum touch target**: Keep radius >= 10 for easy thumb interaction (44x44 pts minimum on iOS)
2. **Contrast**: Ensure thumb stands out from track background
3. **Overlay feedback**: Use semi-transparent overlays for subtle interaction feedback
4. **Stroke borders**: Add strokes to improve visibility on varied backgrounds
5. **Custom icons**: Match your app's design language with custom thumb icons
6. **Disabled state**: Provide clear visual distinction for disabled thumbs
7. **Test on devices**: Verify thumb size is comfortable on actual devices

## Related Properties

- **Track**: See [track.md](track.md) for track customization
- **Selection**: See [intervals-and-selection.md](intervals-and-selection.md) for drag behavior
- **Events**: See [events-and-commands.md](events-and-commands.md) for handling thumb interactions
