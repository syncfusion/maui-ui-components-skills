# Animation and Events

## Table of Contents
- [Overview](#overview)
- [Animation Duration](#animation-duration)
- [Animation Easing](#animation-easing)
- [Programmatic Expand/Collapse](#programmatic-expandcollapse)
- [Expanding Event](#expanding-event)
- [Expanded Event](#expanded-event)
- [Collapsing Event](#collapsing-event)
- [Collapsed Event](#collapsed-event)
- [Event Usage Patterns](#event-usage-patterns)
- [Complete Examples](#complete-examples)

---

## Overview

The Syncfusion .NET MAUI Expander provides control over:
- **Animation timing:** Customize expand/collapse duration
- **Animation style:** Control easing for smooth transitions
- **Programmatic control:** Expand/collapse via code
- **Event system:** Four events for interaction control

**Event Flow:**
1. User taps header → `Expanding` event (cancellable)
2. Animation starts
3. Animation completes → `Expanded` event

OR

1. User taps header → `Collapsing` event (cancellable)
2. Animation starts
3. Animation completes → `Collapsed` event

---

## Animation Duration

The `AnimationDuration` property controls how long the expand/collapse animation takes (in milliseconds).

**Default:** 300ms

### XAML

```xml
<syncfusion:SfExpander x:Name="expander" 
                       AnimationDuration="250">
    <!-- Header and Content -->
</syncfusion:SfExpander>
```

### C#

```csharp
expander.AnimationDuration = 250;
```

### Recommended Values

| Duration | Use Case |
|----------|----------|
| 150-200ms | Fast, snappy transitions for simple content |
| 250-300ms | Balanced timing (default) |
| 400-600ms | Slower, more deliberate animations for complex content |
| 0ms | Instant expand/collapse (no animation) |

### Example: Different Speeds

```xml
<StackLayout Spacing="8">
    
    <!-- Fast animation -->
    <syncfusion:SfExpander AnimationDuration="150">
        <syncfusion:SfExpander.Header>
            <Grid><Label Text="Fast (150ms)"/></Grid>
        </syncfusion:SfExpander.Header>
        <syncfusion:SfExpander.Content>
            <Grid Padding="15"><Label Text="Quick content"/></Grid>
        </syncfusion:SfExpander.Content>
    </syncfusion:SfExpander>
    
    <!-- Normal animation -->
    <syncfusion:SfExpander AnimationDuration="300">
        <syncfusion:SfExpander.Header>
            <Grid><Label Text="Normal (300ms)"/></Grid>
        </syncfusion:SfExpander.Header>
        <syncfusion:SfExpander.Content>
            <Grid Padding="15"><Label Text="Standard content"/></Grid>
        </syncfusion:SfExpander.Content>
    </syncfusion:SfExpander>
    
    <!-- Slow animation -->
    <syncfusion:SfExpander AnimationDuration="500">
        <syncfusion:SfExpander.Header>
            <Grid><Label Text="Slow (500ms)"/></Grid>
        </syncfusion:SfExpander.Header>
        <syncfusion:SfExpander.Content>
            <Grid Padding="15"><Label Text="Smooth content"/></Grid>
        </syncfusion:SfExpander.Content>
    </syncfusion:SfExpander>
    
</StackLayout>
```

---

## Animation Easing

The `AnimationEasing` property controls the rate of change during animation (acceleration/deceleration curve).

**Default:** `Linear`

### Available Easing Options

- `Linear` - Constant speed throughout
- `SinOut` - Start fast, end slow (smooth deceleration)
- `SinIn` - Start slow, end fast
- `SinInOut` - Slow start and end, fast middle
- `None` - No animation to decelerate towards the final value.



### XAML

```xml
<syncfusion:SfExpander x:Name="expander"
                       AnimationEasing="SinOut">
    <!-- Header and Content -->
</syncfusion:SfExpander>
```

### C#

```csharp
expander.AnimationEasing = ExpanderAnimationEasing.SinOut;
```

### Comparison Example

```xml
<StackLayout Spacing="8">
    
    <syncfusion:SfExpander AnimationDuration="400" AnimationEasing="Linear">
        <syncfusion:SfExpander.Header>
            <Grid><Label Text="Linear - Constant speed"/></Grid>
        </syncfusion:SfExpander.Header>
        <syncfusion:SfExpander.Content>
            <Grid Padding="15"><Label Text="Even pace animation"/></Grid>
        </syncfusion:SfExpander.Content>
    </syncfusion:SfExpander>
    
    <syncfusion:SfExpander AnimationDuration="400" AnimationEasing="SinOut">
        <syncfusion:SfExpander.Header>
            <Grid><Label Text="SinOut - Smooth deceleration"/></Grid>
        </syncfusion:SfExpander.Header>
        <syncfusion:SfExpander.Content>
            <Grid Padding="15"><Label Text="Smooth ending"/></Grid>
        </syncfusion:SfExpander.Content>
    </syncfusion:SfExpander>
    
</StackLayout>
```

**Recommendation:** Use `SinOut` for the smoothest, most natural-feeling animations.

---

## Programmatic Expand/Collapse

The `IsExpanded` property controls whether content is visible.

**Default:** `false` (collapsed)

### XAML

```xml
<syncfusion:SfExpander x:Name="expander" 
                       IsExpanded="True">
    <!-- Header and Content -->
</syncfusion:SfExpander>
```

### C# - Setting Initial State

```csharp
expander.IsExpanded = true;  // Start expanded
```

### C# - Toggle Programmatically

```csharp
private void OnToggleClicked(object sender, EventArgs e)
{
    expander.IsExpanded = !expander.IsExpanded;
}
```

### Expand on Button Click

```xml
<StackLayout>
    <Button Text="Expand All" Clicked="OnExpandAllClicked"/>
    
    <syncfusion:SfExpander x:Name="expander1">
        <!-- Content -->
    </syncfusion:SfExpander>
    
    <syncfusion:SfExpander x:Name="expander2">
        <!-- Content -->
    </syncfusion:SfExpander>
</StackLayout>
```

```csharp
private void OnExpandAllClicked(object sender, EventArgs e)
{
    expander1.IsExpanded = true;
    expander2.IsExpanded = true;
}
```

---

## Expanding Event

**When fired:** Before the expander starts expanding (user taps collapsed header or `IsExpanded` set to `true`).

**Cancellable:** ✅ Yes - Set `e.Cancel = true` to prevent expansion.

### XAML

```xml
<syncfusion:SfExpander Expanding="OnExpanding">
    <!-- Header and Content -->
</syncfusion:SfExpander>
```

### C# - Basic Handler

```csharp
private void OnExpanding(object sender, ExpandingAndCollapsingEventArgs e)
{
    // Code to run before expansion
    Debug.WriteLine("Expander is about to expand");
}
```

### C# - Cancel Expansion

```csharp
private void OnExpanding(object sender, ExpandingAndCollapsingEventArgs e)
{
    // Prevent expansion based on condition
    if (someCondition)
    {
        e.Cancel = true;
        DisplayAlert("Warning", "Cannot expand at this time", "OK");
    }
}
```

### Use Case: Require Authentication

```csharp
private bool isUserAuthenticated = false;

private void OnExpanding(object sender, ExpandingAndCollapsingEventArgs e)
{
    if (!isUserAuthenticated)
    {
        e.Cancel = true;
        DisplayAlert("Authentication Required", 
                     "Please log in to view this content", 
                     "OK");
    }
}
```

---

## Expanded Event

**When fired:** After the expander finishes expanding (animation complete).

**Cancellable:** ❌ No

### XAML

```xml
<syncfusion:SfExpander Expanded="OnExpanded">
    <!-- Header and Content -->
</syncfusion:SfExpander>
```

### C# - Basic Handler

```csharp
private void OnExpanded(object sender, ExpandedAndCollapsedEventArgs e)
{
    // Code to run after expansion completes
    Debug.WriteLine("Expander is now fully expanded");
}
```

### Use Case: Load Data After Expansion

```csharp
private void OnExpanded(object sender, ExpandedAndCollapsedEventArgs e)
{
    // Load heavy data only after user expands
    LoadDetailedData();
}

private async void LoadDetailedData()
{
    var data = await FetchDataFromServer();
    // Bind data to content view
}
```

### Use Case: Analytics Tracking

```csharp
private void OnExpanded(object sender, ExpandedAndCollapsedEventArgs e)
{
    // Track user interaction
    Analytics.TrackEvent("Expander_Expanded", new Dictionary<string, string>
    {
        { "Section", "Invoice Details" },
        { "Timestamp", DateTime.Now.ToString() }
    });
}
```

---

## Collapsing Event

**When fired:** Before the expander starts collapsing (user taps expanded header or `IsExpanded` set to `false`).

**Cancellable:** ✅ Yes - Set `e.Cancel = true` to prevent collapse.

### XAML

```xml
<syncfusion:SfExpander Collapsing="OnCollapsing">
    <!-- Header and Content -->
</syncfusion:SfExpander>
```

### C# - Basic Handler

```csharp
private void OnCollapsing(object sender, ExpandingAndCollapsingEventArgs e)
{
    // Code to run before collapse
    Debug.WriteLine("Expander is about to collapse");
}
```

### C# - Cancel Collapse

```csharp
private void OnCollapsing(object sender, ExpandingAndCollapsingEventArgs e)
{
    // Prevent collapse based on condition
    if (hasUnsavedChanges)
    {
        e.Cancel = true;
        DisplayAlert("Warning", 
                     "You have unsaved changes. Save before closing.", 
                     "OK");
    }
}
```

### Use Case: Confirm Before Closing

```csharp
private async void OnCollapsing(object sender, ExpandingAndCollapsingEventArgs e)
{
    bool confirmed = await DisplayAlert(
        "Confirm", 
        "Are you sure you want to close this section?", 
        "Yes", 
        "No");
    
    if (!confirmed)
    {
        e.Cancel = true;
    }
}
```

---

## Collapsed Event

**When fired:** After the expander finishes collapsing (animation complete).

**Cancellable:** ❌ No

### XAML

```xml
<syncfusion:SfExpander Collapsed="OnCollapsed">
    <!-- Header and Content -->
</syncfusion:SfExpander>
```

### C# - Basic Handler

```csharp
private void OnCollapsed(object sender, ExpandedAndCollapsedEventArgs e)
{
    // Code to run after collapse completes
    Debug.WriteLine("Expander is now fully collapsed");
}
```

### Use Case: Free Resources

```csharp
private void OnCollapsed(object sender, ExpandedAndCollapsedEventArgs e)
{
    // Release memory when content is hidden
    ClearCachedData();
}
```

### Use Case: Update UI

```csharp
private void OnCollapsed(object sender, ExpandedAndCollapsedEventArgs e)
{
    // Update summary text after collapse
    summaryLabel.Text = "Click to view details";
}
```

---

## Event Usage Patterns

### Pattern 1: All Events Combined

```xml
<syncfusion:SfExpander Expanding="OnExpanding"
                       Expanded="OnExpanded"
                       Collapsing="OnCollapsing"
                       Collapsed="OnCollapsed">
    <!-- Header and Content -->
</syncfusion:SfExpander>
```

```csharp
private void OnExpanding(object sender, ExpandingAndCollapsingEventArgs e)
{
    Debug.WriteLine("1. About to expand");
}

private void OnExpanded(object sender, ExpandedAndCollapsedEventArgs e)
{
    Debug.WriteLine("2. Expansion complete");
}

private void OnCollapsing(object sender, ExpandingAndCollapsingEventArgs e)
{
    Debug.WriteLine("3. About to collapse");
}

private void OnCollapsed(object sender, ExpandedAndCollapsedEventArgs e)
{
    Debug.WriteLine("4. Collapse complete");
}
```

### Pattern 2: Conditional Expansion Based on Data

```csharp
private bool hasData = true;

private void OnExpanding(object sender, ExpandingAndCollapsingEventArgs e)
{
    if (!hasData)
    {
        e.Cancel = true;
        DisplayAlert("No Data", "No details available", "OK");
    }
}

private void OnExpanded(object sender, ExpandedAndCollapsedEventArgs e)
{
    // Load data after expanding
    LoadData();
}
```

### Pattern 3: Accordion Behavior (One Expanded at a Time)

```csharp
private SfExpander currentlyExpanded = null;

private void OnExpanding(object sender, ExpandingAndCollapsingEventArgs e)
{
    var expander = sender as SfExpander;
    
    // Collapse previously expanded expander
    if (currentlyExpanded != null && currentlyExpanded != expander)
    {
        currentlyExpanded.IsExpanded = false;
    }
    
    currentlyExpanded = expander;
}
```

### Pattern 4: Loading Indicator During Expansion

```xml
<syncfusion:SfExpander Expanding="OnExpanding" Expanded="OnExpanded">
    <syncfusion:SfExpander.Header>
        <Grid><Label Text="Click to load data"/></Grid>
    </syncfusion:SfExpander.Header>
    <syncfusion:SfExpander.Content>
        <Grid Padding="15">
            <ActivityIndicator x:Name="loadingIndicator" 
                             IsVisible="False" 
                             IsRunning="False"/>
            <StackLayout x:Name="contentStack" IsVisible="False">
                <!-- Actual content here -->
            </StackLayout>
        </Grid>
    </syncfusion:SfExpander.Content>
</syncfusion:SfExpander>
```

```csharp
private async void OnExpanding(object sender, ExpandingAndCollapsingEventArgs e)
{
    loadingIndicator.IsVisible = true;
    loadingIndicator.IsRunning = true;
}

private async void OnExpanded(object sender, ExpandedAndCollapsedEventArgs e)
{
    // Simulate data loading
    await Task.Delay(1000);
    var data = await LoadDataAsync();
    
    // Hide loading, show content
    loadingIndicator.IsVisible = false;
    loadingIndicator.IsRunning = false;
    contentStack.IsVisible = true;
}
```

---

## Complete Examples

### Example 1: FAQ with Event Tracking

```xml
<syncfusion:SfExpander AnimationDuration="250"
                       AnimationEasing="SinOut"
                       Expanding="OnFAQExpanding"
                       Expanded="OnFAQExpanded">
    <syncfusion:SfExpander.Header>
        <Grid Padding="15">
            <Label Text="How do I reset my password?" FontSize="16"/>
        </Grid>
    </syncfusion:SfExpander.Header>
    <syncfusion:SfExpander.Content>
        <Grid Padding="15">
            <Label Text="Click 'Forgot Password' on the login page..." 
                   FontSize="14"/>
        </Grid>
    </syncfusion:SfExpander.Content>
</syncfusion:SfExpander>
```

```csharp
private void OnFAQExpanding(object sender, ExpandingAndCollapsingEventArgs e)
{
    Debug.WriteLine("User is viewing FAQ answer");
}

private void OnFAQExpanded(object sender, ExpandedAndCollapsedEventArgs e)
{
    // Track analytics
    Analytics.TrackEvent("FAQ_Viewed", new Dictionary<string, string>
    {
        { "Question", "Reset Password" }
    });
}
```

### Example 2: Settings Panel with Unsaved Changes Check

```xml
<syncfusion:SfExpander x:Name="settingsExpander"
                       AnimationDuration="300"
                       Collapsing="OnSettingsCollapsing">
    <syncfusion:SfExpander.Header>
        <Grid Padding="15">
            <Label Text="Advanced Settings" FontSize="16" FontAttributes="Bold"/>
        </Grid>
    </syncfusion:SfExpander.Header>
    <syncfusion:SfExpander.Content>
        <StackLayout Padding="15" Spacing="10">
            <Entry x:Name="serverUrlEntry" Placeholder="Server URL"/>
            <Entry x:Name="apiKeyEntry" Placeholder="API Key"/>
            <Button Text="Save" Clicked="OnSaveClicked"/>
        </StackLayout>
    </syncfusion:SfExpander.Content>
</syncfusion:SfExpander>
```

```csharp
private bool hasUnsavedChanges = false;

private void OnEntryTextChanged(object sender, TextChangedEventArgs e)
{
    hasUnsavedChanges = true;
}

private async void OnSettingsCollapsing(object sender, ExpandingAndCollapsingEventArgs e)
{
    if (hasUnsavedChanges)
    {
        bool saveChanges = await DisplayAlert(
            "Unsaved Changes",
            "You have unsaved changes. Save before closing?",
            "Save",
            "Discard");
        
        if (saveChanges)
        {
            SaveSettings();
            hasUnsavedChanges = false;
        }
        else
        {
            // User chose to discard
            hasUnsavedChanges = false;
        }
    }
}

private void OnSaveClicked(object sender, EventArgs e)
{
    SaveSettings();
    hasUnsavedChanges = false;
    settingsExpander.IsExpanded = false;
}

private void SaveSettings()
{
    // Save logic here
    Debug.WriteLine("Settings saved");
}
```

### Example 3: Programmatic Expand/Collapse with Animation Control

```xml
<StackLayout>
    <Grid ColumnDefinitions="*,*" Margin="10">
        <Button Text="Expand All" Clicked="OnExpandAllClicked"/>
        <Button Text="Collapse All" Clicked="OnCollapseAllClicked" Grid.Column="1"/>
    </Grid>
    
    <syncfusion:SfExpander x:Name="expander1" AnimationDuration="200">
        <syncfusion:SfExpander.Header>
            <Grid Padding="12"><Label Text="Section 1"/></Grid>
        </syncfusion:SfExpander.Header>
        <syncfusion:SfExpander.Content>
            <Grid Padding="15"><Label Text="Content 1"/></Grid>
        </syncfusion:SfExpander.Content>
    </syncfusion:SfExpander>
    
    <syncfusion:SfExpander x:Name="expander2" AnimationDuration="200">
        <syncfusion:SfExpander.Header>
            <Grid Padding="12"><Label Text="Section 2"/></Grid>
        </syncfusion:SfExpander.Header>
        <syncfusion:SfExpander.Content>
            <Grid Padding="15"><Label Text="Content 2"/></Grid>
        </syncfusion:SfExpander.Content>
    </syncfusion:SfExpander>
    
    <syncfusion:SfExpander x:Name="expander3" AnimationDuration="200">
        <syncfusion:SfExpander.Header>
            <Grid Padding="12"><Label Text="Section 3"/></Grid>
        </syncfusion:SfExpander.Header>
        <syncfusion:SfExpander.Content>
            <Grid Padding="15"><Label Text="Content 3"/></Grid>
        </syncfusion:SfExpander.Content>
    </syncfusion:SfExpander>
</StackLayout>
```

```csharp
private void OnExpandAllClicked(object sender, EventArgs e)
{
    expander1.IsExpanded = true;
    expander2.IsExpanded = true;
    expander3.IsExpanded = true;
}

private void OnCollapseAllClicked(object sender, EventArgs e)
{
    expander1.IsExpanded = false;
    expander2.IsExpanded = false;
    expander3.IsExpanded = false;
}
```

---

## Related Features

- **Getting Started:** See `getting-started.md` for basic setup
- **Header/Content:** See `header-content-customization.md` for layout options
- **Appearance:** See `appearance-styling.md` for styling options
