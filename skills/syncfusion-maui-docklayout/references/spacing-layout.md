# Spacing and Layout Configuration

## Table of Contents
- [HorizontalSpacing Property](#horizontalspacing-property)
- [VerticalSpacing Property](#verticalspacing-property)
- [Combining Horizontal and Vertical Spacing](#combining-horizontal-and-vertical-spacing)
- [ShouldExpandLastChild Property](#shouldexpandlastchild-property)
- [Layout Scenarios and Best Practices](#layout-scenarios-and-best-practices)
- [Common Patterns](#common-patterns)
- [Troubleshooting](#troubleshooting)

This guide covers spacing properties and last child expansion behavior in SfDockLayout, helping you control gaps between elements and optimize space utilization.

## HorizontalSpacing Property

The `HorizontalSpacing` property specifies the horizontal gap (in pixels) between docked child elements.

### Property Details
- **Type**: `double`
- **Default**: `0`
- **Applies to**: Gaps between left/right docked elements and center content

### Basic Usage

#### XAML
```xaml
<sf:SfDockLayout HorizontalSpacing="15">
    <Label Text="Left" 
           WidthRequest="80" 
           sf:SfDockLayout.Dock="Left" 
           Background="Blue" />
    
    <Label Text="Center" 
           Background="Gray" />
</sf:SfDockLayout>
```

#### C#
```csharp
SfDockLayout dockLayout = new SfDockLayout 
{ 
    HorizontalSpacing = 15 
};

dockLayout.Children.Add(
    new Label { Text = "Left", WidthRequest = 80, Background = Colors.Blue }, 
    Dock.Left
);

dockLayout.Children.Add(
    new Label { Text = "Center", Background = Colors.Gray }
);
```

**Result**: 15-pixel gap between the left element and center content.

### Example: Sidebar with Spacing

```xaml
<sf:SfDockLayout HorizontalSpacing="20">
    <!-- Left navigation panel -->
    <VerticalStackLayout WidthRequest="200" 
                         BackgroundColor="LightBlue"
                         sf:SfDockLayout.Dock="Left">
        <Label Text="Navigation" Margin="10" FontSize="16" />
        <Button Text="Dashboard" />
        <Button Text="Reports" />
    </VerticalStackLayout>
    
    <!-- Right info panel -->
    <VerticalStackLayout WidthRequest="200" 
                         BackgroundColor="LightGreen"
                         sf:SfDockLayout.Dock="Right">
        <Label Text="Info" Margin="10" FontSize="16" />
        <Label Text="User: John" />
    </VerticalStackLayout>
    
    <!-- Center content with 20px gaps on both sides -->
    <Label Text="Main Content Area" 
           BackgroundColor="White" 
           VerticalOptions="Center" 
           HorizontalOptions="Center" />
</sf:SfDockLayout>
```

## VerticalSpacing Property

The `VerticalSpacing` property specifies the vertical gap (in pixels) between docked child elements.

### Property Details
- **Type**: `double`
- **Default**: `0`
- **Applies to**: Gaps between top/bottom docked elements and center content

### Basic Usage

#### XAML
```xaml
<sf:SfDockLayout VerticalSpacing="10">
    <Label Text="Header" 
           HeightRequest="60" 
           sf:SfDockLayout.Dock="Top" 
           Background="Red" />
    
    <Label Text="Center" 
           Background="Gray" />
</sf:SfDockLayout>
```

#### C#
```csharp
SfDockLayout dockLayout = new SfDockLayout 
{ 
    VerticalSpacing = 10 
};

dockLayout.Children.Add(
    new Label { Text = "Header", HeightRequest = 60, Background = Colors.Red }, 
    Dock.Top
);

dockLayout.Children.Add(
    new Label { Text = "Center", Background = Colors.Gray }
);
```

**Result**: 10-pixel gap between the header and center content.

### Example: Header and Footer with Spacing

```xaml
<sf:SfDockLayout VerticalSpacing="15">
    <!-- Top app bar -->
    <Grid HeightRequest="56" 
          BackgroundColor="#6200EE"
          sf:SfDockLayout.Dock="Top">
        <Label Text="My App" 
               TextColor="White" 
               VerticalOptions="Center" 
               Margin="16,0" />
    </Grid>
    
    <!-- Bottom status bar -->
    <Grid HeightRequest="30" 
          BackgroundColor="#E0E0E0"
          sf:SfDockLayout.Dock="Bottom">
        <Label Text="Ready | Connected" 
               VerticalOptions="Center" 
               Margin="8,0" />
    </Grid>
    
    <!-- Main content with 15px gaps top and bottom -->
    <ScrollView>
        <Label Text="Content Area" Margin="20" />
    </ScrollView>
</sf:SfDockLayout>
```

## Combining Horizontal and Vertical Spacing

Use both properties together for uniform spacing around the center content.

### Example: Complete Layout with Spacing

```xaml
<sf:SfDockLayout HorizontalSpacing="10" VerticalSpacing="10">
    <!-- Header -->
    <BoxView Color="#F06292" 
             HeightRequest="80" 
             sf:SfDockLayout.Dock="Top" />
    
    <!-- Footer -->
    <BoxView Color="#9575CD" 
             HeightRequest="80" 
             sf:SfDockLayout.Dock="Bottom" />
    
    <!-- Left sidebar -->
    <BoxView Color="#E57373" 
             WidthRequest="80" 
             sf:SfDockLayout.Dock="Left" />
    
    <!-- Right sidebar -->
    <BoxView Color="#BA68C8" 
             WidthRequest="80" 
             sf:SfDockLayout.Dock="Right" />
    
    <!-- Center content with 10px gaps on all sides -->
    <BoxView Color="#64B5F6" />
</sf:SfDockLayout>
```

### C# Implementation

```csharp
SfDockLayout dockLayout = new SfDockLayout 
{ 
    HorizontalSpacing = 10, 
    VerticalSpacing = 10 
};

dockLayout.Children.Add(
    new BoxView { Color = Color.FromArgb("#F06292"), HeightRequest = 80 }, 
    Dock.Top
);

dockLayout.Children.Add(
    new BoxView { Color = Color.FromArgb("#9575CD"), HeightRequest = 80 }, 
    Dock.Bottom
);

dockLayout.Children.Add(
    new BoxView { Color = Color.FromArgb("#E57373"), WidthRequest = 80 }, 
    Dock.Left
);

dockLayout.Children.Add(
    new BoxView { Color = Color.FromArgb("#BA68C8"), WidthRequest = 80 }, 
    Dock.Right
);

dockLayout.Children.Add(
    new BoxView { Color = Color.FromArgb("#64B5F6") }
);

Content = dockLayout;
```

## ShouldExpandLastChild Property

The `ShouldExpandLastChild` property determines whether the last docked child automatically occupies all remaining space after other docked children have been positioned.

### Property Details
- **Type**: `bool`
- **Default**: `true`
- **When true**: Last child expands to fill available space
- **When false**: Last child uses its own size constraints

### Default Behavior (true)

When `ShouldExpandLastChild` is `true` (default), the last child automatically fills remaining space:

```xaml
<sf:SfDockLayout>
    <Label Text="Header" 
           HeightRequest="60" 
           sf:SfDockLayout.Dock="Top" 
           Background="Blue" />
    
    <!-- Last child expands to fill remaining space -->
    <Label Text="Content fills all space" 
           Background="LightGray" />
</sf:SfDockLayout>
```

**Result**: Content label occupies all space below the header.

### Disabled Expansion (false)

When set to `false`, the last child does NOT automatically expand. You must specify its size explicitly.

#### XAML
```xaml
<sf:SfDockLayout ShouldExpandLastChild="False">
    <Label Text="Top" 
           HeightRequest="80" 
           sf:SfDockLayout.Dock="Top" 
           Background="Red" />
    
    <!-- Last child needs explicit size when expansion is disabled -->
    <Label Text="Center (fixed size)" 
           HeightRequest="100"
           WidthRequest="100"
           Background="Gray" />
</sf:SfDockLayout>
```

#### C#
```csharp
SfDockLayout dockLayout = new SfDockLayout 
{ 
    ShouldExpandLastChild = false 
};

dockLayout.Children.Add(
    new Label { Text = "Top", HeightRequest = 80, Background = Colors.Red }, 
    Dock.Top
);

dockLayout.Children.Add(
    new Label 
    { 
        Text = "Center (fixed size)", 
        HeightRequest = 100, 
        WidthRequest = 100,
        Background = Colors.Gray 
    }
);

Content = dockLayout;
```

**Result**: Center label has a fixed size of 100x100, leaving empty space around it.

### When to Disable Expansion

Disable expansion when you want:
1. **Fixed-size center elements**: Precise control over the center element's dimensions
2. **Multiple non-docked elements**: Several elements without dock positions that need explicit sizing
3. **Centered small content**: A small element centered in the remaining space without filling it

### Example: Centered Logo

```xaml
<sf:SfDockLayout ShouldExpandLastChild="False">
    <!-- Header -->
    <Label Text="App Header" 
           HeightRequest="60" 
           BackgroundColor="#6200EE"
           TextColor="White"
           VerticalTextAlignment="Center"
           HorizontalTextAlignment="Center"
           sf:SfDockLayout.Dock="Top" />
    
    <!-- Centered logo (not expanded) -->
    <Image Source="logo.png" 
           WidthRequest="200" 
           HeightRequest="200"
           HorizontalOptions="Center"
           VerticalOptions="Center" />
</sf:SfDockLayout>
```

## Layout Scenarios and Best Practices

### Scenario 1: Application Shell with Uniform Spacing

```xaml
<sf:SfDockLayout HorizontalSpacing="8" VerticalSpacing="8" Margin="8">
    <!-- Title bar -->
    <Grid HeightRequest="56" 
          BackgroundColor="#6200EE"
          sf:SfDockLayout.Dock="Top">
        <Label Text="Dashboard" 
               TextColor="White" 
               FontSize="20"
               VerticalOptions="Center" 
               Margin="16,0" />
    </Grid>
    
    <!-- Navigation drawer -->
    <VerticalStackLayout WidthRequest="250" 
                         BackgroundColor="#F5F5F5"
                         sf:SfDockLayout.Dock="Left">
        <Label Text="Menu" Margin="16" FontSize="18" FontAttributes="Bold" />
        <Button Text="Home" Margin="8" />
        <Button Text="Analytics" Margin="8" />
        <Button Text="Settings" Margin="8" />
    </VerticalStackLayout>
    
    <!-- Status bar -->
    <Label Text="Ready • Online • 5 notifications" 
           HeightRequest="30" 
           BackgroundColor="#E0E0E0"
           VerticalTextAlignment="Center"
           Margin="12,0"
           sf:SfDockLayout.Dock="Bottom" />
    
    <!-- Main content area -->
    <ScrollView BackgroundColor="White">
        <VerticalStackLayout Padding="20">
            <Label Text="Dashboard Content" FontSize="24" />
            <Label Text="Your data appears here..." />
        </VerticalStackLayout>
    </ScrollView>
</sf:SfDockLayout>
```

**Key points:**
- 8-pixel spacing creates visual separation between regions
- Content area fills remaining space (default behavior)
- Outer margin adds padding from screen edges

### Scenario 2: No Expansion for Precise Layout

```xaml
<sf:SfDockLayout ShouldExpandLastChild="False">
    <Label Text="Toolbar" 
           HeightRequest="50" 
           BackgroundColor="DarkSlateGray"
           sf:SfDockLayout.Dock="Top" />
    
    <!-- Centered card with fixed dimensions -->
    <Frame WidthRequest="300" 
           HeightRequest="400"
           HorizontalOptions="Center"
           VerticalOptions="Center"
           CornerRadius="12"
           HasShadow="True"
           Padding="20">
        <VerticalStackLayout>
            <Label Text="Login" FontSize="24" />
            <Entry Placeholder="Username" Margin="0,20,0,10" />
            <Entry Placeholder="Password" IsPassword="True" Margin="0,0,0,20" />
            <Button Text="Sign In" />
        </VerticalStackLayout>
    </Frame>
</sf:SfDockLayout>
```

**Key points:**
- Expansion disabled for precise card placement
- Card centered in remaining space
- Background visible around card

### Scenario 3: Dynamic Spacing Based on Screen Size

```csharp
public class ResponsivePage : ContentPage
{
    private SfDockLayout dockLayout;
    
    public ResponsivePage()
    {
        dockLayout = new SfDockLayout();
        
        dockLayout.Children.Add(
            new Label { Text = "Header", HeightRequest = 60, Background = Colors.Blue }, 
            Dock.Top
        );
        
        dockLayout.Children.Add(
            new Label { Text = "Sidebar", WidthRequest = 200, Background = Colors.Green }, 
            Dock.Left
        );
        
        dockLayout.Children.Add(
            new Label { Text = "Content", Background = Colors.White }
        );
        
        Content = dockLayout;
    }
    
    protected override void OnSizeAllocated(double width, double height)
    {
        base.OnSizeAllocated(width, height);
        
        // Adjust spacing based on screen width
        if (width < 600) // Mobile
        {
            dockLayout.HorizontalSpacing = 4;
            dockLayout.VerticalSpacing = 4;
        }
        else if (width < 1200) // Tablet
        {
            dockLayout.HorizontalSpacing = 8;
            dockLayout.VerticalSpacing = 8;
        }
        else // Desktop
        {
            dockLayout.HorizontalSpacing = 16;
            dockLayout.VerticalSpacing = 16;
        }
    }
}
```

## Common Patterns

### Pattern: Card Dashboard

```xaml
<sf:SfDockLayout HorizontalSpacing="12" VerticalSpacing="12" Padding="12">
    <Label Text="Statistics Dashboard" 
           FontSize="24"
           HeightRequest="60"
           sf:SfDockLayout.Dock="Top" />
    
    <Grid ColumnDefinitions="*,*,*" ColumnSpacing="12">
        <Frame Grid.Column="0" Padding="16">
            <Label Text="Sales: $45k" />
        </Frame>
        <Frame Grid.Column="1" Padding="16">
            <Label Text="Users: 1.2k" />
        </Frame>
        <Frame Grid.Column="2" Padding="16">
            <Label Text="Orders: 234" />
        </Frame>
    </Grid>
</sf:SfDockLayout>
```

### Pattern: Collapsible Sidebar

```csharp
private bool isSidebarExpanded = true;
private BoxView sidebar;

private void ToggleSidebar()
{
    if (isSidebarExpanded)
    {
        sidebar.WidthRequest = 0;
        dockLayout.HorizontalSpacing = 0;
    }
    else
    {
        sidebar.WidthRequest = 250;
        dockLayout.HorizontalSpacing = 10;
    }
    
    isSidebarExpanded = !isSidebarExpanded;
}
```

## Troubleshooting

### Spacing Not Visible
- **Cause**: Background colors are the same or spacing value is too small
- **Solution**: Increase spacing value or use contrasting colors to verify

### Last Child Not Expanding
- **Cause**: `ShouldExpandLastChild` is set to `false`
- **Solution**: Either set to `true` or provide explicit size constraints

### Unexpected Empty Space
- **Cause**: `ShouldExpandLastChild` is `false` without explicit sizing
- **Solution**: Set explicit `WidthRequest` and `HeightRequest` on the last child

### Spacing Too Large
- **Cause**: Both margin and spacing applied
- **Solution**: Use either spacing properties OR margins on elements, not both simultaneously

### Inconsistent Gaps
- **Cause**: Mixed use of spacing properties and element margins
- **Solution**: Prefer spacing properties for consistency; reserve margins for internal element padding
