# Searching in .NET MAUI ComboBox

## Table of Contents
- [Overview](#overview)
- [Search Based on Member Path](#search-based-on-member-path)
- [Edit Mode Searching](#edit-mode-searching)
- [Text Search Modes](#text-search-modes)
  - [StartsWith Mode](#startswith-mode)
  - [Contains Mode](#contains-mode)
- [Minimum Prefix Characters](#minimum-prefix-characters)
- [Best Practices](#best-practices)

## Overview

The ComboBox control provides rich text searching functionality. The `TextSearchMode` property regulates how the control behaves when it receives user input during selection and filtering operations.

## Search Based on Member Path

The `DisplayMemberPath` and `TextMemberPath` properties specify the property path by which searching must be done when custom data is bound to the `ItemsSource` property.

### DisplayMemberPath

Specifies the property path whose value is displayed as text in the dropdown menu. The default value is `string.Empty`.

### TextMemberPath

Specifies the property path whose value is used to perform searching based on user input received in the selection box. The default value is `string.Empty`.

**When TextMemberPath is null or empty**, searching will be performed based on `DisplayMemberPath`.

**Important Notes:**
- `DisplayMemberPath` and `TextMemberPath` are effective only for collection items that hold two or more properties
- When both are null or `string.Empty`, searching is performed based on the class name with namespace of the item

**Example:**

```csharp
// Model with multiple properties
public class SocialMedia
{
    public string Name { get; set; }
    public int ID { get; set; }
}
```

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    ItemsSource="{Binding SocialMedias}"
                    DisplayMemberPath="Name"
                    TextMemberPath="Name" />
```

**C#:**
```csharp
SfComboBox comboBox = new SfComboBox
{
    ItemsSource = socialMediaViewModel.SocialMedias,
    DisplayMemberPath = "Name",
    TextMemberPath = "Name"
};
```

## Edit Mode Searching

In edit mode, searching is performed based on the `TextMemberPath` property while entering text into the selection box.

**Search Priority:**
1. If `TextMemberPath` is set → Search based on TextMemberPath
2. If `TextMemberPath` is null/empty → Search based on DisplayMemberPath

**Example: Search by ID Instead of Name**

```xml
<editors:SfComboBox x:Name="comboBox"
                    IsEditable="true"
                    ItemsSource="{Binding SocialMedias}"
                    TextMemberPath="ID"
                    DisplayMemberPath="Name" />
```

```csharp
SfComboBox comboBox = new SfComboBox
{
    IsEditable = true,
    ItemsSource = socialMediaViewModel.SocialMedias,
    TextMemberPath = "ID",
    DisplayMemberPath = "Name"
};
```

**Behavior:**
- User types: "5"
- Searches by ID property
- Displays matching item's Name in dropdown
- Auto-appends if match found (in StartsWith mode)

**Note:** Auto-appending of text is supported only in editable mode and when `TextSearchMode` is set to `StartsWith`.

## Text Search Modes

The `TextSearchMode` property controls how the control matches user input against items. The default is `StartsWith`, which is case-insensitive and ignores accents.

**Available Modes:**
- **StartsWith** - Matches items that begin with the entered text
- **Contains** - Matches items that contain the entered text anywhere

### StartsWith Mode

Searches for items based on the starting text. The first matching item is highlighted and (in editable mode) the remaining text is auto-appended.

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    TextSearchMode="StartsWith"
                    ItemsSource="{Binding SocialMedias}"
                    DisplayMemberPath="Name"
                    TextMemberPath="Name" />
```

**C#:**
```csharp
SfComboBox comboBox = new SfComboBox
{
    TextSearchMode = ComboBoxTextSearchMode.StartsWith,
    ItemsSource = socialMediaViewModel.SocialMedias,
    TextMemberPath = "Name",
    DisplayMemberPath = "Name"
};
```

**Behavior:**
- Input: "Fa" → Matches: Facebook, Facetime
- First match is highlighted
- In editable mode: Auto-completes to "Facebook"
- Case insensitive: "fa" matches "Facebook"
- Accent insensitive: "cafe" matches "Café"

**Best Use Cases:**
- Name searches (first name, last name)
- Title searches
- Code/ID prefix searches
- Alphabetical navigation

### Contains Mode

Searches for items containing the specific text anywhere in the value. The first matching item is highlighted in the dropdown.

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    IsEditable="True"
                    TextSearchMode="Contains"
                    ItemsSource="{Binding SocialMedias}"
                    TextMemberPath="Name"
                    DisplayMemberPath="Name" />
```

**C#:**
```csharp
SfComboBox comboBox = new SfComboBox
{
    IsEditable = true,
    TextSearchMode = ComboBoxTextSearchMode.Contains,
    ItemsSource = socialMediaViewModel.SocialMedias,
    TextMemberPath = "Name",
    DisplayMemberPath = "Name"
};
```

**Behavior:**
- Input: "book" → Matches: Facebook, Notebook
- First match is highlighted
- No auto-append in this mode
- Case insensitive
- Accent insensitive

**Best Use Cases:**
- Description searches
- Keyword searches within content
- Flexible user input scenarios
- When users may not know the exact start of the text

**Note:** Auto-appending of the first suggested item text is NOT supported in Contains mode.

## Minimum Prefix Characters

Instead of displaying suggestions on every character entry, matches can be filtered and displayed after a few character entries using the `MinimumPrefixCharacters` property. The default value is `1`.

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

**Result:** Dropdown appears only after typing 3 or more characters.

**Benefits:**
- Reduces unnecessary filtering operations
- Improves performance for large datasets
- Better for API-based searches
- Prevents overwhelming users with too many results

**Recommended Values:**
- Small datasets (<100 items): 1-2 characters
- Medium datasets (100-1000 items): 2-3 characters
- Large datasets (>1000 items): 3-4 characters
- API/database searches: 3+ characters

## Best Practices

### Choosing the Right Search Mode

**Use StartsWith when:**
- Users typically know the beginning of items
- Alphabetical organization is important
- Auto-complete is desired
- Examples: Names, titles, countries, products

**Use Contains when:**
- Users may search by any part of the item
- Items have descriptions or multi-word names
- Keyword search is more natural
- Examples: Descriptions, addresses, multi-word titles

### Optimizing Search Performance

1. **Set appropriate MinimumPrefixCharacters**
   ```csharp
   // For large datasets
   comboBox.MinimumPrefixCharacters = 3;
   ```

2. **Choose TextMemberPath wisely**
   ```csharp
   // Search by indexed or frequently used properties
   comboBox.TextMemberPath = "Code"; // Instead of long descriptions
   ```

3. **Combine with filtering for real-time results**
   ```xml
   <editors:SfComboBox IsFilteringEnabled="True"
                       IsEditable="True"
                       TextSearchMode="StartsWith"
                       MinimumPrefixCharacters="2" />
   ```

### Member Path Configuration

**Single Property Objects:**
```csharp
// Simple string collection
var items = new ObservableCollection<string> { "Apple", "Banana", "Cherry" };
comboBox.ItemsSource = items;
// No need to set DisplayMemberPath or TextMemberPath
```

**Multi-Property Objects:**
```csharp
// Complex objects
public class Product
{
    public string Code { get; set; }
    public string Name { get; set; }
    public string Description { get; set; }
}

// Configuration
comboBox.DisplayMemberPath = "Name"; // Show name in dropdown
comboBox.TextMemberPath = "Code";    // Search by code when typing
```

**Search Different Property than Display:**
```xml
<!-- Display employee name, but allow searching by employee ID -->
<editors:SfComboBox IsEditable="True"
                    DisplayMemberPath="Name"
                    TextMemberPath="EmployeeID"
                    ItemsSource="{Binding Employees}" />
```

## Common Scenarios

### Case-Insensitive Name Search

```csharp
// Default behavior - case insensitive
comboBox.TextSearchMode = ComboBoxTextSearchMode.StartsWith;
// "john" matches "John", "JOHN", "JoHn"
```

### Flexible Keyword Search

```csharp
// Search anywhere in the text
comboBox.TextSearchMode = ComboBoxTextSearchMode.Contains;
comboBox.IsFilteringEnabled = true;
// "dev" matches "Developer", "Development", "Web Developer"
```

### Code-Based Search with Name Display

```xml
<editors:SfComboBox IsEditable="True"
                    DisplayMemberPath="ProductName"
                    TextMemberPath="ProductCode"
                    ItemsSource="{Binding Products}" />
```

```csharp
// User types "ABC123" (product code)
// Dropdown shows "Laptop Pro" (product name)
```

### Multi-Property Search (Requires Custom Filter)

For searching across multiple properties, combine with custom filtering:

```csharp
// In custom filter behavior
public async Task<object?> GetMatchingIndexes(SfComboBox source, ComboBoxFilterInfo filterInfo)
{
    var items = source.ItemsSource.Cast<Employee>();
    var filtered = items.Where(e => 
        e.Name.Contains(filterInfo.Text, StringComparison.OrdinalIgnoreCase) ||
        e.Department.Contains(filterInfo.Text, StringComparison.OrdinalIgnoreCase) ||
        e.EmployeeID.Contains(filterInfo.Text));
    
    return await Task.FromResult(filtered);
}
```

### Progressive Search (Minimum Characters)

```xml
<!-- Wait for 3 characters before searching -->
<editors:SfComboBox IsEditable="True"
                    IsFilteringEnabled="True"
                    MinimumPrefixCharacters="3"
                    TextSearchMode="Contains"
                    Placeholder="Type at least 3 characters to search..."
                    ItemsSource="{Binding Countries}" />
```

### Auto-Complete with StartsWith

```xml
<editors:SfComboBox IsEditable="True"
                    TextSearchMode="StartsWith"
                    ItemsSource="{Binding Countries}"
                    Placeholder="Start typing country name..." />
```

```csharp
// User types: "Uni"
// Auto-completes to: "United States"
// Other matches shown in dropdown: "United Kingdom", etc.
```

## Comparison: StartsWith vs Contains

| Feature | StartsWith | Contains |
|---------|-----------|----------|
| **Auto-append** | Yes (in editable mode) | No |
| **Match location** | Beginning only | Anywhere in text |
| **Use case** | Known prefix | Keyword search |
| **Performance** | Faster (early termination) | Slower (full scan) |
| **User expectation** | Traditional dropdown | Search box behavior |
| **Best for** | Names, codes, titles | Descriptions, flexible search |

## Related Topics

- [Filtering](filtering.md) - Enable real-time filtering during search
- [Editing Modes](editing-modes.md) - Configure editable behavior
- [UI Customization](ui-customization.md) - Style search results
- [Highlighting Text](highlighting-text.md) - Highlight matched text in results
