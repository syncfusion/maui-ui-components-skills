# Troubleshooting .NET MAUI Autocomplete

Common issues, platform-specific behaviors, and solutions for the Syncfusion .NET MAUI Autocomplete control.

## Installation and Setup Issues

### Control Not Appearing

**Problem:** Autocomplete control doesn't render or is invisible.

**Solutions:**
1. Verify `ConfigureSyncfusionCore()` is called in `MauiProgram.cs`:
   ```csharp
   builder.ConfigureSyncfusionCore()
   ```

2. Check NuGet package is installed:
   ```bash
   dotnet list package
   # Should show: Syncfusion.Maui.Inputs
   ```

3. Verify namespace is correct:
   ```xml
   xmlns:editors="clr-namespace:Syncfusion.Maui.Inputs;assembly=Syncfusion.Maui.Inputs"
   ```

4. Check control dimensions:
   ```xml
   <editors:SfAutocomplete WidthRequest="250" HeightRequest="50" />
   ```

### Items Not Displaying

**Problem:** Dropdown is empty or items don't show.

**Solutions:**
1. Set `DisplayMemberPath` for complex objects:
   ```csharp
   autocomplete.DisplayMemberPath = "Name";
   ```

2. Verify ItemsSource is not null:
   ```csharp
   if (autocomplete.ItemsSource == null)
       Console.WriteLine("ItemsSource is null!");
   ```

3. Check data binding context:
   ```xml
   <ContentPage.BindingContext>
       <local:ViewModel />
   </ContentPage.BindingContext>
   ```

## Platform-Specific Issues

### Android Limitations

**HorizontalTextAlignment Dynamic Changes**

**Problem:** Changing `HorizontalTextAlignment` at runtime doesn't work.

**Solution:** Set alignment in XAML or during initialization:
```xml
<editors:SfAutocomplete HorizontalTextAlignment="Center" />
```

**Workaround:** Recreate control if alignment must change dynamically.

---

**Completed Event Not Supported**

**Problem:** `Completed` event doesn't fire on Android.

**Solution:** Use `ValueChanged` or `SelectionChanged` events instead:
```csharp
autocomplete.ValueChanged += (sender, e) =>
{
    // Handle value changes
};
```

---

**CursorPosition Two-Way Binding**

**Problem:** Two-way binding on `CursorPosition` doesn't work on Android.

**Solution:** Use one-way binding or set programmatically:
```csharp
autocomplete.CursorPosition = 4; // One-way only
```

### iOS Considerations

**AOT Publishing**

**Problem:** App crashes with `MissingMethodException` when published with AOT.

**Solution:** Add `[Preserve(AllMembers = true)]` to model classes:
```csharp
using System.Runtime.Serialization;

[Preserve(AllMembers = true)]
public class SocialMedia
{
    public string Name { get; set; }
    public int ID { get; set; }
}
```

**Why:** AOT compilation removes unused code. Preserve attribute ensures reflection works.

### Liquid Glass Effect Platform Requirements

**Problem:** Liquid Glass Effect not working.

**Requirements:**
- .NET 10 or later
- iOS 26 or macOS 26 or later

**Solution:** Check platform version and .NET version:
```csharp
#if NET10_0_OR_GREATER && (IOS || MACCATALYST)
    autocomplete.EnableLiquidGlassEffect = true;
#endif
```

**Fallback:** Provide alternative styling for unsupported platforms.

## Data Binding Issues

### DisplayMemberPath Not Working

**Problem:** Custom objects display class name instead of property value.

**Solution:** Ensure both `DisplayMemberPath` and `TextMemberPath` are set:
```xml
<editors:SfAutocomplete DisplayMemberPath="Name"
                        TextMemberPath="Name"
                        ItemsSource="{Binding SocialMedias}" />
```

**Note:** For simple string collections, these are not needed.

### SelectedItem Not Updating

**Problem:** `SelectedItem` binding doesn't update ViewModel.

**Solutions:**
1. Use two-way binding:
   ```xml
   <editors:SfAutocomplete SelectedItem="{Binding SelectedMedia, Mode=TwoWay}" />
   ```

2. Implement INotifyPropertyChanged in ViewModel:
   ```csharp
   private SocialMedia _selectedMedia;
   public SocialMedia SelectedMedia
   {
       get => _selectedMedia;
       set
       {
           _selectedMedia = value;
           OnPropertyChanged(nameof(SelectedMedia));
       }
   }
   ```

## Performance Issues

### Slow Performance with Large Datasets

**Problem:** Control lags with thousands of items.

**Solutions:**

1. **Use MinimumPrefixCharacters:**
   ```xml
   <editors:SfAutocomplete MinimumPrefixCharacters="3" />
   ```

2. **Implement LoadMore:**
   ```xml
   <editors:SfAutocomplete MaximumSuggestion="5" />
   ```

3. **Use Async Filtering:**
   ```csharp
   public async Task<object> GetMatchingItemsAsync(SfAutocomplete source, AutocompleteFilterInfo filterInfo)
   {
       return await Task.Run(() =>
       {
           // Perform filtering on background thread
           return filteredItems;
       });
   }
   ```

4. **Optimize ItemTemplate:**
   - Keep templates simple
   - Avoid complex layouts
   - Use appropriate image sizes

### Memory Leaks

**Problem:** Memory usage increases over time.

**Solutions:**

1. **Dispose CancellationTokenSource:**
   ```csharp
   cancellationTokenSource?.Cancel();
   cancellationTokenSource?.Dispose();
   cancellationTokenSource = new CancellationTokenSource();
   ```

2. **Unsubscribe from events:**
   ```csharp
   protected override void OnDisappearing()
   {
       autocomplete.SelectionChanged -= OnSelectionChanged;
       base.OnDisappearing();
   }
   ```

3. **Clear collections:**
   ```csharp
   FilteredItems?.Clear();
   ```

## Filtering and Search Issues

### Custom Filter Not Working

**Problem:** Custom `IAutocompleteFilterBehavior` not applied.

**Solutions:**

1. Verify interface implementation:
   ```csharp
   public class CustomFilter : IAutocompleteFilterBehavior
   {
       public async Task<object> GetMatchingItemsAsync(SfAutocomplete source, AutocompleteFilterInfo filterInfo)
       {
           // Implementation
       }
   }
   ```

2. Check return type (not null):
   ```csharp
   return await Task.FromResult(filteredItems ?? new List<Item>());
   ```

3. Verify FilterBehavior assignment:
   ```xml
   <editors:SfAutocomplete.FilterBehavior>
       <local:CustomFilter />
   </editors:SfAutocomplete.FilterBehavior>
   ```

### Async Filtering Cancellation Issues

**Problem:** Previous async operations not cancelling.

**Solution:** Properly manage CancellationTokenSource:
```csharp
private CancellationTokenSource _cts;

public async Task<object> GetMatchingItemsAsync(SfAutocomplete source, AutocompleteFilterInfo filterInfo)
{
    // Cancel previous operation
    _cts?.Cancel();
    _cts?.Dispose();
    _cts = new CancellationTokenSource();
    
    try
    {
        return await Task.Run(() =>
        {
            _cts.Token.ThrowIfCancellationRequested();
            // Filtering logic
            return results;
        }, _cts.Token);
    }
    catch (OperationCanceledException)
    {
        return new List<Item>();
    }
}
```

## UI and Styling Issues

### Dropdown Not Showing

**Problem:** Dropdown doesn't appear when typing.

**Solutions:**

1. Check `MinimumPrefixCharacters`:
   ```xml
   <editors:SfAutocomplete MinimumPrefixCharacters="1" />
   ```

2. Verify `DropDownPlacement`:
   ```xml
   <editors:SfAutocomplete DropDownPlacement="Auto" />
   ```

3. Check available screen space (use `Auto` placement).

4. Ensure ItemsSource has items:
   ```csharp
   if (ItemsSource?.Count > 0)
       Console.WriteLine($"Items: {ItemsSource.Count}");
   ```

### Token Display Issues

**Problem:** Tokens not wrapping or displaying incorrectly.

**Solutions:**

1. Verify mode settings:
   ```xml
   <editors:SfAutocomplete SelectionMode="Multiple"
                           MultiSelectionDisplayMode="Token"
                           TokensWrapMode="Wrap" />
   ```

2. Enable AutoSize for dynamic height:
   ```xml
   <editors:SfAutocomplete EnableAutoSize="True" />
   ```

3. Check parent container constraints.

### Text Highlighting Not Working

**Problem:** Matched text not highlighted.

**Solutions:**

1. Set `TextHighlightMode`:
   ```xml
   <editors:SfAutocomplete TextHighlightMode="FirstOccurrence"
                           HighlightedTextColor="Red" />
   ```

2. For multiple occurrences, use Contains search:
   ```xml
   <editors:SfAutocomplete TextSearchMode="Contains"
                           TextHighlightMode="MultipleOccurrence" />
   ```

## Event Handling Issues

### Events Not Firing

**Problem:** SelectionChanged or other events don't trigger.

**Solutions:**

1. Verify event subscription:
   ```xml
   <editors:SfAutocomplete SelectionChanged="OnSelectionChanged" />
   ```

2. Check method signature:
   ```csharp
   private void OnSelectionChanged(object sender, SelectionChangedEventArgs e)
   {
       // Correct signature
   }
   ```

3. Ensure control has focus and is interactive.

### SelectionChanging Cancel Not Working

**Problem:** Setting `e.Cancel = true` doesn't prevent selection.

**Solution:** Use `SelectionChanging` (not `SelectionChanged`):
```csharp
private void OnSelectionChanging(object sender, SelectionChangingEventArgs e)
{
    if (SomeCondition)
    {
        e.Cancel = true;  // Prevents selection
    }
}
```

## Debugging Tips

1. **Enable Diagnostic Logging:**
   ```csharp
   autocomplete.SelectionChanged += (s, e) =>
   {
       Console.WriteLine($"Added: {e.AddedItems.Count}, Removed: {e.RemovedItems.Count}");
   };
   ```

2. **Check Control State:**
   ```csharp
   Console.WriteLine($"ItemsSource Count: {autocomplete.ItemsSource?.Count}");
   Console.WriteLine($"Is DropDownOpen: {autocomplete.IsDropDownOpen}");
   Console.WriteLine($"Selected Item: {autocomplete.SelectedItem}");
   ```

3. **Verify Binding:**
   ```xml
   <!-- Add FallbackValue to detect binding issues -->
   <editors:SfAutocomplete ItemsSource="{Binding Items, FallbackValue={}}" />
   ```

4. **Use Breakpoints** in custom filter methods to trace execution.

## Common Error Messages

### "Cannot resolve type 'SfAutocomplete'"

**Cause:** Namespace not imported or NuGet package not installed.

**Solution:**
```xml
xmlns:editors="clr-namespace:Syncfusion.Maui.Inputs;assembly=Syncfusion.Maui.Inputs"
```

### "System.NullReferenceException in ItemsSource"

**Cause:** ItemsSource is null or not initialized.

**Solution:**
```csharp
if (ViewModel.Items == null)
    ViewModel.Items = new ObservableCollection<Item>();
```

### "MissingMethodException" on iOS AOT

**Cause:** Methods stripped during AOT compilation.

**Solution:**
```csharp
[Preserve(AllMembers = true)]
public class YourModel { }
```

## Best Practices

1. **Always handle exceptions** in custom filters and event handlers
2. **Dispose resources** properly (CancellationTokenSource, subscriptions)
3. **Test on all target platforms** - behavior varies
4. **Use async/await** for expensive operations
5. **Implement offline fallbacks** for AI-powered features
6. **Monitor performance** with large datasets
7. **Follow platform guidelines** for UI/UX consistency

## Getting Help

If issues persist:

1. Check Syncfusion documentation: https://help.syncfusion.com/maui/autocomplete/overview
2. Search Syncfusion forums
3. Review GitHub samples: https://github.com/SyncfusionExamples
4. Contact Syncfusion support with:
   - Platform and .NET version
   - Minimal reproducible code
   - Error messages and stack traces
   - Expected vs actual behavior
