# Visual Customization in .NET MAUI Tab View

## Table of Contents
- [Selection Indicator Customization](#selection-indicator-customization)
- [Visual State Managers](#visual-state-managers)
- [Color Schemes and Themes](#color-schemes-and-themes)
- [Liquid Glass Effect](#liquid-glass-effect)
- [Custom Indicator Views](#custom-indicator-views)
- [Animations and Transitions](#animations-and-transitions)
- [Complete Styling Examples](#complete-styling-examples)

---

Comprehensive guide for customizing the visual appearance of .NET MAUI Tab View including indicators, visual states, themes, and advanced effects.

## Selection Indicator Customization

The selection indicator visually highlights the currently active tab.

### Indicator Background

Set the indicator color:

**XAML:**
```xaml
<tabView:SfTabView IndicatorBackground="#6200EE">
    <tabView:SfTabView.Items>
        <!-- Tabs -->
    </tabView:SfTabView.Items>
</tabView:SfTabView>
```

**C#:**
```csharp
tabView.IndicatorBackground = new SolidColorBrush(Color.FromArgb("#6200EE"));
```

### Indicator Placement

Control where the indicator appears:

**Options:**
- `Top`: Indicator at top edge of tab bar
- `Bottom`: Indicator at bottom edge (default)
- `Fill`: Indicator fills entire tab background
- `Left`: Indicator at left edge of tab bar
- `Right`: Indicator at right edge

**XAML Examples:**

```xaml
<!-- Bottom indicator (Material Design style) -->
<tabView:SfTabView IndicatorPlacement="Bottom"
                   IndicatorBackground="#6200EE"
                   TabBarPlacement="Top">
    <tabView:SfTabView.Items>
        <tabView:SfTabItem Header="Tab 1" />
        <tabView:SfTabItem Header="Tab 2" />
    </tabView:SfTabView.Items>
</tabView:SfTabView>

<!-- Top indicator -->
<tabView:SfTabView IndicatorPlacement="Top"
                   IndicatorBackground="#FF6B6B"
                   TabBarPlacement="Bottom">
    <tabView:SfTabView.Items>
        <tabView:SfTabItem Header="Tab 1" />
        <tabView:SfTabItem Header="Tab 2" />
    </tabView:SfTabView.Items>
</tabView:SfTabView>

<!-- Fill background (subtle highlight) -->
<tabView:SfTabView IndicatorPlacement="Fill"
                   IndicatorBackground="#E3F2FD">
    <tabView:SfTabView.Items>
        <tabView:SfTabItem Header="Tab 1" TextColor="#1976D2" />
        <tabView:SfTabItem Header="Tab 2" TextColor="#1976D2" />
    </tabView:SfTabView.Items>
</tabView:SfTabView>

<!-- Left indicator -->
<tabView:SfTabView IndicatorPlacement="Left">
    <tabView:SfTabView.Items>
        <!-- Use custom visual states or styling instead -->
    </tabView:SfTabView.Items>
</tabView:SfTabView>

<!-- Right indicator -->
<tabView:SfTabView IndicatorPlacement="Right">
    <tabView:SfTabView.Items>
        <!-- Use custom visual states or styling instead -->
    </tabView:SfTabView.Items>
</tabView:SfTabView>
```

### Indicator Width Mode

Control how indicator width is calculated:

**Options:**
- `Fit`: Width matches header content
- `Stretch`: Width spans full tab width

**XAML:**
```xaml
<!-- Fit to text -->
<tabView:SfTabView IndicatorWidthMode="Fit"
                   IndicatorBackground="#4CAF50">
    <tabView:SfTabView.Items>
        <tabView:SfTabItem Header="Short" />
        <tabView:SfTabItem Header="Longer Tab Name" />
    </tabView:SfTabView.Items>
</tabView:SfTabView>

<!-- Stretch full width -->
<tabView:SfTabView IndicatorWidthMode="Stretch"
                   IndicatorBackground="#FF9800">
    <tabView:SfTabView.Items>
        <tabView:SfTabItem Header="Tab 1" />
        <tabView:SfTabItem Header="Tab 2" />
    </tabView:SfTabView.Items>
</tabView:SfTabView>
```

**C#:**
```csharp
tabView.IndicatorWidthMode = IndicatorWidthMode.Fit;
// or
tabView.IndicatorWidthMode = IndicatorWidthMode.Stretch;
```

### Custom Indicator Thickness

While there's no direct property, use Fill mode with semi-transparent color for visual effect:

```xaml
<tabView:SfTabView IndicatorPlacement="Fill"
                   IndicatorBackground="#226200EE">  <!-- Alpha channel for opacity -->
    <tabView:SfTabView.Items>
        <!-- Tabs -->
    </tabView:SfTabView.Items>
</tabView:SfTabView>
```

## Visual State Managers

Use Visual State Manager (VSM) to customize tab appearance based on state.

### Tab States

Available states:
- `Normal`: Default state
- `Selected`: Currently active tab
- `Disabled`: Tab is disabled (IsEnabled = false)

### Basic Visual State Implementation

```xaml
<tabView:SfTabItem Header="Home">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup Name="CommonStates">
            
            <!-- Normal State -->
            <VisualState Name="Normal">
                <VisualState.Setters>
                    <Setter Property="TextColor" Value="Gray" />
                    <Setter Property="FontSize" Value="14" />
                </VisualState.Setters>
            </VisualState>
            
            <!-- Selected State -->
            <VisualState Name="Selected">
                <VisualState.Setters>
                    <Setter Property="TextColor" Value="#6200EE" />
                    <Setter Property="FontSize" Value="16" />
                    <Setter Property="FontAttributes" Value="Bold" />
                </VisualState.Setters>
            </VisualState>
            
            <!-- Disabled State -->
            <VisualState Name="Disabled">
                <VisualState.Setters>
                    <Setter Property="TextColor" Value="LightGray" />
                    <Setter Property="FontSize" Value="14" />
                </VisualState.Setters>
            </VisualState>
            
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
    <tabView:SfTabItem.Content>
        <!-- Content -->
    </tabView:SfTabItem.Content>
</tabView:SfTabItem>
```

### Visual State with Custom Header

Apply VSM to custom header content:

```xaml
<tabView:SfTabItem>
    <tabView:SfTabItem.HeaderContent>
        <StackLayout Orientation="Vertical" Spacing="4" x:Name="headerStack">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup Name="CommonStates">
                    <VisualState Name="Normal">
                        <VisualState.Setters>
                            <Setter Property="Opacity" Value="0.6" />
                        </VisualState.Setters>
                    </VisualState>
                    <VisualState Name="Selected">
                        <VisualState.Setters>
                            <Setter Property="Opacity" Value="1.0" />
                            <Setter Property="Scale" Value="1.1" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
            
            <Image Source="home.png" WidthRequest="24" HeightRequest="24" />
            <Label Text="Home" FontSize="12" HorizontalOptions="Center" />
        </StackLayout>
    </tabView:SfTabItem.HeaderContent>
</tabView:SfTabItem>
```

### Programmatic Visual State Changes

```csharp
tabView.SelectionChanged += (s, e) =>
{
    // Update visual states for all tabs
    for (int i = 0; i < tabView.Items.Count; i++)
    {
        var tab = tabView.Items[i];
        var state = i == e.NewIndex ? "Selected" : "Normal";
        
        VisualStateManager.GoToState(tab, state);
        
        // Or update specific properties
        if (i == e.NewIndex)
        {
            tab.TextColor = Color.FromArgb("#6200EE");
            tab.FontAttributes = FontAttributes.Bold;
        }
        else
        {
            tab.TextColor = Colors.Gray;
            tab.FontAttributes = FontAttributes.None;
        }
    }
};
```

## Color Schemes and Themes

### Material Design Theme

```xaml
<tabView:SfTabView TabBarBackground="#6200EE"
                   TabBarHeight="48"
                   IndicatorPlacement="Bottom"
                   IndicatorBackground="White">
    <tabView:SfTabView.Items>
        <tabView:SfTabItem Header="TAB 1" 
                           TextColor="White" 
                           FontSize="14"
                           FontAttributes="Bold" />
        <tabView:SfTabItem Header="TAB 2" 
                           TextColor="White" 
                           FontSize="14"
                           FontAttributes="Bold" />
        <tabView:SfTabItem Header="TAB 3" 
                           TextColor="White" 
                           FontSize="14"
                           FontAttributes="Bold" />
    </tabView:SfTabView.Items>
</tabView:SfTabView>
```

### iOS-Style Theme

```xaml
<tabView:SfTabView TabBarBackground="White"
                   TabBarHeight="50"
                   TabBarPlacement="Bottom"
                   IndicatorPlacement="None">
    <tabView:SfTabView.Items>
        <tabView:SfTabItem Header="Home" 
                           ImageSource="home.png"
                           ImagePosition="Top"
                           TextColor="#007AFF"
                           FontSize="10" />
        <tabView:SfTabItem Header="Search" 
                           ImageSource="search.png"
                           ImagePosition="Top"
                           TextColor="#007AFF"
                           FontSize="10" />
        <tabView:SfTabItem Header="Profile" 
                           ImageSource="user.png"
                           ImagePosition="Top"
                           TextColor="#007AFF"
                           FontSize="10" />
    </tabView:SfTabView.Items>
</tabView:SfTabView>
```

### Dark Theme

```xaml
<tabView:SfTabView TabBarBackground="#1E1E1E"
                   TabBarHeight="48"
                   IndicatorPlacement="Bottom"
                   IndicatorBackground="#BB86FC">
    <tabView:SfTabView.Items>
        <tabView:SfTabItem Header="Home" TextColor="#E1E1E1" />
        <tabView:SfTabItem Header="Settings" TextColor="#E1E1E1" />
    </tabView:SfTabView.Items>
</tabView:SfTabView>
```

### Dynamic Theme Switching

```csharp
public void ApplyTheme(bool isDarkMode)
{
    if (isDarkMode)
    {
        tabView.TabBarBackground = new SolidColorBrush(Color.FromArgb("#1E1E1E"));
        tabView.IndicatorBackground = new SolidColorBrush(Color.FromArgb("#BB86FC"));
        
        foreach (var tab in tabView.Items)
        {
            tab.TextColor = Color.FromArgb("#E1E1E1");
        }
    }
    else
    {
        tabView.TabBarBackground = new SolidColorBrush(Colors.White);
        tabView.IndicatorBackground = new SolidColorBrush(Color.FromArgb("#6200EE"));
        
        foreach (var tab in tabView.Items)
        {
            tab.TextColor = Color.FromArgb("#333333");
        }
    }
}

// Listen to system theme changes
Application.Current.RequestedThemeChanged += (s, e) =>
{
    ApplyTheme(e.RequestedTheme == AppTheme.Dark);
};
```

## Liquid Glass Effect

Liquid Glass is a special visual effect available in Syncfusion MAUI controls.

### Enabling Liquid Glass

**Note:** Requires specific Syncfusion theme/styling support. Check documentation for availability in your version.

```xaml
<tabView:SfTabView x:Name="tabView"
                   TabBarBackground="Transparent">
    <!-- Apply liquid glass background to parent container -->
</tabView:SfTabView>
```

### Simulating Glass Effect

Create a frosted glass appearance using gradients and opacity:

```xaml
<Grid>
    <!-- Background image or content -->
    <Image Source="background.jpg" Aspect="AspectFill" />
    
    <!-- Tab View with semi-transparent background -->
    <tabView:SfTabView VerticalOptions="End">
        <tabView:SfTabView.TabBarBackground>
            <LinearGradientBrush StartPoint="0,0" EndPoint="0,1">
                <GradientStop Color="#CCFFFFFF" Offset="0" />
                <GradientStop Color="#99FFFFFF" Offset="1" />
            </LinearGradientBrush>
        </tabView:SfTabView.TabBarBackground>
        <tabView:SfTabView.Items>
            <tabView:SfTabItem Header="Tab 1" TextColor="#333333" />
            <tabView:SfTabItem Header="Tab 2" TextColor="#333333" />
        </tabView:SfTabView.Items>
    </tabView:SfTabView>
</Grid>
```

## Custom Indicator Views

While the control doesn't directly support custom indicator views, you can achieve custom effects:

### Rounded Pill Indicator

Use Fill mode with rounded tab styling:

```xaml
<tabView:SfTabView IndicatorPlacement="Fill"
                   IndicatorBackground="#E3F2FD"
                   TabWidthMode="SizeToContent">
    <tabView:SfTabView.Items>
        <tabView:SfTabItem>
            <tabView:SfTabItem.Content>
                <Frame CornerRadius="20" 
                       Padding="15,8"
                       HasShadow="False"
                       BackgroundColor="Transparent">
                    <Label Text="Home" TextColor="#1976D2" />
                </Frame>
            </tabView:SfTabItem.Content>
        </tabView:SfTabItem>
    </tabView:SfTabView.Items>
</tabView:SfTabView>
```

### Custom Underline with Border

```xaml
<tabView:SfTabItem>
    <tabView:SfTabItem.HeaderContent>
        <Grid>
            <Label Text="Custom Tab" 
                   Padding="15,10"
                   VerticalTextAlignment="Center" />
            <BoxView Color="#6200EE" 
                     HeightRequest="3"
                     VerticalOptions="End"
                     IsVisible="{Binding IsSelected}" />
        </Grid>
    </tabView:SfTabItem.HeaderContent>
</tabView:SfTabItem>
```

## Animations and Transitions

### Selection Animation

```csharp
tabView.SelectionChanged += async (s, e) =>
{
    var oldTab = e.OldIndex >= 0 ? tabView.Items[(int)e.OldIndex] : null;
    var newTab = tabView.Items[(int)e.NewIndex];
    
    // Animate old tab out
    if (oldTab?.Content is View oldHeader)
    {
        await oldHeader.ScaleTo(0.9, 100);
        await oldHeader.ScaleTo(1.0, 100);
    }
    
    // Animate new tab in
    if (newTab.Content is View newHeader)
    {
        await newHeader.ScaleTo(1.1, 100);
        await newHeader.ScaleTo(1.0, 100);
    }
};
```

### Fade Transition

```csharp
tabView.SelectionChanging += (s, e) =>
{
    if (e.Index != tabView.SelectedIndex)
    {
        var currentTab = tabView.Items[(int)tabView.SelectedIndex];
        var nextTab = tabView.Items[e.Index];
        
        // Fade out current content
        if (currentTab.Content is View currentContent)
        {
            currentContent.FadeTo(0, 150);
        }
        
        // Fade in next content
        if (nextTab.Content is View nextContent)
        {
            nextContent.Opacity = 0;
            nextContent.FadeTo(1, 150);
        }
    }
};
```

### Slide Animation

```csharp
tabView.SelectionChanged += (s, e) =>
{
    var newTab = tabView.Items[(int)e.NewIndex];
    
    if (newTab.Content is View content)
    {
        // Slide in from right or left based on direction
        double startX = e.NewIndex > e.OldIndex ? 100 : -100;
        content.TranslationX = startX;
        content.TranslateTo(0, 0, 250, Easing.CubicOut);
    }
};
```

## Complete Styling Examples

### Example 1: Premium App Style

```xaml
<tabView:SfTabView TabBarHeight="60"
                   TabBarPlacement="Bottom"
                   IndicatorPlacement="Top"
                   IndicatorBackground="#F39C12">
    <tabView:SfTabView.TabBarBackground>
        <LinearGradientBrush StartPoint="0,0" EndPoint="1,0">
            <GradientStop Color="#1A1A2E" Offset="0" />
            <GradientStop Color="#16213E" Offset="1" />
        </LinearGradientBrush>
    </tabView:SfTabView.TabBarBackground>
    <tabView:SfTabView.Items>
        <tabView:SfTabItem Header="Dashboard" 
                           ImageSource="chart.png"
                           ImagePosition="Top"
                           TextColor="#ECF0F1"
                           FontSize="11" />
        <tabView:SfTabItem Header="Portfolio" 
                           ImageSource="briefcase.png"
                           ImagePosition="Top"
                           TextColor="#ECF0F1"
                           FontSize="11" />
        <tabView:SfTabItem Header="Trades" 
                           ImageSource="trending.png"
                           ImagePosition="Top"
                           TextColor="#ECF0F1"
                           FontSize="11" />
        <tabView:SfTabItem Header="Account" 
                           ImageSource="user.png"
                           ImagePosition="Top"
                           TextColor="#ECF0F1"
                           FontSize="11" />
    </tabView:SfTabView.Items>
</tabView:SfTabView>
```

### Example 2: Colorful Segmented Control Style

```xaml
<tabView:SfTabView TabWidthMode="Default"
                   TabBarHeight="44"
                   IndicatorPlacement="Fill">
    <tabView:SfTabView.Items>
        <tabView:SfTabItem Header="Photos">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup Name="CommonStates">
                    <VisualState Name="Selected">
                        <VisualState.Setters>
                            <Setter Property="TextColor" Value="White" />
                        </VisualState.Setters>
                    </VisualState>
                    <VisualState Name="Normal">
                        <VisualState.Setters>
                            <Setter Property="TextColor" Value="#6200EE" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
        </tabView:SfTabItem>
        <!-- More tabs with same pattern -->
    </tabView:SfTabView.Items>
</tabView:SfTabView>
```

### Example 3: Minimalist Design

```xaml
<tabView:SfTabView TabBarBackground="White"
                   TabBarHeight="48"
                   TabWidthMode="SizeToContent"
                   IndicatorPlacement="Bottom"
                   IndicatorBackground="Black"
                   IndicatorWidthMode="Fit">
    <tabView:SfTabView.Items>
        <tabView:SfTabItem Header="Minimal" 
                           TextColor="#333333"
                           FontSize="15"
                           FontFamily="HelveticaNeue-Light" />
        <tabView:SfTabItem Header="Clean" 
                           TextColor="#333333"
                           FontSize="15"
                           FontFamily="HelveticaNeue-Light" />
        <tabView:SfTabItem Header="Simple" 
                           TextColor="#333333"
                           FontSize="15"
                           FontFamily="HelveticaNeue-Light" />
    </tabView:SfTabView.Items>
</tabView:SfTabView>
```

## Best Practices

1. **Indicator Contrast:** Ensure indicator color contrasts well with tab bar background
2. **Visual States:** Use VSM for consistent state-based styling
3. **Theme Support:** Implement both light and dark themes
4. **Animation Performance:** Keep animations short (100-250ms) for responsiveness
5. **Custom Headers:** Maintain consistent sizing and alignment across custom headers
6. **Accessibility:** Ensure sufficient color contrast for text visibility
7. **Platform Conventions:** Follow platform-specific design guidelines when appropriate

## Common Issues

**Issue:** Indicator not visible  
**Solution:** Check IndicatorBackground contrasts with TabBarBackground, verify IndicatorPlacement is not None

**Issue:** Visual states not applying  
**Solution:** Ensure VisualStateManager is properly configured, check state names match ("Normal", "Selected", "Disabled")

**Issue:** Animations causing lag  
**Solution:** Reduce animation duration, simplify animated elements, avoid animating large content views

**Issue:** Custom indicator not showing  
**Solution:** Use IndicatorPlacement="Fill" and implement custom visual feedback in Content

**Issue:** Theme not updating  
**Solution:** Manually update all relevant properties when theme changes, subscribe to RequestedThemeChanged event
