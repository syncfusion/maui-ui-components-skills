# Card Layout Features

## Table of Contents
- [Overview](#overview)
- [ShowSwipedCard Property](#showswipedcard-property)
- [VisibleIndex Property](#visibleindex-property)
- [SwipeDirection Property](#swipedirection-property)
- [Managing Multiple Cards](#managing-multiple-cards)
- [Common Scenarios](#common-scenarios)
- [Best Practices](#best-practices)

## Overview

`SfCardLayout` is a container that displays multiple `SfCardView` items in a stacked layout. Only one card is visible at a time, and users can swipe to navigate between cards. This creates an interactive, Tinder-style card interface.

**Key Features:**
- Stack multiple cards with only one visible
- Swipe navigation in four directions (Left, Right, Top, Bottom)
- Show swiped cards at layout edges
- Programmatic control over visible card
- Full event support for card changes

**Important:** SfCardLayout only accepts `SfCardView` as direct children.

## ShowSwipedCard Property

The `ShowSwipedCard` property determines whether swiped cards are displayed at the edge of the card layout after being dismissed.

**Type:** `bool`  
**Default:** `false`

### Basic Usage

**XAML:**
```xml
<cards:SfCardLayout ShowSwipedCard="True" HeightRequest="400">
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

**C#:**
```csharp
SfCardLayout cardLayout = new SfCardLayout
{
    ShowSwipedCard = true,
    HeightRequest = 400
};

cardLayout.Children.Add(new SfCardView 
{ 
    Content = new Label { Text = "Card 1", BackgroundColor = Colors.Cyan } 
});

cardLayout.Children.Add(new SfCardView 
{ 
    Content = new Label { Text = "Card 2", BackgroundColor = Colors.Yellow } 
});

cardLayout.Children.Add(new SfCardView 
{ 
    Content = new Label { Text = "Card 3", BackgroundColor = Colors.Orange } 
});
```

### Visual Effect

- **ShowSwipedCard = false:** Swiped cards disappear completely
- **ShowSwipedCard = true:** Swiped cards remain visible at the edge, creating a stack effect

### When to Use

**Enable ShowSwipedCard when:**
- Users need visual context of how many cards remain
- Creating a "deck of cards" visual metaphor
- Building browsing interfaces where users can see discarded items

**Disable ShowSwipedCard when:**
- You want a cleaner, focused interface
- Creating a "one at a time" flow
- Performance is critical with many cards

## VisibleIndex Property

The `VisibleIndex` property gets or sets the index of the card that should be displayed at the front of the card layout.

**Type:** `int`  
**Default:** `0`

### Basic Usage

**XAML:**
```xml
<cards:SfCardLayout VisibleIndex="1" HeightRequest="400">
    <cards:SfCardView>
        <Label Text="Card 0"/>
    </cards:SfCardView>
    
    <cards:SfCardView>
        <Label Text="Card 1 - Visible on start"/>
    </cards:SfCardView>
    
    <cards:SfCardView>
        <Label Text="Card 2"/>
    </cards:SfCardView>
</cards:SfCardLayout>
```

**C#:**
```csharp
SfCardLayout cardLayout = new SfCardLayout
{
    VisibleIndex = 1,  // Start with second card
    HeightRequest = 400
};

// Add cards...
```

### Programmatic Navigation

```csharp
// Navigate to specific card
cardLayout.VisibleIndex = 2;  // Show third card

// Navigate to next card
cardLayout.VisibleIndex++;

// Navigate to previous card
cardLayout.VisibleIndex--;

// Navigate to first card
cardLayout.VisibleIndex = 0;

// Navigate to last card
cardLayout.VisibleIndex = cardLayout.Children.Count - 1;
```

### Example: Navigation Buttons

```csharp
var cardLayout = new SfCardLayout { HeightRequest = 400 };

// Add cards
for (int i = 0; i < 5; i++)
{
    cardLayout.Children.Add(new SfCardView
    {
        Content = new Label 
        { 
            Text = $"Card {i + 1}",
            HorizontalTextAlignment = TextAlignment.Center,
            VerticalTextAlignment = TextAlignment.Center,
            FontSize = 24
        }
    });
}

// Navigation controls
var buttonStack = new HorizontalStackLayout
{
    HorizontalOptions = LayoutOptions.Center,
    Spacing = 20
};

var prevButton = new Button { Text = "Previous" };
prevButton.Clicked += (s, e) =>
{
    if (cardLayout.VisibleIndex > 0)
        cardLayout.VisibleIndex--;
};

var nextButton = new Button { Text = "Next" };
nextButton.Clicked += (s, e) =>
{
    if (cardLayout.VisibleIndex < cardLayout.Children.Count - 1)
        cardLayout.VisibleIndex++;
};

buttonStack.Children.Add(prevButton);
buttonStack.Children.Add(nextButton);
```

### Getting Current Index

```csharp
int currentIndex = cardLayout.VisibleIndex;
Console.WriteLine($"Currently showing card {currentIndex}");

// Get current card
if (cardLayout.VisibleIndex >= 0 && 
    cardLayout.VisibleIndex < cardLayout.Children.Count)
{
    var currentCard = cardLayout.Children[cardLayout.VisibleIndex] as SfCardView;
}
```

## SwipeDirection Property

The `SwipeDirection` property specifies the direction(s) in which cards can be swiped. This controls the swipe gesture behavior.

**Type:** `CardSwipeDirection` (enum with flags support)  
**Default:** `CardSwipeDirection.Right`

**Available Values:**
- `CardSwipeDirection.Left`
- `CardSwipeDirection.Right`
- `CardSwipeDirection.Top`
- `CardSwipeDirection.Bottom`

### Single Direction Examples

**Swipe Left Only:**
```xml
<cards:SfCardLayout SwipeDirection="Left" HeightRequest="400">
    <!-- Cards -->
</cards:SfCardLayout>
```

```csharp
SfCardLayout cardLayout = new SfCardLayout
{
    SwipeDirection = CardSwipeDirection.Left,
    HeightRequest = 400
};
```

**Swipe Right Only:**
```csharp
cardLayout.SwipeDirection = CardSwipeDirection.Right;
```

**Swipe Up (Top):**
```csharp
cardLayout.SwipeDirection = CardSwipeDirection.Top;
```

**Swipe Down (Bottom):**
```csharp
cardLayout.SwipeDirection = CardSwipeDirection.Bottom;
```

### Direction-Based Actions Example

```csharp
var cardLayout = new SfCardLayout
{
    SwipeDirection = CardSwipeDirection.Left,
    ShowSwipedCard = true,
    HeightRequest = 500
};

// Track swipe directions
cardLayout.VisibleIndexChanged += (s, e) =>
{
    // Determine swipe direction based on index change
    if (e.NewIndex > e.OldIndex)
    {
        Console.WriteLine("Swiped left (next card)");
        // Handle "reject" action
    }
    else if (e.NewIndex < e.OldIndex)
    {
        Console.WriteLine("Swiped right (previous card)");
        // Handle "accept" action
    }
};
```

### Use Cases by Direction

**Left/Right (Horizontal):**
- Dating apps (swipe left to pass, right to like)
- Product browsing
- Image galleries
- Content feeds

**Top/Bottom (Vertical):**
- Story viewers
- Vertical content feeds
- News articles
- Social media posts

## Managing Multiple Cards

### Adding Cards Dynamically

```csharp
SfCardLayout cardLayout = new SfCardLayout();

// Add cards programmatically
for (int i = 1; i <= 10; i++)
{
    var card = new SfCardView
    {
        CornerRadius = 15,
        Margin = 5,
        Content = new Grid
        {
            BackgroundColor = GetRandomColor(),
            Children =
            {
                new Label
                {
                    Text = $"Card {i}",
                    HorizontalOptions = LayoutOptions.Center,
                    VerticalOptions = LayoutOptions.Center,
                    FontSize = 28,
                    TextColor = Colors.White
                }
            }
        }
    };
    
    cardLayout.Children.Add(card);
}
```

### Removing Cards

```csharp
// Remove specific card
cardLayout.Children.RemoveAt(index);

// Remove current visible card
if (cardLayout.VisibleIndex >= 0 && 
    cardLayout.VisibleIndex < cardLayout.Children.Count)
{
    cardLayout.Children.RemoveAt(cardLayout.VisibleIndex);
}

// Clear all cards
cardLayout.Children.Clear();
```

### Inserting Cards

```csharp
// Insert at specific position
var newCard = new SfCardView { Content = new Label { Text = "New Card" } };
cardLayout.Children.Insert(2, newCard);

// Add to end
cardLayout.Children.Add(newCard);
```

### Getting Card Count

```csharp
int totalCards = cardLayout.Children.Count;
int remainingCards = cardLayout.Children.Count - cardLayout.VisibleIndex;
```

## Common Scenarios

### Scenario 1: Dating App Style Interface

```csharp
public class ProfileCardLayout : ContentView
{
    private SfCardLayout cardLayout;
    private List<Profile> profiles;
    
    public ProfileCardLayout(List<Profile> profiles)
    {
        this.profiles = profiles;
        
        cardLayout = new SfCardLayout
        {
            SwipeDirection = CardSwipeDirection.Right,
            ShowSwipedCard = true,
            HeightRequest = 500,
            WidthRequest = 350,
            BackgroundColor = Colors.Transparent
        };
        
        // Populate cards
        foreach (var profile in profiles)
        {
            cardLayout.Children.Add(CreateProfileCard(profile));
        }
        
        // Handle swipes
        cardLayout.VisibleIndexChanged += OnCardSwiped;
        
        Content = cardLayout;
    }
    
    private SfCardView CreateProfileCard(Profile profile)
    {
        return new SfCardView
        {
            CornerRadius = 20,
            Content = new Grid
            {
                Children =
                {
                    new Image { Source = profile.PhotoUrl, Aspect = Aspect.AspectFill },
                    new VerticalStackLayout
                    {
                        VerticalOptions = LayoutOptions.End,
                        Padding = 20,
                        BackgroundColor = Colors.Black.WithAlpha(0.5f),
                        Children =
                        {
                            new Label 
                            { 
                                Text = profile.Name, 
                                FontSize = 24, 
                                TextColor = Colors.White,
                                FontAttributes = FontAttributes.Bold
                            },
                            new Label 
                            { 
                                Text = $"{profile.Age}, {profile.Location}",
                                TextColor = Colors.White
                            }
                        }
                    }
                }
            }
        };
    }
    
    private void OnCardSwiped(object sender, CardVisibleIndexChangedEventArgs e)
    {
        if (e.NewIndex > e.OldIndex)
        {
            // Swiped left - Pass
            ProcessSwipe(profiles[e.OldIndex], SwipeAction.Pass);
        }
        else
        {
            // Swiped right - Like
            ProcessSwipe(profiles[e.OldIndex], SwipeAction.Like);
        }
    }
}
```

### Scenario 2: Product Showcase

```csharp
public SfCardLayout CreateProductShowcase(List<Product> products)
{
    var cardLayout = new SfCardLayout
    {
        SwipeDirection = CardSwipeDirection.Left,
        ShowSwipedCard = false,
        HeightRequest = 600,
        VisibleIndex = 0
    };
    
    foreach (var product in products)
    {
        var card = new SfCardView
        {
            CornerRadius = 15,
            BorderWidth = 1,
            BorderColor = Colors.LightGray,
            Content = new Grid
            {
                RowDefinitions =
                {
                    new RowDefinition { Height = 300 },
                    new RowDefinition { Height = GridLength.Auto }
                },
                Children =
                {
                    new Image 
                    { 
                        Source = product.ImageUrl,
                        Aspect = Aspect.AspectFill
                    }.Row(0),
                    
                    new VerticalStackLayout
                    {
                        Padding = 15,
                        Children =
                        {
                            new Label 
                            { 
                                Text = product.Name,
                                FontSize = 20,
                                FontAttributes = FontAttributes.Bold
                            },
                            new Label 
                            { 
                                Text = product.Description,
                                FontSize = 14,
                                TextColor = Colors.Gray
                            },
                            new Label 
                            { 
                                Text = $"${product.Price:F2}",
                                FontSize = 24,
                                TextColor = Colors.Green,
                                FontAttributes = FontAttributes.Bold
                            }
                        }
                    }.Row(1)
                }
            }
        };
        
        cardLayout.Children.Add(card);
    }
    
    return cardLayout;
}
```

### Scenario 3: Onboarding Flow

```csharp
public class OnboardingCards : ContentView
{
    private SfCardLayout cardLayout;
    private Button nextButton;
    
    public OnboardingCards()
    {
        cardLayout = new SfCardLayout
        {
            SwipeDirection = CardSwipeDirection.Left,
            VisibleIndex = 0,
            HeightRequest = 500
        };
        
        // Add onboarding screens
        cardLayout.Children.Add(CreateOnboardingCard(
            "Welcome", 
            "Welcome to our app!", 
            "welcome_icon.png"
        ));
        
        cardLayout.Children.Add(CreateOnboardingCard(
            "Features", 
            "Discover amazing features", 
            "features_icon.png"
        ));
        
        cardLayout.Children.Add(CreateOnboardingCard(
            "Get Started", 
            "Ready to begin?", 
            "start_icon.png"
        ));
        
        // Next button
        nextButton = new Button 
        { 
            Text = "Next",
            HorizontalOptions = LayoutOptions.Center
        };
        
        nextButton.Clicked += (s, e) =>
        {
            if (cardLayout.VisibleIndex < cardLayout.Children.Count - 1)
            {
                cardLayout.VisibleIndex++;
            }
            else
            {
                // Complete onboarding
                CompleteOnboarding();
            }
            
            // Update button text on last card
            nextButton.Text = cardLayout.VisibleIndex == cardLayout.Children.Count - 1 
                ? "Get Started" 
                : "Next";
        };
        
        Content = new VerticalStackLayout
        {
            Children = { cardLayout, nextButton }
        };
    }
    
    private SfCardView CreateOnboardingCard(string title, string description, string icon)
    {
        return new SfCardView
        {
            Content = new VerticalStackLayout
            {
                Spacing = 20,
                Padding = 30,
                Children =
                {
                    new Image { Source = icon, HeightRequest = 150 },
                    new Label 
                    { 
                        Text = title,
                        FontSize = 28,
                        FontAttributes = FontAttributes.Bold,
                        HorizontalTextAlignment = TextAlignment.Center
                    },
                    new Label 
                    { 
                        Text = description,
                        FontSize = 16,
                        HorizontalTextAlignment = TextAlignment.Center,
                        TextColor = Colors.Gray
                    }
                }
            }
        };
    }
}
```

## Best Practices

### 1. Set Appropriate Height

Always set `HeightRequest` for proper card display:

```csharp
cardLayout.HeightRequest = 500;  // Recommended minimum: 300-600
```

### 2. Limit Card Count

For performance, avoid adding hundreds of cards at once. Consider:
- Lazy loading
- Pagination
- Virtual scrolling for large datasets

### 3. Use ShowSwipedCard Wisely

Enable for better user context, disable for cleaner UI:

```csharp
cardLayout.ShowSwipedCard = true;  // Better for browsing
cardLayout.ShowSwipedCard = false; // Better for focused tasks
```

### 4. Handle Edge Cases

```csharp
// Check bounds before changing index
if (cardLayout.VisibleIndex >= 0 && 
    cardLayout.VisibleIndex < cardLayout.Children.Count)
{
    // Safe to access
}
```

### 5. Combine with Events

Always handle `VisibleIndexChanged` for tracking and analytics:

```csharp
cardLayout.VisibleIndexChanged += (s, e) =>
{
    LogCardView(e.NewIndex);
    UpdateUI();
};
```
