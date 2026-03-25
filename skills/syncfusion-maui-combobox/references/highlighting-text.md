# Highlighting Matched Text in .NET MAUI ComboBox

## Overview

The Syncfusion .NET MAUI ComboBox control supports highlighting the matched text in dropdown suggestions based on the entered input. This visual feedback helps users quickly identify matching items as they type.

## Text Highlight Mode

The `TextHighlightMode` property controls how matching text is highlighted in the dropdown items.

**Available Modes:**
- `None` - No highlighting
- `FirstOccurrence` - Highlights only the first match in each item
- `MultipleOccurrence` - Highlights all matches in each item

**Default Value:** `None`

### None Mode

No text highlighting occurs.

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    ItemsSource="{Binding SocialMedias}"
                    DisplayMemberPath="Name"
                    TextMemberPath="Name"
                    Placeholder="Enter Media"
                    TextHighlightMode="None" />
```

### FirstOccurrence Mode

Highlights only the first occurrence of the matching text in each suggestion item.

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    ItemsSource="{Binding SocialMedias}"
                    DisplayMemberPath="Name"
                    TextMemberPath="Name"
                    Placeholder="Enter Media"
                    TextHighlightMode="FirstOccurrence" />
```

**C#:**
```csharp
SfComboBox comboBox = new SfComboBox
{
    ItemsSource = socialMediaViewModel.SocialMedias,
    DisplayMemberPath = "Name",
    TextMemberPath = "Name",
    Placeholder = "Enter Media",
    TextHighlightMode = OccurrenceMode.FirstOccurrence
};
```

**Example:** If user types "book" and an item contains "Facebook Bookmarks", only "book" in "Facebook" will be highlighted.

### MultipleOccurrence Mode

Highlights all occurrences of the matching text in each suggestion item.

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    ItemsSource="{Binding SocialMedias}"
                    DisplayMemberPath="Name"
                    TextMemberPath="Name"
                    Placeholder="Enter Media"
                    TextHighlightMode="MultipleOccurrence" />
```

**C#:**
```csharp
SfComboBox comboBox = new SfComboBox
{
    ItemsSource = socialMediaViewModel.SocialMedias,
    DisplayMemberPath = "Name",
    TextMemberPath = "Name",
    Placeholder = "Enter Media",
    TextHighlightMode = OccurrenceMode.MultipleOccurrence
};
```

**Example:** If user types "book" and an item contains "Facebook Bookmarks", both "book" in "Facebook" and "Book" in "Bookmarks" will be highlighted.

## Highlighted Text Color

The `HighlightedTextColor` property sets the color used for highlighting matched text.

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    ItemsSource="{Binding SocialMedias}"
                    DisplayMemberPath="Name"
                    TextMemberPath="Name"
                    Placeholder="Enter Media"
                    TextHighlightMode="FirstOccurrence"
                    HighlightedTextColor="Red" />
```

**C#:**
```csharp
SfComboBox comboBox = new SfComboBox
{
    ItemsSource = socialMediaViewModel.SocialMedias,
    DisplayMemberPath = "Name",
    TextMemberPath = "Name",
    Placeholder = "Enter Media",
    TextHighlightMode = OccurrenceMode.FirstOccurrence,
    HighlightedTextColor = Colors.Red
};
```

## Highlighted Text Font Attributes

The `HighlightedTextFontAttributes` property sets the font style for highlighted text (Bold, Italic, or both).

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    ItemsSource="{Binding SocialMedias}"
                    DisplayMemberPath="Name"
                    TextMemberPath="Name"
                    Placeholder="Enter Media"
                    TextHighlightMode="MultipleOccurrence"
                    HighlightedTextColor="Blue"
                    HighlightedTextFontAttributes="Bold" />
```

**C#:**
```csharp
SfComboBox comboBox = new SfComboBox
{
    ItemsSource = socialMediaViewModel.SocialMedias,
    DisplayMemberPath = "Name",
    TextMemberPath = "Name",
    Placeholder = "Enter Media",
    TextHighlightMode = OccurrenceMode.MultipleOccurrence,
    HighlightedTextColor = Colors.Blue,
    HighlightedTextFontAttributes = FontAttributes.Bold
};
```

**Available Font Attributes:**
- `None` - Normal text
- `Bold` - Bold text
- `Italic` - Italic text
- `Bold | Italic` - Bold and italic

## Combined Highlighting Customization

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    ItemsSource="{Binding SocialMedias}"
                    DisplayMemberPath="Name"
                    TextMemberPath="Name"
                    Placeholder="Search Social Media"
                    TextHighlightMode="MultipleOccurrence"
                    HighlightedTextColor="Orange"
                    HighlightedTextFontAttributes="Italic" />
```

**C#:**
```csharp
SfComboBox comboBox = new SfComboBox
{
    ItemsSource = socialMediaViewModel.SocialMedias,
    DisplayMemberPath = "Name",
    TextMemberPath = "Name",
    Placeholder = "Search Social Media",
    TextHighlightMode = OccurrenceMode.MultipleOccurrence,
    HighlightedTextColor = Colors.Orange,
    HighlightedTextFontAttributes = FontAttributes.Italic
};
```

## Use Cases

### 1. Search-Heavy Applications

For applications where users frequently search through large lists, use `MultipleOccurrence` with distinct colors:

**C#:**
```csharp
comboBox.TextHighlightMode = OccurrenceMode.MultipleOccurrence;
comboBox.HighlightedTextColor = Colors.Yellow;
comboBox.HighlightedTextFontAttributes = FontAttributes.Bold;
```

### 2. Subtle Highlighting

For a more subtle appearance, use `FirstOccurrence` with a light accent color:

**C#:**
```csharp
comboBox.TextHighlightMode = OccurrenceMode.FirstOccurrence;
comboBox.HighlightedTextColor = Color.FromArgb("#90CAF9"); // Light blue
comboBox.HighlightedTextFontAttributes = FontAttributes.None;
```

### 3. Accessibility-Friendly Highlighting

For better visibility, use bold font with high-contrast colors:

**C#:**
```csharp
comboBox.TextHighlightMode = OccurrenceMode.FirstOccurrence;
comboBox.HighlightedTextColor = Colors.DarkBlue;
comboBox.HighlightedTextFontAttributes = FontAttributes.Bold;
comboBox.DropDownItemTextColor = Colors.Black; // Ensure good contrast
```

## Behavior with Filtering

Text highlighting works seamlessly with filtering. When `IsFilteringEnabled` is true:

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    IsEditable="True"
                    IsFilteringEnabled="True"
                    ItemsSource="{Binding SocialMedias}"
                    DisplayMemberPath="Name"
                    TextMemberPath="Name"
                    TextSearchMode="Contains"
                    TextHighlightMode="MultipleOccurrence"
                    HighlightedTextColor="Green"
                    HighlightedTextFontAttributes="Bold" />
```

**Behavior:**
- As user types, dropdown filters to matching items
- Highlighted text updates dynamically with each keystroke
- Both "StartsWith" and "Contains" search modes support highlighting

## Behavior with TextMemberPath

Highlighting applies to the property specified in `TextMemberPath`:

**C#:**
```csharp
public class SocialMedia
{
    public string Name { get; set; }
    public string Category { get; set; }
}

// ComboBox configuration
comboBox.DisplayMemberPath = "Name";
comboBox.TextMemberPath = "Name"; // Highlighting applies to Name property
comboBox.TextHighlightMode = OccurrenceMode.MultipleOccurrence;
```

**Note:** If using `ItemTemplate`, highlighting applies to the bound text property, not custom template elements.

## Best Practices

1. **Choose Appropriate Mode:**
   - Use `FirstOccurrence` for cleaner appearance with long text
   - Use `MultipleOccurrence` when users need to see all matches

2. **Color Contrast:**
   - Ensure sufficient contrast between `HighlightedTextColor` and `DropDownItemTextColor`
   - Test on different backgrounds and themes

3. **Font Attributes:**
   - Bold is generally more visible than italic
   - Avoid combining both unless necessary for accessibility

4. **Performance:**
   - `MultipleOccurrence` may have slightly more overhead with very large datasets
   - Consider `FirstOccurrence` for lists with 1000+ items

5. **User Experience:**
   - Enable highlighting when filtering is enabled for better visual feedback
   - Match highlight color with your app's accent color for consistency

## Limitations

- Highlighting does not apply to custom `ItemTemplate` views directly
- Only the text specified by `TextMemberPath` is highlighted
- Case-insensitive highlighting (matches are case-insensitive)

## Related Topics

- [Searching](searching.md) - Text search modes and behavior
- [Filtering](filtering.md) - Filter suggestions based on input
- [UI Customization](ui-customization.md) - Overall appearance customization
- [Getting Started](getting-started.md) - Basic implementation
