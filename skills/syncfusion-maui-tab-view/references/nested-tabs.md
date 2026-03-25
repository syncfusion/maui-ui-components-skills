# Nested Tabs in .NET MAUI Tab View

## Table of Contents
- [Overview](#overview)
- [Understanding Nested Tab Structure](#understanding-nested-tab-structure)
- [Basic Nested Tab Implementation](#basic-nested-tab-implementation)
- [Different Header Placements Per Level](#different-header-placements-per-level)
- [Navigation Between Nested Tabs](#navigation-between-nested-tabs)
- [Layout Patterns and Best Practices](#layout-patterns-and-best-practices)
- [Styling Nested Tabs](#styling-nested-tabs)
- [Performance Optimization with EnableVirtualization](#performance-optimization-with-enablevirtualization)
- [Best Practices](#best-practices)
- [Common Issues](#common-issues)
- [Three-Level Nesting Example](#three-level-nesting-example)

---

Guide for implementing nested tab structures with multiple levels of navigation in .NET MAUI Tab View.

## Overview

Nested tabs allow you to place SfTabView controls within the content of other SfTabView controls, creating hierarchical navigation structures. This is useful for complex applications with multiple levels of categorization.

## Understanding Nested Tab Structure

**Basic Concept:**
- Parent TabView contains primary navigation tabs
- Each parent tab's content can contain a child TabView
- Child TabViews have their own independent tab bars and content
- Each level can have different `TabBarPlacement` settings

**Use Cases:**
- Multi-level settings interfaces
- Categorized content with subcategories
- Dashboard with section-specific filters
- File explorers with folder hierarchies

## Basic Nested Tab Implementation

### Two-Level Nested Tabs

**XAML:**
```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:tabView="clr-namespace:Syncfusion.Maui.TabView;assembly=Syncfusion.Maui.TabView"
             x:Class="TabViewMauiSample.MainPage"
             BackgroundColor="White">
    
    <!-- Parent TabView -->
    <tabView:SfTabView x:Name="parentTabView"
                        TabBarBackground="#FDF8F6"
                        TabWidthMode="SizeToContent"
                        TabBarPlacement="Top"
                        IndicatorBackground="#6200EE"
                        IndicatorPlacement="Top">
        <tabView:SfTabView.Items>
            
            <!-- First Parent Tab with Nested TabView -->
            <tabView:SfTabItem Header="Photos">
                <tabView:SfTabItem.Content>
                    
                    <!-- Nested Child TabView -->
                    <tabView:SfTabView x:Name="photosTabView"
                                        TabBarBackground="#F5F5F5"
                                        TabWidthMode="Default"
                                        TabBarPlacement="Bottom"
                                        IndicatorBackground="#FF6B6B"
                                        IndicatorPlacement="Top">
                        <tabView:SfTabView.Items>
                            
                            <tabView:SfTabItem Header="Camera">
                                <tabView:SfTabItem.Content>
                                    <Grid BackgroundColor="#E3F2FD">
                                        <Label Text="Camera Roll" 
                                               HorizontalOptions="Center" 
                                               VerticalOptions="Center"
                                               FontSize="20" />
                                    </Grid>
                                </tabView:SfTabItem.Content>
                            </tabView:SfTabItem>
                            
                            <tabView:SfTabItem Header="Albums">
                                <tabView:SfTabItem.Content>
                                    <Grid BackgroundColor="#FFF3E0">
                                        <Label Text="Photo Albums" 
                                               HorizontalOptions="Center" 
                                               VerticalOptions="Center"
                                               FontSize="20" />
                                    </Grid>
                                </tabView:SfTabItem.Content>
                            </tabView:SfTabItem>
                            
                            <tabView:SfTabItem Header="Favorites">
                                <tabView:SfTabItem.Content>
                                    <Grid BackgroundColor="#F1F8E9">
                                        <Label Text="Favorite Photos" 
                                               HorizontalOptions="Center" 
                                               VerticalOptions="Center"
                                               FontSize="20" />
                                    </Grid>
                                </tabView:SfTabItem.Content>
                            </tabView:SfTabItem>
                            
                        </tabView:SfTabView.Items>
                    </tabView:SfTabView>
                    
                </tabView:SfTabItem.Content>
            </tabView:SfTabItem>
            
            <!-- Second Parent Tab with Nested TabView -->
            <tabView:SfTabItem Header="Videos">
                <tabView:SfTabItem.Content>
                    
                    <tabView:SfTabView TabBarBackground="#F5F5F5"
                                        TabWidthMode="Default"
                                        TabBarPlacement="Bottom"
                                        IndicatorBackground="#4CAF50">
                        <tabView:SfTabView.Items>
                            
                            <tabView:SfTabItem Header="Recent">
                                <tabView:SfTabItem.Content>
                                    <Grid BackgroundColor="#E8F5E9">
                                        <Label Text="Recent Videos" 
                                               HorizontalOptions="Center" 
                                               VerticalOptions="Center"
                                               FontSize="20" />
                                    </Grid>
                                </tabView:SfTabItem.Content>
                            </tabView:SfTabItem>
                            
                            <tabView:SfTabItem Header="Playlists">
                                <tabView:SfTabItem.Content>
                                    <Grid BackgroundColor="#FFF9C4">
                                        <Label Text="Video Playlists" 
                                               HorizontalOptions="Center" 
                                               VerticalOptions="Center"
                                               FontSize="20" />
                                    </Grid>
                                </tabView:SfTabItem.Content>
                            </tabView:SfTabItem>
                            
                        </tabView:SfTabView.Items>
                    </tabView:SfTabView>
                    
                </tabView:SfTabItem.Content>
            </tabView:SfTabItem>
            
            <!-- Third Parent Tab (No Nested Tabs) -->
            <tabView:SfTabItem Header="Documents">
                <tabView:SfTabItem.Content>
                    <Grid BackgroundColor="#FCE4EC">
                        <Label Text="All Documents" 
                               HorizontalOptions="Center" 
                               VerticalOptions="Center"
                               FontSize="20" />
                    </Grid>
                </tabView:SfTabItem.Content>
            </tabView:SfTabItem>
            
        </tabView:SfTabView.Items>
    </tabView:SfTabView>
    
</ContentPage>
```

### C# Implementation

```csharp
using Syncfusion.Maui.TabView;

namespace TabViewMauiSample
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            
            // Parent TabView
            var parentTabView = new SfTabView
            {
                TabBarBackground = new SolidColorBrush(Color.FromArgb("#FDF8F6")),
                TabWidthMode = TabWidthMode.SizeToContent,
                TabBarPlacement = TabBarPlacement.Top,
                IndicatorBackground = new SolidColorBrush(Color.FromArgb("#6200EE")),
                IndicatorPlacement = IndicatorPlacement.Top
            };
            
            // First parent tab with nested tabs (Photos)
            var photosTab = new SfTabItem { Header = "Photos" };
            
            // Child TabView for Photos
            var photosTabView = new SfTabView
            {
                TabBarBackground = new SolidColorBrush(Color.FromArgb("#F5F5F5")),
                TabWidthMode = TabWidthMode.Default,
                TabBarPlacement = TabBarPlacement.Bottom,
                IndicatorBackground = new SolidColorBrush(Color.FromArgb("#FF6B6B")),
                IndicatorPlacement = IndicatorPlacement.Top
            };
            
            // Add child tabs
            photosTabView.Items.Add(new SfTabItem
            {
                Header = "Camera",
                Content = new Grid
                {
                    BackgroundColor = Color.FromArgb("#E3F2FD"),
                    Children =
                    {
                        new Label
                        {
                            Text = "Camera Roll",
                            HorizontalOptions = LayoutOptions.Center,
                            VerticalOptions = LayoutOptions.Center,
                            FontSize = 20
                        }
                    }
                }
            });
            
            photosTabView.Items.Add(new SfTabItem
            {
                Header = "Albums",
                Content = new Grid
                {
                    BackgroundColor = Color.FromArgb("#FFF3E0"),
                    Children =
                    {
                        new Label
                        {
                            Text = "Photo Albums",
                            HorizontalOptions = LayoutOptions.Center,
                            VerticalOptions = LayoutOptions.Center,
                            FontSize = 20
                        }
                    }
                }
            });
            
            // Set nested TabView as parent tab content
            photosTab.Content = photosTabView;
            parentTabView.Items.Add(photosTab);
            
            // Add other parent tabs...
            
            this.Content = parentTabView;
        }
    }
}
```

## Different Header Placements Per Level

One of the key benefits of nested tabs is configuring different `TabBarPlacement` for each level:

### Pattern 1: Top Parent, Bottom Child

Common for mobile apps - main navigation at top, sub-navigation at bottom:

```xaml
<!-- Parent: Top -->
<tabView:SfTabView TabBarPlacement="Top">
    <tabView:SfTabView.Items>
        <tabView:SfTabItem Header="Section A">
            <tabView:SfTabItem.Content>
                <!-- Child: Bottom -->
                <tabView:SfTabView TabBarPlacement="Bottom">
                    <tabView:SfTabView.Items>
                        <tabView:SfTabItem Header="Sub 1" />
                        <tabView:SfTabItem Header="Sub 2" />
                    </tabView:SfTabView.Items>
                </tabView:SfTabView>
            </tabView:SfTabItem.Content>
        </tabView:SfTabItem>
    </tabView:SfTabView.Items>
</tabView:SfTabView>
```

### Pattern 2: Both Top (Different Heights)

Desktop pattern - primary and secondary navigation both at top:

```xaml
<!-- Parent: Top, Height 60 -->
<tabView:SfTabView TabBarPlacement="Top" TabBarHeight="60">
    <tabView:SfTabView.Items>
        <tabView:SfTabItem Header="Main Section">
            <tabView:SfTabItem.Content>
                <!-- Child: Top, Height 48 -->
                <tabView:SfTabView TabBarPlacement="Top" TabBarHeight="48">
                    <tabView:SfTabView.Items>
                        <tabView:SfTabItem Header="Subsection" />
                    </tabView:SfTabView.Items>
                </tabView:SfTabView>
            </tabView:SfTabItem.Content>
        </tabView:SfTabItem>
    </tabView:SfTabView.Items>
</tabView:SfTabView>
```

### Pattern 3: Bottom Parent, Top Child

Reversed pattern for specific use cases:

```xaml
<!-- Parent: Bottom -->
<tabView:SfTabView TabBarPlacement="Bottom">
    <tabView:SfTabView.Items>
        <tabView:SfTabItem Header="Main">
            <tabView:SfTabItem.Content>
                <!-- Child: Top -->
                <tabView:SfTabView TabBarPlacement="Top">
                    <tabView:SfTabView.Items>
                        <tabView:SfTabItem Header="Filter 1" />
                        <tabView:SfTabItem Header="Filter 2" />
                    </tabView:SfTabView.Items>
                </tabView:SfTabView>
            </tabView:SfTabItem.Content>
        </tabView:SfTabItem>
    </tabView:SfTabView.Items>
</tabView:SfTabView>
```

## Navigation Between Nested Tabs

### Programmatic Navigation

Navigate to specific tabs in nested structure:

```csharp
// Navigate parent tab
parentTabView.SelectedIndex = 1;  // Second tab

// Navigate child tab within current parent tab
var currentParentTab = parentTabView.Items[(int)parentTabView.SelectedIndex];
var childTabView = currentParentTab.Content as SfTabView;
if (childTabView != null)
{
    childTabView.SelectedIndex = 2;  // Third child tab
}
```

### Direct Access to Nested TabView

```csharp
// Reference nested TabView by name (from XAML)
photosTabView.SelectedIndex = 0;

// Or access through parent structure
var photosTab = parentTabView.Items[0];
var photosChildTabs = photosTab.Content as SfTabView;
photosChildTabs.SelectedIndex = 1;
```

### Coordinated Navigation

Synchronize parent and child tab selection:

```csharp
// When parent tab changes, reset child to first tab
parentTabView.SelectionChanged += (s, e) =>
{
    var newTab = parentTabView.Items[(int)e.NewIndex];
    if (newTab.Content is SfTabView childTabView)
    {
        childTabView.SelectedIndex = 0;  // Reset to first child tab
    }
};

// Track child tab selection
photosTabView.SelectionChanged += (s, e) =>
{
    Console.WriteLine($"Photos sub-tab changed to: {e.NewIndex}");
};
```

## Layout Patterns and Best Practices

### Pattern 1: Settings with Categories

```xaml
<tabView:SfTabView TabBarPlacement="Top" TabWidthMode="SizeToContent">
    <tabView:SfTabView.Items>
        
        <!-- General Settings -->
        <tabView:SfTabItem Header="General">
            <tabView:SfTabItem.Content>
                <tabView:SfTabView TabBarPlacement="Top" TabBarHeight="40">
                    <tabView:SfTabView.Items>
                        <tabView:SfTabItem Header="Appearance" />
                        <tabView:SfTabItem Header="Language" />
                        <tabView:SfTabItem Header="Notifications" />
                    </tabView:SfTabView.Items>
                </tabView:SfTabView>
            </tabView:SfTabItem.Content>
        </tabView:SfTabItem>
        
        <!-- Privacy Settings -->
        <tabView:SfTabItem Header="Privacy">
            <tabView:SfTabItem.Content>
                <tabView:SfTabView TabBarPlacement="Top" TabBarHeight="40">
                    <tabView:SfTabView.Items>
                        <tabView:SfTabItem Header="Data" />
                        <tabView:SfTabItem Header="Permissions" />
                        <tabView:SfTabItem Header="Security" />
                    </tabView:SfTabView.Items>
                </tabView:SfTabView>
            </tabView:SfTabItem.Content>
        </tabView:SfTabItem>
        
    </tabView:SfTabView.Items>
</tabView:SfTabView>
```

### Pattern 2: Dashboard with Filtered Views

```xaml
<tabView:SfTabView TabBarPlacement="Top">
    <tabView:SfTabView.Items>
        
        <tabView:SfTabItem Header="Sales">
            <tabView:SfTabItem.Content>
                <!-- Time period filters as nested tabs -->
                <tabView:SfTabView TabBarPlacement="Top" TabBarHeight="40">
                    <tabView:SfTabView.Items>
                        <tabView:SfTabItem Header="Today" />
                        <tabView:SfTabItem Header="This Week" />
                        <tabView:SfTabItem Header="This Month" />
                        <tabView:SfTabItem Header="This Year" />
                    </tabView:SfTabView.Items>
                </tabView:SfTabView>
            </tabView:SfTabItem.Content>
        </tabView:SfTabItem>
        
        <tabView:SfTabItem Header="Analytics" />
        <tabView:SfTabItem Header="Reports" />
        
    </tabView:SfTabView.Items>
</tabView:SfTabView>
```

### Pattern 3: Content Categories with Subcategories

```xaml
<tabView:SfTabView TabBarPlacement="Bottom" TabBarHeight="60">
    <tabView:SfTabView.Items>
        
        <tabView:SfTabItem Header="News" ImageSource="news.png" ImagePosition="Top">
            <tabView:SfTabItem.Content>
                <tabView:SfTabView TabBarPlacement="Top">
                    <tabView:SfTabView.Items>
                        <tabView:SfTabItem Header="Top Stories" />
                        <tabView:SfTabItem Header="Technology" />
                        <tabView:SfTabItem Header="Business" />
                        <tabView:SfTabItem Header="Sports" />
                    </tabView:SfTabView.Items>
                </tabView:SfTabView>
            </tabView:SfTabItem.Content>
        </tabView:SfTabItem>
        
        <tabView:SfTabItem Header="Videos" ImageSource="video.png" ImagePosition="Top" />
        <tabView:SfTabItem Header="Profile" ImageSource="user.png" ImagePosition="Top" />
        
    </tabView:SfTabView.Items>
</tabView:SfTabView>
```

## Styling Nested Tabs

### Visual Hierarchy with Colors

Differentiate parent and child tabs using colors:

```xaml
<!-- Parent: Dark theme -->
<tabView:SfTabView TabBarBackground="#6200EE"
                   IndicatorBackground="White">
    <tabView:SfTabView.Items>
        <tabView:SfTabItem Header="Section" TextColor="White">
            <tabView:SfTabItem.Content>
                <!-- Child: Light theme -->
                <tabView:SfTabView TabBarBackground="#F5F5F5"
                                   IndicatorBackground="#6200EE">
                    <tabView:SfTabView.Items>
                        <tabView:SfTabItem Header="Subsection" TextColor="#333333" />
                    </tabView:SfTabView.Items>
                </tabView:SfTabView>
            </tabView:SfTabItem.Content>
        </tabView:SfTabItem>
    </tabView:SfTabView.Items>
</tabView:SfTabView>
```

### Size Differentiation

Make parent tabs visually larger:

```xaml
<!-- Parent: Larger -->
<tabView:SfTabView TabBarHeight="60">
    <tabView:SfTabView.Items>
        <tabView:SfTabItem Header="Main" FontSize="16">
            <tabView:SfTabItem.Content>
                <!-- Child: Smaller -->
                <tabView:SfTabView TabBarHeight="44">
                    <tabView:SfTabView.Items>
                        <tabView:SfTabItem Header="Sub" FontSize="12" />
                    </tabView:SfTabView.Items>
                </tabView:SfTabView>
            </tabView:SfTabItem.Content>
        </tabView:SfTabItem>
    </tabView:SfTabView.Items>
</tabView:SfTabView>
```

## Performance Optimization with EnableVirtualization

When using nested tabs or tabs with many items, enable virtualization to improve initial loading performance:

**XAML:**
```xaml
<!-- Parent tab view with virtualization -->
<tabView:SfTabView EnableVirtualization="True" 
                   TabBarPlacement="Top">
    <tabView:SfTabView.Items>
        <tabView:SfTabItem Header="Category 1">
            <tabView:SfTabItem.Content>
                <!-- Child tab view with virtualization -->
                <tabView:SfTabView EnableVirtualization="True"
                                   TabBarPlacement="Bottom">
                    <tabView:SfTabView.Items>
                        <!-- Many child tabs -->
                        <tabView:SfTabItem Header="Item 1" />
                        <tabView:SfTabItem Header="Item 2" />
                        <!-- ... 20+ items ... -->
                    </tabView:SfTabView.Items>
                </tabView:SfTabView>
            </tabView:SfTabItem.Content>
        </tabView:SfTabItem>
    </tabView:SfTabView.Items>
</tabView:SfTabView>
```

**C#:**
```csharp
var parentTabView = new SfTabView
{
    EnableVirtualization = true,
    TabBarPlacement = TabBarPlacement.Top
};

var childTabView = new SfTabView
{
    EnableVirtualization = true,
    TabBarPlacement = TabBarPlacement.Bottom
};

// Add many items to child
for (int i = 1; i <= 30; i++)
{
    childTabView.Items.Add(new SfTabItem 
    { 
        Header = $"Item {i}",
        Content = CreateContentView(i)
    });
}

parentTabView.Items.Add(new SfTabItem
{
    Header = "Category 1",
    Content = childTabView
});
```

**When to Enable:**
- Parent or child tab view has 10+ items
- Complex nested structures with heavy content
- ItemsSource binding with large collections
- Targeting lower-end devices

**Benefits:**
- Faster initial rendering of nested structures
- Reduced memory usage with many nested tabs
- Improved scrolling performance
- Better user experience on resource-constrained devices

## Best Practices

1. **Limit Nesting Depth:**
   - Maximum 2-3 levels of nesting
   - Deeper nesting becomes confusing for users

2. **Visual Differentiation:**
   - Use different colors/sizes for parent vs child tabs
   - Different placements help distinguish levels

3. **Navigation Clarity:**
   - Make it clear which level user is navigating
   - Consider breadcrumbs for deep nesting

4. **Performance:**
   - Enable virtualization with `EnableVirtualization="True"` for nested views with many tabs
   - Lazy load nested tab content when possible
   - Don't create all nested structures upfront if not needed

5. **Consistent Patterns:**
   - Use same placement pattern throughout app
   - Maintain visual hierarchy consistently

6. **Mobile Considerations:**
   - Parent: Top or Bottom
   - Child: Opposite of parent for clear separation

7. **Tab Reset:**
   - Consider resetting child tabs when parent changes
   - Or preserve child selection based on use case

## Common Issues

**Issue:** Nested tabs not rendering  
**Solution:** Ensure parent tab Content is set to child SfTabView, verify both have Items populated

**Issue:** Confusing navigation with same placement  
**Solution:** Use different TabBarPlacement or visual styling for parent vs child

**Issue:** Performance degradation with many nested levels  
**Solution:** Reduce nesting depth, implement lazy loading, simplify tab structure

**Issue:** Touch targets too small with multiple tab bars  
**Solution:** Adjust TabBarHeight appropriately for each level, ensure minimum 44pt touch targets

**Issue:** Indicators overlapping  
**Solution:** Use different IndicatorPlacement for each level (e.g., parent=Bottom, child=Top)

## Three-Level Nesting Example

For complex hierarchies (use sparingly):

```xaml
<!-- Level 1: Main Categories -->
<tabView:SfTabView TabBarPlacement="Top" TabBarHeight="60">
    <tabView:SfTabView.Items>
        <tabView:SfTabItem Header="Products">
            <tabView:SfTabItem.Content>
                <!-- Level 2: Product Categories -->
                <tabView:SfTabView TabBarPlacement="Top" TabBarHeight="48">
                    <tabView:SfTabView.Items>
                        <tabView:SfTabItem Header="Electronics">
                            <tabView:SfTabItem.Content>
                                <!-- Level 3: Subcategories -->
                                <tabView:SfTabView TabBarPlacement="Bottom" TabBarHeight="40">
                                    <tabView:SfTabView.Items>
                                        <tabView:SfTabItem Header="Phones" />
                                        <tabView:SfTabItem Header="Laptops" />
                                        <tabView:SfTabItem Header="Tablets" />
                                    </tabView:SfTabView.Items>
                                </tabView:SfTabView>
                            </tabView:SfTabItem.Content>
                        </tabView:SfTabItem>
                    </tabView:SfTabView.Items>
                </tabView:SfTabView>
            </tabView:SfTabItem.Content>
        </tabView:SfTabItem>
    </tabView:SfTabView.Items>
</tabView:SfTabView>
```

**⚠️ Warning:** Three levels is the maximum recommended. Beyond this, consider alternative navigation patterns like drill-down lists or hierarchical menus.
