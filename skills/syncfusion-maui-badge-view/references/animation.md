# Badge Animation

Guide to enabling and customizing badge animations in .NET MAUI Badge View.

## Animation Overview

Badge animations provide visual feedback when badge content changes. The Badge View supports scale animation that draws attention to badge updates.

**When animations trigger:**
- BadgeText property changes
- Badge becomes visible
- Badge icon changes

**Animation does NOT trigger:**
- Initial badge load
- Badge settings changes (color, position, etc.)
- When animation is set to None

## Enable Animation

Use the `Animation` property in `BadgeSettings` to enable or disable animations.

### Animation Values

| Value | Description | When to Use |
|-------|-------------|-------------|
| `Scale` | Badge scales up then returns to normal size | Dynamic notifications, real-time updates |
| `None` | No animation (default) | Static badges, performance-critical scenarios |

### Basic Animation Setup

**XAML:**
```xml
<badge:SfBadgeView BadgeText="6">
    <badge:SfBadgeView.Content>
        <Image Source="notification_icon.png" 
               HeightRequest="70" 
               WidthRequest="70"/>
    </badge:SfBadgeView.Content>
    <badge:SfBadgeView.BadgeSettings>
        <badge:BadgeSettings Type="Error" 
                            Animation="Scale"/>
    </badge:SfBadgeView.BadgeSettings>
</badge:SfBadgeView>
```

**C#:**
```csharp
var badgeView = new SfBadgeView
{
    BadgeText = "6",
    BadgeSettings = new BadgeSettings
    {
        Type = BadgeType.Error,
        Animation = BadgeAnimation.Scale
    }
};
```

### Disable Animation

**XAML:**
```xml
<badge:SfBadgeView BadgeText="10">
    <badge:SfBadgeView.Content>
        <Button Text="Messages"/>
    </badge:SfBadgeView.Content>
    <badge:SfBadgeView.BadgeSettings>
        <badge:BadgeSettings Animation="None"/>
    </badge:SfBadgeView.BadgeSettings>
</badge:SfBadgeView>
```

**C#:**
```csharp
var badgeSettings = new BadgeSettings
{
    Animation = BadgeAnimation.None
};
```

## Animation Duration

Control animation speed using the `AnimationDuration` property (measured in milliseconds).

### Default Duration

Default value: **250 milliseconds**

### Setting Custom Duration

**XAML:**
```xml
<badge:SfBadgeView BadgeText="6">
    <badge:SfBadgeView.Content>
        <Image Source="icon.png" 
               HeightRequest="70" 
               WidthRequest="70"/>
    </badge:SfBadgeView.Content>
    <badge:SfBadgeView.BadgeSettings>
        <badge:BadgeSettings Animation="Scale" 
                            AnimationDuration="600"/>
    </badge:SfBadgeView.BadgeSettings>
</badge:SfBadgeView>
```

**C#:**
```csharp
var badgeSettings = new BadgeSettings
{
    Animation = BadgeAnimation.Scale,
    AnimationDuration = 600 // milliseconds
};
```

### Duration Guidelines

| Duration (ms) | Speed | When to Use |
|---------------|-------|-------------|
| 100-150 | Very Fast | Subtle notifications |
| 200-300 | Fast (default) | Standard notifications |
| 400-600 | Moderate | Important updates |
| 700-1000 | Slow | Critical alerts |

**Common duration values:**
- Quick updates: `150ms`
- Standard: `250ms` (default)
- Emphasized: `400ms`
- Very noticeable: `600ms`

## Practical Animation Examples

### Real-Time Message Counter

Animate badge when new messages arrive:

**XAML:**
```xml
<badge:SfBadgeView x:Name="messageBadge" 
                   BadgeText="0">
    <badge:SfBadgeView.Content>
        <Button Text="Messages" 
                Clicked="OnMessagesClicked"
                WidthRequest="120" 
                HeightRequest="60"/>
    </badge:SfBadgeView.Content>
    <badge:SfBadgeView.BadgeSettings>
        <badge:BadgeSettings Type="Primary" 
                            Animation="Scale" 
                            AnimationDuration="300"/>
    </badge:SfBadgeView.BadgeSettings>
</badge:SfBadgeView>
```

**C# Code-Behind:**
```csharp
private int _messageCount = 0;

public void OnNewMessageReceived()
{
    _messageCount++;
    messageBadge.BadgeText = _messageCount.ToString();
    // Animation triggers automatically on BadgeText change
}
```

### Notification Bell with Animation

**C#:**
```csharp
public class NotificationView : ContentView
{
    private SfBadgeView _badgeView;
    private int _notificationCount = 0;
    
    public NotificationView()
    {
        _badgeView = new SfBadgeView
        {
            BadgeText = "0",
            Content = new Image
            {
                Source = "bell_icon.png",
                WidthRequest = 50,
                HeightRequest = 50
            },
            BadgeSettings = new BadgeSettings
            {
                Type = BadgeType.Error,
                Animation = BadgeAnimation.Scale,
                AnimationDuration = 400,
                Position = BadgePosition.TopRight,
                Offset = new Point(-8, -8)
            }
        };
        
        Content = _badgeView;
    }
    
    public void AddNotification()
    {
        _notificationCount++;
        _badgeView.BadgeText = _notificationCount.ToString();
        // Badge animates on text change
    }
    
    public void ClearNotifications()
    {
        _notificationCount = 0;
        _badgeView.BadgeText = "0";
    }
}
```

### Shopping Cart Badge

**XAML:**
```xml
<badge:SfBadgeView x:Name="cartBadge" 
                   BadgeText="0">
    <badge:SfBadgeView.Content>
        <ImageButton Source="cart_icon.png"
                     WidthRequest="40"
                     HeightRequest="40"
                     Clicked="OnCartClicked"/>
    </badge:SfBadgeView.Content>
    <badge:SfBadgeView.BadgeSettings>
        <badge:BadgeSettings Type="Success" 
                            Animation="Scale" 
                            AnimationDuration="250"
                            AutoHide="True"/>
    </badge:SfBadgeView.BadgeSettings>
</badge:SfBadgeView>
```

**C#:**
```csharp
private int _cartItems = 0;

public void AddToCart()
{
    _cartItems++;
    cartBadge.BadgeText = _cartItems.ToString();
    // Animation plays when count increases
}

public void RemoveFromCart()
{
    if (_cartItems > 0)
    {
        _cartItems--;
        cartBadge.BadgeText = _cartItems.ToString();
        // Animation plays when count decreases
        // Badge auto-hides when reaching 0
    }
}
```

### Dynamic Status Updates

**C#:**
```csharp
public class UserStatusBadge
{
    private SfBadgeView _statusBadge;
    
    public UserStatusBadge(Image userAvatar)
    {
        _statusBadge = new SfBadgeView
        {
            Content = userAvatar,
            BadgeSettings = new BadgeSettings
            {
                Position = BadgePosition.BottomRight,
                Animation = BadgeAnimation.Scale,
                AnimationDuration = 500,
                Type = BadgeType.Success
            }
        };
    }
    
    public void UpdateStatus(UserStatus status)
    {
        switch (status)
        {
            case UserStatus.Online:
                _statusBadge.BadgeSettings.Icon = BadgeIcon.Available;
                _statusBadge.BadgeSettings.Type = BadgeType.Success;
                break;
            case UserStatus.Away:
                _statusBadge.BadgeSettings.Icon = BadgeIcon.Away;
                _statusBadge.BadgeSettings.Type = BadgeType.Warning;
                break;
            case UserStatus.Busy:
                _statusBadge.BadgeSettings.Icon = BadgeIcon.Busy;
                _statusBadge.BadgeSettings.Type = BadgeType.Error;
                break;
        }
        // Animation triggers when icon changes
    }
}

public enum UserStatus
{
    Online,
    Away,
    Busy
}
```

## Animation with Other Features

### Animation + Position

```xml
<badge:SfBadgeView BadgeText="5">
    <badge:SfBadgeView.Content>
        <Button Text="Alerts"/>
    </badge:SfBadgeView.Content>
    <badge:SfBadgeView.BadgeSettings>
        <badge:BadgeSettings Animation="Scale"
                            AnimationDuration="300"
                            Position="TopRight"
                            Offset="-5,5"
                            Type="Warning"/>
    </badge:SfBadgeView.BadgeSettings>
</badge:SfBadgeView>
```

### Animation + Custom Colors

```csharp
var animatedBadge = new BadgeSettings
{
    Animation = BadgeAnimation.Scale,
    AnimationDuration = 400,
    Type = BadgeType.None,
    Background = new SolidColorBrush(Colors.Purple),
    TextColor = Colors.White,
    FontSize = 14,
    FontAttributes = FontAttributes.Bold
};
```

### Animation + AutoHide

```xml
<badge:SfBadgeView BadgeText="3">
    <badge:SfBadgeView.Content>
        <Button Text="Tasks"/>
    </badge:SfBadgeView.Content>
    <badge:SfBadgeView.BadgeSettings>
        <badge:BadgeSettings Animation="Scale"
                            AnimationDuration="350"
                            AutoHide="True"
                            Type="Info"/>
    </badge:SfBadgeView.BadgeSettings>
</badge:SfBadgeView>
```

## Animation Performance Considerations

### When to Use Animation

**✅ Good use cases:**
- Real-time notification updates
- User action feedback (add to cart, new message)
- Status changes
- Count increments/decrements
- Attention-grabbing for important updates

**❌ Avoid animation when:**
- Displaying static badges
- Many badges animating simultaneously
- Low-performance devices
- Battery-constrained scenarios
- Accessibility concerns (some users sensitive to motion)

### Performance Tips

1. **Limit simultaneous animations:** Avoid animating many badges at once
2. **Use appropriate duration:** Shorter durations (200-300ms) perform better
3. **Disable when not needed:** Set `Animation="None"` for static badges
4. **Test on real devices:** Verify animation performance on target devices

## Troubleshooting

### Animation Not Playing

**Problem:** Badge doesn't animate when BadgeText changes.

**Solutions:**
1. Verify `Animation="Scale"` is set
2. Ensure BadgeText is actually changing (not set to same value)
3. Check that badge is visible (`IsVisible="True"`)
4. Confirm AnimationDuration is not 0

### Animation Too Fast/Slow

**Problem:** Animation speed doesn't feel right.

**Solutions:**
1. Adjust `AnimationDuration` property
2. Try values between 200-600ms for best results
3. Test on actual devices (not just emulator)
4. Consider user preferences for motion

### Animation Causing Performance Issues

**Problem:** App lags when badge animates.

**Solutions:**
1. Reduce AnimationDuration (try 150-200ms)
2. Limit number of animated badges on screen
3. Use `Animation="None"` on non-critical badges
4. Profile app to identify bottlenecks

### Animation Not Noticeable

**Problem:** Users don't notice badge updates.

**Solutions:**
1. Increase AnimationDuration (try 400-600ms)
2. Combine with color change (Type property)
3. Use Error or Warning type for attention
4. Consider additional visual cues (color flash)

## Best Practices

1. **Use animation for dynamic content:** Enable Scale animation for badges that change frequently
2. **Choose appropriate duration:** 250-400ms works well for most cases
3. **Disable for static badges:** Use `None` for badges that rarely change
4. **Test user experience:** Verify animation feels natural and not distracting
5. **Consider accessibility:** Some users may be sensitive to motion
6. **Combine with other cues:** Use animation alongside color/type changes
7. **Optimize performance:** Limit simultaneous animations in lists or grids
8. **Update responsively:** Ensure badge text updates reflect actual data changes
