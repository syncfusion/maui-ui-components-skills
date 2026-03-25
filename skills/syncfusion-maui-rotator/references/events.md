# Events and Migration Guide

## Table of Contents
- [Events](#events)
  - [SelectedIndexChanged Event](#selectedindexchanged-event)
  - [Event Patterns](#event-patterns)
  - [Common Scenarios](#common-scenarios)
  - [Namespace Changes](#namespace-changes)
  - [Property Changes](#property-changes)
  - [Handler Registration](#handler-registration)
  - [Migration Steps](#migration-steps)

## Events

### SelectedIndexChanged Event

Notifies when the selected item changes, either through user interaction (swiping, clicking navigation) or programmatic changes.

**Event:** `SelectedIndexChanged`  
**EventArgs:** `SelectedIndexChangedEventArgs`  
**Properties:**
- `NewIndex` (int): The newly selected index
- `OldIndex` (int): The previously selected index

### Basic Event Handling

**XAML:**
```xml
<syncfusion:SfRotator x:Name="rotator"
                      ItemsSource="{Binding ImageCollection}"
                      SelectedIndexChanged="OnRotatorIndexChanged"
                      HeightRequest="400">
    <syncfusion:SfRotator.ItemTemplate>
        <DataTemplate>
            <Image Source="{Binding Image}"/>
        </DataTemplate>
    </syncfusion:SfRotator.ItemTemplate>
</syncfusion:SfRotator>
```

**C# Code-Behind:**
```csharp
using Syncfusion.Maui.Core.Rotator;

private void OnRotatorIndexChanged(object sender, SelectedIndexChangedEventArgs e)
{
    var oldIndex = e.OldIndex;
    var newIndex = e.NewIndex;
    
    Console.WriteLine($"Index changed from {oldIndex} to {newIndex}");
}
```

### Complete Event Example

**XAML:**
```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:syncfusion="clr-namespace:Syncfusion.Maui.Rotator;assembly=Syncfusion.Maui.Rotator"
             xmlns:local="clr-namespace:YourApp"
             x:Class="YourApp.MainPage">
    <ContentPage.BindingContext>
        <local:RotatorViewModel/>
    </ContentPage.BindingContext>
    
    <ContentPage.Content>
        <Grid RowDefinitions="Auto, *, Auto">
            <!-- Status Label -->
            <Label x:Name="statusLabel"
                   Text="Current Index: 0"
                   FontSize="18"
                   Padding="10"
                   Grid.Row="0"/>
            
            <!-- Rotator -->
            <syncfusion:SfRotator x:Name="rotator"
                                  SelectedIndexChanged="OnRotatorIndexChanged"
                                  ItemsSource="{Binding ImageCollection}"
                                  NavigationStripMode="Dots"
                                  VerticalOptions="Start"
                                  Grid.Row="1">
                <syncfusion:SfRotator.ItemTemplate>
                    <DataTemplate>
                        <Image Source="{Binding Image}"/>
                    </DataTemplate>
                </syncfusion:SfRotator.ItemTemplate>
            </syncfusion:SfRotator>
            
            <!-- Item Counter -->
            <Label x:Name="counterLabel"
                   Text="Item 1 of 5"
                   HorizontalOptions="Center"
                   FontSize="16"
                   Padding="10"
                   Grid.Row="2"/>
        </Grid>
    </ContentPage.Content>
</ContentPage>
```

**C# Code-Behind:**
```csharp
using Syncfusion.Maui.Core.Rotator;

namespace YourApp
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
        }
        
        private void OnRotatorIndexChanged(object sender, SelectedIndexChangedEventArgs e)
        {
            // Update status label
            statusLabel.Text = $"Current Index: {e.NewIndex}";
            
            // Update counter
            var rotator = sender as SfRotator;
            var totalItems = rotator?.ItemsSource?.Cast<object>().Count() ?? 0;
            counterLabel.Text = $"Item {e.NewIndex + 1} of {totalItems}";
            
            // Log the change
            Console.WriteLine($"Rotator index changed from {e.OldIndex} to {e.NewIndex}");
        }
    }
}
```

### Subscribing to Events in C#

```csharp
using Syncfusion.Maui.Rotator;

public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        
        SfRotator rotator = new SfRotator();
        rotator.ItemsSource = GetImageCollection();
        
        // Subscribe to event
        rotator.SelectedIndexChanged += Rotator_SelectedIndexChanged;
        
        this.Content = rotator;
    }
    
    private void Rotator_SelectedIndexChanged(object sender, SelectedIndexChangedEventArgs e)
    {
        // Handle event
        DisplayAlert("Index Changed", 
                    $"New index: {e.NewIndex}", 
                    "OK");
    }
}
```

### Unsubscribing from Events

```csharp
// Unsubscribe when no longer needed
rotator.SelectedIndexChanged -= Rotator_SelectedIndexChanged;
```

## Event Patterns

### Pattern 1: Update Related UI

```csharp
private void OnRotatorIndexChanged(object sender, SelectedIndexChangedEventArgs e)
{
    var rotator = sender as SfRotator;
    var collection = rotator.ItemsSource as List<ProductModel>;
    
    if (collection != null && e.NewIndex < collection.Count)
    {
        var selectedProduct = collection[e.NewIndex];
        
        // Update other UI elements
        productNameLabel.Text = selectedProduct.Name;
        productPriceLabel.Text = $"${selectedProduct.Price:F2}";
        productDescLabel.Text = selectedProduct.Description;
    }
}
```

### Pattern 2: Analytics Tracking

```csharp
private void OnRotatorIndexChanged(object sender, SelectedIndexChangedEventArgs e)
{
    // Track user navigation
    AnalyticsService.TrackEvent("RotatorItemViewed", new Dictionary<string, string>
    {
        { "Index", e.NewIndex.ToString() },
        { "PreviousIndex", e.OldIndex.ToString() },
        { "Timestamp", DateTime.Now.ToString() }
    });
}
```

### Pattern 3: Preload Adjacent Content

```csharp
private void OnRotatorIndexChanged(object sender, SelectedIndexChangedEventArgs e)
{
    var rotator = sender as SfRotator;
    var collection = rotator.ItemsSource as List<ImageModel>;
    
    if (collection != null)
    {
        // Preload next image
        int nextIndex = e.NewIndex + 1;
        if (nextIndex < collection.Count)
        {
            PreloadImage(collection[nextIndex].ImageUrl);
        }
    }
}

private async void PreloadImage(string url)
{
    // Preload logic
    await ImageCacheService.CacheImageAsync(url);
}
```

### Pattern 4: Conditional Navigation

```csharp
private void OnRotatorIndexChanged(object sender, SelectedIndexChangedEventArgs e)
{
    // Prevent navigation to certain items
    if (e.NewIndex == 3 && !userHasPremium)
    {
        // Revert to previous index
        var rotator = sender as SfRotator;
        rotator.SelectedIndex = e.OldIndex;
        
        DisplayAlert("Premium Content", 
                    "This content requires premium subscription", 
                    "OK");
    }
}
```

### Pattern 5: Save State

```csharp
private void OnRotatorIndexChanged(object sender, SelectedIndexChangedEventArgs e)
{
    // Save current position for restoration later
    Preferences.Set("LastRotatorIndex", e.NewIndex);
}

// Restore on page load
protected override void OnAppearing()
{
    base.OnAppearing();
    
    int savedIndex = Preferences.Get("LastRotatorIndex", 0);
    rotator.SelectedIndex = savedIndex;
}
```

## Common Scenarios

### Scenario 1: Synchronized Components

Synchronize rotator with other UI components.

```xml
<Grid RowDefinitions="*, Auto">
    <!-- Rotator -->
    <syncfusion:SfRotator x:Name="rotator"
                          SelectedIndexChanged="OnRotatorIndexChanged"
                          ItemsSource="{Binding Images}"
                          Grid.Row="0">
        <syncfusion:SfRotator.ItemTemplate>
            <DataTemplate>
                <Image Source="{Binding Image}"/>
            </DataTemplate>
        </syncfusion:SfRotator.ItemTemplate>
    </syncfusion:SfRotator>
    
    <!-- Title Display -->
    <Label x:Name="titleLabel"
           Text=""
           FontSize="20"
           FontAttributes="Bold"
           HorizontalOptions="Center"
           Padding="10"
           Grid.Row="1"/>
</Grid>
```

```csharp
private void OnRotatorIndexChanged(object sender, SelectedIndexChangedEventArgs e)
{
    var images = (BindingContext as RotatorViewModel)?.Images;
    if (images != null && e.NewIndex < images.Count)
    {
        titleLabel.Text = images[e.NewIndex].Title;
    }
}
```

### Scenario 2: Form Navigation

Use rotator for multi-step forms.

```csharp
private int currentStep = 0;
private int totalSteps = 5;

private void OnRotatorIndexChanged(object sender, SelectedIndexChangedEventArgs e)
{
    currentStep = e.NewIndex;
    
    // Update progress
    progressBar.Progress = (double)(currentStep + 1) / totalSteps;
    
    // Update button visibility
    previousButton.IsVisible = currentStep > 0;
    nextButton.Text = currentStep == totalSteps - 1 ? "Submit" : "Next";
    
    // Validate current step before allowing navigation
    if (e.NewIndex > e.OldIndex && !ValidateStep(e.OldIndex))
    {
        rotator.SelectedIndex = e.OldIndex;
        DisplayAlert("Error", "Please complete all required fields", "OK");
    }
}

private bool ValidateStep(int stepIndex)
{
    // Validation logic
    return true;
}
```

### Scenario 3: Tutorial/Onboarding

Track progress through tutorial steps.

```csharp
private bool[] stepsCompleted;

private void OnRotatorIndexChanged(object sender, SelectedIndexChangedEventArgs e)
{
    // Mark old step as completed
    if (e.OldIndex >= 0 && e.OldIndex < stepsCompleted.Length)
    {
        stepsCompleted[e.OldIndex] = true;
    }
    
    // Check if all steps completed
    if (stepsCompleted.All(s => s))
    {
        finishButton.IsVisible = true;
    }
    
    // Update skip button
    skipButton.Text = e.NewIndex == stepsCompleted.Length - 1 ? "Finish" : "Skip";
}
```
## Best Practices

### Event Handling

1. **Always Check for Null:**
   ```csharp
   private void OnRotatorIndexChanged(object sender, SelectedIndexChangedEventArgs e)
   {
       var rotator = sender as SfRotator;
       if (rotator == null) return;
       
       // Safe to proceed
   }
   ```

2. **Unsubscribe When Appropriate:**
   ```csharp
   protected override void OnDisappearing()
   {
       base.OnDisappearing();
       rotator.SelectedIndexChanged -= OnRotatorIndexChanged;
   }
   ```

3. **Avoid Heavy Operations:**
   ```csharp
   private void OnRotatorIndexChanged(object sender, SelectedIndexChangedEventArgs e)
   {
       // Use async for heavy operations
       _ = UpdateDataAsync(e.NewIndex);
   }
   ```

### Migration

1. **Test Thoroughly:** Test on all target platforms after migration
2. **Update Dependencies:** Ensure all Syncfusion packages are compatible versions
3. **Review Breaking Changes:** Check Syncfusion release notes for any API changes
4. **Update Documentation:** Update any internal documentation with new namespaces

## Common Issues

### Issue: SelectedIndexChanged Not Firing

**Check:**
1. Event is properly subscribed (XAML or C#)
2. SelectedIndex is actually changing
3. ItemsSource is not null
4. Handler is registered (ConfigureSyncfusionCore)


### Issue: Events Fire Multiple Times

**Solution:**
- Check for duplicate event subscriptions
- Ensure you're not subscribing in a frequently called method
- Unsubscribe before resubscribing if needed
