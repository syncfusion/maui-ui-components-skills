# Events and Interaction in .NET MAUI Tab View

## Table of Contents
- [Overview](#overview)
- [TabItemTapped Event](#tabitemtapped-event)
- [SelectionChanging Event](#selectionchanging-event)
- [SelectionChanged Event](#selectionchanged-event)
- [Programmatic Selection](#programmatic-selection)
- [Event Flow and Order](#event-flow-and-order)
- [Complete Example: Wizard Navigation](#complete-example-wizard-navigation)
- [Best Practices](#best-practices)
- [Common Issues](#common-issues)

---

Comprehensive guide for handling tab selection events, programmatic navigation, and implementing custom interaction logic in .NET MAUI Tab View.

## Overview

SfTabView provides event-driven interaction patterns for:
- Detecting when tabs are tapped
- Validating before allowing tab changes
- Tracking selection changes
- Implementing custom navigation logic
- Responding to user interactions

## TabItemTapped Event

Triggered whenever a tab header is tapped, regardless of whether it's already selected.

### Event Arguments

`TabItemTappedEventArgs` provides:
- `TabItem`: The SfTabItem that was tapped
- `Cancel`: Boolean to prevent the event from completing

### Basic Usage

**XAML:**
```xaml
<tabView:SfTabView x:Name="tabView" TabItemTapped="TabView_TabItemTapped" />
```

**C#:**
```csharp
// Event handler
private void TabView_TabItemTapped(object sender, TabItemTappedEventArgs e)
{
    // Access the tapped tab
    var tappedTab = e.TabItem;
    Console.WriteLine($"Tab tapped: {tappedTab.Header}");
    
    // Modify tab properties
    e.TabItem.FontSize = 18;
    
    // Cancel the event if needed
    if (SomeCondition)
    {
        e.Cancel = true;
    }
}

// Or subscribe programmatically
tabView.TabItemTapped += TabView_TabItemTapped;
```

### Use Cases

#### Track User Interaction

```csharp
private void TabView_TabItemTapped(object sender, TabItemTappedEventArgs e)
{
    // Analytics tracking
    AnalyticsService.TrackEvent("TabTapped", new Dictionary<string, string>
    {
        { "TabName", e.TabItem.Header?.ToString() },
        { "Timestamp", DateTime.Now.ToString() }
    });
}
```

#### Show Confirmation on Tab Tap

```csharp
private async void TabView_TabItemTapped(object sender, TabItemTappedEventArgs e)
{
    if (e.TabItem.Header.ToString() == "Delete")
    {
        bool confirm = await DisplayAlert(
            "Confirm", 
            "Are you sure you want to delete?", 
            "Yes", 
            "No"
        );
        
        if (!confirm)
        {
            e.Cancel = true;  // Cancel tab selection
        }
    }
}
```

#### Dynamic Tab Styling on Tap

```csharp
private void TabView_TabItemTapped(object sender, TabItemTappedEventArgs e)
{
    // Temporarily highlight tapped tab
    var originalColor = e.TabItem.TextColor;
    e.TabItem.TextColor = Colors.Red;
    
    Device.StartTimer(TimeSpan.FromMilliseconds(200), () =>
    {
        e.TabItem.TextColor = originalColor;
        return false;  // Stop timer
    });
}
```

## SelectionChanging Event

Triggered **before** the selection changes, allowing you to validate and cancel the navigation.

### Event Arguments

`SelectionChangingEventArgs` provides:
- `Index`: The index of the tab that is about to be selected
- `Cancel`: Boolean to prevent the selection change

### Basic Usage

**XAML:**
```xaml
<tabView:SfTabView x:Name="tabView" SelectionChanging="TabView_SelectionChanging" />
```

**C#:**
```csharp
private void TabView_SelectionChanging(object sender, SelectionChangingEventArgs e)
{
    // e.Index is the new tab index about to be selected
    Console.WriteLine($"Navigating to tab index: {e.Index}");
    
    // Cancel navigation if needed
    if (e.Index == 2 && !isUserAuthenticated)
    {
        e.Cancel = true;
    }
}

// Programmatic subscription
tabView.SelectionChanging += TabView_SelectionChanging;
```

### Use Cases

#### Authentication Guard

```csharp
private bool isUserAuthenticated = false;

private async void TabView_SelectionChanging(object sender, SelectionChangingEventArgs e)
{
    // Protect premium tabs
    if (e.Index >= 3 && !isUserAuthenticated)
    {
        e.Cancel = true;
        
        bool login = await DisplayAlert(
            "Authentication Required",
            "Please login to access this section",
            "Login",
            "Cancel"
        );
        
        if (login)
        {
            // Navigate to login page
            await Navigation.PushAsync(new LoginPage());
        }
    }
}
```

#### Unsaved Changes Warning

```csharp
private bool hasUnsavedChanges = false;

private async void TabView_SelectionChanging(object sender, SelectionChangingEventArgs e)
{
    if (hasUnsavedChanges)
    {
        bool proceed = await DisplayAlert(
            "Unsaved Changes",
            "You have unsaved changes. Do you want to leave?",
            "Leave",
            "Stay"
        );
        
        if (!proceed)
        {
            e.Cancel = true;  // Stay on current tab
        }
        else
        {
            hasUnsavedChanges = false;  // Reset flag
        }
    }
}
```

#### Conditional Access Based on State

```csharp
private bool dataLoaded = false;

private void TabView_SelectionChanging(object sender, SelectionChangingEventArgs e)
{
    // Prevent navigating to data tab if data isn't loaded
    if (e.Index == 1 && !dataLoaded)
    {
        e.Cancel = true;
        DisplayAlert("Not Ready", "Data is still loading...", "OK");
        
        // Load data asynchronously
        LoadDataAsync();
    }
}
```

#### Sequential Tab Navigation

```csharp
private int maxAccessibleTab = 0;

private void TabView_SelectionChanging(object sender, SelectionChangingEventArgs e)
{
    // Only allow forward navigation to next tab
    if (e.Index > maxAccessibleTab + 1)
    {
        e.Cancel = true;
        DisplayAlert(
            "Complete Previous Steps",
            "Please complete the current step before proceeding",
            "OK"
        );
    }
    else if (e.Index > maxAccessibleTab)
    {
        maxAccessibleTab = e.Index;  // Update progress
    }
}
```

## SelectionChanged Event

Triggered **after** the selection has changed successfully.

### Event Arguments

`SelectionChangedEventArgs` provides:
- `NewIndex`: Index of the newly selected tab
- `OldIndex`: Index of the previously selected tab (if any)

### Basic Usage

**XAML:**
```xaml
<tabView:SfTabView x:Name="tabView" SelectionChanged="TabView_SelectionChanged" />
```

**C#:**
```csharp
private void TabView_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    Console.WriteLine($"Selection changed from {e.OldIndex} to {e.NewIndex}");
    
    // Access the newly selected tab
    var newTab = tabView.Items[e.NewIndex];
    Console.WriteLine($"Now viewing: {newTab.Header}");
}

// Programmatic subscription
tabView.SelectionChanged += TabView_SelectionChanged;
```

### Use Cases

#### Update UI Based on Selection

```csharp
private void TabView_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    // Update page title
    var selectedTab = tabView.Items[e.NewIndex];
    Title = $"App - {selectedTab.Header}";
    
    // Update toolbar buttons
    UpdateToolbarForTab(e.NewIndex);
}
```

#### Load Content on Demand

```csharp
private void TabView_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    var selectedTab = tabView.Items[e.NewIndex];
    
    // Lazy load content
    if (selectedTab.Content == null || IsContentEmpty(selectedTab.Content))
    {
        LoadTabContentAsync(e.NewIndex);
    }
}

private async Task LoadTabContentAsync(int tabIndex)
{
    var tab = tabView.Items[tabIndex];
    
    // Show loading indicator
    tab.Content = new ActivityIndicator { IsRunning = true };
    
    // Load data
    var data = await FetchDataForTab(tabIndex);
    
    // Update content
    tab.Content = CreateContentView(data);
}
```

#### Track Navigation History

```csharp
private Stack<int> navigationHistory = new Stack<int>();

private void TabView_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    // Track tab history for back navigation
    navigationHistory.Push(e.OldIndex);
    
    Console.WriteLine($"Navigation history: {string.Join(" -> ", navigationHistory.Reverse())}");
}

private void GoBackToPreviousTab()
{
    if (navigationHistory.Count > 0)
    {
        int previousTab = navigationHistory.Pop();
        tabView.SelectedIndex = previousTab;
    }
}
```

#### Sync State Between Tabs

```csharp
private void TabView_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    // Save state from old tab
    if (e.OldIndex >= 0)
    {
        SaveTabState(e.OldIndex);
    }
    
    // Restore state for new tab
    RestoreTabState(e.NewIndex);
}
```

## Programmatic Selection

### SelectedIndex Property

Navigate to tabs programmatically:

```csharp
// Navigate to specific tab
tabView.SelectedIndex = 2;  // Third tab

// Navigate to first tab
tabView.SelectedIndex = 0;

// Navigate to last tab
tabView.SelectedIndex = tabView.Items.Count - 1;

// Navigate forward
tabView.SelectedIndex++;

// Navigate backward
if (tabView.SelectedIndex > 0)
{
    tabView.SelectedIndex--;
}
```

### Navigation Methods

```csharp
// Navigate to next tab
public void NavigateNext()
{
    if (tabView.SelectedIndex < tabView.Items.Count - 1)
    {
        tabView.SelectedIndex++;
    }
}

// Navigate to previous tab
public void NavigatePrevious()
{
    if (tabView.SelectedIndex > 0)
    {
        tabView.SelectedIndex--;
    }
}

// Navigate to tab by header name
public void NavigateToTab(string headerName)
{
    for (int i = 0; i < tabView.Items.Count; i++)
    {
        if (tabView.Items[i].Header?.ToString() == headerName)
        {
            tabView.SelectedIndex = i;
            break;
        }
    }
}
```

### With Animation

```csharp
// Smooth transition to tab
public async Task NavigateToTabSmoothly(int targetIndex)
{
    int currentIndex = (int)tabView.SelectedIndex;
    int step = targetIndex > currentIndex ? 1 : -1;
    
    while (currentIndex != targetIndex)
    {
        await Task.Delay(100);  // Delay between steps
        currentIndex += step;
        tabView.SelectedIndex = currentIndex;
    }
}
```

## Event Flow and Order

When a tab is selected, events fire in this order:

1. **TabItemTapped** (if user taps header)
2. **SelectionChanging** (can cancel here)
3. **SelectionChanged** (if not cancelled)

```csharp
private void SetupEventLogging()
{
    tabView.TabItemTapped += (s, e) =>
        Console.WriteLine("1. TabItemTapped");
    
    tabView.SelectionChanging += (s, e) =>
        Console.WriteLine("2. SelectionChanging");
    
    tabView.SelectionChanged += (s, e) =>
        Console.WriteLine("3. SelectionChanged");
}
```

## Complete Example: Wizard Navigation

```csharp
public partial class WizardPage : ContentPage
{
    private bool[] stepCompleted = new bool[4];
    
    public WizardPage()
    {
        InitializeComponent();
        
        // Setup wizard logic
        wizardTabView.SelectionChanging += WizardTabView_SelectionChanging;
        wizardTabView.SelectionChanged += WizardTabView_SelectionChanged;
        
        // Initially only first step accessible
        wizardTabView.Items[0].IsEnabled = true;
        for (int i = 1; i < wizardTabView.Items.Count; i++)
        {
            wizardTabView.Items[i].IsEnabled = false;
        }
    }
    
    private void WizardTabView_SelectionChanging(object sender, SelectionChangingEventArgs e)
    {
        int currentStep = (int)wizardTabView.SelectedIndex;
        
        // Moving forward - check if current step is complete
        if (e.Index > currentStep && !stepCompleted[currentStep])
        {
            e.Cancel = true;
            DisplayAlert(
                "Complete Current Step",
                "Please complete all required fields in the current step",
                "OK"
            );
        }
    }
    
    private void WizardTabView_SelectionChanged(object sender, SelectionChangedEventArgs e)
    {
        // Update progress indicator
        UpdateProgressBar(e.NewIndex);
        
        // Update navigation buttons
        previousButton.IsEnabled = e.NewIndex > 0;
        nextButton.Text = e.NewIndex == wizardTabView.Items.Count - 1 
            ? "Finish" 
            : "Next";
    }
    
    private void OnNextClicked(object sender, EventArgs e)
    {
        int currentIndex = (int)wizardTabView.SelectedIndex;
        
        // Validate current step
        if (ValidateStep(currentIndex))
        {
            stepCompleted[currentIndex] = true;
            
            // Enable next step
            if (currentIndex < wizardTabView.Items.Count - 1)
            {
                wizardTabView.Items[currentIndex + 1].IsEnabled = true;
                wizardTabView.SelectedIndex = currentIndex + 1;
            }
            else
            {
                // Wizard complete
                SubmitWizard();
            }
        }
    }
    
    private void OnPreviousClicked(object sender, EventArgs e)
    {
        if (wizardTabView.SelectedIndex > 0)
        {
            wizardTabView.SelectedIndex--;
        }
    }
    
    private bool ValidateStep(int stepIndex)
    {
        // Implement validation logic
        return true;
    }
    
    private void UpdateProgressBar(int currentStep)
    {
        double progress = (currentStep + 1.0) / wizardTabView.Items.Count;
        progressBar.Progress = progress;
        progressLabel.Text = $"Step {currentStep + 1} of {wizardTabView.Items.Count}";
    }
    
    private async void SubmitWizard()
    {
        // Submit wizard data
        await DisplayAlert("Complete", "Wizard completed successfully!", "OK");
    }
}
```

## Best Practices

1. **Use SelectionChanging for validation** - Cancel navigation if conditions aren't met
2. **Use SelectionChanged for updates** - Load content, update UI after successful navigation
3. **Avoid heavy operations in events** - Keep event handlers fast, use async for slow operations
4. **Unsubscribe when disposing** - Prevent memory leaks by removing event handlers
5. **Provide user feedback** - Show alerts or messages when cancelling navigation
6. **Track state properly** - Use OldIndex and NewIndex to manage state transitions
7. **Test edge cases** - First tab, last tab, rapid tab switching

## Common Issues

**Issue:** Events firing multiple times  
**Solution:** Check if you're subscribing multiple times, unsubscribe before re-subscribing

**Issue:** SelectionChanged not firing  
**Solution:** Verify SelectionChanging isn't cancelling the navigation (e.Cancel = true)

**Issue:** Can't prevent tab selection  
**Solution:** Use SelectionChanging (not SelectionChanged) and set e.Cancel = true

**Issue:** Async operations causing issues  
**Solution:** Don't use async void in event handlers, use async Task and await properly

**Issue:** TabItemTapped fires even when tab is disabled  
**Solution:** Check e.TabItem.IsEnabled in handler and ignore if false
