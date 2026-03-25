# Swiping Gestures in .NET MAUI Tab View

## Table of Contents
- [Overview](#overview)
- [Enabling Swipe Gestures](#enabling-swipe-gestures)
- [Swipe Behavior](#swipe-behavior)
- [Mobile-Optimized Patterns](#mobile-optimized-patterns)
- [Conditional Swipe Enabling](#conditional-swipe-enabling)
- [Platform-Specific Behavior](#platform-specific-behavior)
- [Combining Swipe with Events](#combining-swipe-with-events)
- [Accessibility Considerations](#accessibility-considerations)
- [Performance Optimization](#performance-optimization)
- [Testing Swipe Gestures](#testing-swipe-gestures)
- [Troubleshooting](#troubleshooting)
- [Best Practices](#best-practices)
- [Example: Complete Mobile App with Swipe](#example-complete-mobile-app-with-swipe)

---

Guide for enabling and configuring swipe gestures for mobile-friendly tab navigation in .NET MAUI Tab View.

## Overview

Swipe gestures allow users to navigate between tabs by swiping left or right on the tab content area, providing an intuitive mobile-first interaction pattern common in modern apps.

**Key Features:**
- Native touch gesture support
- Smooth animations during swipe
- Configurable enable/disable per TabView
- Works alongside tap navigation
- Platform-specific optimizations

## Enabling Swipe Gestures

### EnableSwiping Property

Enable or disable swipe navigation using the `EnableSwiping` property:

**XAML:**
```xaml
<tabView:SfTabView EnableSwiping="True">
    <tabView:SfTabView.Items>
        <tabView:SfTabItem Header="Home" />
        <tabView:SfTabItem Header="Profile" />
        <tabView:SfTabItem Header="Settings" />
    </tabView:SfTabView.Items>
</tabView:SfTabView>
```

**C#:**
```csharp
tabView.EnableSwiping = true;
```

**Default Value:** `false` (swipe disabled by default)

### Basic Swipe Implementation

```csharp
using Syncfusion.Maui.TabView;

namespace SwipeTabViewSample
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            
            var tabView = new SfTabView
            {
                EnableSwiping = true,  // Enable swipe navigation
                TabBarPlacement = TabBarPlacement.Bottom
            };
            
            // Add tabs
            tabView.Items.Add(new SfTabItem
            {
                Header = "Home",
                Content = new Grid
                {
                    Children =
                    {
                        new Label
                        {
                            Text = "Swipe left to see Profile",
                            HorizontalOptions = LayoutOptions.Center,
                            VerticalOptions = LayoutOptions.Center
                        }
                    }
                }
            });
            
            tabView.Items.Add(new SfTabItem
            {
                Header = "Profile",
                Content = new Grid
                {
                    Children =
                    {
                        new Label
                        {
                            Text = "Swipe left/right to navigate",
                            HorizontalOptions = LayoutOptions.Center,
                            VerticalOptions = LayoutOptions.Center
                        }
                    }
                }
            });
            
            tabView.Items.Add(new SfTabItem
            {
                Header = "Settings",
                Content = new Grid
                {
                    Children =
                    {
                        new Label
                        {
                            Text = "Swipe right to go back",
                            HorizontalOptions = LayoutOptions.Center,
                            VerticalOptions = LayoutOptions.Center
                        }
                    }
                }
            });
            
            this.Content = tabView;
        }
    }
}
```

## Swipe Behavior

### Navigation Direction

**Swipe Right:** Navigate to previous tab (left direction)
- From tab index 2 → 1 → 0
- Stops at first tab (index 0)

**Swipe Left:** Navigate to next tab (right direction)
- From tab index 0 → 1 → 2
- Stops at last tab

### Edge Behavior

Swiping at boundaries does **not** wrap around:
- Swiping left on last tab: No navigation
- Swiping right on first tab: No navigation

```csharp
// This behavior is automatic - no wrapping at edges
tabView.EnableSwiping = true;
// First tab: Only swipe left works
// Last tab: Only swipe right works
// Middle tabs: Both directions work
```

### Animation and Feedback

The control provides built-in visual feedback:
- Content slides smoothly during swipe
- Selection indicator animates to new position
- Elastic bounce at edges when no more tabs

## Mobile-Optimized Patterns

### Pattern 1: Bottom Navigation with Swipe

Common mobile app pattern combining bottom tabs with swipe:

```xaml
<tabView:SfTabView EnableSwiping="True"
                   TabBarPlacement="Bottom"
                   TabBarHeight="60">
    <tabView:SfTabView.Items>
        <tabView:SfTabItem Header="Home" 
                           ImageSource="home.png" 
                           ImagePosition="Top" />
        <tabView:SfTabItem Header="Search" 
                           ImageSource="search.png" 
                           ImagePosition="Top" />
        <tabView:SfTabItem Header="Favorites" 
                           ImageSource="heart.png" 
                           ImagePosition="Top" />
        <tabView:SfTabItem Header="Profile" 
                           ImageSource="user.png" 
                           ImagePosition="Top" />
    </tabView:SfTabView.Items>
</tabView:SfTabView>
```

### Pattern 2: Content Viewer with Swipe

Image gallery or content browsing pattern:

```xaml
<tabView:SfTabView EnableSwiping="True"
                   TabBarPlacement="Top"
                   TabWidthMode="SizeToContent">
    <tabView:SfTabView.Items>
        <tabView:SfTabItem Header="Photo 1">
            <tabView:SfTabItem.Content>
                <Image Source="photo1.jpg" Aspect="AspectFill" />
            </tabView:SfTabItem.Content>
        </tabView:SfTabItem>
        <tabView:SfTabItem Header="Photo 2">
            <tabView:SfTabItem.Content>
                <Image Source="photo2.jpg" Aspect="AspectFill" />
            </tabView:SfTabItem.Content>
        </tabView:SfTabItem>
        <tabView:SfTabItem Header="Photo 3">
            <tabView:SfTabItem.Content>
                <Image Source="photo3.jpg" Aspect="AspectFill" />
            </tabView:SfTabItem.Content>
        </tabView:SfTabItem>
    </tabView:SfTabView.Items>
</tabView:SfTabView>
```

### Pattern 3: Onboarding Flow

Tutorial or walkthrough screens:

```xaml
<tabView:SfTabView EnableSwiping="True"
                   TabBarPlacement="None">  <!-- Hide tab bar for onboarding -->
    <tabView:SfTabView.Items>
        <tabView:SfTabItem>
            <tabView:SfTabItem.Content>
                <StackLayout Padding="30" Spacing="20">
                    <Image Source="welcome.png" HeightRequest="200" />
                    <Label Text="Welcome to App" 
                           FontSize="24" 
                           FontAttributes="Bold"
                           HorizontalTextAlignment="Center" />
                    <Label Text="Swipe to continue" 
                           TextColor="Gray"
                           HorizontalTextAlignment="Center" />
                </StackLayout>
            </tabView:SfTabItem.Content>
        </tabView:SfTabItem>
        <!-- More onboarding screens -->
    </tabView:SfTabView.Items>
</tabView:SfTabView>
```

**Note:** While TabBarPlacement="None" hides the tab bar, swipe still works for onboarding flows.

## Conditional Swipe Enabling

### Toggle Based on Context

```csharp
// Enable swipe for mobile, disable for desktop
if (DeviceInfo.Platform == DevicePlatform.Android || 
    DeviceInfo.Platform == DevicePlatform.iOS)
{
    tabView.EnableSwiping = true;
}
else
{
    tabView.EnableSwiping = false;
}

// Enable only in specific orientation
if (DeviceDisplay.MainDisplayInfo.Orientation == DisplayOrientation.Portrait)
{
    tabView.EnableSwiping = true;
}
```

### User Preference

```csharp
// Allow user to toggle swipe navigation
public class SettingsPage : ContentPage
{
    public SettingsPage()
    {
        var swipeSwitch = new Switch
        {
            IsToggled = Preferences.Get("EnableSwipe", true)
        };
        
        swipeSwitch.Toggled += (s, e) =>
        {
            Preferences.Set("EnableSwipe", e.Value);
            // Update all tab views
            MessagingCenter.Send(this, "SwipeSettingChanged", e.Value);
        };
    }
}

// In TabView page
MessagingCenter.Subscribe<SettingsPage, bool>(this, "SwipeSettingChanged", (sender, enabled) =>
{
    tabView.EnableSwiping = enabled;
});
```

### Disable During Edit Mode

```csharp
private bool isEditMode = false;

private void OnEditModeToggled(bool editing)
{
    isEditMode = editing;
    
    // Disable swipe during editing to prevent accidental navigation
    tabView.EnableSwiping = !editing;
    
    if (editing)
    {
        DisplayAlert("Edit Mode", "Swipe disabled while editing", "OK");
    }
}
```

## Platform-Specific Behavior

### Android

- Native touch gestures with material ripple effect
- Smooth hardware-accelerated animations
- Respects system gesture navigation settings

### iOS

- Native UIKit gesture recognizers
- Follows iOS swipe conventions
- Integrates with iOS back swipe gesture

### Windows

- Touch and precision touchpad support
- Mouse drag not supported (use tap navigation instead)
- Works with touch-enabled displays

### macOS

- Trackpad swipe gestures supported
- Two-finger swipe left/right
- Smooth animations

## Combining Swipe with Events

Track swipe navigation through SelectionChanged event:

```csharp
tabView.EnableSwiping = true;

tabView.SelectionChanged += (s, e) =>
{
    // Detect if navigation was likely from swipe
    int difference = e.NewIndex - e.OldIndex;
    
    if (Math.Abs(difference) == 1)
    {
        // Likely swipe navigation (adjacent tab)
        Console.WriteLine($"User swiped {(difference > 0 ? "left" : "right")}");
    }
    else
    {
        // Likely tap navigation (non-adjacent tab)
        Console.WriteLine("User tapped tab");
    }
};
```

## Accessibility Considerations

### Providing Alternatives

Always provide alternative navigation methods when swipe is enabled:

1. **Visible Tab Bar:** Users can still tap tabs
2. **Navigation Buttons:** Add explicit next/previous buttons
3. **Screen Reader Announcements:** Ensure proper labels

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="*" />
        <RowDefinition Height="Auto" />
    </Grid.RowDefinitions>
    
    <!-- Tab View with swipe -->
    <tabView:SfTabView Grid.Row="0" EnableSwiping="True">
        <tabView:SfTabView.Items>
            <!-- Tabs -->
        </tabView:SfTabView.Items>
    </tabView:SfTabView>
    
    <!-- Alternative navigation buttons -->
    <Grid Grid.Row="1" Padding="10" ColumnDefinitions="*,*">
        <Button Grid.Column="0" 
                Text="← Previous" 
                Clicked="OnPreviousClicked"
                AutomationProperties.Name="Navigate to previous tab" />
        <Button Grid.Column="1" 
                Text="Next →" 
                Clicked="OnNextClicked"
                AutomationProperties.Name="Navigate to next tab" />
    </Grid>
</Grid>
```

### Screen Reader Support

```csharp
// Announce tab changes for screen readers
tabView.SelectionChanged += (s, e) =>
{
    var newTab = tabView.Items[e.NewIndex];
    SemanticScreenReader.Announce($"Navigated to {newTab.Header} tab");
};
```

## Performance Optimization

### Lazy Load Content

Prevent loading all tab content upfront when swipe is enabled:

```csharp
tabView.EnableSwiping = true;

tabView.SelectionChanged += (s, e) =>
{
    var selectedTab = tabView.Items[e.NewIndex];
    
    if (selectedTab.Content == null)
    {
        // Load content only when tab is accessed
        selectedTab.Content = LoadTabContent(e.NewIndex);
    }
};

private View LoadTabContent(int tabIndex)
{
    // Expensive content creation
    return new ContentView { /* ... */ };
}
```

### Preload Adjacent Tabs

Improve swipe smoothness by preloading adjacent tabs:

```csharp
tabView.SelectionChanged += (s, e) =>
{
    int currentIndex = e.NewIndex;
    
    // Preload previous tab
    if (currentIndex > 0 && tabView.Items[currentIndex - 1].Content == null)
    {
        tabView.Items[currentIndex - 1].Content = LoadTabContent(currentIndex - 1);
    }
    
    // Preload next tab
    if (currentIndex < tabView.Items.Count - 1 && 
        tabView.Items[currentIndex + 1].Content == null)
    {
        tabView.Items[currentIndex + 1].Content = LoadTabContent(currentIndex + 1);
    }
};
```

## Testing Swipe Gestures

### Emulator/Simulator Testing

**Android Emulator:**
- Click and drag across content area
- Use mouse to simulate touch gestures

**iOS Simulator:**
- Click and drag with mouse
- Use trackpad swipe gestures on Mac

**⚠️ Note:** Emulator swipe may feel less responsive than actual device

### Physical Device Testing

Always test on real devices for accurate swipe behavior:
- Varies by screen size and device responsiveness
- Test with different swipe speeds (slow, fast)
- Test at content edges (first tab, last tab)
- Verify no conflicts with other gestures (zoom, scroll)

## Troubleshooting

**Issue:** Swipe not working  
**Solution:** 
- Verify `EnableSwiping = true`
- Check content doesn't have conflicting gesture recognizers
- Test on actual device (not just emulator)

**Issue:** Swipe conflicts with ScrollView  
**Solution:**
- ScrollView vertical scrolling should work with horizontal swipe
- Avoid nested horizontal ScrollViews
- Consider disabling swipe if horizontal scrolling is needed

**Issue:** Accidental swipes during scrolling  
**Solution:**
- Swipe detection is directional - vertical scrolls shouldn't trigger tab change
- If issues persist, adjust content layout to minimize conflict

**Issue:** Swipe feels unresponsive  
**Solution:**
- Test on actual device (not emulator)
- Reduce content complexity for smoother animations
- Implement lazy loading for heavy content

**Issue:** Can't disable swipe for specific tabs  
**Solution:**
- `EnableSwiping` applies to entire TabView, not individual tabs
- Use SelectionChanging event to prevent navigation if needed

## Best Practices

1. **Mobile-First:** Enable swipe for mobile platforms, consider disabling for desktop
2. **Provide Alternatives:** Always offer tap navigation alongside swipe
3. **User Control:** Allow users to disable swipe in settings if desired
4. **Accessibility:** Ensure screen reader support and alternative navigation
5. **Test on Devices:** Always test swipe on real devices, not just emulators
6. **Performance:** Lazy load content to keep swipe animations smooth
7. **Visual Hints:** Provide subtle UI cues that swipe is available
8. **Respect Platform Conventions:** Don't override system gestures

## Example: Complete Mobile App with Swipe

```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:tabView="clr-namespace:Syncfusion.Maui.TabView;assembly=Syncfusion.Maui.TabView">
    
    <tabView:SfTabView EnableSwiping="True"
                       TabBarPlacement="Bottom"
                       TabBarHeight="60"
                       TabWidthMode="Default"
                       IndicatorPlacement="Top"
                       IndicatorBackground="#6200EE">
        <tabView:SfTabView.Items>
            
            <tabView:SfTabItem Header="Home" 
                               ImageSource="home.png" 
                               ImagePosition="Top"
                               FontSize="12">
                <tabView:SfTabItem.Content>
                    <ScrollView>
                        <StackLayout Padding="20" Spacing="15">
                            <Label Text="Home Feed" FontSize="24" FontAttributes="Bold" />
                            <Label Text="Swipe left to see Explore" TextColor="Gray" />
                            <!-- Home content -->
                        </StackLayout>
                    </ScrollView>
                </tabView:SfTabItem.Content>
            </tabView:SfTabItem>
            
            <tabView:SfTabItem Header="Explore" 
                               ImageSource="compass.png" 
                               ImagePosition="Top"
                               FontSize="12">
                <tabView:SfTabItem.Content>
                    <ScrollView>
                        <StackLayout Padding="20" Spacing="15">
                            <Label Text="Explore Content" FontSize="24" FontAttributes="Bold" />
                            <!-- Explore content -->
                        </StackLayout>
                    </ScrollView>
                </tabView:SfTabItem.Content>
            </tabView:SfTabItem>
            
            <tabView:SfTabItem Header="Notifications" 
                               ImageSource="bell.png" 
                               ImagePosition="Top"
                               FontSize="12">
                <tabView:SfTabItem.Content>
                    <ScrollView>
                        <StackLayout Padding="20" Spacing="15">
                            <Label Text="Notifications" FontSize="24" FontAttributes="Bold" />
                            <!-- Notifications content -->
                        </StackLayout>
                    </ScrollView>
                </tabView:SfTabItem.Content>
            </tabView:SfTabItem>
            
            <tabView:SfTabItem Header="Profile" 
                               ImageSource="user.png" 
                               ImagePosition="Top"
                               FontSize="12">
                <tabView:SfTabItem.Content>
                    <ScrollView>
                        <StackLayout Padding="20" Spacing="15">
                            <Label Text="Your Profile" FontSize="24" FontAttributes="Bold" />
                            <Label Text="Swipe right to go back" TextColor="Gray" />
                            <!-- Profile content -->
                        </StackLayout>
                    </ScrollView>
                </tabView:SfTabItem.Content>
            </tabView:SfTabItem>
            
        </tabView:SfTabView.Items>
    </tabView:SfTabView>
    
</ContentPage>
```
