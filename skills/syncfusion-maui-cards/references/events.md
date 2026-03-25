# Events and Interactions

## Table of Contents
- [Overview](#overview)
- [Tapped Event](#tapped-event)
- [VisibleIndexChanging Event](#visibleindexchanging-event)
- [VisibleIndexChanged Event](#visibleindexchanged-event)
- [Dismissing Event](#dismissing-event)
- [Dismissed Event](#dismissed-event)
- [Event Usage Patterns](#event-usage-patterns)
- [Best Practices](#best-practices)

## Overview

The .NET MAUI Cards control provides comprehensive event handling for user interactions. Events help you respond to user actions like tapping cards, swiping between cards, and dismissing cards.

**Available Events:**

**SfCardLayout Events:**
- `Tapped` - Fires when any card is tapped
- `VisibleIndexChanging` - Fires before visible card changes (cancelable)
- `VisibleIndexChanged` - Fires after visible card changes

**SfCardView Events (standalone only):**
- `Dismissing` - Fires before card dismissal (cancelable)
- `Dismissed` - Fires after card is dismissed

**Important:** `Dismissing` and `Dismissed` events only work for standalone `SfCardView`, not when it's a child of `SfCardLayout`.

## Tapped Event

The `Tapped` event is triggered when any card view in the card layout is tapped.

**Event Handler Signature:**
```csharp
void OnCardTapped(object sender, CardTappedEventArgs e)
```

**Event Arguments:**
- `CardView` - Gets the tapped `SfCardView` instance

### Basic Usage

**XAML:**
```xml
<cards:SfCardLayout Tapped="OnCardTapped" HeightRequest="400">
    <cards:SfCardView>
        <Label Text="Card 1" BackgroundColor="Cyan"/>
    </cards:SfCardView>
    
    <cards:SfCardView>
        <Label Text="Card 2" BackgroundColor="Yellow"/>
    </cards:SfCardView>
    
    <cards:SfCardView>
        <Label Text="Card 3" BackgroundColor="Orange"/>
    </cards:SfCardView>
</cards:SfCardLayout>
```

**Code-behind:**
```csharp
private void OnCardTapped(object sender, CardTappedEventArgs e)
{
    var tappedCard = e.CardView;
    DisplayAlert("Card Tapped", "You tapped a card!", "OK");
}
```

### Example: Navigate on Tap

```csharp
private void OnCardTapped(object sender, CardTappedEventArgs e)
{
    var cardView = e.CardView;
    
    // Get card content or data
    if (cardView.Content is Label label)
    {
        string cardText = label.Text;
        // Navigate to detail page
        Navigation.PushAsync(new CardDetailPage(cardText));
    }
}
```

### Example: Toggle Selection

```csharp
private SfCardView selectedCard = null;

private void OnCardTapped(object sender, CardTappedEventArgs e)
{
    var tappedCard = e.CardView;
    
    // Deselect previous
    if (selectedCard != null)
    {
        selectedCard.BorderWidth = 0;
    }
    
    // Select tapped card
    selectedCard = tappedCard;
    selectedCard.BorderWidth = 3;
    selectedCard.BorderColor = Colors.Blue;
}
```

### Example: Expand Card Details

```csharp
private Dictionary<SfCardView, bool> expandedStates = new();

private void OnCardTapped(object sender, CardTappedEventArgs e)
{
    var card = e.CardView;
    
    if (!expandedStates.ContainsKey(card))
        expandedStates[card] = false;
    
    // Toggle expanded state
    expandedStates[card] = !expandedStates[card];
    
    // Animate height change
    if (expandedStates[card])
    {
        card.HeightRequest = 400; // Expanded
    }
    else
    {
        card.HeightRequest = 200; // Collapsed
    }
}
```

## VisibleIndexChanging Event

The `VisibleIndexChanging` event fires before the visible card index changes. This event can be canceled to prevent the card change.

**Event Handler Signature:**
```csharp
void OnVisibleIndexChanging(object sender, CardVisibleIndexChangingEventArgs e)
```

**Event Arguments:**
- `OldIndex` - Index of the current card
- `NewIndex` - Index of the card that will become visible
- `Cancel` - Set to `true` to cancel the index change

### Basic Usage

**XAML:**
```xml
<cards:SfCardLayout VisibleIndexChanging="OnVisibleIndexChanging" HeightRequest="400">
    <cards:SfCardView>
        <Label Text="Card 0" BackgroundColor="Cyan"/>
    </cards:SfCardView>
    
    <cards:SfCardView>
        <Label Text="Card 1" BackgroundColor="Yellow"/>
    </cards:SfCardView>
    
    <cards:SfCardView>
        <Label Text="Card 2" BackgroundColor="Orange"/>
    </cards:SfCardView>
</cards:SfCardLayout>
```

**Code-behind:**
```csharp
private void OnVisibleIndexChanging(object sender, CardVisibleIndexChangingEventArgs e)
{
    Console.WriteLine($"Changing from card {e.OldIndex} to card {e.NewIndex}");
}
```

### Example: Prevent Navigation to Specific Card

```csharp
private void OnVisibleIndexChanging(object sender, CardVisibleIndexChangingEventArgs e)
{
    // Prevent navigating to card at index 2
    if (e.NewIndex == 2)
    {
        e.Cancel = true;
        DisplayAlert("Restricted", "This card is locked", "OK");
    }
}
```

### Example: Confirmation Dialog

```csharp
private async void OnVisibleIndexChanging(object sender, CardVisibleIndexChangingEventArgs e)
{
    // Ask for confirmation before moving to next card
    bool answer = await DisplayAlert(
        "Confirm", 
        "Are you sure you want to move to the next card?", 
        "Yes", 
        "No"
    );
    
    if (!answer)
    {
        e.Cancel = true;
    }
}
```

### Example: Validate Before Proceeding

```csharp
private void OnVisibleIndexChanging(object sender, CardVisibleIndexChangingEventArgs e)
{
    // Validate current card before allowing navigation
    if (!IsCurrentCardValid(e.OldIndex))
    {
        e.Cancel = true;
        DisplayAlert("Validation Error", "Please complete all fields", "OK");
    }
}

private bool IsCurrentCardValid(int cardIndex)
{
    // Implement validation logic
    return true;
}
```

### Example: Track Swipe Direction

```csharp
private void OnVisibleIndexChanging(object sender, CardVisibleIndexChangingEventArgs e)
{
    if (e.NewIndex > e.OldIndex)
    {
        Console.WriteLine("Swiping left (forward)");
    }
    else
    {
        Console.WriteLine("Swiping right (backward)");
    }
}
```

## VisibleIndexChanged Event

The `VisibleIndexChanged` event fires after the visible card index has changed.

**Event Handler Signature:**
```csharp
void OnVisibleIndexChanged(object sender, CardVisibleIndexChangedEventArgs e)
```

**Event Arguments:**
- `OldIndex` - Index of the previous card
- `NewIndex` - Index of the current visible card

### Basic Usage

**XAML:**
```xml
<cards:SfCardLayout VisibleIndexChanged="OnVisibleIndexChanged" HeightRequest="400">
    <!-- Cards -->
</cards:SfCardLayout>
```

**Code-behind:**
```csharp
private void OnVisibleIndexChanged(object sender, CardVisibleIndexChangedEventArgs e)
{
    Console.WriteLine($"Changed from card {e.OldIndex} to card {e.NewIndex}");
}
```

### Example: Update UI Indicators

```csharp
private Label pageIndicator;

private void OnVisibleIndexChanged(object sender, CardVisibleIndexChangedEventArgs e)
{
    var cardLayout = sender as SfCardLayout;
    int totalCards = cardLayout.Children.Count;
    
    // Update page indicator
    pageIndicator.Text = $"{e.NewIndex + 1} / {totalCards}";
}
```

### Example: Track Analytics

```csharp
private void OnVisibleIndexChanged(object sender, CardVisibleIndexChangedEventArgs e)
{
    // Log card views for analytics
    LogCardView(e.NewIndex, DateTime.Now);
    
    // Track user engagement
    TrackSwipeDirection(e.OldIndex, e.NewIndex);
}

private void LogCardView(int cardIndex, DateTime timestamp)
{
    Console.WriteLine($"Card {cardIndex} viewed at {timestamp}");
    // Send to analytics service
}

private void TrackSwipeDirection(int oldIndex, int newIndex)
{
    var direction = newIndex > oldIndex ? "forward" : "backward";
    Console.WriteLine($"User swiped {direction}");
}
```

### Example: Preload Next Card Data

```csharp
private void OnVisibleIndexChanged(object sender, CardVisibleIndexChangedEventArgs e)
{
    var cardLayout = sender as SfCardLayout;
    int nextIndex = e.NewIndex + 1;
    
    // Preload data for next card
    if (nextIndex < cardLayout.Children.Count)
    {
        PreloadCardData(nextIndex);
    }
}

private async void PreloadCardData(int index)
{
    // Load images, data, etc. for better performance
    Console.WriteLine($"Preloading data for card {index}");
}
```

### Example: Dating App Like/Pass

```csharp
private List<Profile> profiles;
private List<Profile> likedProfiles = new();
private List<Profile> passedProfiles = new();

private void OnVisibleIndexChanged(object sender, CardVisibleIndexChangedEventArgs e)
{
    // Determine action based on swipe direction
    if (e.NewIndex > e.OldIndex)
    {
        // Swiped left - Pass
        passedProfiles.Add(profiles[e.OldIndex]);
        Console.WriteLine($"Passed on profile {e.OldIndex}");
    }
    else
    {
        // Swiped right - Like
        likedProfiles.Add(profiles[e.OldIndex]);
        Console.WriteLine($"Liked profile {e.OldIndex}");
    }
    
    // Check if all cards viewed
    if (e.NewIndex == profiles.Count - 1)
    {
        DisplayAlert("Complete", $"Liked: {likedProfiles.Count}, Passed: {passedProfiles.Count}", "OK");
    }
}
```

## Dismissing Event

The `Dismissing` event fires when a card is about to be dismissed by swiping. This event can be canceled to prevent dismissal.

**Event Handler Signature:**
```csharp
void OnCardDismissing(object sender, CardDismissingEventArgs e)
```

**Event Arguments:**
- `DismissDirection` - Direction of the dismissal (Left or Right)
- `Cancel` - Set to `true` to prevent dismissal

**IMPORTANT:** This event only works for standalone `SfCardView` with `SwipeToDismiss="True"`, NOT when it's a child of `SfCardLayout`.

### Basic Usage

**XAML:**
```xml
<cards:SfCardView Dismissing="OnCardDismissing" 
                  SwipeToDismiss="True"
                  HeightRequest="200">
    <Label Text="Swipe to dismiss"/>
</cards:SfCardView>
```

**Code-behind:**
```csharp
private void OnCardDismissing(object sender, CardDismissingEventArgs e)
{
    Console.WriteLine($"Card dismissing in direction: {e.DismissDirection}");
}
```

### Example: Confirmation Before Dismiss

```csharp
private async void OnCardDismissing(object sender, CardDismissingEventArgs e)
{
    bool answer = await DisplayAlert(
        "Confirm", 
        "Are you sure you want to dismiss this card?", 
        "Yes", 
        "No"
    );
    
    if (!answer)
    {
        e.Cancel = true;
    }
}
```

### Example: Prevent Accidental Dismissal

```csharp
private void OnCardDismissing(object sender, CardDismissingEventArgs e)
{
    // Always cancel and require long press or button
    e.Cancel = true;
    DisplayAlert("Tip", "Use the delete button to remove this card", "OK");
}
```

### Example: Direction-Based Actions

```csharp
private void OnCardDismissing(object sender, CardDismissingEventArgs e)
{
    if (e.DismissDirection == SwipeDirection.Left)
    {
        // Allow left swipe for delete
        Console.WriteLine("Deleting card");
    }
    else if (e.DismissDirection == SwipeDirection.Right)
    {
        // Cancel right swipe for archive
        e.Cancel = true;
        ArchiveCard();
        DisplayAlert("Archived", "Card archived instead of deleted", "OK");
    }
}
```

## Dismissed Event

The `Dismissed` event fires after a card has been successfully dismissed.

**Event Handler Signature:**
```csharp
void OnCardDismissed(object sender, CardDismissedEventArgs e)
```

**Event Arguments:**
- `DismissDirection` - Direction of the dismissal (Left or Right)

**IMPORTANT:** This event only works for standalone `SfCardView` with `SwipeToDismiss="True"`, NOT when it's a child of `SfCardLayout`.

### Basic Usage

**XAML:**
```xml
<cards:SfCardView Dismissed="OnCardDismissed" 
                  SwipeToDismiss="True"
                  HeightRequest="200">
    <Label Text="Swipe to dismiss"/>
</cards:SfCardView>
```

**Code-behind:**
```csharp
private void OnCardDismissed(object sender, CardDismissedEventArgs e)
{
    Console.WriteLine($"Card dismissed in direction: {e.DismissDirection}");
}
```

### Example: Clean Up Resources

```csharp
private void OnCardDismissed(object sender, CardDismissedEventArgs e)
{
    var card = sender as SfCardView;
    
    // Clean up resources
    if (card.Content is Image image)
    {
        image.Source = null;
    }
    
    // Remove from parent
    if (card.Parent is Layout parent)
    {
        parent.Children.Remove(card);
    }
    
    Console.WriteLine($"Card dismissed and cleaned up");
}
```

### Example: Undo Functionality

```csharp
private Stack<SfCardView> dismissedCards = new();

private void OnCardDismissed(object sender, CardDismissedEventArgs e)
{
    var card = sender as SfCardView;
    
    // Store for undo
    dismissedCards.Push(card);
    
    // Show undo toast
    ShowUndoToast();
}

private async void ShowUndoToast()
{
    bool undo = await DisplayAlert("Dismissed", "Card dismissed", "Undo", "OK");
    
    if (undo && dismissedCards.Count > 0)
    {
        var card = dismissedCards.Pop();
        card.IsDismissed = false;
    }
}
```

### Example: Direction-Based Actions

```csharp
private void OnCardDismissed(object sender, CardDismissedEventArgs e)
{
    if (e.DismissDirection == SwipeDirection.Left)
    {
        // Left swipe - Delete
        DeleteCard(sender as SfCardView);
    }
    else if (e.DismissDirection == SwipeDirection.Right)
    {
        // Right swipe - Archive
        ArchiveCard(sender as SfCardView);
    }
}

private void DeleteCard(SfCardView card)
{
    Console.WriteLine("Card deleted permanently");
    // Remove from database
}

private void ArchiveCard(SfCardView card)
{
    Console.WriteLine("Card archived");
    // Move to archive
}
```

### Example: Update Counter

```csharp
private int dismissedCount = 0;
private Label counterLabel;

private void OnCardDismissed(object sender, CardDismissedEventArgs e)
{
    dismissedCount++;
    counterLabel.Text = $"Dismissed: {dismissedCount}";
}
```

## Event Usage Patterns

### Pattern 1: Complete Card Lifecycle

```csharp
public class CardWithLifecycle : SfCardView
{
    public CardWithLifecycle()
    {
        SwipeToDismiss = true;
        
        // Subscribe to events
        Dismissing += OnDismissing;
        Dismissed += OnDismissed;
    }
    
    private void OnDismissing(object sender, CardDismissingEventArgs e)
    {
        Console.WriteLine($"Dismissing: {e.DismissDirection}");
        
        // Validation
        if (!CanDismiss())
        {
            e.Cancel = true;
        }
    }
    
    private void OnDismissed(object sender, CardDismissedEventArgs e)
    {
        Console.WriteLine($"Dismissed: {e.DismissDirection}");
        
        // Cleanup
        Cleanup();
    }
    
    private bool CanDismiss()
    {
        // Implement validation
        return true;
    }
    
    private void Cleanup()
    {
        // Release resources
        Content = null;
    }
}
```

### Pattern 2: Card Layout with Full Event Handling

```csharp
public class InteractiveCardLayout : ContentView
{
    private SfCardLayout cardLayout;
    private Label statusLabel;
    
    public InteractiveCardLayout()
    {
        cardLayout = new SfCardLayout
        {
            SwipeDirection = CardSwipeDirection.Left,
            HeightRequest = 500
        };
        
        // Subscribe to all events
        cardLayout.Tapped += OnCardTapped;
        cardLayout.VisibleIndexChanging += OnIndexChanging;
        cardLayout.VisibleIndexChanged += OnIndexChanged;
        
        statusLabel = new Label { HorizontalOptions = LayoutOptions.Center };
        
        Content = new VerticalStackLayout
        {
            Children = { cardLayout, statusLabel }
        };
    }
    
    private void OnCardTapped(object sender, CardTappedEventArgs e)
    {
        statusLabel.Text = "Card tapped!";
    }
    
    private void OnIndexChanging(object sender, CardVisibleIndexChangingEventArgs e)
    {
        statusLabel.Text = $"Changing: {e.OldIndex} → {e.NewIndex}";
    }
    
    private void OnIndexChanged(object sender, CardVisibleIndexChangedEventArgs e)
    {
        statusLabel.Text = $"Now showing card {e.NewIndex}";
    }
}
```

## Best Practices

### 1. Unsubscribe from Events

```csharp
protected override void OnDisappearing()
{
    base.OnDisappearing();
    
    // Unsubscribe to prevent memory leaks
    cardLayout.Tapped -= OnCardTapped;
    cardLayout.VisibleIndexChanged -= OnVisibleIndexChanged;
}
```

### 2. Handle Async Operations Safely

```csharp
private async void OnCardTapped(object sender, CardTappedEventArgs e)
{
    try
    {
        await NavigateToDetailPage();
    }
    catch (Exception ex)
    {
        await DisplayAlert("Error", ex.Message, "OK");
    }
}
```

### 3. Use Cancel Wisely

Don't overuse `Cancel` - it can frustrate users:

```csharp
// Good: Validate critical operations
if (!IsFormValid())
    e.Cancel = true;

// Bad: Cancel too often
if (random.Next(0, 2) == 0)  // Don't do this!
    e.Cancel = true;
```

### 4. Provide User Feedback

Always inform users when you cancel their action:

```csharp
if (!CanProceed())
{
    e.Cancel = true;
    await DisplayAlert("Cannot Proceed", "Please complete all fields", "OK");
}
```

### 5. Track for Analytics

```csharp
private void OnVisibleIndexChanged(object sender, CardVisibleIndexChangedEventArgs e)
{
    // Track user behavior
    Analytics.TrackEvent("CardSwipe", new Dictionary<string, string>
    {
        { "OldIndex", e.OldIndex.ToString() },
        { "NewIndex", e.NewIndex.ToString() },
        { "Direction", e.NewIndex > e.OldIndex ? "forward" : "backward" }
    });
}
```
