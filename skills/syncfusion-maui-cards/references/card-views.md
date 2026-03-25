# Card View Features

## Table of Contents
- [Overview](#overview)
- [SwipeToDismiss Property](#swipetodismiss-property)
- [IsDismissed Property](#isdismissed-property)
- [FadeOutOnSwiping Property](#fadeoutonswiping-property)
- [Common Scenarios](#common-scenarios)
- [Best Practices](#best-practices)
- [Limitations](#limitations)

## Overview

`SfCardView` represents a single card UI element that can display any content. It provides swipe-to-dismiss functionality, programmatic dismissal control, and visual effects. This component is ideal for notifications, alerts, removable items, or any dismissible content.

**Key Features:**
- Swipe-to-dismiss in left/right directions
- Programmatic control over dismissed state
- Fade effect during swiping
- Full content customization
- Event handling for dismissal

## SwipeToDismiss Property

The `SwipeToDismiss` property enables or disables the swiping feature, allowing users to dismiss the card by swiping left or right.

**Type:** `bool`  
**Default:** `false`

### Basic Usage

**XAML:**
```xml
<cards:SfCardView SwipeToDismiss="True">
    <Label Text="SfCardView" 
           Background="MediumPurple" 
           HorizontalTextAlignment="Center" 
           VerticalTextAlignment="Center"/>
</cards:SfCardView>
```

**C#:**
```csharp
SfCardView cardView = new SfCardView
{
    SwipeToDismiss = true,
    Content = new Label
    {
        Text = "SfCardView",
        HorizontalTextAlignment = TextAlignment.Center,
        VerticalTextAlignment = TextAlignment.Center,
        BackgroundColor = Colors.MediumPurple
    }
};
```

### How It Works

1. User swipes the card left or right
2. Card follows the finger/pointer
3. If swipe distance exceeds threshold, card dismisses
4. If swipe is released before threshold, card returns to position
5. `Dismissing` event fires (can be canceled)
6. Card animates out of view
7. `Dismissed` event fires

### Example: Dismissible Notification

```csharp
var notificationCard = new SfCardView
{
    SwipeToDismiss = true,
    Padding = 15,
    CornerRadius = 8,
    BackgroundColor = Colors.White,
    Content = new VerticalStackLayout
    {
        Children =
        {
            new Label 
            { 
                Text = "New Message", 
                FontSize = 18,
                FontAttributes = FontAttributes.Bold 
            },
            new Label 
            { 
                Text = "You have received a new message from John",
                FontSize = 14,
                TextColor = Colors.Gray
            }
        }
    }
};

// Handle dismissed event
notificationCard.Dismissed += (s, e) =>
{
    Console.WriteLine($"Notification dismissed in direction: {e.DismissDirection}");
    // Remove from UI, update database, etc.
};
```

## IsDismissed Property

The `IsDismissed` property allows you to get or set the dismissed state of the card programmatically. This is useful for dismissing cards based on business logic rather than user gestures.

**Type:** `bool`  
**Default:** `false`

### Basic Usage

**XAML:**
```xml
<cards:SfCardView x:Name="myCard" IsDismissed="False">
    <Label Text="SfCardView"/>
</cards:SfCardView>

<Button Text="Dismiss Card" Clicked="OnDismissClicked"/>
```

**C# (Code-behind):**
```csharp
private void OnDismissClicked(object sender, EventArgs e)
{
    myCard.IsDismissed = true;
}
```

### Programmatic Dismissal Example

```csharp
SfCardView cardView = new SfCardView
{
    IsDismissed = false,
    Content = new Label { Text = "Auto-dismiss in 3 seconds" }
};

// Dismiss after 3 seconds
Device.StartTimer(TimeSpan.FromSeconds(3), () =>
{
    cardView.IsDismissed = true;
    return false; // Stop timer
});
```

### Checking Dismissed State

```csharp
if (cardView.IsDismissed)
{
    Console.WriteLine("Card is currently dismissed");
}
else
{
    Console.WriteLine("Card is visible");
}
```

### Restoring a Dismissed Card

```csharp
// Re-show a dismissed card
cardView.IsDismissed = false;
```

## FadeOutOnSwiping Property

The `FadeOutOnSwiping` property enables a fade effect as the card is swiped, creating a smooth visual transition during dismissal.

**Type:** `bool`  
**Default:** `false`

**IMPORTANT:** This property only works for standalone `SfCardView`. It does NOT work when SfCardView is a child of SfCardLayout.

### Basic Usage

**XAML:**
```xml
<cards:SfCardView FadeOutOnSwiping="True" SwipeToDismiss="True">
    <Label Text="Swipe me - I fade out!" 
           Background="LightBlue"
           HorizontalTextAlignment="Center" 
           VerticalTextAlignment="Center"/>
</cards:SfCardView>
```

**C#:**
```csharp
SfCardView cardView = new SfCardView
{
    FadeOutOnSwiping = true,
    SwipeToDismiss = true,
    Content = new Label
    {
        Text = "Swipe me - I fade out!",
        HorizontalTextAlignment = TextAlignment.Center,
        VerticalTextAlignment = TextAlignment.Center,
        BackgroundColor = Colors.LightBlue
    }
};
```

### Visual Effect

- **Without FadeOutOnSwiping:** Card moves horizontally at full opacity until dismissed
- **With FadeOutOnSwiping:** Card gradually becomes transparent as it moves, creating a smooth fade-out effect

### Example: Elegant Dismissal

```csharp
var elegantCard = new SfCardView
{
    SwipeToDismiss = true,
    FadeOutOnSwiping = true,
    CornerRadius = 12,
    BorderWidth = 0,
    BackgroundColor = Colors.White,
    Shadow = new Shadow
    {
        Brush = Colors.Black,
        Opacity = 0.3f,
        Radius = 10,
        Offset = new Point(0, 2)
    },
    Content = new Grid
    {
        Padding = 20,
        Children =
        {
            new Label 
            { 
                Text = "Elegant Card",
                FontSize = 20,
                HorizontalOptions = LayoutOptions.Center,
                VerticalOptions = LayoutOptions.Center
            }
        }
    }
};
```

## Common Scenarios

### Scenario 1: Dismissible Alert

```csharp
public class DismissibleAlert : SfCardView
{
    public DismissibleAlert(string message, Color backgroundColor)
    {
        SwipeToDismiss = true;
        FadeOutOnSwiping = true;
        CornerRadius = 8;
        Margin = 10;
        BackgroundColor = backgroundColor;
        
        Content = new Label
        {
            Text = message,
            Padding = 15,
            TextColor = Colors.White,
            FontAttributes = FontAttributes.Bold
        };
        
        // Auto-dismiss after 5 seconds
        Device.StartTimer(TimeSpan.FromSeconds(5), () =>
        {
            IsDismissed = true;
            return false;
        });
    }
}

// Usage
var alert = new DismissibleAlert("Operation successful!", Colors.Green);
```

### Scenario 2: Inbox Message Card

```csharp
public SfCardView CreateMessageCard(string sender, string subject, string preview)
{
    var card = new SfCardView
    {
        SwipeToDismiss = true,
        Padding = 15,
        Margin = new Thickness(10, 5),
        CornerRadius = 10,
        BorderWidth = 1,
        BorderColor = Colors.LightGray,
        BackgroundColor = Colors.White
    };
    
    var layout = new VerticalStackLayout
    {
        Spacing = 5,
        Children =
        {
            new Label 
            { 
                Text = sender, 
                FontSize = 16, 
                FontAttributes = FontAttributes.Bold 
            },
            new Label 
            { 
                Text = subject, 
                FontSize = 14 
            },
            new Label 
            { 
                Text = preview, 
                FontSize = 12, 
                TextColor = Colors.Gray,
                MaxLines = 2,
                LineBreakMode = LineBreakMode.TailTruncation
            }
        }
    };
    
    card.Content = layout;
    
    // Handle dismissal
    card.Dismissed += (s, e) =>
    {
        // Archive or delete message
        ArchiveMessage(subject);
    };
    
    return card;
}
```

### Scenario 3: Todo Item Card

```csharp
public SfCardView CreateTodoCard(TodoItem item)
{
    var card = new SfCardView
    {
        SwipeToDismiss = true,
        FadeOutOnSwiping = true,
        CornerRadius = 8,
        Margin = new Thickness(15, 5),
        BackgroundColor = item.IsCompleted ? Colors.LightGray : Colors.White
    };
    
    var grid = new Grid
    {
        Padding = 15,
        ColumnDefinitions =
        {
            new ColumnDefinition { Width = GridLength.Star },
            new ColumnDefinition { Width = GridLength.Auto }
        }
    };
    
    grid.Add(new Label 
    { 
        Text = item.Title,
        VerticalOptions = LayoutOptions.Center,
        TextDecorations = item.IsCompleted ? TextDecorations.Strikethrough : TextDecorations.None
    }, 0, 0);
    
    grid.Add(new CheckBox 
    { 
        IsChecked = item.IsCompleted,
        VerticalOptions = LayoutOptions.Center
    }, 1, 0);
    
    card.Content = grid;
    
    // When dismissed, mark as completed
    card.Dismissed += (s, e) =>
    {
        item.IsCompleted = true;
        SaveTodoItem(item);
    };
    
    return card;
}
```

## Best Practices

### 1. Use SwipeToDismiss for Temporary Content

Perfect for:
- Notifications
- Alerts
- Temporary messages
- Dismissible list items

### 2. Combine with Events

Always handle the `Dismissed` event to clean up or update state:

```csharp
cardView.Dismissed += (s, e) =>
{
    // Update database
    // Remove from collection
    // Log analytics
};
```

### 3. Provide Visual Feedback

Use `FadeOutOnSwiping` for smooth, polished dismissal animations:

```csharp
cardView.FadeOutOnSwiping = true; // Better UX
```

### 4. Set Appropriate Sizing

Give cards enough space to be swipeable:

```csharp
cardView.HeightRequest = 100; // Minimum recommended
cardView.WidthRequest = 300;
```

### 5. Consider Auto-Dismiss

For time-sensitive notifications, combine swipe-to-dismiss with auto-dismiss:

```csharp
var card = new SfCardView { SwipeToDismiss = true };

// Auto-dismiss after timeout
Device.StartTimer(TimeSpan.FromSeconds(5), () =>
{
    card.IsDismissed = true;
    return false;
});
```

## Limitations

### 1. SwipeToDismiss in CardLayout

**Issue:** `SwipeToDismiss` does NOT work when `SfCardView` is a child of `SfCardLayout`.

**Reason:** In CardLayout, swipe gestures are used for card navigation between stacked cards.

**Solution:** Use `SwipeToDismiss` only for standalone cards.

### 2. FadeOutOnSwiping in CardLayout

**Issue:** `FadeOutOnSwiping` does NOT work when `SfCardView` is a child of `SfCardLayout`.

**Solution:** Use `FadeOutOnSwiping` only for standalone cards.

### 3. Swipe Direction

**Limitation:** Swipe-to-dismiss only supports left and right directions. Top and bottom swipes are not supported for dismissal.

## Performance Tips

1. **Dispose properly:** When dismissing cards, ensure proper cleanup
2. **Use IsDismissed for batch operations:** More efficient than animating multiple dismissals
3. **Limit card count:** For many dismissible items, consider virtualization patterns
