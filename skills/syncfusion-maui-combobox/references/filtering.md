# Filtering in .NET MAUI ComboBox

## Table of Contents
- [Overview](#overview)
- [Enable Filtering](#enable-filtering)
- [Filter Modes](#filter-modes)
  - [StartsWith Mode](#startswith-mode)
  - [Contains Mode](#contains-mode)
- [Custom Filtering](#custom-filtering)
  - [Implementing IComboBoxFilterBehavior](#implementing-icomboboxfilterbehavior)
  - [GetMatchingIndexes Method](#getmatchingindexes-method)
  - [Applying Custom Filter](#applying-custom-filter)
- [Minimum Prefix Characters](#minimum-prefix-characters)
- [Best Practices](#best-practices)
- [Common Scenarios](#common-scenarios)

## Overview

The ComboBox has built-in support to filter data items based on the text entered in the editing text box. The filter operation starts as soon as you begin typing characters in the component.

## Enable Filtering

To enable filtering functionality, set both the `IsFilteringEnabled` and `IsEditable` properties to `true`. The default value is `false`. The dropdown will open automatically when you start typing.

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    IsEditable="true"
                    IsFilteringEnabled="true"
                    ItemsSource="{Binding Cities}"
                    TextMemberPath="CityName"
                    DisplayMemberPath="CityName" />
```

**C#:**
```csharp
SfComboBox comboBox = new SfComboBox
{
    IsEditable = true,
    IsFilteringEnabled = true,
    ItemsSource = cityViewModel.Cities,
    TextMemberPath = "CityName",
    DisplayMemberPath = "CityName"
};
```

**Note:** Filtering is supported only in editable mode.

## Filter Modes

The string comparison for filtering suggestions can be changed using the `TextSearchMode` property. The default filtering type is `StartsWith`, which ignores accent and is case insensitive.

**Available Filter Modes:**
- **StartsWith:** Filters items that begin with the entered text
- **Contains:** Filters items that contain the entered text anywhere

### StartsWith Mode

Filter matching items based on the starting text. The first filtered item will be appended to the typed input and highlighted in the dropdown.

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    TextSearchMode="StartsWith"
                    IsEditable="true"
                    IsFilteringEnabled="true"
                    ItemsSource="{Binding Cities}"
                    TextMemberPath="CityName"
                    DisplayMemberPath="CityName" />
```

**C#:**
```csharp
SfComboBox comboBox = new SfComboBox
{
    TextSearchMode = ComboBoxTextSearchMode.StartsWith,
    IsEditable = true,
    IsFilteringEnabled = true,
    ItemsSource = cityViewModel.Cities,
    TextMemberPath = "CityName",
    DisplayMemberPath = "CityName"
};
```

**Behavior:**
- Input: "Chi" → Results: Chicago, China, etc.
- Auto-appends remaining text of first match
- Case insensitive
- Accent insensitive

### Contains Mode

Filter matching items that contain specific text. The first filtered item will be highlighted in the dropdown.

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    TextSearchMode="Contains"
                    IsEditable="true"
                    IsFilteringEnabled="true"
                    ItemsSource="{Binding Cities}"
                    TextMemberPath="CityName"
                    DisplayMemberPath="CityName" />
```

**C#:**
```csharp
SfComboBox comboBox = new SfComboBox
{
    TextSearchMode = ComboBoxTextSearchMode.Contains,
    IsEditable = true,
    IsFilteringEnabled = true,
    ItemsSource = cityViewModel.Cities,
    TextMemberPath = "CityName",
    DisplayMemberPath = "CityName"
};
```

**Behavior:**
- Input: "or" → Results: Toronto, Oxford, New York, etc.
- Does not auto-append text
- Case insensitive
- Accent insensitive

**Note:** Auto-appending of the first suggested item text to typed input is not supported in Contains mode.

## Custom Filtering

The ComboBox provides support to apply your own custom filter logic by using the `FilterBehavior` property. This allows you to create sophisticated filtering algorithms, including AI-powered search.

### Implementing IComboBoxFilterBehavior

Create a class that implements the `IComboBoxFilterBehavior` interface.

```csharp
public class CityFilteringBehavior : IComboBoxFilterBehavior
{
    public async Task<object?> GetMatchingIndexes(SfComboBox source, ComboBoxFilterInfo filterInfo)
    {
        // Custom filtering logic here
        return await Task.FromResult(filteredItems);
    }
}
```

### GetMatchingIndexes Method

Implement the `GetMatchingIndexes` method to create your own suggestion list containing the filtered items.

**Method Parameters:**
- **source** - The ComboBox owner that holds ItemsSource, Items properties
- **filterInfo** - Contains details about the text entered in the ComboBox

**Example: Filter by City or Country Name**

```csharp
// Model
public class CityInfo
{
    public string CityName { get; set; }
    public string CountryName { get; set; }
    public bool IsCapital { get; set; }
}

// Custom Filter Behavior
public class CityFilteringBehavior : IComboBoxFilterBehavior
{
    public async Task<object?> GetMatchingIndexes(SfComboBox source, ComboBoxFilterInfo filterInfo)
    {
        IEnumerable itemsSource = source.ItemsSource as IEnumerable;
        
        var filteredItems = (from CityInfo item in itemsSource
                            where item.CountryName.StartsWith(filterInfo.Text, StringComparison.CurrentCultureIgnoreCase) ||
                                  item.CityName.StartsWith(filterInfo.Text, StringComparison.CurrentCultureIgnoreCase)
                            select item);
        
        return await Task.FromResult(filteredItems);
    }
}
```

### Applying Custom Filter

Apply the custom filtering to the ComboBox using the `FilterBehavior` property.

**XAML:**
```xml
<editors:SfComboBox TextMemberPath="CityName"
                    DisplayMemberPath="CityName"
                    IsEditable="True"
                    IsFilteringEnabled="True"
                    ItemsSource="{Binding Cities}">
    <editors:SfComboBox.FilterBehavior>
        <local:CityFilteringBehavior/>
    </editors:SfComboBox.FilterBehavior>
</editors:SfComboBox>
```

**C#:**
```csharp
CityViewModel cityViewModel = new CityViewModel();
CityFilteringBehavior cityFilteringBehavior = new CityFilteringBehavior();

SfComboBox comboBox = new SfComboBox
{
    TextMemberPath = "CityName",
    DisplayMemberPath = "CityName",
    IsEditable = true,
    IsFilteringEnabled = true,
    FilterBehavior = cityFilteringBehavior,
    ItemsSource = cityViewModel.Cities
};
```

**Result:** Cities are displayed based on either city name or country name entered.

## Advanced Custom Filtering Examples

### Case-Sensitive Filtering

```csharp
public class CaseSensitiveFilterBehavior : IComboBoxFilterBehavior
{
    public async Task<object?> GetMatchingIndexes(SfComboBox source, ComboBoxFilterInfo filterInfo)
    {
        IEnumerable itemsSource = source.ItemsSource as IEnumerable;
        
        var filteredItems = itemsSource.Cast<YourModel>()
            .Where(item => item.Name.StartsWith(filterInfo.Text, StringComparison.Ordinal));
        
        return await Task.FromResult(filteredItems);
    }
}
```

### Multi-Property Filtering

```csharp
public class MultiPropertyFilterBehavior : IComboBoxFilterBehavior
{
    public async Task<object?> GetMatchingIndexes(SfComboBox source, ComboBoxFilterInfo filterInfo)
    {
        IEnumerable itemsSource = source.ItemsSource as IEnumerable;
        
        var filteredItems = itemsSource.Cast<Employee>()
            .Where(item => 
                item.Name.Contains(filterInfo.Text, StringComparison.OrdinalIgnoreCase) ||
                item.Department.Contains(filterInfo.Text, StringComparison.OrdinalIgnoreCase) ||
                item.ID.ToString().Contains(filterInfo.Text));
        
        return await Task.FromResult(filteredItems);
    }
}
```

### Weighted/Ranked Filtering

```csharp
public class RankedFilterBehavior : IComboBoxFilterBehavior
{
    public async Task<object?> GetMatchingIndexes(SfComboBox source, ComboBoxFilterInfo filterInfo)
    {
        IEnumerable itemsSource = source.ItemsSource as IEnumerable;
        
        var filteredItems = itemsSource.Cast<Product>()
            .Where(item => item.Name.Contains(filterInfo.Text, StringComparison.OrdinalIgnoreCase))
            .OrderByDescending(item => 
                item.Name.StartsWith(filterInfo.Text, StringComparison.OrdinalIgnoreCase) ? 1 : 0)
            .ThenBy(item => item.Name.IndexOf(filterInfo.Text, StringComparison.OrdinalIgnoreCase))
            .ThenBy(item => item.Name);
        
        return await Task.FromResult(filteredItems);
    }
}
```

### Async Database Filtering

```csharp
public class DatabaseFilterBehavior : IComboBoxFilterBehavior
{
    private readonly IDataService _dataService;
    
    public DatabaseFilterBehavior(IDataService dataService)
    {
        _dataService = dataService;
    }
    
    public async Task<object?> GetMatchingIndexes(SfComboBox source, ComboBoxFilterInfo filterInfo)
    {
        // Fetch filtered data from database
        var filteredItems = await _dataService.SearchAsync(filterInfo.Text);
        return filteredItems;
    }
}
```

## Minimum Prefix Characters

Instead of displaying the suggestion list on every character entry, you can filter and display matches after a few character entries using the `MinimumPrefixCharacters` property. The default value is `1`.

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    IsFilteringEnabled="True"
                    IsEditable="True"
                    ItemsSource="{Binding SocialMedias}"
                    MinimumPrefixCharacters="3"
                    DisplayMemberPath="Name"
                    TextMemberPath="Name" />
```

**C#:**
```csharp
SfComboBox comboBox = new SfComboBox
{
    IsFilteringEnabled = true,
    IsEditable = true,
    ItemsSource = socialMediaViewModel.SocialMedias,
    MinimumPrefixCharacters = 3,
    TextMemberPath = "Name",
    DisplayMemberPath = "Name"
};
```

**Result:** Dropdown appears only after typing 3 characters.

**Use Cases:**
- Large datasets where filtering every character is expensive
- API-based filtering with rate limiting
- Improving performance by reducing filter operations

## Best Practices

### Performance Optimization

1. **Use MinimumPrefixCharacters for Large Datasets**
   ```csharp
   comboBox.MinimumPrefixCharacters = 2; // Wait for 2 chars
   ```

2. **Implement Async Filtering for Network Operations**
   ```csharp
   public async Task<object?> GetMatchingIndexes(SfComboBox source, ComboBoxFilterInfo filterInfo)
   {
       using var cts = new CancellationTokenSource(TimeSpan.FromSeconds(5));
       var results = await _apiService.SearchAsync(filterInfo.Text, cts.Token);
       return results;
   }
   ```

3. **Cache Results**
   ```csharp
   private Dictionary<string, IEnumerable> _cache = new();
   
   public async Task<object?> GetMatchingIndexes(SfComboBox source, ComboBoxFilterInfo filterInfo)
   {
       if (_cache.TryGetValue(filterInfo.Text, out var cachedResults))
           return await Task.FromResult(cachedResults);
       
       // Perform filtering
       var results = FilterItems(source.ItemsSource, filterInfo.Text);
       _cache[filterInfo.Text] = results;
       return await Task.FromResult(results);
   }
   ```

### User Experience

1. **Choose Appropriate Filter Mode**
   - Use `StartsWith` for name/title searches
   - Use `Contains` for description/content searches

2. **Set Reasonable MinimumPrefixCharacters**
   - 1-2 for small datasets (<100 items)
   - 2-3 for medium datasets (100-1000 items)
   - 3+ for large datasets (>1000 items)

3. **Handle No Results**
   - Combine with NoResultsFoundText/Template
   - Provide helpful feedback

### Custom Filtering Guidelines

1. **Return Consistent Types**
   - Ensure returned collection matches ItemsSource type

2. **Handle Null/Empty Input**
   ```csharp
   if (string.IsNullOrEmpty(filterInfo.Text))
       return await Task.FromResult(source.ItemsSource);
   ```

3. **Optimize LINQ Queries**
   - Use indexed properties
   - Avoid unnecessary conversions
   - Consider AsParallel() for large datasets

## Common Scenarios

### Filter Capitals Only

```csharp
public class CapitalFilterBehavior : IComboBoxFilterBehavior
{
    public async Task<object?> GetMatchingIndexes(SfComboBox source, ComboBoxFilterInfo filterInfo)
    {
        IEnumerable itemsSource = source.ItemsSource as IEnumerable;
        
        var filteredItems = itemsSource.Cast<CityInfo>()
            .Where(item => 
                item.IsCapital &&
                item.CityName.StartsWith(filterInfo.Text, StringComparison.OrdinalIgnoreCase));
        
        return await Task.FromResult(filteredItems);
    }
}
```

### Fuzzy Search

```csharp
public class FuzzyFilterBehavior : IComboBoxFilterBehavior
{
    public async Task<object?> GetMatchingIndexes(SfComboBox source, ComboBoxFilterInfo filterInfo)
    {
        IEnumerable itemsSource = source.ItemsSource as IEnumerable;
        
        var filteredItems = itemsSource.Cast<Product>()
            .Where(item => CalculateLevenshteinDistance(
                item.Name.ToLower(), 
                filterInfo.Text.ToLower()) <= 2);
        
        return await Task.FromResult(filteredItems);
    }
    
    private int CalculateLevenshteinDistance(string source, string target)
    {
        // Levenshtein distance algorithm implementation
        // ...
    }
}
```

### Combined Filtering with External Data

```csharp
public class EnhancedFilterBehavior : IComboBoxFilterBehavior
{
    private readonly IExternalDataService _externalService;
    
    public async Task<object?> GetMatchingIndexes(SfComboBox source, ComboBoxFilterInfo filterInfo)
    {
        // Filter local items
        IEnumerable localItems = source.ItemsSource as IEnumerable;
        var localFiltered = localItems.Cast<Item>()
            .Where(item => item.Name.Contains(filterInfo.Text));
        
        // Fetch from external source
        var externalItems = await _externalService.SearchAsync(filterInfo.Text);
        
        // Combine results
        var combinedResults = localFiltered.Concat(externalItems).Distinct();
        
        return combinedResults;
    }
}
```

## Related Topics

- [Searching](searching.md) - Text search modes and behavior
- [AI Smart Searching](ai-smart-searching.md) - AI-powered intelligent filtering
- [UI Customization](ui-customization.md) - Styling filtered results
- [Advanced Features](advanced-features.md) - NoResultsFound handling
