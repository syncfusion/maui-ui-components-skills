# Advanced Features in .NET MAUI Autocomplete

## Table of Contents
- [Header and Footer Views](#header-and-footer-views)
- [Text Highlighting](#text-highlighting)
- [No Results Found](#no-results-found)
- [AutoSizing](#autosizing)
- [Liquid Glass Effect](#liquid-glass-effect)

## Header and Footer Views

Add custom header and footer content to the dropdown for enhanced user experience.

### Header View

Display content at the top of the dropdown:

```xml
<editors:SfAutocomplete ShowDropdownHeaderView="True"
                        DropdownHeaderViewHeight="50"
                        ItemsSource="{Binding SocialMedias}"
                        DisplayMemberPath="Name">
    <editors:SfAutocomplete.DropdownHeaderView>
        <StackLayout BackgroundColor="#f0f0f0">
            <Label Text="Popular Social Media" 
                   FontSize="20" 
                   HorizontalOptions="Center" 
                   VerticalOptions="Center" 
                   TextColor="#006bcd"/>
        </StackLayout>
    </editors:SfAutocomplete.DropdownHeaderView>
</editors:SfAutocomplete>
```

```csharp
autocomplete.ShowDropdownHeaderView = true;
autocomplete.DropdownHeaderViewHeight = 50;

var headerView = new StackLayout
{
    BackgroundColor = Color.FromHex("#f0f0f0"),
    Children =
    {
        new Label
        {
            Text = "Popular Social Media",
            FontSize = 20,
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.Center,
            TextColor = Color.FromHex("#006bcd")
        }
    }
};
autocomplete.DropdownHeaderView = headerView;
```

### Footer View

Display content at the bottom of the dropdown:

```xml
<editors:SfAutocomplete ShowDropdownFooterView="True"
                        DropdownFooterViewHeight="50"
                        ItemsSource="{Binding SocialMedias}"
                        DisplayMemberPath="Name">
    <editors:SfAutocomplete.DropdownFooterView>
        <StackLayout BackgroundColor="#f0f0f0">
            <Label Text="Add New" 
                   FontSize="20" 
                   HorizontalOptions="Center" 
                   VerticalOptions="Center" 
                   TextColor="#006bcd"/>
        </StackLayout>
    </editors:SfAutocomplete.DropdownFooterView>
</editors:SfAutocomplete>
```

**Use Cases:**
- Display category headers
- Add "Create New" or "Load More" buttons
- Show counts or statistics
- Provide contextual help

## Text Highlighting

Highlight matching characters in the dropdown to improve visibility.

### Highlighting Modes

**FirstOccurrence** - Highlights first match only:
```xml
<editors:SfAutocomplete TextHighlightMode="FirstOccurrence"
                        HighlightedTextColor="Red"
                        HighlightedTextFontAttributes="Bold"
                        ItemsSource="{Binding SocialMedias}"
                        DisplayMemberPath="Name"
                        TextMemberPath="Name" />
```

**MultipleOccurrence** - Highlights all matches (use with Contains mode):
```xml
<editors:SfAutocomplete TextSearchMode="Contains"
                        TextHighlightMode="MultipleOccurrence"
                        HighlightedTextColor="Red"
                        HighlightedTextFontAttributes="Bold"
                        ItemsSource="{Binding SocialMedias}"
                        DisplayMemberPath="Name"
                        TextMemberPath="Name" />
```

### Highlight Styling

```csharp
autocomplete.TextHighlightMode = OccurrenceMode.MultipleOccurrence;
autocomplete.HighlightedTextColor = Colors.Red;
autocomplete.HighlightedTextFontAttributes = FontAttributes.Bold;
```

**Example:** Typing "gram" with MultipleOccurrence highlights:
- Insta**gram**
- Tele**gram**

## No Results Found

Customize the message displayed when no items match the search.

### No Results Text

```xml
<editors:SfAutocomplete NoResultsFoundText="No matches found"
                        ItemsSource="{Binding SocialMedias}"
                        DisplayMemberPath="Name"
                        TextMemberPath="Name" />
```

```csharp
autocomplete.NoResultsFoundText = "No matches found";
```

**Default:** Displays built-in message

**To disable:** Set to empty string:
```csharp
autocomplete.NoResultsFoundText = string.Empty;
```

### No Results Template

Customize the appearance with a template:

```xml
<editors:SfAutocomplete ItemsSource="{Binding SocialMedias}"
                        DisplayMemberPath="Name"
                        TextMemberPath="Name">
    <editors:SfAutocomplete.NoResultsFoundTemplate>
        <DataTemplate>
            <StackLayout Padding="20">
                <Label Text="😔 No Results" 
                       FontSize="20" 
                       FontAttributes="Bold"
                       TextColor="Gray"
                       HorizontalOptions="Center"/>
                <Label Text="Try a different search term" 
                       FontSize="14" 
                       TextColor="LightGray"
                       HorizontalOptions="Center"/>
            </StackLayout>
        </DataTemplate>
    </editors:SfAutocomplete.NoResultsFoundTemplate>
</editors:SfAutocomplete>
```

```csharp
autocomplete.NoResultsFoundTemplate = new DataTemplate(() =>
{
    return new StackLayout
    {
        Padding = new Thickness(20),
        Children =
        {
            new Label
            {
                Text = "😔 No Results",
                FontSize = 20,
                FontAttributes = FontAttributes.Bold,
                TextColor = Colors.Gray,
                HorizontalOptions = LayoutOptions.Center
            },
            new Label
            {
                Text = "Try a different search term",
                FontSize = 14,
                TextColor = Colors.LightGray,
                HorizontalOptions = LayoutOptions.Center
            }
        }
    };
});
```

## AutoSizing

Enable automatic height adjustment based on content in multiple selection mode.

### Configuration

```xml
<editors:SfAutocomplete SelectionMode="Multiple"
                        MultiSelectionDisplayMode="Token"
                        TokensWrapMode="Wrap"
                        EnableAutoSize="True"
                        ItemsSource="{Binding SocialMedias}"
                        DisplayMemberPath="Name"
                        TextMemberPath="Name" />
```

```csharp
autocomplete.EnableAutoSize = true;
autocomplete.SelectionMode = AutocompleteSelectionMode.Multiple;
autocomplete.TokensWrapMode = AutocompleteTokensWrapMode.Wrap;
```

**Requirements:**
- `SelectionMode` must be `Multiple`
- `TokensWrapMode` must be `Wrap`

**Default:** false

**Behavior:** Control height expands as more tokens are added and wrapped to new lines.

**Use Cases:**
- Dynamic forms where height should adapt
- Tag input controls
- Multi-selection without fixed height constraints

## Liquid Glass Effect

Apply modern, translucent glass-like visual effect (.NET 10+ only).

### Requirements

- **.NET 10** or later
- **iOS 26** or **macOS 26** or later

### Implementation

**Step 1:** Wrap control in SfGlassEffectView:

```xml
<Grid BackgroundColor="Transparent">
    <Image Source="Wallpaper.png" Aspect="AspectFill"/>
    
    <core:SfGlassEffectView EffectType="Regular"
                            CornerRadius="20">
        <editors:SfAutocomplete Background="Transparent"
                                DropDownBackground="Transparent"
                                EnableLiquidGlassEffect="True"
                                HeightRequest="40"
                                WidthRequest="300"
                                ItemsSource="{Binding SocialMedias}"
                                DisplayMemberPath="Name" />
    </core:SfGlassEffectView>
</Grid>
```

**Step 2:** Enable and configure:

```csharp
var glassView = new SfGlassEffectView
{
    EffectType = LiquidGlassEffectType.Regular,
    CornerRadius = 20
};

var autocomplete = new SfAutocomplete
{
    Background = Colors.Transparent,
    DropDownBackground = Colors.Transparent,
    EnableLiquidGlassEffect = true,
    HeightRequest = 40,
    WidthRequest = 300,
    ItemsSource = viewModel.Names,
    DisplayMemberPath = "Name"
};

glassView.Content = autocomplete;
```

**Key Points:**
- Set both `Background` and `DropDownBackground` to `Transparent`
- Enable with `EnableLiquidGlassEffect="True"`
- Wrap in `SfGlassEffectView` for effect rendering
- Provide background image for best visual effect

**Effect Types:**
- `Regular` - Standard glass effect
- Other types available in SfGlassEffectView documentation

**Note:** Only supported on specific platforms and .NET versions. Check platform requirements before implementation.

## Complete Examples

### Searchable Dropdown with Header and Highlighting

```xml
<editors:SfAutocomplete ShowDropdownHeaderView="True"
                        DropdownHeaderViewHeight="40"
                        TextHighlightMode="MultipleOccurrence"
                        HighlightedTextColor="#3B82F6"
                        HighlightedTextFontAttributes="Bold"
                        ItemsSource="{Binding Countries}"
                        DisplayMemberPath="Name"
                        TextSearchMode="Contains">
    <editors:SfAutocomplete.DropdownHeaderView>
        <Label Text="Select Country" 
               FontSize="16"
               FontAttributes="Bold"
               Margin="10"
               TextColor="#374151"/>
    </editors:SfAutocomplete.DropdownHeaderView>
</editors:SfAutocomplete>
```

### Multi-Select with AutoSize and Footer

```xml
<editors:SfAutocomplete SelectionMode="Multiple"
                        MultiSelectionDisplayMode="Token"
                        TokensWrapMode="Wrap"
                        EnableAutoSize="True"
                        ShowDropdownFooterView="True"
                        DropdownFooterViewHeight="40"
                        ItemsSource="{Binding SocialMedias}"
                        DisplayMemberPath="Name">
    <editors:SfAutocomplete.DropdownFooterView>
        <Button Text="Create New Tag" 
                BackgroundColor="#EEF2FF"
                TextColor="#3B82F6"
                Clicked="OnCreateTag"/>
    </editors:SfAutocomplete.DropdownFooterView>
</editors:SfAutocomplete>
```

## Best Practices

1. **Header/Footer**
   - Keep content concise and relevant
   - Use appropriate heights (40-60px typical)
   - Consider dropdown scroll behavior

2. **Text Highlighting**
   - Use contrasting colors for visibility
   - Match highlight style to app theme
   - Use MultipleOccurrence with Contains search mode

3. **No Results**
   - Provide helpful messaging
   - Suggest alternative actions
   - Keep templates simple for quick rendering

4. **AutoSizing**
   - Test with various selection counts
   - Set reasonable MaxDropDownHeight
   - Consider screen size constraints

5. **Liquid Glass Effect**
   - Verify platform compatibility
   - Use with appropriate backgrounds
   - Test performance on target devices
   - Provide fallback for unsupported platforms

## Next Steps

- **AI-powered features** - See [ai-powered-loadmore.md](ai-powered-loadmore.md)
- **Troubleshooting** - See [troubleshooting.md](troubleshooting.md)
