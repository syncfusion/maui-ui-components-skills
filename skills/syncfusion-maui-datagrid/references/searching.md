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

```xml
<syncfusion:SfDataGrid ItemsSource="{Binding OrderInfoCollection}">
        <syncfusion:SfDataGrid.DefaultStyle>
            <syncfusion:DataGridStyle SearchTextColor="Black" 
                                    SearchTextBackground="LightBlue" 
                                    SearchHighlightTextColor="Black" 
                                    SearchHighlightTextBackground="LightGreen" />
        </syncfusion:SfDataGrid.DefaultStyle>
    </syncfusion:SfDataGrid>
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
