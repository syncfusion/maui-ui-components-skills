# Columns

## Table of Contents
-[Automatic Column Generation](#automatic-column-generation)
- [Manual Column Definition](#manual-column-definition)
- [Column Types](#column-types)
- [Column Properties](#column-properties)
- [Column Sizing](#column-sizing)
- [Column Manipulation](#column-manipulation)
- [Data Annotations](#data-annotations)
- [Troubleshooting](#troubleshooting)

## Automatic Column Generation

The DataGrid automatically creates columns based on the `AutoGenerateColumnsMode` property.

### Auto Generation Modes

| Mode | Description |
|------|-------------|
| `None` | Only manually defined columns are shown. Sorting retained for explicit columns when Item Source changes. |
| `Reset` | **Default.** Retains explicit columns + generates new columns for other properties. Sorting retained for explicit columns. |
| `ResetAll` | Clears all columns and generates new ones. Ignores explicit columns. Sorting cleared. |
| `RetainOld` | Creates columns if none exist; retains explicit columns if defined. Sorting retained. |
| `SmartReset` | Retains explicit columns +columns with matching MappingName. Creates new for others. Sorting retained for matching columns. |

###Usage:

```xml
<syncfusion:SfDataGrid ItemsSource="{Binding Orders}"
                       AutoGenerateColumnsMode="Reset" />
```

```csharp
dataGrid.AutoGenerateColumnsMode = AutoGenerateColumnsMode.Reset;
```

### Auto-Generated Column Types

Based on property types:

| .NET Type | Column Type |
|-----------|-------------|
| `string`, `object` | DataGridTextColumn |
| `int`, `float`, `double`, `decimal` (+ nullable) | DataGridNumericColumn |
| `DateTime` | DataGridDateColumn |
| `bool` | DataGridCheckBoxColumn |
| `ImageSource` | DataGridImageColumn |

### Customize Auto-Generated Columns

Use the `AutoGeneratingColumn` event:

```xml
<syncfusion:SfDataGrid AutoGeneratingColumn="DataGrid_AutoGeneratingColumn" />
```

```csharp
private void DataGrid_AutoGeneratingColumn(object sender, DataGridAutoGeneratingColumnEventArgs e)
{
    // Skip a column
    if (e.Column.MappingName == "InternalID")
    {
        e.Cancel = true;
        return;
    }
    
    // Apply formatting
    if (e.Column.MappingName == "Freight")
    {
        e.Column.Format = "C2"; // Currency with 2 decimals
        e.Column.HeaderText = "Shipping Cost";
    }
    
    // Change column width
    if (e.Column.MappingName == "OrderDate")
    {
        e.Column.Width = 150;
        e.Column.Format = "MM/dd/yyyy";
    }
}
```

### Auto-Generate for Custom Types

Control generation for complex properties:

```xml
<syncfusion:SfDataGrid AutoGenerateColumnsModeForCustomType="Both" />
```

Options:
- `None` - Don't generate for custom types
- `Parent` - Generate only parent property
- `Child` - Generate only child properties
- `Both` - Generate both parent and children

## Manual Column Definition

Set `AutoGenerateColumnsMode="None"` and define columns explicitly:

```xml
<syncfusion:SfDataGrid ItemsSource="{Binding Orders}"
                       AutoGenerateColumnsMode="None">
    <syncfusion:SfDataGrid.Columns>
        <syncfusion:DataGridNumericColumn MappingName="OrderID" 
                                          HeaderText="Order #" 
                                          Width="100"/>
        <syncfusion:DataGridTextColumn MappingName="CustomerID" 
                                       HeaderText="Customer" 
                                       Width="150"/>
        <syncfusion:DataGridTextColumn MappingName="ShipCountry" 
                                       HeaderText="Country"/>
        <syncfusion:DataGridNumericColumn MappingName="Freight" 
                                          Format="C2"
                                          HeaderText="Shipping Cost"/>
        <syncfusion:DataGridDateColumn MappingName="OrderDate" 
                                       Format="MM/dd/yyyy"
                                       HeaderText="Order Date"/>
    </syncfusion:SfDataGrid.Columns>
</syncfusion:SfDataGrid>
```

**Code-Behind:**

```csharp
dataGrid.AutoGenerateColumnsMode = AutoGenerateColumnsMode.None;

dataGrid.Columns.Add(new DataGridNumericColumn
{
    MappingName = "OrderID",
    HeaderText = "Order ID",
    Width = 100
});

dataGrid.Columns.Add(new DataGridTextColumn
{
    MappingName = "CustomerID",
    HeaderText = "Customer",
    Width = 150
});
```

## Column Types

### 1. DataGridTextColumn

Display text or string data:

```xml
<syncfusion:DataGridTextColumn MappingName="CustomerName" 
                               HeaderText="Customer" />
```

### 2. DataGridNumericColumn

Display numeric data (int, float, double, decimal):

```xml
<syncfusion:DataGridNumericColumn MappingName="OrderID" 
                                  HeaderText="Order #" />
<syncfusion:DataGridNumericColumn MappingName="Freight" 
                                  Format="C2"
                                  HeaderText="Cost" />
```

### 3. DataGridDateColumn

Display date/time values:

```xml
<syncfusion:DataGridDateColumn MappingName="OrderDate" 
                               Format="MM/dd/yyyy"
                               HeaderText="Order Date" />
```

Common formats:
- `"d"` - Short date (1/15/2024)
- `"D"` - Long date (Monday, January 15, 2024)
- `"MM/dd/yyyy"` - Custom format
- `"yyyy-MM-dd"` - ISO format

### 4. DataGridCheckBoxColumn

Display boolean values as checkboxes:

```xml
<syncfusion:DataGridCheckBoxColumn MappingName="IsActive" 
                                   HeaderText="Active" />
```

**Note:** By default, checkboxes are read-only. Enable editing:

```xml
<syncfusion:SfDataGrid AllowEditing="True">
    <syncfusion:SfDataGrid.Columns>
        <syncfusion:DataGridCheckBoxColumn MappingName="IsActive" 
                                           AllowEditing="True" />
    </syncfusion:SfDataGrid.Columns>
</syncfusion:SfDataGrid>
```

### 5. DataGridImageColumn

Display images from ImageSource:

```xml
<syncfusion:DataGridImageColumn MappingName="ProductImage" 
                                HeaderText="Photo"
                                Width="80" />
```

Model property:
```csharp
public ImageSource ProductImage { get; set; }
```

### 6. DataGridComboBoxColumn

Display dropdown selection:

```xml
<syncfusion:DataGridComboBoxColumn MappingName="Country"
                                   HeaderText="Country"
                                   ItemsSource="{Binding Countries}" />
```

### 7. DataGridPickerColumn

Display picker for selection:

```xml
<syncfusion:DataGridPickerColumn MappingName="Status"
                                 HeaderText="Order Status"
                                 ItemsSource="{Binding StatusList}" />
```

### 8. DataGridTemplateColumn

Custom column with templates:

```xml
<syncfusion:DataGridTemplateColumn MappingName="ProductInfo" 
                                    HeaderText="Product">
    <syncfusion:DataGridTemplateColumn.CellTemplate>
        <DataTemplate>
            <StackLayout Orientation="Horizontal" Padding="5">
                <Image Source="{Binding ProductImage}" 
                       HeightRequest="40" 
                       WidthRequest="40" />
                <Label Text="{Binding ProductName}" 
                       VerticalOptions="Center" 
                       Margin="10,0,0,0" />
            </StackLayout>
        </DataTemplate>
    </syncfusion:DataGridTemplateColumn.CellTemplate>
</syncfusion:DataGridTemplateColumn>
```

## Column Properties

### MappingName (Required)

Binds column to a property:

```xml
<syncfusion:DataGridTextColumn MappingName="CustomerID" />
```

For complex properties:
```xml
<syncfusion:DataGridTextColumn MappingName="Customer.Name" />
```

For dynamic properties:
```xml
<syncfusion:DataGridTextColumn MappingName="[PropertyName]" />
```

### HeaderText

Custom header text:

```xml
<syncfusion:DataGridNumericColumn MappingName="OrderID" 
                                  HeaderText="Order Number" />
```

If not set, uses `MappingName`.

### Width

Set specific column width:

```xml
<syncfusion:DataGridTextColumn MappingName="CustomerID" 
                               Width="150" />
```

### MinimumWidth / MaximumWidth

Constrain column width:

```xml
<syncfusion:DataGridTextColumn MappingName="Comments" 
                               MinimumWidth="100"
                               MaximumWidth="300" />
```

### Format

Apply formatting to data:

```xml
<!-- Currency -->
<syncfusion:DataGridNumericColumn MappingName="Price" 
                                  Format="C" />

<!-- Percentage -->
<syncfusion:DataGridNumericColumn MappingName="DiscountRate" 
                                  Format="P2" />

<!-- Custom date -->
<syncfusion:DataGridDateColumn MappingName="OrderDate" 
                               Format="MMM dd, yyyy" />

<!-- Custom numeric -->
<syncfusion:DataGridNumericColumn MappingName="Quantity" 
                                  Format="N0" />
```

### CultureInfo

Apply culture-specific formatting:

```csharp
dataGrid.Columns.Add(new DataGridNumericColumn
{
    MappingName = "Price",
    Format = "C",
    CultureInfo = new CultureInfo("en-US") // $1,234.56
});

dataGrid.Columns.Add(new DataGridNumericColumn
{
    MappingName = "Price",
    Format = "C",
    CultureInfo = new CultureInfo("fr-FR") // 1 234,56 €
});
```

### TextAlignment

Align cell and header text:

```xml
<syncfusion:DataGridNumericColumn MappingName="OrderID"
                                  CellTextAlignment="End"
                                  HeaderTextAlignment="Center" />
```

Options: `Start`, `Center`, `End`

**Default alignment:**
- Numeric/Date columns: Right (End)
- Text columns: Left (Start)

### Visible

Show/hide column:

```xml
<syncfusion:DataGridTextColumn MappingName="InternalNotes" 
                               Visible="False" />
```

```csharp
// Toggle visibility
dataGrid.Columns["InternalNotes"].Visible = false;
```

### AllowEditing

Control editing per column:

```xml
<syncfusion:DataGridNumericColumn MappingName="OrderID" 
                                  AllowEditing="False" />
```

### AllowSorting / AllowFiltering

Control sorting/filtering per column:

```xml
<syncfusion:DataGridTextColumn MappingName="Comments" 
                               AllowSorting="True"
                               AllowFiltering="False" />
```

### CellPadding / HeaderPadding

Add padding to cells:

```xml
<syncfusion:DataGridTextColumn MappingName="ProductName"
                               CellPadding="10,5,10,5"
                               HeaderPadding="10,8,10,8" />
```

### HeaderTemplate

Custom header design:

```xml
<syncfusion:DataGridTextColumn MappingName="Status">
    <syncfusion:DataGridTextColumn.HeaderTemplate>
        <DataTemplate>
            <StackLayout Orientation="Horizontal">
                <Image Source="status_icon.png" 
                       HeightRequest="20" 
                       WidthRequest="20" />
                <Label Text="Status" 
                       VerticalOptions="Center" 
                       Margin="5,0,0,0"/>
            </StackLayout>
        </DataTemplate>
    </syncfusion:DataGridTextColumn.HeaderTemplate>
</syncfusion:DataGridTextColumn>
```

### CellTemplate

Custom cell design:

```xml
<syncfusion:DataGridTextColumn MappingName="Priority">
    <syncfusion:DataGridTextColumn.CellTemplate>
        <DataTemplate>
            <Label Text="{Binding Priority}" 
                   TextColor="White"
                   BackgroundColor="{Binding Priority, Converter={StaticResource PriorityColorConverter}}"
                   HorizontalOptions="FillAndExpand"
                   VerticalOptions="FillAndExpand"
                   HorizontalTextAlignment="Center"
                   VerticalTextAlignment="Center" />
        </DataTemplate>
    </syncfusion:DataGridTextColumn.CellTemplate>
</syncfusion:DataGridTextColumn>
```

**Reusable CellTemplate:**

Set `SetCellBoundValue="True"` to use one template for multiple columns:

```xml
<ContentPage.Resources>
    <DataTemplate x:Key="cellTemplate">
        <Label Text="{Binding Path=Value}" 
               HorizontalOptions="Center" 
               VerticalOptions="Center" />
    </DataTemplate>
</ContentPage.Resources>

<syncfusion:DataGridTextColumn MappingName="CustomerID"
                               SetCellBoundValue="True"
                               CellTemplate="{StaticResource cellTemplate}" />
<syncfusion:DataGridTextColumn MappingName="ProductName"
                               SetCellBoundValue="True"
                               CellTemplate="{StaticResource cellTemplate}" />
```

When `SetCellBoundValue="True"`:
- `Value` - Mapped property value
- `Record` - Entire data object

### LineBreakMode

Control text wrapping in cells:

```xml
<syncfusion:DataGridTextColumn MappingName="Description"
                               LineBreakMode="WordWrap" />
```

Options: `NoWrap`, `WordWrap`, `CharacterWrap`, `HeadTruncation`, `TailTruncation`, `MiddleTruncation`

**Note:** Truncation modes don't work on Windows.

### HeaderLineBreakMode

Control text wrapping in headers:

```xml
<syncfusion:DataGridTextColumn MappingName="LongHeaderName"
                               HeaderLineBreakMode="WordWrap" />
```

## Column Sizing

### ColumnWidthMode Options

Control how column widths are calculated:

| Mode | Description |
|------|-------------|
| `None` | Uses `Width` or `DefaultColumnWidth` |
| `Auto` | Fits based on header AND cell content |
| `Fill` | Divides total width equally |
| `FitByHeader` | Fits based on header content only |
| `FitByCell` | Fits based on cell content only |
| `LastColumnFill` | Uses default width; last column fills remaining space |

### Grid-Level Sizing

Apply to all columns:

```xml
<syncfusion:SfDataGrid ColumnWidthMode="Auto" />
```

### Column-Level Sizing

Override for specific column:

```xml
<syncfusion:SfDataGrid ColumnWidthMode="None">
    <syncfusion:SfDataGrid.Columns>
        <syncfusion:DataGridTextColumn MappingName="CustomerID"
                                       ColumnWidthMode="Auto" />
        <syncfusion:DataGridTextColumn MappingName="Comments"
                                       ColumnWidthMode="Fill" />
    </syncfusion:SfDataGrid.Columns>
</syncfusion:SfDataGrid>
```

### Default Column Width

Set common width for all columns:

```xml
<syncfusion:SfDataGrid DefaultColumnWidth="120" />
```

### Refresh Column Sizer

Recalculate column widths at runtime:

```csharp
// After data changes
dataGrid.ColumnSizer.Refresh(true);
```

### Get Actual Width

Retrieve calculated width:

```csharp
double width = dataGrid.Columns["OrderID"].ActualWidth;
```

## Column Manipulation

### Add Column

```csharp
dataGrid.Columns.Add(new DataGridTextColumn
{
    MappingName = "NewColumn",
    HeaderText = "New Column"
});
```

### Access Column

By index:
```csharp
DataGridColumn column = dataGrid.Columns[0];
```

By MappingName:
```csharp
DataGridColumn column = dataGrid.Columns["OrderID"];
```

### Remove Column

```csharp
// By object
dataGrid.Columns.Remove(column);

// By index
dataGrid.Columns.RemoveAt(0);
```

### Clear All Columns

```csharp
dataGrid.Columns.Clear();
```

### Column Chooser

Allow users to show/hide columns:

```xml
<syncfusion:SfDataGrid ShowColumnChooser="True" />
```

## Data Annotations

### Exclude Column

```csharp
[Display(AutoGenerateField = false)]
public int InternalID { get; set; }
```

### Custom Header Text

```csharp
[Display(Name = "Customer Name")]
public string CustomerID { get; set; }
```

### Column Order

```csharp
[Display(Order = 0)]
public int OrderID { get; set; }

[Display(Order = 1)]
public string CustomerID { get; set; }
```

### Read-Only Column

```csharp
[ReadOnly(true)]
public string Country { get; set; }
```

### Enable Editing

```csharp
[Editable(true)]
public string Status { get;set; }
```

### Format Column

```csharp
[DisplayFormat(DataFormatString = "yyyy")]
public DateTime OrderDate { get; set; }

[DisplayFormat(DataFormatString = "Country: {0}")]
public string Country { get; set; }
```

### Group Under Stacked Header

```csharp
[Display(GroupName = "Order Details")]
public string OrderID { get; set; }

[Display(GroupName = "Order Details")]
public DateTime OrderDate { get; set; }
```

## Troubleshooting

### Columns Not Appearing

**Problem:** Grid shows no columns.

**Solutions:**
- Check `AutoGenerateColumnsMode` (should be `Reset` or `ResetAll` for auto-generation)
- Verify `ItemsSource` has data
- For manual columns, set `AutoGenerateColumnsMode="None"`
- Check `MappingName` matches property names exactly (case-sensitive)

### Wrong Column Type Generated

**Problem:** Numeric data shows in text column.

**Solutions:**
- Ensure property type matches (use `int`, `decimal`, not `string`)
- Use `AutoGeneratingColumn` event to change column type:

```csharp
private void DataGrid_AutoGeneratingColumn(object sender, DataGridAutoGeneratingColumnEventArgs e)
{
    if (e.Column.MappingName == "Quantity")
    {
        e.Column = new DataGridNumericColumn { MappingName = "Quantity" };
    }
}
```

### Format Not Applied

**Problem:** Format string doesn't work.

**Solutions:**
- Use correct format specifiers (`C` for currency, `N` for number, `P` for percent)
- For auto-generated columns, use `AutoGeneratingColumn` event
- Check property type matches format (numeric formats need numeric types)

### Column Width Issues

**Problem:** Columns too narrow or wide.

**Solutions:**
- Set `ColumnWidthMode="Auto"` to fit content
- Set explicit `Width` for specific columns
- Use `MinimumWidth` and `MaximumWidth` to constrain
- Call `dataGrid.ColumnSizer.Refresh(true)` after data changes

### Template Not Showing

**Problem:** CellTemplate or HeaderTemplate not displayed.

**Solutions:**
- Check DataTemplate syntax
- Verify binding paths
- For `CellTemplate`, ensure column type supports it (not CheckBox, Image, Unbound columns)
- Set `SetCellBoundValue="True"` for reusable templates

### Dynamic Properties Not Working

**Problem:** ExpandoObject properties don't show.

**Solutions:**
- Add square brackets: `MappingName="[PropertyName]"`
- Or use `AutoGeneratingColumn` event:

```csharp
private void DataGrid_AutoGeneratingColumn(object sender, DataGridAutoGeneratingColumnEventArgs e)
{
    e.Column.MappingName = "[" + e.Column.MappingName + "]";
}
```

### Complex Properties Not Binding

**Problem:** Nested properties don't display.

**Solutions:**
- Use dot notation: `MappingName="Customer.Name"`
- For indexers/dictionaries, set `UseBindingValue="True"`

## Next Steps

- Read [column-operations.md](column-operations.md) for resizing, drag-drop, and freeze panes
- Read [editing.md](editing.md) to enable cell editing
- Read [styling-customization.md](styling-customization.md) for advanced styling
