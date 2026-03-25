# How-To Guide

## Table of Contents
- [Programmatic Tab Selection](#programmatic-tab-selection)
- [Badge and Notification Implementation](#badge-and-notification-implementation)
- [Disable Hover Effect](#disable-hover-effect)
- [Conditional Tab Visibility](#conditional-tab-visibility)
- [Custom Animation Duration](#custom-animation-duration)
- [Tab Accessibility](#tab-accessibility)
- [Scroll Button Customization](#scroll-button-customization)
- [Common Patterns](#common-patterns)
- [Best Practices](#best-practices)
- [How to Optimize Performance with Virtualization](#how-to-optimize-performance-with-virtualization)
- [Common Issues](#common-issues)

---

Practical scenarios and solutions for common Tab View implementation challenges.

## Programmatic Tab Selection

### Select Tab by Index

Use the `SelectedIndex` property to programmatically navigate to a specific tab.

**XAML:**
```xaml
<tabView:SfTabView x:Name="tabView" SelectedIndex="2">
    <tabView:SfTabView.Items>
        <tabView:SfTabItem Header="Home" />
        <tabView:SfTabItem Header="Profile" />
        <tabView:SfTabItem Header="Settings" />
    </tabView:SfTabView.Items>
</tabView:SfTabView>
```

**C#:**
```csharp
// Select third tab (zero-indexed)
tabView.SelectedIndex = 2;

// Select programmatically from button click
private void NavigateToSettings_Clicked(object sender, EventArgs e)
{
    tabView.SelectedIndex = 2;
}
```

### Get Currently Selected Tab

Use `SelectedIndex` property or `IsSelected` property of individual tab items:

```csharp
// Get selected index
int currentTab = tabView.SelectedIndex;

// Check if specific tab is selected
bool isHomeSelected = tabView.Items[0].IsSelected;
```

### Detect Selection Changes

Monitor tab selection using the `SelectionChanged` event:

```csharp
tabView.SelectionChanged += (sender, e) =>
{
    var oldIndex = e.OldIndex;
    var newIndex = e.NewIndex;
    
    Console.WriteLine($"Changed from tab {oldIndex} to {newIndex}");
    
    // Update UI based on selected tab
    if (newIndex == 0)
    {
        // Home tab selected
    }
};
```

## Badge and Notification Implementation

### Add Badge with Count

Display notification counts or status indicators on tab headers.

**XAML:**
```xaml
xmlns:core="clr-namespace:Syncfusion.Maui.Core;assembly=Syncfusion.Maui.Core"

<tabView:SfTabView>
    <tabView:SfTabView.Items>
        <tabView:SfTabItem Header="Messages" 
                           ImageSource="messages.png"
                           ImagePosition="Top"
                           BadgeText="12">
            <tabView:SfTabItem.BadgeSettings>
                <core:BadgeSettings Type="Error" 
                                   FontSize="12" 
                                   FontAttributes="Bold" />
            </tabView:SfTabItem.BadgeSettings>
        </tabView:SfTabItem>
        
        <tabView:SfTabItem Header="Notifications" 
                           ImageSource="bell.png"
                           ImagePosition="Top"
                           BadgeText="3">
            <tabView:SfTabItem.BadgeSettings>
                <core:BadgeSettings Type="Warning" 
                                   FontSize="12" />
            </tabView:SfTabItem.BadgeSettings>
        </tabView:SfTabItem>
        
        <tabView:SfTabItem Header="Profile" 
                           ImageSource="user.png"
                           ImagePosition="Top" />
    </tabView:SfTabView.Items>
</tabView:SfTabView>
```

**C# with ViewModel binding:**
```csharp
using Syncfusion.Maui.Core;
using Syncfusion.Maui.TabView;

public class TabViewModel : INotifyPropertyChanged
{
    private string _messageCount = "12";
    
    public string MessageCount
    {
        get => _messageCount;
        set
        {
            _messageCount = value;
            OnPropertyChanged();
        }
    }
    
    // Update badge dynamically
    public void UpdateMessageCount(int count)
    {
        MessageCount = count.ToString();
    }
}

// In code-behind or view model
var messagesTab = new SfTabItem
{
    Header = "Messages",
    ImageSource = "messages.png",
    ImagePosition = TabImagePosition.Top,
    BadgeText = "12",
    BadgeSettings = new BadgeSettings
    {
        Type = BadgeType.Error,
        FontSize = 12,
        FontAttributes = FontAttributes.Bold
    }
};
```

### Badge Types and Styling

Customize badge appearance for different notification types:

**XAML:**
```xaml
xmlns:core="clr-namespace:Syncfusion.Maui.Core;assembly=Syncfusion.Maui.Core"

<!-- Error/Critical (Red) -->
<tabView:SfTabItem BadgeText="5">
    <tabView:SfTabItem.BadgeSettings>
        <core:BadgeSettings Type="Error" 
                           FontSize="14"
                           FontAttributes="Bold" />
    </tabView:SfTabItem.BadgeSettings>
</tabView:SfTabItem>

<!-- Warning (Orange/Yellow) -->
<tabView:SfTabItem BadgeText="2">
    <tabView:SfTabItem.BadgeSettings>
        <core:BadgeSettings Type="Warning" 
                           FontSize="12" />
    </tabView:SfTabItem.BadgeSettings>
</tabView:SfTabItem>

<!-- Success (Green) -->
<tabView:SfTabItem BadgeText="✓">
    <tabView:SfTabItem.BadgeSettings>
        <core:BadgeSettings Type="Success" 
                           FontSize="14" />
    </tabView:SfTabItem.BadgeSettings>
</tabView:SfTabItem>

<!-- Info (Blue) -->
<tabView:SfTabItem BadgeText="New">
    <tabView:SfTabItem.BadgeSettings>
        <core:BadgeSettings Type="Info" 
                           FontSize="10"
                           FontFamily="OpenSans-SemiBold" />
    </tabView:SfTabItem.BadgeSettings>
</tabView:SfTabItem>

<!-- Custom styling -->
<tabView:SfTabItem BadgeText="99+">
    <tabView:SfTabItem.BadgeSettings>
        <core:BadgeSettings Type="None"
                           Background="#FF6B6B"
                           TextColor="White"
                           Stroke="White"
                           StrokeThickness="2"
                           FontSize="12"
                           FontAttributes="Bold"
                           CornerRadius="10"
                           Offset="-5,-5" />
    </tabView:SfTabItem.BadgeSettings>
</tabView:SfTabItem>
```

### Dynamic Badge Updates

Update badges in response to data changes:

```csharp
// Update badge text
tabView.Items[0].BadgeText = "15";

// Show/hide badge
tabView.Items[0].BadgeText = hasNewMessages ? messageCount.ToString() : string.Empty;

// Update from ViewModel
public class MainViewModel : INotifyPropertyChanged
{
    public ObservableCollection<TabItemModel> TabItems { get; set; }
    
    public async Task UpdateNotifications()
    {
        var count = await GetUnreadNotificationsCount();
        var notificationTab = TabItems.FirstOrDefault(t => t.Title == "Notifications");
        
        if (notificationTab != null)
        {
            notificationTab.BadgeText = count > 0 ? count.ToString() : string.Empty;
        }
    }
}

// With ItemsSource binding
public class TabItemModel : INotifyPropertyChanged
{
    private string _badgeText;
    
    public string BadgeText
    {
        get => _badgeText;
        set
        {
            _badgeText = value;
            OnPropertyChanged();
        }
    }
}
```

**XAML with data binding:**
```xaml
<tabView:SfTabView ItemsSource="{Binding TabItems}">
    <tabView:SfTabView.HeaderItemTemplate>
        <DataTemplate>
            <tabView:SfTabItem Header="{Binding Title}"
                              ImageSource="{Binding Icon}"
                              BadgeText="{Binding BadgeText}">
                <tabView:SfTabItem.BadgeSettings>
                    <core:BadgeSettings Type="Error" FontSize="12" />
                </tabView:SfTabItem.BadgeSettings>
            </tabView:SfTabItem>
        </DataTemplate>
    </tabView:SfTabView.HeaderItemTemplate>
</tabView:SfTabView>
```

### Badge Use Cases

| Scenario | Badge Text | Badge Type | Use When |
|----------|-----------|------------|----------|
| Unread Messages | "5", "23", "99+" | Error | User has new messages to read |
| Pending Tasks | "3", "8" | Warning | Tasks require attention |
| Notifications | "2", "10" | Information | General updates available |
| Completed | "✓", "Done" | Success | Task/process completed |
| Status Indicator | "New", "Beta" | Information | Feature status |
| No Badge | "" (empty) | N/A | No notifications/updates |

## Disable Hover Effect

Remove hover background effect on Windows/macOS:

**XAML:**
```xaml
<ContentPage.Resources>
    <Color x:Key="SfTabViewHoverBackground">Transparent</Color>
</ContentPage.Resources>

<tabView:SfTabView EnableRippleAnimation="False">
    <tabView:SfTabView.Items>
        <tabView:SfTabItem Header="Home" />
    </tabView:SfTabView.Items>
</tabView:SfTabView>
```

**Use when:**
- Custom tab styling conflicts with hover effect
- Building mobile-first apps where hover isn't relevant
- Custom interaction patterns

## Conditional Tab Visibility

### Show/Hide Tabs Based on Logic

```csharp
// Hide admin tab for non-admin users
public void ConfigureTabsForUser(bool isAdmin)
{
    var adminTab = tabView.Items.FirstOrDefault(t => t.Header?.ToString() == "Admin");
    
    if (adminTab != null)
    {
        if (!isAdmin)
        {
            tabView.Items.Remove(adminTab);
        }
        else
        {
            adminTab.IsEnabled = true;
        }
    }
}

// Show tabs after authentication
public async Task LoadUserTabs()
{
    var user = await GetCurrentUser();
    
    tabView.Items.Clear();
    
    // Always add these
    tabView.Items.Add(new SfTabItem { Header = "Home" });
    tabView.Items.Add(new SfTabItem { Header = "Profile" });
    
    // Conditional tabs
    if (user.HasPremium)
    {
        tabView.Items.Add(new SfTabItem { Header = "Premium Features" });
    }
    
    if (user.IsAdmin)
    {
        tabView.Items.Add(new SfTabItem { Header = "Admin Panel" });
    }
}
```

## Custom Animation Duration

Control transition speed when switching tabs:

**XAML:**
```xaml
<tabView:SfTabView ContentTransitionDuration="500">
    <!-- 500ms transition (default is 300ms) -->
</tabView:SfTabView>
```

**C#:**
```csharp
// Fast transition (mobile feel)
tabView.ContentTransitionDuration = 200;

// Slow transition (emphasis)
tabView.ContentTransitionDuration = 800;

// No animation
tabView.ContentTransitionDuration = 0;
```

**Use cases:**
- Mobile: 200-300ms (snappy)
- Desktop: 300-500ms (smooth)
- Emphasis: 600-1000ms (draw attention)
- Data-heavy: 0ms (instant, no animation overhead)

## Tab Accessibility

### Font Auto-Scaling for Accessibility

Enable automatic font scaling based on OS settings:

**XAML:**
```xaml
<tabView:SfTabView>
    <tabView:SfTabView.Items>
        <tabView:SfTabItem Header="Home" 
                           FontAutoScalingEnabled="True" />
        <tabView:SfTabItem Header="Settings" 
                           FontAutoScalingEnabled="True" />
    </tabView:SfTabView.Items>
</tabView:SfTabView>
```

**C#:**
```csharp
foreach (var item in tabView.Items)
{
    item.FontAutoScalingEnabled = true;
}
```

### Semantic Labels for Screen Readers

```csharp
// Add semantic descriptions
tabView.Items[0].AutomationId = "HomeTab";
tabView.Items[1].AutomationId = "ProfileTab";
tabView.Items[2].AutomationId = "SettingsTab";
```

## Scroll Button Customization

Enable and customize scroll buttons for many tabs:

**XAML:**
```xaml
<tabView:SfTabView TabWidthMode="SizeToContent"
                   IsScrollButtonEnabled="True"
                   ScrollButtonBackground="#6200EE"
                   ScrollButtonColor="White">
    <tabView:SfTabView.Items>
        <tabView:SfTabItem Header="Tab 1" />
        <tabView:SfTabItem Header="Tab 2" />
        <tabView:SfTabItem Header="Tab 3" />
        <!-- Many more tabs -->
    </tabView:SfTabView.Items>
</tabView:SfTabView>
```

**C#:**
```csharp
tabView.TabWidthMode = TabWidthMode.SizeToContent;
tabView.IsScrollButtonEnabled = true;
tabView.ScrollButtonBackground = new SolidColorBrush(Color.FromArgb("#6200EE"));
tabView.ScrollButtonColor = Colors.White;
```

**Use when:**
- More than 5-7 tabs with variable width
- Desktop applications with many sections
- Document/browser-style interfaces

## Common Patterns

### Wizard/Stepper Pattern

```csharp
public class WizardPage : ContentPage
{
    private SfTabView tabView;
    
    public WizardPage()
    {
        tabView = new SfTabView
        {
            SelectedIndex = 0,
            EnableSwiping  = false // Prevent manual navigation
        };
        
        // Add wizard steps as tabs
        tabView.Items.Add(CreateStep("Personal Info", 1));
        tabView.Items.Add(CreateStep("Address", 2));
        tabView.Items.Add(CreateStep("Payment", 3));
        tabView.Items.Add(CreateStep("Confirm", 4));
        
        Content = tabView;
    }
    
    private SfTabItem CreateStep(string title, int step)
    {
        var tabItem = new SfTabItem
        {
            Header = $"Step {step}",
            Content = new StackLayout
            {
                Children =
                {
                    new Label { Text = title, FontSize = 24 },
                    new Button 
                    { 
                        Text = "Next", 
                        Command = new Command(() => GoToNextStep())
                    }
                }
            }
        };
        
        return tabItem;
    }
    
    private void GoToNextStep()
    {
        if (tabView.SelectedIndex < tabView.Items.Count - 1)
        {
            tabView.SelectedIndex++;
        }
        else
        {
            // Wizard complete
            CompleteWizard();
        }
    }
}
```

### Dashboard Pattern

```csharp
public class DashboardPage : ContentPage
{
    public DashboardPage()
    {
        var tabView = new SfTabView
        {
            TabBarPlacement = TabBarPlacement.Top,
            TabBarHeight = 48,
            TabWidthMode = TabWidthMode.Default
        };
        
        // Overview tab
        tabView.Items.Add(new SfTabItem
        {
            Header = "Overview",
            ImageSource = "dashboard.png",
            ImagePosition = TabImagePosition.Left,
            Content = CreateOverviewContent()
        });
        
        // Analytics tab with badge
        var analyticsTab = new SfTabItem
        {
            Header = "Analytics",
            ImageSource = "chart.png",
            ImagePosition = TabImagePosition.Left,
            BadgeText = "New",
            BadgeSettings = new BadgeSettings
            {
                Type = BadgeType.Success,
                FontSize = 10
            },
            Content = CreateAnalyticsContent()
        };
        tabView.Items.Add(analyticsTab);
        
        // Reports tab
        tabView.Items.Add(new SfTabItem
        {
            Header = "Reports",
            ImageSource = "document.png",
            ImagePosition = TabImagePosition.Left,
            Content = CreateReportsContent()
        });
        
        Content = tabView;
    }
}
```

## Best Practices

1. **Badge Numbers:**
   - Use "99+" for counts over 99
   - Clear badge when user views content
   - Update badges in real-time if possible

2. **Programmatic Navigation:**
   - Validate SelectedIndex is within bounds
   - Use SelectionChanging event to cancel invalid transitions
   - Consider animation duration for UX

3. **Accessibility:**
   - Enable FontAutoScalingEnabled for better readability
   - Provide meaningful header text
   - Test with screen readers

4. **Performance:**
   - Enable virtualization with `EnableVirtualization="True"` for many tabs
   - Use lazy loading for tab content
   - Set ContentTransitionDuration to 0 for data-heavy views
   - Consider using ItemsSource for dynamic tabs

## How to Optimize Performance with Virtualization

For applications with many tab items or complex content, enable virtualization to improve initial loading performance:

**XAML:**
```xaml
<tabView:SfTabView EnableVirtualization="True" 
                   TabWidthMode="SizeToContent">
    <tabView:SfTabView.Items>
        <!-- Many tab items -->
        <tabView:SfTabItem Header="Tab 1" />
        <tabView:SfTabItem Header="Tab 2" />
        <!-- ... 20+ tabs ... -->
    </tabView:SfTabView.Items>
</tabView:SfTabView>
```

**C#:**
```csharp
var tabView = new SfTabView
{
    EnableVirtualization = true,
    TabWidthMode = TabWidthMode.SizeToContent
};

// Add many tabs
for (int i = 1; i <= 50; i++)
{
    tabView.Items.Add(new SfTabItem 
    { 
        Header = $"Tab {i}",
        Content = new ContentView() // Content loaded on-demand
    });
}
```

**When to Use:**
- Applications with 10+ tabs
- Tabs with heavy content (images, charts, data grids)
- ItemsSource binding with large collections
- Nested tab views with multiple levels

**Benefits:**
- Faster initial rendering
- Reduced memory footprint
- Improved scrolling performance
- Better experience on lower-end devices

## Common Issues

**Issue:** Badge not visible  
**Solution:** Ensure you've added the Syncfusion.Maui.Core namespace for BadgeSettings

**Issue:** SelectedIndex change doesn't trigger SelectionChanged  
**Solution:** SelectionChanged only fires for interactive changes. Use SelectionChanging for programmatic tracking

**Issue:** Badge text truncated  
**Solution:** Increase BadgeSettings.FontSize or use shorter text (e.g., "99+" instead of "999")

**Issue:** Tabs not scrolling with many items  
**Solution:** Set TabWidthMode to SizeToContent and IsScrollButtonEnabled to true
