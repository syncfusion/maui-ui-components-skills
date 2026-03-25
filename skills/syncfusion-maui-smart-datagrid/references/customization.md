# Customization and Configuration

## Table of Contents
- [Suggested Prompts](#suggested-prompts)
- [Initial Prompt](#initial-prompt)
- [Enable/Disable Smart Actions](#enabledisable-smart-actions)
- [Programmatic AssistView Control](#programmatic-assistview-control)
- [Dynamic AI Operations](#dynamic-ai-operations)
- [Event Handling](#event-handling)

## Suggested Prompts

The `SuggestedPrompts` property provides predefined suggestions that appear in the AssistView banner. Users can click suggestions instead of typing commands, reducing learning curve and encouraging feature discovery.

### XAML Configuration

```xaml
<syncfusion:SfSmartDataGrid ItemsSource="{Binding OrderInfoCollection}">
    <syncfusion:SfSmartDataGrid.AssistViewSettings>
        <syncfusion:DataGridAssistViewSettings SuggestedPrompts="{Binding Suggestions}" />
    </syncfusion:SfSmartDataGrid.AssistViewSettings>
</syncfusion:SfSmartDataGrid>
```

### ViewModel Setup

Create suggestions in your ViewModel:

```csharp
using Syncfusion.Maui.AIAssistView;

public class OrderInfoRepository
{
    public ObservableCollection<ISuggestion> Suggestions { get; set; } = new()
    {
        new AssistSuggestion() { Text = "Sort by OrderID ascending" },
        new AssistSuggestion() { Text = "Filter ShipCountry equals Germany" },
        new AssistSuggestion() { Text = "Group by ShipCountry" },
        new AssistSuggestion() { Text = "Highlight rows where Total > 5000" }
    };

    public ObservableCollection<OrderInfo> OrderInfoCollection { get; set; }

    public OrderInfoRepository()
    {
        OrderInfoCollection = new();
        GenerateOrders();
    }
}
```

### Code-Behind Configuration

If using code-behind instead of binding:

```csharp
var suggestions = new ObservableCollection<ISuggestion>
{
    new AssistSuggestion() { Text = "Show high-value orders" },
    new AssistSuggestion() { Text = "Group by region" },
    new AssistSuggestion() { Text = "Sort alphabetically" }
};

dataGrid.AssistViewSettings.SuggestedPrompts = suggestions;
```

### Best Practices

- **Keep 3-5 suggestions:** Too many overwhelms users; too few doesn't help
- **Use specific commands:** Avoid vague suggestions like "sort data"
- **Match user workflows:** Include suggestions for common tasks in your domain
- **Update dynamically:** Change suggestions based on data or user role

## Initial Prompt

The `Prompt` property auto-executes a command when the AssistView opens for the first time. Useful for initializing grid state automatically.

### XAML Configuration

```xaml
<syncfusion:SfSmartDataGrid ItemsSource="{Binding OrderInfoCollection}">
    <syncfusion:SfSmartDataGrid.AssistViewSettings>
        <syncfusion:DataGridAssistViewSettings Prompt="Sort by OrderID descending" />
    </syncfusion:SfSmartDataGrid.AssistViewSettings>
</syncfusion:SfSmartDataGrid>
```

### Code-Behind Configuration

```csharp
dataGrid.AssistViewSettings.Prompt = "Group by ShipCountry and sort by OrderID ascending";
```

### Use Cases

```csharp
// Scenario 1: Sales dashboard - show recent orders
dataGrid.AssistViewSettings.Prompt = "Sort by OrderDate descending";

// Scenario 2: Regional view - group by location
dataGrid.AssistViewSettings.Prompt = "Group by ShipCountry";

// Scenario 3: High-value alert - highlight large orders
dataGrid.AssistViewSettings.Prompt = "Highlight rows where Total greaterThan 10000 color Gold";

// Scenario 4: Data validation - show pending items
dataGrid.AssistViewSettings.Prompt = "Filter Status equals Pending";
```

### Important Notes

- Prompt executes only on **first AssistView opening**
- After user closes AssistView, prompt no longer auto-executes
- To apply operation on app startup (not AssistView), use `GetResponseAsync()` instead

## Enable/Disable Smart Actions

The `EnableSmartActions` property controls whether AI operations are applied to the grid. Set to `false` to preview operations without modifying data, or to prevent unintended changes.

### XAML Configuration

```xaml
<!-- Enable smart actions (default behavior) -->
<syncfusion:SfSmartDataGrid ItemsSource="{Binding OrderInfoCollection}">
    <syncfusion:SfSmartDataGrid.AssistViewSettings>
        <syncfusion:DataGridAssistViewSettings EnableSmartActions="True" />
    </syncfusion:SfSmartDataGrid.AssistViewSettings>
</syncfusion:SfSmartDataGrid>

<!-- Disable smart actions - AI responds but doesn't modify grid -->
<syncfusion:SfSmartDataGrid ItemsSource="{Binding OrderInfoCollection}">
    <syncfusion:SfSmartDataGrid.AssistViewSettings>
        <syncfusion:DataGridAssistViewSettings EnableSmartActions="False" />
    </syncfusion:SfSmartDataGrid.AssistViewSettings>
</syncfusion:SfSmartDataGrid>
```

### Code-Behind Configuration

```csharp
// Enable smart actions
dataGrid.AssistViewSettings.EnableSmartActions = true;

// Disable smart actions for preview mode
dataGrid.AssistViewSettings.EnableSmartActions = false;
```

### Use Cases

**Enable (Default):**
- Production applications where users need immediate results
- Dashboard applications for interactive exploration
- Report generators that apply changes

**Disable:**
- Admin audit mode - review commands before execution
- Training/demo mode - show what would happen without modifying data
- Restricted data views - allow users to request but not access changes

### Code Example

```csharp
private void OnToggleSmartActionsClicked(object sender, EventArgs e)
{
    dataGrid.AssistViewSettings.EnableSmartActions = !dataGrid.AssistViewSettings.EnableSmartActions;
    
    string status = dataGrid.AssistViewSettings.EnableSmartActions ? "ENABLED" : "DISABLED";
    DisplayAlert("Smart Actions", $"Smart actions {status}", "OK");
}
```

## Programmatic AssistView Control

Show or hide the AssistView popup from code using `ShowAssistView()` and `CloseAssistView()` methods.

### Show AssistView

```csharp
// Show relative to default assist button
dataGrid.ShowAssistView();

// Show relative to a specific view (e.g., custom button)
private void OnCustomAssistButtonClicked(object sender, EventArgs e)
{
    dataGrid.ShowAssistView(customButton);
}
```

### Close AssistView

```csharp
// Close the popup
dataGrid.CloseAssistView();

// Close after operation completes
private async void OnApplyFilterClicked(object sender, EventArgs e)
{
    await dataGrid.GetResponseAsync("Filter ShipCountry equals Germany");
    dataGrid.CloseAssistView();
}
```

### Practical Examples

```csharp
// Example 1: Open AssistView when user clicks toolbar icon
private void OnAssistToolbarClicked(object sender, EventArgs e)
{
    dataGrid.ShowAssistView();
}

// Example 2: Show AssistView relative to button user clicked
private void OnContextMenuAssist(object sender, EventArgs e)
{
    if (sender is Button button)
    {
        dataGrid.ShowAssistView(button);
    }
}

// Example 3: Auto-close AssistView after quick action
private async void OnQuickSortClicked(object sender, EventArgs e)
{
    await dataGrid.GetResponseAsync("Sort by OrderID ascending");
    
    // Close after a delay for UX feedback
    await Task.Delay(500);
    dataGrid.CloseAssistView();
}

// Example 4: Show AssistView on context menu long-press (mobile)
private void OnGridLongPress(object sender, EventArgs e)
{
    dataGrid.ShowAssistView();
}
```

## Dynamic AI Operations

Use `GetResponseAsync()` to apply AI operations programmatically without showing the AssistView popup. Ideal for quick actions, batch operations, or server-triggered changes.

### Basic Usage

```csharp
// Single operation
private async void OnSortButtonClicked(object sender, EventArgs e)
{
    await dataGrid.GetResponseAsync("Sort by OrderID descending");
}

// Multiple sequential operations
private async void OnAnalyzeDataClicked(object sender, EventArgs e)
{
    await dataGrid.GetResponseAsync("Filter ShipCountry equals Germany");
    await dataGrid.GetResponseAsync("Group by ShipCity");
    await dataGrid.GetResponseAsync("Sort by OrderID ascending");
}
```

### Advanced Pattern: Batch Operations

```csharp
private async void OnBatchAnalysis(object sender, EventArgs e)
{
    try
    {
        // Apply multiple operations
        var operations = new List<string>
        {
            "Filter Revenue greaterThan 5000",
            "Group by Category and Subcategory",
            "Sort by OrderDate descending",
            "Highlight rows where Status = Pending color Yellow"
        };

        foreach (var operation in operations)
        {
            await dataGrid.GetResponseAsync(operation);
            await Task.Delay(100); // Visual feedback between operations
        }

        // Show completion message
        await DisplayAlert("Analysis Complete", "Grid analysis finished", "OK");
    }
    catch (Exception ex)
    {
        await DisplayAlert("Error", $"Operation failed: {ex.Message}", "OK");
    }
}
```

### Pattern: User-Triggered Advanced Queries

```csharp
// User selects options from picker controls
private async void OnApplyFilterClicked(object sender, EventArgs e)
{
    var country = countryPicker.SelectedItem?.ToString() ?? "";
    var minRevenue = int.Parse(minRevenuePicker.SelectedItem?.ToString() ?? "0");

    if (!string.IsNullOrEmpty(country))
    {
        var command = $"Filter ShipCountry equals {country} AND Revenue greaterThan {minRevenue}";
        await dataGrid.GetResponseAsync(command);
    }
}
```

## Event Handling

Handle AssistView lifecycle events to validate, modify, or log user interactions.

### AssistViewRequest Event

Triggered when user sends a prompt. Allows validation, modification, or cancellation:

```xaml
<syncfusion:SfSmartDataGrid ItemsSource="{Binding OrderInfoCollection}"
                           AssistViewRequest="OnAssistRequest">
</syncfusion:SfSmartDataGrid>
```

```csharp
private void OnAssistRequest(object sender, AssistViewRequestEventArgs e)
{
    var prompt = e.Prompt;
    
    // Log user interaction
    Debug.WriteLine($"User command: {prompt}");
    
    // Validate command (example: disallow delete operations)
    if (prompt.ToLower().Contains("delete"))
    {
        DisplayAlert("Not Allowed", "Delete operations are restricted", "OK");
        e.Cancel = true; // Prevent operation
        return;
    }
    
    // Allow operation
}
```

### AssistViewOpening Event

Triggered before AssistView popup opens. Prevent opening or prepare UI:

```xaml
<syncfusion:SfSmartDataGrid ItemsSource="{Binding OrderInfoCollection}">
    <syncfusion:SfSmartDataGrid.AssistViewSettings>
        <syncfusion:DataGridAssistViewSettings AssistViewOpening="OnAssistOpening" />
    </syncfusion:SfSmartDataGrid.AssistViewSettings>
</syncfusion:SfSmartDataGrid>
```

```csharp
private void OnAssistOpening(object sender, AssistViewOpeningEventArgs e)
{
    Debug.WriteLine("AssistView opening");
    
    // Check permissions before allowing access
    if (!HasAIAccess())
    {
        DisplayAlert("Access Denied", "AI features not available", "OK");
        e.Cancel = true;
        return;
    }
    
    // Update suggestions based on current context
    LoadContextualSuggestions();
}

private bool HasAIAccess()
{
    // Check user role/permissions
    return Preferences.Get("ai_enabled", false);
}
```

### AssistViewClosing Event

Triggered before AssistView closes. Save state or validate completeness:

```xaml
<syncfusion:SfSmartDataGrid ItemsSource="{Binding OrderInfoCollection}">
    <syncfusion:SfSmartDataGrid.AssistViewSettings>
        <syncfusion:DataGridAssistViewSettings AssistViewClosing="OnAssistClosing" />
    </syncfusion:SfSmartDataGrid.AssistViewSettings>
</syncfusion:SfSmartDataGrid>
```

```csharp
private void OnAssistClosing(object sender, AssistViewClosingEventArgs e)
{
    Debug.WriteLine("AssistView closing");
    
    // Save grid state
    SaveGridState();
    
    // Trigger export or reporting
    if (shouldExportOnClose)
    {
        ExportCurrentView();
    }
}

private void SaveGridState()
{
    // Serialize current sort/filter/group state to preferences
    var state = new GridState
    {
        SortColumns = dataGrid.SortColumns,
        FilteredColumns = dataGrid.FilteredColumns
    };
    Preferences.Set("last_grid_state", JsonConvert.SerializeObject(state));
}
```

### Complete Event Handling Example

```csharp
public class DataGridViewModel
{
    public void SetupEventHandlers(SfSmartDataGrid grid)
    {
        // Request event - validate before execution
        grid.AssistViewRequest += (sender, e) =>
        {
            Debug.WriteLine($"Request: {e.Prompt}");
            
            // Example: Log all operations
            LogOperation(e.Prompt);
            
            // Example: Prevent certain operations
            if (IsSensitiveOperation(e.Prompt))
            {
                e.Cancel = true;
                ShowWarning("This operation requires approval");
            }
        };

        // Opening event - prepare UI
        grid.AssistViewSettings.AssistViewOpening += (sender, e) =>
        {
            Debug.WriteLine("Assist opening");
            RefreshSuggestions();
        };

        // Closing event - cleanup
        grid.AssistViewSettings.AssistViewClosing += (sender, e) =>
        {
            Debug.WriteLine("Assist closing");
            SaveViewState();
        };
    }

    private void LogOperation(string prompt)
    {
        // Send to analytics service
        AnalyticsService.LogUserAction("grid_assist", prompt);
    }

    private bool IsSensitiveOperation(string prompt)
    {
        var sensitive = new[] { "delete", "export", "share", "modify" };
        return sensitive.Any(s => prompt.ToLower().Contains(s));
    }

    private void ShowWarning(string message) => MainThread.BeginInvokeOnMainThread(() =>
    {
        Application.Current?.MainPage?.DisplayAlert("Warning", message, "OK");
    });

    private void RefreshSuggestions() { /* Update suggestions */ }
    private void SaveViewState() { /* Save state */ }
}
```
