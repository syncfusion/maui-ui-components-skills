# Selection

## Table of Contents
- [Overview](#overview)
- [Enable Data Point Selection](#enable-data-point-selection)
- [Enable Series Selection](#enable-series-selection)
- [Selection Types](#selection-types)
- [Selection Customization](#selection-customization)
- [Selection Events](#selection-events)
- [Programmatic Selection](#programmatic-selection)
- [Complete Examples](#complete-examples)

## Overview

Selection allows users to interactively select data points or entire series in a chart. The SfCartesianChart provides selection behavior support through `DataPointSelectionBehavior` and `SeriesSelectionBehavior` classes, enabling highlighting and interaction with chart elements.

## Enable Data Point Selection

To enable data point selection, create an instance of `DataPointSelectionBehavior` and assign it to the series `SelectionBehavior` property.

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.XAxes>
        <chart:CategoryAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:ColumnSeries ItemsSource="{Binding Data}"
                       XBindingPath="Category"
                       YBindingPath="Value">
        <chart:ColumnSeries.SelectionBehavior>
            <chart:DataPointSelectionBehavior SelectionBrush="#314A6E"/>
        </chart:ColumnSeries.SelectionBehavior>
    </chart:ColumnSeries>
</chart:SfCartesianChart>
```

```csharp
SfCartesianChart chart = new SfCartesianChart();

CategoryAxis primaryAxis = new CategoryAxis();
chart.XAxes.Add(primaryAxis);

NumericalAxis secondaryAxis = new NumericalAxis();
chart.YAxes.Add(secondaryAxis);

DataPointSelectionBehavior selection = new DataPointSelectionBehavior();
selection.SelectionBrush = Color.FromArgb("#314A6E");

ColumnSeries series = new ColumnSeries()
{
    ItemsSource = new ViewModel().Data,
    XBindingPath = "Category",
    YBindingPath = "Value",
    SelectionBehavior = selection
};

chart.Series.Add(series);
this.Content = chart;
```

## Enable Series Selection

To enable series selection, create an instance of `SeriesSelectionBehavior` and assign it to the chart's `SelectionBehavior` property.

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.SelectionBehavior>
        <chart:SeriesSelectionBehavior SelectionBrush="#314A6E"/>
    </chart:SfCartesianChart.SelectionBehavior>
    
    <chart:SfCartesianChart.XAxes>
        <chart:CategoryAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:ColumnSeries ItemsSource="{Binding Data}"
                       XBindingPath="Country"
                       YBindingPath="Kids"/>
    
    <chart:ColumnSeries ItemsSource="{Binding Data}"
                       XBindingPath="Country"
                       YBindingPath="Adults"/>
    
    <chart:ColumnSeries ItemsSource="{Binding Data}"
                       XBindingPath="Country"
                       YBindingPath="Seniors"/>
</chart:SfCartesianChart>
```

```csharp
SfCartesianChart chart = new SfCartesianChart();

CategoryAxis primaryAxis = new CategoryAxis();
chart.XAxes.Add(primaryAxis);

NumericalAxis secondaryAxis = new NumericalAxis();
chart.YAxes.Add(secondaryAxis);

SeriesSelectionBehavior selection = new SeriesSelectionBehavior();
selection.SelectionBrush = Color.FromArgb("#314A6E");
chart.SelectionBehavior = selection;

ColumnSeries series1 = new ColumnSeries()
{
    ItemsSource = new ViewModel().Data,
    XBindingPath = "Country",
    YBindingPath = "Kids"
};

ColumnSeries series2 = new ColumnSeries()
{
    ItemsSource = new ViewModel().Data,
    XBindingPath = "Country",
    YBindingPath = "Adults"
};

ColumnSeries series3 = new ColumnSeries()
{
    ItemsSource = new ViewModel().Data,
    XBindingPath = "Country",
    YBindingPath = "Seniors"
};

chart.Series.Add(series1);
chart.Series.Add(series2);
chart.Series.Add(series3);
this.Content = chart;
```

## Selection Types

The `Type` property in `ChartSelectionBehavior` controls the selection mode using the `ChartSelectionType` enum:

**Available Types:**
- **`Single`**: Select only one item at a time
- **`SingleDeselect`**: Select and deselect only one item at a time
- **`Multiple`**: Select and deselect multiple items at a time
- **`None`**: Disable selection (default)

### Single Selection

```xml
<chart:ColumnSeries ItemsSource="{Binding Data}"
                   XBindingPath="Category"
                   YBindingPath="Value">
    <chart:ColumnSeries.SelectionBehavior>
        <chart:DataPointSelectionBehavior Type="Single" 
                                         SelectionBrush="#314A6E"/>
    </chart:ColumnSeries.SelectionBehavior>
</chart:ColumnSeries>
```

```csharp
DataPointSelectionBehavior selection = new DataPointSelectionBehavior()
{
    Type = ChartSelectionType.Single,
    SelectionBrush = Color.FromArgb("#314A6E")
};

ColumnSeries series = new ColumnSeries()
{
    ItemsSource = new ViewModel().Data,
    XBindingPath = "Category",
    YBindingPath = "Value",
    SelectionBehavior = selection
};
```

### Multiple Selection

```xml
<chart:ColumnSeries ItemsSource="{Binding Data}"
                   XBindingPath="Category"
                   YBindingPath="Value">
    <chart:ColumnSeries.SelectionBehavior>
        <chart:DataPointSelectionBehavior Type="Multiple" 
                                         SelectionBrush="#314A6E"/>
    </chart:ColumnSeries.SelectionBehavior>
</chart:ColumnSeries>
```

```csharp
DataPointSelectionBehavior selection = new DataPointSelectionBehavior()
{
    Type = ChartSelectionType.Multiple,
    SelectionBrush = Color.FromArgb("#314A6E")
};
```

## Selection Customization

The `SelectionBrush` property customizes the appearance of selected items.

### For Data Point Selection

```xml
<chart:ColumnSeries ItemsSource="{Binding Data}"
                   XBindingPath="Category"
                   YBindingPath="Value">
    <chart:ColumnSeries.SelectionBehavior>
        <chart:DataPointSelectionBehavior SelectionBrush="Red"/>
    </chart:ColumnSeries.SelectionBehavior>
</chart:ColumnSeries>
```

```csharp
DataPointSelectionBehavior selection = new DataPointSelectionBehavior()
{
    SelectionBrush = Colors.Red
};

ColumnSeries series = new ColumnSeries()
{
    SelectionBehavior = selection
};
```

### For Series Selection

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.SelectionBehavior>
        <chart:SeriesSelectionBehavior SelectionBrush="LightBlue"/>
    </chart:SfCartesianChart.SelectionBehavior>
</chart:SfCartesianChart>
```

```csharp
SeriesSelectionBehavior selection = new SeriesSelectionBehavior()
{
    SelectionBrush = Colors.LightBlue
};

chart.SelectionBehavior = selection;
```

## Selection Events

The selection behaviors provide events to handle selection changes.

### SelectionChanging Event

This event is raised before the selection changes and can be canceled.

```csharp
DataPointSelectionBehavior selection = new DataPointSelectionBehavior();
selection.SelectionChanging += Selection_SelectionChanging;

private void Selection_SelectionChanging(object sender, ChartSelectionChangingEventArgs e)
{
    // Cancel selection if needed
    if (e.NewIndexes[0] == 3)
    {
        e.Cancel = true;
    }
}
```

### SelectionChanged Event

This event is raised after the selection changes.

```csharp
DataPointSelectionBehavior selection = new DataPointSelectionBehavior();
selection.SelectionChanged += Selection_SelectionChanged;

private void Selection_SelectionChanged(object sender, ChartSelectionChangedEventArgs e)
{
    var newIndexes = e.NewIndexes;
    var oldIndexes = e.OldIndexes;
    
    // Handle selection change
    DisplayAlert("Selection", $"Selected index: {newIndexes[0]}", "OK");
}
```

## Programmatic Selection

You can programmatically control selection using the `SelectedIndex` and `SelectedIndexes` properties.

### Single Item Selection

```csharp
DataPointSelectionBehavior selection = new DataPointSelectionBehavior();
selection.SelectedIndex = 2; // Select item at index 2
```

```xml
<chart:ColumnSeries ItemsSource="{Binding Data}"
                   XBindingPath="Category"
                   YBindingPath="Value">
    <chart:ColumnSeries.SelectionBehavior>
        <chart:DataPointSelectionBehavior SelectedIndex="2"/>
    </chart:ColumnSeries.SelectionBehavior>
</chart:ColumnSeries>
```

### Multiple Items Selection

```csharp
DataPointSelectionBehavior selection = new DataPointSelectionBehavior()
{
    Type = ChartSelectionType.Multiple
};
selection.SelectedIndexes = new List<int> { 0, 2, 4 };
```

### Clear Selection

Use the `ClearSelection()` method to clear all selections.

```csharp
DataPointSelectionBehavior selection = new DataPointSelectionBehavior();
selection.ClearSelection();
```

## Complete Examples

### Data Point Selection Example

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.XAxes>
        <chart:CategoryAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:ColumnSeries ItemsSource="{Binding Data}"
                       XBindingPath="Month"
                       YBindingPath="Value">
        <chart:ColumnSeries.SelectionBehavior>
            <chart:DataPointSelectionBehavior Type="Multiple" 
                                             SelectionBrush="#4CAF50"
                                             SelectionChanging="OnSelectionChanging"
                                             SelectionChanged="OnSelectionChanged"/>
        </chart:ColumnSeries.SelectionBehavior>
    </chart:ColumnSeries>
</chart:SfCartesianChart>
```

### Series Selection Example

```csharp
SfCartesianChart chart = new SfCartesianChart();

SeriesSelectionBehavior seriesSelection = new SeriesSelectionBehavior()
{
    Type = ChartSelectionType.SingleDeselect,
    SelectionBrush = Color.FromArgb("#FF6B6B")
};
seriesSelection.SelectionChanged += OnSeriesSelectionChanged;
chart.SelectionBehavior = seriesSelection;

private void OnSeriesSelectionChanged(object sender, ChartSelectionChangedEventArgs e)
{
    // Process series selection
    if (e.NewIndexes.Count > 0)
    {
        int selectedSeriesIndex = e.NewIndexes[0];
        // Handle selected series
    }
}
{
    private int _selectedProductIndex = -1;
    
    public ObservableCollection<SalesModel> SalesData { get; set; }
    
    public int SelectedProductIndex
    {
        get => _selectedProductIndex;
        set
        {
            _selectedProductIndex = value;
            OnPropertyChanged();
            OnProductSelected();
        }
    }
    
    private void OnProductSelected()
    {
        if (_selectedProductIndex >= 0 && _selectedProductIndex < SalesData.Count)
        {
            var selected = SalesData[_selectedProductIndex];
            // Process selection
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

## Best Practices

### When to Use Selection

1. **Interactive Dashboards**: Let users explore data
2. **Detail Views**: Show details for selected items
3. **Filtering**: Filter related data based on selection
4. **Highlighting**: Emphasize specific data points
5. **Drill-Down**: Navigate to detailed views

### Selection Guidelines

1. **Visual Feedback**: Use distinct selection color
2. **Clear Indication**: Ensure selection is obvious
3. **Deselection**: Provide way to clear selection
4. **Performance**: Test with large datasets
5. **Accessibility**: Consider keyboard navigation

### Color Selection

1. Use high-contrast colors for selection
2. Ensure visibility against chart background
3. Consider color-blind friendly options
4. Test selection colors on target devices

### Common Patterns

**Dashboard Selection:**
```xml
<chart:ColumnSeries SelectionBehavior="SelectPoint"
                    SelectionBrush="#FF5722"
                    SelectedIndex="{Binding SelectedIndex, Mode=TwoWay}"/>
```

**Comparison Highlighting:**
```xml
<chart:LineSeries SelectionBehavior="SelectSeries"
                  SelectionBrush="#4CAF50"/>
```

## Troubleshooting

**Issue**: Selection not working
- **Solution**: Set `SelectionBehavior` property

**Issue**: Selection color not visible
- **Solution**: Choose contrasting `SelectionBrush` color

**Issue**: Can't deselect
- **Solution**: Set `SelectedIndex = -1` or handle tap again

**Issue**: Multiple selection not working
- **Solution**: Verify series type supports multiple selection

**Issue**: Selection lost on data update
- **Solution**: Store and restore `SelectedIndex` after updates
