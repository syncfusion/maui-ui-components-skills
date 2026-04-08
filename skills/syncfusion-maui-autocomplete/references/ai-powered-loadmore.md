# AI-Powered Smart Search and LoadMore in .NET MAUI Autocomplete

## Table of Contents
- [AI-Powered Smart Searching](#ai-powered-smart-searching)
- [LoadMore Feature](#loadmore-feature)

## AI-Powered Smart Searching

Implement intelligent search using Azure OpenAI for context-aware, error-tolerant searching.

For detailed guidance, please refer to our [documentation](https://help.syncfusion.com/maui/autocomplete/ai-powered-smart-searching)

# LoadMore & Performance Guidance for .NET MAUI Autocomplete

This document describes the `LoadMore` feature and performance recommendations for the `SfAutocomplete` control. Examples that integrate AI/LLM services or perform client-side calls to external LLM/APIs have been removed for safety.

## LoadMore Feature

Restrict displayed items and load more on demand for better performance with large datasets.

### Basic Configuration

```xml
<editors:SfAutocomplete MaximumSuggestion="10"
                        ItemsSource="{Binding SocialMedias}"
                        DisplayMemberPath="Name"
                        TextMemberPath="Name" />
```

```csharp
autocomplete.MaximumSuggestion = 10;
```

**Behavior:** Shows only the first `MaximumSuggestion` items; a Load More action reveals or fetches additional items.

### Custom LoadMore Text

```xml
<editors:SfAutocomplete MaximumSuggestion="5"
                        LoadMoreText="Show more options..."
                        ItemsSource="{Binding SocialMedias}"
                        DisplayMemberPath="Name"
                        TextMemberPath="Name" />
```

### LoadMore Template

Customize the LoadMore button appearance:

```xml
<editors:SfAutocomplete MaximumSuggestion="5"
                        ItemsSource="{Binding SocialMedias}"
                        DisplayMemberPath="Name"
                        TextMemberPath="Name">
    <editors:SfAutocomplete.LoadMoreTemplate>
        <DataTemplate>
            <Grid BackgroundColor="LightGreen" Padding="10">
                <Label Text="↓ Load more items..." 
                       VerticalOptions="Center" 
                       FontAttributes="Bold" 
                       HorizontalOptions="Center" 
                       TextColor="Red"/>
            </Grid>
        </DataTemplate>
    </editors:SfAutocomplete.LoadMoreTemplate>
</editors:SfAutocomplete>
```

```csharp
autocomplete.LoadMoreTemplate = new DataTemplate(() =>
{
    var grid = new Grid
    {
        BackgroundColor = Colors.LightGreen,
        Padding = new Thickness(10)
    };
    
    var label = new Label
    {
        Text = "↓ Load more items...",
        TextColor = Colors.Red,
        HorizontalOptions = LayoutOptions.Center,
        VerticalOptions = LayoutOptions.Center,
        FontAttributes = FontAttributes.Bold
    };
    
    grid.Children.Add(label);
    return grid;
});
```

### LoadMoreButtonTapped Event

Handle when user taps LoadMore button:

```xml
<editors:SfAutocomplete MaximumSuggestion="5"
                        LoadMoreButtonTapped="OnLoadMore"
                        ItemsSource="{Binding SocialMedias}"
                        DisplayMemberPath="Name"
                        TextMemberPath="Name" />
```

```csharp
private void OnLoadMore(object sender, EventArgs e)
{
    // Perform additional loading logic
    Console.WriteLine("Load More button tapped");
    
    // Example: Load next batch from API
    await LoadNextBatch();
}
```

**Use Cases:**
- Track analytics on user behavior
- Load additional data from API
- Show loading indicator
- Implement pagination

## Best Practices

### AI-Powered Search

1. **Error Handling**
   - Implement robust error handling for API failures
   - Provide offline fallback
   - Handle network timeouts

2. **Performance**
   - Cancel previous requests on new input
   - Implement debouncing for rapid typing
   - Cache frequently searched terms

3. **Prompt Engineering**
   - Be specific about output format
   - Include examples in prompt
   - Test prompts with edge cases

4. **Cost Management**
   - Implement minimum character requirements
   - Use local filtering for simple queries
   - Monitor API usage

### LoadMore

1. **User Experience**
   - Set appropriate MaximumSuggestion (10-20 typical)
   - Provide clear LoadMore indication
   - Show loading state when fetching more items

## Performance Best Practices

- Use `MinimumPrefixCharacters` to avoid performing work for very short inputs.
- Debounce user input when calling expensive operations.
- Perform heavy filtering on background threads and cancel outdated operations with `CancellationTokenSource`.
- Prefer server-side pagination and filtering for very large datasets; return only the whitelisted fields required by the UI.
- Limit payload sizes and the number of items returned by any remote service.
- Keep item templates lightweight to reduce rendering cost.

## Next Steps

- **Searching and filtering** - See [references/searching-filtering.md](searching-filtering.md)
- **Troubleshooting** - See [references/troubleshooting.md](troubleshooting.md)
