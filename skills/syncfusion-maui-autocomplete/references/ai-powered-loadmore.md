# AI-Powered Smart Search and LoadMore in .NET MAUI Autocomplete

## Table of Contents
- [AI-Powered Smart Searching](#ai-powered-smart-searching)
- [LoadMore Feature](#loadmore-feature)

## AI-Powered Smart Searching

Implement intelligent search using Azure OpenAI for context-aware, error-tolerant searching.

### Azure OpenAI Setup

**Step 1:** Create Azure OpenAI resource and note:
- Endpoint URL
- API Key
- Deployment Name

**Step 2:** Install NuGet package:
```bash
dotnet add package Azure.AI.OpenAI --version 1.0.0-beta.12
```

**Step 3:** Create Azure service base class:

```csharp
using Azure.AI.OpenAI;
using Azure;

public abstract class AzureBaseService
{
    internal const string Endpoint = "YOUR_ENDPOINT_URL";
    internal const string DeploymentName = "YOUR_DEPLOYMENT_NAME";
    internal const string Key = "YOUR_API_KEY";
    
    protected ChatClient Client { get; set; }
    protected string ChatHistory { get; set; }
    
    public AzureBaseService()
    {
        GetAzureOpenAIKernel();
    }
    
    private void GetAzureOpenAIKernel()
    {
        try
        {
            var client = new AzureOpenAIClient(
                new Uri(Endpoint), 
                new AzureKeyCredential(Key))
                .AsChatClient(modelId: DeploymentName);
            
            this.Client = client;
        }
        catch (Exception ex)
        {
            Debug.WriteLine($"Failed to initialize Azure OpenAI: {ex.Message}");
        }
    }
}
```

**Step 4:** Create AI service:

```csharp
public class AzureAIService : AzureBaseService
{
    public async Task<string> GetCompletion(string prompt, CancellationToken cancellationToken)
    {
        ChatHistory = string.Empty;
        
        if (ChatHistory != null)
        {
            ChatHistory = ChatHistory + "You are a filtering assistant.";
            ChatHistory = ChatHistory + prompt;
            
            try
            {
                if (Client != null)
                {
                    cancellationToken.ThrowIfCancellationRequested();
                    var chatResponse = await Client.CompleteAsync(prompt);
                    return chatResponse.ToString();
                }
            }
            catch (RequestFailedException ex)
            {
                Debug.WriteLine($"Request failed: {ex.Message}");
                throw;
            }
            catch (Exception ex)
            {
                Debug.WriteLine($"An error occurred: {ex.Message}");
                throw;
            }
        }
        
        return string.Empty;
    }
}
```

### Implementing AI Custom Filter

**Step 1:** Create model and ViewModel:

```csharp
// Model
public class CountryModel
{
    public string Name { get; set; }
}

// ViewModel
public class CountryViewModel : INotifyPropertyChanged
{
    public ObservableCollection<CountryModel> Countries { get; set; }
    
    public CountryViewModel()
    {
        Countries = new ObservableCollection<CountryModel>
        {
            new CountryModel { Name = "Afghanistan" },
            new CountryModel { Name = "Albania" },
            new CountryModel { Name = "Algeria" },
            new CountryModel { Name = "United States" },
            new CountryModel { Name = "United Kingdom" },
            // ... more countries
        };
    }
}
```

**Step 2:** Implement custom AI filter:

```csharp
public class CustomFilter : IAutocompleteFilterBehavior
{
    private readonly AzureAIService _azureAIService;
    public ObservableCollection<CountryModel> Countries { get; set; }
    public ObservableCollection<CountryModel> FilteredCountries { get; set; }
    private CancellationTokenSource _cancellationTokenSource;
    
    public CustomFilter()
    {
        _azureAIService = new AzureAIService();
        Countries = new ObservableCollection<CountryModel>();
        FilteredCountries = new ObservableCollection<CountryModel>();
        _cancellationTokenSource = new CancellationTokenSource();
    }
    
    public async Task<object> GetMatchingItemsAsync(SfAutocomplete source, AutocompleteFilterInfo filterInfo)
    {
        if (string.IsNullOrEmpty(filterInfo.Text))
        {
            _cancellationTokenSource?.Cancel();
            FilteredCountries.Clear();
            return await Task.FromResult(FilteredCountries);
        }
        
        Countries = (ObservableCollection<CountryModel>)source.ItemsSource;
        
        // If no API credentials, use offline search
        if (!AzureBaseService.IsCredentialValid)
        {
            var offlineResults = OfflineSearch(filterInfo.Text);
            return await Task.FromResult(offlineResults);
        }
        
        // Cancel previous request
        _cancellationTokenSource?.Cancel();
        _cancellationTokenSource = new CancellationTokenSource();
        var cancellationToken = _cancellationTokenSource.Token;
        
        // Build AI prompt
        string listItems = string.Join(", ", Countries.Select(c => c.Name));
        string outputTemplate = string.Join("\n", Countries.Take(5).Select(c => c.Name));
        
        var filterCountries = await FilterCountriesUsingAzureAI(
            filterInfo.Text, 
            listItems, 
            outputTemplate, 
            cancellationToken);
        
        return await Task.FromResult(filterCountries);
    }
    
    private async Task<ObservableCollection<CountryModel>> FilterCountriesUsingAzureAI(
        string userInput, 
        string itemsList, 
        string outputTemplate, 
        CancellationToken cancellationToken)
    {
        if (!string.IsNullOrEmpty(userInput))
        {
            var prompt = $@"
                Filter the list items based on the user input using character Starting with 
                and Phonetic algorithms like Soundex or Damerau-Levenshtein Distance.
                
                The filter should:
                - Ignore spelling mistakes
                - Be case insensitive
                - Return only filtered items, one per line
                - No explanations, hyphens, numbering, or minus signs
                - Only items from the List Items
                - Arrange items starting with user input's first letter at the top
                
                Examples:
                - userInput: a, filter items starting with A
                - userInput: in, filter items starting with In
                - userInput: pa, filter items starting with Pa
                
                User input: {userInput}
                List of Items: {itemsList}
                
                If no items found, return 'Empty'
                Output format like: {outputTemplate}";
            
            var completion = await _azureAIService.GetCompletion(prompt, cancellationToken);
            
            var filteredCountryNames = completion
                .Split('\n')
                .Select(x => x.Trim())
                .Where(x => !string.IsNullOrEmpty(x))
                .ToList();
            
            if (FilteredCountries.Count > 0)
                FilteredCountries.Clear();
            
            FilteredCountries.AddRange(
                Countries.Where(i => 
                    filteredCountryNames.Any(item => 
                        i.Name.StartsWith(item, StringComparison.OrdinalIgnoreCase))));
        }
        
        return FilteredCountries;
    }
    
    private ObservableCollection<CountryModel> OfflineSearch(string text)
    {
        // Implement Soundex/Levenshtein fallback
        return new ObservableCollection<CountryModel>(
            Countries.Where(c => 
                c.Name.Contains(text, StringComparison.OrdinalIgnoreCase)));
    }
}
```

**Step 3:** Apply filter in XAML:

```xml
<editors:SfAutocomplete DropDownPlacement="Bottom"
                        MaxDropDownHeight="200"
                        TextSearchMode="Contains"
                        DisplayMemberPath="Name"
                        TextMemberPath="Name"
                        ItemsSource="{Binding Countries}">
    <editors:SfAutocomplete.FilterBehavior>
        <local:CustomFilter />
    </editors:SfAutocomplete.FilterBehavior>
</editors:SfAutocomplete>
```

### Key Benefits

1. **Spelling Tolerance** - Handles typos and misspellings
2. **Phonetic Matching** - Finds similar-sounding items
3. **Context-Aware** - Understands user intent
4. **Offline Fallback** - Works without internet
5. **Cancellation Support** - Handles rapid typing

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

**Behavior:** Only shows first 10 items, with "Load More" button to reveal remaining items.

### Custom LoadMore Text

```xml
<editors:SfAutocomplete MaximumSuggestion="5"
                        LoadMoreText="Show more options..."
                        ItemsSource="{Binding SocialMedias}"
                        DisplayMemberPath="Name"
                        TextMemberPath="Name" />
```

```csharp
autocomplete.LoadMoreText = "Show more options...";
```

**Default:** Built-in text

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

## Complete Examples

### AI-Powered Search with LoadMore

```xml
<ContentPage.BindingContext>
    <local:CountryViewModel />
</ContentPage.BindingContext>

<editors:SfAutocomplete Placeholder="Search countries..."
                        MaximumSuggestion="2"
                        LoadMoreText="Load more countries..."
                        LoadMoreButtonTapped="OnLoadMore"
                        DisplayMemberPath="Name"
                        TextMemberPath="Name"
                        ItemsSource="{Binding Countries}">
    <editors:SfAutocomplete.FilterBehavior>
        <local:CustomFilter />
    </editors:SfAutocomplete.FilterBehavior>
</editors:SfAutocomplete>
```

### Custom LoadMore with Pagination

```csharp
public class PaginatedViewModel : INotifyPropertyChanged
{
    public ObservableCollection<Item> Items { get; set; }
    private int currentPage = 1;
    private const int PageSize = 20;
    
    public async Task LoadNextPage()
    {
        var newItems = await ApiService.GetItems(currentPage, PageSize);
        foreach (var item in newItems)
        {
            Items.Add(item);
        }
        currentPage++;
    }
}
```

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

2. **Performance**
   - Use LoadMore instead of loading all items upfront
   - Implement virtual scrolling for very large datasets
   - Consider pagination for API calls

3. **Testing**
   - Test with various dataset sizes
   - Verify LoadMore behavior at end of list
   - Test rapid clicking of LoadMore button

## Next Steps

- **Troubleshooting** - See [troubleshooting.md](troubleshooting.md) for common issues
- **Performance optimization** - See troubleshooting for large dataset handling
