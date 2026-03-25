# Tab Bar Configuration

## Table of Contents
- [Tab Width Modes](#tab-width-modes)
- [TabBarHeight Customization](#tabbarheight-customization)
- [TabBarPlacement](#tabbarplacement)
- [TabBarBackground Styling](#tabbarbackground-styling)
- [Indicator Configuration](#indicator-configuration)
- [Tab Spacing and Padding](#tab-spacing-and-padding)
- [Complete Configuration Examples](#complete-configuration-examples)

---

Comprehensive guide for configuring the tab bar appearance, layout, and behavior in .NET MAUI Tab View.

## Tab Width Modes

The `TabWidthMode` property determines how tab widths are calculated.

### Default Mode (Fixed Width)

Divides available width equally among all tabs. Creates a fixed, non-scrollable tab bar.

**Use When:**
- You have 2-4 tabs
- Want equal-sized tabs
- Screen space is sufficient for all tabs

**XAML:**
```xaml
<tabView:SfTabView TabWidthMode="Default">
    <tabView:SfTabView.Items>
        <tabView:SfTabItem Header="Home" />
        <tabView:SfTabItem Header="Profile" />
        <tabView:SfTabItem Header="Settings" />
    </tabView:SfTabView.Items>
</tabView:SfTabView>
```

**C#:**
```csharp
tabView.TabWidthMode = TabWidthMode.Default;
```

**⚠️ Warning:** With more than 4 tabs, text may be truncated. Use SizeToContent instead.

### SizeToContent Mode (Scrollable)

Each tab width fits its content (text/icon). Enables horizontal scrolling if tabs exceed screen width.

**Use When:**
- You have 5+ tabs
- Tab labels vary in length
- Want content-based sizing

**XAML:**
```xaml
<tabView:SfTabView TabWidthMode="SizeToContent">
    <tabView:SfTabView.Items>
        <tabView:SfTabItem Header="Dashboard" />
        <tabView:SfTabItem Header="Reports" />
        <tabView:SfTabItem Header="Analytics" />
        <tabView:SfTabItem Header="Settings" />
        <tabView:SfTabItem Header="Help" />
        <tabView:SfTabItem Header="About" />
    </tabView:SfTabView.Items>
</tabView:SfTabView>
```

**C#:**
```csharp
tabView.TabWidthMode = TabWidthMode.SizeToContent;
```

**Visual Comparison:**

| Mode | Behavior | Scrolling | Best For |
|------|----------|-----------|----------|
| `Default` | Equal width tabs | None | 2-4 tabs, consistent labels |
| `SizeToContent` | Content-fit width | Horizontal | 5+ tabs, varying labels |

## TabBarHeight Customization

Control the height of the tab bar. Default is 48 pixels.

### Standard Heights

```csharp
// Default height - text only headers
tabView.TabBarHeight = 48;

// Recommended for icon + text (top/bottom position)
tabView.TabBarHeight = 72;

// Custom height
tabView.TabBarHeight = 60;
```

### Height Recommendations

| Header Content | Recommended Height | Reason |
|----------------|-------------------|--------|
| Text only | 48 | Default, sufficient space |
| Icon only | 48-56 | Accommodate icon size |
| Icon + Text (Horizontal) | 56-64 | Side-by-side layout |
| Icon + Text (Vertical) | 72 | Stacked layout needs more height |

### Example with Icons

**XAML:**
```xaml
<tabView:SfTabView TabBarHeight="72">
    <tabView:SfTabView.Items>
        <tabView:SfTabItem Header="Home" 
                           ImageSource="home.png"
                           ImagePosition="Top" />
        <tabView:SfTabItem Header="Search" 
                           ImageSource="search.png"
                           ImagePosition="Top" />
        <tabView:SfTabItem Header="Profile" 
                           ImageSource="profile.png"
                           ImagePosition="Top" />
    </tabView:SfTabView.Items>
</tabView:SfTabView>
```

**C#:**
```csharp
tabView.TabBarHeight = 72;

tabView.Items.Add(new SfTabItem
{
    Header = "Home",
    ImageSource = "home.png",
    ImagePosition = TabImagePosition.Top
});
```

## TabBarPlacement

Position the tab bar at the top, bottom, left, or right of the control.

### Top Placement (Default)

Standard desktop/web pattern. Tab bar appears above content.

**XAML:**
```xaml
<tabView:SfTabView TabBarPlacement="Top">
    <tabView:SfTabView.Items>
        <!-- Tab items -->
    </tabView:SfTabView.Items>
</tabView:SfTabView>
```

**C#:**
```csharp
tabView.TabBarPlacement = TabBarPlacement.Top;
```

### Bottom Placement

Common mobile app navigation pattern. Tab bar appears below content.

**XAML:**
```xaml
<tabView:SfTabView TabBarPlacement="Bottom"
                   TabBarHeight="60">
    <tabView:SfTabView.Items>
        <tabView:SfTabItem Header="Home" ImageSource="home.png" ImagePosition="Top" />
        <tabView:SfTabItem Header="Search" ImageSource="search.png" ImagePosition="Top" />
        <tabView:SfTabItem Header="Notifications" ImageSource="bell.png" ImagePosition="Top" />
        <tabView:SfTabItem Header="Profile" ImageSource="user.png" ImagePosition="Top" />
    </tabView:SfTabView.Items>
</tabView:SfTabView>
```

**C#:**
```csharp
tabView.TabBarPlacement = TabBarPlacement.Bottom;
tabView.TabBarHeight = 60;
```

### Left Placement

Vertical tab bar positioned on the left side. Useful for desktop applications with many tabs or wide-screen layouts.

**XAML:**
```xaml
<tabView:SfTabView TabBarPlacement="Left"
                   TabBarSize="150">
    <tabView:SfTabView.Items>
        <tabView:SfTabItem Header="Dashboard" ImageSource="dashboard.png" ImagePosition="Top" />
        <tabView:SfTabItem Header="Analytics" ImageSource="chart.png" ImagePosition="Top" />
        <tabView:SfTabItem Header="Reports" ImageSource="report.png" ImagePosition="Top" />
        <tabView:SfTabItem Header="Settings" ImageSource="settings.png" ImagePosition="Top" />
    </tabView:SfTabView.Items>
</tabView:SfTabView>
```

**C#:**
```csharp
tabView.TabBarPlacement = TabBarPlacement.Left;
tabView.TabBarSize = 150;  // Controls width when placement is Left
```

**Note:** When `TabBarPlacement` is set to `Left`, the `TabBarHeight` property controls the **width** of the vertical tab bar.

### Right Placement

Vertical tab bar positioned on the right side. Alternative to left placement for specific design requirements.

**XAML:**
```xaml
<tabView:SfTabView TabBarPlacement="Right"
                   TabBarSize="150">
    <tabView:SfTabView.Items>
        <tabView:SfTabItem Header="Messages" ImageSource="mail.png" ImagePosition="Top" />
        <tabView:SfTabItem Header="Calendar" ImageSource="calendar.png" ImagePosition="Top" />
        <tabView:SfTabItem Header="Tasks" ImageSource="tasks.png" ImagePosition="Top" />
        <tabView:SfTabItem Header="Notes" ImageSource="notes.png" ImagePosition="Top" />
    </tabView:SfTabView.Items>
</tabView:SfTabView>
```

**C#:**
```csharp
tabView.TabBarPlacement = TabBarPlacement.Right;
tabView.TabBarSize = 150;  // Controls width when placement is Right
```

**Note:** When `TabBarPlacement` is set to `Right`, the `TabBarSize` property controls the **width** of the vertical tab bar.

**Use Cases:**

| Placement | Best For | Common Patterns |
|-----------|----------|-----------------|
| `Top` | Desktop apps, web-like UIs | Documents, settings, dashboards |
| `Bottom` | Mobile apps | Primary navigation, main sections |
| `Left` | Desktop apps with many tabs, wide-screen layouts | Admin panels, IDE-style interfaces, tool palettes |
| `Right` | Alternative vertical layout, RTL languages | Side panels, auxiliary navigation, secondary tools |

## TabBarBackground Styling

Customize the tab bar background with solid colors or gradients.

### Solid Color Background

**XAML:**
```xaml
<tabView:SfTabView TabBarBackground="#6200EE">
    <tabView:SfTabView.Items>
        <!-- Tab items -->
    </tabView:SfTabView.Items>
</tabView:SfTabView>
```

**C#:**
```csharp
tabView.TabBarBackground = new SolidColorBrush(Color.FromArgb("#6200EE"));
```

### Gradient Background

**XAML:**
```xaml
<tabView:SfTabView>
    <tabView:SfTabView.TabBarBackground>
        <LinearGradientBrush StartPoint="0,0" EndPoint="1,0">
            <GradientStop Color="#6200EE" Offset="0.0" />
            <GradientStop Color="#3700B3" Offset="1.0" />
        </LinearGradientBrush>
    </tabView:SfTabView.TabBarBackground>
    <tabView:SfTabView.Items>
        <!-- Tab items -->
    </tabView:SfTabView.Items>
</tabView:SfTabView>
```

**C#:**
```csharp
var gradient = new LinearGradientBrush
{
    StartPoint = new Point(0, 0),
    EndPoint = new Point(1, 0),
    GradientStops = new GradientStopCollection
    {
        new GradientStop { Color = Color.FromArgb("#6200EE"), Offset = 0.0f },
        new GradientStop { Color = Color.FromArgb("#3700B3"), Offset = 1.0f }
    }
};
tabView.TabBarBackground = gradient;
```

### Transparent Background

```csharp
tabView.TabBarBackground = new SolidColorBrush(Colors.Transparent);
```

## Indicator Configuration

The selection indicator shows which tab is currently active.

### IndicatorBackground

Set the indicator color:

**XAML:**
```xaml
<tabView:SfTabView IndicatorBackground="#FF6B6B">
    <tabView:SfTabView.Items>
        <!-- Tab items -->
    </tabView:SfTabView.Items>
</tabView:SfTabView>
```

**C#:**
```csharp
tabView.IndicatorBackground = new SolidColorBrush(Color.FromArgb("#FF6B6B"));
```

### IndicatorPlacement

Control where the indicator appears relative to the tab.

**Options:**
- `Top`: Indicator at top edge
- `Bottom`: Indicator at bottom edge (default)
- `Fill`: Indicator fills entire tab background
- `None`: No indicator shown

**XAML Examples:**

```xaml
<!-- Indicator at top -->
<tabView:SfTabView IndicatorPlacement="Top"
                   IndicatorBackground="#6200EE">
    <tabView:SfTabView.Items>
        <!-- Tab items -->
    </tabView:SfTabView.Items>
</tabView:SfTabView>

<!-- Indicator at bottom -->
<tabView:SfTabView IndicatorPlacement="Bottom"
                   IndicatorBackground="#6200EE">
    <tabView:SfTabView.Items>
        <!-- Tab items -->
    </tabView:SfTabView.Items>
</tabView:SfTabView>

<!-- Fill entire tab background -->
<tabView:SfTabView IndicatorPlacement="Fill"
                   IndicatorBackground="#E3F2FD">
    <tabView:SfTabView.Items>
        <!-- Tab items -->
    </tabView:SfTabView.Items>
</tabView:SfTabView>

<!-- Indicator at left -->
<tabView:SfTabView IndicatorPlacement="Left">
    <tabView:SfTabView.Items>
        <!-- Tab items -->
    </tabView:SfTabView.Items>
</tabView:SfTabView>

<!-- Indicator at right -->
<tabView:SfTabView IndicatorPlacement="Right">
    <tabView:SfTabView.Items>
        <!-- Tab items -->
    </tabView:SfTabView.Items>
</tabView:SfTabView>
```

**C#:**
```csharp
// Top indicator
tabView.IndicatorPlacement = TabIndicatorPlacement.Top;
tabView.IndicatorBackground = new SolidColorBrush(Color.FromArgb("#6200EE"));

// Bottom indicator
tabView.IndicatorPlacement = TabIndicatorPlacement.Bottom;

// Fill background
tabView.IndicatorPlacement = TabIndicatorPlacement.Fill;
tabView.IndicatorBackground = new SolidColorBrush(Color.FromArgb("#E3F2FD"));

```

### IndicatorWidthMode

Control how the indicator width is calculated.

**XAML:**
```xaml
<!-- Fit to header text width -->
<tabView:SfTabView IndicatorWidthMode="Fit">
    <tabView:SfTabView.Items>
        <!-- Tab items -->
    </tabView:SfTabView.Items>
</tabView:SfTabView>

<!-- Stretch to full tab width -->
<tabView:SfTabView IndicatorWidthMode="Stretch">
    <tabView:SfTabView.Items>
        <!-- Tab items -->
    </tabView:SfTabView.Items>
</tabView:SfTabView>
```

**C#:**
```csharp
// Fit to content
tabView.IndicatorWidthMode = IndicatorWidthMode.Fit;

// Stretch to full width
tabView.IndicatorWidthMode = IndicatorWidthMode.Stretch;
```

## Tab Spacing and Padding

Control spacing between tabs and internal padding.

### TabHeaderPadding

Adjust padding inside each tab header:

**XAML:**
```xaml
<tabView:SfTabView TabHeaderPadding="15,10,15,10">
    <tabView:SfTabView.Items>
        <tabView:SfTabItem Header="Home" />
        <tabView:SfTabItem Header="Profile" />
    </tabView:SfTabView.Items>
</tabView:SfTabView>
```

**C#:**
```csharp
var tabView = SfTabView
{
    TabHeaderPadding = new Thickness(15, 10, 15, 10)
};
var tabItems = new TabItemCollection
{
    new SfTabItem
    {
        Header = "Home"
    }
};
tabView.Items = tabItems;
```

## Complete Configuration Examples

### Example 1: Mobile Bottom Navigation

```xaml
<tabView:SfTabView TabBarPlacement="Bottom"
                   TabBarHeight="60"
                   TabWidthMode="Default"
                   TabBarBackground="White"
                   IndicatorPlacement="Top"
                   IndicatorBackground="#6200EE">
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

### Example 2: Desktop Scrollable Tabs

```xaml
<tabView:SfTabView TabBarPlacement="Top"
                   TabBarHeight="48"
                   TabWidthMode="SizeToContent"
                   TabBarBackground="#F5F5F5"
                   IndicatorPlacement="Bottom"
                   IndicatorBackground="#FF6B6B">
    <tabView:SfTabView.Items>
        <tabView:SfTabItem Header="Dashboard" />
        <tabView:SfTabItem Header="User Management" />
        <tabView:SfTabItem Header="Reports" />
        <tabView:SfTabItem Header="Analytics" />
        <tabView:SfTabItem Header="Settings" />
        <tabView:SfTabItem Header="Help & Support" />
    </tabView:SfTabView.Items>
</tabView:SfTabView>
```

### Example 3: Material Design Style

```xaml
<tabView:SfTabView TabBarPlacement="Top"
                   TabBarHeight="48"
                   TabWidthMode="Default"
                   TabBarBackground="#6200EE"
                   IndicatorPlacement="Bottom"
                   IndicatorBackground="White">
    <tabView:SfTabView.Items>
        <tabView:SfTabItem Header="RECENT" TextColor="White" />
        <tabView:SfTabItem Header="FAVORITES" TextColor="White" />
        <tabView:SfTabItem Header="NEARBY" TextColor="White" />
    </tabView:SfTabView.Items>
</tabView:SfTabView>
```

### Example 4: Fill Indicator Style

```xaml
<tabView:SfTabView TabBarPlacement="Top"
                   TabBarHeight="50"
                   TabWidthMode="Default"
                   TabBarBackground="#F0F0F0"
                   IndicatorPlacement="Fill"
                   IndicatorBackground="#E3F2FD">
    <tabView:SfTabView.Items>
        <tabView:SfTabItem Header="Overview" />
        <tabView:SfTabItem Header="Details" />
        <tabView:SfTabItem Header="Reviews" />
    </tabView:SfTabView.Items>
</tabView:SfTabView>
```

### Example 5: Vertical Left Sidebar Navigation (Desktop/Tablet)

```xaml
<tabView:SfTabView TabBarPlacement="Left"
                   TabBarSize="200"
                   TabWidthMode="SizeToContent"
                   TabBarBackground="#263238"
                   IndicatorPlacement="Fill"
                   IndicatorBackground="#37474F">
    <tabView:SfTabView.Items>
        <tabView:SfTabItem Header="Dashboard" 
                           ImageSource="dashboard.png" 
                           ImagePosition="Top"
                           TextColor="White" />
        <tabView:SfTabItem Header="Projects" 
                           ImageSource="projects.png" 
                           ImagePosition="Top"
                           TextColor="White" />
        <tabView:SfTabItem Header="Tasks" 
                           ImageSource="tasks.png" 
                           ImagePosition="Top"
                           TextColor="White" />
        <tabView:SfTabItem Header="Reports" 
                           ImageSource="reports.png" 
                           ImagePosition="Top"
                           TextColor="White" />
        <tabView:SfTabItem Header="Settings" 
                           ImageSource="settings.png" 
                           ImagePosition="Top"
                           TextColor="White" />
    </tabView:SfTabView.Items>
</tabView:SfTabView>
```

**C# alternative for vertical navigation:**
```csharp
var tabView = new SfTabView
{
    TabBarPlacement = TabBarPlacement.Left,
    TabBarHeight = 200, // Acts as width for vertical placement
    TabWidthMode = TabWidthMode.SizeToContent,
    TabBarBackground = new SolidColorBrush(Color.FromArgb("#263238")),
    IndicatorPlacement = TabIndicatorPlacement.Fill,
    IndicatorBackground = new SolidColorBrush(Color.FromArgb("#37474F"))
};

// Add vertical navigation items
tabView.Items.Add(new SfTabItem 
{ 
    Header = "Dashboard", 
    ImageSource = "dashboard.png", 
    ImagePosition = TabImagePosition.Top,
    TextColor = Colors.White
});
// Add remaining items...
```

### Example 6: Right-Side Vertical Panel (Wide Screens)

```xaml
<tabView:SfTabView TabBarPlacement="Right"
                   TabBarHeight="150"
                   TabWidthMode="Default"
                   TabBarBackground="#FAFAFA"
                   IndicatorPlacement="Fill"
                   IndicatorBackground="#E0E0E0">
    <tabView:SfTabView.Items>
        <tabView:SfTabItem Header="Properties" 
                           ImageSource="properties.png" 
                           ImagePosition="Top" />
        <tabView:SfTabItem Header="Layers" 
                           ImageSource="layers.png" 
                           ImagePosition="Top" />
        <tabView:SfTabItem Header="History" 
                           ImageSource="history.png" 
                           ImagePosition="Top" />
        <tabView:SfTabItem Header="Info" 
                           ImageSource="info.png" 
                           ImagePosition="Top" />
    </tabView:SfTabView.Items>
</tabView:SfTabView>
```

**Use case:** IDE-style tools panel on right side for design/development tools

## Best Practices

1. **Choose appropriate width mode:**
   - Use `Default` for 2-4 tabs with similar label lengths
   - Use `SizeToContent` for 5+ tabs or varying label lengths

2. **Set proper TabBarHeight and TabBarSize:**
   - **Horizontal placements (Top/Bottom):**
     - 48px for text-only headers
     - 72px for icon + text with vertical layout
     - 56-64px for icon + text with horizontal layout
   - **Vertical placements (Left/Right):**
     - 150-200px acts as tab bar width
     - Adjust based on longest header text + icon + padding

3. **Match indicator placement to TabBarPlacement:**
   - Top tab bar → Bottom indicator (most common)
   - Bottom tab bar → Top indicator
   - Left/Right tab bar → Fill indicator (recommended for vertical)
   - Fill indicator for subtle selection highlight

4. **Consider platform conventions:**
   - **Mobile:** Bottom placement, icons with labels, portrait orientation
   - **Desktop:** Top placement (documents/pages) or Left placement (navigation/tools)
   - **Tablet:** Top/Bottom for landscape, Left for landscape split view
   - **Wide screens:** Left/Right vertical sidebars for persistent navigation

5. **Maintain sufficient contrast** between TabBarBackground and tab text/icons

6. **Test scrolling behavior** with SizeToContent mode on different screen sizes

7. **Vertical placement considerations:**
   - Use Left placement for primary navigation (common pattern)
   - Use Right placement for contextual tools/properties panels
   - Set adequate TabBarSize (acts as width) to prevent text truncation
   - Consider icon + text with `ImagePosition="Top"` for better vertical flow
   - Test on different screen widths - vertical bars work best on tablet/desktop

## Common Issues

**Issue:** Tabs are truncated with Default mode  
**Solution:** Switch to SizeToContent mode or reduce tab count

**Issue:** Indicator not visible  
**Solution:** Check IndicatorBackground contrasts with TabBarBackground, verify IndicatorPlacement is not None

**Issue:** Tab bar too small for icon + text  
**Solution:** Increase TabBarSize to 72 for vertical icon position (Top/Bottom placement)

**Issue:** Horizontal scrolling not working  
**Solution:** Ensure TabWidthMode is set to SizeToContent

**Issue:** Vertical tab bar (Left/Right) text is truncated  
**Solution:** Increase TabBarSize value (it controls width for vertical placements). Start with 150-200px and adjust based on content

**Issue:** Vertical tabs not displaying properly on mobile  
**Solution:** Vertical placements (Left/Right) are optimized for tablet/desktop. For mobile, prefer Top/Bottom placement with responsive layout detection

**Issue:** Left/Right placement indicator not visible  
**Solution:** Use `IndicatorPlacement="Fill"` for vertical tab bars instead of Top/Bottom placement. Fill provides better visual feedback for vertical layouts
