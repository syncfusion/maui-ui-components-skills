# Sorting and Filtering

## Table of Contents
- [Sorting](#sorting)
  - [Show Sort Numbers](#show-sort-numbers)
  - [Sorting Gesture (Double Tap)](#sorting-gesture-double-tap)
  - [Sorting Events](#sorting-events)
  - [Sort Icon Customization](#sort-icon-customization)
- [Filtering](#filtering)
  - [Filtering Events](#filtering-events)
  - [FilterPopupStyle](#filterpopupstyle)
  - [Advanced Filter Type](#advanced-filter-type)
  - [Filtering Null Values](#filtering-null-values)
  - [Instant Filtering](#instant-filtering)
  - [Loading Performance](#loading-performance)
  - [Customizing Filter Popup Menu](#customizing-filter-popup-menu)
  - [Customize Filter Icon](#customize-filter-icon)
- [Filter Row](#filter-row)
- [Combining Sorting and Filtering](#combining-sorting-and-filtering)

## Sorting

### Enable Sorting

```xml
<syncfusion:SfDataGrid SortingMode="Single"
                       ItemsSource="{Binding Orders}" />
```

```csharp
dataGrid.SortingMode = DataGridSortingMode.Single;
```

**Features:**
- Click column header to sort
- Click again to reverse sort direction
- Click third time to clear sorting

### Multi-Column Sorting

Hold Ctrl/Cmd and click multiple headers:

```xml
<syncfusion:SfDataGrid SortingMode="Multiple" />
```

```csharp
dataGrid.SortingMode = DataGridSortingMode.Multiple;
```

### Programmatic Sorting

```csharp
// Sort ascending
dataGrid.SortColumnDescriptions.Add(new SortColumnDescription
{
    ColumnName = "OrderID",
    SortDirection = System.ComponentModel.ListSortDirection.Ascending
});

// Sort descending
dataGrid.SortColumnDescriptions.Add(new SortColumnDescription
{
    ColumnName = "OrderDate",
    SortDirection = System.ComponentModel.ListSortDirection.Descending
});
```

### Custom Sorting

Implement `IComparer`:

```csharp
public class CustomSorter : IComparer<object>
{
    public int Compare(object x, object y)
    {
        var order1 = x as OrderInfo;
        var order2 = y as OrderInfo;
        
        // Custom logic
        return string.Compare(order1.CustomerID, order2.CustomerID, StringComparison.OrdinalIgnoreCase);
    }
}

// Apply custom sorter
dataGrid.SortComparers.Add(new SortComparer
{
    PropertyName = "CustomerID",
    Comparer = new CustomSorter()
});
```

### Disable Sorting for Column

```xml
<syncfusion:DataGridTextColumn MappingName="Notes"
                               AllowSorting="False" />
```

### Tri-State Sorting

Enable/disable clearing sort:

```xml
<syncfusion:SfDataGrid AllowTriStateSorting="True" />
```

When true: Ascending → Descending → Unsorted

When false: Ascending → Descending only

### Sort Icons

Customize sort indicator icons:

```xml
<syncfusion:SfDataGrid.DefaultStyle>
    <syncfusion:DataGridStyle SortIconColor="Red" />
</syncfusion:SfDataGrid.DefaultStyle>
```

### Show Sort Numbers

```xml
<syncfusion:SfDataGrid x:Name="dataGrid" 
                       SortingMode="Multiple" 
                       ShowSortNumbers="True" 
                       ItemsSource="{Binding OrderInfoCollection}" />
```

```csharp
dataGrid.SortingMode = DataGridSortingMode.Multiple;
dataGrid.ShowSortNumbers = true;
```

### Sorting Gesture (Double Tap)

```xml
<syncfusion:SfDataGrid x:Name="dataGrid" 
                       SortingMode="Single" 
                       SortingGestureType="DoubleTap" 
                       ItemsSource="{Binding OrderInfoCollection}" />
```

```csharp
dataGrid.SortingMode = DataGridSortingMode.Single;
dataGrid.SortingGestureType = DataGridSortingGestureType.DoubleTap;
```

### Sorting Events

#### SortColumnsChanging

```xml
<syncfusion:SfDataGrid x:Name="dataGrid" 
                       SortingMode="Single" 
                       SortColumnsChanging="dataGrid_SortColumnsChanging" 
                       ItemsSource="{Binding OrderInfoCollection}" />
```

```csharp
private void dataGrid_SortColumnsChanging(object sender, DataGridSortColumnsChangingEventArgs e)
{
    // Cancel sorting for a particular column
    if (e.AddedItems[0].ColumnName == "OrderID")
    {
        e.Cancel = true;
    }
}
```

#### SortColumnsChanged

```csharp
dataGrid.SortColumnsChanged += dataGrid_SortColumnsChanged;

private void dataGrid_SortColumnsChanged(object sender, DataGridSortColumnsChangedEventArgs e)
{
    // Get the sorted columns
    var addedItems = e.AddedItems;
    var removedItems = e.RemovedItems;
}
```

### Sort Icon Customization

```xml
<syncfusion:SfDataGrid ItemsSource="{Binding OrderInfoCollection}"
                       x:Name="dataGrid"
                       SortingMode="Multiple">
    <syncfusion:SfDataGrid.SortIconTemplate>
        <DataTemplate>
            <Image Source="expand_less.png" HeightRequest="20" WidthRequest="20"/>
        </DataTemplate>
    </syncfusion:SfDataGrid.SortIconTemplate>
</syncfusion:SfDataGrid>
```

```csharp
dataGrid.SortingMode = DataGridSortingMode.Multiple;
dataGrid.SortIconTemplate = new DataTemplate(() =>
{
    var imageView = new Image()
    {
        Source = "expand_less.png",
        Aspect = Aspect.AspectFit,
        HeightRequest = 20,
        WidthRequest = 20,
    };
    return imageView;
});
```

## Filtering

### Enable Filtering

```xml
<syncfusion:SfDataGrid AllowFiltering="True" />
```

```csharp
dataGrid.AllowFiltering = true;
```

### Programmatic Filtering


### Filter by Property

```csharp
// Filter using View property
dataGrid.View.Filter = FilterRecords;

private bool FilterRecords(object data)
{
    var order = data as OrderInfo;
    
    // Show only orders with quantity > 10
    return order.Quantity > 10;
}

// Refresh view after filter change
dataGrid.View.RefreshFilter();
```

### Complex Filters

```csharp
dataGrid.View.Filter = record =>
{
    var order = record as OrderInfo;
    
    // Multiple conditions
    return order.Quantity > 5 &&
           order.UnitPrice < 100 &&
           order.ShipCountry == "USA";
};

dataGrid.View.RefreshFilter();
```

### Clear Filters

```csharp
dataGrid.View.Filter = null;
dataGrid.View.RefreshFilter();
```

### Disable Filtering for Column

```xml
<syncfusion:DataGridTextColumn MappingName="OrderID"
                               AllowFiltering="False" />
```

### Filtering Events

#### FilterChanging

```xml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       ItemsSource="{Binding OrderInfoCollection}" 
                       AllowFiltering="True"
                       FilterChanging="dataGrid_FilterChanging"/>
```

```csharp
private void dataGrid_FilterChanging(object sender, DataGridFilterChangingEventArgs e)
{
    // Handle filter change logic
}
```

#### FilterChanged

```csharp
dataGrid.FilterChanged += dataGrid_FilterChanged;

private void dataGrid_FilterChanged(object sender, DataGridFilterChangedEventArgs e)
{
    // Get filtered records
}
```

#### FilterItemsPopulating

```csharp
dataGrid.FilterItemsPopulating += dataGrid_FilterItemsPopulating;

private void dataGrid_FilterItemsPopulating(object sender, DataGridFilterItemsPopulatingEventArgs e)
{
    // Customize filter view properties
}
```

#### FilterItemsPopulated
 
```csharp
dataGrid.FilterItemsPopulated += dataGrid_FilterItemsPopulated;

private void dataGrid_FilterItemsPopulated(object sender, DataGridFilterItemsPopulatedEventArgs e)
{
    // Update filter items after population
}
```

### FilterPopupStyle

```xml
<ContentPage.Resources>
    <Style x:Key="filterViewStyle" TargetType="syncfusion:DataGridFilterView">
        <Setter Property="FilterMode" Value="AdvancedFilter"/>
        <Setter Property="CanGenerateUniqueItems" Value="False"/>
    </Style>
</ContentPage.Resources>

<syncfusion:SfDataGrid x:Name="dataGrid"
                       ItemsSource="{Binding OrderInfoCollection}" 
                       AllowFiltering="True"
                       FilterPopupStyle="{StaticResource filterViewStyle}"/>
```

### Advanced Filter Type

**Options:**
- `StringTyped` - Text filter options
- `NumberTyped` - Numeric filter options
- `DateTyped` - Date filter options
- `StronglyTyped` - Automatically detected based on underlying data type

```xml
<syncfusion:DataGridTextColumn MappingName="OrderID" FilterBehavior="StringTyped"/>
<syncfusion:DataGridNumericColumn MappingName="Quantity" FilterBehavior="NumberTyped"/>
<syncfusion:DataGridDateColumn MappingName="OrderDate" FilterBehavior="DateTyped"/>
```

```csharp
dataGrid.Columns["OrderID"].FilterBehavior = FilterBehavior.StringTyped;
dataGrid.Columns["Quantity"].FilterBehavior = FilterBehavior.NumberTyped;
```

### Filtering Null Values

When `AllowBlankFilters` is `True`:
- Combobox options have Null and Not Null choices in advanced filter
- Checkbox filtering UI includes a Blanks option

```xml
<syncfusion:DataGridTextColumn MappingName="OrderID" AllowBlankFilters="False"/>
```

```csharp
dataGrid.Columns["OrderID"].AllowBlankFilters = false;
```

### Instant Filtering

```xml
<syncfusion:DataGridTextColumn MappingName="OrderID" ImmediateUpdateColumnFilter="True"/>
```

```csharp
dataGrid.Columns["OrderID"].ImmediateUpdateColumnFilter = true;
```

### Loading Performance

```xml
<ContentPage.Resources>
    <Style x:Key="filterViewStyle" TargetType="syncfusion:DataGridFilterView">
        <Setter Property="FilterMode" Value="AdvancedFilter"/>
        <Setter Property="CanGenerateUniqueItems" Value="False"/>
    </Style>
</ContentPage.Resources>

<syncfusion:SfDataGrid x:Name="dataGrid"
                       ItemsSource="{Binding OrderInfoCollection}" 
                       AllowFiltering="True"
                       FilterPopupStyle="{StaticResource filterViewStyle}"/>
```

### Customizing Filter Popup Menu

#### Visibility of Sort Options

```xml
<ContentPage.Resources>
    <Style x:Key="filterViewStyle" TargetType="syncfusion:DataGridFilterView">
        <Setter Property="SortOptionsVisibility" Value="False"/>
    </Style>
</ContentPage.Resources>

<syncfusion:SfDataGrid x:Name="dataGrid"
                       ItemsSource="{Binding OrderInfoCollection}" 
                       AllowFiltering="True"
                       FilterPopupStyle="{StaticResource filterViewStyle}"/>
```

#### Customizing Sorting Text

```csharp
dataGrid.FilterItemsPopulating += dataGrid_FilterItemsPopulating;

private void dataGrid_FilterItemsPopulating(object sender, DataGridFilterItemsPopulatingEventArgs e)
{
    if (e.Column.MappingName == "OrderID")
    {
        var filterView = e.FilterPopupStyle as DataGridFilterView;
        filterView.AscendingSortString = "Sort Ascending";
        filterView.DescendingSortString = "Sort Descending";
    }
}
```

### Customize Filter Icon

```xml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       ItemsSource="{Binding OrderInfoCollection}" 
                       AllowFiltering="True">
    <syncfusion:SfDataGrid.DefaultStyle>
        <syncfusion:DataGridStyle FilterIconColor="Blue"/>
    </syncfusion:SfDataGrid.DefaultStyle>
</syncfusion:SfDataGrid>
```

## Filter Row

Display filter row for user-driven filtering:

```xml
<syncfusion:SfDataGrid AllowFiltering="True"
                       FilterRowPosition="FixedTop" />
```

**FilterRowPosition Options:**
- `FixedTop` - Filter row at top of grid
- `Top` - Filter row below headers
- `Bottom` - Filter row at bottom
- `None` - No filter row (default)


## Combining Sorting and Filtering

```xml
<syncfusion:SfDataGrid SortingMode="Multiple"
                       AllowFiltering="True"
                       FilterRowPosition="FixedTop" />
```

```csharp
// Sort first
dataGrid.SortColumnDescriptions.Add(new SortColumnDescription
{
    ColumnName = "OrderDate",
    SortDirection = System.ComponentModel.ListSortDirection.Descending
});

// Then filter
dataGrid.View.Filter = order =>
{
    var o = order as OrderInfo;
    return o.Quantity > 5;
};

dataGrid.View.RefreshFilter();
```

## Common Patterns

### Pattern 1: Date Range Filter

```csharp
DateTime startDate = new DateTime(2024, 1, 1);
DateTime endDate = new DateTime(2024, 12, 31);

dataGrid.View.Filter = record =>
{
    var order = record as OrderInfo;
    return order.OrderDate >= startDate && order.OrderDate <= endDate;
};

dataGrid.View.RefreshFilter();
```

### Pattern 2: Text Search Filter

```csharp
string searchText = "John";

dataGrid.View.Filter = record =>
{
    var order = record as OrderInfo;
    return order.CustomerID.Contains(searchText, StringComparison.OrdinalIgnoreCase) ||
           order.ShipCountry.Contains(searchText, StringComparison.OrdinalIgnoreCase);
};

dataGrid.View.RefreshFilter();
```

### Pattern 3: Multi-Criterial Filter

```csharp
dataGrid.View.Filter = record =>
{
    var order = record as OrderInfo;
    
    bool quantityFilter = order.Quantity >= 10;
    bool priceFilter = order.UnitPrice <= 50;
    bool countryFilter = order.ShipCountry == "USA" || order.ShipCountry == "Canada";
    
    return quantityFilter && priceFilter && countryFilter;
};

dataGrid.View.RefreshFilter();
```

## Troubleshooting

### Sorting Not Working

**Solutions:**
- Set `SortingMode="Single"` or `SortingMode="Multiple"`
- Check column `AllowSorting` not set to False
- Verify data implements proper comparison

### Filter Not Applied

**Solutions:**
- Call `dataGrid.View.RefreshFilter()` after setting filter
- Check filter logic returns correct boolean
- Verify filter is not null

### Filter Row Not Showing

**Solutions:**
- Set `FilterRowPosition` to FixedTop, Top, or Bottom

## Next Steps

- Read [searching.md](searching.md) for search functionality
- Read [grouping-summaries.md](grouping-summaries.md) for grouping
