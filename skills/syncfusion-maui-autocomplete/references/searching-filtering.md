# Searching and Filtering in .NET MAUI Autocomplete

## Table of Contents
- [Overview](#overview)
- [Search Based on Member Paths](#search-based-on-member-paths)
- [Filtering Modes](#filtering-modes)
- [Minimum Prefix Characters](#minimum-prefix-characters)
- [Custom Filtering](#custom-filtering)
- [Custom Search Highlighting](#custom-search-highlighting)
- [Asynchronous Item Loading](#asynchronous-item-loading)
- [Show Suggestions on Focus](#show-suggestions-on-focus)

## Overview

The Autocomplete control provides powerful search and filtering capabilities to help users find items quickly as they type. The control supports built-in filtering modes, custom filter logic, and async data loading.

## Search Based on Member Paths

### DisplayMemberPath

The `DisplayMemberPath` property specifies which property value to display in the dropdown list. When `TextMemberPath` is null or empty, searching is also performed based on `DisplayMemberPath`.

```xml
<editors:SfAutocomplete ItemsSource="{Binding SocialMedias}"
                        DisplayMemberPath="Name" />
```

```csharp
SfAutocomplete autocomplete = new SfAutocomplete
{
    ItemsSource = socialMediaViewModel.SocialMedias,
    DisplayMemberPath = "Name"
};
```

Typing "T" will filter items where the `Name` property starts with "T": Twitter, Telegram, TikTok, etc.

### TextMemberPath

The `TextMemberPath` property specifies which property to use for searching. This takes precedence over `DisplayMemberPath`.

```xml
<editors:SfAutocomplete ItemsSource="{Binding SocialMedias}"
                        DisplayMemberPath="Name"
                        TextMemberPath="ID" />
```

```csharp
SfAutocomplete autocomplete = new SfAutocomplete
{
    ItemsSource = socialMediaViewModel.SocialMedias,
    DisplayMemberPath = "Name",
    TextMemberPath = "ID"
};
```

With this configuration, searching is performed on the `ID` property, but the `Name` is displayed in the dropdown.

**Note:** Both properties work only with complex objects (classes with multiple properties). For simple strings, they are not needed.

## Filtering Modes

The `TextSearchMode` property controls how filtering is performed. It supports case-insensitive, accent-ignoring search.

### StartsWith (Default)

Filters items that start with the entered text. The first matching item is highlighted.

```xml
<editors:SfAutocomplete TextSearchMode="StartsWith"
                        ItemsSource="{Binding SocialMedias}"
                        DisplayMemberPath="Name"
                        TextMemberPath="Name" />
```

```csharp
autocomplete.TextSearchMode = AutocompleteTextSearchMode.StartsWith;
```

### Contains

Filters items that contain the entered text anywhere in the property value.

```xml
<editors:SfAutocomplete TextSearchMode="Contains"
                        ItemsSource="{Binding SocialMedias}"
                        DisplayMemberPath="Name"
                        TextMemberPath="Name" />
```

```csharp
autocomplete.TextSearchMode = AutocompleteTextSearchMode.Contains;
```

Typing "gram" will match "Instagram", "Telegram", etc.

## Minimum Prefix Characters

Use `MinimumPrefixCharacters` to require a minimum number of characters before displaying suggestions. This improves performance with large datasets.

```xml
<editors:SfAutocomplete MinimumPrefixCharacters="3"
                        ItemsSource="{Binding SocialMedias}"
                        DisplayMemberPath="Name"
                        TextMemberPath="Name" />
```

```csharp
autocomplete.MinimumPrefixCharacters = 3;
```

**Default:** 1 character

With `MinimumPrefixCharacters="3"`, the dropdown only appears after typing 3 characters.

## Custom Filtering

Implement `IAutocompleteFilterBehavior` to apply custom filter logic beyond the built-in modes.

### Step 1: Create Filter Behavior Class

```csharp
using Syncfusion.Maui.Inputs;
using System.Collections;
using System.Collections.ObjectModel;

public class CityFilteringBehavior : IAutocompleteFilterBehavior
{
    public async Task<object> GetMatchingItemsAsync(SfAutocomplete source, AutocompleteFilterInfo filterInfo)
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

**Parameters:**
- **source**: The SfAutocomplete control instance (access ItemsSource, etc.)
- **filterInfo**: Contains the `Text` property with user input

**Return:** Filtered collection of items to display

### Step 2: Apply Filter Behavior

```xml
<editors:SfAutocomplete DisplayMemberPath="CityName"
                        ItemsSource="{Binding Cities}">
    <editors:SfAutocomplete.FilterBehavior>
        <local:CityFilteringBehavior />
    </editors:SfAutocomplete.FilterBehavior>
</editors:SfAutocomplete>
```

```csharp
autocomplete.FilterBehavior = new CityFilteringBehavior();
```

### Example: Filter by Multiple Properties

```csharp
// Model
public class CityInfo
{
    public string CityName { get; set; }
    public string CountryName { get; set; }
    public bool IsCapital { get; set; }
}

// Custom Filter
public class CityFilteringBehavior : IAutocompleteFilterBehavior
{
    public async Task<object> GetMatchingItemsAsync(SfAutocomplete source, AutocompleteFilterInfo filterInfo)
    {
        IEnumerable itemsSource = source.ItemsSource as IEnumerable;
        
        // Filter by city name OR country name
        var filteredItems = itemsSource.Cast<CityInfo>()
            .Where(city => 
                city.CityName.Contains(filterInfo.Text, StringComparison.OrdinalIgnoreCase) ||
                city.CountryName.Contains(filterInfo.Text, StringComparison.OrdinalIgnoreCase));

        return await Task.FromResult(filteredItems.ToList());
    }
}
```

Typing "India" will show all cities in India, or typing "Delhi" will show Delhi.

## Custom Search Highlighting

Implement `IAutocompleteSearchBehavior` to customize which item is highlighted by default in the dropdown.

### Step 1: Create Search Behavior Class

```csharp
using Syncfusion.Maui.Inputs;

public class CapitalCitySearchingBehavior : IAutocompleteSearchBehavior
{
    public int GetHighlightIndex(SfAutocomplete source, AutocompleteSearchInfo searchInfo)
    {
        // Highlight the first capital city in filtered results
        var filteredCapitals = from CityInfo cityInfo in searchInfo.FilteredItems
                               where cityInfo.IsCapital
                               select searchInfo.FilteredItems.IndexOf(cityInfo);
        
        if (filteredCapitals.Count() > 0)
            return filteredCapitals.FirstOrDefault();

        return 0; // Default to first item
    }
}
```

**Parameters:**
- **source**: The SfAutocomplete control instance
- **searchInfo**: Contains `FilteredItems` collection

**Return:** Index of the item to highlight (0-based)

### Step 2: Apply Search Behavior

```xml
<editors:SfAutocomplete DisplayMemberPath="CityName"
                        ItemsSource="{Binding Cities}">
    <editors:SfAutocomplete.FilterBehavior>
        <local:CityFilteringBehavior />
    </editors:SfAutocomplete.FilterBehavior>
    <editors:SfAutocomplete.SearchBehavior>
        <local:CapitalCitySearchingBehavior />
    </editors:SfAutocomplete.SearchBehavior>
</editors:SfAutocomplete>
```

```csharp
autocomplete.SearchBehavior = new CapitalCitySearchingBehavior();
```

When entering a country name like "USA", the first capital city (e.g., "Washington") will be highlighted.

### Complete Example with Both Behaviors

```xml
<ContentPage.BindingContext>
    <local:CityViewModel />
</ContentPage.BindingContext>

<editors:SfAutocomplete DisplayMemberPath="CityName"
                        ItemsSource="{Binding Cities}">
    <editors:SfAutocomplete.FilterBehavior>
        <local:CityFilteringBehavior />
    </editors:SfAutocomplete.FilterBehavior>
    <editors:SfAutocomplete.SearchBehavior>
        <local:CapitalCitySearchingBehavior />
    </editors:SfAutocomplete.SearchBehavior>
</editors:SfAutocomplete>
```

## Asynchronous Item Loading

Load data dynamically at runtime using async operations in custom filters. This is useful for API calls, database queries, or intensive filtering.

### Using Task.Run for Async Filtering

```csharp
public class CustomAsyncFilter : IAutocompleteFilterBehavior
{
    private CancellationTokenSource cancellationTokenSource;

    public async Task<object> GetMatchingItemsAsync(SfAutocomplete source, AutocompleteFilterInfo filterInfo)
    {
        // Cancel previous operation if user types again
        if (cancellationTokenSource != null)
        {
            cancellationTokenSource.Cancel();
            cancellationTokenSource.Dispose();
        }

        cancellationTokenSource = new CancellationTokenSource();
        CancellationToken token = cancellationTokenSource.Token;

        return await Task.Run(() =>
        {
            // Simulate loading large dataset or API call
            List<string> list = new List<string>();
            for (int i = 0; i < 100000; i++)
            {
                list.Add(filterInfo.Text + i);
            }

            return list;
        }, token);
    }
}
```

**Apply:**
```xml
<editors:SfAutocomplete>
    <editors:SfAutocomplete.FilterBehavior>
        <local:CustomAsyncFilter />
    </editors:SfAutocomplete.FilterBehavior>
</editors:SfAutocomplete>
```

**Key Points:**
- Use `CancellationTokenSource` to cancel outdated requests
- Always dispose previous token before creating new one
- `await Task.Run()` performs work on background thread
- Return filtered items from the task

For detailed guidance for the Async Filter, please refer to our [documentation](https://help.syncfusion.com/maui/autocomplete/searching-filtering)

## Show Suggestions on Focus

Display the complete dropdown list when the control receives focus, before any text is entered.

```xml
<editors:SfAutocomplete ShowSuggestionsOnFocus="True"
                        ItemsSource="{Binding SocialMedias}"
                        DisplayMemberPath="Name"
                        TextMemberPath="Name" />
```

```csharp
autocomplete.ShowSuggestionsOnFocus = true;
```

**Default:** false

**Use Case:** Show all available options immediately when user clicks/taps the control.

## Best Practices

1. **Performance with Large Datasets**
   - Use `MinimumPrefixCharacters` to delay filtering
   - Implement async filtering for expensive operations
   - Consider pagination with LoadMore (see ai-powered-loadmore.md)

2. **Cancellation Tokens**
   - Always cancel previous async operations when user types
   - Dispose CancellationTokenSource properly
   - Handle OperationCanceledException

3. **Filter Logic**
   - Use case-insensitive comparisons for better UX
   - Consider phonetic matching for names (see AI-powered features)
   - Return empty collection, not null, when no matches

4. **Search Highlighting**
   - Default to index 0 if no special logic applies
   - Ensure returned index is within bounds of FilteredItems
   - Use for prioritizing results (e.g., exact matches first)

5. **Third-Party Content Safety**
    - Treat responses from remote APIs or LLMs as untrusted data; validate and sanitize before use.
    - Prefer server-side API/LLM calls that enforce authentication, rate-limiting, moderation, and schema validation.
    - Require strict, machine-parseable response formats (JSON with a known schema) and whitelist only expected properties.
    - Limit payload sizes and number of returned items; never bind arbitrary large responses directly to `ItemsSource`.
    - Escape UI-visible strings and apply content-moderation checks for offensive or unsafe suggestions.

## Next Steps

- **Selection handling** - See [selection.md](selection.md)
- **AI-powered search** - See [ai-powered-loadmore.md](ai-powered-loadmore.md) for Azure OpenAI integration
- **UI customization** - See [ui-customization.md](ui-customization.md) for text highlighting
