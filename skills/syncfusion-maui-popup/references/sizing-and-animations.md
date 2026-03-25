# Popup Sizing and Animations

## Table of Contents
- [Overview](#overview)
- [Popup Sizing](#popup-sizing)
- [Auto-Sizing Modes](#auto-sizing-modes)
- [Full-Screen Mode](#full-screen-mode)
- [Animation Types](#animation-types)
- [Animation Configuration](#animation-configuration)
- [Custom Animations](#custom-animations)
- [Best Practices](#best-practices)

## Overview

Control the size and appearance animations of your popups to create polished, professional user experiences. The SfPopup provides:
- Fixed width and height configuration
- Auto-sizing based on content
- Full-screen mode for modal experiences
- Built-in animation types (zoom, fade, slide)
- Configurable animation duration and easing

## Popup Sizing

### Fixed Width and Height

Set explicit dimensions using `HeightRequest` and `WidthRequest` properties.

**XAML:**
```xml
<sfPopup:SfPopup x:Name="sfPopup"
                 HeightRequest="300"
                 WidthRequest="400">
</sfPopup:SfPopup>
```

**C#:**
```csharp
sfPopup.HeightRequest = 300;
sfPopup.WidthRequest = 400;
sfPopup.Show();
```

### Responsive Sizing

Use percentages of screen dimensions for responsive design:

```csharp
private void ShowResponsivePopup()
{
    var displayInfo = DeviceDisplay.MainDisplayInfo;
    var screenWidth = displayInfo.Width / displayInfo.Density;
    var screenHeight = displayInfo.Height / displayInfo.Density;
    
    // 80% of screen width, 60% of screen height
    sfPopup.WidthRequest = screenWidth * 0.8;
    sfPopup.HeightRequest = screenHeight * 0.6;
    sfPopup.Show();
}
```

### Device-Specific Sizing

```csharp
private void ConfigurePopupSize()
{
    if (DeviceInfo.Platform == DevicePlatform.Android || 
        DeviceInfo.Platform == DevicePlatform.iOS)
    {
        // Mobile: Larger popup
        sfPopup.WidthRequest = 320;
        sfPopup.HeightRequest = 400;
    }
    else
    {
        // Desktop: Smaller, more compact
        sfPopup.WidthRequest = 400;
        sfPopup.HeightRequest = 300;
    }
    
    sfPopup.Show();
}
```

### Minimum and Maximum Sizes

Combine sizing with constraints:

```xml
<sfPopup:SfPopup x:Name="sfPopup"
                 MinimumHeightRequest="200"
                 MinimumWidthRequest="300"
                 MaximumHeightRequest="600"
                 MaximumWidthRequest="800">
    <sfPopup:SfPopup.ContentTemplate>
        <DataTemplate>
            <ScrollView>
                <StackLayout Padding="20">
                    <!-- Content that may vary in size -->
                </StackLayout>
            </ScrollView>
        </DataTemplate>
    </sfPopup:SfPopup.ContentTemplate>
</sfPopup:SfPopup>
```

## Auto-Sizing Modes

The `AutoSizeMode` property controls how the popup automatically adjusts its dimensions based on content.

### AutoSizeMode Options

| Mode | Description |
|------|-------------|
| `None` | Uses fixed `HeightRequest` and `WidthRequest` |
| `Height` | Width is fixed, height adjusts to content |
| `Width` | Height is fixed, width adjusts to content |
| `Both` | Both dimensions adjust to content |

### Auto-Size by Height

Width is fixed, height adjusts to fit content.

**XAML:**
```xml
<sfPopup:SfPopup x:Name="sfPopup"
                 WidthRequest="350"
                 AutoSizeMode="Height">
    <sfPopup:SfPopup.ContentTemplate>
        <DataTemplate>
            <StackLayout Padding="20">
                <Label Text="This is dynamic content that determines the height." />
                <Label Text="Add more content and the popup will grow." />
                <Label Text="The width remains fixed at 350." />
            </StackLayout>
        </DataTemplate>
    </sfPopup:SfPopup.ContentTemplate>
</sfPopup:SfPopup>
```

**C#:**
```csharp
sfPopup.WidthRequest = 350;
sfPopup.AutoSizeMode = PopupAutoSizeMode.Height;
sfPopup.Show();
```

### Auto-Size by Width

Height is fixed, width adjusts to fit content.

**XAML:**
```xml
<sfPopup:SfPopup x:Name="sfPopup"
                 HeightRequest="200"
                 AutoSizeMode="Width">
    <sfPopup:SfPopup.ContentTemplate>
        <DataTemplate>
            <StackLayout Padding="20">
                <Label Text="Width adjusts to this content" />
            </StackLayout>
        </DataTemplate>
    </sfPopup:SfPopup.ContentTemplate>
</sfPopup:SfPopup>
```

**C#:**
```csharp
sfPopup.HeightRequest = 200;
sfPopup.AutoSizeMode = PopupAutoSizeMode.Width;
sfPopup.Show();
```

### Auto-Size Both Dimensions

Both width and height adjust to content.

**XAML:**
```xml
<sfPopup:SfPopup x:Name="sfPopup"
                 AutoSizeMode="Both">
    <sfPopup:SfPopup.ContentTemplate>
        <DataTemplate>
            <StackLayout Padding="20" Spacing="10">
                <Label Text="Popup size adjusts" 
                       FontSize="16" 
                       FontAttributes="Bold" />
                <Label Text="to fit this content" />
                <Button Text="OK" WidthRequest="100" />
            </StackLayout>
        </DataTemplate>
    </sfPopup:SfPopup.ContentTemplate>
</sfPopup:SfPopup>
```

**C#:**
```csharp
sfPopup.AutoSizeMode = PopupAutoSizeMode.Both;
sfPopup.Show();
```

### No Auto-Sizing (Default)

Use fixed dimensions only.

```csharp
sfPopup.AutoSizeMode = PopupAutoSizeMode.None;
sfPopup.HeightRequest = 300;
sfPopup.WidthRequest = 400;
sfPopup.Show();
```

### Auto-Size with Constraints

```xml
<sfPopup:SfPopup x:Name="sfPopup"
                 AutoSizeMode="Both"
                 MinimumHeightRequest="150"
                 MinimumWidthRequest="250"
                 MaximumHeightRequest="500"
                 MaximumWidthRequest="600">
    <sfPopup:SfPopup.ContentTemplate>
        <DataTemplate>
            <StackLayout Padding="20">
                <!-- Content will size between min and max bounds -->
            </StackLayout>
        </DataTemplate>
    </sfPopup:SfPopup.ContentTemplate>
</sfPopup:SfPopup>
```

## Full-Screen Mode

Display the popup covering the entire screen.

### Using IsFullScreen Property

**XAML:**
```xml
<sfPopup:SfPopup x:Name="sfPopup" 
                 IsFullScreen="True">
</sfPopup:SfPopup>
```

**C#:**
```csharp
sfPopup.IsFullScreen = true;
sfPopup.Show();
```

### Using Show(bool) Method

```csharp
// Show in full-screen mode
sfPopup.Show(isFullScreen: true);
```

### Full-Screen with Close Button

```xml
<sfPopup:SfPopup x:Name="fullScreenPopup"
                 IsFullScreen="True"
                 ShowCloseButton="True"
                 HeaderTitle="Full-Screen Form">
    <sfPopup:SfPopup.ContentTemplate>
        <DataTemplate>
            <ScrollView>
                <StackLayout Padding="20" Spacing="15">
                    <Label Text="Registration Form" 
                           FontSize="24" 
                           FontAttributes="Bold" />
                    
                    <Entry Placeholder="First Name" />
                    <Entry Placeholder="Last Name" />
                    <Entry Placeholder="Email" Keyboard="Email" />
                    <Entry Placeholder="Password" IsPassword="True" />
                    <Editor Placeholder="Bio" HeightRequest="100" />
                    
                    <Button Text="Submit" 
                            BackgroundColor="#6750A4" 
                            TextColor="White"
                            Margin="0,20,0,0" />
                </StackLayout>
            </ScrollView>
        </DataTemplate>
    </sfPopup:SfPopup.ContentTemplate>
</sfPopup:SfPopup>
```

### Toggle Full-Screen Dynamically

```csharp
private bool isFullScreen = false;

private void OnToggleSize_Clicked(object sender, EventArgs e)
{
    isFullScreen = !isFullScreen;
    sfPopup.IsFullScreen = isFullScreen;
    
    if (!sfPopup.IsOpen)
    {
        sfPopup.Show();
    }
}
```

## Animation Types

The `AnimationMode` property controls the popup's entry and exit animations.

### Available Animation Modes

| Mode | Description |
|------|-------------|
| `Zoom` | Scale from small to normal size (default) |
| `Fade` | Fade in/out with opacity change |
| `SlideOnLeft` | Slide in from left side |
| `SlideOnRight` | Slide in from right side |
| `SlideOnTop` | Slide in from top |
| `SlideOnBottom` | Slide in from bottom |
| `None` | No animation |

### Zoom Animation

**XAML:**
```xml
<sfPopup:SfPopup x:Name="sfPopup"
                 AnimationMode="Zoom">
</sfPopup:SfPopup>
```

**C#:**
```csharp
sfPopup.AnimationMode = PopupAnimationMode.Zoom;
sfPopup.Show();
```

### Fade Animation

**XAML:**
```xml
<sfPopup:SfPopup x:Name="sfPopup"
                 AnimationMode="Fade">
</sfPopup:SfPopup>
```

**C#:**
```csharp
sfPopup.AnimationMode = PopupAnimationMode.Fade;
sfPopup.Show();
```

### Slide Animations

**Slide from Left:**
```csharp
sfPopup.AnimationMode = PopupAnimationMode.SlideOnLeft;
sfPopup.Show();
```

**Slide from Right:**
```csharp
sfPopup.AnimationMode = PopupAnimationMode.SlideOnRight;
sfPopup.Show();
```

**Slide from Top:**
```csharp
sfPopup.AnimationMode = PopupAnimationMode.SlideOnTop;
sfPopup.Show();
```

**Slide from Bottom:**
```csharp
sfPopup.AnimationMode = PopupAnimationMode.SlideOnBottom;
sfPopup.Show();
```

### No Animation

```csharp
sfPopup.AnimationMode = PopupAnimationMode.None;
sfPopup.Show();
```

### Context-Aware Animations

Choose animations based on positioning:

```csharp
private void ShowContextPopup(Button targetButton)
{
    // Slide from the side the button is on
    var buttonX = targetButton.X;
    var screenWidth = DeviceDisplay.MainDisplayInfo.Width / DeviceDisplay.MainDisplayInfo.Density;
    
    if (buttonX < screenWidth / 2)
    {
        // Button on left side, slide from left
        sfPopup.AnimationMode = PopupAnimationMode.SlideOnLeft;
    }
    else
    {
        // Button on right side, slide from right
        sfPopup.AnimationMode = PopupAnimationMode.SlideOnRight;
    }
    
    sfPopup.ShowRelativeToView(targetButton, PopupRelativePosition.AlignBottom);
}
```

## Animation Configuration

### Animation Duration

Control how long animations take using the `AnimationDuration` property (in milliseconds).

**XAML:**
```xml
<sfPopup:SfPopup x:Name="sfPopup"
                 AnimationMode="Zoom"
                 AnimationDuration="500">
</sfPopup:SfPopup>
```

**C#:**
```csharp
sfPopup.AnimationMode = PopupAnimationMode.Zoom;
sfPopup.AnimationDuration = 500; // 0.5 seconds
sfPopup.Show();
```

### Fast Animation

```csharp
sfPopup.AnimationDuration = 200; // Quick, snappy animation
sfPopup.Show();
```

### Slow Animation

```csharp
sfPopup.AnimationDuration = 1000; // Slow, smooth animation
sfPopup.Show();
```

### Animation Easing

Control the animation curve using the `AnimationEasing` property.

**Available Easing Functions:**
- `Linear`: use a constant velocity to animate the view and is the default type.
- `SinIn`: smoothly accelerate the animation to its final value.
- `SinOut`: smoothly decelerate the animation to its final value.
- `SinInOut`: smoothly accelerate the animation at the beginning and then smoothly decelerates the animation at the end.

**XAML:**
```xml
<sfPopup:SfPopup x:Name="sfPopup"
                 AnimationMode="Zoom"
                 AnimationDuration="400"
                 AnimationEasing="SinIn">
</sfPopup:SfPopup>
```

**C#:**
```csharp
sfPopup.AnimationMode = PopupAnimationMode.Zoom;
sfPopup.AnimationDuration = 400;
sfPopup.AnimationEasing = PopupAnimationEasing.SinIn;
sfPopup.Show();
```

## Custom Animations

### Sequential Animations

Animate popup opening followed by content animation:

```csharp
private async void ShowWithSequentialAnimation()
{
    // Show popup
    sfPopup.AnimationMode = PopupAnimationMode.Zoom;
    sfPopup.AnimationDuration = 300;
    sfPopup.Show();
    
    // Wait for popup animation
    await Task.Delay(300);
    
    // Animate content inside popup
    var contentView = FindContentView(sfPopup);
    if (contentView != null)
    {
        await contentView.ScaleTo(1.1, 200);
        await contentView.ScaleTo(1.0, 200);
    }
}
```

### Conditional Animations

Different animations based on context:

```csharp
private void ShowNotification(bool isSuccess)
{
    if (isSuccess)
    {
        // Success: Smooth fade from top
        sfPopup.AnimationMode = PopupAnimationMode.SlideOnTop;
        sfPopup.AnimationDuration = 300;
        sfPopup.AnimationEasing = Easing.SinOut;
        sfPopup.Message = "✓ Operation successful";
    }
    else
    {
        // Error: Quick attention-grabbing zoom
        sfPopup.AnimationMode = PopupAnimationMode.Zoom;
        sfPopup.AnimationDuration = 200;
        sfPopup.AnimationEasing = Easing.BounceOut;
        sfPopup.Message = "⚠ Operation failed";
    }
    
    sfPopup.Show();
}
```

## Best Practices

### Sizing Best Practices

1. **Use Auto-Sizing for Dynamic Content:**
   ```csharp
   // Content size may vary
   sfPopup.AutoSizeMode = PopupAutoSizeMode.Both;
   sfPopup.MinimumHeightRequest = 150;
   sfPopup.MaximumHeightRequest = 600;
   ```

2. **Fixed Sizing for Consistent Experience:**
   ```csharp
   // Predictable layout
   sfPopup.HeightRequest = 300;
   sfPopup.WidthRequest = 400;
   sfPopup.AutoSizeMode = PopupAutoSizeMode.None;
   ```

3. **Responsive Design:**
   ```csharp
   // Adapt to screen size
   var screenWidth = DeviceDisplay.MainDisplayInfo.Width / DeviceDisplay.MainDisplayInfo.Density;
   sfPopup.WidthRequest = Math.Min(400, screenWidth * 0.9);
   ```

4. **Full-Screen for Complex Forms:**
   ```csharp
   // Large forms or content
   sfPopup.IsFullScreen = true;
   ```

### Animation Best Practices

1. **Match Animation to Context:**
   - **Zoom**: Alerts, confirmations (draws attention)
   - **Fade**: Subtle, non-intrusive notifications
   - **Slide**: Contextual menus, tooltips (shows direction)

2. **Keep Duration Short:**
   ```csharp
   // 200-400ms feels responsive
   sfPopup.AnimationDuration = 300;
   ```

3. **Avoid Long Animations:**
   ```csharp
   // Don't: Slow animations frustrate users
   sfPopup.AnimationDuration = 1500; // Too slow!
   ```

4. **Use Appropriate Easing:**
   ```csharp
   // Smooth, natural feel
   sfPopup.AnimationEasing = Easing.CubicOut;
   
   // Playful, attention-grabbing
   sfPopup.AnimationEasing = Easing.BounceOut;
   ```

5. **Consider Accessibility:**
   ```csharp
   // Respect user preferences for reduced motion
   if (ShouldReduceMotion())
   {
       sfPopup.AnimationMode = PopupAnimationMode.None;
   }
   else
   {
       sfPopup.AnimationMode = PopupAnimationMode.Zoom;
       sfPopup.AnimationDuration = 300;
   }
   ```

6. **Test on Different Devices:**
   - Animations may feel different on various devices
   - Test on low-end and high-end devices
   - Ensure smooth performance across platforms

### Performance Tips

1. **Avoid Heavy Content During Animation:**
   - Load complex content after animation completes
   - Use placeholders during animation

2. **Reuse Popup Instances:**
   ```csharp
   // Don't create new popups repeatedly
   // Reuse and reconfigure instead
   private SfPopup _reuseablePopup;
   
   private void ShowPopup(string message)
   {
       _reuseablePopup ??= new SfPopup();
       _reuseablePopup.Message = message;
       _reuseablePopup.Show();
   }
   ```

3. **Optimize Content Templates:**
   - Keep content template lightweight
   - Defer loading of heavy resources
   - Use virtualization for lists

### Example: Comprehensive Configuration

```csharp
private void ShowWellConfiguredPopup()
{
    var displayInfo = DeviceDisplay.MainDisplayInfo;
    var screenWidth = displayInfo.Width / displayInfo.Density;
    
    sfPopup.WidthRequest = Math.Min(400, screenWidth * 0.85);
    sfPopup.AutoSizeMode = PopupAutoSizeMode.Height;
    sfPopup.MinimumHeightRequest = 200;
    sfPopup.MaximumHeightRequest = 600;
    
    sfPopup.AnimationMode = PopupAnimationMode.Zoom;
    sfPopup.AnimationDuration = 300;
    sfPopup.AnimationEasing = Easing.CubicOut;
    
    sfPopup.ShowCloseButton = true;
    sfPopup.Show();
}
```
