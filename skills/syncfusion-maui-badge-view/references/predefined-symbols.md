# Predefined Symbols and Icons

Guide to using predefined badge icons for status indicators and symbols in Badge View.

## Badge Icons Overview

Badge View supports predefined icons as an alternative to text badges. Icons are useful for:
- Status indicators (online, away, busy)
- Action symbols (add, delete, prohibit)
- Visual indicators without text

**Important:** When both `Icon` and `BadgeText` are set, `BadgeText` takes priority and the icon is not displayed.

## Available Badge Icons

| Icon | Description | Common Use Case |
|------|-------------|-----------------|
| `Add` | Plus/add symbol | Add to favorites, add contact |
| `Available` | Available/online status | User online status |
| `Away` | Away status | User away from keyboard |
| `Busy` | Busy/do not disturb | User busy status |
| `Delete` | Delete/remove symbol | Remove item, clear notification |
| `Dot` | Simple dot indicator | Unread indicator, presence |
| `None` | No icon (default) | Use when displaying text only |
| `Prohibit1` | Prohibition symbol (style 1) | Blocked, restricted access |
| `Prohibit2` | Prohibition symbol (style 2) | Alternative prohibition style |

## Using Badge Icons

### Basic Icon Usage

**XAML:**
```xml
<badge:SfBadgeView>
    <badge:SfBadgeView.Content>
        <Image Source="user_avatar.png" 
               HeightRequest="70" 
               WidthRequest="60"/>
    </badge:SfBadgeView.Content>
    <badge:SfBadgeView.BadgeSettings>
        <badge:BadgeSettings Type="Warning" 
                            Position="BottomRight" 
                            Icon="Away"/>
    </badge:SfBadgeView.BadgeSettings>
</badge:SfBadgeView>
```

**C#:**
```csharp
var badgeView = new SfBadgeView
{
    Content = new Image
    {
        Source = "user_avatar.png",
        HeightRequest = 70,
        WidthRequest = 60
    },
    BadgeSettings = new BadgeSettings
    {
        Type = BadgeType.Warning,
        Position = BadgePosition.BottomRight,
        Icon = BadgeIcon.Away
    }
};
```

## Icon Examples by Type

### Available (Online Status)

**XAML:**
```xml
<badge:SfBadgeView>
    <badge:SfBadgeView.Content>
        <Frame WidthRequest="60" 
               HeightRequest="60" 
               CornerRadius="30"
               Padding="0">
            <Image Source="avatar.png"/>
        </Frame>
    </badge:SfBadgeView.Content>
    <badge:SfBadgeView.BadgeSettings>
        <badge:BadgeSettings Icon="Available" 
                            Type="Success" 
                            Position="BottomRight"
                            Offset="2,2"/>
    </badge:SfBadgeView.BadgeSettings>
</badge:SfBadgeView>
```

**Common with:** `Type="Success"` (green), `Position="BottomRight"`

### Away Status

**C#:**
```csharp
var awayBadge = new SfBadgeView
{
    Content = CreateUserAvatar(),
    BadgeSettings = new BadgeSettings
    {
        Icon = BadgeIcon.Away,
        Type = BadgeType.Warning,
        Position = BadgePosition.BottomRight
    }
};
```

**Common with:** `Type="Warning"` (orange/yellow), `Position="BottomRight"`

### Busy Status

**XAML:**
```xml
<badge:SfBadgeView>
    <badge:SfBadgeView.Content>
        <Image Source="profile.png" 
               WidthRequest="50" 
               HeightRequest="50"/>
    </badge:SfBadgeView.Content>
    <badge:SfBadgeView.BadgeSettings>
        <badge:BadgeSettings Icon="Busy" 
                            Type="Error" 
                            Position="BottomRight"/>
    </badge:SfBadgeView.BadgeSettings>
</badge:SfBadgeView>
```

**Common with:** `Type="Error"` (red), `Position="BottomRight"`

### Add Icon

**C#:**
```csharp
var addBadge = new SfBadgeView
{
    Content = new Image
    {
        Source = "contact_icon.png",
        WidthRequest = 60,
        HeightRequest = 60
    },
    BadgeSettings = new BadgeSettings
    {
        Icon = BadgeIcon.Add,
        Type = BadgeType.Primary,
        Position = BadgePosition.TopRight
    }
};
```

**Common with:** `Type="Primary"` or `Type="Success"`, `Position="TopRight"`

### Delete Icon

**XAML:**
```xml
<badge:SfBadgeView>
    <badge:SfBadgeView.Content>
        <Button Text="Remove Item" 
                WidthRequest="100" 
                HeightRequest="50"/>
    </badge:SfBadgeView.Content>
    <badge:SfBadgeView.BadgeSettings>
        <badge:BadgeSettings Icon="Delete" 
                            Type="Error" 
                            Position="TopRight"/>
    </badge:SfBadgeView.BadgeSettings>
</badge:SfBadgeView>
```

**Common with:** `Type="Error"` (red), `Position="TopRight"` or `TopLeft`

### Dot Indicator

**C#:**
```csharp
var dotBadge = new SfBadgeView
{
    Content = new Label { Text = "New Items" },
    BadgeSettings = new BadgeSettings
    {
        Icon = BadgeIcon.Dot,
        Type = BadgeType.Info,
        Position = BadgePosition.TopRight,
        Offset = new Point(-5, 5)
    }
};
```

**Common with:** Any type, minimal space needed, `Position="TopRight"`

### Prohibit Icons

**XAML:**
```xml
<!-- Prohibit Style 1 -->
<badge:SfBadgeView>
    <badge:SfBadgeView.Content>
        <Image Source="restricted_content.png"/>
    </badge:SfBadgeView.Content>
    <badge:SfBadgeView.BadgeSettings>
        <badge:BadgeSettings Icon="Prohibit1" 
                            Type="Error" 
                            Position="Top"/>
    </badge:SfBadgeView.BadgeSettings>
</badge:SfBadgeView>

<!-- Prohibit Style 2 -->
<badge:SfBadgeView>
    <badge:SfBadgeView.Content>
        <Image Source="blocked_user.png"/>
    </badge:SfBadgeView.Content>
    <badge:SfBadgeView.BadgeSettings>
        <badge:BadgeSettings Icon="Prohibit2" 
                            Type="Dark" 
                            Position="Top"/>
    </badge:SfBadgeView.BadgeSettings>
</badge:SfBadgeView>
```

**Common with:** `Type="Error"` or `Type="Dark"`, `Position="Top"`

## Icon vs Text Priority

### Text Takes Priority

When both `BadgeText` and `Icon` are set, only text is displayed:

**XAML:**
```xml
<!-- Only "5" will be displayed, not the Away icon -->
<badge:SfBadgeView BadgeText="5">
    <badge:SfBadgeView.Content>
        <Image Source="avatar.png"/>
    </badge:SfBadgeView.Content>
    <badge:SfBadgeView.BadgeSettings>
        <badge:BadgeSettings Icon="Away" Type="Warning"/>
    </badge:SfBadgeView.BadgeSettings>
</badge:SfBadgeView>
```

### Display Icon Only

To display icon, ensure `BadgeText` is not set or is empty/null:

**C#:**
```csharp
var iconBadge = new SfBadgeView
{
    // BadgeText is not set - icon will display
    Content = new Image { Source = "user.png" },
    BadgeSettings = new BadgeSettings
    {
        Icon = BadgeIcon.Available,
        Type = BadgeType.Success
    }
};
```

### Switch Between Text and Icon

**C#:**
```csharp
public class NotificationBadge
{
    private SfBadgeView _badge;
    
    public void ShowCount(int count)
    {
        // Show text - icon will be hidden
        _badge.BadgeText = count.ToString();
    }
    
    public void ShowIcon()
    {
        // Clear text to show icon
        _badge.BadgeText = string.Empty;
        _badge.BadgeSettings.Icon = BadgeIcon.Dot;
    }
}
```

## Real-World Icon Patterns

### User Status Indicators

**C#:**
```csharp
public class UserStatusView : ContentView
{
    private SfBadgeView _statusBadge;
    
    public UserStatusView(string userImagePath)
    {
        _statusBadge = new SfBadgeView
        {
            Content = new Frame
            {
                WidthRequest = 60,
                HeightRequest = 60,
                CornerRadius = 30,
                Padding = 0,
                Content = new Image { Source = userImagePath }
            },
            BadgeSettings = new BadgeSettings
            {
                Position = BadgePosition.BottomRight,
                Offset = new Point(0, 0)
            }
        };
        
        Content = _statusBadge;
        SetStatus(UserStatus.Offline);
    }
    
    public void SetStatus(UserStatus status)
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
            case UserStatus.Offline:
                _statusBadge.BadgeSettings.Icon = BadgeIcon.None;
                _statusBadge.BadgeSettings.IsVisible = false;
                break;
        }
    }
}

public enum UserStatus
{
    Online,
    Away,
    Busy,
    Offline
}
```

### Contact List with Status

**XAML:**
```xml
<CollectionView ItemsSource="{Binding Contacts}">
    <CollectionView.ItemTemplate>
        <DataTemplate>
            <Grid ColumnDefinitions="60,*" 
                  Padding="10"
                  ColumnSpacing="15">
                
                <badge:SfBadgeView Grid.Column="0">
                    <badge:SfBadgeView.Content>
                        <Image Source="{Binding AvatarPath}" 
                               WidthRequest="50" 
                               HeightRequest="50"/>
                    </badge:SfBadgeView.Content>
                    <badge:SfBadgeView.BadgeSettings>
                        <badge:BadgeSettings Icon="{Binding StatusIcon}" 
                                            Type="{Binding StatusType}" 
                                            Position="BottomRight"/>
                    </badge:SfBadgeView.BadgeSettings>
                </badge:SfBadgeView>
                
                <VerticalStackLayout Grid.Column="1" VerticalOptions="Center">
                    <Label Text="{Binding Name}" FontSize="16" FontAttributes="Bold"/>
                    <Label Text="{Binding StatusText}" FontSize="12" TextColor="Gray"/>
                </VerticalStackLayout>
                
            </Grid>
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

### Action Badges on Cards

**C#:**
```csharp
public Frame CreateProductCard(Product product)
{
    var card = new Frame
    {
        CornerRadius = 10,
        Padding = 10,
        Content = new VerticalStackLayout
        {
            Spacing = 10
        }
    };
    
    // Product image with add/remove badge
    var imageBadge = new SfBadgeView
    {
        Content = new Image
        {
            Source = product.ImagePath,
            WidthRequest = 150,
            HeightRequest = 150,
            Aspect = Aspect.AspectFill
        },
        BadgeSettings = new BadgeSettings
        {
            Icon = product.IsInCart ? BadgeIcon.Delete : BadgeIcon.Add,
            Type = product.IsInCart ? BadgeType.Error : BadgeType.Success,
            Position = BadgePosition.TopRight,
            Offset = new Point(-10, 10)
        }
    };
    
    var tapGesture = new TapGestureRecognizer();
    tapGesture.Tapped += (s, e) => ToggleCart(product, imageBadge);
    imageBadge.GestureRecognizers.Add(tapGesture);
    
    (card.Content as VerticalStackLayout).Children.Add(imageBadge);
    
    return card;
}
```

## Icon with Custom Styling

### Icon with Custom Colors

**XAML:**
```xml
<badge:SfBadgeView>
    <badge:SfBadgeView.Content>
        <Image Source="user.png" WidthRequest="60" HeightRequest="60"/>
    </badge:SfBadgeView.Content>
    <badge:SfBadgeView.BadgeSettings>
        <badge:BadgeSettings Icon="Available"
                            Type="None"
                            Background="LimeGreen"
                            Position="BottomRight"/>
    </badge:SfBadgeView.BadgeSettings>
</badge:SfBadgeView>
```

### Icon with Animation

**C#:**
```csharp
var animatedIconBadge = new BadgeSettings
{
    Icon = BadgeIcon.Dot,
    Type = BadgeType.Error,
    Animation = BadgeAnimation.Scale,
    AnimationDuration = 400,
    Position = BadgePosition.TopRight
};
```

### Icon with Stroke

**XAML:**
```xml
<badge:SfBadgeView>
    <badge:SfBadgeView.Content>
        <Image Source="profile.png"/>
    </badge:SfBadgeView.Content>
    <badge:SfBadgeView.BadgeSettings>
        <badge:BadgeSettings Icon="Busy"
                            Type="Error"
                            Stroke="White"
                            StrokeThickness="2"
                            Position="BottomRight"/>
    </badge:SfBadgeView.BadgeSettings>
</badge:SfBadgeView>
```

## Troubleshooting

### Icon Not Visible

**Problem:** Icon doesn't appear on badge.

**Solutions:**
1. Verify `BadgeText` is not set (text takes priority over icon)
2. Ensure `Icon` property is set to value other than `None`
3. Check badge `IsVisible` is true
4. Verify badge has sufficient space to render

### Wrong Icon Displayed

**Problem:** Different icon than expected appears.

**Solutions:**
1. Check `Icon` property value
2. Verify no code is overriding icon elsewhere
3. Ensure using correct `BadgeIcon` enum value

### Icon Appears with Text

**Problem:** Both icon and text showing (not possible).

**Solution:** 
- This is not possible - text always takes priority
- If seeing both, likely two separate badge views
- Check your view hierarchy

### Icon Too Small/Large

**Problem:** Icon size doesn't look right.

**Solutions:**
1. Icons size based on badge size - adjust content size
2. Use `Offset` to position better
3. Consider using text badge instead for size control
4. Adjust parent container constraints

## Best Practices

1. **Use icons for status:** Icons work well for user status (online, away, busy)
2. **Text for counts:** Use text badges for numbers and counts
3. **Consistent icon usage:** Use same icons for same meanings throughout app
4. **Appropriate colors:** Match icon type with color (Available=Success, Busy=Error)
5. **Clear icon priority:** Remember text overrides icon
6. **Position properly:** Status icons typically go BottomRight on avatars
7. **Test visibility:** Ensure icons are visible against content backgrounds
8. **Document icon meanings:** Make icon purposes clear to users
