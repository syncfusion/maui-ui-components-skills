# Customization and Styling

## Table of Contents
- [Overview](#overview)
- [Border Customization](#border-customization)
- [Corner Radius](#corner-radius)
- [Background and Colors](#background-and-colors)
- [Indicator Customization](#indicator-customization)
- [Shadows and Elevation](#shadows-and-elevation)
- [Advanced Styling Examples](#advanced-styling-examples)
- [Best Practices](#best-practices)

## Overview

The .NET MAUI Cards control provides extensive customization options for creating visually appealing and distinctive card designs. You can customize borders, corners, colors, indicators, and more to match your application's design language.

## Border Customization

### BorderColor Property

Sets the border color of the card view.

**Type:** `Color`  
**Default:** `Transparent`

**XAML:**
```xml
<cards:SfCardView BorderColor="Blue" BorderWidth="2">
    <Label Text="Card with blue border"/>
</cards:SfCardView>
```

**C#:**
```csharp
var card = new SfCardView
{
    BorderColor = Colors.Blue,
    BorderWidth = 2,
    Content = new Label { Text = "Card with blue border" }
};
```

### BorderWidth Property

Sets the thickness of the card's border.

**Type:** `double`  
**Default:** `0`

**XAML:**
```xml
<cards:SfCardView BorderColor="Gray" BorderWidth="3">
    <Label Text="Thick border card"/>
</cards:SfCardView>
```

**C#:**
```csharp
var card = new SfCardView
{
    BorderColor = Colors.Gray,
    BorderWidth = 3,
    Content = new Label { Text = "Thick border card" }
};
```

### Border Examples

**Subtle Border:**
```csharp
var card = new SfCardView
{
    BorderColor = Colors.LightGray,
    BorderWidth = 1,
    BackgroundColor = Colors.White
};
```

**Accent Border:**
```csharp
var card = new SfCardView
{
    BorderColor = Colors.Blue,
    BorderWidth = 2,
    BackgroundColor = Colors.White
};
```

**No Border (Flat Design):**
```csharp
var card = new SfCardView
{
    BorderWidth = 0,  // No border
    BackgroundColor = Colors.White
};
```

## Corner Radius

The `CornerRadius` property allows you to create rounded corners on cards.

**Type:** `CornerRadius`  
**Default:** `0` (sharp corners)

### Uniform Corners

**XAML:**
```xml
<cards:SfCardView CornerRadius="15">
    <Label Text="Rounded card"/>
</cards:SfCardView>
```

**C#:**
```csharp
var card = new SfCardView
{
    CornerRadius = 15,
    Content = new Label { Text = "Rounded card" }
};
```

### Individual Corner Customization

**XAML:**
```xml
<cards:SfCardView>
    <cards:SfCardView.CornerRadius>
        <CornerRadius TopLeft="20" TopRight="20" BottomLeft="5" BottomRight="5"/>
    </cards:SfCardView.CornerRadius>
    <Label Text="Custom corners"/>
</cards:SfCardView>
```

**C#:**
```csharp
var card = new SfCardView
{
    CornerRadius = new CornerRadius(20, 20, 5, 5),  // TL, TR, BL, BR
    Content = new Label { Text = "Custom corners" }
};
```

### Corner Radius Guidelines

- **0-5**: Subtle rounding
- **8-12**: Standard modern design
- **15-20**: Prominent rounded look
- **25+**: Pill-shaped or circular

## Background and Colors

### Solid Colors

**XAML:**
```xml
<cards:SfCardView BackgroundColor="PeachPuff">
    <Label Text="Colored card"/>
</cards:SfCardView>
```

**C#:**
```csharp
var card = new SfCardView
{
    BackgroundColor = Colors.PeachPuff,
    Content = new Label { Text = "Colored card" }
};
```

### Gradient Backgrounds

**XAML:**
```xml
<cards:SfCardView>
    <cards:SfCardView.Background>
        <LinearGradientBrush StartPoint="0,0" EndPoint="1,1">
            <GradientStop Color="#6a11cb" Offset="0.0"/>
            <GradientStop Color="#2575fc" Offset="1.0"/>
        </LinearGradientBrush>
    </cards:SfCardView.Background>
    <Label Text="Gradient card" TextColor="White"/>
</cards:SfCardView>
```

**C#:**
```csharp
var card = new SfCardView
{
    Background = new LinearGradientBrush
    {
        StartPoint = new Point(0, 0),
        EndPoint = new Point(1, 1),
        GradientStops =
        {
            new GradientStop { Color = Color.FromArgb("#6a11cb"), Offset = 0.0f },
            new GradientStop { Color = Color.FromArgb("#2575fc"), Offset = 1.0f }
        }
    },
    Content = new Label 
    { 
        Text = "Gradient card",
        TextColor = Colors.White
    }
};
```

### Transparent Backgrounds

```csharp
var card = new SfCardView
{
    BackgroundColor = Colors.Transparent,
    Content = new Label { Text = "Transparent card" }
};
```

## Indicator Customization

Indicators are visual elements (usually a colored line/bar) that can appear on any edge of the card to signify status, category, or importance.

### IndicatorColor Property

Sets the color of the indicator.

**Type:** `Color`  
**Default:** `Transparent`

**XAML:**
```xml
<cards:SfCardView IndicatorColor="Red" IndicatorThickness="5" IndicatorPosition="Left">
    <Label Text="Card with red indicator"/>
</cards:SfCardView>
```

**C#:**
```csharp
var card = new SfCardView
{
    IndicatorColor = Colors.Red,
    IndicatorThickness = 5,
    IndicatorPosition = IndicatorPosition.Left,
    Content = new Label { Text = "Card with red indicator" }
};
```

### IndicatorThickness Property

Sets the thickness of the indicator line.

**Type:** `double`  
**Default:** `0`

**Examples:**
```csharp
// Subtle indicator
card.IndicatorThickness = 3;

// Standard indicator
card.IndicatorThickness = 5;

// Prominent indicator
card.IndicatorThickness = 8;

// Bold indicator
card.IndicatorThickness = 12;
```

### IndicatorPosition Property

Sets the position of the indicator.

**Type:** `IndicatorPosition` (enum)  
**Values:** `Top`, `Bottom`, `Left`, `Right`  
**Default:** `Left`

**XAML:**
```xml
<!-- Top indicator -->
<cards:SfCardView IndicatorColor="Blue" IndicatorThickness="4" IndicatorPosition="Top">
    <Label Text="Top indicator"/>
</cards:SfCardView>

<!-- Right indicator -->
<cards:SfCardView IndicatorColor="Green" IndicatorThickness="4" IndicatorPosition="Right">
    <Label Text="Right indicator"/>
</cards:SfCardView>

<!-- Bottom indicator -->
<cards:SfCardView IndicatorColor="Orange" IndicatorThickness="4" IndicatorPosition="Bottom">
    <Label Text="Bottom indicator"/>
</cards:SfCardView>
```

**C#:**
```csharp
// Left indicator (default)
card.IndicatorPosition = IndicatorPosition.Left;

// Top indicator
card.IndicatorPosition = IndicatorPosition.Top;

// Right indicator
card.IndicatorPosition = IndicatorPosition.Right;

// Bottom indicator
card.IndicatorPosition = IndicatorPosition.Bottom;
```

### Indicator Use Cases

**Status Indication:**
```csharp
// Priority levels
var highPriorityCard = new SfCardView
{
    IndicatorColor = Colors.Red,
    IndicatorThickness = 5,
    IndicatorPosition = IndicatorPosition.Left
};

var mediumPriorityCard = new SfCardView
{
    IndicatorColor = Colors.Orange,
    IndicatorThickness = 5,
    IndicatorPosition = IndicatorPosition.Left
};

var lowPriorityCard = new SfCardView
{
    IndicatorColor = Colors.Green,
    IndicatorThickness = 5,
    IndicatorPosition = IndicatorPosition.Left
};
```

**Category Indication:**
```csharp
public SfCardView CreateCategoryCard(string category)
{
    var colorMap = new Dictionary<string, Color>
    {
        { "Work", Colors.Blue },
        { "Personal", Colors.Green },
        { "Urgent", Colors.Red },
        { "Ideas", Colors.Purple }
    };
    
    return new SfCardView
    {
        IndicatorColor = colorMap[category],
        IndicatorThickness = 6,
        IndicatorPosition = IndicatorPosition.Left,
        Content = new Label { Text = $"{category} Task" }
    };
}
```

## Advanced Styling Examples

### Example 1: Credit Card Design

```csharp
public SfCardView CreateCreditCard(string bankName, string cardNumber, string holderName)
{
    var card = new SfCardView
    {
        BackgroundColor = Color.FromArgb("#472902"),
        CornerRadius = 15,
        HeightRequest = 200,
        WidthRequest = 350,
        Margin = 20,
    };
    
    var grid = new Grid
    {
        Padding = 20,
        RowDefinitions =
        {
            new RowDefinition { Height = GridLength.Auto },
            new RowDefinition { Height = GridLength.Auto },
            new RowDefinition { Height = 30 },
            new RowDefinition { Height = GridLength.Auto }
        }
    };
    
    // Bank name
    grid.Add(new Label
    {
        Text = bankName,
        TextColor = Colors.White,
        FontSize = 20,
        FontAttributes = FontAttributes.Bold,
        HorizontalOptions = LayoutOptions.Start
    }, 0, 0);
    
    // Chip and card type
    var chipGrid = new Grid
    {
        Padding = new Thickness(0, 20, 0, 15),
        ColumnDefinitions =
        {
            new ColumnDefinition { Width = 60 },
            new ColumnDefinition { Width = GridLength.Star }
        }
    };
    
    chipGrid.Add(new Image
    {
        Source = "cardchip.png",
        WidthRequest = 60,
        HeightRequest = 30,
        HorizontalOptions = LayoutOptions.Center,
        VerticalOptions = LayoutOptions.Center
    }, 0, 0);
    
    chipGrid.Add(new Label
    {
        Text = "Business Elite",
        FontAttributes = FontAttributes.Bold,
        TextColor = Colors.White,
        FontSize = 17,
        HorizontalOptions = LayoutOptions.Start,
        VerticalOptions = LayoutOptions.Center,
        Padding = new Thickness(30, 0, 0, 0)
    }, 1, 0);
    
    grid.Add(chipGrid, 0, 1);
    
    // Cardholder name
    grid.Add(new Label
    {
        Text = holderName,
        FontSize = 17,
        FontAttributes = FontAttributes.Bold,
        TextColor = Colors.White,
        HorizontalOptions = LayoutOptions.Start,
        VerticalOptions = LayoutOptions.End
    }, 0, 2);
    
    // Card number
    grid.Add(new Label
    {
        Text = cardNumber,
        TextColor = Colors.White,
        FontSize = 16,
        HorizontalOptions = LayoutOptions.Start,
        VerticalOptions = LayoutOptions.End,
        Padding = new Thickness(0, 10, 0, 0),
        LetterSpacing = 2
    }, 0, 3);
    
    card.Content = grid;
    return card;
}

// Usage
var creditCard = CreateCreditCard(
    "Wells Fargo", 
    "9057 4081 2175 0056",
    "Rick Sanchez"
);
```

### Example 2: Material Design Card

```csharp
public SfCardView CreateMaterialCard(string title, string subtitle, string body)
{
    var card = new SfCardView
    {
        BackgroundColor = Colors.White,
        CornerRadius = 4,
        BorderWidth = 0,
        Margin = 10,
    };
    
    var layout = new VerticalStackLayout
    {
        Padding = 16,
        Spacing = 8
    };
    
    layout.Children.Add(new Label
    {
        Text = title,
        FontSize = 20,
        FontAttributes = FontAttributes.Bold,
        TextColor = Colors.Black
    });
    
    layout.Children.Add(new Label
    {
        Text = subtitle,
        FontSize = 14,
        TextColor = Colors.Gray
    });
    
    layout.Children.Add(new BoxView
    {
        HeightRequest = 1,
        BackgroundColor = Colors.LightGray,
        Margin = new Thickness(0, 8)
    });
    
    layout.Children.Add(new Label
    {
        Text = body,
        FontSize = 14,
        TextColor = Colors.DarkGray
    });
    
    card.Content = layout;
    return card;
}
```

### Example 3: Status Card with Indicator

```csharp
public SfCardView CreateStatusCard(string status, string message, Color statusColor)
{
    var card = new SfCardView
    {
        BackgroundColor = Colors.White,
        CornerRadius = 8,
        BorderWidth = 1,
        BorderColor = Colors.LightGray,
        Margin = 10,
        IndicatorColor = statusColor,
        IndicatorThickness = 6,
        IndicatorPosition = IndicatorPosition.Left
    };
    
    var layout = new VerticalStackLayout
    {
        Padding = 15,
        Spacing = 5
    };
    
    layout.Children.Add(new Label
    {
        Text = status.ToUpper(),
        FontSize = 12,
        FontAttributes = FontAttributes.Bold,
        TextColor = statusColor
    });
    
    layout.Children.Add(new Label
    {
        Text = message,
        FontSize = 16,
        TextColor = Colors.Black
    });
    
    card.Content = layout;
    return card;
}

// Usage
var successCard = CreateStatusCard("Success", "Operation completed", Colors.Green);
var warningCard = CreateStatusCard("Warning", "Please review", Colors.Orange);
var errorCard = CreateStatusCard("Error", "Something went wrong", Colors.Red);
```

### Example 4: Image Card with Gradient Overlay

```csharp
public SfCardView CreateImageCard(string imageUrl, string title, string subtitle)
{
    var card = new SfCardView
    {
        CornerRadius = 15,
        HeightRequest = 300,
        WidthRequest = 250,
        Margin = 10
    };
    
    var grid = new Grid();
    
    // Background image
    grid.Children.Add(new Image
    {
        Source = imageUrl,
        Aspect = Aspect.AspectFill
    });
    
    // Gradient overlay
    var overlayGrid = new Grid
    {
        VerticalOptions = LayoutOptions.End
    };
    
    overlayGrid.Background = new LinearGradientBrush
    {
        StartPoint = new Point(0, 0),
        EndPoint = new Point(0, 1),
        GradientStops =
        {
            new GradientStop { Color = Colors.Transparent, Offset = 0.0f },
            new GradientStop { Color = Colors.Black.WithAlpha(0.8f), Offset = 1.0f }
        }
    };
    
    var textLayout = new VerticalStackLayout
    {
        Padding = 20,
        Spacing = 5
    };
    
    textLayout.Children.Add(new Label
    {
        Text = title,
        FontSize = 22,
        FontAttributes = FontAttributes.Bold,
        TextColor = Colors.White
    });
    
    textLayout.Children.Add(new Label
    {
        Text = subtitle,
        FontSize = 14,
        TextColor = Colors.White
    });
    
    overlayGrid.Children.Add(textLayout);
    grid.Children.Add(overlayGrid);
    
    card.Content = grid;
    return card;
}
```

### Example 5: Glassmorphism Card

```csharp
public SfCardView CreateGlassCard(string content)
{
    var card = new SfCardView
    {
        BackgroundColor = Colors.White.WithAlpha(0.1f),
        CornerRadius = 20,
        BorderWidth = 1,
        BorderColor = Colors.White.WithAlpha(0.2f),
        Margin = 15,
    };
    
    var layout = new VerticalStackLayout
    {
        Padding = 30,
        Spacing = 10
    };
    
    layout.Children.Add(new Label
    {
        Text = content,
        FontSize = 18,
        TextColor = Colors.White,
        HorizontalTextAlignment = TextAlignment.Center
    });
    
    card.Content = layout;
    return card;
}

// Use on a gradient background for best effect
```

## Best Practices

### 1. Consistent Corner Radius

Use consistent corner radius across your app:

```csharp
// Define constants
public static class CardStyles
{
    public const double StandardRadius = 12;
    public const double SmallRadius = 8;
    public const double LargeRadius = 20;
}

// Apply consistently
var card = new SfCardView { CornerRadius = CardStyles.StandardRadius };
```

### 2. Use Indicators for Status

Leverage indicators for quick visual scanning:

```csharp
public static Color GetPriorityColor(Priority priority)
{
    return priority switch
    {
        Priority.High => Colors.Red,
        Priority.Medium => Colors.Orange,
        Priority.Low => Colors.Green,
        _ => Colors.Gray
    };
}
```

### 3. Match Your Brand

Create themed card styles:

```csharp
public static SfCardView CreateBrandedCard()
{
    return new SfCardView
    {
        BackgroundColor = AppColors.Primary,
        CornerRadius = AppSizes.CornerRadius,
        BorderWidth = 0,
    };
}
```

### 4. Consider Dark Mode

Adjust colors for theme:

```csharp
var isDarkMode = Application.Current.RequestedTheme == AppTheme.Dark;

var card = new SfCardView
{
    BackgroundColor = isDarkMode ? Colors.Dark : Colors.White,
    BorderColor = isDarkMode ? Colors.DarkGray : Colors.LightGray
};
```

### 5. Performance Considerations

- Limit shadow usage on many cards
- Use simple gradients over complex ones
- Avoid unnecessary transparency layers
