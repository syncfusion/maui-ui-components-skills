# UI Customization in .NET MAUI Autocomplete

## Table of Contents
- [Basic Styling](#basic-styling)
- [Dropdown Customization](#dropdown-customization)
- [Token Styling](#token-styling)
- [Text and Alignment](#text-and-alignment)
- [Events](#events)

## Basic Styling

### Placeholder

Display hint text when the control is empty:

```xml
<editors:SfAutocomplete Placeholder="Select a social media"
                        PlaceholderColor="Gray"
                        ItemsSource="{Binding SocialMedias}"
                        DisplayMemberPath="Name" />
```

```csharp
autocomplete.Placeholder = "Select a social media";
autocomplete.PlaceholderColor = Colors.Gray;
```

### Clear Button

Customize the clear button appearance:

**Icon Color:**
```xml
<editors:SfAutocomplete ClearButtonIconColor="Red"
                        ItemsSource="{Binding SocialMedias}"
                        DisplayMemberPath="Name" />
```

**Custom Path:**
```xml
<editors:SfAutocomplete>
    <editors:SfAutocomplete.ClearButtonPath>
        <Path Data="M1.70711 0.292893C1.31658 -0.097631 0.683417 -0.097631 0.292893 0.292893C-0.097631 0.683417 -0.097631 1.31658 0.292893 1.70711L5.58579 7L0.292893 12.2929C-0.097631 12.6834 -0.097631 13.3166 0.292893 13.7071C0.683417 14.0976 1.31658 14.0976 1.70711 13.7071L7 8.41421L12.2929 13.7071C12.6834 14.0976 13.3166 14.0976 13.7071 13.7071C14.0976 13.3166 14.0976 12.6834 13.7071 12.2929L8.41421 7L13.7071 1.70711C14.0976 1.31658 14.0976 0.683417 13.7071 0.292893C13.3166 -0.097631 12.6834 -0.097631 12.2929 0.292893L7 5.58579L1.70711 0.292893Z" 
              Fill="Red" 
              Stroke="Red"/>
    </editors:SfAutocomplete.ClearButtonPath>
</editors:SfAutocomplete>
```

```csharp
var path = new Path 
{ 
    Data = (PathGeometry)converter.ConvertFromInvariantString(customPathData),
    Fill = Colors.Red,
    Stroke = Colors.Red
};
autocomplete.ClearButtonPath = path;
```

### Border and Stroke

**Stroke (Border Color):**
```xml
<editors:SfAutocomplete Stroke="Red"
                        ItemsSource="{Binding SocialMedias}"
                        DisplayMemberPath="Name" />
```

**Show/Hide Border:**
```xml
<editors:SfAutocomplete ShowBorder="False" />
```

```csharp
autocomplete.ShowBorder = false; // Default: true
```

### Selection Text Highlight

```xml
<editors:SfAutocomplete SelectionTextHighlightColor="Green"
                        ItemsSource="{Binding SocialMedias}"
                        DisplayMemberPath="Name" />
```

## Dropdown Customization

### Size and Placement

**Max Height:**
```xml
<editors:SfAutocomplete MaxDropDownHeight="200"
                        ItemsSource="{Binding SocialMedias}"
                        DisplayMemberPath="Name" />
```

**Width:**
```xml
<editors:SfAutocomplete DropDownWidth="400"
                        ItemsSource="{Binding SocialMedias}"
                        DisplayMemberPath="Name" />
```

**Placement:**
```xml
<editors:SfAutocomplete DropDownPlacement="Top"
                        ItemsSource="{Binding SocialMedias}"
                        DisplayMemberPath="Name" />
```

```csharp
public enum AutocompleteDropDownPlacement
{
    Top,      // Above text box
    Bottom,   // Below text box (default)
    Auto,     // Based on available space
    None      // Don't show dropdown
}
```

**Item Height:**
```xml
<editors:SfAutocomplete DropDownItemHeight="80"
                        ItemsSource="{Binding SocialMedias}"
                        DisplayMemberPath="Name" />
```

**Item Padding:**
```xml
<editors:SfAutocomplete ItemPadding="10,20,0,0"
                        ItemsSource="{Binding SocialMedias}"
                        DisplayMemberPath="Name" />
```

### Item Template

Customize dropdown item appearance:

```xml
<editors:SfAutocomplete ItemsSource="{Binding Employees}"
                        DisplayMemberPath="Name"
                        TextMemberPath="Name">
    <editors:SfAutocomplete.ItemTemplate>
        <DataTemplate>
            <ViewCell>
                <Grid ColumnDefinitions="48,220" RowDefinitions="50">
                    <Image Grid.Column="0"
                           Source="{Binding ProfilePicture}"
                           Aspect="AspectFit"/>
                    <StackLayout Grid.Column="1" Margin="15,0,0,0">
                        <Label Text="{Binding Name}" 
                               FontSize="14" 
                               TextColor="Blue"/>
                        <Label Text="{Binding Designation}" 
                               FontSize="12" 
                               TextColor="Coral"/>
                    </StackLayout>
                </Grid>
            </ViewCell>
        </DataTemplate>
    </editors:SfAutocomplete.ItemTemplate>
</editors:SfAutocomplete>
```

### Item Text Styling

**Font Attributes:**
```xml
<editors:SfAutocomplete DropDownItemFontAttributes="Italic"
                        DropDownItemFontFamily="OpenSansSemibold"
                        DropDownItemFontSize="16"
                        DropDownItemTextColor="DarkViolet" />
```

### Background and Borders

**Dropdown Background:**
```xml
<editors:SfAutocomplete DropDownBackground="YellowGreen" />
```

**Selected Item Background:**
```xml
<editors:SfAutocomplete SelectedDropDownItemBackground="LightSeaGreen" />
```

**Selected Item Text Style:**
```xml
<editors:SfAutocomplete>
    <editors:SfAutocomplete.SelectedDropDownItemTextStyle>
        <editors:DropDownTextStyle TextColor="Orange" 
                                   FontSize="16" 
                                   FontAttributes="Bold"/>
    </editors:SfAutocomplete.SelectedDropDownItemTextStyle>
</editors:SfAutocomplete>
```

**Border:**
```xml
<editors:SfAutocomplete DropDownStroke="DarkOrange"
                        DropDownStrokeThickness="5" />
```

**Shadow:**
```xml
<editors:SfAutocomplete IsDropDownShadowVisible="False" />
```

**Corner Radius:**
```xml
<editors:SfAutocomplete DropDownCornerRadius="25" />
```

### Show Suggestions on Focus

```xml
<editors:SfAutocomplete ShowSuggestionsOnFocus="True" />
```

## Token Styling

For multiple selection with token display:

```xml
<editors:SfAutocomplete SelectionMode="Multiple">
    <editors:SfAutocomplete.TokenItemStyle>
        <Style TargetType="core:SfChipGroup">
            <Setter Property="ChipTextColor" Value="White"/>
            <Setter Property="ChipFontAttributes" Value="Bold"/>
            <Setter Property="CloseButtonColor" Value="White"/>
            <Setter Property="ChipBackground" Value="#d3a7ff"/>
            <Setter Property="ChipStroke" Value="#5118e3"/>
            <Setter Property="ChipStrokeThickness" Value="6"/>
            <Setter Property="ChipCornerRadius" Value="18"/>
        </Style>
    </editors:SfAutocomplete.TokenItemStyle>
</editors:SfAutocomplete>
```

## Text and Alignment

### Text Alignment

```xml
<editors:SfAutocomplete HorizontalTextAlignment="Center" 
                        VerticalTextAlignment="Start"/>
```

```csharp
autocomplete.HorizontalTextAlignment = TextAlignment.Center;
autocomplete.VerticalTextAlignment = TextAlignment.Start;
```

**Note:** Dynamic changes to `HorizontalTextAlignment` may not work on Android.

### Cursor Position

```xml
<editors:SfAutocomplete CursorPosition="4" />
```

```csharp
autocomplete.CursorPosition = 4;
```

**Note:** Two-way binding not supported on Android.

### Return Type

Specify keyboard return button type:

```xml
<editors:SfAutocomplete ReturnType="Next" />
```

```csharp
public enum ReturnType
{
    Default,
    Done,
    Go,
    Next,
    Search,
    Send
}
```

### Return Command

Execute command when return key pressed:

```xml
<editors:SfAutocomplete ReturnCommand="{Binding AlertCommand}"
                        ReturnCommandParameter="Return key pressed"/>
```

```csharp
public class ViewModel
{
    public ICommand AlertCommand => new Command<string>(OnAlertCommandExecuted);

    private async void OnAlertCommandExecuted(string parameter)
    {
        await Application.Current.MainPage.DisplayAlert("Alert", parameter, "OK");
    }
}
```

## Events

### Completed Event

Fires when user presses return key:

```xml
<editors:SfAutocomplete Completed="OnCompleted" />
```

```csharp
private async void OnCompleted(object sender, EventArgs e)
{
    await DisplayAlert("Message", "Text entering completed", "close");
}
```

**Note:** Not supported on Android.

### DropDownOpening Event

Fires before dropdown opens (cancellable):

```xml
<editors:SfAutocomplete DropDownOpening="OnDropDownOpening" />
```

```csharp
private void OnDropDownOpening(object sender, CancelEventArgs e)
{
    // Cancel dropdown opening if needed
    e.Cancel = true;
}
```

### DropDownOpened Event

Fires after dropdown opens:

```xml
<editors:SfAutocomplete DropDownOpened="OnDropDownOpened" />
```

```csharp
private void OnDropDownOpened(object sender, EventArgs e)
{
    Console.WriteLine("Dropdown opened");
}
```

### DropDownClosed Event

Fires after dropdown closes:

```xml
<editors:SfAutocomplete DropDownClosed="OnDropDownClosed" />
```

```csharp
private async void OnDropDownClosed(object sender, EventArgs e)
{
    await DisplayAlert("Message", "DropDown Closed", "close");
}
```

### ValueChanged Event

Fires when text value changes:

```xml
<editors:SfAutocomplete ValueChanged="OnValueChanged" />
```

```csharp
private async void OnValueChanged(object sender, AutocompleteValueChangedEventArgs e)
{
    string oldValue = e.OldValue?.ToString();
    string newValue = e.NewValue?.ToString();
    await DisplayAlert("Alert", $"Changed from '{oldValue}' to '{newValue}'", "Ok");
}
```

**EventArgs Properties:**
- `OldValue` - Previous text value
- `NewValue` - New text value

### ClearButtonClicked Event

Fires when clear button is clicked:

```xml
<editors:SfAutocomplete ClearButtonClicked="OnClearButtonClicked" />
```

```csharp
private async void OnClearButtonClicked(object sender, EventArgs e)
{
    await DisplayAlert("Message", "Clear Button Clicked", "ok");
}
```

## Complete Customization Examples

### Modern Styled Autocomplete

```xml
<editors:SfAutocomplete Placeholder="Search..."
                        PlaceholderColor="#6B7280"
                        Stroke="#E5E7EB"
                        Background="White"
                        TextColor="#111827"
                        ClearButtonIconColor="#6B7280"
                        DropDownBackground="White"
                        DropDownStroke="#E5E7EB"
                        DropDownStrokeThickness="1"
                        DropDownCornerRadius="8"
                        SelectedDropDownItemBackground="#EEF2FF"
                        MaxDropDownHeight="300"
                        ItemsSource="{Binding SocialMedias}"
                        DisplayMemberPath="Name" />
```

### Dark Theme Autocomplete

```xml
<editors:SfAutocomplete Placeholder="Search..."
                        PlaceholderColor="#9CA3AF"
                        Stroke="#374151"
                        Background="#1F2937"
                        TextColor="#F9FAFB"
                        ClearButtonIconColor="#9CA3AF"
                        DropDownBackground="#1F2937"
                        DropDownStroke="#374151"
                        DropDownStrokeThickness="1"
                        SelectedDropDownItemBackground="#374151"
                        DropDownItemTextColor="#F9FAFB"
                        ItemsSource="{Binding SocialMedias}"
                        DisplayMemberPath="Name" />
```

### Multi-Select with Custom Tokens

```xml
<editors:SfAutocomplete SelectionMode="Multiple"
                        MultiSelectionDisplayMode="Token"
                        TokensWrapMode="Wrap"
                        Placeholder="Add tags..."
                        ItemsSource="{Binding SocialMedias}"
                        DisplayMemberPath="Name">
    <editors:SfAutocomplete.TokenItemStyle>
        <Style TargetType="core:SfChipGroup">
            <Setter Property="ChipTextColor" Value="White"/>
            <Setter Property="ChipBackground" Value="#3B82F6"/>
            <Setter Property="ChipStroke" Value="#2563EB"/>
            <Setter Property="ChipStrokeThickness" Value="1"/>
            <Setter Property="ChipCornerRadius" Value="12"/>
            <Setter Property="CloseButtonColor" Value="White"/>
        </Style>
    </editors:SfAutocomplete.TokenItemStyle>
</editors:SfAutocomplete>
```

## Best Practices

1. **Color Consistency**
   - Use theme colors for consistent branding
   - Ensure text is readable against backgrounds
   - Consider dark mode support

2. **Dropdown Sizing**
   - Set MaxDropDownHeight for large datasets
   - Use Auto placement for adaptive positioning
   - Consider screen size when setting DropDownWidth

3. **Item Templates**
   - Keep templates simple for performance
   - Use appropriate image sizes
   - Consider accessibility with text alternatives

4. **Events**
   - Avoid heavy operations in frequently-fired events
   - Use async/await for long-running operations
   - Implement proper error handling

5. **Platform Differences**
   - Test on all target platforms
   - Account for Android limitations
   - Provide fallbacks for platform-specific features

## Next Steps

- **Advanced features** - See [advanced-features.md](advanced-features.md) for headers, highlighting
- **Troubleshooting** - See [troubleshooting.md](troubleshooting.md) for platform issues
