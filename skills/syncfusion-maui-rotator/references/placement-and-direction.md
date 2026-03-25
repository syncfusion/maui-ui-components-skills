# Placement and Direction Configuration

## Table of Contents
- [Overview](#overview)
- [NavigationStripPosition](#navigationstripposition)
- [NavigationDirection](#navigationdirection)
- [Combining Position and Direction](#combining-position-and-direction)
- [Complete Examples](#complete-examples)
- [Best Practices](#best-practices)

## Overview

The SfRotator provides flexible control over:
1. **Position:** Where navigation controls appear (Top, Bottom, Left, Right)
2. **Direction:** How items slide/transition (Horizontal, Vertical, and directional variants)

These properties work together to create the desired user experience.

## NavigationStripPosition

Controls where the navigation strip (dots or thumbnails) appears relative to the main content.

**Property:** `NavigationStripPosition`  
**Type:** `NavigationStripPosition` enum  
**Values:** `Top` | `Bottom` | `Left` | `Right`  
**Default:** `Bottom`

### Bottom Position (Default)

Navigation appears below the main content.

**XAML:**
```xml
<syncfusion:SfRotator ItemsSource="{Binding ImageCollection}"
                      NavigationStripMode="Dots"
                      NavigationStripPosition="Bottom"
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
using Syncfusion.Maui.Core.Rotator;

rotator.NavigationStripPosition = NavigationStripPosition.Bottom;
```

### Top Position

Navigation appears above the main content.

**XAML:**
```xml
<syncfusion:SfRotator ItemsSource="{Binding ImageCollection}"
                      NavigationStripMode="Dots"
                      NavigationStripPosition="Top"
                      BackgroundColor="#ececec"
                      HeightRequest="550"
                      WidthRequest="550">
    <syncfusion:SfRotator.ItemTemplate>
        <DataTemplate>
            <Image Source="{Binding Image}"/>
        </DataTemplate>
    </syncfusion:SfRotator.ItemTemplate>
</syncfusion:SfRotator>
```

**C#:**
```csharp
rotator.NavigationStripPosition = NavigationStripPosition.Top;
```

### Left Position

Navigation appears on the left side.

**XAML:**
```xml
<syncfusion:SfRotator ItemsSource="{Binding ImageCollection}"
                      NavigationStripMode="Thumbnail"
                      NavigationStripPosition="Left"
                      HeightRequest="500"
                      WidthRequest="600">
    <syncfusion:SfRotator.ItemTemplate>
        <DataTemplate>
            <Image Source="{Binding Image}"/>
        </DataTemplate>
    </syncfusion:SfRotator.ItemTemplate>
</syncfusion:SfRotator>
```

**C#:**
```csharp
rotator.NavigationStripPosition = NavigationStripPosition.Left;
```

### Right Position

Navigation appears on the right side.

**XAML:**
```xml
<syncfusion:SfRotator ItemsSource="{Binding ImageCollection}"
                      NavigationStripMode="Thumbnail"
                      NavigationStripPosition="Right"
                      HeightRequest="500"
                      WidthRequest="600">
    <syncfusion:SfRotator.ItemTemplate>
        <DataTemplate>
            <Image Source="{Binding Image}"/>
        </DataTemplate>
    </syncfusion:SfRotator.ItemTemplate>
</syncfusion:SfRotator>
```

**C#:**
```csharp
rotator.NavigationStripPosition = NavigationStripPosition.Right;
```

## NavigationDirection

Controls how items transition and which directions users can navigate.

**Property:** `NavigationDirection`  
**Type:** `NavigationDirection` enum  
**Values:**
- `Horizontal` - Bidirectional left/right navigation
- `Vertical` - Bidirectional up/down navigation
- `LeftToRight` - Unidirectional, left to right only
- `RightToLeft` - Unidirectional, right to left only
- `TopToBottom` - Unidirectional, top to bottom only
- `BottomToTop` - Unidirectional, bottom to top only

**Default:** `Horizontal`

### Horizontal Direction

Items slide left and right. Users can swipe in both directions.

**XAML:**
```xml
<syncfusion:SfRotator ItemsSource="{Binding ImageCollection}"
                      NavigationDirection="Horizontal"
                      NavigationStripMode="Dots"
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
using Syncfusion.Maui.Core.Rotator;

rotator.NavigationDirection = NavigationDirection.Horizontal;
```

### Vertical Direction

Items slide up and down. Users can swipe in both directions.

**XAML:**
```xml
<syncfusion:SfRotator ItemsSource="{Binding ImageCollection}"
                      NavigationDirection="Vertical"
                      NavigationStripMode="Dots"
                      NavigationStripPosition="Right"
                      HeightRequest="500">
    <syncfusion:SfRotator.ItemTemplate>
        <DataTemplate>
            <Image Source="{Binding Image}"/>
        </DataTemplate>
    </syncfusion:SfRotator.ItemTemplate>
</syncfusion:SfRotator>
```

**C#:**
```csharp
rotator.NavigationDirection = NavigationDirection.Vertical;
```

### LeftToRight Direction

Items transition only from left to right (unidirectional).

**XAML:**
```xml
<syncfusion:SfRotator ItemsSource="{Binding ImageCollection}"
                      NavigationDirection="LeftToRight"
                      NavigationStripMode="Thumbnail"
                      EnableAutoPlay="True"
                      NavigationDelay="3000"
                      EnableLooping="True"
                      HeightRequest="450">
    <syncfusion:SfRotator.ItemTemplate>
        <DataTemplate>
            <Image Source="{Binding Image}"/>
        </DataTemplate>
    </syncfusion:SfRotator.ItemTemplate>
</syncfusion:SfRotator>
```

**C#:**
```csharp
rotator.NavigationDirection = NavigationDirection.LeftToRight;
```

### RightToLeft Direction

Items transition only from right to left (unidirectional).

**XAML:**
```xml
<syncfusion:SfRotator ItemsSource="{Binding ImageCollection}"
                      NavigationDirection="RightToLeft"
                      EnableAutoPlay="True"
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
rotator.NavigationDirection = NavigationDirection.RightToLeft;
```

### TopToBottom Direction

Items transition only from top to bottom (unidirectional).

**XAML:**
```xml
<syncfusion:SfRotator ItemsSource="{Binding ImageCollection}"
                      NavigationDirection="TopToBottom"
                      NavigationStripMode="Dots"
                      NavigationStripPosition="Right"
                      EnableAutoPlay="True"
                      NavigationDelay="2500"
                      HeightRequest="500">
    <syncfusion:SfRotator.ItemTemplate>
        <DataTemplate>
            <Image Source="{Binding Image}"/>
        </DataTemplate>
    </syncfusion:SfRotator.ItemTemplate>
</syncfusion:SfRotator>
```

**C#:**
```csharp
rotator.NavigationDirection = NavigationDirection.TopToBottom;
```

### BottomToTop Direction

Items transition only from bottom to top (unidirectional).

**Complete Example:**

**XAML:**
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
                              NavigationDirection="BottomToTop"
                              NavigationDelay="2000"
                              SelectedIndex="2"
                              NavigationStripMode="Thumbnail"
                              BackgroundColor="#ececec"
                              EnableAutoPlay="True"
                              EnableLooping="True"
                              NavigationStripPosition="Bottom"
                              HeightRequest="550">
            <syncfusion:SfRotator.ItemTemplate>
                <DataTemplate>
                    <Image Source="{Binding Image}"/>
                </DataTemplate>
            </syncfusion:SfRotator.ItemTemplate>
        </syncfusion:SfRotator>
    </ContentPage.Content>
</ContentPage>
```

**C#:**
```csharp
using Syncfusion.Maui.Core.Rotator;
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
rotator.NavigationStripMode = NavigationStripMode.Thumbnail;
rotator.NavigationDirection = NavigationDirection.BottomToTop;
rotator.EnableAutoPlay = true;
rotator.NavigationDelay = 2000;
rotator.EnableLooping = true;
rotator.ItemsSource = imageCollection;
```

## Combining Position and Direction

Strategic combinations create different user experiences.

### Pattern 1: Horizontal Carousel with Bottom Navigation

Classic image gallery layout.

```xml
<syncfusion:SfRotator ItemsSource="{Binding ImageCollection}"
                      NavigationDirection="Horizontal"
                      NavigationStripPosition="Bottom"
                      NavigationStripMode="Dots"
                      HeightRequest="400">
    <syncfusion:SfRotator.ItemTemplate>
        <DataTemplate>
            <Image Source="{Binding Image}" Aspect="AspectFill"/>
        </DataTemplate>
    </syncfusion:SfRotator.ItemTemplate>
</syncfusion:SfRotator>
```

**Use Case:** Product galleries, photo albums, landing page banners

### Pattern 2: Vertical Story with Side Navigation

Stories or timeline navigation.

```xml
<syncfusion:SfRotator ItemsSource="{Binding Stories}"
                      NavigationDirection="Vertical"
                      NavigationStripPosition="Right"
                      NavigationStripMode="Dots"
                      EnableSwiping="True"
                      HeightRequest="600">
    <syncfusion:SfRotator.ItemTemplate>
        <DataTemplate>
            <Grid>
                <Image Source="{Binding Image}"/>
                <Label Text="{Binding Title}" 
                       VerticalOptions="End"
                       BackgroundColor="#80000000"
                       TextColor="White"
                       Padding="20"/>
            </Grid>
        </DataTemplate>
    </syncfusion:SfRotator.ItemTemplate>
</syncfusion:SfRotator>
```

**Use Case:** News feeds, social media stories, scrollable content

### Pattern 3: Auto-Advancing Horizontal with Thumbnails

Product showcase or portfolio.

```xml
<syncfusion:SfRotator ItemsSource="{Binding Products}"
                      NavigationDirection="LeftToRight"
                      NavigationStripPosition="Bottom"
                      NavigationStripMode="Thumbnail"
                      EnableAutoPlay="True"
                      NavigationDelay="4000"
                      EnableLooping="True"
                      SelectedThumbnailStroke="Blue"
                      UnselectedThumbnailStroke="Gray"
                      HeightRequest="500">
    <syncfusion:SfRotator.ItemTemplate>
        <DataTemplate>
            <Image Source="{Binding ImageUrl}"/>
        </DataTemplate>
    </syncfusion:SfRotator.ItemTemplate>
</syncfusion:SfRotator>
```

**Use Case:** Promotional banners, featured products, portfolio showcase

### Pattern 4: Vertical Auto-Scroll with Top Dots

News ticker or announcement rotator.

```xml
<syncfusion:SfRotator ItemsSource="{Binding Announcements}"
                      NavigationDirection="TopToBottom"
                      NavigationStripPosition="Top"
                      NavigationStripMode="Dots"
                      DotPlacement="Outside"
                      EnableAutoPlay="True"
                      NavigationDelay="5000"
                      EnableLooping="True"
                      EnableSwiping="False"
                      HeightRequest="300">
    <syncfusion:SfRotator.ItemTemplate>
        <DataTemplate>
            <Label Text="{Binding Message}"
                   FontSize="18"
                   HorizontalOptions="Center"
                   VerticalOptions="Center"
                   Padding="20"/>
        </DataTemplate>
    </syncfusion:SfRotator.ItemTemplate>
</syncfusion:SfRotator>
```

**Use Case:** Announcements, notifications, ticker messages

## Complete Examples

### Example 1: Full-Screen Horizontal Gallery

```xml
<syncfusion:SfRotator ItemsSource="{Binding GalleryImages}"
                      NavigationDirection="Horizontal"
                      NavigationStripPosition="Bottom"
                      NavigationStripMode="Thumbnail"
                      SelectedThumbnailStroke="#2196F3"
                      UnselectedThumbnailStroke="#9E9E9E"
                      NavigationButtonIconColor="White"
                      NavigationButtonBackgroundColor="#80000000"
                      BackgroundColor="Black"
                      VerticalOptions="FillAndExpand"
                      HorizontalOptions="FillAndExpand">
    <syncfusion:SfRotator.ItemTemplate>
        <DataTemplate>
            <Image Source="{Binding Image}" 
                   Aspect="AspectFit"
                   BackgroundColor="Black"/>
        </DataTemplate>
    </syncfusion:SfRotator.ItemTemplate>
</syncfusion:SfRotator>
```

### Example 2: Vertical Timeline with Left Navigation

```csharp
var rotator = new SfRotator
{
    ItemsSource = timelineEvents,
    NavigationDirection = NavigationDirection.Vertical,
    NavigationStripPosition = NavigationStripPosition.Left,
    NavigationStripMode = NavigationStripMode.Dots,
    DotsStroke = Colors.Gray,
    SelectedDotColor = Colors.Green,
    UnselectedDotColor = Colors.LightGray,
    EnableSwiping = true,
    HeightRequest = 700
};
```

### Example 3: Auto-Advancing Diagonal Slideshow

```xml
<syncfusion:SfRotator ItemsSource="{Binding SlideImages}"
                      NavigationDirection="LeftToRight"
                      NavigationStripPosition="Bottom"
                      NavigationStripMode="Dots"
                      DotPlacement="Outside"
                      DotsStroke="White"
                      SelectedDotColor="White"
                      UnselectedDotColor="#80FFFFFF"
                      EnableAutoPlay="True"
                      NavigationDelay="3000"
                      EnableLooping="True"
                      EnableSwiping="False"
                      BackgroundColor="#1A1A1A"
                      HeightRequest="450">
    <syncfusion:SfRotator.ItemTemplate>
        <DataTemplate>
            <Grid>
                <Image Source="{Binding Image}" Aspect="AspectFill"/>
                <BoxView Color="#40000000" VerticalOptions="End" HeightRequest="100"/>
                <Label Text="{Binding Caption}"
                       TextColor="White"
                       FontSize="24"
                       FontAttributes="Bold"
                       VerticalOptions="End"
                       Margin="20,0,20,30"/>
            </Grid>
        </DataTemplate>
    </syncfusion:SfRotator.ItemTemplate>
</syncfusion:SfRotator>
```

## Best Practices

### 1. Match Position to Direction

**Horizontal Direction:**
- Use Bottom or Top position (most intuitive)
- Left/Right positions work but may feel unusual

**Vertical Direction:**
- Use Left or Right position (most intuitive)
- Top/Bottom positions work but may overlap content

### 2. Consider Use Case

**Galleries/Photos:**
```csharp
NavigationDirection = NavigationDirection.Horizontal
NavigationStripPosition = NavigationStripPosition.Bottom
NavigationStripMode = NavigationStripMode.Thumbnail
```

**Stories/Feeds:**
```csharp
NavigationDirection = NavigationDirection.Vertical
NavigationStripPosition = NavigationStripPosition.Right
NavigationStripMode = NavigationStripMode.Dots
```

**Banners/Promos:**
```csharp
NavigationDirection = NavigationDirection.LeftToRight
EnableAutoPlay = true
NavigationStripMode = NavigationStripMode.Dots
```

### 3. Unidirectional for Auto-Advance

Use unidirectional navigation (LeftToRight, TopToBottom, etc.) when EnableAutoPlay is true:

```xml
<syncfusion:SfRotator NavigationDirection="LeftToRight"
                      EnableAutoPlay="True"
                      EnableLooping="True">
    <!-- Content -->
</syncfusion:SfRotator>
```

### 4. Mobile vs Desktop

**Mobile (Portrait):**
- Prefer Horizontal direction
- Use Bottom position
- Dots work better than thumbnails

**Tablet/Desktop (Landscape):**
- Can use Vertical or Horizontal
- Thumbnails work well
- Side positions (Left/Right) are viable

### 5. Accessibility Considerations

- Ensure navigation controls are easily reachable
- Provide sufficient touch target sizes
- Consider swipe gesture alternatives (navigation buttons)

## Layout Recommendations

### Portrait Orientation
```
┌─────────────────┐
│                 │
│     Content     │ ← Main rotator content
│                 │
├─────────────────┤
│ ● ● ● ● ●       │ ← Bottom position works best
└─────────────────┘
```

## Common Issues

### Issue: Navigation Strip Covers Content

**Solution:**
- Use `DotPlacement="Outside"` for dots
- Adjust HeightRequest to accommodate navigation
- Consider different NavigationStripPosition

### Issue: Unidirectional Not Working

**Check:**
- Verify NavigationDirection is set to LeftToRight, RightToLeft, TopToBottom, or BottomToTop
- Ensure EnableLooping is true for continuous navigation
- Check if EnableAutoPlay is needed for the desired behavior

### Issue: Layout Feels Awkward

**Review:**
- Match direction to position (horizontal with top/bottom, vertical with left/right)
- Test on target devices
- Consider user expectations based on similar apps
