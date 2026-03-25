# AI-Powered Features in Smart DataGrid

## Overview

The Smart DataGrid supports natural language commands to perform complex grid operations without manual UI interaction. Users simply type requests like "Sort by customer name" or "Filter for Germany orders" and the AI intelligently interprets the command and applies it to the grid.

## Sorting

Sorting organizes data by one or more columns in ascending or descending order. The AI understands directional keywords and can apply multi-column sorting in a single command.

### Single Column Sorting

Sort a grid by one column:

```
sort by City ascending
sort by OrderID descending
sort by Customer
```

**Result:** Grid reorders rows based on the specified column and direction.

### Multi-Column Sorting

Sort by multiple columns simultaneously for hierarchical organization:

```
sort by ShipCountry ascending and Customer descending
sort by Category and OrderID
```

**Result:** Primary sort by first column, then secondary sort by second column, etc.

### Clear Sorting

Remove all sorting to return to original order:

```
clear sorting
clear sort
```

**Result:** Grid returns to unsorted state.

### Code Example

```csharp
// Programmatically apply sorting
private async void OnSortButtonClicked(object sender, EventArgs e)
{
    // Single column sort
    await dataGrid.GetResponseAsync("sort by OrderID descending");
    
    // Multi-column sort
    await dataGrid.GetResponseAsync("sort by ShipCountry ascending and OrderID descending");
    
    // Clear sort
    await dataGrid.GetResponseAsync("clear sorting");
}
```

## Grouping

Grouping organizes data by one or more columns as specified in the command. Multiple grouping levels can be created to build hierarchical data structures.

### Single Level Grouping

Group data by one column:

```
group by ShipCountry
group by Category
```

**Result:** Grid groups all rows sharing the same value in the specified column.

### Multi-Level Grouping

Create hierarchical grouping with multiple columns:

```
group by ShipCountry and ShipCity
group by Category, Subcategory, and Product
```

**Result:** Primary grouping by first column, secondary grouping within each group, etc.

### Clear Grouping

Remove all grouping:

```
clear grouping
clear groups
```

**Result:** Grid returns to ungrouped state.

### Code Example

```csharp
// Apply grouping programmatically
private async void OnGroupButtonClicked(object sender, EventArgs e)
{
    // Single level group
    await dataGrid.GetResponseAsync("group by ShipCountry");
    
    // Multi-level group
    await dataGrid.GetResponseAsync("group by ShipCountry and ShipCity");
    
    // Clear grouping
    await dataGrid.GetResponseAsync("clear grouping");
}
```

## Filtering

Filtering applies conditions to show only rows matching specific criteria. The AI supports standard comparison operators (equals, greaterThan, lessThan, contains, between) and logical operators (AND, OR) for complex filters.

### Comparison Operators

**Equals:** Match exact values
```
filter ShipCountry equals Germany
filter Status equals Active
```

**Contains:** Match substring
```
filter Customer contains Martin
filter City contains New
```

**GreaterThan:** Numeric comparison
```
filter OrderID greaterThan 1005
filter Total greaterThan 1000
```

**LessThan:** Numeric comparison
```
filter OrderID lessThan 1010
filter Revenue lessThan 500
```

**Between:** Range matching
```
filter OrderID between 1005 1010
filter OrderDate between 2025-01-01 2025-12-31
```

### Logical Combinations

Combine conditions with AND/OR logic:

```
filter Country equals Germany AND Revenue greaterThan 1000
filter Status equals Pending OR Status equals Processing
filter Category equals Electronics AND Price lessThan 500 OR Discount greaterThan 20
```

**Note:** Use AND to require all conditions, OR to require any condition.

### Clear Filtering

Remove all filters to show all rows:

```
clear filters
clear filter
```

**Result:** Grid displays all rows without filtering.

### Code Example

```csharp
// Apply filters programmatically
private async void OnFilterButtonClicked(object sender, EventArgs e)
{
    // Simple filter
    await dataGrid.GetResponseAsync("filter ShipCountry equals Germany");
    
    // Multi-condition filter
    await dataGrid.GetResponseAsync("filter ShipCountry equals Germany AND OrderID greaterThan 1005");
    
    // Complex filter with OR
    await dataGrid.GetResponseAsync("filter ShipCountry equals Germany OR ShipCountry equals UK");
    
    // Clear filter
    await dataGrid.GetResponseAsync("clear filters");
}
```

## Highlighting

Highlighting applies visual styles to rows or cells that meet specified conditions. You can specify custom colors or use the default highlight color defined in SmartAssistStyle.

### Highlight Rows

Highlight entire rows matching a condition:

```
highlight rows where Total > 1000
highlight rows where Status = Active color LightGreen
highlight rows where Country = Germany color #FF6B6B
```

**Color Options:**
- Named colors: `Red`, `Green`, `Blue`, `LightPink`, `LightGreen`, `LightBlue`, `Yellow`
- Hex colors: `#FF6B6B`, `#00AA00`
- RGB format: `rgb(255, 107, 107)`

### Highlight Cells

Highlight specific cells in a column:

```
highlight cells in OrderID lessThan 1005 color Red
highlight cells in Revenue where Revenue > 5000 color Gold
```

### Multiple Highlights

Apply multiple highlight rules in one command:

```
highlight rows where Total > 1000 color LightPink AND highlight cells in OrderID lessThan 1005 color Red
```

### Clear Highlights

Remove highlights individually or all at once:

```
clear highlight
clear all highlights
clear row highlight
clear cell highlight
```

### Code Example

```csharp
// Apply highlights programmatically
private async void OnHighlightButtonClicked(object sender, EventArgs e)
{
    // Highlight rows with custom color
    await dataGrid.GetResponseAsync("highlight rows where Total > 1000 color LightPink");
    
    // Highlight cells in column
    await dataGrid.GetResponseAsync("highlight cells in OrderID lessThan 1005 color Red");
    
    // Multiple highlights
    await dataGrid.GetResponseAsync("highlight rows where Status = Pending color Yellow AND highlight cells in Priority = High color Red");
    
    // Clear all highlights
    await dataGrid.GetResponseAsync("clear highlight");
}
```

## Combined Operations

Chain multiple operations in sequence to create complex data transformations:

```csharp
private async void OnAnalyzeButtonClicked(object sender, EventArgs e)
{
    // Step 1: Filter to specific country
    await dataGrid.GetResponseAsync("filter ShipCountry equals Germany");
    
    // Step 2: Group by city
    await dataGrid.GetResponseAsync("group by ShipCity");
    
    // Step 3: Sort by order count
    await dataGrid.GetResponseAsync("sort by OrderID descending");
    
    // Step 4: Highlight high-value orders
    await dataGrid.GetResponseAsync("highlight rows where Total greaterThan 5000 color Gold");
}
```

## Advanced Patterns

### Pattern 1: Business Intelligence Dashboard
Enable users to explore data hierarchically:

```csharp
// Initial load - group by country
await dataGrid.GetResponseAsync("group by ShipCountry");

// User clicks to expand Germany group
// Then in UI click handler
await dataGrid.GetResponseAsync("group by ShipCountry and ShipCity");

// User drills down to specific city
// Apply time-based filter
await dataGrid.GetResponseAsync("filter ShipCity equals Berlin AND OrderDate greaterThan 2025-06-01");
```

### Pattern 2: Interactive Report
Build a customizable report interface:

```xaml
<!-- User selects from buttons -->
<StackLayout>
    <Button Text="Show High Revenue" Clicked="OnHighRevenue" />
    <Button Text="Group by Region" Clicked="OnGroupRegion" />
    <Button Text="Recent Orders" Clicked="OnRecentOrders" />
</StackLayout>
```

```csharp
private async void OnHighRevenue(object sender, EventArgs e)
{
    await dataGrid.GetResponseAsync("filter Revenue greaterThan 10000 color LightGreen");
}

private async void OnGroupRegion(object sender, EventArgs e)
{
    await dataGrid.GetResponseAsync("group by ShipCountry and ShipCity");
}

private async void OnRecentOrders(object sender, EventArgs e)
{
    await dataGrid.GetResponseAsync("sort by OrderDate descending");
}
```

### Pattern 3: Data Validation Highlighting
Highlight problematic data:

```csharp
private async void ValidateData(object sender, EventArgs e)
{
    // Highlight missing fields
    await dataGrid.GetResponseAsync("highlight rows where Customer = empty color Red");
    
    // Highlight unusually high values
    await dataGrid.GetResponseAsync("highlight rows where Total greaterThan 99999 color Yellow");
    
    // Highlight duplicate orders
    await dataGrid.GetResponseAsync("highlight cells in OrderID where duplicate color Orange");
}
```

## Troubleshooting AI Operations

### Issue: "Highlighting not applying color"
**Solution:** Ensure SmartAssistStyle.HighlightColor is set, or specify color explicitly in command.

### Issue: "Filter with AND/OR not working"
**Solution:** Use uppercase `AND` and `OR`. Example: `filter Status equals Active AND Priority = High`

### Issue: "Column name not recognized"
**Solution:** Verify column names match exactly (case-sensitive). Use column MappingName, not HeaderText.

### Issue: "Operation reversed (ascending vs descending)"
**Solution:** The AI may interpret intent differently. Be explicit: "sort ascending" vs "sort descending"
