# Precision Modes in .NET MAUI Rating

The `Precision` property of the `SfRating` control defines the accuracy level of rating selection. It determines how users can rate items—whether in full increments, half-step increments, or exact decimal values.

## Overview

The Rating control supports three precision modes:

1. **Standard** - Full item rating (whole numbers)
2. **Half** - Half-step rating (0.5 increments)
3. **Exact** - Precise decimal rating (any value)

> **Default:** `Standard` precision

## Precision Enum Values

```csharp
public enum Precision
{
    Standard,  // Full item selection: 1, 2, 3, 4, 5
    Half,      // Half-step selection: 0.5, 1.0, 1.5, 2.0, 2.5, etc.
    Exact      // Exact decimal selection: 1.25, 2.73, 3.89, etc.
}
```

## Standard Precision

When the precision mode is set to `Standard`, rating items are filled completely based on the rating value. Users can only select whole number ratings.

### Behavior
- Click on star 1 → Value = 1.0
- Click on star 3 → Value = 3.0
- Click on star 5 → Value = 5.0

### Use Cases
- Simple yes/no quality ratings
- Basic satisfaction surveys
- Quick feedback mechanisms
- When granularity is not important

### Implementation

**XAML:**
```xml
<rating:SfRating x:Name="rating" 
                 Value="3"
                 Precision="Standard" />
```

**C#:**
```csharp
SfRating rating = new SfRating();
rating.Precision = Precision.Standard;
rating.Value = 3;  // Will display 3 full stars
```

### Example Output
```
Value = 1: ★☆☆☆☆
Value = 2: ★★☆☆☆
Value = 3: ★★★☆☆
Value = 4: ★★★★☆
Value = 5: ★★★★★
```

## Half Precision

When the precision mode is set to `Half`, rating items are filled partially based on the rating value. Users can select half-star increments.

### Behavior
- Click on left half of star 2 → Value = 1.5
- Click on right half of star 2 → Value = 2.0
- Click on left half of star 4 → Value = 3.5
- Click on right half of star 4 → Value = 4.0

### Use Cases
- Product reviews (e.g., Amazon, Yelp)
- Movie/TV ratings (e.g., Netflix, IMDb)
- App store ratings (Google Play, App Store)
- Restaurant reviews
- Hotel ratings

### Implementation

**XAML:**
```xml
<rating:SfRating x:Name="rating" 
                 Value="3.5"
                 Precision="Half" />
```

**C#:**
```csharp
SfRating rating = new SfRating();
rating.Precision = Precision.Half;
rating.Value = 3.5;  // Will display 3.5 stars (3 full + 1 half)
```

### Example Output
```
Value = 1.0: ★☆☆☆☆
Value = 1.5: ★½☆☆☆
Value = 2.0: ★★☆☆☆
Value = 2.5: ★★½☆☆
Value = 3.0: ★★★☆☆
Value = 3.5: ★★★½☆
Value = 4.0: ★★★★☆
Value = 4.5: ★★★★½
Value = 5.0: ★★★★★
```

### Visual Representation

Half precision shows partial fills:
- **Full star**: Value covers entire item (1.0, 2.0, 3.0, etc.)
- **Half star**: Value covers half of item (1.5, 2.5, 3.5, etc.)

## Exact Precision

When the precision mode is set to `Exact`, rating items are filled precisely based on the exact decimal rating value. Users can select any decimal position within the rating range.

### Behavior
- Click at 25% of star 3 → Value ≈ 2.25
- Click at 60% of star 4 → Value ≈ 3.60
- Click at 85% of star 2 → Value ≈ 1.85

The fill level corresponds directly to the click/tap position within the star.

### Use Cases
- Scientific data visualization
- Performance metrics (0-100 converted to 0-5)
- Data-driven dashboards
- Average rating displays (e.g., 4.73 stars)
- Precise measurement scales

### Implementation

**XAML:**
```xml
<rating:SfRating x:Name="rating" 
                 Value="3.75"
                 Precision="Exact" />
```

**C#:**
```csharp
SfRating rating = new SfRating();
rating.Precision = Precision.Exact;
rating.Value = 3.75;  // Will display 3.75 stars (3 full + 75% filled star)
```

### Example Output
```
Value = 1.25: ★¼☆☆☆
Value = 2.60: ★★⅗☆☆
Value = 3.33: ★★★⅓☆
Value = 4.75: ★★★★¾
Value = 4.89: ★★★★⁹⁄₁₀
```

### Visual Representation

Exact precision shows proportional fills matching the exact value:
- Value = 2.25 → 2 full stars + star 3 filled 25%
- Value = 3.80 → 3 full stars + star 4 filled 80%
- Value = 4.10 → 4 full stars + star 5 filled 10%

## Choosing the Right Precision

| Precision | Best For | User Experience | Data Granularity |
|-----------|----------|-----------------|------------------|
| **Standard** | Simple ratings, quick feedback | Easy, binary choice per item | Low (whole numbers) |
| **Half** | Product reviews, star ratings | Moderate, familiar to users | Medium (0.5 increments) |
| **Exact** | Displaying averages, metrics | Display-only or advanced users | High (any decimal) |

### Decision Guide

**Use Standard when:**
- Simplicity is key
- Users need quick, decisive ratings
- Exact precision is not important
- Creating basic feedback forms

**Use Half when:**
- Building consumer-facing ratings (products, movies, apps)
- Users need moderate precision
- Following industry standards (Amazon, Yelp, etc.)
- Balance between simplicity and granularity is needed

**Use Exact when:**
- Displaying calculated averages (e.g., "Rated 4.73 by 892 users")
- Showing precise measurements or scores
- Data visualization and dashboards
- Read-only displays of exact values

## Complete Examples

### Example 1: Product Review with Half Precision

```xml
<VerticalStackLayout Padding="20" Spacing="15">
    <Label Text="Rate this product:" FontSize="18" FontAttributes="Bold" />
    
    <rating:SfRating x:Name="productRating"
                     Value="4.5"
                     ItemCount="5"
                     Precision="Half"
                     ItemSize="40"
                     ValueChanged="OnRatingChanged" />
    
    <Label x:Name="ratingLabel" 
           Text="Rating: 4.5 stars"
           FontSize="14" 
           TextColor="Gray" />
</VerticalStackLayout>
```

```csharp
private void OnRatingChanged(object sender, ValueChangedEventArgs e)
{
    ratingLabel.Text = $"Rating: {e.Value:F1} stars";
}
```

### Example 2: Display Average Rating (Exact, Read-Only)

```xml
<HorizontalStackLayout Spacing="10">
    <rating:SfRating Value="4.73"
                     ItemCount="5"
                     Precision="Exact"
                     IsReadOnly="True"
                     ItemSize="30" />
    
    <Label Text="4.73 (892 reviews)" 
           VerticalOptions="Center"
           FontSize="14" />
</HorizontalStackLayout>
```

### Example 3: Comparison of All Precision Modes

```xml
<VerticalStackLayout Padding="20" Spacing="25">
    
    <!-- Standard Precision -->
    <StackLayout Spacing="10">
        <Label Text="Standard Precision" FontAttributes="Bold" />
        <rating:SfRating Value="3" Precision="Standard" ItemSize="35" />
        <Label Text="Value: 3.0 (whole numbers only)" FontSize="12" TextColor="Gray" />
    </StackLayout>
    
    <!-- Half Precision -->
    <StackLayout Spacing="10">
        <Label Text="Half Precision" FontAttributes="Bold" />
        <rating:SfRating Value="3.5" Precision="Half" ItemSize="35" />
        <Label Text="Value: 3.5 (0.5 increments)" FontSize="12" TextColor="Gray" />
    </StackLayout>
    
    <!-- Exact Precision -->
    <StackLayout Spacing="10">
        <Label Text="Exact Precision" FontAttributes="Bold" />
        <rating:SfRating Value="3.75" Precision="Exact" ItemSize="35" />
        <Label Text="Value: 3.75 (any decimal value)" FontSize="12" TextColor="Gray" />
    </StackLayout>
    
</VerticalStackLayout>
```

## Programmatic Value Setting

All precision modes support programmatic value setting:

```csharp
// Standard - will round to nearest whole number if needed
standardRating.Value = 3.7;  // Displays as 3 or 4

// Half - will round to nearest 0.5 if needed  
halfRating.Value = 3.7;  // Displays as 3.5 or 4.0

// Exact - displays exact value
exactRating.Value = 3.7;  // Displays as 3.7
```

## Data Binding

Precision modes work seamlessly with data binding:

```xml
<rating:SfRating Value="{Binding AverageRating}"
                 Precision="Exact"
                 IsReadOnly="True" />
```

```csharp
public class ProductViewModel : INotifyPropertyChanged
{
    private double _averageRating = 4.73;
    
    public double AverageRating
    {
        get => _averageRating;
        set
        {
            _averageRating = value;
            OnPropertyChanged();
        }
    }
    
    // INotifyPropertyChanged implementation
}
```

## Best Practices

1. **Match Industry Standards**: Use Half precision for consumer products to match user expectations
2. **Read-Only for Averages**: When displaying calculated averages with Exact precision, set `IsReadOnly="True"`
3. **Consistent Precision**: Use the same precision mode throughout your app for consistency
4. **Clear Labeling**: Show the actual numeric value alongside Exact precision ratings for clarity
5. **Consider Accessibility**: Ensure precision mode choice doesn't hinder accessibility or usability

## Edge Cases

### Rounding Behavior
When setting values programmatically that don't match the precision mode:

```csharp
// Standard: Rounds to nearest integer
standardRating.Value = 3.7;  // Displays 4 stars

// Half: Rounds to nearest 0.5
halfRating.Value = 3.7;  // Displays 3.5 stars

// Exact: No rounding
exactRating.Value = 3.7;  // Displays 3.7 stars
```

### Minimum and Maximum Values
All precision modes respect the ItemCount range:
- Minimum: 0
- Maximum: ItemCount value (default 5)

```csharp
rating.ItemCount = 5;
rating.Value = 7;  // Will be clamped to 5
rating.Value = -1; // Will be clamped to 0
```

## Next Steps

- **Rating Shapes**: Explore different visual shapes → [rating-shapes.md](rating-shapes.md)
- **Appearance Styling**: Customize colors and sizing → [appearance-styling.md](appearance-styling.md)
- **Interactive Features**: Handle events and user interactions → [interactive-features.md](interactive-features.md)
