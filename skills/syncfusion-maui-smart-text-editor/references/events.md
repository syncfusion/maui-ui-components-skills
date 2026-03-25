# Events

`SfSmartTextEditor` exposes a `TextChanged` event and a `TextChangedCommand` property to react to text changes — use the event for code-behind patterns and the command for MVVM / data-binding patterns.

---

## TextChanged Event

`TextChanged` fires whenever the editor's text changes. The event args provide both the old and new text values, so you can compare, validate, or react to the change.

**EventArgs properties:**
| Property | Type | Description |
|----------|------|-------------|
| `NewTextValue` | `string` | The updated text after the change |
| `OldTextValue` | `string` | The text before the change |

### XAML Subscription

```xml
<smarttexteditor:SfSmartTextEditor
    x:Name="smartTextEditor"
    TextChanged="OnTextChanged" />
```

### Code-Behind Handler

```csharp
private void OnTextChanged(object sender, Syncfusion.Maui.SmartComponents.TextChangedEventArgs e)
{
    string previous = e.OldTextValue;
    string current  = e.NewTextValue;

    // Example: update a character counter label
    CharacterCountLabel.Text = $"{current.Length} / 300";
}
```

### C# Subscription (no XAML)

```csharp
smartTextEditor.TextChanged += OnTextChanged;
```

---

## TextChangedCommand (MVVM)

`TextChangedCommand` is the bindable command equivalent of `TextChanged`. Bind it to an `ICommand` in your ViewModel to keep your UI and logic cleanly separated.

### XAML Binding

```xml
<ContentPage.BindingContext>
    <local:SmartTextEditorViewModel />
</ContentPage.BindingContext>

<smarttexteditor:SfSmartTextEditor
    x:Name="smartTextEditor"
    TextChangedCommand="{Binding TextChangedCommand}" />
```

### ViewModel

```csharp
public class SmartTextEditorViewModel
{
    public ICommand TextChangedCommand { get; }

    public SmartTextEditorViewModel()
    {
        TextChangedCommand = new Command(OnTextChanged);
    }

    private void OnTextChanged()
    {
        // React to text change — use Text binding on the editor
        // to read the current value in your ViewModel
    }
}
```

> `TextChangedCommand` does not pass the new/old text values as a parameter. For value access in MVVM, bind `Text` to a ViewModel property alongside `TextChangedCommand`:

```xml
<smarttexteditor:SfSmartTextEditor
    Text="{Binding ReplyText, Mode=TwoWay}"
    TextChangedCommand="{Binding TextChangedCommand}" />
```

```csharp
public class SmartTextEditorViewModel : INotifyPropertyChanged
{
    private string _replyText = string.Empty;

    public string ReplyText
    {
        get => _replyText;
        set
        {
            _replyText = value;
            OnPropertyChanged();
        }
    }

    public ICommand TextChangedCommand { get; }

    public SmartTextEditorViewModel()
    {
        TextChangedCommand = new Command(() =>
        {
            // ReplyText already has the latest value via two-way binding
            int remaining = 300 - ReplyText.Length;
        });
    }

    public event PropertyChangedEventHandler? PropertyChanged;
    protected void OnPropertyChanged([CallerMemberName] string? name = null)
        => PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
}
```

---

## Common Patterns

### Character counter
```csharp
private void OnTextChanged(object sender, TextChangedEventArgs e)
{
    int remaining = 300 - e.NewTextValue.Length;
    CounterLabel.Text = $"{remaining} characters remaining";
}
```

### Auto-save on change
```csharp
private async void OnTextChanged(object sender, TextChangedEventArgs e)
{
    await _draftService.SaveDraftAsync(e.NewTextValue);
}
```

### Input validation
```csharp
private void OnTextChanged(object sender, TextChangedEventArgs e)
{
    bool isValid = e.NewTextValue.Length >= 10;
    SubmitButton.IsEnabled = isValid;
}
```
