# Responsive Layouts, RTL, and Accessibility

## Table of Contents
- [Horizontal vs Vertical Layouts](#horizontal-vs-vertical-layouts)
- [Responsive Design for Multiple Devices](#responsive-design-for-multiple-devices)
- [Handling Orientation Changes](#handling-orientation-changes)
- [RTL (Right-to-Left) Support](#rtl-right-to-left-support)
- [Accessibility Features](#accessibility-features)
- [Cross-Platform Consistency](#cross-platform-consistency)
- [Dashboard and Workspace Patterns](#dashboard-and-workspace-patterns)

## Horizontal vs Vertical Layouts

The Grid Splitter supports two primary arrangements for panes: horizontal (side-by-side) and vertical (stacked).

### Horizontal Orientation (Side-by-Side)

Panes are arranged left-to-right. Dragging separators changes **widths**. Best for:
- Sidebar + content layouts
- Navigation + main view
- Multi-column data grids
- Split editors (code + preview)

```xaml
<gridSplitter:SfGridSplitter Orientation="Horizontal">
    <gridSplitter:SplitterPane Size="250">
        <!-- Left pane -->
    </gridSplitter:SplitterPane>
    <gridSplitter:SplitterPane Size="1*">
        <!-- Right pane -->
    </gridSplitter:SplitterPane>
</gridSplitter:SfGridSplitter>
```

### Vertical Orientation (Stacked)

Panes are arranged top-to-bottom. Dragging separators changes **heights**. Best for:
- Toolbar + content + footer
- Editor + output/console
- Properties + canvas + preview
- Multi-section forms

```xaml
<gridSplitter:SfGridSplitter Orientation="Vertical" Height="600">
    <gridSplitter:SplitterPane Size="50">
        <!-- Top pane (toolbar) -->
    </gridSplitter:SplitterPane>
    <gridSplitter:SplitterPane Size="1*">
        <!-- Main pane -->
    </gridSplitter:SplitterPane>
    <gridSplitter:SplitterPane Size="150">
        <!-- Bottom pane (output) -->
    </gridSplitter:SplitterPane>
</gridSplitter:SfGridSplitter>
```

### Nested Layouts (Horizontal + Vertical)

Combine orientations for complex layouts:

```xaml
<!-- Outer: Horizontal (sidebar + rest) -->
<gridSplitter:SfGridSplitter Orientation="Horizontal">
    <!-- Sidebar -->
    <gridSplitter:SplitterPane Size="250">
        <Label Text="Navigation"/>
    </gridSplitter:SplitterPane>

    <!-- Inner splitter: Vertical (editor + output) -->
    <gridSplitter:SplitterPane Size="1*">
        <gridSplitter:SfGridSplitter Orientation="Vertical">
            <gridSplitter:SplitterPane Size="1*">
                <Editor Placeholder="Code"/>
            </gridSplitter:SplitterPane>
            <gridSplitter:SplitterPane Size="200">
                <Label Text="Output"/>
            </gridSplitter:SplitterPane>
        </gridSplitter:SfGridSplitter>
    </gridSplitter:SplitterPane>
</gridSplitter:SfGridSplitter>
```

## Responsive Design for Multiple Devices

Design layouts that adapt to different screen sizes, resolutions, and device types.

### Detecting Screen Size

```csharp
public class ScreenSizeHelper
{
    public static double ScreenWidth => DeviceDisplay.MainDisplayInfo.Width / DeviceDisplay.MainDisplayInfo.Density;
    public static double ScreenHeight => DeviceDisplay.MainDisplayInfo.Height / DeviceDisplay.MainDisplayInfo.Density;
    
    public static bool IsMobile => ScreenWidth < 600;
    public static bool IsTablet => ScreenWidth >= 600 && ScreenWidth < 1200;
    public static bool IsDesktop => ScreenWidth >= 1200;
}
```

### Mobile Layout (< 600px)

Vertical stacking with large touch targets:

```xaml
<gridSplitter:SfGridSplitter Orientation="Vertical" x:Name="MobileSplitter">
    <!-- Top navigation -->
    <gridSplitter:SplitterPane Size="60" IsCollapsible="True" MinimumSize="30">
        <HorizontalStackLayout Padding="10" Spacing="5">
            <Button Text="Menu" WidthRequest="60"/>
            <Button Text="Search" WidthRequest="60"/>
        </HorizontalStackLayout>
    </gridSplitter:SplitterPane>

    <!-- Content area (main focus) -->
    <gridSplitter:SplitterPane Size="1*" MinimumSize="300">
        <CollectionView ItemsSource="{Binding Items}">
            <!-- Items -->
        </CollectionView>
    </gridSplitter:SplitterPane>

    <!-- Detail panel (collapsible) -->
    <gridSplitter:SplitterPane Size="200" IsCollapsible="True" MinimumSize="30">
        <VerticalStackLayout Padding="10">
            <Label Text="Details"/>
        </VerticalStackLayout>
    </gridSplitter:SplitterPane>
</gridSplitter:SfGridSplitter>
```

### Tablet Layout (600-1200px)

Two-column layout with balanced panes:

```xaml
<gridSplitter:SfGridSplitter Orientation="Horizontal" x:Name="TabletSplitter">
    <!-- Navigation/Master list -->
    <gridSplitter:SplitterPane Size="300" MinimumSize="250">
        <CollectionView ItemsSource="{Binding Items}"/>
    </gridSplitter:SplitterPane>

    <!-- Content/Detail view -->
    <gridSplitter:SplitterPane Size="1*" MinimumSize="300">
        <VerticalStackLayout Padding="20">
            <Label Text="Content" FontSize="18" FontAttributes="Bold"/>
        </VerticalStackLayout>
    </gridSplitter:SplitterPane>
</gridSplitter:SfGridSplitter>
```

### Desktop Layout (≥ 1200px)

Three-column layout with full feature set:

```xaml
<gridSplitter:SfGridSplitter Orientation="Horizontal" x:Name="DesktopSplitter">
    <!-- Sidebar navigation -->
    <gridSplitter:SplitterPane Size="200" MinimumSize="150" IsCollapsible="True">
        <VerticalStackLayout Padding="10"/>
    </gridSplitter:SplitterPane>

    <!-- Main content -->
    <gridSplitter:SplitterPane Size="2*" MinimumSize="400">
        <VerticalStackLayout Padding="20"/>
    </gridSplitter:SplitterPane>

    <!-- Details panel -->
    <gridSplitter:SplitterPane Size="250" MinimumSize="200" IsCollapsible="True">
        <VerticalStackLayout Padding="10"/>
    </gridSplitter:SplitterPane>
</gridSplitter:SfGridSplitter>
```

### Responsive Helper Methods

```csharp
public partial class MainPage : ContentPage
{
    protected override void OnSizeAllocated(double width, double height)
    {
        base.OnSizeAllocated(width, height);
        
        if (width < 600)
            ApplyMobileLayout();
        else if (width < 1200)
            ApplyTabletLayout();
        else
            ApplyDesktopLayout();
    }
    
    private void ApplyMobileLayout()
    {
        gridSplitter.Orientation = GridSplitterOrientation.Vertical;
        // Adjust pane sizes for mobile
    }
    
    private void ApplyTabletLayout()
    {
        gridSplitter.Orientation = GridSplitterOrientation.Horizontal;
        // Adjust pane sizes for tablet
    }
    
    private void ApplyDesktopLayout()
    {
        gridSplitter.Orientation = GridSplitterOrientation.Horizontal;
        // Adjust pane sizes for desktop
    }
}
```

## Handling Orientation Changes

Respond to device orientation changes (portrait/landscape) and reorganize layout accordingly.

### Detecting Orientation

```csharp
public class OrientationHelper
{
    public static DisplayOrientation CurrentOrientation => 
        DeviceDisplay.Current.MainDisplayInfo.Orientation;
    
    public static bool IsPortrait => 
        CurrentOrientation == DisplayOrientation.Portrait;
    
    public static bool IsLandscape => 
        CurrentOrientation == DisplayOrientation.Landscape;
}
```

### Responding to Orientation Changes

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        
        // Subscribe to orientation changes
        DeviceDisplay.Current.MainDisplayInfoChanged += OnDisplayInfoChanged;
    }
    
    private void OnDisplayInfoChanged(object sender, DisplayInfoChangedEventArgs e)
    {
        bool isPortrait = e.DisplayInfo.Orientation == DisplayOrientation.Portrait;
        
        if (isPortrait)
        {
            ApplyPortraitLayout();
        }
        else
        {
            ApplyLandscapeLayout();
        }
    }
    
    private void ApplyPortraitLayout()
    {
        // Vertical stacking for portrait
        gridSplitter.Orientation = GridSplitterOrientation.Vertical;
    }
    
    private void ApplyLandscapeLayout()
    {
        // Horizontal layout for landscape
        gridSplitter.Orientation = GridSplitterOrientation.Horizontal;
    }
}
```

### Persistent Orientation Preferences

```csharp
public class LayoutPreferences
{
    private const string ORIENTATION_KEY = "preferred_orientation";
    
    public static void SaveOrientationPreference(DisplayOrientation orientation)
    {
        Preferences.Set(ORIENTATION_KEY, orientation.ToString());
    }
    
    public static DisplayOrientation LoadOrientationPreference()
    {
        string stored = Preferences.Get(ORIENTATION_KEY, "Portrait");
        return Enum.Parse<DisplayOrientation>(stored);
    }
}
```

## RTL (Right-to-Left) Support

The Grid Splitter provides full RTL support for right-to-left languages (Arabic, Hebrew, etc.).

### Enable RTL Layout

```xaml
<gridSplitter:SfGridSplitter x:Name="gridSplitter" Orientation="Horizontal" FlowDirection="RightToLeft">
    <!-- RTL-aware panes -->
    <gridSplitter:SplitterPane Size="1*">
        <Label Text="محتوى"/>  <!-- Arabic text -->
    </gridSplitter:SplitterPane>
    <gridSplitter:SplitterPane Size="1*">
        <Label Text="תוכן"/>  <!-- Hebrew text -->
    </gridSplitter:SplitterPane>
</gridSplitter:SfGridSplitter>

```

### Conditional RTL Based on Culture

```csharp
public void ApplyLayoutForCulture(string cultureCode)
{
    bool isRTL = cultureCode switch
    {
        "ar-SA" => true,  // Arabic
        "ar-AE" => true,  // Arabic UAE
        "he-IL" => true,  // Hebrew
        "fa-IR" => true,  // Persian
        "ur-PK" => true,  // Urdu
        _ => false
    };
    
    gridSplitter.FlowDirection = isRTL 
        ? FlowDirection.RightToLeft 
        : FlowDirection.LeftToRight;
}
```

### RTL with Navigation

```xaml
<!-- RTL enabled: Items appear right-aligned -->
<gridSplitter:SfGridSplitter Orientation="Horizontal" FlowDirection="RightToLeft">
    <!-- Navigation panel (appears on right in RTL) -->
    <gridSplitter:SplitterPane Size="250">
        <VerticalStackLayout Padding="10">
            <Label Text="القائمة" FlowDirection="RightToLeft"/>  <!-- Menu in Arabic -->
            <Button Text="الرئيسية"/>  <!-- Home -->
            <Button Text="الإعدادات"/>  <!-- Settings -->
        </VerticalStackLayout>
    </gridSplitter:SplitterPane>

    <!-- Content panel (appears on left in RTL) -->
    <gridSplitter:SplitterPane Size="1*">
        <Label Text="المحتوى الرئيسي" FlowDirection="RightToLeft"/>  <!-- Main Content -->
    </gridSplitter:SplitterPane>
</gridSplitter:SfGridSplitter>
```

## Accessibility Features

Ensure your Grid Splitter layout is accessible to all users, including those using assistive technologies.

### Keyboard Navigation

Users can navigate between panes using keyboard:
- **Tab**: Navigate between panes
- **Arrow Keys**: Adjust pane sizes
- **Enter/Space**: Toggle collapse/expand on collapsible panes
- **Shift+Tab**: Navigate in reverse

```xaml
<gridSplitter:SfGridSplitter>
    <!-- Panes are automatically keyboard navigable -->
    <gridSplitter:SplitterPane IsCollapsible="True">
        <Label Text="Use Tab to navigate, arrows to resize"/>
    </gridSplitter:SplitterPane>
</gridSplitter:SfGridSplitter>
```

### Screen Reader Support

Enable accessibility labels for screen readers:

```xaml
<gridSplitter:SfGridSplitter SemanticProperties.Description="Layout with resizable content panes">
    <gridSplitter:SplitterPane SemanticProperties.Hint="Navigation pane - use arrow keys to resize">
        <Label Text="Navigation" SemanticProperties.Description="Navigation menu"/>
    </gridSplitter:SplitterPane>
    
    <gridSplitter:SplitterPane SemanticProperties.Hint="Main content pane">
        <Label Text="Content" SemanticProperties.Description="Main content area"/>
    </gridSplitter:SplitterPane>
</gridSplitter:SfGridSplitter>
```

### Semantic Properties in C#

```csharp
SplitterPane navigationPane = new SplitterPane
{
    Content = new Label { Text = "Navigation" }
};

// Set accessibility labels
SemanticProperties.SetDescription(navigationPane, "Navigation pane containing main menu items");
SemanticProperties.SetHint(navigationPane, "Contains Dashboard, Reports, Settings options");
```

### High Contrast Support

Ensure sufficient color contrast for accessibility:

```xaml
<!-- ✓ Good: High contrast between pane and separator -->
<gridSplitter:SfGridSplitter BackgroundColor="Black" ResizeIconColor="White">
    <gridSplitter:SplitterPane BackgroundColor="White">
        <Label Text="Content" TextColor="Black"/>
    </gridSplitter:SplitterPane>
</gridSplitter:SfGridSplitter>

<!-- ✗ Poor: Low contrast - hard to read -->
<gridSplitter:SfGridSplitter BackgroundColor="LightGray" ResizeIconColor="White">
    <gridSplitter:SplitterPane BackgroundColor="White">
        <Label Text="Content" TextColor="LightGray"/>
    </gridSplitter:SplitterPane>
</gridSplitter:SfGridSplitter>
```

## Cross-Platform Consistency

Grid Splitter provides uniform behavior across Android, iOS, Windows, and macOS.

### Platform-Specific Adjustments

```csharp
public void ConfigureForPlatform()
{
    if (DeviceInfo.Platform == DevicePlatform.Android)
    {
        // Android-specific configuration
        gridSplitter.SeparatorSize = 3;
        gridSplitter.ResizeIconColor = Colors.Blue;
    }
    else if (DeviceInfo.Platform == DevicePlatform.iOS)
    {
        // iOS-specific configuration
        gridSplitter.SeparatorSize = 2;
        gridSplitter.ResizeIconColor = Colors.Gray;
    }
    else if (DeviceInfo.Platform == DevicePlatform.WinUI)
    {
        // Windows-specific configuration
        gridSplitter.SeparatorSize = 4;
    }
    else if (DeviceInfo.Platform == DevicePlatform.macOS)
    {
        // macOS-specific configuration
        gridSplitter.SeparatorSize = 2;
    }
}
```

### Device-Specific Touch Targets

```csharp
public double GetTouchTargetSize()
{
    if (DeviceInfo.Platform == DevicePlatform.Android)
    {
        return 48; // Android: 48dp minimum
    }
    else if (DeviceInfo.Platform == DevicePlatform.iOS)
    {
        return 44; // iOS: 44pt minimum
    }
    else if (DeviceInfo.Platform == DevicePlatform.WinUI)
    {
        return 40; // Windows
    }
    else if (DeviceInfo.Platform == DevicePlatform.MacCatalyst)
    {
        return 20; // Mac Catalyst
    }

    return 44;
}
```

## Dashboard and Workspace Patterns

Implement common layout patterns for dashboards and workspace applications.

### Analytics Dashboard

Multi-pane dashboard with filters, charts, and details:

```xaml
<gridSplitter:SfGridSplitter Orientation="Horizontal">
    <!-- Filters sidebar -->
    <gridSplitter:SplitterPane Size="200" MinimumSize="150" IsCollapsible="True">
        <VerticalStackLayout Padding="10" Spacing="10">
            <Label Text="Filters" FontAttributes="Bold"/>
            <DatePicker />
            <DatePicker />
            <Picker Title="Category"/>
            <Button Text="Apply"/>
        </VerticalStackLayout>
    </gridSplitter:SplitterPane>

    <!-- Charts area -->
    <gridSplitter:SplitterPane Size="2*">
        <VerticalStackLayout Padding="20">
            <Label Text="Analytics Charts" FontSize="18" FontAttributes="Bold"/>
            <!-- Chart controls -->
        </VerticalStackLayout>
    </gridSplitter:SplitterPane>

    <!-- Details panel -->
    <gridSplitter:SplitterPane Size="250" MinimumSize="200" IsCollapsible="True">
        <ScrollView>
            <VerticalStackLayout Padding="10" Spacing="5">
                <Label Text="Details" FontAttributes="Bold"/>
                <Label Text="Metric 1: 1,234"/>
                <Label Text="Metric 2: 567"/>
            </VerticalStackLayout>
        </ScrollView>
    </gridSplitter:SplitterPane>
</gridSplitter:SfGridSplitter>
```

### Code Editor Workspace

IDE-style layout with project tree, editor, and output:

```xaml
<gridSplitter:SfGridSplitter Orientation="Horizontal">
    <!-- Project explorer -->
    <gridSplitter:SplitterPane Size="250" MinimumSize="150" IsCollapsible="True">
        <VerticalStackLayout Padding="10">
            <Label Text="Project" FontAttributes="Bold"/>
            <!-- File tree -->
        </VerticalStackLayout>
    </gridSplitter:SplitterPane>

    <!-- Main editor -->
    <gridSplitter:SplitterPane Size="1*" MinimumSize="300">
        <Editor Placeholder="Write code here..." FontSize="12"/>
    </gridSplitter:SplitterPane>

    <!-- Properties/Inspector -->
    <gridSplitter:SplitterPane Size="200" MinimumSize="150" IsCollapsible="True">
        <VerticalStackLayout Padding="10">
            <Label Text="Properties" FontAttributes="Bold"/>
        </VerticalStackLayout>
    </gridSplitter:SplitterPane>
</gridSplitter:SfGridSplitter>
```

### Data Management Workspace

Master-detail view with CRUD operations:

```xaml
<gridSplitter:SfGridSplitter Orientation="Horizontal">
    <!-- Master list -->
    <gridSplitter:SplitterPane Size="1*" MinimumSize="250">
        <DataGrid x:Name="DataGrid" ItemsSource="{Binding Items}"/>
    </gridSplitter:SplitterPane>

    <!-- Detail form -->
    <gridSplitter:SplitterPane Size="350" MinimumSize="300">
        <ScrollView>
            <VerticalStackLayout Padding="20" Spacing="10">
                <Entry Placeholder="Name" Text="{Binding SelectedItem.Name}"/>
                <Entry Placeholder="Email" Text="{Binding SelectedItem.Email}"/>
                <Button Text="Save" Command="{Binding SaveCommand}"/>
                <Button Text="Delete" Command="{Binding DeleteCommand}"/>
            </VerticalStackLayout>
        </ScrollView>
    </gridSplitter:SplitterPane>
</gridSplitter:SfGridSplitter>
```

---

**Congratulations!** You now have comprehensive knowledge of the Grid Splitter control. Refer back to other references as needed, or explore the official Syncfusion documentation for advanced scenarios.
