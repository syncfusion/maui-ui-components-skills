# Advanced Features

## Table of Contents
- [Right-to-Left (RTL) Support](#right-to-left-rtl-support)
- [Adaptive Layouts for Different Screen Sizes](#adaptive-layouts-for-different-screen-sizes)
- [Adaptive Layout Patterns](#adaptive-layout-patterns)
- [Sample Projects and Resources](#sample-projects-and-resources)
- [Performance Tips](#performance-tips)
- [Troubleshooting](#troubleshooting)

This guide covers advanced DockLayout features including Right-to-Left (RTL) support, adaptive layouts, and best practices for building responsive multi-platform applications.

## Right-to-Left (RTL) Support

The .NET MAUI DockLayout control fully supports Right-to-Left (RTL) layout direction for languages like Arabic, Hebrew, Persian, and Urdu. When enabled, docking positions are mirrored to align with RTL language standards.

### FlowDirection Property

Set the `FlowDirection` property to control layout direction.

#### Property Details
- **Type**: `FlowDirection` enum
- **Values**: 
  - `LeftToRight` (default)
  - `RightToLeft`
- **Behavior**: Mirrors left/right dock positions when set to RightToLeft

### Enabling RTL in XAML

```xaml
<sf:SfDockLayout FlowDirection="RightToLeft">
    <!-- Left becomes right, right becomes left in RTL -->
    <Label Text="يسار" 
           WidthRequest="80" 
           sf:SfDockLayout.Dock="Left" 
           Background="#CA7842" />
    
    <Label Text="يمين" 
           WidthRequest="80" 
           sf:SfDockLayout.Dock="Right" 
           Background="#71C0BB" />
    
    <Label Text="محتوى" 
           Background="#64B5F6" />
</sf:SfDockLayout>
```

**Result**: 
- Element docked to "Left" appears on the **right side** of the screen
- Element docked to "Right" appears on the **left side** of the screen
- Top and Bottom positions remain unchanged

### Enabling RTL in C#

```csharp
using Syncfusion.Maui.Core;

SfDockLayout dockLayout = new SfDockLayout
{
    FlowDirection = FlowDirection.RightToLeft
};

// Dock to "Left" - will appear on right side in RTL
dockLayout.Children.Add(
    new Label { Text = "يسار", WidthRequest = 80, Background = Color.FromArgb("#CA7842") }, 
    Dock.Left
);

// Dock to "Right" - will appear on left side in RTL
dockLayout.Children.Add(
    new Label { Text = "يمين", WidthRequest = 80, Background = Color.FromArgb("#71C0BB") }, 
    Dock.Right
);

dockLayout.Children.Add(
    new Label { Text = "محتوى", Background = Color.FromArgb("#64B5F6") }
);

Content = dockLayout;
```

### Device-Based RTL

The layout direction can also be determined automatically based on the device's language settings. When the device is set to an RTL language, .NET MAUI automatically sets `FlowDirection` to `RightToLeft`.

```csharp
public MainPage()
{
    var dockLayout = new SfDockLayout();
    
    // FlowDirection will be inherited from device settings
    // Or explicitly check:
    if (CultureInfo.CurrentUICulture.TextInfo.IsRightToLeft)
    {
        dockLayout.FlowDirection = FlowDirection.RightToLeft;
    }
    
    // Add children...
    Content = dockLayout;
}
```

### RTL Best Practices

1. **Test with RTL Languages**: Always test your layout with actual RTL content
2. **Use Logical Positioning**: Think in terms of "Start" and "End" rather than "Left" and "Right"
3. **Consider Icons**: Mirror directional icons (arrows, back buttons) for RTL
4. **Text Alignment**: Ensure text alignment matches the flow direction

### Complete RTL Example

```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sf="clr-namespace:Syncfusion.Maui.Core;assembly=Syncfusion.Maui.Core"
             x:Class="MyApp.RTLPage"
             FlowDirection="RightToLeft">

    <sf:SfDockLayout HorizontalSpacing="10" VerticalSpacing="10">
        <!-- Header (unchanged by RTL) -->
        <Grid HeightRequest="60" 
              BackgroundColor="#6200EE"
              sf:SfDockLayout.Dock="Top">
            <Label Text="تطبيقي" 
                   TextColor="White" 
                   FontSize="20"
                   VerticalOptions="Center" 
                   Margin="16,0" />
        </Grid>
        
        <!-- Navigation menu (docked "Left", appears on right) -->
        <VerticalStackLayout WidthRequest="200" 
                             BackgroundColor="#F5F5F5"
                             sf:SfDockLayout.Dock="Left">
            <Label Text="القائمة" Margin="16" FontSize="18" />
            <Button Text="الرئيسية" />
            <Button Text="الإعدادات" />
        </VerticalStackLayout>
        
        <!-- Main content -->
        <Label Text="محتوى رئيسي" 
               Margin="20"
               FontSize="16" />
    </sf:SfDockLayout>
    
</ContentPage>
```

## Adaptive Layouts for Different Screen Sizes

DockLayout automatically adapts to various screen sizes and orientations, but you can enhance responsiveness with code-based adjustments.

### Responsive Width Breakpoints

```csharp
public class ResponsiveDockPage : ContentPage
{
    private SfDockLayout dockLayout;
    private VerticalStackLayout sidebar;
    
    public ResponsiveDockPage()
    {
        dockLayout = new SfDockLayout { HorizontalSpacing = 8, VerticalSpacing = 8 };
        
        // Header
        dockLayout.Children.Add(
            new Label 
            { 
                Text = "Responsive App", 
                HeightRequest = 60, 
                BackgroundColor = Colors.DarkBlue,
                TextColor = Colors.White,
                VerticalTextAlignment = TextAlignment.Center,
                Padding = new Thickness(16, 0)
            }, 
            Dock.Top
        );
        
        // Sidebar
        sidebar = new VerticalStackLayout 
        { 
            WidthRequest = 250, 
            BackgroundColor = Colors.LightGray 
        };
        sidebar.Children.Add(new Label { Text = "Menu", Margin = 16 });
        sidebar.Children.Add(new Button { Text = "Home" });
        sidebar.Children.Add(new Button { Text = "Settings" });
        
        dockLayout.Children.Add(sidebar, Dock.Left);
        
        // Main content
        dockLayout.Children.Add(
            new ScrollView 
            { 
                Content = new Label 
                { 
                    Text = "Main Content", 
                    Margin = 20 
                } 
            }
        );
        
        Content = dockLayout;
    }
    
    protected override void OnSizeAllocated(double width, double height)
    {
        base.OnSizeAllocated(width, height);
        
        // Mobile portrait: Move sidebar to top
        if (width < 600)
        {
            SfDockLayout.SetDock(sidebar, Dock.Top);
            sidebar.WidthRequest = -1;  // Full width
            sidebar.HeightRequest = 200;
            dockLayout.HorizontalSpacing = 0;
            dockLayout.VerticalSpacing = 8;
        }
        // Tablet/Desktop: Sidebar on left
        else
        {
            SfDockLayout.SetDock(sidebar, Dock.Left);
            sidebar.WidthRequest = 250;
            sidebar.HeightRequest = -1;
            dockLayout.HorizontalSpacing = 8;
            dockLayout.VerticalSpacing = 8;
        }
    }
}
```

### Handling Orientation Changes

```csharp
public class OrientationAwarePage : ContentPage
{
    private SfDockLayout dockLayout;
    private BoxView infoPanel;
    
    public OrientationAwarePage()
    {
        dockLayout = new SfDockLayout();
        
        // Top header
        dockLayout.Children.Add(
            new Label { Text = "Header", HeightRequest = 60, Background = Colors.Blue }, 
            Dock.Top
        );
        
        // Info panel
        infoPanel = new BoxView 
        { 
            Color = Colors.Orange, 
            WidthRequest = 200 
        };
        dockLayout.Children.Add(infoPanel, Dock.Right);
        
        // Main content
        dockLayout.Children.Add(
            new Label { Text = "Content", Background = Colors.White }
        );
        
        Content = dockLayout;
    }
    
    protected override void OnSizeAllocated(double width, double height)
    {
        base.OnSizeAllocated(width, height);
        
        bool isLandscape = width > height;
        
        if (isLandscape)
        {
            // Landscape: Info panel on right
            SfDockLayout.SetDock(infoPanel, Dock.Right);
            infoPanel.WidthRequest = 250;
            infoPanel.HeightRequest = -1;
        }
        else
        {
            // Portrait: Info panel at bottom
            SfDockLayout.SetDock(infoPanel, Dock.Bottom);
            infoPanel.WidthRequest = -1;
            infoPanel.HeightRequest = 150;
        }
    }
}
```

### Platform-Specific Layouts

```csharp
public class PlatformAwarePage : ContentPage
{
    public PlatformAwarePage()
    {
        var dockLayout = new SfDockLayout();
        
        // Header
        dockLayout.Children.Add(
            new Label { Text = "Cross-Platform App", HeightRequest = 60, Background = Colors.Purple }, 
            Dock.Top
        );
        
        // Sidebar (different sizes per platform)
        var sidebar = new BoxView { Color = Colors.Gray };
        
        if (DeviceInfo.Platform == DevicePlatform.Android || 
            DeviceInfo.Platform == DevicePlatform.iOS)
        {
            // Mobile: Narrower sidebar
            sidebar.WidthRequest = 60;
        }
        else
        {
            // Desktop: Wider sidebar
            sidebar.WidthRequest = 250;
        }
        
        dockLayout.Children.Add(sidebar, Dock.Left);
        
        // Main content
        dockLayout.Children.Add(
            new Label { Text = "Content Area", Background = Colors.White }
        );
        
        Content = dockLayout;
    }
}
```

## Adaptive Layout Patterns

### Pattern: Collapsible Sidebar

```csharp
public class CollapsibleSidebarPage : ContentPage
{
    private SfDockLayout dockLayout;
    private VerticalStackLayout sidebar;
    private Button toggleButton;
    private bool isSidebarVisible = true;
    
    public CollapsibleSidebarPage()
    {
        dockLayout = new SfDockLayout { HorizontalSpacing = 8 };
        
        // Toggle button in header
        toggleButton = new Button 
        { 
            Text = "☰", 
            WidthRequest = 50,
            FontSize = 20
        };
        toggleButton.Clicked += ToggleSidebar;
        
        var header = new Grid 
        { 
            HeightRequest = 60, 
            BackgroundColor = Colors.DarkSlateGray,
            ColumnDefinitions = new ColumnDefinitionCollection 
            { 
                new ColumnDefinition { Width = 60 },
                new ColumnDefinition { Width = GridLength.Star }
            }
        };
        header.Children.Add(toggleButton);
        header.Children.Add(
            new Label 
            { 
                Text = "App Title", 
                TextColor = Colors.White, 
                VerticalOptions = LayoutOptions.Center,
                Margin = new Thickness(16, 0)
            }
        );
        Grid.SetColumn(header.Children[1], 1);
        
        dockLayout.Children.Add(header, Dock.Top);
        
        // Collapsible sidebar
        sidebar = new VerticalStackLayout 
        { 
            WidthRequest = 250, 
            BackgroundColor = Colors.LightGray 
        };
        sidebar.Children.Add(new Label { Text = "Navigation", Margin = 16 });
        sidebar.Children.Add(new Button { Text = "Dashboard" });
        sidebar.Children.Add(new Button { Text = "Reports" });
        sidebar.Children.Add(new Button { Text = "Settings" });
        
        dockLayout.Children.Add(sidebar, Dock.Left);
        
        // Main content
        dockLayout.Children.Add(
            new Label { Text = "Main Content Area", Margin = 20, Background = Colors.White }
        );
        
        Content = dockLayout;
    }
    
    private async void ToggleSidebar(object sender, EventArgs e)
    {
        if (isSidebarVisible)
        {
            // Hide sidebar
            await sidebar.FadeTo(0, 200);
            sidebar.WidthRequest = 0;
            dockLayout.HorizontalSpacing = 0;
            isSidebarVisible = false;
        }
        else
        {
            // Show sidebar
            sidebar.WidthRequest = 250;
            dockLayout.HorizontalSpacing = 8;
            await sidebar.FadeTo(1, 200);
            isSidebarVisible = true;
        }
    }
}
```

### Pattern: Responsive Dashboard

```csharp
public class ResponsiveDashboard : ContentPage
{
    private SfDockLayout mainLayout;
    private Grid statsPanel;
    
    public ResponsiveDashboard()
    {
        mainLayout = new SfDockLayout { VerticalSpacing = 12, HorizontalSpacing = 12, Padding = 12 };
        
        // Title
        mainLayout.Children.Add(
            new Label 
            { 
                Text = "Dashboard", 
                FontSize = 28, 
                HeightRequest = 60 
            }, 
            Dock.Top
        );
        
        // Stats panel (responsive grid)
        statsPanel = new Grid 
        { 
            RowSpacing = 12, 
            ColumnSpacing = 12 
        };
        
        // Will be configured in OnSizeAllocated
        Content = mainLayout;
    }
    
    protected override void OnSizeAllocated(double width, double height)
    {
        base.OnSizeAllocated(width, height);
        
        ConfigureStatsPanel(width);
    }
    
    private void ConfigureStatsPanel(double width)
    {
        statsPanel.Children.Clear();
        statsPanel.ColumnDefinitions.Clear();
        statsPanel.RowDefinitions.Clear();
        
        if (width < 600) // Mobile: 1 column
        {
            statsPanel.ColumnDefinitions.Add(new ColumnDefinition { Width = GridLength.Star });
            AddStatsCards(1);
            mainLayout.Children.Remove(statsPanel);
            mainLayout.Children.Add(statsPanel, Dock.Top);
        }
        else if (width < 1200) // Tablet: 2 columns
        {
            statsPanel.ColumnDefinitions.Add(new ColumnDefinition { Width = GridLength.Star });
            statsPanel.ColumnDefinitions.Add(new ColumnDefinition { Width = GridLength.Star });
            AddStatsCards(2);
            mainLayout.Children.Remove(statsPanel);
            mainLayout.Children.Add(statsPanel, Dock.Top);
        }
        else // Desktop: 4 columns
        {
            for (int i = 0; i < 4; i++)
            {
                statsPanel.ColumnDefinitions.Add(new ColumnDefinition { Width = GridLength.Star });
            }
            AddStatsCards(4);
            mainLayout.Children.Remove(statsPanel);
            mainLayout.Children.Add(statsPanel, Dock.Top);
        }
    }
    
    private void AddStatsCards(int columns)
    {
        var stats = new[] 
        { 
            ("Sales", "$45,231"), 
            ("Users", "1,234"), 
            ("Orders", "567"), 
            ("Revenue", "$89,012") 
        };
        
        for (int i = 0; i < stats.Length; i++)
        {
            var card = new Frame 
            { 
                Padding = 16, 
                CornerRadius = 8,
                Content = new VerticalStackLayout 
                {
                    Children = 
                    {
                        new Label { Text = stats[i].Item1, FontSize = 14 },
                        new Label { Text = stats[i].Item2, FontSize = 24, FontAttributes = FontAttributes.Bold }
                    }
                }
            };
            
            int row = i / columns;
            int col = i % columns;
            
            if (row >= statsPanel.RowDefinitions.Count)
            {
                statsPanel.RowDefinitions.Add(new RowDefinition { Height = GridLength.Auto });
            }
            
            Grid.SetRow(card, row);
            Grid.SetColumn(card, col);
            statsPanel.Children.Add(card);
        }
    }
}
```

## Sample Projects and Resources

### Official Sample Repository

Access complete working examples from the [Syncfusion GitHub repository](https://github.com/SyncfusionExamples/GettingStarted_DockLayout_MAUI).

### Video Tutorial

Watch the [official DockLayout video tutorial](https://www.youtube.com/watch?v=g2NU8b_9aAg) for visual walkthrough.

### Additional Resources

- **API Reference**: [SfDockLayout Documentation](https://help.syncfusion.com/cr/maui/Syncfusion.Maui.Core.SfDockLayout.html)
- **Community Forums**: [Syncfusion MAUI Forums](https://www.syncfusion.com/forums/maui)
- **Knowledge Base**: [DockLayout Articles](https://www.syncfusion.com/kb/maui)

## Performance Tips

1. **Minimize Layout Changes**: Avoid frequent dock position changes; batch updates when possible
2. **Use Fixed Sizes**: Provide explicit sizes for docked elements to reduce layout calculations
3. **Limit Nesting**: Deep nesting of DockLayouts can impact performance; consider alternative layouts
4. **Cache References**: Store references to frequently accessed child elements
5. **Optimize Spacing**: Use spacing properties instead of margins for better performance

## Troubleshooting

### RTL Not Working
- **Cause**: FlowDirection not set or parent overrides it
- **Solution**: Set FlowDirection on both DockLayout and parent ContentPage

### Layout Doesn't Adapt to Screen Size
- **Cause**: Missing OnSizeAllocated override or layout logic
- **Solution**: Implement OnSizeAllocated and adjust dock positions/sizes based on width/height

### Sidebar Flickers During Orientation Change
- **Cause**: Layout recalculation during transition
- **Solution**: Use animations (FadeTo, TranslateTo) to smooth transitions

### Content Overflows on Small Screens
- **Cause**: Fixed sizes don't account for small screens
- **Solution**: Use relative sizing or implement breakpoints with minimum sizes
