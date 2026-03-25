# Customization in .NET MAUI Toolbar

Learn how to customize toolbar appearance with text styles, colors, separators, navigation buttons, and visual configurations.

## Table of Contents
- [Overview](#overview)
- [Toolbar Item Customization](#toolbar-item-customization)
- [Text Styling](#text-styling)
- [Selection Highlight Color](#selection-highlight-color)
- [Separator Styling](#separator-styling)
- [Navigation Button Customization](#navigation-button-customization)
- [Navigation Button Templates](#navigation-button-templates)
- [More Button Customization](#more-button-customization)
- [Divider Line Customization](#divider-line-customization)
- [Corner Radius](#corner-radius)
- [Selection Corner Radius](#selection-corner-radius)
- [Complete Customization Example](#complete-customization-example)
- [Best Practices](#best-practices)

## Overview

The SfToolbar provides extensive customization options through:
- **Text Styling** - Font, size, color, and attributes for item text using `ToolbarTextStyle`
- **Item Properties** - `IsEnabled`, icon colors via `FontImageSource.Color`, and `SelectionHighlightColor`
- **Separator Styling** - `Stroke` color and `StrokeThickness` for `SeparatorToolbarItem`
- **Button Colors** - Icon and background colors for overflow buttons (`ForwardButtonIconColor`, `BackwardButtonIconColor`, `MoreButtonIconColor`, etc.)
- **Corner Radius** - Rounded corners for toolbar (`CornerRadius`) and selection (`SelectionCornerRadius`)
- **Templates** - Custom `DataTemplate` views for navigation buttons (`ForwardButtonTemplate`, `BackwardButtonTemplate`)

## Toolbar Item Customization

Customize individual toolbar items using properties like `IsEnabled`, `TextStyle`, icon `Color`, and `SelectionHighlightColor`.

### Item Enabled State

Control whether toolbar items are enabled or disabled.

**XAML:**
```xaml
<toolbar:SfToolbar HeightRequest="56">
    <toolbar:SfToolbar.Items>
        <toolbar:SfToolbarItem Name="Bold" Text="Bold" ToolTipText="Bold">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE770;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        
        <toolbar:SfToolbarItem Name="Underline" 
                               Text="Underline" 
                               IsEnabled="False"
                               ToolTipText="Underline (Disabled)">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE762;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
    </toolbar:SfToolbar.Items>
</toolbar:SfToolbar>
```

**C#:**
```csharp
SfToolbar toolbar = new SfToolbar { HeightRequest = 56 };

ObservableCollection<BaseToolbarItem> items = new ObservableCollection<BaseToolbarItem>
{
    new SfToolbarItem
    {
        Name = "Bold",
        Text = "Bold",
        ToolTipText = "Bold",
        Icon = new FontImageSource { Glyph = "\uE770", FontFamily = "MauiMaterialAssets" }
    },
    new SfToolbarItem
    {
        Name = "Underline",
        Text = "Underline",
        IsEnabled = false,
        ToolTipText = "Underline (Disabled)",
        Icon = new FontImageSource { Glyph = "\uE762", FontFamily = "MauiMaterialAssets" }
    }
};

toolbar.Items = items;
```

### Icon Color Customization

Set icon color using the `Color` property of `FontImageSource`.

**XAML:**
```xaml
<toolbar:SfToolbar HeightRequest="56">
    <toolbar:SfToolbar.Items>
        <toolbar:SfToolbarItem Name="AlignLeft" Text="Align Left" TextPosition="Right">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE751;" 
                                 Color="Blue" 
                                 FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        
        <toolbar:SfToolbarItem Name="AlignRight" Text="Align Right" TextPosition="Right">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE753;" 
                                 Color="Red" 
                                 FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        
        <toolbar:SfToolbarItem Name="AlignCenter" Text="Align Center" TextPosition="Right">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE752;" 
                                 Color="Green" 
                                 FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
    </toolbar:SfToolbar.Items>
</toolbar:SfToolbar>
```

**C#:**
```csharp
new SfToolbarItem
{
    Name = "AlignLeft",
    Text = "Align Left",
    TextPosition = ToolbarItemTextPosition.Right,
    Icon = new FontImageSource 
    { 
        Glyph = "\uE751", 
        Color = Colors.Blue,
        FontFamily = "MauiMaterialAssets" 
    }
}
```

## Text Styling

Customize text appearance using the `TextStyle` property with `ToolbarTextStyle`.

### Basic Text Styling

**XAML:**
```xaml
<toolbar:SfToolbar HeightRequest="56">
    <toolbar:SfToolbar.Items>
        <toolbar:SfToolbarItem Name="Bold" 
                               Text="Bold" 
                               TextPosition="Right"
                               Size="60,56">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE770;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
            <toolbar:SfToolbarItem.TextStyle>
                <toolbar:ToolbarTextStyle TextColor="Red"
                                          FontSize="14"
                                          FontAttributes="Bold"
                                          FontFamily="OpenSansSemibold"
                                          FontAutoScalingEnabled="True" />
            </toolbar:SfToolbarItem.TextStyle>
        </toolbar:SfToolbarItem>
    </toolbar:SfToolbar.Items>
</toolbar:SfToolbar>
```

**C#:**
```csharp
new SfToolbarItem
{
    Name = "Bold",
    Text = "Bold",
    TextPosition = ToolbarItemTextPosition.Right,
    Size = new Size(60, 56),
    Icon = new FontImageSource { Glyph = "\uE770", FontFamily = "MauiMaterialAssets" },
    TextStyle = new ToolbarTextStyle
    {
        TextColor = Colors.Red,
        FontSize = 14,
        FontAttributes = FontAttributes.Bold,
        FontFamily = "OpenSansSemibold",
        FontAutoScalingEnabled = true
    }
}
```

### ToolbarTextStyle Properties

- **TextColor** - Color of the text
- **FontSize** - Size of the font (double value)
- **FontAttributes** - `FontAttributes.Bold`, `FontAttributes.Italic`, or `FontAttributes.None`
- **FontFamily** - Custom font family name
- **FontAutoScalingEnabled** - Enable/disable auto-scaling for accessibility (bool)

### Multiple Text Styles Example

**XAML:**
```xaml
<toolbar:SfToolbar HeightRequest="56" WidthRequest="850">
    <toolbar:SfToolbar.Items>
        <toolbar:SfToolbarItem Name="Bold" Text="Bold" TextPosition="Right" Size="90,56">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE770;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
            <toolbar:SfToolbarItem.TextStyle>
                <toolbar:ToolbarTextStyle TextColor="Red"
                                          FontSize="14"
                                          FontAttributes="Bold" />
            </toolbar:SfToolbarItem.TextStyle>
        </toolbar:SfToolbarItem>
        
        <toolbar:SfToolbarItem Name="Italic" Text="Italic" TextPosition="Right" Size="90,56">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE771;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
            <toolbar:SfToolbarItem.TextStyle>
                <toolbar:ToolbarTextStyle TextColor="Green"
                                          FontSize="16"
                                          FontAttributes="Italic"
                                          FontFamily="OpenSansRegular"
                                          FontAutoScalingEnabled="False" />
            </toolbar:SfToolbarItem.TextStyle>
        </toolbar:SfToolbarItem>
    </toolbar:SfToolbar.Items>
</toolbar:SfToolbar>
```

## Selection Highlight Color

Set a custom highlight color for selected toolbar items using `SelectionHighlightColor`.

### Basic Selection Highlight

**XAML:**
```xaml
<toolbar:SfToolbar HeightRequest="56">
    <toolbar:SfToolbar.Items>
        <toolbar:SfToolbarItem Name="Bold" 
                               Text="Bold"
                               SelectionHighlightColor="LightGreen">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE770;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        
        <toolbar:SfToolbarItem Name="Underline" 
                               Text="Underline"
                               SelectionHighlightColor="LightBlue">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE762;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
    </toolbar:SfToolbar.Items>
</toolbar:SfToolbar>
```

**C#:**
```csharp
new SfToolbarItem
{
    Name = "Bold",
    Text = "Bold",
    SelectionHighlightColor = Colors.LightGreen,
    Icon = new FontImageSource { Glyph = "\uE770", FontFamily = "MauiMaterialAssets" }
}
```

### Multiple Selection Highlights

**XAML:**
```xaml
<toolbar:SfToolbar HeightRequest="56" SelectionMode="Multiple">
    <toolbar:SfToolbar.Items>
        <toolbar:SfToolbarItem Name="Bold" 
                               Text="Bold"
                               SelectionHighlightColor="LightGreen">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE770;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        
        <toolbar:SfToolbarItem Name="Underline" 
                               Text="Underline"
                               SelectionHighlightColor="LightBlue">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE762;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        
        <toolbar:SfToolbarItem Name="Italic" 
                               Text="Italic"
                               SelectionHighlightColor="Violet">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE771;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
    </toolbar:SfToolbar.Items>
</toolbar:SfToolbar>
```

## Separator Styling

Customize separator appearance using `Stroke` and `StrokeThickness` properties of `SeparatorToolbarItem`.

### Basic Separator Styling

**XAML:**
```xaml
<toolbar:SfToolbar HeightRequest="56">
    <toolbar:SfToolbar.Items>
        <toolbar:SfToolbarItem Name="Bold" ToolTipText="Bold">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE770;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        
        <toolbar:SeparatorToolbarItem Stroke="Red" StrokeThickness="3" />
        
        <toolbar:SfToolbarItem Name="Italic" ToolTipText="Italic">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE771;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
    </toolbar:SfToolbar.Items>
</toolbar:SfToolbar>
```

**C#:**
```csharp
ObservableCollection<BaseToolbarItem> items = new ObservableCollection<BaseToolbarItem>
{
    new SfToolbarItem
    {
        Name = "Bold",
        ToolTipText = "Bold",
        Icon = new FontImageSource { Glyph = "\uE770", FontFamily = "MauiMaterialAssets" }
    },
    new SeparatorToolbarItem
    {
        Stroke = Colors.Red,
        StrokeThickness = 3
    },
    new SfToolbarItem
    {
        Name = "Italic",
        ToolTipText = "Italic",
        Icon = new FontImageSource { Glyph = "\uE771", FontFamily = "MauiMaterialAssets" }
    }
};
```

### Separator Properties

- **Stroke** - Color of the separator line
- **StrokeThickness** - Width of the separator line (double value)

### Multiple Separators with Different Styles

**XAML:**
```xaml
<toolbar:SfToolbar HeightRequest="56">
    <toolbar:SfToolbar.Items>
        <toolbar:SfToolbarItem Name="Cut" ToolTipText="Cut">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE719;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        
        <toolbar:SeparatorToolbarItem Stroke="Gray" StrokeThickness="1" />
        
        <toolbar:SfToolbarItem Name="Copy" ToolTipText="Copy">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE710;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        
        <toolbar:SeparatorToolbarItem Stroke="Blue" StrokeThickness="2" />
        
        <toolbar:SfToolbarItem Name="Paste" ToolTipText="Paste">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE711;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
    </toolbar:SfToolbar.Items>
</toolbar:SfToolbar>
```

## Navigation Button Customization

Customize navigation buttons (forward/backward) colors when using navigation overflow mode.

### Navigation Button Colors

Customize icon and background colors for forward and backward navigation buttons.

**XAML:**
```xaml
<toolbar:SfToolbar HeightRequest="56" 
                   WidthRequest="270"
                   OverflowMode="NavigationButtons"
                   ForwardButtonIconColor="Red"
                   BackwardButtonIconColor="Red"
                   ForwardButtonBackground="Aqua"
                   BackwardButtonBackground="Aqua">
    <toolbar:SfToolbar.Items>
        <toolbar:SfToolbarItem Name="Bold" ToolTipText="Bold">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE770;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        <toolbar:SfToolbarItem Name="Underline" ToolTipText="Underline">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE762;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        <toolbar:SfToolbarItem Name="Italic" ToolTipText="Italic">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE771;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        <toolbar:SfToolbarItem Name="AlignLeft" ToolTipText="Align-Left">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE751;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        <toolbar:SfToolbarItem Name="AlignRight" ToolTipText="Align-Right">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE753;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
    </toolbar:SfToolbar.Items>
</toolbar:SfToolbar>
```

**C#:**
```csharp
SfToolbar toolbar = new SfToolbar
{
    HeightRequest = 56,
    WidthRequest = 270,
    OverflowMode = ToolbarItemOverflowMode.NavigationButtons,
    ForwardButtonIconColor = Colors.Red,
    BackwardButtonIconColor = Colors.Red,
    ForwardButtonBackground = Colors.Aqua,
    BackwardButtonBackground = Colors.Aqua
};
```

### Navigation Button Properties

- **ForwardButtonIconColor** - Icon color of the forward navigation button
- **BackwardButtonIconColor** - Icon color of the backward navigation button
- **ForwardButtonBackground** - Background color of the forward button
- **BackwardButtonBackground** - Background color of the backward button

## Navigation Button Templates

Customize navigation button appearance using `DataTemplate` for complete control over button views.

### Custom Navigation Button Templates

**XAML:**
```xaml
<toolbar:SfToolbar HeightRequest="56" 
                   WidthRequest="270"
                   OverflowMode="NavigationButtons">
    <toolbar:SfToolbar.BackwardButtonTemplate>
        <DataTemplate>
            <Grid HorizontalOptions="Start">
                <Image Source="less.png" WidthRequest="18" HeightRequest="20" />
            </Grid>
        </DataTemplate>
    </toolbar:SfToolbar.BackwardButtonTemplate>
    
    <toolbar:SfToolbar.ForwardButtonTemplate>
        <DataTemplate>
            <Grid HorizontalOptions="Start">
                <Image Source="greater.png" WidthRequest="18" HeightRequest="20" />
            </Grid>
        </DataTemplate>
    </toolbar:SfToolbar.ForwardButtonTemplate>
    
    <toolbar:SfToolbar.Items>
        <toolbar:SfToolbarItem Name="Bold" ToolTipText="Bold">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE770;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        <!-- More items -->
    </toolbar:SfToolbar.Items>
</toolbar:SfToolbar>
```

**C#:**
```csharp
toolbar.BackwardButtonTemplate = new DataTemplate(() =>
{
    var grid = new Grid { HorizontalOptions = LayoutOptions.Start };
    var image = new Image 
    { 
        Source = "less.png", 
        WidthRequest = 18, 
        HeightRequest = 20 
    };
    grid.Children.Add(image);
    return grid;
});

toolbar.ForwardButtonTemplate = new DataTemplate(() =>
{
    var grid = new Grid { HorizontalOptions = LayoutOptions.Start };
    var image = new Image 
    { 
        Source = "greater.png", 
        WidthRequest = 18, 
        HeightRequest = 20 
    };
    grid.Children.Add(image);
    return grid;
});
```

### Template Properties

- **ForwardButtonTemplate** - Custom `DataTemplate` view for forward navigation button
- **BackwardButtonTemplate** - Custom `DataTemplate` view for backward navigation button

## More Button Customization

Customize the more button (three dots/ellipsis) appearance when using more button overflow mode.

### More Button Colors

**XAML:**
```xaml
<toolbar:SfToolbar HeightRequest="56" 
                   WidthRequest="270"
                   OverflowMode="MoreButton"
                   MoreButtonIconColor="Red"
                   MoreButtonBackground="Yellow">
    <toolbar:SfToolbar.Items>
        <toolbar:SfToolbarItem Name="Bold" ToolTipText="Bold">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE770;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        <toolbar:SfToolbarItem Name="Underline" ToolTipText="Underline">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE762;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        <!-- More items to trigger overflow -->
    </toolbar:SfToolbar.Items>
</toolbar:SfToolbar>
```

**C#:**
```csharp
SfToolbar toolbar = new SfToolbar
{
    HeightRequest = 56,
    WidthRequest = 270,
    OverflowMode = ToolbarItemOverflowMode.MoreButton,
    MoreButtonIconColor = Colors.Red,
    MoreButtonBackground = Colors.Yellow
};
```

### More Button Properties

- **MoreButtonIconColor** - Icon color of the more button
- **MoreButtonBackground** - Background color of the more button

## Divider Line Customization

Customize the divider line between visible and overflow items in navigation mode.

### Divider Line Styling

**XAML:**
```xaml
<toolbar:SfToolbar HeightRequest="56"
                   WidthRequest="280"
                   OverflowMode="NavigationButtons"
                   DividedLineStroke="Red"
                   DividedLineStrokeThickness="5">
    <toolbar:SfToolbar.Items>
        <toolbar:SfToolbarItem Name="Bold" ToolTipText="Bold">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE770;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        <!-- More items -->
    </toolbar:SfToolbar.Items>
</toolbar:SfToolbar>
```

**C#:**
```csharp
SfToolbar toolbar = new SfToolbar
{
    HeightRequest = 56,
    WidthRequest = 280,
    OverflowMode = ToolbarItemOverflowMode.NavigationButtons,
    DividedLineStroke = Colors.Red,
    DividedLineStrokeThickness = 5
};
```

### Divider Line Properties

- **DividedLineStroke** - Color of the divider line
- **DividedLineStrokeThickness** - Thickness of the divider line (double value)

## Corner Radius

Customize toolbar corners using the `CornerRadius` property.

### Basic Corner Radius

**XAML:**
```xaml
<toolbar:SfToolbar HeightRequest="56" 
                   WidthRequest="330"
                   CornerRadius="30">
    <toolbar:SfToolbar.Items>
        <toolbar:SfToolbarItem Name="Bold" Text="Bold" TextPosition="Right" Size="90,40">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE770;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        <toolbar:SfToolbarItem Name="Underline" Text="Underline" TextPosition="Right" Size="90,40">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE762;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
    </toolbar:SfToolbar.Items>
</toolbar:SfToolbar>
```

**C#:**
```csharp
SfToolbar toolbar = new SfToolbar
{
    HeightRequest = 56,
    WidthRequest = 330,
    CornerRadius = 30
};
```

### Asymmetric Corner Radius

**XAML:**
```xaml
<!-- Round only top corners -->
<toolbar:SfToolbar HeightRequest="56" CornerRadius="16,16,0,0">
    <toolbar:SfToolbar.Items>
        <!-- Items -->
    </toolbar:SfToolbar.Items>
</toolbar:SfToolbar>
```

**C#:**
```csharp
// TopLeft, TopRight, BottomRight, BottomLeft
toolbar.CornerRadius = new CornerRadius(16, 16, 0, 0);
```

## Selection Corner Radius

Customize the corner radius of the selection highlight.

### Basic Selection Corner Radius

**XAML:**
```xaml
<toolbar:SfToolbar HeightRequest="56" 
                   WidthRequest="330"
                   SelectionCornerRadius="20">
    <toolbar:SfToolbar.Items>
        <toolbar:SfToolbarItem Name="Bold" Text="Bold" TextPosition="Right" Size="90,40">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE770;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        <toolbar:SfToolbarItem Name="Underline" Text="Underline" TextPosition="Right" Size="90,40">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE762;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
    </toolbar:SfToolbar.Items>
</toolbar:SfToolbar>
```

**C#:**
```csharp
SfToolbar toolbar = new SfToolbar
{
    HeightRequest = 56,
    WidthRequest = 330,
    SelectionCornerRadius = 20
};
```

## Complete Customization Example

A comprehensive example showcasing multiple customization features.

**XAML:**
```xaml
<toolbar:SfToolbar HeightRequest="56" 
                   WidthRequest="500"
                   BackgroundColor="#6200EE"
                   CornerRadius="15"
                   SelectionCornerRadius="10"
                   SelectionMode="Multiple"
                   OverflowMode="MoreButton"
                   MoreButtonIconColor="White"
                   MoreButtonBackground="#3700B3">
    <toolbar:SfToolbar.Items>
        <toolbar:SfToolbarItem Name="Bold" 
                               Text="Bold" 
                               TextPosition="Right"
                               SelectionHighlightColor="#BB86FC">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE770;" 
                                 Color="White"
                                 FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
            <toolbar:SfToolbarItem.TextStyle>
                <toolbar:ToolbarTextStyle TextColor="White"
                                          FontSize="14"
                                          FontAttributes="Bold" />
            </toolbar:SfToolbarItem.TextStyle>
        </toolbar:SfToolbarItem>
        
        <toolbar:SeparatorToolbarItem Stroke="White" StrokeThickness="2" />
        
        <toolbar:SfToolbarItem Name="Italic" 
                               Text="Italic" 
                               TextPosition="Right"
                               SelectionHighlightColor="#03DAC6">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE771;" 
                                 Color="White"
                                 FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
            <toolbar:SfToolbarItem.TextStyle>
                <toolbar:ToolbarTextStyle TextColor="White"
                                          FontSize="14" />
            </toolbar:SfToolbarItem.TextStyle>
        </toolbar:SfToolbarItem>
    </toolbar:SfToolbar.Items>
</toolbar:SfToolbar>
```

## Best Practices

### 1. **Consistent Color Schemes**

Use consistent colors across toolbar elements for a cohesive design.

```csharp
// Define color palette
Color primaryColor = Color.FromArgb("#6200EE");
Color secondaryColor = Color.FromArgb("#03DAC6");
Color backgroundColor = Color.FromArgb("#FFFFFF");

toolbar.BackgroundColor = backgroundColor;
toolbar.MoreButtonBackground = primaryColor;
toolbar.MoreButtonIconColor = Colors.White;
```

### 2. **Accessible Text Styling**

Enable font auto-scaling and ensure sufficient contrast.

```xaml
<toolbar:ToolbarTextStyle TextColor="Black"
                          FontSize="14"
                          FontAutoScalingEnabled="True" />
```

### 3. **Visual Feedback for Selection**

Always provide clear visual feedback using `SelectionHighlightColor`.

```xaml
<toolbar:SfToolbarItem Name="Bold" 
                       SelectionHighlightColor="LightBlue">
    <!-- Item configuration -->
</toolbar:SfToolbarItem>
```

### 4. **Appropriate Separator Usage**

Use separators to group related items, not between every item.

```xaml
<!-- Good: Group text formatting separately from alignment -->
<toolbar:SfToolbarItem Name="Bold" />
<toolbar:SfToolbarItem Name="Italic" />
<toolbar:SeparatorToolbarItem Stroke="Gray" StrokeThickness="1" />
<toolbar:SfToolbarItem Name="AlignLeft" />
<toolbar:SfToolbarItem Name="AlignRight" />
```

### 5. **Corner Radius Consistency**

Match toolbar and selection corner radius for visual harmony.

```xaml
<toolbar:SfToolbar CornerRadius="12" SelectionCornerRadius="8">
    <!-- Items -->
</toolbar:SfToolbar>
```

### 6. **Platform-Specific Colors**

Use platform-specific colors for native look and feel.

```xaml
<toolbar:SfToolbar.BackgroundColor>
    <OnPlatform x:TypeArguments="Color">
        <On Platform="iOS" Value="#007AFF" />
        <On Platform="Android" Value="#6200EE" />
        <On Platform="WinUI" Value="#0078D4" />
    </OnPlatform>
</toolbar:SfToolbar.BackgroundColor>
```

### 7. **Disabled State Indication**

Use visual cues (reduced opacity, gray color) for disabled items.

```xaml
<toolbar:SfToolbarItem Name="Save" IsEnabled="False">
    <toolbar:SfToolbarItem.Icon>
        <FontImageSource Glyph="&#xE74E;" 
                         Color="Gray"
                         FontFamily="MauiMaterialAssets" />
    </toolbar:SfToolbarItem.Icon>
</toolbar:SfToolbarItem>
```

### 8. **Custom Templates for Branding**

Use custom navigation button templates for brand-specific designs.

```xaml
<toolbar:SfToolbar.ForwardButtonTemplate>
    <DataTemplate>
        <Border BackgroundColor="{StaticResource BrandColor}"
                StrokeThickness="0"
                Padding="10">
            <Image Source="brand_arrow.png" />
        </Border>
    </DataTemplate>
</toolbar:SfToolbar.ForwardButtonTemplate>
```

### 9. **Avoid Over-Customization**

Balance customization with platform conventions to maintain usability.

### 10. **Test on Multiple Platforms**

Always test customizations on all target platforms (iOS, Android, Windows) as rendering may vary.
