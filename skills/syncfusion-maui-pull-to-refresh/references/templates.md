# Custom Templates in .NET MAUI PullToRefresh

## Table of Contents
- [Overview](#overview)
- [PullingViewTemplate](#pullingviewtemplate)
- [RefreshingViewTemplate](#refreshingviewtemplate)
- [Creating Custom Templates](#creating-custom-templates)
- [Using SfCircularProgressBar](#using-sfcircularprogressbar)
- [Integrating with Pulling Event](#integrating-with-pulling-event)
- [Animation During Refresh](#animation-during-refresh)
- [Complete Template Examples](#complete-template-examples)
- [Best Practices](#best-practices)

## Overview

The PullToRefresh control allows you to replace the default progress indicator with custom views using templates. You can create unique pulling and refreshing experiences with custom progress indicators, animations, labels, images, or any .NET MAUI view.

### Template Properties

- **PullingViewTemplate**: Custom view displayed while pulling down (before release)
- **RefreshingViewTemplate**: Custom view displayed during the refresh operation (after release)

## PullingViewTemplate

The `PullingViewTemplate` defines the view displayed while the user is actively pulling down on the content, before releasing the gesture.

### Basic PullingViewTemplate

```xml
<syncfusion:SfPullToRefresh x:Name="pullToRefresh">
    <syncfusion:SfPullToRefresh.PullingViewTemplate>
        <DataTemplate>
            <Frame CornerRadius="25" 
                   BackgroundColor="White"
                   HasShadow="True"
                   Padding="10">
                <Label Text="Pull to refresh..."
                       TextColor="Gray"
                       FontSize="12"
                       HorizontalOptions="Center"
                       VerticalOptions="Center"/>
            </Frame>
        </DataTemplate>
    </syncfusion:SfPullToRefresh.PullingViewTemplate>
    
    <syncfusion:SfPullToRefresh.PullableContent>
        <ListView ItemsSource="{Binding Items}"/>
    </syncfusion:SfPullToRefresh.PullableContent>
</syncfusion:SfPullToRefresh>
```

### PullingViewTemplate with Icon

```xml
<syncfusion:SfPullToRefresh.PullingViewTemplate>
    <DataTemplate>
        <Grid WidthRequest="50" HeightRequest="50">
            <Frame CornerRadius="25" 
                   BackgroundColor="LightBlue"
                   HasShadow="True"
                   Padding="0">
                <Image Source="refresh_icon.png"
                       WidthRequest="30"
                       HeightRequest="30"
                       HorizontalOptions="Center"
                       VerticalOptions="Center"/>
            </Frame>
        </Grid>
    </DataTemplate>
</syncfusion:SfPullToRefresh.PullingViewTemplate>
```

### PullingViewTemplate with Custom View

```xml
<syncfusion:SfPullToRefresh.PullingViewTemplate>
    <DataTemplate>
        <StackLayout WidthRequest="60" 
                     HeightRequest="60"
                     BackgroundColor="White">
            <Label x:Name="pullProgressLabel"
                   Text="0%"
                   FontSize="16"
                   FontAttributes="Bold"
                   TextColor="DarkBlue"
                   HorizontalOptions="Center"
                   VerticalOptions="Center"/>
            <Label Text="↓ Pull down"
                   FontSize="10"
                   TextColor="Gray"
                   HorizontalOptions="Center"/>
        </StackLayout>
    </DataTemplate>
</syncfusion:SfPullToRefresh.PullingViewTemplate>
```

## RefreshingViewTemplate

The `RefreshingViewTemplate` defines the view displayed during the refresh operation, after the user releases the pull gesture.

### Basic RefreshingViewTemplate

```xml
<syncfusion:SfPullToRefresh x:Name="pullToRefresh">
    <syncfusion:SfPullToRefresh.RefreshingViewTemplate>
        <DataTemplate>
            <Frame CornerRadius="25" 
                   BackgroundColor="White"
                   HasShadow="True"
                   Padding="15">
                <StackLayout Orientation="Horizontal" Spacing="10">
                    <ActivityIndicator IsRunning="True"
                                      Color="Blue"
                                      WidthRequest="20"
                                      HeightRequest="20"/>
                    <Label Text="Refreshing..."
                           TextColor="Blue"
                           FontSize="14"
                           VerticalOptions="Center"/>
                </StackLayout>
            </Frame>
        </DataTemplate>
    </syncfusion:SfPullToRefresh.RefreshingViewTemplate>
    
    <syncfusion:SfPullToRefresh.PullableContent>
        <ListView ItemsSource="{Binding Items}"/>
    </syncfusion:SfPullToRefresh.PullableContent>
</syncfusion:SfPullToRefresh>
```

### RefreshingViewTemplate with Animation

```xml
<syncfusion:SfPullToRefresh.RefreshingViewTemplate>
    <DataTemplate>
        <Grid WidthRequest="50" HeightRequest="50">
            <Frame CornerRadius="25" 
                   BackgroundColor="LightGreen"
                   HasShadow="True">
                <ActivityIndicator IsRunning="True"
                                  Color="DarkGreen"
                                  WidthRequest="30"
                                  HeightRequest="30"/>
            </Frame>
        </Grid>
    </DataTemplate>
</syncfusion:SfPullToRefresh.RefreshingViewTemplate>
```

## Creating Custom Templates

You can use both templates together to create a complete custom refresh experience.

### Combined Template Example

```xml
<syncfusion:SfPullToRefresh x:Name="pullToRefresh"
                             Pulling="OnPulling"
                             Refreshing="OnRefreshing">
    <!-- Pulling Template -->
    <syncfusion:SfPullToRefresh.PullingViewTemplate>
        <DataTemplate>
            <Frame CornerRadius="30"
                   BackgroundColor="White"
                   BorderColor="LightGray"
                   HasShadow="False"
                   WidthRequest="60"
                   HeightRequest="60"
                   Padding="0">
                <Label x:Name="pullLabel"
                       Text="↓"
                       FontSize="32"
                       TextColor="Blue"
                       HorizontalOptions="Center"
                       VerticalOptions="Center"/>
            </Frame>
        </DataTemplate>
    </syncfusion:SfPullToRefresh.PullingViewTemplate>
    
    <!-- Refreshing Template -->
    <syncfusion:SfPullToRefresh.RefreshingViewTemplate>
        <DataTemplate>
            <Frame CornerRadius="30"
                   BackgroundColor="Blue"
                   HasShadow="True"
                   WidthRequest="60"
                   HeightRequest="60"
                   Padding="0">
                <ActivityIndicator IsRunning="True"
                                  Color="White"
                                  WidthRequest="40"
                                  HeightRequest="40"/>
            </Frame>
        </DataTemplate>
    </syncfusion:SfPullToRefresh.RefreshingViewTemplate>
    
    <syncfusion:SfPullToRefresh.PullableContent>
        <ListView ItemsSource="{Binding Items}"/>
    </syncfusion:SfPullToRefresh.PullableContent>
</syncfusion:SfPullToRefresh>
```

## Using SfCircularProgressBar

Syncfusion's `SfCircularProgressBar` provides a rich progress indicator for custom templates.

### Prerequisites

Install the ProgressBar NuGet package:
```bash
dotnet add package Syncfusion.Maui.ProgressBar
```

### Complete Implementation with SfCircularProgressBar

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:syncfusion="clr-namespace:Syncfusion.Maui.PullToRefresh;assembly=Syncfusion.Maui.PullToRefresh"
             xmlns:progressBar="clr-namespace:Syncfusion.Maui.ProgressBar;assembly=Syncfusion.Maui.ProgressBar"
             x:Class="MyApp.TemplateExample">
    
    <syncfusion:SfPullToRefresh x:Name="pullToRefresh"
                                 RefreshViewHeight="60"
                                 RefreshViewWidth="60"
                                 Pulling="OnPulling"
                                 Refreshing="OnRefreshing">
        
        <syncfusion:SfPullToRefresh.PullingViewTemplate>
            <DataTemplate>
                <Frame x:Name="progressFrame"
                       CornerRadius="30"
                       BackgroundColor="White"
                       BorderColor="LightGray"
                       HasShadow="False"
                       Padding="0"
                       WidthRequest="60"
                       HeightRequest="60">
                    <progressBar:SfCircularProgressBar x:Name="circularProgress"
                                                       Progress="0"
                                                       ProgressThickness="6"
                                                       TrackThickness="0.8"
                                                       ProgressRadiusFactor="0.7"
                                                       TrackRadiusFactor="0.1"
                                                       SegmentCount="10"
                                                       SegmentGapWidth="1"
                                                       ProgressFill="Blue"
                                                       TrackFill="White"
                                                       IsIndeterminate="False"
                                                       WidthRequest="55"
                                                       HeightRequest="55">
                        <progressBar:SfCircularProgressBar.Content>
                            <Label x:Name="progressLabel"
                                   Text="0"
                                   FontSize="9"
                                   TextColor="Blue"
                                   WidthRequest="20"
                                   HorizontalTextAlignment="Center"/>
                        </progressBar:SfCircularProgressBar.Content>
                    </progressBar:SfCircularProgressBar>
                </Frame>
            </DataTemplate>
        </syncfusion:SfPullToRefresh.PullingViewTemplate>
        
        <syncfusion:SfPullToRefresh.RefreshingViewTemplate>
            <DataTemplate>
                <Frame CornerRadius="30"
                       BackgroundColor="White"
                       BorderColor="LightGray"
                       HasShadow="False"
                       Padding="0"
                       WidthRequest="60"
                       HeightRequest="60">
                    <progressBar:SfCircularProgressBar Progress="0"
                                                       ProgressThickness="6"
                                                       ProgressRadiusFactor="0.7"
                                                       SegmentCount="10"
                                                       SegmentGapWidth="1"
                                                       IsIndeterminate="True"
                                                       IndeterminateAnimationDuration="750"
                                                       ProgressFill="Blue"
                                                       WidthRequest="55"
                                                       HeightRequest="55">
                        <progressBar:SfCircularProgressBar.Content>
                            <Label Text=""
                                   FontSize="9"
                                   TextColor="Blue"
                                   WidthRequest="20"
                                   HorizontalTextAlignment="Center"/>
                        </progressBar:SfCircularProgressBar.Content>
                    </progressBar:SfCircularProgressBar>
                </Frame>
            </DataTemplate>
        </syncfusion:SfPullToRefresh.RefreshingViewTemplate>
        
        <syncfusion:SfPullToRefresh.PullableContent>
            <ListView x:Name="listView" ItemsSource="{Binding Items}"/>
        </syncfusion:SfPullToRefresh.PullableContent>
    </syncfusion:SfPullToRefresh>
    
</ContentPage>
```

## Integrating with Pulling Event

Update the template dynamically based on pull progress using the `Pulling` event.

### Code-Behind for Progress Updates

```csharp
using Syncfusion.Maui.PullToRefresh;
using Syncfusion.Maui.ProgressBar;

namespace MyApp
{
    public partial class TemplateExample : ContentPage
    {
        private SfCircularProgressBar progressbar;
        private Label progressLabel;
        private Frame progressFrame;
        
        public TemplateExample()
        {
            InitializeComponent();
            
            // Initialize custom template controls
            InitializeTemplateControls();
        }
        
        private void InitializeTemplateControls()
        {
            progressbar = new SfCircularProgressBar();
            progressLabel = new Label();
            progressFrame = new Frame();
            
            // Configure progress label
            progressLabel.TextColor = Colors.Blue;
            progressLabel.FontSize = 9;
            progressLabel.WidthRequest = 20;
            progressLabel.HorizontalTextAlignment = TextAlignment.Center;
            
            // Configure frame
            progressFrame.BorderColor = Colors.LightGray;
            progressFrame.BackgroundColor = Colors.White;
            progressFrame.CornerRadius = 30;
            progressFrame.Content = progressbar;
            progressFrame.Padding = 0;
            progressFrame.HasShadow = false;
            
            // Configure circular progress bar
            progressbar.SegmentCount = 10;
            progressbar.ProgressThickness = 6;
            progressbar.ProgressRadiusFactor = 0.7;
            progressbar.SegmentGapWidth = 1;
            progressbar.WidthRequest = 55;
            progressbar.HeightRequest = 55;
            progressbar.IndeterminateAnimationDuration = 750;
            progressbar.Content = progressLabel;
            
            // Set as template (if creating programmatically)
            var pullingTemplate = new DataTemplate(() =>
            {
                return new ViewCell { View = progressFrame };
            });
            
            pullToRefresh.PullingViewTemplate = pullingTemplate;
            pullToRefresh.RefreshingViewTemplate = pullingTemplate;
        }
        
        private void OnPulling(object sender, PullingEventArgs e)
        {
            // Update progress bar during pull
            progressbar.TrackThickness = 0.8;
            progressbar.TrackRadiusFactor = 0.1;
            progressbar.IsIndeterminate = false;
            progressbar.ProgressFill = Colors.Blue;
            progressbar.TrackFill = Colors.White;
            
            // Convert progress to percentage
            var absoluteProgress = Convert.ToInt32(Math.Abs(e.Progress));
            progressbar.Progress = absoluteProgress;
            progressbar.SetProgress(absoluteProgress, 1, Easing.CubicInOut);
            
            // Update label
            progressLabel.Text = e.Progress.ToString();
        }
        
        private async void OnRefreshing(object sender, EventArgs e)
        {
            // Hide progress label during refresh
            progressLabel.IsVisible = false;
            
            pullToRefresh.IsRefreshing = true;
            
            // Animate refresh indicator
            await AnimateRefresh();
            
            // Refresh data
            await viewModel.RefreshDataAsync();
            
            pullToRefresh.IsRefreshing = false;
            
            // Show progress label again
            progressLabel.IsVisible = true;
        }
        
        private async Task AnimateRefresh()
        {
            progressbar.Progress = 0;
            progressbar.IsIndeterminate = true;
            
            // Animate color changes
            await Task.Delay(750);
            progressbar.ProgressFill = Colors.Red;
            
            await Task.Delay(750);
            progressbar.ProgressFill = Colors.Green;
            
            await Task.Delay(750);
            progressbar.ProgressFill = Colors.Orange;
            
            await Task.Delay(750);
        }
    }
}
```

## Animation During Refresh

Create animated refresh indicators with color transitions and progress changes.

### Simple Animation Example

```csharp
private async Task AnimateRefresh()
{
    // Start indeterminate animation
    progressbar.IsIndeterminate = true;
    
    // Color sequence animation
    Color[] colors = { Colors.Blue, Colors.Red, Colors.Green, Colors.Orange };
    
    foreach (var color in colors)
    {
        progressbar.ProgressFill = color;
        await Task.Delay(500);
    }
}
```

### Progressive Loading Animation

```csharp
private async Task AnimateProgressiveLoad()
{
    progressbar.IsIndeterminate = false;
    
    // Simulate progressive loading
    for (int i = 0; i <= 100; i += 10)
    {
        progressbar.Progress = i;
        await Task.Delay(200);
    }
}
```

### Rotation Animation

```xml
<syncfusion:SfPullToRefresh.RefreshingViewTemplate>
    <DataTemplate>
        <Image x:Name="refreshIcon"
               Source="refresh_icon.png"
               WidthRequest="40"
               HeightRequest="40">
            <Image.Behaviors>
                <toolkit:IconTintColorBehavior TintColor="Blue"/>
            </Image.Behaviors>
        </Image>
    </DataTemplate>
</syncfusion:SfPullToRefresh.RefreshingViewTemplate>
```

```csharp
private async void OnRefreshing(object sender, EventArgs e)
{
    pullToRefresh.IsRefreshing = true;
    
    // Start rotation animation
    var rotationTask = RotateIconContinuously();
    
    // Refresh data
    await viewModel.RefreshDataAsync();
    
    pullToRefresh.IsRefreshing = false;
}

private async Task RotateIconContinuously()
{
    while (pullToRefresh.IsRefreshing)
    {
        await refreshIcon.RotateTo(360, 1000, Easing.Linear);
        refreshIcon.Rotation = 0;
    }
}
```

## Complete Template Examples

### Example 1: Weather App Style

```xml
<syncfusion:SfPullToRefresh x:Name="pullToRefresh"
                             Refreshing="OnRefreshing">
    <syncfusion:SfPullToRefresh.PullingViewTemplate>
        <DataTemplate>
            <StackLayout Spacing="5">
                <Image Source="cloud_icon.png"
                       WidthRequest="30"
                       HeightRequest="30"
                       HorizontalOptions="Center"/>
                <Label Text="Pull to update"
                       FontSize="10"
                       TextColor="Gray"
                       HorizontalOptions="Center"/>
            </StackLayout>
        </DataTemplate>
    </syncfusion:SfPullToRefresh.PullingViewTemplate>
    
    <syncfusion:SfPullToRefresh.RefreshingViewTemplate>
        <DataTemplate>
            <StackLayout Spacing="5">
                <ActivityIndicator IsRunning="True"
                                  Color="Blue"
                                  WidthRequest="30"
                                  HeightRequest="30"/>
                <Label Text="Updating weather..."
                       FontSize="10"
                       TextColor="Blue"
                       HorizontalOptions="Center"/>
            </StackLayout>
        </DataTemplate>
    </syncfusion:SfPullToRefresh.RefreshingViewTemplate>
    
    <syncfusion:SfPullToRefresh.PullableContent>
        <StackLayout>
            <Label Text="{Binding Temperature}"/>
            <Label Text="{Binding Condition}"/>
        </StackLayout>
    </syncfusion:SfPullToRefresh.PullableContent>
</syncfusion:SfPullToRefresh>
```

### Example 2: Social Media Style

```xml
<syncfusion:SfPullToRefresh.PullingViewTemplate>
    <DataTemplate>
        <Frame CornerRadius="25"
               BackgroundColor="#1DA1F2"
               HasShadow="True"
               Padding="12">
            <Label Text="↓"
                   FontSize="24"
                   TextColor="White"
                   HorizontalOptions="Center"
                   VerticalOptions="Center"/>
        </Frame>
    </DataTemplate>
</syncfusion:SfPullToRefresh.PullingViewTemplate>

<syncfusion:SfPullToRefresh.RefreshingViewTemplate>
    <DataTemplate>
        <Frame CornerRadius="25"
               BackgroundColor="#1DA1F2"
               HasShadow="True"
               Padding="12">
            <ActivityIndicator IsRunning="True"
                              Color="White"
                              WidthRequest="20"
                              HeightRequest="20"/>
        </Frame>
    </DataTemplate>
</syncfusion:SfPullToRefresh.RefreshingViewTemplate>
```

### Example 3: Material Design Style

```xml
<syncfusion:SfPullToRefresh.PullingViewTemplate>
    <DataTemplate>
        <Frame CornerRadius="30"
               BackgroundColor="White"
               HasShadow="True"
               Padding="0"
               WidthRequest="60"
               HeightRequest="60">
            <Grid>
                <BoxView Color="LightGray"
                         CornerRadius="30"
                         Opacity="0.3"/>
                <Label Text="⟳"
                       FontSize="28"
                       TextColor="#6200EE"
                       HorizontalOptions="Center"
                       VerticalOptions="Center"/>
            </Grid>
        </Frame>
    </DataTemplate>
</syncfusion:SfPullToRefresh.PullingViewTemplate>
```

## Best Practices

### 1. Keep Templates Lightweight

Avoid complex nested views in templates:

```xml
<!-- Good: Simple and performant -->
<DataTemplate>
    <Frame CornerRadius="25" BackgroundColor="White">
        <ActivityIndicator IsRunning="True" Color="Blue"/>
    </Frame>
</DataTemplate>

<!-- Avoid: Too complex -->
<DataTemplate>
    <Frame>
        <Grid>
            <StackLayout>
                <Border>
                    <Shadow/>
                    <!-- Multiple nested views -->
                </Border>
            </StackLayout>
        </Grid>
    </Frame>
</DataTemplate>
```

### 2. Match Template Size to RefreshView

Ensure template size matches RefreshViewWidth and RefreshViewHeight:

```xml
<syncfusion:SfPullToRefresh RefreshViewWidth="60"
                             RefreshViewHeight="60">
    <syncfusion:SfPullToRefresh.PullingViewTemplate>
        <DataTemplate>
            <!-- Template with matching size -->
            <Frame WidthRequest="60" HeightRequest="60">
                <!-- Content -->
            </Frame>
        </DataTemplate>
    </syncfusion:SfPullToRefresh.PullingViewTemplate>
</syncfusion:SfPullToRefresh>
```

### 3. Use Appropriate Animations

- **Pulling:** Use subtle progress indicators
- **Refreshing:** Use continuous animations (ActivityIndicator, IsIndeterminate)

### 4. Provide Visual Feedback

Always indicate the current state:

```xml
<!-- Pulling: Shows progress -->
<Label Text="{Binding Progress}%"/>

<!-- Refreshing: Shows loading state -->
<Label Text="Refreshing..."/>
```

### 5. Test on Multiple Platforms

Templates may render differently across platforms. Test on:
- Android
- iOS
- Windows
- macOS

### 6. Consider Accessibility

Ensure templates are accessible:

```xml
<Label Text="Pull to refresh"
       AutomationProperties.IsInAccessibleTree="True"
       AutomationProperties.HelpText="Pull down to refresh content"/>
```

### 7. Optimize for Performance

- Avoid heavy images in templates
- Use vector graphics when possible
- Minimize bindings in templates
- Reuse template instances when possible
