# Grouping and Summaries

## Table of Contents
- [Grouping](#grouping)
  - [Enable UI Grouping](#enable-ui-grouping)
  - [GroupDropAreaText](#groupdropareatext)
  - [GroupDropAreaHeight](#groupdropareahight)
  - [Customize the GroupDropArea](#customize-the-groupdroparea)
  - [Customize the GroupDropAreaItem](#customize-the-groupdroparea-item)
  - [Programmatic Grouping](#programmatic-grouping)
  - [Clearing or Removing a Group](#clearing-or-removing-a-group)
  - [Multi Grouping](#multi-grouping)
  - [Custom Grouping](#custom-grouping)
  - [Sorting Records in Grouped Columns](#sorting-records-in-grouped-columns)
  - [Display Based Grouping using GroupMode](#display-based-grouping-using-groupmode)
  - [AllowGroupExpandCollapse](#allowgroupexpandcollapse)
  - [Expand Groups Initially](#expand-groups-initially)
  - [Grouping Events](#grouping-events)
  - [Grouping Customizations](#grouping-customizations)
- [Summaries](#summaries)
- [GroupSummaries](#group-summaries)
- [Caption Summaries](#caption-summaries)

## Grouping

### Enable UI Grouping
```xml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       ItemsSource="{Binding OrderInfoCollection}"
                       AllowGrouping="True">
</syncfusion:SfDataGrid>
```

```csharp
dataGrid.AllowGrouping = true;
```

### GroupDropAreaText

```xml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       ItemsSource="{Binding OrderInfoCollection}"
                       GroupDropAreaText="Drag and drop the column here"
                       AllowGrouping="True">
</syncfusion:SfDataGrid>
```

```csharp
dataGrid.AllowGrouping = true;
dataGrid.GroupDropAreaText = "Drag and drop the column here";
```

### GroupDropAreaHeight

```xml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       ItemsSource="{Binding OrderInfoCollection}"
                       GroupDropAreaHeight="100"
                       AllowGrouping="True">
</syncfusion:SfDataGrid>
```

```csharp
dataGrid.AllowGrouping = true;
dataGrid.GroupDropAreaHeight = 100;
```

### Customize the GroupDropArea

```xml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       ItemsSource="{Binding OrderInfoCollection}"
                       AllowGrouping="True">
    <syncfusion:SfDataGrid.DefaultStyle>
        <syncfusion:DataGridStyle GroupDropAreaTextColor="Blue"
                                  GroupDropAreaBackgroundColor="Yellow"
                                  GroupDropAreaFontAttribute="Bold"
                                  GroupDropAreaFontSize="18"
                                  GroupDropAreaStroke="Brown"
                                  GroupDropAreaStrokeThickness="2"
                                  GroupDropAreaFontFamily="Roboto"/>
    </syncfusion:SfDataGrid.DefaultStyle>
</syncfusion:SfDataGrid>
```

```csharp
dataGrid.AllowGrouping = true;
var defaultStyle = new DataGridStyle()
{
    GroupDropAreaTextColor = Colors.Blue,
    GroupDropAreaBackgroundColor = Colors.Yellow,
    GroupDropAreaFontAttribute = FontAttributes.Bold,
    GroupDropAreaFontSize = 18,
    GroupDropAreaStroke = Colors.Brown,
    GroupDropAreaStrokeThickness = 2,
    GroupDropAreaFontFamily = "Roboto"
};
dataGrid.DefaultStyle = defaultStyle;
```

### Customize the GroupDropAreaItem

```xml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       ItemsSource="{Binding OrderInfoCollection}"
                       AllowGrouping="True">
    <syncfusion:SfDataGrid.DefaultStyle>
        <syncfusion:DataGridStyle GroupDropItemTextColor="Blue"
                                  GroupDropItemBackgroundColor="Yellow"
                                  GroupDropItemFontAttribute="Bold"
                                  GroupDropItemFontSize="18"
                                  GroupDropItemStroke="Brown"
                                  GroupDropItemStrokeThickness="2"
                                  GroupDropItemFontFamily="Roboto"
                                  GroupDropItemCloseIconColor="Gray"/>
    </syncfusion:SfDataGrid.DefaultStyle>
</syncfusion:SfDataGrid>
```

```csharp
dataGrid.AllowGrouping = true;
var defaultStyle = new DataGridStyle()
{
    GroupDropItemTextColor = Colors.Blue,
    GroupDropItemBackgroundColor = Colors.Yellow,
    GroupDropItemFontAttribute = FontAttributes.Bold,
    GroupDropItemFontSize = 18,
    GroupDropItemStroke = Colors.Brown,
    GroupDropItemStrokeThickness = 2,
    GroupDropItemFontFamily = "Roboto",
    GroupDropItemCloseIconColor = Colors.Gray
};
dataGrid.DefaultStyle = defaultStyle;
```

### Programmatic Grouping
```xml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       ItemsSource="{Binding OrderInfoCollection}">
    <syncfusion:SfDataGrid.GroupColumnDescriptions>
        <syncfusion:GroupColumnDescription ColumnName="Name" />
    </syncfusion:SfDataGrid.GroupColumnDescriptions>
</syncfusion:SfDataGrid>
```

```csharp
// Group by single column
dataGrid.GroupColumnDescriptions.Add(new GroupColumnDescription
{
    ColumnName = "ShipCountry"
});

// Group by multiple columns
dataGrid.GroupColumnDescriptions.Add(new GroupColumnDescription
{
    ColumnName = "ShipCity"
});
```

### Clearing or Removing a Group

```csharp
// Clear all groups
dataGrid.GroupColumnDescriptions.Clear();

// Remove a group based on group item
var groupColumn = dataGrid.GroupColumnDescriptions[1];
dataGrid.GroupColumnDescriptions.Remove(groupColumn);

// Remove a group based on index
dataGrid.GroupColumnDescriptions.RemoveAt(0);
```

### Multi Grouping

```xml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       ItemsSource="{Binding OrderInfoCollection}"
                       GroupingMode="Multiple">
    <syncfusion:SfDataGrid.GroupColumnDescriptions>
        <syncfusion:GroupColumnDescription ColumnName="ShipCountry" />
        <syncfusion:GroupColumnDescription ColumnName="ShipCity" />
    </syncfusion:SfDataGrid.GroupColumnDescriptions>
</syncfusion:SfDataGrid>
```

```csharp
dataGrid.GroupingMode = GroupingMode.Multiple;
dataGrid.GroupColumnDescriptions.Add(new GroupColumnDescription { ColumnName = "ShipCountry" });
dataGrid.GroupColumnDescriptions.Add(new GroupColumnDescription { ColumnName = "ShipCity" });
```

### Sorting Records in Grouped Columns

```csharp
dataGrid.GroupColumnDescriptions.Add(new GroupColumnDescription()
{
    ColumnName = "OrderID",
    SortGroupRecords = false
});

dataGrid.GroupColumnDescriptions.Add(new GroupColumnDescription()
{
    ColumnName = "Freight",
    SortGroupRecords = true
});
```

### Display Based Grouping using GroupMode

```xml
<syncfusion:DataGridTextColumn HeaderText="Order ID"
                               MappingName="OrderID"
                               GroupMode="Display"
                               Format="#" />
```

```csharp
DataGridTextColumn orderID = new DataGridTextColumn();
orderID.MappingName = "OrderID";
orderID.GroupMode = DataReflectionMode.Display;
orderID.Format = "#";
```

### Group Expand/Collapse

```csharp
// Expand all groups
dataGrid.ExpandAllGroups();

// Collapse all groups
dataGrid.CollapseAllGroups();

// Expand specific group
var group = (dataGrid.View.Groups[0] as Group);
dataGrid.ExpandGroup(group);

// Collapse specific group
dataGrid.CollapseGroup(group);
```

### AllowGroupExpandCollapse

```xml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AllowGroupExpandCollapse="True"
                       ItemsSource="{Binding OrderInfoCollection}">
</syncfusion:SfDataGrid>
```

```csharp
dataGrid.AllowGroupExpandCollapse = true;
```

### Expand Groups Initially

```xml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AutoExpandGroups="True"
                       AllowGroupExpandCollapse="True"
                       ItemsSource="{Binding OrderInfoCollection}">
</syncfusion:SfDataGrid>
```

```csharp
dataGrid.AutoExpandGroups = true;
dataGrid.AllowGroupExpandCollapse = true;
```

### Grouping Events

#### GroupExpanding Event

```csharp
dataGrid.GroupExpanding += dataGrid_GroupExpanding;

private void dataGrid_GroupExpanding(object sender, DataGridColumnGroupChangingEventArgs e)
{
    // Cancel expansion for specific group
    if (e.Group.Key.Equals(1001))
    {
        e.Cancel = true;
    }
}
```

#### GroupExpanded Event

```csharp
dataGrid.GroupExpanded += dataGrid_GroupExpanded;

private void dataGrid_GroupExpanded(object sender, DataGridColumnGroupChangedEventArgs e)
{
    // Handle group expanded logic
}
```

#### GroupCollapsing Event

```csharp
dataGrid.GroupCollapsing += dataGrid_GroupCollapsing;

private void dataGrid_GroupCollapsing(object sender, DataGridColumnGroupChangingEventArgs e)
{
    // Cancel collapse for specific group
    if (e.Group.Key.Equals(1001))
    {
        e.Cancel = true;
    }
}
```

#### GroupCollapsed Event

```csharp
dataGrid.GroupCollapsed += dataGrid_GroupCollapsed;

private void dataGrid_GroupCollapsed(object sender, DataGridColumnGroupChangedEventArgs e)
{
    // Handle group collapsed logic
}
```

### Grouping Customizations

#### Customize Indent Column Width

```xml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       ItemsSource="{Binding OrderInfoCollection}"
                       IndentColumnWidth="60">
</syncfusion:SfDataGrid>
```

```csharp
dataGrid.IndentColumnWidth = 60;
```

#### Customize Grouped Column Visibility

```xml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       ItemsSource="{Binding OrderInfoCollection}"
                       ShowColumnWhenGrouped="False">
</syncfusion:SfDataGrid>
```

```csharp
dataGrid.ShowColumnWhenGrouped = false;
```

#### Customize Group Icon

```xml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       ItemsSource="{Binding OrderInfoCollection}"
                       AllowGroupExpandCollapse="True">
    <syncfusion:SfDataGrid.GroupExpandCollapseTemplate>
        <DataTemplate>
            <Image Source="expand_more.png" HeightRequest="24" WidthRequest="24"/>
        </DataTemplate>
    </syncfusion:SfDataGrid.GroupExpandCollapseTemplate>
</syncfusion:SfDataGrid>
```

```csharp
dataGrid.GroupExpandCollapseTemplate = new DataTemplate(() =>
{
    var imageView = new Image()
    {
        Source = "expand_more.png",
        Aspect = Aspect.AspectFit,
        HeightRequest = 24,
        WidthRequest = 24,
    };
    return imageView;
});
```

#### Customize Indent Column Background Color

```xml
<ContentPage.Resources>
    <Style TargetType="syncfusion:DataGridIndentCell">
        <Setter Property="Background" Value="LightGray"/>
    </Style>
</ContentPage.Resources>
```

### Custom Grouping
```xml
  <ContentPage.Resources>
        <ResourceDictionary>
            <local:GroupConverter x:Key="groupConverter" />
        </ResourceDictionary>
    </ContentPage.Resources>

    <ContentPage.BindingContext>
        <local:ViewModel x:Name="viewModel" />
    </ContentPage.BindingContext>

    <syncfusion:SfDataGrid x:Name="dataGrid"
                            ItemsSource="{Binding OrderInfoCollection}">

        <syncfusion:SfDataGrid.GroupColumnDescriptions>
            <syncfusion:GroupColumnDescription ColumnName="Freight"
                                                Converter="{StaticResource groupConverter}" />
        </syncfusion:SfDataGrid.GroupColumnDescriptions>
    </syncfusion:SfDataGrid>
```

```csharp
dataGrid.GroupColumnDescriptions.Add (new GroupColumnDescription () {
    ColumnName = "Freight",
    Converter = new GroupConverter()
});

public class GroupConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        var orderInfo = value as OrderInfo;
        if (orderInfo.Freight > 0 && orderInfo.Freight <= 250)
            return "<=250";
        else if (orderInfo.Freight > 250 && orderInfo.Freight <= 500)
            return ">250 & <=500";
        else if (orderInfo.Freight > 500 && orderInfo.Freight <= 750)
            return ">500 & <=750";
        else
            return ">1000";
    }

    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return null;
    }
}
```

## Summaries

### Table Summary

Display summaries at bottom of grid:

```xml
<sfGrid:SfDataGrid.TableSummaryRows>
        <sfGrid:DataGridTableSummaryRow Title="Total Salary :{TotalSalary} for {ProductCount} members"
                                        Position="Top"
                                        ShowSummaryInRow="True">
            <sfGrid:DataGridTableSummaryRow.SummaryColumns>
                <sfGrid:DataGridSummaryColumn Name="TotalSalary"
                                              Format="{}{Sum:C0}"
                                              MappingName="Salary"
                                              SummaryType="DoubleAggregate" />
                <sfGrid:DataGridSummaryColumn Name="ProductCount"
                                              Format="{}{Count}"
                                              MappingName="Salary"
                                              SummaryType="CountAggregate" />
            </sfGrid:DataGridTableSummaryRow.SummaryColumns>
        </sfGrid:DataGridTableSummaryRow>
        <sfGrid:DataGridTableSummaryRow Position="Top"
                                        ShowSummaryInRow="False">
            <sfGrid:DataGridTableSummaryRow.SummaryColumns>
                <sfGrid:DataGridSummaryColumn Name="TotalSalary"
                                              Format="{}{Sum:C0}"
                                              MappingName="Salary"
                                              SummaryType="DoubleAggregate" />
            </sfGrid:DataGridTableSummaryRow.SummaryColumns>
        </sfGrid:DataGridTableSummaryRow>
    </sfGrid:SfDataGrid.TableSummaryRows>
```

```csharp
dataGrid.TableSummaryRows.Add(new DataGridTableSummaryRow
{
    ShowSummaryInRow = false,
    Title = "Quantity:{TotalQuantity} for {AveragePrice}",
    SummaryColumns = new ObservableCollection<DataGridSummaryColumn>
    {
        new DataGridSummaryColumn
        {
            Name = "TotalQuantity",
            MappingName = "Quantity",
            SummaryType = SummaryType.DoubleAggregate,
            Format = "Total: {Sum}"
        },
        new DataGridSummaryColumn
        {
            Name = "AveragePrice",
            MappingName = "UnitPrice",
            SummaryType = SummaryType.DoubleAggregate,
            Format = "Avg: {Average:C2}"
        }
    }
});
```

### Built-in Aggregate Types

- `Sum` - Total of values
- `Average` - Mean value
- `Count` - Number of items
- `Min` - Minimum value
- `Max` - Maximum value

### Custom Summary

```xml
<sfGrid:SfDataGrid.TableSummaryRows>
        <sfGrid:DataGridTableSummaryRow Title="Standard Deviation:{TableSummary}"
                                        ShowSummaryInRow="True">
            <sfGrid:DataGridTableSummaryRow.SummaryColumns>
                <sfGrid:DataGridSummaryColumn Name="CustomTotal"
                                              CustomAggregate="{StaticResource customAggregate}"
                                              Format="Total Value: {AggregateValue:C2}"
                                              SummaryType="Custom" />
            </sfGrid:DataGridTableSummaryRow.SummaryColumns>
        </sfGrid:DataGridTableSummaryRow>
    </sfGrid:SfDataGrid.TableSummaryRows>
```

```csharp
public class CustomSummaryAggregate : ISummaryAggregate
{
    public Action<IEnumerable, string, PropertyInfo> CalculateAggregateFunc()
    {
        return (items, property, pd) =>
        {
            var enumerableItems = items.Cast<OrderInfo>();
            object value = enumerableItems.Sum(x => x.Quantity * x.UnitPrice);
            this.AggregateValue = value;
        };
    }
    
    public object AggregateValue { get; set; }
}

// Use custom summary
dataGrid.TableSummaryRows.Add(new DataGridTableSummaryRow
{
    SummaryColumns = new ObservableCollection<ISummaryColumn>
    {
        new DataGridSummaryColumn
        {
            Name = "CustomTotal",
            CustomAggregate = new CustomSummaryAggregate(),
            Format = "Total Value: {AggregateValue:C2}"
        }
    }
});
```

## Group Summaries

Display summaries for each group:

```csharp
// Group by country
dataGrid.GroupColumnDescriptions.Add(new GroupColumnDescription
{
    ColumnName = "ShipCountry"
});

// Add group summary
dataGrid.GroupSummaryRows.Add(new DataGridSummaryRow
{
    ShowSummaryInRow = false,
    SummaryColumns = new ObservableCollection<ISummaryColumn>
    {
        new DataGridSummaryColumn
        {
            Name = "GroupTotal",
            MappingName = "Freight",
            SummaryType = DataGridSummaryType.DoubleAggregate,
            Format = "Total: {Sum:C2}"
        }
    }
});
```

## Caption Summaries

Display summaries in group caption:

```xml
<sfGrid:SfDataGrid x:Name="dataGrid"
                       ItemsSource="{Binding OrderInfoCollection}"
                       ColumnWidthMode="Fill">
    <sfgrid:SfDataGrid.GroupColumnDescriptions>
            <sfgrid:GroupColumnDescription ColumnName="Salary" />
        </sfgrid:SfDataGrid.GroupColumnDescriptions>
            <syncfusion:SfDataGrid.CaptionSummaryRow>
                <syncfusion:DataGridSummaryRow Title="Total Salary :{TotalSalary}" ShowSummaryInRow="True">
                    <syncfusion:DataGridSummaryRow.SummaryColumns>
                        <syncfusion:DataGridSummaryColumn               Name="TotalSalary"
                            Format="{}{Sum:C0}"
                            MappingName="Salary"
                            SummaryType="Int32Aggregate" />
                    </syncfusion:DataGridSummaryRow.SummaryColumns>
                </syncfusion:DataGridSummaryRow>
            </syncfusion:SfDataGrid.CaptionSummaryRow>
    </sfGrid:SfDataGrid>
```

```csharp
dataGrid.GroupColumnDescriptions.Add(new GroupColumnDescription { ColumnName = "Salary" });

dataGrid.CaptionSummaryRow = new DataGridSummaryRow
{
    ShowSummaryInRow = true,
    Title = "Total Salary :{TotalSalary}",
    SummaryColumns = new ObservableCollection<ISummaryColumn>
    {
        new DataGridSummaryColumn
        {
            Name = "TotalSalary",
            MappingName = "Salary",
            SummaryType = DataGridSummaryType.DoubleAggregate,
            Format = "{Sum:c}"
        }
    }
};
```

## Common Patterns

### Pattern 1: Multi-Level Grouping with Summaries

```csharp
// Group by Country, then City
dataGrid.GroupColumnDescriptions.Add(new GroupColumnDescription { ColumnName = "ShipCountry" });
dataGrid.GroupColumnDescrip tions.Add(new GroupColumnDescription { ColumnName = "ShipCity" });

// Summary for each group
dataGrid.GroupSummaryRows.Add(new DataGridSummaryRow
{
    SummaryColumns = new ObservableCollection<ISummaryColumn>
    {
        new DataGridSummaryColumn
        {
            Name = "Count",
            MappingName = "OrderID",
            SummaryType = DataGridSummaryType.CountAggregate,
            Format = "{Count} Orders"
        }
    }
});
```

## Next Steps

- Read [sorting-filtering.md](sorting-filtering.md) for data operations
- Read [advanced-features.md](advanced-features.md) for more features
