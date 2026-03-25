# Advanced Features

## Table of Contents
- [Master-Details View](#master-details-view)
  - [Key Features](#key-features)
  - [Auto-Generating Relations](#auto-generating-relations)
  - [Manually Defining Relations](#manually-defining-relations)
  - [Expand and Collapse DetailsViewDataGrid Programmatically](#expand-and-collapse-detailsviewdatagrid-programmatically)
  - [Appearance Customization](#appearance-customization)
  - [Hide Empty Grid Definitions](#hide-empty-grid-definitions)
  - [Hide Indent Cells in Details View](#hide-indent-cells-in-details-view)
  - [Master-Details View Events](#master-details-view-events)
  - [Getting DetailsViewDataGrid Selection](#getting-detailsviewdatagrid-selection)
- [Record Template View](#record-template-view)
  - [Defining Row Template](#defining-row-template)
  - [TemplateViewDefinition.HeightMode Property](#templateviewdefinitionheightmode-property)
  - [Programmatic Expand/Collapse](#programmatic-expandcollapse)
  - [DetailsViewExpanding Event](#detailsviewexpanding-event)
  - [DetailsViewCollapsing Event](#detailsviewcollapsing-event)
  - [DetailsViewExpanded Event](#detailsviewexpanded-event)
  - [DetailsViewCollapsed Event](#detailsviewcollapsed-event)
- [Empty View](#empty-view)
- [Context Menu](#context-menu)
- [Tooltips](#tooltips)
- [Merged Cells](#merged-cells)
- [Serialization](#serialization)
- [Conditional Styling](#conditional-styling)

## Master-Details View

The SfDataGrid supports displaying hierarchical data using [Master-Details View](https://help.syncfusion.com/cr/maui/Syncfusion.Maui.DataGrid.DetailsViewDataGrid.html), allowing you to represent parent-child relationships in a structured format. This feature enables nesting of multiple levels of related data within the grid.

### Key Features
- Display hierarchical data in a structured format using nested tables
- Expand or collapse DetailsViewDataGrid using an expander or programmatically
- Support for unlimited nesting levels with relational data
- Support for both IEnumerable and DataTable relations

### Auto-Generating Relations

```xml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AutoGenerateRelations="True"
                       ItemsSource="{Binding Employees}">
</syncfusion:SfDataGrid>
```

```csharp
dataGrid.AutoGenerateRelations = true;
```

### Manually Defining Relations

```xml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AutoGenerateRelations="False"
                       ItemsSource="{Binding Employees}">
    <syncfusion:SfDataGrid.DetailsViewDefinition>
        <syncfusion:DataGridViewDefinition RelationalColumn="Sales">
            <syncfusion:DataGridViewDefinition.DataGrid>
                <syncfusion:SfDataGrid ItemsSource="{Binding Sales}" />
            </syncfusion:DataGridViewDefinition.DataGrid>
        </syncfusion:DataGridViewDefinition>
        <syncfusion:DataGridViewDefinition RelationalColumn="Orders">
            <syncfusion:DataGridViewDefinition.DataGrid>
                <syncfusion:SfDataGrid ItemsSource="{Binding Orders}" />
            </syncfusion:DataGridViewDefinition.DataGrid>
        </syncfusion:DataGridViewDefinition>
    </syncfusion:SfDataGrid.DetailsViewDefinition>
</syncfusion:SfDataGrid>
```

```csharp
dataGrid.AutoGenerateRelations = false;

var gridViewDefinition1 = new DataGridViewDefinition();
gridViewDefinition1.RelationalColumn = "Sales";
gridViewDefinition1.DataGrid = new SfDataGrid();

var gridViewDefinition2 = new DataGridViewDefinition();
gridViewDefinition2.RelationalColumn = "Orders";
gridViewDefinition2.DataGrid = new SfDataGrid();

dataGrid.DetailsViewDefinition.Add(gridViewDefinition1);
dataGrid.DetailsViewDefinition.Add(gridViewDefinition2);
```

### Expand and Collapse DetailsViewDataGrid Programmatically

```csharp
// Expand all DetailsViewDataGrid
dataGrid.ExpandAllDetailsView();

// Collapse all DetailsViewDataGrid
dataGrid.CollapseAllDetailsView();

// Expand all up to specific level
dataGrid.ExpandAllDetailsView(2);

// Expand specific row by index
dataGrid.ExpandDetailsViewAt(0);

// Collapse specific row by index
dataGrid.CollapseDetailsViewAt(0);

// Get DetailsViewDataGrid by row index
var detailsViewDataGrid = dataGrid.GetDetailsViewGrid(2);
```

### Appearance Customization

#### Customize Header Appearance

```xml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AutoGenerateRelations="True"
                       ItemsSource="{Binding Employees}">
    <syncfusion:SfDataGrid.DetailsViewDefaultStyle>
        <syncfusion:DataGridStyle HeaderRowBackground="#0074E3"
                                  HeaderRowTextColor="White"/>
    </syncfusion:SfDataGrid.DetailsViewDefaultStyle>
</syncfusion:SfDataGrid>
```

#### Customize DetailsViewPadding

The padding of DetailsViewDataGrid can be customized using the DetailsViewPadding property.

```xml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AutoGenerateRelations="True"
                       ItemsSource="{Binding Employees}"
                       DetailsViewPadding="15">
</syncfusion:SfDataGrid>
```

```csharp
dataGrid.DetailsViewPadding = new Thickness(15);
```

#### Customize ExpanderColumn Width

```xml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AutoGenerateRelations="True"
                       ItemsSource="{Binding Employees}"
                       ExpanderColumnWidth="50">
</syncfusion:SfDataGrid>
```

```csharp
dataGrid.ExpanderColumnWidth = 50;
```

#### Hide Header Row of Master-Details View

```csharp
var detailsViewDataGrid = dataGrid.GetDetailsViewGrid(0);
detailsViewDataGrid.HeaderRowHeight = 0;
```

### Hide Empty Grid Definitions

```xml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AutoGenerateRelations="True"
                       ItemsSource="{Binding Employees}"
                       HideEmptyDataGridViewDefinition="True">
</syncfusion:SfDataGrid>
```

```csharp
dataGrid.HideEmptyDataGridViewDefinition = true;
```

### Hide Indent Cells in Details View

```xml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AutoGenerateRelations="True"
                       ItemsSource="{Binding Employees}"
                       ShowDetailsViewIndentCell="False">
</syncfusion:SfDataGrid>
```

```csharp
dataGrid.ShowDetailsViewIndentCell = false;
```

### Master-Details View Events

#### DetailsViewExpanding Event

```csharp
dataGrid.DetailsViewExpanding += dataGrid_DetailsViewExpanding;

private void dataGrid_DetailsViewExpanding(object sender, DataGridDetailsViewExpandingEventArgs e)
{
    // Cancel expansion for specific records
    if ((e.Record as Employee).EmployeeID == 1002)
    {
        e.Cancel = true;
    }
}
```

#### DetailsViewExpanded Event

```csharp
dataGrid.DetailsViewExpanded += dataGrid_DetailsViewExpanded;

private void dataGrid_DetailsViewExpanded(object sender, DataGridDetailsViewExpandedEventArgs e)
{
    // Handle expanded logic
}
```

#### DetailsViewCollapsing Event

```csharp
dataGrid.DetailsViewCollapsing += dataGrid_DetailsViewCollapsing;

private void dataGrid_DetailsViewCollapsing(object sender, DataGridDetailsViewCollapsingEventArgs e)
{
    // Cancel collapse for specific records
    if ((e.Record as Employee).EmployeeID == 1001)
    {
        e.Cancel = true;
    }
}
```

#### DetailsViewCollapsed Event

```csharp
dataGrid.DetailsViewCollapsed += dataGrid_DetailsViewCollapsed;

private void dataGrid_DetailsViewCollapsed(object sender, DataGridDetailsViewCollapsedEventArgs e)
{
    // Handle collapsed logic
}
```

### Getting DetailsViewDataGrid Selection

```csharp
// Get the currently selected DetailsViewDataGrid
var detailsViewDataGrid = dataGrid.SelectedDetailsViewDataGrid;

// Get DetailsViewDataGrid by row index
var detailsGrid = dataGrid.GetDetailsViewGrid(2);

// Get selected row, rows and index
int selectedIndex = detailsGrid.SelectedIndex;
var selectedRow = detailsGrid.SelectedRow;
var selectedRows = detailsGrid.SelectedRows;
```

## Record Template View

### Defining Row Template

```xml
<syncfusion:SfDataGrid ItemsSource="{Binding Employees}">
    <syncfusion:SfDataGrid.DetailsViewDefinition>
        <syncfusion:TemplateViewDefinition x:Name="Template"
                                           HeightMode="Auto">
            <syncfusion:TemplateViewDefinition.RowTemplate>
                <DataTemplate>
                    <Grid BackgroundColor="AliceBlue">
                        <Label Text="{Binding Data.Name}" />
                    </Grid>
                </DataTemplate>
            </syncfusion:TemplateViewDefinition.RowTemplate>
        </syncfusion:TemplateViewDefinition>
    </syncfusion:SfDataGrid.DetailsViewDefinition>
</syncfusion:SfDataGrid>
```

### TemplateViewDefinition.HeightMode Property

**Height Mode Options:**

- **Auto** - Arranges template for the actual size as the RowTemplate is measured.
- **Fixed** - Arranges template for the specified height in [TemplateViewDefinition.Height](https://help.syncfusion.com/cr/maui/Syncfusion.Maui.DataGrid.TemplateViewDefinition.html#Syncfusion_Maui_DataGrid_TemplateViewDefinition_Height).
- **ViewportHeight** - Arranges template for the ViewPortHeight when the RowTemplate actual height is greater than ViewPortHeight.

```xml
<syncfusion:SfDataGrid ItemsSource="{Binding Employees}">
    <syncfusion:SfDataGrid.DetailsViewDefinition>
        <syncfusion:TemplateViewDefinition x:Name="Template"
                                           HeightMode="Fixed"
                                           Height="200">
            <syncfusion:TemplateViewDefinition.RowTemplate>
                <DataTemplate>
                    <Label Text="{Binding Data.Name}" />
                </DataTemplate>
            </syncfusion:TemplateViewDefinition.RowTemplate>
        </syncfusion:TemplateViewDefinition>
    </syncfusion:SfDataGrid.DetailsViewDefinition>
</syncfusion:SfDataGrid>
```

```csharp
// Auto height
var templateViewDef = new TemplateViewDefinition();
templateViewDef.HeightMode = DetailsViewMode.Auto;

// Fixed height
templateViewDef.HeightMode = DetailsViewMode.Fixed;
templateViewDef.Height = 200;

// Viewport height
templateViewDef.HeightMode = DetailsViewMode.ViewportHeight;
```

### Programmatic Expand/Collapse

```csharp
// Expand all row templates
dataGrid.ExpandAllDetailsView();

// Collapse all row templates
dataGrid.CollapseAllDetailsView();

// Expand specific row template by index
dataGrid.ExpandDetailsViewAt(1);

// Collapse specific row template by index
dataGrid.CollapseDetailsViewAt(1);
```

## Empty View

Display when no data:

```xml
<syncfusion:SfDataGrid ItemsSource="{Binding OrderInfoCollection}">
    <syncfusion:SfDataGrid.EmptyView>
        <StackLayout HorizontalOptions="Center" VerticalOptions="Center">
            <Label Text="No records" FontSize="14"/>
        </StackLayout>
    </syncfusion:SfDataGrid.EmptyView>                   
</syncfusion:SfDataGrid>
```

## Context Menu

Show context menu to the header cell:

```xml
<syncfusion:SfDataGrid x:Name="dataGrid" ItemsSource="{Binding Orders}">
    <syncfusion:SfDataGrid.HeaderContextMenu>
        <syncfusion:MenuItemCollection>
            <syncfusion:MenuItem Text="Sort Ascending"/>
            <syncfusion:MenuItem Text="Sort Descending"/>
            <syncfusion:MenuItem Text="Clear Sorting"/>
            <syncfusion:MenuItem Text="Group by Column"/>
            <syncfusion:MenuItem Text="Best Fit"/>
        </syncfusion:MenuItemCollection>
    </syncfusion:SfDataGrid.HeaderContextMenu>
</syncfusion:SfDataGrid>
```

Show content menu to the data cell:

```xml
<syncfusion:SfDataGrid x:Name="dataGrid" ItemsSource="{Binding Orders}">
    <syncfusion:SfDataGrid.RecordContextMenu>
        <syncfusion:MenuItemCollection>
            <syncfusion:MenuItem Text="Copy"/>
            <syncfusion:MenuItem Text="Paste"/>
            <syncfusion:MenuItem Text="Cut"/>
            <syncfusion:MenuItem Text="Delete"/>
        </syncfusion:MenuItemCollection>
    </syncfusion:SfDataGrid.RecordContextMenu>
</syncfusion:SfDataGrid>
```

## Tooltips

Show tooltips on cells:

```xml
<syncfusion:SfDataGrid ItemsSource="{Binding OrdersInfo}"
                              ShowToolTip="True" />
```

```csharp
dataGrid.CellToolTipOpening += DataGrid_CellToolTipOpening;

private void DataGrid_CellToolTipOpening(object sender, DataGridCellToolTipOpeningEventArgs e)
{
    // e.Column - Gets the GridColumn of the cell for which the tooltip is being shown.
    // e.RowData - Gets the data associated with a specific row.
    // e.RowColumnIndex - Gets the row and column index of the cell.
    // e.ToolTipText - Gets the text content that is displayed within the tooltip.
}
```

## Merged Cells

Merge cells with same values:

```csharp

dataGrid.QueryCoveredRange += dataGrid_QueryCoveredRange;

private void dataGrid_QueryCoveredRange(object sender, DataGridQueryCoveredRangeEventArgs e)
{
     if (e.RowColumnIndex.ColumnIndex == 1)
    {
        if (e.RowColumnIndex.RowIndex >= 1 && e.RowColumnIndex.RowIndex <= 5)
        {
            e.Range = new CoveredCellInfo(1, 1, 1, 5);
            e.Handled = true;
        }
    }
}
```

## Serialization

Save/load grid state:

```csharp
string localPath = Path.Combine(FileSystem.AppDataDirectory, "DataGrid.xml");

// Serialize
using (var file = File.Create(localPath))
{
    DataGridSerializationOptions options = new DataGridSerializationOptions();
    options.SerializeSorting = false;
    options.SerializeGrouping = false;
    options.SerializeColumns = false;
    options.SerializeCaptionSummary = false;
    options.SerializeGroupSummaries = false;
    options.SerializeTableSummaries = false;
    options.SerializeStackedHeaders = false;
    options.SerializeDetailsViewDefinition = false;
    options.SerializeUnboundRows = false;
    dataGrid.Serialize(file,options);
}

// Deserialize
using (var file = File.Open(localPath, FileMode.Open))
{
    DataGridDeserializationOptions options = new DataGridDeserializationOptions();
    options.DeserializeSorting = false;
    options.DeserializeGrouping = false;
    options.DeserializeColumns = false;
    options.DeserializeCaptionSummary = false;
    options.DeserializeGroupSummaries = false;
    options.DeserializeTableSummaries = false;
    options.DeserializeStackedHeaders = false;
    options.DeserializeDetailsViewDefinition = false;
    options.DeserializeUnboundRows = false;
    dataGrid.Deserialize(file,options);
}

```

## Conditional Styling

Style cells based on data:

```xml
<ContentPage xmlns:syncfusion="http://schemas.syncfusion.com/maui">
    <ContentPage.Resources>
        <local:ColorConverter x:Key="converter"/>
        <Style TargetType="syncfusion:DataGridCell">
            <Setter Property="Background" Value="{Binding OrderID, Converter={StaticResource converter}}"/>
        </Style>
    </ContentPage.Resources>
</ContentPage>
```

```csharp

public class ColorConverter : IValueConverter
{
    object IValueConverter.Convert(object value, Type targetType, object parameter, CultureInfo info)
    {
        if ((int)value < 1006)
            return Colors.LightBlue;
        else
            return Colors.White;
    }
    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        throw new NotImplementedException();
    }
}
```

## Next Steps

- Read [styling-customization.md](styling-customization.md) for styling
- Read [performance-events.md](performance-events.md) for events
