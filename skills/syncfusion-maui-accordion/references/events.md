# Events in .NET MAUI Accordion

This guide covers the four built-in events in the Syncfusion .NET MAUI Accordion, enabling you to respond to user interactions and control expansion/collapse behavior.

## Overview

The SfAccordion control provides four events for handling accordion item state changes:

| Event | Timing | Cancellable | Use Case |
|-------|--------|-------------|----------|
| **Expanding** | Before expansion begins | ✅ Yes | Validate before expanding, prevent expansion |
| **Expanded** | After expansion completes | ❌ No | Track expansion, load data, analytics |
| **Collapsing** | Before collapse begins | ✅ Yes | Validate before collapsing, prevent collapse |
| **Collapsed** | After collapse completes | ❌ No | Track collapse, clean up resources |

**Common Event Arguments:**
- `Index` - The index of the affected accordion item
- `Cancel` - Boolean to prevent the action (Expanding/Collapsing only)

## Expanding Event

The `Expanding` event fires **before** an accordion item begins to expand. You can cancel the expansion by setting `e.Cancel = true`.

**Event Signature:**
```csharp
public event EventHandler<ExpandingAndCollapsingEventArgs> Expanding;
```

**EventArgs Properties:**
- `Index` (int) - The zero-based index of the item being expanded
- `Cancel` (bool) - Set to `true` to cancel expansion

### Basic Usage

**XAML:**
```xml
<syncfusion:SfAccordion x:Name="accordion" Expanding="Accordion_Expanding">
    <syncfusion:SfAccordion.Items>
        <syncfusion:AccordionItem>
            <syncfusion:AccordionItem.Header>
                <Grid HeightRequest="48">
                    <Label Text="Section 1" Margin="16,14,0,14" />
                </Grid>
            </syncfusion:AccordionItem.Header>
            <syncfusion:AccordionItem.Content>
                <Grid Padding="16">
                    <Label Text="Content 1" />
                </Grid>
            </syncfusion:AccordionItem.Content>
        </syncfusion:AccordionItem>
    </syncfusion:SfAccordion.Items>
</syncfusion:SfAccordion>
```

**C#:**
```csharp
private void Accordion_Expanding(object sender, ExpandingAndCollapsingEventArgs e)
{
    // Log the expansion attempt
    Debug.WriteLine($"Item at index {e.Index} is about to expand");
}
```

### Cancelling Expansion

Prevent specific items from expanding:

```csharp
private void Accordion_Expanding(object sender, ExpandingAndCollapsingEventArgs e)
{
    // Prevent index 2 from expanding
    if (e.Index == 2)
    {
        e.Cancel = true;
        DisplayAlert("Restricted", "This section cannot be expanded", "OK");
    }
}
```

### Use Cases

**1. Permission Validation:**
```csharp
private void Accordion_Expanding(object sender, ExpandingAndCollapsingEventArgs e)
{
    if (!UserHasPermission(e.Index))
    {
        e.Cancel = true;
        DisplayAlert("Access Denied", "You don't have permission to view this section", "OK");
    }
}
```

**2. Lazy Loading Data:**
```csharp
private void Accordion_Expanding(object sender, ExpandingAndCollapsingEventArgs e)
{
    // Load data before expansion
    var item = accordion.Items[e.Index];
    if (item.Tag == null) // First time expanding
    {
        LoadDataForItem(e.Index);
        item.Tag = "loaded";
    }
}
```

**3. Single Expansion Enforcement (Custom):**
```csharp
private void Accordion_Expanding(object sender, ExpandingAndCollapsingEventArgs e)
{
    // Collapse all other items when one expands
    for (int i = 0; i < accordion.Items.Count; i++)
    {
        if (i != e.Index)
        {
            accordion.Items[i].IsExpanded = false;
        }
    }
}
```

## Expanded Event

The `Expanded` event fires **after** an accordion item has fully expanded. This event cannot be cancelled.

**Event Signature:**
```csharp
public event EventHandler<ExpandedAndCollapsedEventArgs> Expanded;
```

**EventArgs Properties:**
- `Index` (int) - The zero-based index of the expanded item

### Basic Usage

**XAML:**
```xml
<syncfusion:SfAccordion Expanded="Accordion_Expanded">
    <!-- Items -->
</syncfusion:SfAccordion>
```

**C#:**
```csharp
private void Accordion_Expanded(object sender, ExpandedAndCollapsedEventArgs e)
{
    Debug.WriteLine($"Item at index {e.Index} is now expanded");
}
```

### Use Cases

**1. Analytics Tracking:**
```csharp
private void Accordion_Expanded(object sender, ExpandedAndCollapsedEventArgs e)
{
    // Track which sections users view
    Analytics.LogEvent($"Accordion_Section_{e.Index}_Expanded");
}
```

**2. Load Heavy Content After Expansion:**
```csharp
private void Accordion_Expanded(object sender, ExpandedAndCollapsedEventArgs e)
{
    var item = accordion.Items[e.Index];
    if (item.Content is Grid grid)
    {
        // Load images or heavy content now that item is visible
        LoadImagesForSection(grid, e.Index);
    }
}
```

**3. Scroll to Top:**
```csharp
private void Accordion_Expanded(object sender, ExpandedAndCollapsedEventArgs e)
{
    // Automatically scroll expanded item to top
    accordion.BringIntoView(accordion.Items[e.Index]);
}
```

**4. Update UI Elements:**
```csharp
private void Accordion_Expanded(object sender, ExpandedAndCollapsedEventArgs e)
{
    // Update status label
    statusLabel.Text = $"Viewing: {GetSectionTitle(e.Index)}";
}
```

## Collapsing Event

The `Collapsing` event fires **before** an accordion item begins to collapse. You can cancel the collapse by setting `e.Cancel = true`.

**Event Signature:**
```csharp
public event EventHandler<ExpandingAndCollapsingEventArgs> Collapsing;
```

**EventArgs Properties:**
- `Index` (int) - The zero-based index of the item being collapsed
- `Cancel` (bool) - Set to `true` to cancel collapse

### Basic Usage

**XAML:**
```xml
<syncfusion:SfAccordion Collapsing="Accordion_Collapsing">
    <!-- Items -->
</syncfusion:SfAccordion>
```

**C#:**
```csharp
private void Accordion_Collapsing(object sender, ExpandingAndCollapsingEventArgs e)
{
    Debug.WriteLine($"Item at index {e.Index} is about to collapse");
}
```

### Cancelling Collapse

Prevent specific items from collapsing:

```csharp
private void Accordion_Collapsing(object sender, ExpandingAndCollapsingEventArgs e)
{
    // Keep first item always expanded
    if (e.Index == 0)
    {
        e.Cancel = true;
    }
}
```

### Use Cases

**1. Unsaved Changes Warning:**
```csharp
private void Accordion_Collapsing(object sender, ExpandingAndCollapsingEventArgs e)
{
    if (HasUnsavedChanges(e.Index))
    {
        var result = DisplayAlert("Unsaved Changes", 
            "You have unsaved changes. Collapse anyway?", 
            "Yes", "No");
        
        if (!result.Result)
        {
            e.Cancel = true;
        }
    }
}
```

**2. Form Validation:**
```csharp
private void Accordion_Collapsing(object sender, ExpandingAndCollapsingEventArgs e)
{
    if (!ValidateFormInSection(e.Index))
    {
        e.Cancel = true;
        DisplayAlert("Validation Error", "Please fill all required fields", "OK");
    }
}
```

**3. Keep Critical Sections Open:**
```csharp
private void Accordion_Collapsing(object sender, ExpandingAndCollapsingEventArgs e)
{
    // Prevent collapse of sections marked as critical
    var item = accordion.Items[e.Index];
    if (item.Tag as string == "critical")
    {
        e.Cancel = true;
        DisplayAlert("Notice", "This section must remain visible", "OK");
    }
}
```

## Collapsed Event

The `Collapsed` event fires **after** an accordion item has fully collapsed. This event cannot be cancelled.

**Event Signature:**
```csharp
public event EventHandler<ExpandedAndCollapsedEventArgs> Collapsed;
```

**EventArgs Properties:**
- `Index` (int) - The zero-based index of the collapsed item

### Basic Usage

**XAML:**
```xml
<syncfusion:SfAccordion Collapsed="Accordion_Collapsed">
    <!-- Items -->
</syncfusion:SfAccordion>
```

**C#:**
```csharp
private void Accordion_Collapsed(object sender, ExpandedAndCollapsedEventArgs e)
{
    Debug.WriteLine($"Item at index {e.Index} is now collapsed");
}
```

### Use Cases

**1. Clean Up Resources:**
```csharp
private void Accordion_Collapsed(object sender, ExpandedAndCollapsedEventArgs e)
{
    // Dispose of heavy resources when collapsed
    var item = accordion.Items[e.Index];
    if (item.Content is Grid grid)
    {
        UnloadImagesForSection(grid);
    }
}
```

**2. Reset UI State:**
```csharp
private void Accordion_Collapsed(object sender, ExpandedAndCollapsedEventArgs e)
{
    // Reset any UI elements associated with this section
    statusLabel.Text = "No section selected";
}
```

**3. Analytics Tracking:**
```csharp
private void Accordion_Collapsed(object sender, ExpandedAndCollapsedEventArgs e)
{
    // Track duration section was open
    var duration = DateTime.Now - sectionOpenTimes[e.Index];
    Analytics.LogEvent($"Section_{e.Index}_ViewDuration", duration.TotalSeconds);
}
```

## Complete Event Example

Here's a comprehensive example using all four events:

**MainPage.xaml:**
```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:syncfusion="clr-namespace:Syncfusion.Maui.Accordion;assembly=Syncfusion.Maui.Expander"
             x:Class="AccordionApp.MainPage">
    
    <StackLayout>
        <Label x:Name="statusLabel" 
               Text="No events yet" 
               Padding="16" 
               BackgroundColor="LightGray" />
        
        <syncfusion:SfAccordion x:Name="accordion"
                                Expanding="Accordion_Expanding"
                                Expanded="Accordion_Expanded"
                                Collapsing="Accordion_Collapsing"
                                Collapsed="Accordion_Collapsed">
            <syncfusion:SfAccordion.Items>
                <syncfusion:AccordionItem>
                    <syncfusion:AccordionItem.Header>
                        <Grid HeightRequest="48">
                            <Label Text="Public Section" Margin="16,14,0,14" />
                        </Grid>
                    </syncfusion:AccordionItem.Header>
                    <syncfusion:AccordionItem.Content>
                        <Grid Padding="16">
                            <Label Text="This section can expand/collapse freely" />
                        </Grid>
                    </syncfusion:AccordionItem.Content>
                </syncfusion:AccordionItem>
                
                <syncfusion:AccordionItem>
                    <syncfusion:AccordionItem.Header>
                        <Grid HeightRequest="48">
                            <Label Text="Restricted Section" Margin="16,14,0,14" />
                        </Grid>
                    </syncfusion:AccordionItem.Header>
                    <syncfusion:AccordionItem.Content>
                        <Grid Padding="16">
                            <Label Text="This section cannot be expanded" />
                        </Grid>
                    </syncfusion:AccordionItem.Content>
                </syncfusion:AccordionItem>
            </syncfusion:SfAccordion.Items>
        </syncfusion:SfAccordion>
    </StackLayout>
</ContentPage>
```

**MainPage.xaml.cs:**
```csharp
using Syncfusion.Maui.Accordion;

public partial class MainPage : ContentPage
{
    private Dictionary<int, DateTime> openTimes = new Dictionary<int, DateTime>();
    
    public MainPage()
    {
        InitializeComponent();
    }
    
    private void Accordion_Expanding(object sender, ExpandingAndCollapsingEventArgs e)
    {
        statusLabel.Text = $"Expanding item {e.Index}...";
        
        // Prevent index 1 from expanding
        if (e.Index == 1)
        {
            e.Cancel = true;
            statusLabel.Text = "Access Denied: Section 1 is restricted";
            DisplayAlert("Restricted", "This section cannot be expanded", "OK");
        }
    }
    
    private void Accordion_Expanded(object sender, ExpandedAndCollapsedEventArgs e)
    {
        statusLabel.Text = $"Item {e.Index} is now expanded";
        openTimes[e.Index] = DateTime.Now;
        
        // Track analytics
        Debug.WriteLine($"Section {e.Index} opened at {DateTime.Now}");
    }
    
    private void Accordion_Collapsing(object sender, ExpandingAndCollapsingEventArgs e)
    {
        statusLabel.Text = $"Collapsing item {e.Index}...";
        
        // Could add validation here if needed
    }
    
    private void Accordion_Collapsed(object sender, ExpandedAndCollapsedEventArgs e)
    {
        statusLabel.Text = $"Item {e.Index} is now collapsed";
        
        // Calculate view duration
        if (openTimes.ContainsKey(e.Index))
        {
            var duration = DateTime.Now - openTimes[e.Index];
            Debug.WriteLine($"Section {e.Index} was open for {duration.TotalSeconds:F1} seconds");
            openTimes.Remove(e.Index);
        }
    }
}
```

## Best Practices

### 1. Keep Event Handlers Fast
Avoid long-running operations in event handlers - use async methods or background tasks for heavy work.

### 2. Use Cancellable Events for Validation
Use `Expanding` and `Collapsing` events for validation before state changes.

### 3. Use Completion Events for Updates
Use `Expanded` and `Collapsed` events for updating UI or analytics after state changes.

### 4. Provide User Feedback
When cancelling an event, inform the user why with a dialog or message.

### 5. Avoid Recursive Calls
Don't modify `IsExpanded` inside event handlers for the same accordion - it can cause infinite loops.

## Troubleshooting

**Problem:** Events firing multiple times
**Solution:** Ensure you're not programmatically changing `IsExpanded` inside event handlers

**Problem:** Cancel doesn't work
**Solution:** Only `Expanding` and `Collapsing` support cancellation - `Expanded` and `Collapsed` do not

**Problem:** Index out of range error
**Solution:** Check `accordion.Items.Count` before accessing items by index

**Problem:** Event handler not firing
**Solution:** Verify the event is wired up in XAML or C# and the handler signature matches

## Additional Resources

- [.NET MAUI Event Handling](https://learn.microsoft.com/en-us/dotnet/maui/fundamentals/events)
- [Accordion Events Documentation](https://help.syncfusion.com/maui/accordion/events)
