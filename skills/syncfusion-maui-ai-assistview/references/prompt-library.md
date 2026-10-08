# Prompt Library in SfAIAssistView

## Overview

`SfPromptLibrary` is a self-contained view that lets users browse, search, and pick from a
collection of predefined prompts. When a prompt is selected, you can react through the
`PromptSelected` event or handle the selection through `PromptSelectedCommand`.

## Table of Contents
- [Bind the prompt library to SfAIAssistView](#bind-the-prompt-library-to-sfaiassistview)
- [Supply prompts via ItemsSource](#supply-prompts-via-itemssource)
- [Handle selection](#handle-selection)
- [PromptItem model](#promptitem-model)

---

## Bind the prompt library to SfAIAssistView

Assign an `SfPromptLibrary` instance to `SfAIAssistView.PromptLibrary`.

### XAML

```xaml
<syncfusion:SfAIAssistView x:Name="assistView">
    <syncfusion:SfAIAssistView.PromptLibrary>
        <syncfusion:SfPromptLibrary x:Name="promptLibrary"
                                    ItemsSource="{Binding Prompts}" />
    </syncfusion:SfAIAssistView.PromptLibrary>
</syncfusion:SfAIAssistView>
```

### C#

```csharp
assistView.PromptLibrary = new SfPromptLibrary
{
    ItemsSource = viewModel.Prompts
};
```

---

## Supply prompts via ItemsSource

`ItemsSource` accepts any `IEnumerable` of `PromptItem`. Section and topic navigation is
derived from the items themselves — no nested collection is required.

### XAML

```xaml
<syncfusion:SfPromptLibrary ItemsSource="{Binding Prompts}" />
```

### C#

```csharp
var prompts = new ObservableCollection<PromptItem>
{
    new PromptItem
    {
        Title = "Summarize the following text",
        Description = "Condense long passages into a short summary",
        Section = "Writing",
        Topic = "Summary",
        PromptContent = "Please summarize the following text:\n\n",
        Version = "1.0"
    },
    new PromptItem
    {
        Title = "Translate to French",
        Description = "Translate English text to French",
        Section = "Translation",
        Topic = "French",
        PromptContent = "Translate the following text to French:\n\n",
        Version = "1.0"
    }
};

promptLibrary.ItemsSource = prompts;
```

---

## Handle selection

React to a prompt being selected either through the `PromptSelected` event or the
`PromptSelectedCommand`. The selected `PromptItem` is delivered via
`PromptSelectedEventArgs.Prompt`.

### XAML

```xaml
<syncfusion:SfPromptLibrary ItemsSource="{Binding Prompts}"
                            PromptSelectedCommand="{Binding InsertPromptCommand}" />
```

### C#

```csharp
// Event
promptLibrary.PromptSelected += (sender, e) =>
{
    var selected = e.Prompt;
    // selected.Title, selected.PromptContent, selected.Section, etc.
};

// Command (MVVM)
promptLibrary.PromptSelectedCommand = new Command<PromptItem>(prompt =>
{
    // Insert prompt.PromptContent into the request editor
});
```

---

## PromptItem model

| Property | Type | Description |
|---|---|---|
| `Title` | `string` | The title displayed on the prompt card |
| `Description` | `string` | A short description of the prompt |
| `Section` | `string` | The section used to group the prompt in the library |
| `Topic` | `string` | The optional topic used to filter the prompt |
| `PromptContent` | `string` | The prompt text inserted when the item is selected |
| `Version` | `string` | The prompt version (default `"1.0"`) |

---

## Bindable properties

- `ItemsSource` (`IEnumerable`) — the prompt catalog
- `PromptSelectedCommand` (`ICommand`) — executed on prompt selection

## Events

- `PromptSelected` (`PromptSelectedEventArgs`) — fired on prompt selection; the `Prompt`
  property returns the chosen `PromptItem`.
