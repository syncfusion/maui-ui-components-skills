# Populating Segment Items

This guide explains how to add and configure items in the Segmented Control using different data sources and formats.

## Overview

The SfSegmentedControl supports multiple ways to populate segments:
- **String arrays** - Simple text-only segments
- **SfSegmentItem objects** - Segments with text, icons, or both
- **Data binding** - Dynamic item sources from view models
- **Mixed content** - Combination of text and images

## Using String Arrays (Simple Approach)

The simplest way to populate segments is using a string array or list.

### XAML Approach

```xml
<buttons:SfSegmentedControl>
    <buttons:SfSegmentedControl.ItemsSource>
        <x:Array Type="{x:Type x:String}">
            <x:String>Day</x:String>
            <x:String>Week</x:String>
            <x:String>Month</x:String>
            <x:String>Year</x:String>
        </x:Array>
    </buttons:SfSegmentedControl.ItemsSource>
</buttons:SfSegmentedControl>
```

### C# Approach

```csharp
var segmentedControl = new SfSegmentedControl
{
    ItemsSource = new List<string> 
    { 
        "Day", "Week", "Month", "Year" 
    }
};
```

**When to use:** Text-only segments with default styling, such as view switchers, time period selectors, or simple filters.

## Using SfSegmentItem Objects (Advanced Approach)

Use `SfSegmentItem` objects when you need:
- Icons or images alongside text
- Per-item customization (colors, backgrounds, styles)
- Disabled segments
- Custom widths for individual segments

### Text-Only Segments

```csharp
var segmentedControl = new SfSegmentedControl
{
    ItemsSource = new List<SfSegmentItem>
    {
        new SfSegmentItem { Text = "Day" },
        new SfSegmentItem { Text = "Week" },
        new SfSegmentItem { Text = "Month" },
        new SfSegmentItem { Text = "Year" }
    }
};
```

### Segments with Images

```csharp
var segmentedControl = new SfSegmentedControl
{
    ItemsSource = new List<SfSegmentItem>
    {
        new SfSegmentItem 
        { 
            Text = "List", 
            ImageSource = "list_icon.png" 
        },
        new SfSegmentItem 
        { 
            Text = "Grid", 
            ImageSource = "grid_icon.png" 
        },
        new SfSegmentItem 
        { 
            Text = "Map", 
            ImageSource = "map_icon.png" 
        }
    }
};
```

**Image location:** Place images in the `Resources/Images` folder of your MAUI project.

### Icon-Only Segments

```csharp
var segmentedControl = new SfSegmentedControl
{
    ItemsSource = new List<SfSegmentItem>
    {
        new SfSegmentItem { ImageSource = "home.png" },
        new SfSegmentItem { ImageSource = "search.png" },
        new SfSegmentItem { ImageSource = "profile.png" }
    }
};
```

**Note:** Omit the `Text` property to display only icons. Ensure icons have sufficient contrast and size (24-32 pixels recommended).

## Using Font Icons

For scalable vector icons, use font icons from icon fonts like Material Icons or Font Awesome.

### Setup Font Icons

1. **Add font file to project:**
   - Place `.ttf` file in `Resources/Fonts` folder

2. **Register font in MauiProgram.cs:**
   ```csharp
   builder.ConfigureFonts(fonts =>
   {
       fonts.AddFont("MaterialIcons-Regular.ttf", "MaterialIcons");
   });
   ```

3. **Use font icons in segments:**
   ```csharp
   var segmentedControl = new SfSegmentedControl
   {
       ItemsSource = new List<SfSegmentItem>
       {
           new SfSegmentItem 
           { 
               Text = "\ue88a",  // Material icon code
               TextStyle = new SegmentTextStyle 
               { 
                   FontFamily = "MaterialIcons",
                   FontSize = 24
               }
           }
       }
   };
   ```

## Data Binding with ViewModels

Bind the ItemsSource to a collection in your ViewModel for dynamic updates.

### ViewModel Setup

```csharp
public class MainViewModel : INotifyPropertyChanged
{
    private ObservableCollection<string> _periods;
    
    public ObservableCollection<string> Periods
    {
        get => _periods;
        set
        {
            _periods = value;
            OnPropertyChanged();
        }
    }
    
    public MainViewModel()
    {
        Periods = new ObservableCollection<string>
        {
            "Day", "Week", "Month", "Year"
        };
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

### XAML Binding

```xml
<ContentPage xmlns:buttons="clr-namespace:Syncfusion.Maui.Buttons;assembly=Syncfusion.Maui.Buttons"
             x:Class="MyApp.MainPage">
    
    <ContentPage.BindingContext>
        <local:MainViewModel />
    </ContentPage.BindingContext>
    
    <buttons:SfSegmentedControl ItemsSource="{Binding Periods}"/>
    
</ContentPage>
```

### Dynamic Item Addition

```csharp
// Add item dynamically
viewModel.Periods.Add("Quarter");

// Remove item
viewModel.Periods.Remove("Day");

// Clear all items
viewModel.Periods.Clear();
```

**Benefit:** Using `ObservableCollection` automatically updates the UI when items change.

## Advanced SfSegmentItem Configuration

### Per-Item Customization

```csharp
var segmentedControl = new SfSegmentedControl
{
    ItemsSource = new List<SfSegmentItem>
    {
        new SfSegmentItem 
        { 
            Text = "All",
            Background = Colors.LightGray,
            TextStyle = new SegmentTextStyle 
            { 
                TextColor = Colors.Black,
                FontSize = 14,
                FontAttributes = FontAttributes.Bold
            }
        },
        new SfSegmentItem 
        { 
            Text = "Active",
            Background = Colors.LightBlue,
            SelectedSegmentBackground = Colors.Blue,
            SelectedSegmentTextColor = Colors.White
        },
        new SfSegmentItem 
        { 
            Text = "Completed",
            IsEnabled = false  // Disabled segment
        }
    }
};
```

### Custom Width Per Item

```csharp
var segmentedControl = new SfSegmentedControl
{
    ItemsSource = new List<SfSegmentItem>
    {
        new SfSegmentItem { Text = "S", Width = 40 },   // Narrow
        new SfSegmentItem { Text = "Medium", Width = 80 },
        new SfSegmentItem { Text = "Large Text", Width = 120 }  // Wide
    }
};
```

**Use case:** Accommodate varying text lengths or emphasize important options with wider segments.

## Populating from Data Models

Map custom data models to SfSegmentItem objects.

### Custom Model

```csharp
public class ViewOption
{
    public string Name { get; set; }
    public string IconPath { get; set; }
    public bool IsAvailable { get; set; }
}
```

### Mapping to SfSegmentItem

```csharp
var viewOptions = new List<ViewOption>
{
    new ViewOption { Name = "List", IconPath = "list.png", IsAvailable = true },
    new ViewOption { Name = "Grid", IconPath = "grid.png", IsAvailable = true },
    new ViewOption { Name = "Chart", IconPath = "chart.png", IsAvailable = false }
};

var segmentedControl = new SfSegmentedControl
{
    ItemsSource = viewOptions.Select(option => new SfSegmentItem
    {
        Text = option.Name,
        ImageSource = option.IconPath,
        IsEnabled = option.IsAvailable
    }).ToList()
};
```

## Best Practices

### Number of Items

- **Optimal:** 2-5 segments for immediate visibility
- **Acceptable:** 6-8 segments with scrolling enabled
- **Avoid:** More than 10 segments (consider alternative UI like dropdown)

### Text Length

- **Recommended:** 3-10 characters per segment
- **Maximum:** 15 characters before text may truncate
- Use abbreviations for longer terms (e.g., "Jan" instead of "January")

### Icons

- **Size:** 20-32 pixels for clarity
- **Format:** PNG with transparency or SVG
- **Contrast:** Ensure icons are visible against segment background
- **Accessibility:** Always provide text labels alongside icons for screen readers

### Performance

- Use `ObservableCollection` only when items change frequently
- For static lists, use `List<string>` or `List<SfSegmentItem>` (lower overhead)
- Avoid creating new item collections on every render

## Common Patterns

### Category Filter

```csharp
var categoryFilter = new SfSegmentedControl
{
    ItemsSource = new List<string> 
    { 
        "All", "Photos", "Videos", "Documents", "Music" 
    }
};
```

### Time Period Selector

```csharp
var timePeriod = new SfSegmentedControl
{
    ItemsSource = new List<SfSegmentItem>
    {
        new SfSegmentItem { Text = "1D", SelectedSegmentTextColor = Colors.Blue },
        new SfSegmentItem { Text = "1W", SelectedSegmentTextColor = Colors.Blue },
        new SfSegmentItem { Text = "1M", SelectedSegmentTextColor = Colors.Blue },
        new SfSegmentItem { Text = "1Y", SelectedSegmentTextColor = Colors.Blue }
    }
};
```

### View Mode Toggle

```csharp
var viewMode = new SfSegmentedControl
{
    ItemsSource = new List<SfSegmentItem>
    {
        new SfSegmentItem { ImageSource = "list_view.png" },
        new SfSegmentItem { ImageSource = "grid_view.png" },
        new SfSegmentItem { ImageSource = "detail_view.png" }
    }
};
```

## Troubleshooting

### Items Not Displaying

- Verify ItemsSource is not null
- Ensure collection has at least 2 items
- Check that strings are not empty or whitespace

### Images Not Showing

- Confirm image files are in `Resources/Images` folder
- Verify image file names match exactly (case-sensitive on some platforms)
- Check Build Action is set to "MauiImage" in project file

### Binding Not Updating

- Use `ObservableCollection` instead of `List` for dynamic updates
- Implement `INotifyPropertyChanged` on ViewModel
- Verify BindingContext is set correctly

## Next Steps

- **Configure selection:** See [selection.md](selection.md) for selection modes and indicators
- **Customize appearance:** See [customization.md](customization.md) for styling options
- **Handle events:** See [events.md](events.md) for responding to user interactions
