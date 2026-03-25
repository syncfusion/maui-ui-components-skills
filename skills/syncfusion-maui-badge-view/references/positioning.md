# Badge Position Customization

Guide to positioning badges around content using the Position and Offset properties.

## Badge Position Property

The `Position` property determines where the badge appears relative to its content. The default position is `TopRight`.

### Available Positions

| Position | Description | Common Use Case |
|----------|-------------|-----------------|
| `TopRight` | Top-right corner (default) | Notification counts, unread messages |
| `TopLeft` | Top-left corner | Status indicators, "new" labels |
| `BottomRight` | Bottom-right corner | Online status, availability |
| `BottomLeft` | Bottom-left corner | Secondary indicators |
| `Top` | Top center | Centered notifications |
| `Bottom` | Bottom center | Centered status |
| `Left` | Left center | Left-aligned badges |
| `Right` | Right center | Right-aligned badges |

## Using Position Property

### TopRight Position (Default)

**XAML:**
```xml
<badge:SfBadgeView BadgeText="5">
    <badge:SfBadgeView.Content>
        <Button Text="Notifications" 
                WidthRequest="120" 
                HeightRequest="60"/>
    </badge:SfBadgeView.Content>
    <badge:SfBadgeView.BadgeSettings>
        <badge:BadgeSettings Position="TopRight"/>
    </badge:SfBadgeView.BadgeSettings>
</badge:SfBadgeView>
```

**C#:**
```csharp
var badgeView = new SfBadgeView
{
    BadgeText = "5",
    BadgeSettings = new BadgeSettings
    {
        Position = BadgePosition.TopRight
    }
};
```

### TopLeft Position

**XAML:**
```xml
<badge:SfBadgeView BadgeText="New">
    <badge:SfBadgeView.Content>
        <Image Source="product.png" 
               WidthRequest="100" 
               HeightRequest="100"/>
    </badge:SfBadgeView.Content>
    <badge:SfBadgeView.BadgeSettings>
        <badge:BadgeSettings Position="TopLeft" 
                            Type="Success"/>
    </badge:SfBadgeView.BadgeSettings>
</badge:SfBadgeView>
```

### BottomRight Position

Common for status indicators:

**XAML:**
```xml
<badge:SfBadgeView>
    <badge:SfBadgeView.Content>
        <Image Source="user_avatar.png" 
               WidthRequest="60" 
               HeightRequest="60"/>
    </badge:SfBadgeView.Content>
    <badge:SfBadgeView.BadgeSettings>
        <badge:BadgeSettings Icon="Available" 
                            Type="Success" 
                            Position="BottomRight"/>
    </badge:SfBadgeView.BadgeSettings>
</badge:SfBadgeView>
```

### BottomLeft Position

**C#:**
```csharp
var badgeSettings = new BadgeSettings
{
    Position = BadgePosition.BottomLeft,
    Type = BadgeType.Warning
};
```

## Complete Position Examples

### All Eight Positions

**XAML:**
```xml
<Grid RowDefinitions="Auto,Auto,Auto" 
      ColumnDefinitions="*,*,*"
      RowSpacing="20"
      ColumnSpacing="20"
      Padding="20">
    
    <!-- TopLeft -->
    <badge:SfBadgeView Grid.Row="0" Grid.Column="0" BadgeText="TL">
        <badge:SfBadgeView.Content>
            <Button Text="TopLeft" WidthRequest="100" HeightRequest="60"/>
        </badge:SfBadgeView.Content>
        <badge:SfBadgeView.BadgeSettings>
            <badge:BadgeSettings Position="TopLeft"/>
        </badge:SfBadgeView.BadgeSettings>
    </badge:SfBadgeView>
    
    <!-- Top -->
    <badge:SfBadgeView Grid.Row="0" Grid.Column="1" BadgeText="T">
        <badge:SfBadgeView.Content>
            <Button Text="Top" WidthRequest="100" HeightRequest="60"/>
        </badge:SfBadgeView.Content>
        <badge:SfBadgeView.BadgeSettings>
            <badge:BadgeSettings Position="Top"/>
        </badge:SfBadgeView.BadgeSettings>
    </badge:SfBadgeView>
    
    <!-- TopRight -->
    <badge:SfBadgeView Grid.Row="0" Grid.Column="2" BadgeText="TR">
        <badge:SfBadgeView.Content>
            <Button Text="TopRight" WidthRequest="100" HeightRequest="60"/>
        </badge:SfBadgeView.Content>
        <badge:SfBadgeView.BadgeSettings>
            <badge:BadgeSettings Position="TopRight"/>
        </badge:SfBadgeView.BadgeSettings>
    </badge:SfBadgeView>
    
    <!-- Left -->
    <badge:SfBadgeView Grid.Row="1" Grid.Column="0" BadgeText="L">
        <badge:SfBadgeView.Content>
            <Button Text="Left" WidthRequest="100" HeightRequest="60"/>
        </badge:SfBadgeView.Content>
        <badge:SfBadgeView.BadgeSettings>
            <badge:BadgeSettings Position="Left"/>
        </badge:SfBadgeView.BadgeSettings>
    </badge:SfBadgeView>
    
    <!-- Center (using BadgeAlignment) -->
    <badge:SfBadgeView Grid.Row="1" Grid.Column="1" BadgeText="C">
        <badge:SfBadgeView.Content>
            <Button Text="Center" WidthRequest="100" HeightRequest="60"/>
        </badge:SfBadgeView.Content>
        <badge:SfBadgeView.BadgeSettings>
            <badge:BadgeSettings BadgeAlignment="Center" CornerRadius="0"/>
        </badge:SfBadgeView.BadgeSettings>
    </badge:SfBadgeView>
    
    <!-- Right -->
    <badge:SfBadgeView Grid.Row="1" Grid.Column="2" BadgeText="R">
        <badge:SfBadgeView.Content>
            <Button Text="Right" WidthRequest="100" HeightRequest="60"/>
        </badge:SfBadgeView.Content>
        <badge:SfBadgeView.BadgeSettings>
            <badge:BadgeSettings Position="Right"/>
        </badge:SfBadgeView.BadgeSettings>
    </badge:SfBadgeView>
    
    <!-- BottomLeft -->
    <badge:SfBadgeView Grid.Row="2" Grid.Column="0" BadgeText="BL">
        <badge:SfBadgeView.Content>
            <Button Text="BottomLeft" WidthRequest="100" HeightRequest="60"/>
        </badge:SfBadgeView.Content>
        <badge:SfBadgeView.BadgeSettings>
            <badge:BadgeSettings Position="BottomLeft"/>
        </badge:SfBadgeView.BadgeSettings>
    </badge:SfBadgeView>
    
    <!-- Bottom -->
    <badge:SfBadgeView Grid.Row="2" Grid.Column="1" BadgeText="B">
        <badge:SfBadgeView.Content>
            <Button Text="Bottom" WidthRequest="100" HeightRequest="60"/>
        </badge:SfBadgeView.Content>
        <badge:SfBadgeView.BadgeSettings>
            <badge:BadgeSettings Position="Bottom"/>
        </badge:SfBadgeView.BadgeSettings>
    </badge:SfBadgeView>
    
    <!-- BottomRight -->
    <badge:SfBadgeView Grid.Row="2" Grid.Column="2" BadgeText="BR">
        <badge:SfBadgeView.Content>
            <Button Text="BottomRight" WidthRequest="100" HeightRequest="60"/>
        </badge:SfBadgeView.Content>
        <badge:SfBadgeView.BadgeSettings>
            <badge:BadgeSettings Position="BottomRight"/>
        </badge:SfBadgeView.BadgeSettings>
    </badge:SfBadgeView>
    
</Grid>
```

## Offset Property

The `Offset` property provides fine-grained control over badge placement using X,Y coordinates.

### How Offset Works

- **X value:** Horizontal adjustment (positive = right, negative = left)
- **Y value:** Vertical adjustment (positive = down, negative = up)
- Values are in device-independent units

### Basic Offset Usage

**XAML:**
```xml
<badge:SfBadgeView BadgeText="8">
    <badge:SfBadgeView.Content>
        <Image Source="icon.png" 
               HeightRequest="70" 
               WidthRequest="60"/>
    </badge:SfBadgeView.Content>
    <badge:SfBadgeView.BadgeSettings>
        <badge:BadgeSettings Type="Success" 
                            Offset="-5,-10" 
                            Position="BottomRight"/>
    </badge:SfBadgeView.BadgeSettings>
</badge:SfBadgeView>
```

**C#:**
```csharp
var badgeSettings = new BadgeSettings
{
    Type = BadgeType.Success,
    Position = BadgePosition.BottomRight,
    Offset = new Point(-5, -10)
};
```

### Offset Examples

**Move badge inward (toward center):**
```csharp
// TopRight - move left and down
badgeSettings.Offset = new Point(-10, 5);

// BottomRight - move left and up
badgeSettings.Offset = new Point(-10, -5);

// TopLeft - move right and down
badgeSettings.Offset = new Point(10, 5);

// BottomLeft - move right and up
badgeSettings.Offset = new Point(10, -5);
```

**Move badge outward (away from center):**
```csharp
// TopRight - move right and up
badgeSettings.Offset = new Point(5, -5);

// BottomRight - move right and down
badgeSettings.Offset = new Point(5, 5);

// TopLeft - move left and up
badgeSettings.Offset = new Point(-5, -5);

// BottomLeft - move left and down
badgeSettings.Offset = new Point(-5, 5);
```

### Practical Offset Scenarios

**Overlap reduction on circular avatars:**
```xml
<badge:SfBadgeView BadgeText="12">
    <badge:SfBadgeView.Content>
        <Frame WidthRequest="60" 
               HeightRequest="60" 
               CornerRadius="30"
               Padding="0">
            <Image Source="avatar.png"/>
        </Frame>
    </badge:SfBadgeView.Content>
    <badge:SfBadgeView.BadgeSettings>
        <badge:BadgeSettings Position="TopRight" 
                            Offset="-8,-8"
                            Type="Error"/>
    </badge:SfBadgeView.BadgeSettings>
</badge:SfBadgeView>
```

**Precise positioning on small icons:**
```csharp
var badgeView = new SfBadgeView
{
    BadgeText = "3",
    Content = new Image 
    { 
        Source = "small_icon.png",
        WidthRequest = 30,
        HeightRequest = 30
    },
    BadgeSettings = new BadgeSettings
    {
        Position = BadgePosition.TopRight,
        Offset = new Point(-5, -5),
        FontSize = 10
    }
};
```

**Status indicator adjustment:**
```xml
<badge:SfBadgeView>
    <badge:SfBadgeView.Content>
        <Image Source="profile.png" 
               WidthRequest="80" 
               HeightRequest="80"/>
    </badge:SfBadgeView.Content>
    <badge:SfBadgeView.BadgeSettings>
        <badge:BadgeSettings Icon="Available" 
                            Position="BottomRight" 
                            Offset="0,-10"
                            Type="Success"/>
    </badge:SfBadgeView.BadgeSettings>
</badge:SfBadgeView>
```

## Position and Offset Combined

Combine Position and Offset for precise control:

**XAML:**
```xml
<badge:SfBadgeView BadgeText="99+">
    <badge:SfBadgeView.Content>
        <Button Text="Messages" 
                WidthRequest="120" 
                HeightRequest="60"
                BackgroundColor="CornflowerBlue"/>
    </badge:SfBadgeView.Content>
    <badge:SfBadgeView.BadgeSettings>
        <badge:BadgeSettings Background="Red"
                            Position="TopRight"
                            Offset="-10,5"/>
    </badge:SfBadgeView.BadgeSettings>
</badge:SfBadgeView>
```

**C#:**
```csharp
var badgeView = new SfBadgeView
{
    BadgeText = "99+",
    BadgeSettings = new BadgeSettings
    {
        Background = new SolidColorBrush(Colors.Red),
        Position = BadgePosition.TopRight,
        Offset = new Point(-10, 5)
    }
};
```

## Real-World Positioning Patterns

### E-Commerce Product Badge

```xml
<badge:SfBadgeView BadgeText="Sale">
    <badge:SfBadgeView.Content>
        <Image Source="product.png" 
               WidthRequest="150" 
               HeightRequest="150"/>
    </badge:SfBadgeView.Content>
    <badge:SfBadgeView.BadgeSettings>
        <badge:BadgeSettings Type="Error" 
                            Position="TopLeft" 
                            Offset="10,10"/>
    </badge:SfBadgeView.BadgeSettings>
</badge:SfBadgeView>
```

### Chat Avatar with Status

```csharp
var chatBadge = new SfBadgeView
{
    Content = new Frame
    {
        WidthRequest = 50,
        HeightRequest = 50,
        CornerRadius = 25,
        Padding = 0,
        Content = new Image { Source = "chat_avatar.png" }
    },
    BadgeSettings = new BadgeSettings
    {
        Icon = BadgeIcon.Available,
        Position = BadgePosition.BottomRight,
        Offset = new Point(2, 2),
        Type = BadgeType.Success
    }
};
```

### Notification Bell

```xml
<badge:SfBadgeView BadgeText="15">
    <badge:SfBadgeView.Content>
        <Image Source="bell_icon.png" 
               WidthRequest="40" 
               HeightRequest="40"/>
    </badge:SfBadgeView.Content>
    <badge:SfBadgeView.BadgeSettings>
        <badge:BadgeSettings Type="Error" 
                            Position="TopRight" 
                            Offset="-5,-5"
                            FontSize="11"/>
    </badge:SfBadgeView.BadgeSettings>
</badge:SfBadgeView>
```

### Tab Bar Item

```csharp
var tabBadge = new SfBadgeView
{
    BadgeText = "3",
    Content = new VerticalStackLayout
    {
        Spacing = 5,
        Children =
        {
            new Image 
            { 
                Source = "tab_icon.png",
                WidthRequest = 24,
                HeightRequest = 24
            },
            new Label 
            { 
                Text = "Messages",
                FontSize = 10,
                HorizontalTextAlignment = TextAlignment.Center
            }
        }
    },
    BadgeSettings = new BadgeSettings
    {
        Type = BadgeType.Error,
        Position = BadgePosition.TopRight,
        Offset = new Point(-15, 0),
        FontSize = 10
    }
};
```

## Troubleshooting

### Badge Not at Expected Position

**Problem:** Badge appears in wrong location.

**Solutions:**
1. Verify Position property is set correctly
2. Check if content has explicit size set
3. Try adjusting Offset to fine-tune
4. Ensure HorizontalOptions and VerticalOptions on SfBadgeView are appropriate

### Offset Not Working

**Problem:** Offset values don't seem to affect badge position.

**Solutions:**
1. Ensure Position property is set (Offset works relative to Position)
2. Check Offset syntax: `new Point(x, y)` or `"x,y"` in XAML
3. Verify values are not too small to notice
4. Try larger values to see if Offset is working at all

### Badge Overlapping Content

**Problem:** Badge covers important part of content.

**Solutions:**
1. Use negative Offset values to move badge outward
2. Choose different Position (e.g., switch from TopRight to TopLeft)
3. Increase content size to provide more space
4. Reduce badge size with smaller FontSize

### Badge Cut Off at Edge

**Problem:** Badge is partially clipped at screen or container edge.

**Solutions:**
1. Add Padding to parent container
2. Use Offset to move badge inward
3. Ensure SfBadgeView has enough space in layout
4. Check if parent container has ClipToBounds set

### Different Positions on Different Content Sizes

**Problem:** Badge position varies when content size changes.

**Solutions:**
1. Set consistent WidthRequest/HeightRequest on content
2. Use fixed Offset values for consistency
3. Consider using BadgeAlignment instead of Position for certain scenarios
4. Test with min/max content sizes to find optimal Offset

## Best Practices

1. **Use Position for general placement:** Start with Position, then fine-tune with Offset
2. **Consistent positioning:** Use same Position across similar badges in your app
3. **Offset for adjustments:** Use Offset for pixel-perfect positioning
4. **Test on different sizes:** Verify badge position works with various content sizes
5. **Consider touch targets:** Ensure badge doesn't interfere with button tap areas
6. **Account for RTL:** Test positioning in right-to-left layouts if supporting RTL languages
