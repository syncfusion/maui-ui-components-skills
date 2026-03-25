# Navigation Modes and Styling

## Table of Contents
- [Overview](#overview)
- [NavigationStripMode Property](#navigationstripmode-property)
- [Thumbnail Mode](#thumbnail-mode)
- [Dots Mode](#dots-mode)
- [Dots Customization](#dots-customization)
- [Thumbnail Customization](#thumbnail-customization)
- [Navigation Buttons](#navigation-buttons)
- [Dot Placement](#dot-placement)
- [Complete Examples](#complete-examples)

## Overview

The SfRotator provides two navigation modes to help users browse through items:

1. **Thumbnail**: Displays miniature previews of all images
2. **Dots**: Shows indicator dots for each item

The navigation appearance is controlled by the `NavigationStripMode` property.

## NavigationStripMode Property

**Property:** `NavigationStripMode`  
**Type:** `NavigationStripMode` enum  
**Values:** `Thumbnail` | `Dots`  
**Default:** `Dots`

### Setting Navigation Mode in XAML

```xml
<syncfusion:SfRotator NavigationStripMode="Thumbnail"
                      ItemsSource="{Binding ImageCollection}">
    <!-- Content -->
</syncfusion:SfRotator>
```

### Setting Navigation Mode in C#

```csharp
using Syncfusion.Maui.Core.Rotator;

rotator.NavigationStripMode = NavigationStripMode.Thumbnail;
// or
rotator.NavigationStripMode = NavigationStripMode.Dots;
```

## Thumbnail Mode

Thumbnail mode displays small preview images of all rotator items, allowing quick visual navigation.

### Basic Thumbnail Implementation

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
                              NavigationStripMode="Thumbnail"
                              NavigationStripPosition="Bottom"
                              NavigationDelay="2000"
                              SelectedIndex="2"
                              NavigationDirection="Horizontal"
                              BackgroundColor="#ececec"
                              WidthRequest="550"
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
rotator.ItemsSource = imageCollection;
rotator.HeightRequest = 500;
```

### Thumbnail Features

- **Visual Preview:** Users see thumbnails of all images
- **Quick Navigation:** Click any thumbnail to jump to that item
- **Automatic Navigation Buttons:** Previous/Next arrows appear automatically
- **Customizable Borders:** Style selected and unselected thumbnails

## Dots Mode

Dots mode displays simple indicator dots for each item, providing a clean, minimalist navigation style.

### Basic Dots Implementation

**XAML:**
```xml
<syncfusion:SfRotator ItemsSource="{Binding ImageCollection}"
                      NavigationStripMode="Dots"
                      NavigationStripPosition="Bottom"
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
rotator.NavigationStripMode = NavigationStripMode.Dots;
rotator.NavigationStripPosition = NavigationStripPosition.Bottom;
```

### Dots Features

- **Minimalist Design:** Small, unobtrusive indicators
- **Current Position:** Highlighted dot shows active item
- **Tap to Navigate:** Click dots to jump to specific items
- **Fully Customizable:** Control colors for selected, unselected, and stroke

## Dots Customization

Customize dot appearance using three properties:

### DotsStroke Property

Controls the border/outline color of dots.

**Property:** `DotsStroke`  
**Type:** `Color`  
**Default:** System default

**XAML:**
```xml
<syncfusion:SfRotator ItemsSource="{Binding ImageCollection}"
                      NavigationStripMode="Dots"
                      NavigationStripPosition="Bottom"
                      DotsStroke="Aqua"
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
rotator.DotsStroke = Colors.Aqua;
```

### SelectedDotColor Property

Sets the fill color of the currently selected dot.

**Property:** `SelectedDotColor`  
**Type:** `Color`  
**Default:** System accent color

**XAML:**
```xml
<syncfusion:SfRotator DotsStroke="Aqua"
                      SelectedDotColor="Blue"
                      NavigationStripMode="Dots">
    <!-- Content -->
</syncfusion:SfRotator>
```

**C#:**
```csharp
rotator.DotsStroke = Colors.Aqua;
rotator.SelectedDotColor = Colors.Blue;
```

### UnselectedDotColor Property

Sets the fill color of inactive dots.

**Property:** `UnselectedDotColor`  
**Type:** `Color`  
**Default:** Gray

**Complete Dots Customization Example:**

**XAML:**
```xml
<syncfusion:SfRotator ItemsSource="{Binding ImageCollection}"
                      NavigationStripMode="Dots"
                      NavigationStripPosition="Bottom"
                      DotsStroke="DarkBlue"
                      SelectedDotColor="Blue"
                      UnselectedDotColor="LightGray"
                      BackgroundColor="#ececec"
                      WidthRequest="550"
                      HeightRequest="550">
    <syncfusion:SfRotator.ItemTemplate>
        <DataTemplate>
            <Image Source="{Binding Image}"/>
        </DataTemplate>
    </syncfusion:SfRotator.ItemTemplate>
</syncfusion:SfRotator>
```

**C#:**
```csharp
rotator.NavigationStripMode = NavigationStripMode.Dots;
rotator.DotsStroke = Colors.DarkBlue;
rotator.SelectedDotColor = Colors.Blue;
rotator.UnselectedDotColor = Colors.LightGray;
```

## Thumbnail Customization

Customize thumbnail borders to highlight selected and unselected items.

### SelectedThumbnailStroke Property

Sets the border color of the currently selected thumbnail.

**Property:** `SelectedThumbnailStroke`  
**Type:** `Color`  
**Default:** System accent color

**XAML:**
```xml
<syncfusion:SfRotator ItemsSource="{Binding ImageCollection}"
                      NavigationStripMode="Thumbnail"
                      NavigationStripPosition="Bottom"
                      SelectedThumbnailStroke="Green"
                      BackgroundColor="#ececec"
                      WidthRequest="550"
                      HeightRequest="550">
    <syncfusion:SfRotator.ItemTemplate>
        <DataTemplate>
            <Image Source="{Binding Image}"/>
        </DataTemplate>
    </syncfusion:SfRotator.ItemTemplate>
</syncfusion:SfRotator>
```

**C#:**
```csharp
rotator.NavigationStripMode = NavigationStripMode.Thumbnail;
rotator.SelectedThumbnailStroke = Colors.Green;
```

### UnselectedThumbnailStroke Property

Sets the border color of inactive thumbnails.

**Property:** `UnselectedThumbnailStroke`  
**Type:** `Color`  
**Default:** Gray

**Complete Thumbnail Customization Example:**

**XAML:**
```xml
<syncfusion:SfRotator ItemsSource="{Binding ImageCollection}"
                      NavigationStripMode="Thumbnail"
                      NavigationStripPosition="Bottom"
                      SelectedThumbnailStroke="Green"
                      UnselectedThumbnailStroke="Red"
                      BackgroundColor="#ececec"
                      WidthRequest="550"
                      HeightRequest="550">
    <syncfusion:SfRotator.ItemTemplate>
        <DataTemplate>
            <Image Source="{Binding Image}"/>
        </DataTemplate>
    </syncfusion:SfRotator.ItemTemplate>
</syncfusion:SfRotator>
```

**C#:**
```csharp
rotator.NavigationStripMode = NavigationStripMode.Thumbnail;
rotator.SelectedThumbnailStroke = Colors.Green;
rotator.UnselectedThumbnailStroke = Colors.Red;
```

## Navigation Buttons

Navigation buttons (Previous/Next arrows) appear automatically in Thumbnail mode. Customize their appearance or hide them.

### NavigationButtonIconColor Property

Sets the color of the arrow icons.

**Property:** `NavigationButtonIconColor`  
**Type:** `Color`  
**Default:** System default

**XAML:**
```xml
<syncfusion:SfRotator NavigationStripMode="Thumbnail"
                      NavigationButtonIconColor="Blue"
                      ItemsSource="{Binding ImageCollection}">
    <!-- Content -->
</syncfusion:SfRotator>
```

**C#:**
```csharp
rotator.NavigationStripMode = NavigationStripMode.Thumbnail;
rotator.NavigationButtonIconColor = Colors.Blue;
```

### NavigationButtonBackgroundColor Property

Sets the background color of navigation buttons.

**Property:** `NavigationButtonBackgroundColor`  
**Type:** `Color`  
**Default:** Semi-transparent

**XAML:**
```xml
<syncfusion:SfRotator NavigationStripMode="Thumbnail"
                      NavigationButtonBackgroundColor="Pink"
                      NavigationButtonIconColor="Blue"
                      ItemsSource="{Binding ImageCollection}">
    <!-- Content -->
</syncfusion:SfRotator>
```

**C#:**
```csharp
rotator.NavigationButtonBackgroundColor = Colors.Pink;
rotator.NavigationButtonIconColor = Colors.Blue;
```

### ShowNavigationButton Property

Show or hide navigation buttons.

**Property:** `ShowNavigationButton`  
**Type:** `bool`  
**Default:** `true`

**XAML:**
```xml
<syncfusion:SfRotator NavigationStripMode="Thumbnail"
                      ShowNavigationButton="False"
                      ItemsSource="{Binding ImageCollection}">
    <!-- Content -->
</syncfusion:SfRotator>
```

**C#:**
```csharp
rotator.NavigationStripMode = NavigationStripMode.Thumbnail;
rotator.ShowNavigationButton = false;
```

## Dot Placement

Control where dots appear relative to the main content.

**Property:** `DotPlacement`  
**Type:** `DotsPlacement` enum  
**Values:** `Default` | `None` | `Outside`  
**Default:** `Default`

### Default Placement

Dots appear inside the rotator area (default behavior).

```xml
<syncfusion:SfRotator DotPlacement="Default">
    <!-- Content -->
</syncfusion:SfRotator>
```

### None Placement

Hides dots completely (clean appearance).

```xml
<syncfusion:SfRotator DotPlacement="None"
                      NavigationStripMode="Dots">
    <!-- Content -->
</syncfusion:SfRotator>
```

### Outside Placement

Places dots outside the rotator area.

```xml
<syncfusion:SfRotator ItemsSource="{Binding ImageCollection}"
                      NavigationStripMode="Dots"
                      NavigationStripPosition="Bottom"
                      DotPlacement="Outside"
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
using Syncfusion.Maui.Core.Rotator;

rotator.DotPlacement = DotsPlacement.OutSide;
```

## Complete Examples

### Example 1: Fully Customized Dots Mode

```xml
<syncfusion:SfRotator ItemsSource="{Binding ImageCollection}"
                      NavigationStripMode="Dots"
                      NavigationStripPosition="Bottom"
                      DotPlacement="Outside"
                      DotsStroke="#1976D2"
                      SelectedDotColor="#2196F3"
                      UnselectedDotColor="#BDBDBD"
                      BackgroundColor="White"
                      EnableAutoPlay="True"
                      NavigationDelay="3000"
                      EnableLooping="True"
                      HeightRequest="450">
    <syncfusion:SfRotator.ItemTemplate>
        <DataTemplate>
            <Image Source="{Binding Image}" Aspect="AspectFill"/>
        </DataTemplate>
    </syncfusion:SfRotator.ItemTemplate>
</syncfusion:SfRotator>
```

### Example 2: Fully Customized Thumbnail Mode

```xml
<syncfusion:SfRotator ItemsSource="{Binding ImageCollection}"
                      NavigationStripMode="Thumbnail"
                      NavigationStripPosition="Bottom"
                      SelectedThumbnailStroke="#4CAF50"
                      UnselectedThumbnailStroke="#E0E0E0"
                      NavigationButtonIconColor="White"
                      NavigationButtonBackgroundColor="#00796B"
                      BackgroundColor="#FAFAFA"
                      HeightRequest="500"
                      WidthRequest="600">
    <syncfusion:SfRotator.ItemTemplate>
        <DataTemplate>
            <Image Source="{Binding Image}" Aspect="AspectFit"/>
        </DataTemplate>
    </syncfusion:SfRotator.ItemTemplate>
</syncfusion:SfRotator>
```

### Example 3: Clean Mode (No Navigation UI)

```csharp
var rotator = new SfRotator
{
    ItemsSource = imageCollection,
    NavigationStripMode = NavigationStripMode.Dots,
    DotPlacement = DotsPlacement.None,
    ShowNavigationButton = false,
    EnableSwiping = true,
    HeightRequest = 400
};
```

## Best Practices

1. **Choose Mode Based on Content:**
   - Use Thumbnail for image-heavy content where visual preview helps
   - Use Dots for cleaner, less cluttered UI with fewer items

2. **Color Contrast:**
   - Ensure sufficient contrast between dots and background
   - Test selected vs unselected colors for visibility

3. **Mobile Considerations:**
   - Dots work better on smaller screens
   - Thumbnails require more space and may clutter mobile UI

4. **Accessibility:**
   - Provide sufficient size and contrast for navigation elements
   - Consider color-blind users when choosing colors

5. **Performance:**
   - Thumbnail mode loads all image previews upfront
   - Consider performance impact with many items (20+)

## Common Issues

### Issue: Dots/Thumbnails Not Visible

**Check:**
1. `NavigationStripMode` is set correctly
2. `DotPlacement` is not set to `None`
3. Sufficient contrast between navigation colors and background
4. ItemsSource has multiple items (navigation shows only with 2+ items)

### Issue: Navigation Buttons Not Showing

**Solution:**
- Navigation buttons appear only in Thumbnail mode
- Verify `ShowNavigationButton` is `true`
- Ensure there are multiple items to navigate

### Issue: Customization Not Applied

**Check:**
- Properties match the current NavigationStripMode (e.g., SelectedDotColor only works in Dots mode)
- Colors are valid and properly formatted
- XAML namespace is correct
