# Selection in .NET MAUI Circular Charts

Selection allows users to highlight chart segments by tapping them. This guide covers enabling and customizing selection behavior.

## Enabling Selection

Create an instance of `DataPointSelectionBehavior` and assign it to the series' `SelectionBehavior` property.

**XAML:**
```xml
<chart:SfCircularChart>
    <chart:DoughnutSeries ItemsSource="{Binding Data}"
                          XBindingPath="Category"
                          YBindingPath="Value">
        <chart:DoughnutSeries.SelectionBehavior>
            <chart:DataPointSelectionBehavior SelectionBrush="#314A6E"/>
        </chart:DoughnutSeries.SelectionBehavior>
    </chart:DoughnutSeries>
</chart:SfCircularChart>
```

**C#:**
```csharp
DataPointSelectionBehavior selection = new DataPointSelectionBehavior
{
    SelectionBrush = Color.FromHex("#314A6E")
};

DoughnutSeries series = new DoughnutSeries();
series.SelectionBehavior = selection;
```

## Selection Types

The `Type` property controls how selection behaves. Use the `ChartSelectionType` enum.

### Single Selection

User can select only one item at a time. Selecting a new item deselects the previous one.

**XAML:**
```xml
<chart:DataPointSelectionBehavior Type="Single" 
                                  SelectionBrush="Orange"/>
```

**C#:**
```csharp
selection.Type = ChartSelectionType.Single;
```

### Single Deselect

User can select one item, and tap again to deselect it.

**XAML:**
```xml
<chart:DataPointSelectionBehavior Type="SingleDeselect" 
                                  SelectionBrush="Green"/>
```

**C#:**
```csharp
selection.Type = ChartSelectionType.SingleDeselect;
```

### Multiple Selection

User can select and deselect multiple items.

**XAML:**
```xml
<chart:DataPointSelectionBehavior Type="Multiple" 
                                  SelectionBrush="Purple"/>
```

**C#:**
```csharp
selection.Type = ChartSelectionType.Multiple;
```

### None

Disables selection entirely.

**C#:**
```csharp
selection.Type = ChartSelectionType.None;
```

## Selection Brush

The `SelectionBrush` property defines the color applied to selected segments.

**XAML:**
```xml
<chart:DataPointSelectionBehavior SelectionBrush="#FF6347"/>
```

**C#:**
```csharp
selection.SelectionBrush = new SolidColorBrush(Color.FromArgb("#FF6347"));
selection.SelectionBrush = Colors.Tomato;
```

## Programmatic Selection

### Select by Index

Use `SelectedIndex` to select a specific segment programmatically.

**XAML:**
```xml
<chart:DataPointSelectionBehavior SelectedIndex="2"/>
```

**C#:**
```csharp
selection.SelectedIndex = 2;  // Select third segment (0-based)
```

### Select Multiple Indices

Use `SelectedIndexes` for multiple selection.

**C#:**
```csharp
selection.Type = ChartSelectionType.Multiple;
selection.SelectedIndexes = new List<int> { 0, 2, 4 };
```

### Clear Selection

**C#:**
```csharp
// Clear all selections
selection.ClearSelection();

// Or set to invalid index
selection.SelectedIndex = -1;
```

## Selection Events

Handle selection changes with `SelectionChanging` and `SelectionChanged` events.

### SelectionChanging Event

Fires before selection occurs. Can be canceled.

**C#:**
```csharp
DataPointSelectionBehavior selection = new DataPointSelectionBehavior();
selection.SelectionChanging += OnSelectionChanging;

private void OnSelectionChanging(object sender, ChartSelectionChangingEventArgs e)
{
    // Get new and old selections
    var newIndexes = e.NewIndexes;
    var oldIndexes = e.OldIndexes;
    
    // Cancel selection if needed
    if (ShouldCancelSelection(newIndexes))
    {
        e.Cancel = true;
    }
}
```

**Event Properties:**
- **NewIndexes**: Indexes being selected
- **OldIndexes**: Previously selected indexes
- **Cancel**: Set to `true` to prevent selection

### SelectionChanged Event

Fires after selection completes.

**C#:**
```csharp
selection.SelectionChanged += OnSelectionChanged;

private void OnSelectionChanged(object sender, ChartSelectionChangedEventArgs e)
{
    var newIndexes = e.NewIndexes;
    var oldIndexes = e.OldIndexes;
    
    // Handle selection change
    UpdateUI(newIndexes);
    LogSelection(newIndexes);
}
```

**Event Properties:**
- **NewIndexes**: Currently selected indexes
- **OldIndexes**: Previously selected indexes

## Complete Examples

### Example 1: Single Selection with Custom Color

```xml
<chart:SfCircularChart>
    <chart:PieSeries ItemsSource="{Binding Data}"
                     XBindingPath="Product"
                     YBindingPath="Sales"
                     ShowDataLabels="True">
        <chart:PieSeries.SelectionBehavior>
            <chart:DataPointSelectionBehavior Type="SingleDeselect"
                                              SelectionBrush="#E74C3C"/>
        </chart:PieSeries.SelectionBehavior>
    </chart:PieSeries>
</chart:SfCircularChart>
```

### Example 2: Multiple Selection with Events

```csharp
SfCircularChart chart = new SfCircularChart();

DataPointSelectionBehavior selection = new DataPointSelectionBehavior
{
    Type = ChartSelectionType.Multiple,
    SelectionBrush = Colors.DarkBlue
};

selection.SelectionChanged += (s, e) =>
{
    DisplaySelectedCount(e.NewIndexes.Count);
};

DoughnutSeries series = new DoughnutSeries
{
    ItemsSource = data,
    XBindingPath = "Category",
    YBindingPath = "Value",
    SelectionBehavior = selection
};

chart.Series.Add(series);
```

### Example 3: Programmatic Selection on Load

```csharp
// Select highest value segment
int maxIndex = data
    .Select((item, index) => new { item, index })
    .OrderByDescending(x => x.item.Value)
    .First()
    .index;

DataPointSelectionBehavior selection = new DataPointSelectionBehavior
{
    Type = ChartSelectionType.Single,
    SelectedIndex = maxIndex,
    SelectionBrush = Colors.Gold
};

series.SelectionBehavior = selection;
```

### Example 4: Conditional Selection

```csharp
DataPointSelectionBehavior selection = new DataPointSelectionBehavior
{
    Type = ChartSelectionType.Multiple,
    SelectionBrush = Colors.Green
};

selection.SelectionChanging += (s, e) =>
{
    // Prevent selection of segments below threshold
    foreach (var index in e.NewIndexes)
    {
        if (data[index].Value < threshold)
        {
            e.Cancel = true;
            ShowMessage("Cannot select items below threshold");
            break;
        }
    }
};

series.SelectionBehavior = selection;
```

### Example 5: Selection with Tooltip and Data Labels

```xml
<chart:SfCircularChart>
    <chart:PieSeries ItemsSource="{Binding Data}"
                     XBindingPath="Region"
                     YBindingPath="Revenue"
                     ShowDataLabels="True"
                     EnableTooltip="True">
        <chart:PieSeries.SelectionBehavior>
            <chart:DataPointSelectionBehavior Type="SingleDeselect"
                                              SelectionBrush="#16A085"/>
        </chart:PieSeries.SelectionBehavior>
        
        <chart:PieSeries.DataLabelSettings>
            <chart:CircularDataLabelSettings LabelPosition="Outside"/>
        </chart:PieSeries.DataLabelSettings>
    </chart:PieSeries>
</chart:SfCircularChart>
```

### Example 6: Pre-select Multiple Items

```csharp
DataPointSelectionBehavior selection = new DataPointSelectionBehavior
{
    Type = ChartSelectionType.Multiple,
    SelectionBrush = Colors.Purple,
    SelectedIndexes = new List<int> { 0, 2, 4 }  // Pre-select items
};

series.SelectionBehavior = selection;
```

## Selection with Other Features

### Selection + Explode

```xml
<chart:DoughnutSeries ExplodeOnTouch="True" ExplodeRadius="15">
    <chart:DoughnutSeries.SelectionBehavior>
        <chart:DataPointSelectionBehavior Type="Single"
                                          SelectionBrush="Orange"/>
    </chart:DoughnutSeries.SelectionBehavior>
</chart:DoughnutSeries>
```

### Selection + Legend Toggle

```xml
<chart:SfCircularChart>
    <chart:SfCircularChart.Legend>
        <chart:ChartLegend ToggleSeriesVisibility="True"/>
    </chart:SfCircularChart.Legend>
    
    <chart:PieSeries>
        <chart:PieSeries.SelectionBehavior>
            <chart:DataPointSelectionBehavior Type="Multiple"
                                              SelectionBrush="Red"/>
        </chart:PieSeries.SelectionBehavior>
    </chart:PieSeries>
</chart:SfCircularChart>
```

## Tracking Selected Items

```csharp
public class ChartViewModel : INotifyPropertyChanged
{
    private List<int> _selectedIndexes = new List<int>();
    
    public void InitializeChart()
    {
        var selection = new DataPointSelectionBehavior
        {
            Type = ChartSelectionType.Multiple,
            SelectionBrush = Colors.Blue
        };
        
        selection.SelectionChanged += OnSelectionChanged;
        series.SelectionBehavior = selection;
    }
    
    private void OnSelectionChanged(object sender, ChartSelectionChangedEventArgs e)
    {
        _selectedIndexes = e.NewIndexes.ToList();
        
        // Calculate total of selected items
        double selectedTotal = _selectedIndexes
            .Sum(i => Data[i].Value);
        
        SelectedTotal = selectedTotal;
        SelectedCount = _selectedIndexes.Count;
    }
    
    public double SelectedTotal { get; set; }
    public int SelectedCount { get; set; }
}
```

## Best Practices

1. **Selection Type**: Use `SingleDeselect` for most interactive scenarios
2. **Color Choice**: Choose selection colors that contrast with default colors
3. **Multiple Selection**: Reserve for analysis or comparison scenarios
4. **Events**: Use `SelectionChanged` for updating UI based on selection
5. **Feedback**: Combine with tooltips or labels for clear user feedback
6. **Clear Action**: Provide a "Clear Selection" button for multiple selection

## Use Cases

| Scenario | Selection Type | Best Practice |
|----------|---------------|---------------|
| **Highlight item** | Single | Use for emphasis |
| **Compare items** | Multiple | Show comparison metrics |
| **Drill-down** | SingleDeselect | Navigate on selection |
| **Filter data** | Multiple | Update other UI elements |
| **Analytics** | Multiple + Events | Calculate aggregates |

## Troubleshooting

### Selection Not Working

**Check:**
- `SelectionBehavior` is assigned to the series
- `Type` is not set to `None`
- Chart has valid data
- Touch input is not blocked by other UI elements

### Selection Color Not Visible

**Check:**
- `SelectionBrush` is different from segment colors
- Opacity is not set to 0
- Color has sufficient contrast

### Multiple Selection Not Working

**Check:**
- `Type` is set to `ChartSelectionType.Multiple`
- Not using `SelectedIndex` (use `SelectedIndexes` instead)

### Events Not Firing

**Check:**
- Event handlers are properly subscribed
- Selection is actually changing (not clicking the same item)
- `Type` is not set to `None`
