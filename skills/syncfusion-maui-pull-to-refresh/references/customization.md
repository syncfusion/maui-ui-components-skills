# Customization in .NET MAUI PullToRefresh

## Table of Contents
- [Overview](#overview)
- [TransitionMode](#transitionmode)
- [Threshold Properties](#threshold-properties)
- [Progress Indicator Customization](#progress-indicator-customization)
- [Refresh View Dimensions](#refresh-view-dimensions)
- [IsRefreshing Property](#isrefreshing-property)
- [Programmatic Support](#programmatic-support)
- [Size and Layout Considerations](#size-and-layout-considerations)
- [Complete Customization Example](#complete-customization-example)
- [Property Reference Quick Guide](#property-reference-quick-guide)

## Overview

The .NET MAUI PullToRefresh control offers extensive customization options for appearance, behavior, and interaction. You can customize transition modes, thresholds, progress indicator colors and dimensions, and control refresh states programmatically.

## TransitionMode

The `TransitionMode` property specifies how the refresh indicator animates during pull and refresh actions.

### Available Modes

1. **SlideOnTop** (Default): Refresh view slides on top of the pullable content
2. **Push**: Refresh view pushes the pullable content down

### SlideOnTop Mode

The refresh indicator appears above the content, overlaying it during the pull gesture.

```xml
<syncfusion:SfPullToRefresh x:Name="pullToRefresh" 
                             TransitionMode="SlideOnTop"
                             Refreshing="OnRefreshing">
    <syncfusion:SfPullToRefresh.PullableContent>
        <StackLayout BackgroundColor="LightBlue">
            <Label Text="Content with SlideOnTop transition" 
                   Padding="20"/>
        </StackLayout>
    </syncfusion:SfPullToRefresh.PullableContent>
</syncfusion:SfPullToRefresh>
```

```csharp
// C# code
pullToRefresh.TransitionMode = PullToRefreshTransitionType.SlideOnTop;
```

**Visual Behavior:**
- Refresh view appears from above
- Content remains in place during pull
- Progress indicator overlays the content
- Best for minimizing content shift

### Push Mode

The refresh indicator pushes the content down as it appears, moving both simultaneously.

```xml
<syncfusion:SfPullToRefresh x:Name="pullToRefresh" 
                             TransitionMode="Push"
                             Refreshing="OnRefreshing">
    <syncfusion:SfPullToRefresh.PullableContent>
        <StackLayout BackgroundColor="LightGreen">
            <Label Text="Content with Push transition" 
                   Padding="20"/>
        </StackLayout>
    </syncfusion:SfPullToRefresh.PullableContent>
</syncfusion:SfPullToRefresh>
```

```csharp
// C# code
pullToRefresh.TransitionMode = PullToRefreshTransitionType.Push;
```

**Visual Behavior:**
- Refresh view and content move together
- Content pushes down as you pull
- More integrated visual experience
- Best for showing clear separation

### Choosing the Right Mode

| Scenario | Recommended Mode |
|----------|------------------|
| Overlay effect desired | SlideOnTop |
| Content should move with indicator | Push |
| Minimal content disruption | SlideOnTop |
| Clear visual feedback | Push |
| Default/standard behavior | SlideOnTop |

## Threshold Properties

Threshold properties control when and how far the refresh indicator appears during pull gestures.

### RefreshViewThreshold

The starting position of the progress indicator within the refresh view. This determines where the indicator begins to appear.

```xml
<syncfusion:SfPullToRefresh x:Name="pullToRefresh" 
                             RefreshViewThreshold="50">
    <!-- PullableContent -->
</syncfusion:SfPullToRefresh>
```

```csharp
pullToRefresh.RefreshViewThreshold = 50d;
```

**Purpose:**
- Sets the initial visibility point of the indicator
- Lower values: Indicator appears sooner
- Higher values: More pull required before indicator shows
- Default: 30

### PullingThreshold

The maximum pulling distance for the progress indicator. This defines how far users can pull down.

```xml
<syncfusion:SfPullToRefresh x:Name="pullToRefresh" 
                             PullingThreshold="200">
    <!-- PullableContent -->
</syncfusion:SfPullToRefresh>
```

```csharp
pullToRefresh.PullingThreshold = 200d;
```

**Purpose:**
- Defines the maximum pull distance
- Higher values: More pull distance allowed
- Lower values: Refresh triggers sooner
- Affects the sensitivity of the pull gesture
- Default: 150

### Threshold Configuration Example

```xml
<syncfusion:SfPullToRefresh x:Name="pullToRefresh"
                             RefreshViewThreshold="30"
                             PullingThreshold="120"
                             RefreshViewHeight="40"
                             RefreshViewWidth="40">
    <syncfusion:SfPullToRefresh.PullableContent>
        <Label Text="Customized thresholds" />
    </syncfusion:SfPullToRefresh.PullableContent>
</syncfusion:SfPullToRefresh>
```

**Result:**
- Indicator starts appearing at 30 units
- Maximum pull distance is 120 units
- Provides a responsive, controlled pull experience

## Progress Indicator Customization

Customize the appearance of the circular progress indicator that appears during refresh.

### ProgressColor

Sets the color of the progress indicator's arc.

```xml
<syncfusion:SfPullToRefresh x:Name="pullToRefresh" 
                             ProgressColor="Blue">
    <!-- PullableContent -->
</syncfusion:SfPullToRefresh>
```

```csharp
pullToRefresh.ProgressColor = Colors.Blue;
```

### ProgressBackground

Sets the background color of the progress indicator.

```xml
<syncfusion:SfPullToRefresh x:Name="pullToRefresh" 
                             ProgressBackground="White">
    <!-- PullableContent -->
</syncfusion:SfPullToRefresh>
```

```csharp
pullToRefresh.ProgressBackground = Colors.White;
```

### ProgressThickness

Sets the width of the progress indicator's arc stroke.

```xml
<syncfusion:SfPullToRefresh x:Name="pullToRefresh" 
                             ProgressThickness="5">
    <!-- PullableContent -->
</syncfusion:SfPullToRefresh>
```

```csharp
pullToRefresh.ProgressThickness = 5d;
```

**Guidelines:**
- Typical range: 2-6 units
- Thinner: More subtle appearance
- Thicker: More prominent indicator
- Platform-specific: Consider adjusting per platform

### Complete Progress Customization

```xml
<syncfusion:SfPullToRefresh x:Name="pullToRefresh"
                             ProgressColor="DarkBlue"
                             ProgressBackground="LightGray"
                             ProgressThickness="4"
                             RefreshViewHeight="60"
                             RefreshViewWidth="60">
    <syncfusion:SfPullToRefresh.PullableContent>
        <ListView ItemsSource="{Binding Items}"/>
    </syncfusion:SfPullToRefresh.PullableContent>
</syncfusion:SfPullToRefresh>
```

## Refresh View Dimensions

Control the size of the refresh indicator view container.

### RefreshViewWidth

Sets the width of the refresh view.

```xml
<syncfusion:SfPullToRefresh x:Name="pullToRefresh" 
                             RefreshViewWidth="50">
    <!-- PullableContent -->
</syncfusion:SfPullToRefresh>
```

```csharp
pullToRefresh.RefreshViewWidth = 50d;
```

### RefreshViewHeight

Sets the height of the refresh view.

```xml
<syncfusion:SfPullToRefresh x:Name="pullToRefresh" 
                             RefreshViewHeight="50">
    <!-- PullableContent -->
</syncfusion:SfPullToRefresh>
```

```csharp
pullToRefresh.RefreshViewHeight = 50d;
```

### Size Considerations

```xml
<!-- Small indicator -->
<syncfusion:SfPullToRefresh RefreshViewWidth="30"
                             RefreshViewHeight="30"
                             ProgressThickness="2"/>

<!-- Medium indicator (default-ish) -->
<syncfusion:SfPullToRefresh RefreshViewWidth="50"
                             RefreshViewHeight="50"
                             ProgressThickness="3"/>

<!-- Large indicator -->
<syncfusion:SfPullToRefresh RefreshViewWidth="80"
                             RefreshViewHeight="80"
                             ProgressThickness="5"/>
```

## IsRefreshing Property

The `IsRefreshing` property controls the refresh state and progress animation visibility.

### Property Behavior

- **`true`**: Shows progress indicator and starts refresh animation
- **`false`**: Hides progress indicator and stops refresh animation

### Manual Control

```xml
<syncfusion:SfPullToRefresh x:Name="pullToRefresh" 
                             IsRefreshing="False">
    <!-- PullableContent -->
</syncfusion:SfPullToRefresh>
```

```csharp
// Start refreshing
pullToRefresh.IsRefreshing = true;

// Stop refreshing
pullToRefresh.IsRefreshing = false;
```

### Typical Usage Pattern

```csharp
private async void OnRefreshing(object sender, EventArgs e)
{
    // Begin refresh
    pullToRefresh.IsRefreshing = true;
    
    try
    {
        // Perform refresh operation
        await LoadDataAsync();
    }
    finally
    {
        // End refresh (always executes)
        pullToRefresh.IsRefreshing = false;
    }
}
```

### Data Binding

```xml
<syncfusion:SfPullToRefresh IsRefreshing="{Binding IsRefreshing}">
    <!-- PullableContent -->
</syncfusion:SfPullToRefresh>
```

```csharp
public class ViewModel : INotifyPropertyChanged
{
    private bool isRefreshing;
    
    public bool IsRefreshing
    {
        get => isRefreshing;
        set
        {
            isRefreshing = value;
            OnPropertyChanged(nameof(IsRefreshing));
        }
    }
}
```

## Programmatic Support

Control refresh operations programmatically without user interaction.

### StartRefreshing()

Initiates the refresh operation programmatically, showing the progress indicator.

```csharp
// Trigger refresh without user pull gesture
pullToRefresh.StartRefreshing();
```

**Use Cases:**
- Automatic refresh on page load
- Refresh triggered by button click
- Periodic auto-refresh
- Refresh from external event

**Example: Auto-refresh on Page Appearing**

```csharp
protected override void OnAppearing()
{
    base.OnAppearing();
    
    // Automatically refresh when page appears
    pullToRefresh.StartRefreshing();
    
    // Perform data load
    LoadDataAsync();
}
```

### EndRefreshing()

Stops the refresh operation programmatically, hiding the progress indicator.

```csharp
// End the refresh animation
pullToRefresh.EndRefreshing();
```

**Use Cases:**
- Alternative to setting `IsRefreshing = false`
- More explicit method call
- Same result as `IsRefreshing = false`

**Example: Button-Triggered Refresh**

```xml
<StackLayout>
    <Button Text="Refresh Data" 
            Clicked="OnRefreshButtonClicked"/>
    
    <syncfusion:SfPullToRefresh x:Name="pullToRefresh">
        <syncfusion:SfPullToRefresh.PullableContent>
            <ListView ItemsSource="{Binding Items}"/>
        </syncfusion:SfPullToRefresh.PullableContent>
    </syncfusion:SfPullToRefresh>
</StackLayout>
```

```csharp
private async void OnRefreshButtonClicked(object sender, EventArgs e)
{
    // Start programmatic refresh
    pullToRefresh.StartRefreshing();
    
    // Load data
    await viewModel.RefreshDataAsync();
    
    // End refresh
    pullToRefresh.EndRefreshing();
}
```

### Timed Auto-Refresh Example

```csharp
private IDispatcherTimer refreshTimer;

protected override void OnAppearing()
{
    base.OnAppearing();
    
    // Auto-refresh every 30 seconds
    refreshTimer = Dispatcher.CreateTimer();
    refreshTimer.Interval = TimeSpan.FromSeconds(30);
    refreshTimer.Tick += async (s, e) =>
    {
        pullToRefresh.StartRefreshing();
        await LoadDataAsync();
        pullToRefresh.EndRefreshing();
    };
    refreshTimer.Start();
}

protected override void OnDisappearing()
{
    base.OnDisappearing();
    refreshTimer?.Stop();
}
```

## Size and Layout Considerations

**Critical:** PullToRefresh does not have an intrinsic size and requires explicit sizing or layout options.

### Problem

Without size or layout options, the control won't display properly:

```xml
<!-- This may not display correctly -->
<syncfusion:SfPullToRefresh>
    <syncfusion:SfPullToRefresh.PullableContent>
        <Label Text="Content" />
    </syncfusion:SfPullToRefresh.PullableContent>
</syncfusion:SfPullToRefresh>
```

### Solution 1: Use LayoutOptions

```xml
<syncfusion:SfPullToRefresh HorizontalOptions="FillAndExpand"
                             VerticalOptions="FillAndExpand">
    <syncfusion:SfPullToRefresh.PullableContent>
        <Label Text="Content" />
    </syncfusion:SfPullToRefresh.PullableContent>
</syncfusion:SfPullToRefresh>
```

### Solution 2: Set Explicit Size

```xml
<syncfusion:SfPullToRefresh HeightRequest="500"
                             WidthRequest="400">
    <syncfusion:SfPullToRefresh.PullableContent>
        <Label Text="Content" />
    </syncfusion:SfPullToRefresh.PullableContent>
</syncfusion:SfPullToRefresh>
```

### Solution 3: Place in Grid

```xml
<Grid>
    <syncfusion:SfPullToRefresh>
        <syncfusion:SfPullToRefresh.PullableContent>
            <Label Text="Content" />
        </syncfusion:SfPullToRefresh.PullableContent>
    </syncfusion:SfPullToRefresh>
</Grid>
```

## Complete Customization Example

Here's a comprehensive example combining multiple customization options:

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:syncfusion="clr-namespace:Syncfusion.Maui.PullToRefresh;assembly=Syncfusion.Maui.PullToRefresh"
             x:Class="MyApp.CustomizedRefreshPage">
    
    <Grid>
        <syncfusion:SfPullToRefresh x:Name="pullToRefresh"
                                     TransitionMode="Push"
                                     PullingThreshold="150"
                                     RefreshViewThreshold="30"
                                     RefreshViewHeight="60"
                                     RefreshViewWidth="60"
                                     ProgressColor="DarkBlue"
                                     ProgressBackground="LightGray"
                                     ProgressThickness="4"
                                     IsRefreshing="False"
                                     Refreshing="OnRefreshing"
                                     HorizontalOptions="FillAndExpand"
                                     VerticalOptions="FillAndExpand">
            
            <syncfusion:SfPullToRefresh.PullableContent>
                <ScrollView>
                    <StackLayout Padding="20" Spacing="15">
                        <Label Text="Customized PullToRefresh"
                               FontSize="24"
                               FontAttributes="Bold"
                               HorizontalOptions="Center"/>
                        
                        <Frame BackgroundColor="LightBlue" 
                               CornerRadius="10" 
                               Padding="15">
                            <StackLayout>
                                <Label Text="Settings Applied:" 
                                       FontAttributes="Bold"/>
                                <Label Text="• TransitionMode: Push"/>
                                <Label Text="• PullingThreshold: 150"/>
                                <Label Text="• ProgressColor: DarkBlue"/>
                                <Label Text="• ProgressThickness: 4"/>
                            </StackLayout>
                        </Frame>
                        
                        <Label x:Name="statusLabel"
                               Text="Pull down to refresh"
                               FontSize="14"
                               TextColor="Gray"
                               HorizontalOptions="Center"/>
                        
                        <Button Text="Refresh Programmatically"
                                Clicked="OnRefreshButtonClicked"
                                BackgroundColor="DarkBlue"
                                TextColor="White"/>
                    </StackLayout>
                </ScrollView>
            </syncfusion:SfPullToRefresh.PullableContent>
        </syncfusion:SfPullToRefresh>
    </Grid>
    
</ContentPage>
```

```csharp
using Syncfusion.Maui.PullToRefresh;

namespace MyApp
{
    public partial class CustomizedRefreshPage : ContentPage
    {
        public CustomizedRefreshPage()
        {
            InitializeComponent();
        }

        private async void OnRefreshing(object sender, EventArgs e)
        {
            pullToRefresh.IsRefreshing = true;
            statusLabel.Text = "Refreshing...";
            
            // Simulate data refresh
            await Task.Delay(2000);
            
            statusLabel.Text = $"Last refreshed: {DateTime.Now:hh:mm:ss tt}";
            pullToRefresh.IsRefreshing = false;
        }

        private async void OnRefreshButtonClicked(object sender, EventArgs e)
        {
            pullToRefresh.StartRefreshing();
            statusLabel.Text = "Refreshing programmatically...";
            
            await Task.Delay(2000);
            
            statusLabel.Text = $"Programmatic refresh: {DateTime.Now:hh:mm:ss tt}";
            pullToRefresh.EndRefreshing();
        }
    }
}
```

## Property Reference Quick Guide

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| **TransitionMode** | PullToRefreshTransitionType | SlideOnTop | Animation mode (SlideOnTop or Push) |
| **PullingThreshold** | double | 150 | Maximum pull distance |
| **RefreshViewThreshold** | double | 30 | Starting position of indicator |
| **RefreshViewHeight** | double | 50 | Height of refresh view container |
| **RefreshViewWidth** | double | 50 | Width of refresh view container |
| **ProgressColor** | Color | Platform default | Color of progress arc |
| **ProgressBackground** | Color | Platform default | Background color of indicator |
| **ProgressThickness** | double | 3 (Android), 2 (others) | Thickness of progress arc |
| **IsRefreshing** | bool | false | Refresh state control |

### Method Reference

| Method | Description |
|--------|-------------|
| **StartRefreshing()** | Begins refresh programmatically |
| **EndRefreshing()** | Ends refresh programmatically |

### Best Practices Summary

1. **Always set layout options** or explicit size for PullToRefresh
2. **Use try-finally** to ensure `IsRefreshing` is reset
3. **Customize for platform** - adjust thickness for Android vs iOS
4. **Test threshold values** to find the right pull sensitivity
5. **Provide visual feedback** with status labels or timestamps
6. **Handle errors gracefully** during refresh operations
