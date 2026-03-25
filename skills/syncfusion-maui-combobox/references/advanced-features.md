# Advanced Features in .NET MAUI ComboBox

## Table of Contents
- [Enable Auto Size](#enable-auto-size)
- [No Results Found](#no-results-found)
- [Maximum Suggestion Display](#maximum-suggestion-display)
- [Load More](#load-more)
- [Open and Close Dropdown Programmatically](#open-and-close-dropdown-programmatically)
- [Return Command](#return-command)
- [Liquid Glass Effect](#liquid-glass-effect)

## Enable Auto Size

The `EnableAutoSize` property enables dynamic sizing of the ComboBox based on the content of the selected item or the text entry. The default value is `false`.

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    ItemsSource="{Binding SocialMedias}"
                    DisplayMemberPath="Name"
                    TextMemberPath="Name"
                    EnableAutoSize="True"
                    WidthRequest="250"
                    HeightRequest="50" />
```

**C#:**
```csharp
SfComboBox comboBox = new SfComboBox
{
    ItemsSource = socialMediaViewModel.SocialMedias,
    DisplayMemberPath = "Name",
    TextMemberPath = "Name",
    EnableAutoSize = true,
    WidthRequest = 250,
    HeightRequest = 50
};
```

**Behavior:**
- When enabled, the control adjusts its size to fit the content
- Useful when working with variable-length text items
- Respects `WidthRequest` and `HeightRequest` as minimum sizes

## No Results Found

Customize the message and appearance when no matching items are found during filtering or searching.

### No Results Found Text

The `NoResultsFoundText` property sets the text displayed when no items match the search criteria. The default value is `"No results found"`.

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    ItemsSource="{Binding SocialMedias}"
                    DisplayMemberPath="Name"
                    TextMemberPath="Name"
                    IsEditable="True"
                    IsFilteringEnabled="True"
                    NoResultsFoundText="No matches available" />
```

**C#:**
```csharp
SfComboBox comboBox = new SfComboBox
{
    ItemsSource = socialMediaViewModel.SocialMedias,
    DisplayMemberPath = "Name",
    TextMemberPath = "Name",
    IsEditable = true,
    IsFilteringEnabled = true,
    NoResultsFoundText = "No matches available"
};
```

### No Results Found Template

The `NoResultsFoundTemplate` property allows you to provide a custom view when no results are found.

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    ItemsSource="{Binding SocialMedias}"
                    DisplayMemberPath="Name"
                    TextMemberPath="Name"
                    IsEditable="True"
                    IsFilteringEnabled="True">
    <editors:SfComboBox.NoResultsFoundTemplate>
        <DataTemplate>
            <Grid BackgroundColor="LightYellow" 
                  HeightRequest="100">
                <VerticalStackLayout HorizontalOptions="Center"
                                     VerticalOptions="Center"
                                     Spacing="10">
                    <Image Source="search_icon.png" 
                           WidthRequest="40" 
                           HeightRequest="40"/>
                    <Label Text="No matching items found"
                           FontSize="14"
                           FontAttributes="Bold"
                           TextColor="Gray"
                           HorizontalTextAlignment="Center"/>
                    <Label Text="Try a different search term"
                           FontSize="12"
                           TextColor="DarkGray"
                           HorizontalTextAlignment="Center"/>
                </VerticalStackLayout>
            </Grid>
        </DataTemplate>
    </editors:SfComboBox.NoResultsFoundTemplate>
</editors:SfComboBox>
```

**C#:**
```csharp
comboBox.NoResultsFoundTemplate = new DataTemplate(() =>
{
    var grid = new Grid
    {
        BackgroundColor = Colors.LightYellow,
        HeightRequest = 100
    };

    var stackLayout = new VerticalStackLayout
    {
        HorizontalOptions = LayoutOptions.Center,
        VerticalOptions = LayoutOptions.Center,
        Spacing = 10
    };

    stackLayout.Children.Add(new Image
    {
        Source = "search_icon.png",
        WidthRequest = 40,
        HeightRequest = 40
    });

    stackLayout.Children.Add(new Label
    {
        Text = "No matching items found",
        FontSize = 14,
        FontAttributes = FontAttributes.Bold,
        TextColor = Colors.Gray,
        HorizontalTextAlignment = TextAlignment.Center
    });

    stackLayout.Children.Add(new Label
    {
        Text = "Try a different search term",
        FontSize = 12,
        TextColor = Colors.DarkGray,
        HorizontalTextAlignment = TextAlignment.Center
    });

    grid.Children.Add(stackLayout);
    return grid;
});
```

## Maximum Suggestion Display

The `MaximumSuggestion` property limits the number of suggestions displayed in the dropdown at once. When the available items exceed this limit, a "Load More" option appears.

**Default Value:** `-1` (shows all items)

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    ItemsSource="{Binding SocialMedias}"
                    DisplayMemberPath="Name"
                    TextMemberPath="Name"
                    MaximumSuggestion="10" />
```

**C#:**
```csharp
SfComboBox comboBox = new SfComboBox
{
    ItemsSource = socialMediaViewModel.SocialMedias,
    DisplayMemberPath = "Name",
    TextMemberPath = "Name",
    MaximumSuggestion = 10
};
```

**Behavior:**
- Initially displays up to 10 items
- Shows "Load More" button at the bottom if more items are available
- Clicking "Load More" reveals additional items (in batches of `MaximumSuggestion`)

## Load More

When using `MaximumSuggestion`, you can customize the "Load More" button appearance and behavior.

### Load More Text

The `LoadMoreText` property sets the text for the load more button. The default value is `"Load More"`.

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    ItemsSource="{Binding SocialMedias}"
                    DisplayMemberPath="Name"
                    TextMemberPath="Name"
                    MaximumSuggestion="5"
                    LoadMoreText="Show More Items" />
```

**C#:**
```csharp
comboBox.LoadMoreText = "Show More Items";
```

### Load More Template

The `LoadMoreTemplate` property allows you to customize the load more button appearance.

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    ItemsSource="{Binding SocialMedias}"
                    DisplayMemberPath="Name"
                    TextMemberPath="Name"
                    MaximumSuggestion="5">
    <editors:SfComboBox.LoadMoreTemplate>
        <DataTemplate>
            <Grid BackgroundColor="LightBlue" 
                  HeightRequest="50">
                <HorizontalStackLayout HorizontalOptions="Center"
                                       VerticalOptions="Center"
                                       Spacing="8">
                    <Image Source="expand_icon.png" 
                           WidthRequest="20" 
                           HeightRequest="20"/>
                    <Label Text="Load More Options"
                           FontSize="14"
                           FontAttributes="Bold"
                           TextColor="DarkBlue"
                           VerticalOptions="Center"/>
                </HorizontalStackLayout>
            </Grid>
        </DataTemplate>
    </editors:SfComboBox.LoadMoreTemplate>
</editors:SfComboBox>
```

### Load More Button Tapped Event

The `LoadMoreButtonTapped` event is raised when the user taps the load more button.

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    MaximumSuggestion="5"
                    LoadMoreButtonTapped="comboBox_LoadMoreButtonTapped" />
```

**C#:**
```csharp
comboBox.LoadMoreButtonTapped += comboBox_LoadMoreButtonTapped;

private async void comboBox_LoadMoreButtonTapped(object sender, EventArgs e)
{
    // Optionally load data dynamically
    await LoadMoreDataAsync();
    
    await DisplayAlert("Load More", "Loading additional items...", "OK");
}

private async Task LoadMoreDataAsync()
{
    // Simulate loading more data from server
    await Task.Delay(500);
    var newItems = await FetchMoreItemsFromServerAsync();
    
    // Add to existing collection
    foreach (var item in newItems)
    {
        socialMediaViewModel.SocialMedias.Add(item);
    }
}
```

## Open and Close Dropdown Programmatically

The `IsDropDownOpen` property controls the dropdown's open state programmatically.

**XAML:**
```xml
<VerticalStackLayout Spacing="10">
    <editors:SfComboBox x:Name="comboBox"
                        ItemsSource="{Binding SocialMedias}"
                        DisplayMemberPath="Name"
                        TextMemberPath="Name" />
    
    <Button Text="Open Dropdown" 
            Clicked="OnOpenClicked" />
    <Button Text="Close Dropdown" 
            Clicked="OnCloseClicked" />
</VerticalStackLayout>
```

**C#:**
```csharp
private void OnOpenClicked(object sender, EventArgs e)
{
    comboBox.IsDropDownOpen = true;
}

private void OnCloseClicked(object sender, EventArgs e)
{
    comboBox.IsDropDownOpen = false;
}
```

**Common Use Cases:**
- Open dropdown automatically when control receives focus
- Close dropdown after custom action
- Toggle dropdown based on external button click

## Return Command

The `ReturnCommand` is executed when the user completes text entry by pressing the return key in editable mode.

### ReturnCommand Property

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    IsEditable="True"
                    ItemsSource="{Binding SocialMedias}"
                    DisplayMemberPath="Name"
                    TextMemberPath="Name"
                    ReturnCommand="{Binding SearchCommand}" />
```

**C#:**
```csharp
public class ViewModel
{
    public ICommand SearchCommand { get; }

    public ViewModel()
    {
        SearchCommand = new Command<string>(OnSearchExecuted);
    }

    private void OnSearchExecuted(string searchText)
    {
        // Handle search logic
        Debug.WriteLine($"Search executed with text: {searchText}");
    }
}
```

### ReturnCommandParameter

Pass additional data to the command using `ReturnCommandParameter`.

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    IsEditable="True"
                    ItemsSource="{Binding SocialMedias}"
                    DisplayMemberPath="Name"
                    TextMemberPath="Name"
                    ReturnCommand="{Binding SearchCommand}"
                    ReturnCommandParameter="SocialMedia" />
```

**C#:**
```csharp
public class ViewModel
{
    public ICommand SearchCommand { get; }

    public ViewModel()
    {
        SearchCommand = new Command<string>(OnSearchExecuted);
    }

    private void OnSearchExecuted(string parameter)
    {
        // parameter will be "SocialMedia"
        Debug.WriteLine($"Category: {parameter}");
    }
}
```

**Note:** Not supported on Android platform.

## Liquid Glass Effect

The `EnableLiquidGlassEffect` property enables a modern liquid glass visual effect on the ComboBox. This feature is primarily supported on iOS and macOS.

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    ItemsSource="{Binding SocialMedias}"
                    DisplayMemberPath="Name"
                    TextMemberPath="Name"
                    EnableLiquidGlassEffect="True" />
```

**C#:**
```csharp
SfComboBox comboBox = new SfComboBox
{
    ItemsSource = socialMediaViewModel.SocialMedias,
    DisplayMemberPath = "Name",
    TextMemberPath = "Name",
    EnableLiquidGlassEffect = true
};
```

**Platform Support:**
- **iOS/macOS:** Full support with translucent glass effect
- **Android:** Limited or no visual effect
- **Windows:** Limited or no visual effect

**Best Practice:** Test on target platforms to verify the visual appearance.

## Combined Advanced Features Example

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    ItemsSource="{Binding SocialMedias}"
                    DisplayMemberPath="Name"
                    TextMemberPath="Name"
                    IsEditable="True"
                    IsFilteringEnabled="True"
                    EnableAutoSize="True"
                    MaximumSuggestion="10"
                    LoadMoreText="Show More"
                    NoResultsFoundText="No items match your search"
                    LoadMoreButtonTapped="OnLoadMoreTapped"
                    ReturnCommand="{Binding SearchCommand}">
    <editors:SfComboBox.NoResultsFoundTemplate>
        <DataTemplate>
            <Grid HeightRequest="80" BackgroundColor="WhiteSmoke">
                <Label Text="🔍 No results found. Try different keywords."
                       HorizontalOptions="Center"
                       VerticalOptions="Center"
                       FontSize="14"
                       TextColor="Gray"/>
            </Grid>
        </DataTemplate>
    </editors:SfComboBox.NoResultsFoundTemplate>
</editors:SfComboBox>
```

## Best Practices

1. **MaximumSuggestion:**
   - Use for large datasets (1000+ items) to improve performance
   - Set to 20-50 for optimal user experience
   - Combine with filtering for better usability

2. **No Results Found:**
   - Provide helpful guidance in the message
   - Use custom templates for better UX
   - Include suggestions for alternative searches

3. **Load More:**
   - Customize the button to match your app's design
   - Consider lazy loading data when button is tapped
   - Show loading indicators during data fetch

4. **EnableAutoSize:**
   - Useful for dynamic content but test on different screen sizes
   - Ensure minimum sizes are set to prevent layout issues

5. **Liquid Glass Effect:**
   - Test on target platforms before enabling
   - Consider user preferences and accessibility

## Related Topics

- [UI Customization](ui-customization.md) - Appearance and styling
- [Filtering](filtering.md) - Filter items based on input
- [Events and Methods](events-and-methods.md) - All events and methods reference
- [Header and Footer](header-footer.md) - Custom header/footer views
