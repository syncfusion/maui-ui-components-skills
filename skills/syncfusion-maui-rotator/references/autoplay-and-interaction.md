# Autoplay and User Interaction

## Table of Contents
- [Overview](#overview)
- [EnableAutoPlay](#enableautoplay)
- [NavigationDelay](#navigationdelay)
- [EnableLooping](#enablelooping)
- [EnableSwiping](#enableswiping)
- [SelectedIndex](#selectedindex)
- [Complete Examples](#complete-examples)
- [Best Practices](#best-practices)

## Overview

The SfRotator provides comprehensive control over automatic advancement and user interaction:

- **EnableAutoPlay**: Automatically advance through items
- **NavigationDelay**: Control timing between advances
- **EnableLooping**: Enable infinite scrolling
- **EnableSwiping**: Control user swipe gestures
- **SelectedIndex**: Programmatically select items

## EnableAutoPlay

Automatically advances through rotator items at specified intervals.

**Property:** `EnableAutoPlay`  
**Type:** `bool`  
**Default:** `false`

### Basic Autoplay

**XAML:**
```xml
<syncfusion:SfRotator ItemsSource="{Binding ImageCollection}"
                      EnableAutoPlay="True"
                      NavigationDelay="3000"
                      HeightRequest="400">
    <syncfusion:SfRotator.ItemTemplate>
        <DataTemplate>
            <Image Source="{Binding Image}"/>
        </DataTemplate>
    </syncfusion:SfRotator.ItemTemplate>
</syncfusion:SfRotator>
```

**C#:**
```csharp
rotator.EnableAutoPlay = true;
rotator.NavigationDelay = 3000; // 3 seconds
```

### Complete Autoplay Example

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:syncfusion="clr-namespace:Syncfusion.Maui.Rotator;assembly=Syncfusion.Maui.Rotator"
             xmlns:local="clr-namespace:YourApp"
             x:Class="YourApp.MainPage">
    <ContentPage.BindingContext>
        <local:RotatorViewModel/>
    </ContentPage.BindingContext>
    
    <ContentPage.Content>
        <syncfusion:SfRotator ItemsSource="{Binding ImageCollection}"
                              SelectedIndex="2"
                              NavigationDirection="Horizontal"
                              NavigationStripMode="Thumbnail"
                              BackgroundColor="#ececec"
                              EnableAutoPlay="True"
                              NavigationStripPosition="Bottom"
                              HeightRequest="500">
            <syncfusion:SfRotator.ItemTemplate>
                <DataTemplate>
                    <Image Source="{Binding Image}"/>
                </DataTemplate>
            </syncfusion:SfRotator.ItemTemplate>
        </syncfusion:SfRotator>
    </ContentPage.Content>
</ContentPage>
```

**C# Example:**
```csharp
using Syncfusion.Maui.Rotator;

public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        
        SfRotator rotator = new SfRotator();
        
        var imageCollection = new List<RotatorModel>
        {
            new RotatorModel("image1.png"),
            new RotatorModel("image2.png"),
            new RotatorModel("image3.png"),
            new RotatorModel("image4.png"),
            new RotatorModel("image5.png")
        };
        
        var itemTemplate = new DataTemplate(() =>
        {
            var grid = new Grid();
            var image = new Image();
            image.SetBinding(Image.SourceProperty, "Image");
            grid.Children.Add(image);
            return grid;
        });
        
        rotator.ItemTemplate = itemTemplate;
        rotator.EnableAutoPlay = true;
        rotator.NavigationDelay = 2000;
        rotator.ItemsSource = imageCollection;
        
        this.Content = rotator;
    }
}
```

## NavigationDelay

Specifies the delay (in milliseconds) between automatic item transitions when EnableAutoPlay is true.

**Property:** `NavigationDelay`  
**Type:** `int`  
**Unit:** Milliseconds  
**Default:** 2000 (2 seconds)  
**Recommended Range:** 1000 - 10000 ms

### Setting Navigation Delay

**XAML:**
```xml
<syncfusion:SfRotator NavigationDelay="5000"
                      EnableAutoPlay="True"
                      ItemsSource="{Binding ImageCollection}">
    <!-- Content -->
</syncfusion:SfRotator>
```

**C#:**
```csharp
rotator.NavigationDelay = 5000; // 5 seconds
rotator.EnableAutoPlay = true;
```

### Complete Example with Custom Delay

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:syncfusion="clr-namespace:Syncfusion.Maui.Rotator;assembly=Syncfusion.Maui.Rotator"
             xmlns:local="clr-namespace:YourApp"
             x:Class="YourApp.MainPage">
    <ContentPage.BindingContext>
        <local:RotatorViewModel/>
    </ContentPage.BindingContext>
    
    <ContentPage.Content>
        <syncfusion:SfRotator NavigationDelay="4000"
                              ItemsSource="{Binding ImageCollection}"
                              SelectedIndex="2"
                              NavigationDirection="Horizontal"
                              NavigationStripMode="Thumbnail"
                              BackgroundColor="#ececec"
                              EnableAutoPlay="True"
                              NavigationStripPosition="Bottom"
                              HeightRequest="500">
            <syncfusion:SfRotator.ItemTemplate>
                <DataTemplate>
                    <Image Source="{Binding Image}"/>
                </DataTemplate>
            </syncfusion:SfRotator.ItemTemplate>
        </syncfusion:SfRotator>
    </ContentPage.Content>
</ContentPage>
```

### Delay Guidelines

**Fast Rotation (1000-2000ms):**
- Promotional banners
- Quick image previews
- High-level overviews

**Medium Rotation (2000-4000ms):**
- Product showcases
- Feature highlights
- General slideshows

**Slow Rotation (4000-8000ms):**
- Detailed product views
- Reading content (testimonials, quotes)
- Complex visuals requiring comprehension

## EnableLooping

Enables infinite scrolling from the last item back to the first (and vice versa).

**Property:** `EnableLooping`  
**Type:** `bool`  
**Default:** `false`

### Basic Looping

**XAML:**
```xml
<syncfusion:SfRotator EnableLooping="True"
                      EnableAutoPlay="True"
                      NavigationDelay="3000"
                      ItemsSource="{Binding ImageCollection}">
    <!-- Content -->
</syncfusion:SfRotator>
```

**C#:**
```csharp
rotator.EnableLooping = true;
rotator.EnableAutoPlay = true;
rotator.NavigationDelay = 3000;
```

### Complete Looping Example

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:syncfusion="clr-namespace:Syncfusion.Maui.Rotator;assembly=Syncfusion.Maui.Rotator"
             xmlns:local="clr-namespace:YourApp"
             x:Class="YourApp.MainPage">
    <ContentPage.BindingContext>
        <local:RotatorViewModel/>
    </ContentPage.BindingContext>
    
    <ContentPage.Content>
        <syncfusion:SfRotator NavigationDelay="2000"
                              ItemsSource="{Binding ImageCollection}"
                              SelectedIndex="2"
                              NavigationDirection="Horizontal"
                              NavigationStripMode="Thumbnail"
                              BackgroundColor="#ececec"
                              EnableAutoPlay="True"
                              EnableLooping="True"
                              NavigationStripPosition="Bottom"
                              HeightRequest="500">
            <syncfusion:SfRotator.ItemTemplate>
                <DataTemplate>
                    <Image Source="{Binding Image}"/>
                </DataTemplate>
            </syncfusion:SfRotator.ItemTemplate>
        </syncfusion:SfRotator>
    </ContentPage.Content>
</ContentPage>
```

**C# Example:**
```csharp
using Syncfusion.Maui.Rotator;

SfRotator rotator = new SfRotator();

var imageCollection = new List<RotatorModel>
{
    new RotatorModel("image1.png"),
    new RotatorModel("image2.png"),
    new RotatorModel("image3.png"),
    new RotatorModel("image4.png"),
    new RotatorModel("image5.png")
};

var itemTemplate = new DataTemplate(() =>
{
    var grid = new Grid();
    var image = new Image();
    image.SetBinding(Image.SourceProperty, "Image");
    grid.Children.Add(image);
    return grid;
});

rotator.ItemTemplate = itemTemplate;
rotator.EnableAutoPlay = true;
rotator.NavigationDelay = 2000;
rotator.EnableLooping = true; // Infinite scroll
rotator.ItemsSource = imageCollection;
```

### When to Use Looping

**Enable Looping:**
- Promotional carousels (continuous display)
- Dashboard widgets
- Banners and announcements
- Photo galleries (seamless browsing)

**Disable Looping:**
- Tutorials or walkthroughs (defined end)
- Onboarding sequences
- Step-by-step processes
- Limited, sequential content

## EnableSwiping

Controls whether users can swipe/drag to navigate between items.

**Property:** `EnableSwiping`  
**Type:** `bool`  
**Default:** `true`

### Disable User Swiping

Useful for auto-only navigation or restricted interaction.

**XAML:**
```xml
<syncfusion:SfRotator ItemsSource="{Binding ImageCollection}"
                      EnableSwiping="False"
                      EnableAutoPlay="True"
                      NavigationDelay="3000"
                      BackgroundColor="#ececec"
                      HeightRequest="400">
    <syncfusion:SfRotator.ItemTemplate>
        <DataTemplate>
            <Image Source="{Binding Image}"/>
        </DataTemplate>
    </syncfusion:SfRotator.ItemTemplate>
</syncfusion:SfRotator>
```

**C#:**
```csharp
rotator.EnableSwiping = false;
rotator.EnableAutoPlay = true;
```

### Complete Example - Auto-Only Navigation

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:syncfusion="clr-namespace:Syncfusion.Maui.Rotator;assembly=Syncfusion.Maui.Rotator"
             xmlns:local="clr-namespace:YourApp"
             x:Class="YourApp.MainPage">
    <ContentPage.BindingContext>
        <local:RotatorViewModel/>
    </ContentPage.BindingContext>
    
    <ContentPage.Content>
        <syncfusion:SfRotator ItemsSource="{Binding ImageCollection}"
                              BackgroundColor="#ececec"
                              EnableSwiping="False"
                              EnableAutoPlay="True"
                              NavigationDelay="4000"
                              EnableLooping="True"
                              ShowNavigationButton="False"
                              NavigationStripMode="Dots"
                              HeightRequest="400">
            <syncfusion:SfRotator.ItemTemplate>
                <DataTemplate>
                    <Image Source="{Binding Image}"/>
                </DataTemplate>
            </syncfusion:SfRotator.ItemTemplate>
        </syncfusion:SfRotator>
    </ContentPage.Content>
</ContentPage>
```

**C# Example:**
```csharp
SfRotator rotator = new SfRotator();

rotator.ItemsSource = imageCollection;
rotator.EnableSwiping = false; // Disable user interaction
rotator.EnableAutoPlay = true;
rotator.NavigationDelay = 4000;
rotator.EnableLooping = true;
rotator.ShowNavigationButton = false; // No manual controls
```

### When to Disable Swiping

**Disable:**
- Auto-only promotional banners
- Controlled presentations
- Preventing accidental navigation
- Dashboard displays

**Enable (Default):**
- User-driven galleries
- Photo browsers
- Product catalogs
- Interactive content

## SelectedIndex

Programmatically selects a specific item by its zero-based index.

**Property:** `SelectedIndex`  
**Type:** `int`  
**Default:** 0 (first item)  
**Range:** 0 to (ItemCount - 1)

### Setting Initial Selection

**XAML:**
```xml
<syncfusion:SfRotator ItemsSource="{Binding ImageCollection}"
                      SelectedIndex="2"
                      HeightRequest="400">
    <!-- Content -->
</syncfusion:SfRotator>
```

**C#:**
```csharp
rotator.SelectedIndex = 2; // Select third item (zero-based)
```

### Programmatic Navigation

```csharp
public partial class MainPage : ContentPage
{
    private SfRotator rotator;
    
    public MainPage()
    {
        InitializeComponent();
        rotator = this.FindByName<SfRotator>("rotator");
    }
    
    private void OnNextClicked(object sender, EventArgs e)
    {
        if (rotator.SelectedIndex < rotator.ItemsSource.Cast<object>().Count() - 1)
        {
            rotator.SelectedIndex++;
        }
    }
    
    private void OnPreviousClicked(object sender, EventArgs e)
    {
        if (rotator.SelectedIndex > 0)
        {
            rotator.SelectedIndex--;
        }
    }
    
    private void OnGoToFirstClicked(object sender, EventArgs e)
    {
        rotator.SelectedIndex = 0;
    }
    
    private void OnGoToLastClicked(object sender, EventArgs e)
    {
        var count = rotator.ItemsSource.Cast<object>().Count();
        rotator.SelectedIndex = count - 1;
    }
}
```

### Binding SelectedIndex

```xml
<syncfusion:SfRotator ItemsSource="{Binding ImageCollection}"
                      SelectedIndex="{Binding CurrentIndex, Mode=TwoWay}"
                      HeightRequest="400">
    <!-- Content -->
</syncfusion:SfRotator>

<Label Text="{Binding CurrentIndex, StringFormat='Current: {0}'}" />
```

```csharp
public class RotatorViewModel : INotifyPropertyChanged
{
    private int _currentIndex;
    public int CurrentIndex
    {
        get => _currentIndex;
        set
        {
            _currentIndex = value;
            OnPropertyChanged(nameof(CurrentIndex));
        }
    }
    
    // INotifyPropertyChanged implementation
}
```

## Complete Examples

### Example 1: Auto-Advancing Banner

```xml
<syncfusion:SfRotator ItemsSource="{Binding Banners}"
                      EnableAutoPlay="True"
                      NavigationDelay="5000"
                      EnableLooping="True"
                      EnableSwiping="False"
                      ShowNavigationButton="False"
                      NavigationStripMode="Dots"
                      DotPlacement="Outside"
                      SelectedDotColor="White"
                      UnselectedDotColor="#80FFFFFF"
                      BackgroundColor="Black"
                      HeightRequest="300">
    <syncfusion:SfRotator.ItemTemplate>
        <DataTemplate>
            <Image Source="{Binding Image}" Aspect="AspectFill"/>
        </DataTemplate>
    </syncfusion:SfRotator.ItemTemplate>
</syncfusion:SfRotator>
```

### Example 2: User-Controlled Gallery

```xml
<syncfusion:SfRotator ItemsSource="{Binding Photos}"
                      EnableSwiping="True"
                      EnableAutoPlay="False"
                      EnableLooping="False"
                      NavigationStripMode="Thumbnail"
                      SelectedThumbnailStroke="Blue"
                      HeightRequest="500">
    <syncfusion:SfRotator.ItemTemplate>
        <DataTemplate>
            <Image Source="{Binding Image}" Aspect="AspectFit"/>
        </DataTemplate>
    </syncfusion:SfRotator.ItemTemplate>
</syncfusion:SfRotator>
```

### Example 3: Controlled Slideshow with Custom Controls

```csharp
var rotator = new SfRotator
{
    ItemsSource = slides,
    EnableSwiping = false,
    EnableAutoPlay = false,
    SelectedIndex = 0,
    HeightRequest = 600
};

// Custom navigation buttons
var previousButton = new Button { Text = "Previous" };
previousButton.Clicked += (s, e) =>
{
    if (rotator.SelectedIndex > 0)
        rotator.SelectedIndex--;
};

var nextButton = new Button { Text = "Next" };
nextButton.Clicked += (s, e) =>
{
    var maxIndex = ((IEnumerable<object>)rotator.ItemsSource).Count() - 1;
    if (rotator.SelectedIndex < maxIndex)
        rotator.SelectedIndex++;
};
```

## Best Practices

### 1. Timing Guidelines

```csharp
// Fast-paced content
rotator.NavigationDelay = 2000; // 2 seconds

// Standard content
rotator.NavigationDelay = 4000; // 4 seconds

// Content requiring reading
rotator.NavigationDelay = 6000; // 6 seconds
```

### 2. Combine Settings Appropriately

**Auto-only banner:**
```csharp
EnableAutoPlay = true
EnableLooping = true
EnableSwiping = false
ShowNavigationButton = false
```

**Interactive gallery:**
```csharp
EnableAutoPlay = false
EnableLooping = false
EnableSwiping = true
ShowNavigationButton = true
```

**Mixed mode:**
```csharp
EnableAutoPlay = true
EnableLooping = true
EnableSwiping = true  // User can override auto
```

### 3. Accessibility

- Don't disable swiping unless necessary
- Provide alternative navigation (dots, thumbnails, buttons)
- Use appropriate delays (avoid too fast)
- Test with real users

### 4. Performance

- Limit autoplay for large image sets
- Consider battery impact of continuous animation
- Pause autoplay when app is backgrounded (if needed)

## Common Issues

### Issue: Autoplay Not Working

**Check:**
1. `EnableAutoPlay` is `true`
2. `NavigationDelay` is set
3. `ItemsSource` has multiple items
4. `EnableLooping` is `true` if reaching last item

### Issue: Swiping Not Working

**Check:**
1. `EnableSwiping` is `true` (default)
2. Rotator has sufficient size (HeightRequest/WidthRequest)
3. No overlapping UI elements blocking gestures

### Issue: SelectedIndex Not Updating

**Solution:**
- Use TwoWay binding if binding to ViewModel
- Ensure index is within valid range (0 to Count-1)
- Check if SelectedIndexChanged event is firing
