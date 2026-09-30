# Searching

## Enable Search

```csharp
dataGrid.SearchController.Search(searchText);
```

## Search Functionality

### Basic Search

```csharp
string searchText = "John";
dataGrid.SearchController.Search(searchText);
```

### Clear Search

```csharp
dataGrid.SearchController.ClearSearch();
```

### Search Options

```csharp
// Case-sensitive search
dataGrid.SearchController.AllowCaseSensitive = true;

// Search in specific columns
dataGrid.SearchController.SearchType = SearchType.StartsWith;
```

### Search Highlighting

Matched text is automatically highlighted in cells.

### Customize Highlight Color

The text and background colors for searched and highlighted search results can be customized using SearchTextColor, SearchTextBackground, SearchHighlightTextColor, and SearchHighlightTextBackground in SfDataGrid.DefaultStyle.

```xaml
<syncfusion:SfDataGrid ItemsSource="{Binding OrderInfoCollection}">
        <syncfusion:SfDataGrid.DefaultStyle>
            <syncfusion:DataGridStyle SearchTextColor="Black" 
                                    SearchTextBackground="LightBlue" 
                                    SearchHighlightTextColor="Black" 
                                    SearchHighlightTextBackground="LightGreen" />
        </syncfusion:SfDataGrid.DefaultStyle>
    </syncfusion:SfDataGrid>
```

### Change Background Color for Search Match Cells

Highlight entire cells containing search matches with a background color:

```xaml
<syncfusion:SfDataGrid AllowSearching="True"
                       ItemsSource="{Binding Orders}">
    <syncfusion:SfDataGrid.DefaultStyle>
        <syncfusion:DataGridStyle SearchCellBackground="#FFFACD" />
    </syncfusion:SfDataGrid.DefaultStyle>
</syncfusion:SfDataGrid>
```

```csharp
dataGrid.AllowSearching = true;
dataGrid.DefaultStyle.SearchCellBackground = Color.FromArgb("#FFFACD"); // Light yellow
```

**Features:**
- Cell background changes when search text is found
- Helps visually identify matching rows
- Customizable match cell background color

## Built-in Search UI

Enable the built-in search toolbar UI for user-friendly searching:

```xaml
<syncfusion:SfDataGrid AllowSearching="True"
                       ItemsSource="{Binding Orders}" />
```

```csharp
dataGrid.AllowSearching = true;
```

**Features:**
- Search bar appears above the grid
- Navigation buttons for next/previous matches
- Settings icon for search options
- Clear button to reset search
- Case sensitivity toggle
- Pattern matching options

### Customize Built-in Search UI

```xaml
<ContentPage.Resources>
    <Style TargetType="datagrid:DataGridSearchToolbarView">
        <Setter Property="ShowMoreOptions" Value="True"/>
        <Setter Property="ShowNavigationButtons" Value="True"/>
        <Setter Property="ShowClearButton" Value="True"/>
    </Style>
</ContentPage.Resources>

<syncfusion:SfDataGrid AllowSearching="True"
                       ItemsSource="{Binding Orders}" />
```

## Search Navigation

```csharp
// Find next match
dataGrid.SearchController.FindNext(searchText);

// Find previous match
dataGrid.SearchController.FindPrevious(searchText);
```

## Search Pattern

```csharp
// Implement search with UI
Entry searchEntry = new Entry();
searchEntry.TextChanged += (s, e) =>
{
    dataGrid.SearchController.Search(e.NewTextValue);
};

Button nextButton = new Button { Text = "Next" };
nextButton.Clicked += (s, e) =>
{
    dataGrid.SearchController.FindNext(searchEntry.Text);
};

Button clearButton = new Button { Text = "Clear" };
clearButton.Clicked += (s, e) =>
{
    dataGrid.SearchController.ClearSearch();
    searchEntry.Text = string.Empty;
};
```

## Next Steps

- Read [sorting-filtering.md](sorting-filtering.md) for filtering
- Read [selection.md](selection.md) for selection features
