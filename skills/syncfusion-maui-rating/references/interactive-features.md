# Interactive Features in .NET MAUI Rating

The `SfRating` control provides interactive features for user engagement and control display modes. This guide covers the `IsReadOnly` property for restricting interactions and the `ValueChanged` event for responding to rating changes.

## Table of Contents
- [IsReadOnly Property](#isreadonly-property)
- [ValueChanged Event](#valuechanged-event)
- [User Interaction Control](#user-interaction-control)
- [Event Handling Scenarios](#event-handling-scenarios)
- [Data Binding with Events](#data-binding-with-events)
- [Best Practices](#best-practices)

## IsReadOnly Property

The `IsReadOnly` property determines whether users can interact with the rating control to change its value.

> **Default:** `false` (interactive)

### When IsReadOnly is False (Default)

Users can click/tap rating items to change the value.

**XAML:**
```xml
<rating:SfRating x:Name="rating" 
                 Value="3"
                 IsReadOnly="False" />
```

**C#:**
```csharp
SfRating rating = new SfRating();
rating.Value = 3;
rating.IsReadOnly = false; // Interactive, users can change rating
```

**Behavior:**
- Users can tap/click to select rating
- Value changes based on user interaction
- ValueChanged event fires on interaction
- Visual feedback on hover/press

### When IsReadOnly is True

The rating control becomes display-only; users cannot change the value.

**XAML:**
```xml
<rating:SfRating x:Name="rating" 
                 Value="4.5"
                 IsReadOnly="True" />
```

**C#:**
```csharp
SfRating rating = new SfRating();
rating.Value = 4.5;
rating.IsReadOnly = true; // Display-only, users cannot change rating
```

**Behavior:**
- No user interaction allowed
- Value cannot be changed by clicking/tapping
- ValueChanged event does not fire from user interaction
- No hover/press visual feedback

### Use Cases for IsReadOnly

**IsReadOnly = True (Display-Only):**
- Showing existing product ratings (e.g., "Rated 4.5 by 892 users")
- Displaying average ratings in lists
- Read-only review displays
- Historical rating data
- Summary dashboards
- Rating comparisons

**IsReadOnly = False (Interactive):**
- Collecting new user ratings
- Allowing rating changes/updates
- Interactive feedback forms
- User preference selections
- Dynamic rating inputs

### IsReadOnly Examples

**Example 1: Display Average Rating (Read-Only)**

```xml
<HorizontalStackLayout Spacing="10">
    <rating:SfRating Value="4.73"
                     ItemCount="5"
                     Precision="Exact"
                     IsReadOnly="True"
                     ItemSize="30" />
    
    <Label Text="4.73 (892 reviews)" 
           VerticalOptions="Center"
           FontSize="14"
           TextColor="Gray" />
</HorizontalStackLayout>
```

**Example 2: User Rating Input (Interactive)**

```xml
<VerticalStackLayout Spacing="10">
    <Label Text="Rate this product:" 
           FontSize="16" 
           FontAttributes="Bold" />
    
    <rating:SfRating x:Name="userRating"
                     Value="0"
                     ItemCount="5"
                     IsReadOnly="False"
                     ValueChanged="OnRatingChanged" />
    
    <Button Text="Submit Rating" 
            Clicked="OnSubmitClicked" />
</VerticalStackLayout>
```

**Example 3: Conditional Read-Only**

```csharp
public partial class RatingPage : ContentPage
{
    public RatingPage()
    {
        InitializeComponent();
        
        // Make read-only if user already rated
        bool userHasRated = CheckIfUserRated();
        productRating.IsReadOnly = userHasRated;
        
        if (userHasRated)
        {
            // Load their previous rating
            productRating.Value = GetUserRating();
        }
    }
}
```

## ValueChanged Event

The `ValueChanged` event is raised when the rating value changes through user interaction.

### Event Signature

```csharp
public event EventHandler<ValueChangedEventArgs> ValueChanged;
```

### ValueChangedEventArgs

The event args provide information about the rating change:

```csharp
public class ValueChangedEventArgs : EventArgs
{
    public double Value { get; } // The new rating value
}
```

**Properties:**
- **Value** (double): The new rating value after the change

### Basic Event Handling

**XAML:**
```xml
<rating:SfRating x:Name="rating" 
                 Value="0"
                 ValueChanged="OnRatingValueChanged" />
```

**C# Code-Behind:**
```csharp
private void OnRatingValueChanged(object sender, ValueChangedEventArgs e)
{
    double newValue = e.Value;
    DisplayAlert("Rating Changed", $"New rating: {newValue}", "OK");
}
```

### Programmatic Event Subscription

**C#:**
```csharp
SfRating rating = new SfRating();
rating.ValueChanged += OnRatingValueChanged;

// Or with lambda expression
rating.ValueChanged += (sender, e) => 
{
    Debug.WriteLine($"Rating changed to: {e.Value}");
};

// Or with anonymous method
rating.ValueChanged += (sender, e) =>
{
    UpdateAverageRating(e.Value);
    SaveRatingToDatabase(e.Value);
};
```

### Event Handling Examples

**Example 1: Display Current Value**

```xml
<VerticalStackLayout Spacing="15">
    <Label Text="Rate this item:" FontSize="18" />
    
    <rating:SfRating x:Name="rating"
                     Value="0"
                     ItemCount="5"
                     ValueChanged="OnRatingChanged" />
    
    <Label x:Name="valueLabel" 
           Text="No rating yet"
           FontSize="14"
           TextColor="Gray" />
</VerticalStackLayout>
```

```csharp
private void OnRatingChanged(object sender, ValueChangedEventArgs e)
{
    if (e.Value == 0)
    {
        valueLabel.Text = "No rating yet";
    }
    else
    {
        valueLabel.Text = $"Your rating: {e.Value:F1} stars";
    }
}
```

**Example 2: Validation and Confirmation**

```csharp
private async void OnRatingChanged(object sender, ValueChangedEventArgs e)
{
    bool confirm = await DisplayAlert(
        "Confirm Rating",
        $"Rate this product {e.Value} stars?",
        "Yes",
        "No"
    );
    
    if (confirm)
    {
        await SaveRatingAsync(e.Value);
        await DisplayAlert("Success", "Thank you for your rating!", "OK");
    }
    else
    {
        // Reset to previous value if user cancels
        ((SfRating)sender).Value = _previousValue;
    }
}
```

**Example 3: Real-Time Feedback**

```csharp
private void OnRatingChanged(object sender, ValueChangedEventArgs e)
{
    string feedback = e.Value switch
    {
        1 => "😞 Poor",
        2 => "😐 Fair",
        3 => "🙂 Good",
        4 => "😊 Very Good",
        5 => "🤩 Excellent",
        _ => "No rating"
    };
    
    feedbackLabel.Text = feedback;
    feedbackLabel.TextColor = GetColorForRating(e.Value);
}

private Color GetColorForRating(double rating)
{
    return rating switch
    {
        <= 2 => Colors.Red,
        <= 3 => Colors.Orange,
        <= 4 => Colors.YellowGreen,
        _ => Colors.Green
    };
}
```

## User Interaction Control

### Touch/Click Behavior

**Standard Precision:**
- Tap anywhere on star → Select that star's full value

**Half Precision:**
- Tap left half of star → Select X.5 value
- Tap right half of star → Select X.0 value

**Exact Precision:**
- Tap position determines exact decimal value
- Value calculated based on tap location within item

### Interaction States

**Normal (IsReadOnly = False):**
```xml
<rating:SfRating Value="3" />
```
- Interactive
- Responds to taps/clicks
- Shows visual feedback
- Fires ValueChanged event

**Read-Only (IsReadOnly = True):**
```xml
<rating:SfRating Value="3" IsReadOnly="True" />
```
- Non-interactive
- No response to taps/clicks
- No visual feedback on interaction
- Does not fire ValueChanged event

**Disabled (IsEnabled = False):**
```xml
<rating:SfRating Value="3" IsEnabled="False" />
```
- Completely disabled
- May appear grayed out
- No interaction possible

## Event Handling Scenarios

### Scenario 1: Save to Database

```csharp
private async void OnRatingChanged(object sender, ValueChangedEventArgs e)
{
    try
    {
        var rating = new UserRating
        {
            ProductId = _currentProductId,
            UserId = _currentUserId,
            RatingValue = e.Value,
            Timestamp = DateTime.UtcNow
        };
        
        await _databaseService.SaveRatingAsync(rating);
        await DisplayAlert("Success", "Rating saved!", "OK");
    }
    catch (Exception ex)
    {
        await DisplayAlert("Error", "Failed to save rating", "OK");
        Debug.WriteLine($"Save error: {ex.Message}");
    }
}
```

### Scenario 2: Calculate Average

```csharp
private void OnRatingChanged(object sender, ValueChangedEventArgs e)
{
    // Add new rating to collection
    _allRatings.Add(e.Value);
    
    // Calculate new average
    double average = _allRatings.Average();
    double count = _allRatings.Count;
    
    // Update display
    averageRating.Value = average;
    countLabel.Text = $"Based on {count} ratings";
}
```

### Scenario 3: Form Validation

```csharp
private bool _hasRated = false;

private void OnRatingChanged(object sender, ValueChangedEventArgs e)
{
    _hasRated = e.Value > 0;
    UpdateSubmitButtonState();
}

private void UpdateSubmitButtonState()
{
    // Enable submit button only if rating provided
    submitButton.IsEnabled = _hasRated && !string.IsNullOrEmpty(commentEntry.Text);
}
```

### Scenario 4: Analytics Tracking

```csharp
private void OnRatingChanged(object sender, ValueChangedEventArgs e)
{
    // Track rating event in analytics
    AnalyticsService.TrackEvent("Rating_Changed", new Dictionary<string, string>
    {
        { "ProductId", _currentProductId.ToString() },
        { "RatingValue", e.Value.ToString() },
        { "UserId", _currentUserId.ToString() },
        { "Timestamp", DateTime.UtcNow.ToString() }
    });
    
    // Show thank you message
    thankYouLabel.IsVisible = true;
}
```

### Scenario 5: Multi-Criteria Rating

```csharp
public partial class ReviewPage : ContentPage
{
    private double _qualityRating = 0;
    private double _serviceRating = 0;
    private double _valueRating = 0;
    
    private void OnQualityRatingChanged(object sender, ValueChangedEventArgs e)
    {
        _qualityRating = e.Value;
        UpdateOverallRating();
    }
    
    private void OnServiceRatingChanged(object sender, ValueChangedEventArgs e)
    {
        _serviceRating = e.Value;
        UpdateOverallRating();
    }
    
    private void OnValueRatingChanged(object sender, ValueChangedEventArgs e)
    {
        _valueRating = e.Value;
        UpdateOverallRating();
    }
    
    private void UpdateOverallRating()
    {
        double overall = (_qualityRating + _serviceRating + _valueRating) / 3.0;
        overallRating.Value = overall;
        overallLabel.Text = $"Overall: {overall:F2}";
    }
}
```

## Data Binding with Events

### MVVM Pattern with Commands

**ViewModel:**
```csharp
public class ProductViewModel : INotifyPropertyChanged
{
    private double _userRating;
    
    public double UserRating
    {
        get => _userRating;
        set
        {
            if (_userRating != value)
            {
                _userRating = value;
                OnPropertyChanged();
                OnRatingChanged(value);
            }
        }
    }
    
    private void OnRatingChanged(double newRating)
    {
        // Business logic here
        Debug.WriteLine($"Rating changed to: {newRating}");
        SaveRatingAsync(newRating).ConfigureAwait(false);
    }
    
    private async Task SaveRatingAsync(double rating)
    {
        // Save to database
        await Task.Delay(100); // Simulate network call
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

**XAML:**
```xml
<rating:SfRating Value="{Binding UserRating, Mode=TwoWay}" 
                 ItemCount="5" />
```

### Combining Events and Binding

```xml
<rating:SfRating Value="{Binding UserRating, Mode=TwoWay}"
                 ValueChanged="OnRatingChanged"
                 ItemCount="5" />
```

```csharp
// ViewModel handles business logic
public double UserRating { get; set; }

// Code-behind handles UI feedback
private void OnRatingChanged(object sender, ValueChangedEventArgs e)
{
    // Show immediate UI feedback
    ShowThankYouAnimation();
}
```

## Best Practices

### Event Handling

1. **Keep Handlers Fast**: Avoid long-running operations in event handlers
2. **Use Async Properly**: Mark handlers as `async` when performing async operations
3. **Error Handling**: Always wrap event logic in try-catch blocks
4. **Unsubscribe**: Remove event handlers when disposing to prevent memory leaks
5. **Validate Input**: Check e.Value for expected ranges

```csharp
// Good practice
private async void OnRatingChanged(object sender, ValueChangedEventArgs e)
{
    try
    {
        // Validate
        if (e.Value < 0 || e.Value > 5) return;
        
        // Quick UI update
        UpdateUI(e.Value);
        
        // Long operation on background thread
        await Task.Run(() => SaveRatingToDatabase(e.Value));
    }
    catch (Exception ex)
    {
        Debug.WriteLine($"Error: {ex.Message}");
        await ShowErrorMessage();
    }
}
```

### IsReadOnly Usage

1. **Clear Intent**: Make it obvious to users when ratings are read-only
2. **Visual Distinction**: Consider different styling for read-only ratings
3. **Context Matters**: Use IsReadOnly for display, not for permissions
4. **State Management**: Toggle IsReadOnly based on user state (e.g., already rated)

```csharp
// Example: One rating per user
private void LoadProductRating()
{
    var userRating = GetUserRatingForProduct();
    
    if (userRating != null)
    {
        // User already rated - show their rating as read-only
        productRating.Value = userRating.Value;
        productRating.IsReadOnly = true;
        ratingLabel.Text = "Your rating (cannot be changed)";
    }
    else
    {
        // User hasn't rated yet - allow rating
        productRating.Value = 0;
        productRating.IsReadOnly = false;
        ratingLabel.Text = "Rate this product";
    }
}
```

### User Experience

1. **Provide Feedback**: Show confirmation when ratings are saved
2. **Enable Undo**: Consider allowing users to change their rating
3. **Clear Instructions**: Label ratings clearly ("Your Rating" vs "Average Rating")
4. **Responsive UI**: Update UI immediately when value changes
5. **Accessibility**: Ensure ratings are accessible to screen readers

## Complete Example: Product Rating Form

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:rating="clr-namespace:Syncfusion.Maui.Inputs;assembly=Syncfusion.Maui.Inputs"
             x:Class="MyApp.ProductRatingPage">
    
    <ScrollView>
        <VerticalStackLayout Padding="20" Spacing="25">
            
            <!-- Product Info -->
            <Label Text="Rate This Product" 
                   FontSize="24" 
                   FontAttributes="Bold" />
            
            <!-- Average Rating (Read-Only) -->
            <StackLayout Spacing="8">
                <Label Text="Average Rating" FontSize="14" TextColor="Gray" />
                <HorizontalStackLayout Spacing="10">
                    <rating:SfRating x:Name="averageRating"
                                     Value="4.3"
                                     Precision="Exact"
                                     IsReadOnly="True"
                                     ItemSize="30" />
                    <Label Text="4.3 (245 reviews)" 
                           VerticalOptions="Center" 
                           TextColor="Gray" />
                </HorizontalStackLayout>
            </StackLayout>
            
            <!-- User Rating (Interactive) -->
            <StackLayout Spacing="8">
                <Label Text="Your Rating" FontSize="14" FontAttributes="Bold" />
                <rating:SfRating x:Name="userRating"
                                 Value="0"
                                 ItemCount="5"
                                 ItemSize="50"
                                 ItemSpacing="10"
                                 ValueChanged="OnUserRatingChanged" />
                <Label x:Name="feedbackLabel" 
                       Text="Tap to rate"
                       FontSize="16"
                       TextColor="Gray" />
            </StackLayout>
            
            <!-- Comment Section -->
            <Editor x:Name="commentEditor"
                    Placeholder="Add a comment (optional)"
                    HeightRequest="120" />
            
            <!-- Submit Button -->
            <Button x:Name="submitButton"
                    Text="Submit Rating"
                    IsEnabled="False"
                    Clicked="OnSubmitClicked" />
            
        </VerticalStackLayout>
    </ScrollView>
    
</ContentPage>
```

```csharp
public partial class ProductRatingPage : ContentPage
{
    private bool _hasRated = false;
    
    public ProductRatingPage()
    {
        InitializeComponent();
    }
    
    private void OnUserRatingChanged(object sender, ValueChangedEventArgs e)
    {
        _hasRated = e.Value > 0;
        
        // Update feedback
        feedbackLabel.Text = e.Value switch
        {
            0 => "Tap to rate",
            1 => "😞 Poor",
            2 => "😐 Fair",
            3 => "🙂 Good",
            4 => "😊 Very Good",
            5 => "🤩 Excellent!",
            _ => $"{e.Value:F1} stars"
        };
        
        // Enable submit button
        submitButton.IsEnabled = _hasRated;
    }
    
    private async void OnSubmitClicked(object sender, EventArgs e)
    {
        if (!_hasRated)
        {
            await DisplayAlert("Error", "Please provide a rating", "OK");
            return;
        }
        
        try
        {
            // Save rating
            await SaveRatingAsync(userRating.Value, commentEditor.Text);
            
            // Make read-only after submission
            userRating.IsReadOnly = true;
            commentEditor.IsEnabled = false;
            submitButton.IsEnabled = false;
            
            await DisplayAlert("Success", "Thank you for your rating!", "OK");
        }
        catch (Exception ex)
        {
            await DisplayAlert("Error", "Failed to submit rating", "OK");
            Debug.WriteLine($"Error: {ex.Message}");
        }
    }
    
    private async Task SaveRatingAsync(double rating, string comment)
    {
        // Implement database save logic
        await Task.Delay(500); // Simulate network call
    }
}
```

## Accessibility Considerations

1. **Semantic Labels**: Use meaningful labels for screen readers
2. **Keyboard Navigation**: Ensure ratings can be changed via keyboard
3. **Focus Indicators**: Provide clear visual focus states
4. **Announcements**: Announce value changes to screen readers

```xml
<rating:SfRating AutomationId="ProductRating"
                 SemanticProperties.Description="Product rating, 4 out of 5 stars"
                 ValueChanged="OnRatingChanged" />
```

## Next Steps

- **Getting Started**: Learn basic setup and implementation → [getting-started.md](getting-started.md)
- **Precision Modes**: Understand rating accuracy options → [precision-modes.md](precision-modes.md)
- **Rating Shapes**: Explore visual customization → [rating-shapes.md](rating-shapes.md)
- **Appearance Styling**: Customize colors and styling → [appearance-styling.md](appearance-styling.md)
