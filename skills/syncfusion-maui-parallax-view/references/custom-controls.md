# Custom Controls Support

The Syncfusion .NET MAUI Parallax View supports custom scrollable controls beyond the built-in ScrollView and ListView. This guide explains how to implement the `IParallaxView` interface to enable parallax effects with any custom scrollable control.

## When to Use Custom Controls

Use custom controls with parallax when you need to:

- **Integrate third-party scrollable controls** not natively supported by SfParallaxView
- **Create custom scrolling behaviors** with specific performance or interaction requirements
- **Build specialized UI components** like custom carousels, scrollable grids, or infinite scrollers
- **Extend existing controls** to support parallax without modifying the SfParallaxView component

The built-in ScrollView and SfListView already implement IParallaxView, so use custom controls only when these don't meet your needs.

## IParallaxView Interface

The `IParallaxView` interface from `Syncfusion.Maui.Core` provides the contract that enables any control to work with SfParallaxView.

### Interface Definition

```csharp
using Syncfusion.Maui.Core;

public interface IParallaxView
{
    /// <summary>
    /// Gets or sets the total size of the scrollable content.
    /// This represents the entire scrollable area, not just the visible viewport.
    /// </summary>
    Size ScrollableContentSize { get; set; }
    
    /// <summary>
    /// Event raised when scrolling occurs in the control.
    /// Provides scroll position information to the SfParallaxView.
    /// </summary>
    event EventHandler<ParallaxScrollingEventArgs> Scrolling;
}
```

### Interface Members

#### ScrollableContentSize Property

**Purpose:** Represents the total scrollable content size (not just the visible area).

**Type:** `System.Drawing.Size` or `Microsoft.Maui.Graphics.Size`

**When to Set:** 
- In the control's constructor (if size is known)
- In `MeasureOverride` or `OnSizeAllocated` (for dynamic sizing)
- When content is loaded or changed

**Example:**
```csharp
public Size ScrollableContentSize { get; set; }

public CustomScrollView()
{
    // Set total scrollable size
    this.ScrollableContentSize = new Size(400, 2000); // Width: 400, Height: 2000
}
```

#### Scrolling Event

**Purpose:** Notifies SfParallaxView when the control scrolls, providing position information.

**Event Args:** `ParallaxScrollingEventArgs`

**When to Raise:** Whenever the control's scroll position changes.

**Example:**
```csharp
public event EventHandler<ParallaxScrollingEventArgs> Scrolling;

private void OnScrollChanged(double scrollX, double scrollY)
{
    // Raise the event to notify SfParallaxView
    Scrolling?.Invoke(this, new ParallaxScrollingEventArgs(scrollX, scrollY, false));
}
```

## ParallaxScrollingEventArgs

The event args class that communicates scroll information to the SfParallaxView.

### Properties

| Property | Type | Description |
|----------|------|-------------|
| **ScrollX** | double | Horizontal scroll position (X-axis) |
| **ScrollY** | double | Vertical scroll position (Y-axis) |
| **CanAnimate** | bool | Whether to animate the parallax transition |

### Constructor

```csharp
public ParallaxScrollingEventArgs(double scrollX, double scrollY, bool canAnimate)
{
    ScrollX = scrollX;
    ScrollY = scrollY;
    CanAnimate = canAnimate;
}
```

**Parameters:**
- `scrollX`: The X position of the finished scroll (left edge of viewport)
- `scrollY`: The Y position of the finished scroll (top edge of viewport)
- `canAnimate`: Set to `true` for smooth animated transitions, `false` for immediate updates

**Typical Usage:**
```csharp
// For immediate updates (most common)
new ParallaxScrollingEventArgs(e.ScrollX, e.ScrollY, false)

// For animated transitions
new ParallaxScrollingEventArgs(e.ScrollX, e.ScrollY, true)
```

## Complete Custom Control Implementation

Here's a complete example of implementing a custom ListView with parallax support:

### CustomListView Implementation

```csharp
using Syncfusion.Maui.Core;
using System.Collections;

namespace ParallaxViewCustomControl
{
    /// <summary>
    /// Custom ListView that implements IParallaxView for parallax scrolling support.
    /// </summary>
    public class CustomListView : ListView, IParallaxView
    {
        // IParallaxView interface members
        public Size ScrollableContentSize { get; set; }
        public event EventHandler<ParallaxScrollingEventArgs>? Scrolling;
        
        public CustomListView()
        {
            // Subscribe to the ListView's built-in Scrolled event
            this.Scrolled += CustomListView_Scrolled;
        }
        
        /// <summary>
        /// Handle scrolling and notify the SfParallaxView
        /// </summary>
        private void CustomListView_Scrolled(object? sender, ScrolledEventArgs e)
        {
            if (sender is ListView listView && Scrolling != null)
            {
                // Raise the Scrolling event with current scroll position
                // CanAnimate = false for immediate updates (better performance)
                Scrolling.Invoke(this, new ParallaxScrollingEventArgs(
                    e.ScrollX, 
                    e.ScrollY, 
                    false));
            }
        }
        
        /// <summary>
        /// Override measure to calculate and set the total scrollable content size
        /// </summary>
        protected override Size MeasureOverride(double widthConstraint, double heightConstraint)
        {
            var minimumSize = new Size(40, 40);
            Size request = Size.Zero;
            
            // Calculate total content size for uniform row heights
            if (ItemsSource is IList list && 
                HasUnevenRows == false && 
                RowHeight > 0 && 
                !IsGroupingEnabled)
            {
                // Total height = number of items × row height
                request = new Size(widthConstraint, list.Count * RowHeight);
            }
            
            // Set the scrollable content size for parallax calculations
            this.ScrollableContentSize = new SizeRequest(request, minimumSize);
            
            return base.MeasureOverride(widthConstraint, heightConstraint);
        }
        
        /// <summary>
        /// Clean up event subscriptions
        /// </summary>
        protected override void OnHandlerChanged()
        {
            base.OnHandlerChanged();
            
            if (Handler == null)
            {
                this.Scrolled -= CustomListView_Scrolled;
            }
        }
    }
}
```

### Using the Custom Control

**XAML:**
```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    xmlns:parallax="clr-namespace:Syncfusion.Maui.ParallaxView;assembly=Syncfusion.Maui.ParallaxView"
    xmlns:local="clr-namespace:ParallaxViewCustomControl"
    x:Class="ParallaxViewCustomControl.MainPage">
    
    <Grid>
        <!-- Parallax View with custom control as Source -->
        <parallax:SfParallaxView Source="{x:Reference customListView}" x:Name="parallaxView">
            <parallax:SfParallaxView.Content>
                <Image Source="{Binding BackgroundImage}" 
                       BackgroundColor="Transparent"
                       HorizontalOptions="Fill" 
                       VerticalOptions="Fill" 
                       Aspect="AspectFill" />
            </parallax:SfParallaxView.Content>
        </parallax:SfParallaxView>
        
        <!-- Custom ListView -->
        <local:CustomListView x:Name="customListView"
                              ItemsSource="{Binding Items}"
                              BackgroundColor="Transparent"
                              RowHeight="100"
                              HasUnevenRows="False">
            <local:CustomListView.ItemTemplate>
                <DataTemplate>
                    <ViewCell>
                        <Grid Padding="15" BackgroundColor="#80000000">
                            <Label Text="{Binding Title}" 
                                   FontSize="18" 
                                   TextColor="White" />
                        </Grid>
                    </ViewCell>
                </DataTemplate>
            </local:CustomListView.ItemTemplate>
        </local:CustomListView>
    </Grid>
</ContentPage>
```

**C# Code-Behind:**
```csharp
using Syncfusion.Maui.ParallaxView;

namespace ParallaxViewCustomControl
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            BindingContext = new ParallaxViewModel();
        }
    }
    
    public class ParallaxViewModel
    {
        public ImageSource BackgroundImage { get; set; }
        public List<ItemData> Items { get; set; }
        
        public ParallaxViewModel()
        {
            BackgroundImage = ImageSource.FromFile("background.jpg");
            
            Items = new List<ItemData>
            {
                new ItemData { Title = "Item 1" },
                new ItemData { Title = "Item 2" },
                new ItemData { Title = "Item 3" },
                new ItemData { Title = "Item 4" },
                new ItemData { Title = "Item 5" },
                // Add more items...
            };
        }
    }
    
    public class ItemData
    {
        public string Title { get; set; }
    }
}
```

## Advanced Custom Control Example

Here's a more advanced custom control with dynamic content size:

```csharp
using Syncfusion.Maui.Core;

namespace ParallaxViewCustomControl
{
    public class CustomScrollableView : ContentView, IParallaxView
    {
        private ScrollView _scrollView;
        
        public Size ScrollableContentSize { get; set; }
        public event EventHandler<ParallaxScrollingEventArgs>? Scrolling;
        
        public CustomScrollableView()
        {
            // Create internal ScrollView
            _scrollView = new ScrollView
            {
                BackgroundColor = Colors.Transparent
            };
            
            _scrollView.Scrolled += OnScrollViewScrolled;
            
            // Set as content
            Content = _scrollView;
        }
        
        /// <summary>
        /// Set the scrollable content
        /// </summary>
        public View ScrollableContent
        {
            get => _scrollView.Content;
            set
            {
                _scrollView.Content = value;
                UpdateScrollableContentSize();
            }
        }
        
        private void OnScrollViewScrolled(object sender, ScrolledEventArgs e)
        {
            // Notify parallax view of scroll changes
            Scrolling?.Invoke(this, new ParallaxScrollingEventArgs(
                e.ScrollX, 
                e.ScrollY, 
                false));
        }
        
        private void UpdateScrollableContentSize()
        {
            if (_scrollView.Content != null)
            {
                // Measure the content
                var contentSize = _scrollView.Content.Measure(
                    double.PositiveInfinity, 
                    double.PositiveInfinity);
                
                ScrollableContentSize = contentSize.Request;
            }
        }
        
        protected override void OnSizeAllocated(double width, double height)
        {
            base.OnSizeAllocated(width, height);
            UpdateScrollableContentSize();
        }
    }
}
```

## Implementation Checklist

When implementing IParallaxView for custom controls, ensure you:

- ✅ **Implement ScrollableContentSize property** - Store and update the total content size
- ✅ **Implement Scrolling event** - Declare the event with correct signature
- ✅ **Calculate content size accurately** - In constructor, MeasureOverride, or OnSizeAllocated
- ✅ **Raise Scrolling event on scroll changes** - Hook into native scroll events
- ✅ **Pass correct scroll positions** - ScrollX and ScrollY represent viewport position
- ✅ **Use CanAnimate appropriately** - Usually `false` for better performance
- ✅ **Handle cleanup** - Unsubscribe from events when control is disposed
- ✅ **Test on all platforms** - Verify behavior on Android, iOS, Windows, macOS

## Sample Repository

For complete working examples of custom controls with parallax:

**GitHub Repository:**  
[MAUI Parallax View Sample Demos](https://github.com/SyncfusionExamples/MAUI-Parallax-View-Sample-Demos)

**What you'll find:**
- CustomListView with IParallaxView implementation
- Integration examples with XAML and C#
- ViewModels with sample data
- MeasureOverride patterns for dynamic sizing
- Event handler best practices

## Common Implementation Patterns

### Pattern 1: Wrapping Existing ScrollView

```csharp
public class CustomScrollWrapper : ContentView, IParallaxView
{
    private ScrollView _scrollView;
    
    public Size ScrollableContentSize { get; set; }
    public event EventHandler<ParallaxScrollingEventArgs> Scrolling;
    
    public CustomScrollWrapper()
    {
        _scrollView = new ScrollView();
        _scrollView.Scrolled += (s, e) => 
            Scrolling?.Invoke(this, new ParallaxScrollingEventArgs(e.ScrollX, e.ScrollY, false));
        Content = _scrollView;
    }
}
```

### Pattern 2: Extending CollectionView

```csharp
public class ParallaxCollectionView : CollectionView, IParallaxView
{
    public Size ScrollableContentSize { get; set; }
    public event EventHandler<ParallaxScrollingEventArgs> Scrolling;
    
    public ParallaxCollectionView()
    {
        Scrolled += (s, e) => 
            Scrolling?.Invoke(this, new ParallaxScrollingEventArgs(e.ScrollX, e.ScrollY, false));
    }
}
```

### Pattern 3: Custom Grid with Scrolling

```csharp
public class ScrollableGrid : Grid, IParallaxView
{
    private ScrollView _internalScroll;
    
    public Size ScrollableContentSize { get; set; }
    public event EventHandler<ParallaxScrollingEventArgs> Scrolling;
    
    // Implementation details...
}
```

## Troubleshooting

### Issue: Scrolling event not firing
**Cause:** Event not properly subscribed or raised  
**Solution:** Verify you're subscribing to the underlying control's scroll event and raising the Scrolling event with correct parameters

### Issue: ScrollableContentSize is zero
**Cause:** Content size not calculated or set  
**Solution:** Calculate size in MeasureOverride or OnSizeAllocated. For dynamic content, recalculate when content changes

### Issue: Parallax effect jerky or inconsistent
**Cause:** Incorrect scroll position values  
**Solution:** Ensure ScrollX/ScrollY represent the actual viewport position, not delta values. Use CanAnimate=false for smoother performance

### Issue: Memory leaks
**Cause:** Event subscriptions not cleaned up  
**Solution:** Unsubscribe from events in OnHandlerChanged or Dispose methods

## Next Steps

- **[Getting Started](getting-started.md)** - Review basic parallax setup
- **[Source Binding](source-binding.md)** - Learn about built-in control integration
- **[Customization](customization.md)** - Adjust speed and orientation settings

## Best Practices

1. **Use built-in controls when possible** - ScrollView and SfListView already work with parallax
2. **Calculate size accurately** - Incorrect ScrollableContentSize causes positioning issues
3. **Optimize event handling** - Raise Scrolling event only when position changes
4. **Test performance** - Custom controls may impact scroll performance
5. **Document assumptions** - Clearly document expected content types and sizes
6. **Handle edge cases** - Empty content, single item, very large content
7. **Platform-specific testing** - Verify behavior on all target platforms
