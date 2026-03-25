# Layouts in .NET MAUI ListView

## Overview

The SfListView supports different layouts to arrange items in various configurations. The layout determines how items are positioned and rendered on screen.

**Available Layouts:**
- **LinearLayout** - Arranges items in a single column (vertical) or single row (horizontal)
- **GridLayout** - Arranges items in a predefined number of columns/rows

Use the `SfListView.ItemsLayout` property to set the layout.

## Linear Layout (Default)

LinearLayout is the default layout that arranges items linearly in a single column (vertical) or single row (horizontal).

### Vertical Linear Layout

```xml
<syncfusion:SfListView x:Name="listView"
                       ItemsSource="{Binding CategoryInfo}"
                       ItemSize="100">
    <syncfusion:SfListView.ItemsLayout>
        <syncfusion:LinearLayout />
    </syncfusion:SfListView.ItemsLayout>
</syncfusion:SfListView>
```

```csharp
listView.ItemsLayout = new LinearLayout();
```

**Default orientation is Vertical**, so items stack vertically.

### Horizontal Linear Layout

```xml
<syncfusion:SfListView Orientation="Horizontal" ItemSize="150">
    <syncfusion:SfListView.ItemsLayout>
        <syncfusion:LinearLayout />
    </syncfusion:SfListView.ItemsLayout>
</syncfusion:SfListView>
```

```csharp
listView.Orientation = ItemsLayoutOrientation.Horizontal;
listView.ItemsLayout = new LinearLayout();
```

**Use Cases for Linear Layout:**
- Simple lists (contacts, settings, emails)
- Vertical scrolling feeds
- Horizontal carousels
- Single-column lists

## Grid Layout

GridLayout arranges items in a grid with a predefined number of columns (in vertical orientation) or rows (in horizontal orientation).

### Basic Grid Layout

```xml
<syncfusion:SfListView x:Name="listView"
                       ItemsSource="{Binding GalleryInfo}"
                       ItemSize="150">
    <syncfusion:SfListView.ItemsLayout>
        <syncfusion:GridLayout SpanCount="2" />
    </syncfusion:SfListView.ItemsLayout>
</syncfusion:SfListView>
```

```csharp
listView.ItemsLayout = new GridLayout() { SpanCount = 2 };
```

**SpanCount** defines:
- **Number of columns** in vertical orientation
- **Number of rows** in horizontal orientation

### SpanCount Examples

**Two Columns:**
```xml
<syncfusion:GridLayout SpanCount="2" />
```

**Three Columns:**
```xml
<syncfusion:GridLayout SpanCount="3" />
```

**Four Columns:**
```xml
<syncfusion:GridLayout SpanCount="4" />
```

### Grid Layout with Horizontal Orientation

```xml
<syncfusion:SfListView Orientation="Horizontal" ItemSize="150">
    <syncfusion:SfListView.ItemsLayout>
        <syncfusion:GridLayout SpanCount="3" />
    </syncfusion:SfListView.ItemsLayout>
</syncfusion:SfListView>
```

In horizontal orientation, SpanCount defines the number of rows.

**Use Cases for Grid Layout:**
- Photo galleries
- Product catalogs
- App icon grids
- Tile-based interfaces
- Dashboard widgets

## Changing Layout Dynamically

You can switch layouts at runtime based on user preference or screen orientation.

```csharp
public partial class MainPage : ContentPage
{
    private LinearLayout linearLayout = new LinearLayout();
    private GridLayout gridLayout = new GridLayout { SpanCount = 2 };
    
    public MainPage()
    {
        InitializeComponent();
        listView.ItemsLayout = linearLayout;
    }
    
    private void OnLinearLayoutClicked(object sender, EventArgs e)
    {
        listView.ItemsLayout = linearLayout;
    }
    
    private void OnGridLayoutClicked(object sender, EventArgs e)
    {
        listView.ItemsLayout = gridLayout;
    }
}
```

**With Toggle Button:**
```xml
<StackLayout>
    <HorizontalStackLayout HorizontalOptions="Center" Spacing="10" Margin="10">
        <Button Text="Linear" Clicked="OnLinearLayoutClicked" />
        <Button Text="Grid" Clicked="OnGridLayoutClicked" />
    </HorizontalStackLayout>
    
    <syncfusion:SfListView x:Name="listView" 
                           ItemsSource="{Binding Items}"
                           ItemSize="150" />
</StackLayout>
```

## Responsive Span Count Based on Screen Size

Adjust SpanCount dynamically based on screen width to create responsive layouts.

### Method 1: PropertyChanged Event

```csharp
public partial class MainPage : ContentPage
{
    private GridLayout gridLayout;
    
    public MainPage()
    {
        InitializeComponent();
        
        gridLayout = new GridLayout();
        listView.ItemsLayout = gridLayout;
        
        // Subscribe to property changes
        this.PropertyChanged += OnPagePropertyChanged;
    }
    
    private void OnPagePropertyChanged(object sender, PropertyChangedEventArgs e)
    {
        if (e.PropertyName == nameof(Width))
        {
            UpdateSpanCount();
        }
    }
    
    private void UpdateSpanCount()
    {
        // Calculate columns based on screen width and item size
        double itemWidth = 150; // Target item width
        int spanCount = (int)(this.Width / itemWidth);
        
        // Ensure at least 1 column
        spanCount = Math.Max(1, spanCount);
        
        gridLayout.SpanCount = spanCount;
    }
    
    protected override void OnSizeAllocated(double width, double height)
    {
        base.OnSizeAllocated(width, height);
        UpdateSpanCount();
    }
}
```

### Method 2: Based on Device Idiom

```csharp
private void SetLayoutByDeviceIdiom()
{
    var gridLayout = new GridLayout();
    
    switch (DeviceInfo.Idiom)
    {
        case DeviceIdiom.Phone:
            gridLayout.SpanCount = 2; // 2 columns on phone
            break;
        case DeviceIdiom.Tablet:
            gridLayout.SpanCount = 4; // 4 columns on tablet
            break;
        case DeviceIdiom.Desktop:
            gridLayout.SpanCount = 6; // 6 columns on desktop
            break;
        default:
            gridLayout.SpanCount = 2;
            break;
    }
    
    listView.ItemsLayout = gridLayout;
}
```

### Method 3: Based on Orientation

```csharp
protected override void OnSizeAllocated(double width, double height)
{
    base.OnSizeAllocated(width, height);
    
    var gridLayout = listView.ItemsLayout as GridLayout;
    if (gridLayout != null)
    {
        // Portrait: 2 columns, Landscape: 4 columns
        gridLayout.SpanCount = width < height ? 2 : 4;
    }
}
```

## Layout Best Practices

### Choose the Right Layout

**Use LinearLayout when:**
- Displaying simple lists with consistent item heights
- Items vary significantly in height
- Need single-column vertical scrolling
- Building settings pages or menu lists

**Use GridLayout when:**
- Displaying items of similar size (images, tiles)
- Need multi-column layout
- Building photo galleries or product grids
- Maximizing screen space utilization

### Performance Considerations

1. **Fixed ItemSize:** Set a fixed ItemSize for better performance
   ```xml
   <syncfusion:SfListView ItemSize="150" />
   ```

2. **Avoid Complex Templates:** Keep item templates simple for smooth scrolling

3. **Use Appropriate SpanCount:** Too many columns can reduce readability

4. **Cache Layout Objects:** Reuse layout instances instead of creating new ones

### ItemSize in Different Layouts

**LinearLayout (Vertical):**
- ItemSize = Item height
- Item width = ListView width

**LinearLayout (Horizontal):**
- ItemSize = Item width
- Item height = ListView height

**GridLayout (Vertical):**
- ItemSize = Item height
- Item width = ListView width / SpanCount

**GridLayout (Horizontal):**
- ItemSize = Item width
- Item height = ListView height / SpanCount

## Complete Example: Responsive Grid Gallery

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:syncfusion="clr-namespace:Syncfusion.Maui.ListView;assembly=Syncfusion.Maui.ListView"
             x:Class="MyApp.GalleryPage">
             
    <syncfusion:SfListView x:Name="listView"
                           ItemsSource="{Binding Photos}"
                           ItemSize="150">
        <syncfusion:SfListView.ItemsLayout>
            <syncfusion:GridLayout x:Name="gridLayout" SpanCount="2" />
        </syncfusion:SfListView.ItemsLayout>
        
        <syncfusion:SfListView.ItemTemplate>
            <DataTemplate>
                <Frame Margin="5" Padding="0" CornerRadius="8" HasShadow="True">
                    <Image Source="{Binding PhotoUrl}" Aspect="AspectFill" />
                </Frame>
            </DataTemplate>
        </syncfusion:SfListView.ItemTemplate>
    </syncfusion:SfListView>
    
</ContentPage>
```

```csharp
public partial class GalleryPage : ContentPage
{
    public GalleryPage()
    {
        InitializeComponent();
        this.PropertyChanged += OnPagePropertyChanged;
    }
    
    private void OnPagePropertyChanged(object sender, PropertyChangedEventArgs e)
    {
        if (e.PropertyName == nameof(Width))
        {
            // Responsive column count
            int spanCount = this.Width switch
            {
                < 600 => 2,   // Phone portrait
                < 900 => 3,   // Phone landscape / small tablet
                < 1200 => 4,  // Tablet
                _ => 6        // Desktop / large tablet
            };
            
            gridLayout.SpanCount = spanCount;
        }
    }
}
```

## Troubleshooting

**Issue:** Grid items not filling width properly
**Solution:** Ensure ItemSize is set and template uses appropriate sizing

**Issue:** SpanCount not updating
**Solution:** Create new GridLayout instance or trigger layout update

**Issue:** Items overlapping in grid
**Solution:** Add appropriate Margin/Padding in ItemTemplate

**Issue:** Slow scrolling in grid layout
**Solution:** Simplify item templates, use fixed ItemSize, optimize image loading
