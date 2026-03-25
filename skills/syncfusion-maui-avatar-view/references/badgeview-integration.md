# BadgeView Integration with Avatar View

The .NET MAUI SfAvatarView integrates seamlessly with SfBadgeView to display notifications, status indicators, and counts on avatars. This guide covers complete BadgeView integration patterns.

## Overview

BadgeView adds contextual information to avatars:
- **Status indicators** - Online, away, busy, offline
- **Notification counts** - Unread messages, alerts
- **Activity badges** - New content, updates
- **Custom indicators** - Any icon or text

## When to Use Badge Integration

- **Messaging apps** - Show online status or unread message counts
- **Social networks** - Indicate new notifications or friend requests
- **Collaboration tools** - Display presence or activity status
- **Contact lists** - Show availability or verification badges
- **Any scenario** requiring contextual avatar information

## Basic BadgeView Setup

### Prerequisites

BadgeView is included in the same `Syncfusion.Maui.Core` package as AvatarView. No additional installation required.

### Namespace Import

```xaml
xmlns:badge="clr-namespace:Syncfusion.Maui.Core;assembly=Syncfusion.Maui.Core"
xmlns:sfavatar="clr-namespace:Syncfusion.Maui.Core;assembly=Syncfusion.Maui.Core"
```

### Basic Structure

The avatar goes inside the `BadgeView.Content`, and badge configuration goes in `BadgeView.BadgeSettings`:

```xaml
<badge:SfBadgeView>
    <badge:SfBadgeView.Content>
        <sfavatar:SfAvatarView ... />
    </badge:SfBadgeView.Content>
    <badge:SfBadgeView.BadgeSettings>
        <badge:BadgeSettings ... />
    </badge:SfBadgeView.BadgeSettings>
</badge:SfBadgeView>
```

## Status Badge Example

Display online/offline status with a colored badge.

**XAML:**
```xaml
<badge:SfBadgeView HorizontalOptions="Center"
                   VerticalOptions="Center">
    <badge:SfBadgeView.Content>
        <sfavatar:SfAvatarView 
            ContentType="Custom"
            ImageSource="user_profile.png"
            WidthRequest="60"
            HeightRequest="60"
            CornerRadius="30"
            Stroke="Black"
            StrokeThickness="1" />
    </badge:SfBadgeView.Content>
    <badge:SfBadgeView.BadgeSettings>
        <badge:BadgeSettings 
            Type="Success"
            Icon="Available"
            Position="BottomRight"
            Offset="-10,-10"
            Animation="Scale" />
    </badge:SfBadgeView.BadgeSettings>
</badge:SfBadgeView>
```

**C#:**
```csharp
using Syncfusion.Maui.Core;

var badgeView = new SfBadgeView
{
    HorizontalOptions = LayoutOptions.Center,
    VerticalOptions = LayoutOptions.Center
};

var avatarView = new SfAvatarView
{
    ContentType = ContentType.Custom,
    ImageSource = "user_profile.png",
    WidthRequest = 60,
    HeightRequest = 60,
    CornerRadius = 30,
    Stroke = Colors.Black,
    StrokeThickness = 1
};

var badgeSettings = new BadgeSettings
{
    Type = BadgeType.Success,
    Icon = BadgeIcon.Available,
    Position = BadgePosition.BottomRight,
    Offset = new Point(-10, -10),
    Animation = BadgeAnimation.Scale
};

badgeView.Content = avatarView;
badgeView.BadgeSettings = badgeSettings;
```

**Result:** Avatar with green "available" indicator at bottom-right.

## Badge Types

BadgeView supports multiple types with predefined colors:

| Type | Color | Common Use |
|------|-------|------------|
| **Success** | Green | Online, available, verified |
| **Warning** | Orange | Away, idle, warning |
| **Error** | Red | Offline, error, busy |
| **Info** | Blue | Information, notifications |
| **Primary** | Purple | Default, custom |
| **Secondary** | Gray | Inactive, secondary status |
| **Light** | Light Gray | Subtle indicators |
| **Dark** | Dark Gray | High contrast |

### Type Examples

**Success (Online):**
```xaml
<badge:BadgeSettings 
    Type="Success"
    Icon="Available"
    Position="BottomRight" />
```

**Warning (Away):**
```xaml
<badge:BadgeSettings 
    Type="Warning"
    Icon="Away"
    Position="BottomRight" />
```

**Error (Busy):**
```xaml
<badge:BadgeSettings 
    Type="Error"
    Icon="Busy"
    Position="BottomRight" />
```

## Badge Icons

Common icons for avatar badges:

- **Available** - Green checkmark or dot (online)
- **Away** - Clock or moon (away)
- **Busy** - Do not disturb symbol
- **Offline** - X or empty (offline)
- **None** - No icon (text or count only)

### Icon Implementation

```xaml
<badge:BadgeSettings 
    Icon="Available"
    Type="Success"
    Position="BottomRight" />
```

```csharp
badgeSettings.Icon = BadgeIcon.Available;
badgeSettings.Type = BadgeType.Success;
```

## Badge Positioning

Position badges at any corner or edge of the avatar:

### Position Options

- **TopLeft**
- **TopRight**
- **BottomLeft**
- **BottomRight** (most common for status)

### Position Examples

**Top Right (Notification Count):**
```xaml
<badge:SfBadgeView HorizontalOptions="Center"
    VerticalOptions="Center" BadgeText="5">
    <badge:SfBadgeView.Content>
        ...
    </badge:SfBadgeView.Content>
    <badge:SfBadgeView.BadgeSettings>
        <badge:BadgeSettings 
            Type="Error"
            Position="TopRight"
            Offset="-5,-5" />
    </badge:SfBadgeView.BadgeSettings>
</badge:SfBadgeView>
```

**Bottom Right (Status):**
```xaml
<badge:BadgeSettings 
    Type="Success"
    Icon="Available"
    Position="BottomRight"
    Offset="-10,-10" />
```

**Bottom Left:**
```xaml
<badge:BadgeSettings 
    Type="Primary"
    Icon="Available"
    Position="BottomLeft"
    Offset="10,-10" />
```

## Badge Offset

The `Offset` property fine-tunes badge placement using X,Y coordinates:

```xaml
<!-- Move badge 10px left and 10px up from corner -->
<badge:BadgeSettings Offset="-10,-10" />

<!-- Move badge 5px right and 5px down from corner -->
<badge:BadgeSettings Offset="5,5" />
```

**Guidelines:**
- Negative X: Move left
- Positive X: Move right
- Negative Y: Move up
- Positive Y: Move down

**Typical offsets:**
- Small avatars (40-60px): `Offset="-5,-5"`
- Medium avatars (60-80px): `Offset="-10,-10"`
- Large avatars (80px+): `Offset="-12,-12"`

## Notification Count Badge

Display unread message or notification counts.

**XAML:**
```xaml
<badge:SfBadgeView HorizontalOptions="Center"
                   VerticalOptions="Center"
                   BadgeText="12">
    <badge:SfBadgeView.Content>
        <sfavatar:SfAvatarView 
            ContentType="Initials"
            AvatarName="John Doe"
            InitialsType="DoubleCharacter"
            AvatarShape="Circle"
            AvatarSize="Large"
            Background="CornflowerBlue"
            InitialsColor="White" />
    </badge:SfBadgeView.Content>
    <badge:SfBadgeView.BadgeSettings>
        <badge:BadgeSettings 
            Type="Error"
            Position="TopRight"
            Offset="-8,-8"
            Animation="None" />
    </badge:SfBadgeView.BadgeSettings>
</badge:SfBadgeView>
```

**C#:**
```csharp
 var badgeView = new SfBadgeView { BadgeText = "12" };

 var avatar = new SfAvatarView
 {
     ContentType = ContentType.Initials,
     AvatarName = "John Doe",
     InitialsType = InitialsType.DoubleCharacter,
     AvatarShape = AvatarShape.Circle,
     AvatarSize = AvatarSize.Large,
     Background = Colors.CornflowerBlue,
     InitialsColor = Colors.White
 };
 var badgeSettings = new BadgeSettings
 {
     Type = BadgeType.Error,
     Position = BadgePosition.TopRight,
     Offset = new Point(-8, -8),
     Animation = BadgeAnimation.None
 };

 badgeView.Content = avatar;
 badgeView.BadgeSettings = badgeSettings;
```

**Dynamic count updates:**
```csharp
public void UpdateNotificationCount(SfBadgeView badgeView, int count)
{
    if (count > 0)
    {
        badgeView.BadgeText = count > 99 ? "99+" : count.ToString();
        badgeView.BadgeSettings.Type = BadgeType.Error;
    }
    else
    {
        badgeView.BadgeText = string.Empty;
    }
}
```

## Badge Animations

Add visual emphasis when badges appear or change:

### Animation Types

- **None** - No animation (instant)
- **Scale** - Badge scales up when appearing (recommended)

### Animation Examples

**Scale (Default):**
```xaml
<badge:BadgeSettings Animation="Scale" />
```

**No Animation:**
```xaml
<badge:BadgeSettings Animation="None" />
```

**Best practices:**
- Use `Scale` for status changes
- Use `None` for static badges

## Common Badge Patterns

### Pattern 1: Online Status Indicator

```csharp
public SfBadgeView CreateOnlineAvatar(string userName, bool isOnline)
{
    var badgeView = new SfBadgeView();
    
    var avatar = new SfAvatarView
    {
        ContentType = ContentType.Initials,
        AvatarName = userName,
        InitialsType = InitialsType.DoubleCharacter,
        AvatarShape = AvatarShape.Circle,
        AvatarSize = AvatarSize.Medium,
        AvatarColorMode = AvatarColorMode.DarkBackground
    };
    
    var badgeSettings = new BadgeSettings
    {
        Type = isOnline ? BadgeType.Success : BadgeType.Secondary,
        Icon = isOnline ? BadgeIcon.Available : BadgeIcon.Away,
        Position = BadgePosition.BottomRight,
        Offset = new Point(-10, -10),
        Animation = BadgeAnimation.Scale
    };
    
    badgeView.Content = avatar;
    badgeView.BadgeSettings = badgeSettings;
    
    return badgeView;
}
```

### Pattern 2: Message Count Badge

```csharp
public SfBadgeView CreateMessageAvatar(string imagePath, int unreadCount)
{
    var badgeView = new SfBadgeView();
    
    var avatar = new SfAvatarView
    {
        ContentType = ContentType.Custom,
        ImageSource = imagePath,
        AvatarShape = AvatarShape.Circle,
        AvatarSize = AvatarSize.Large
    };
    
    var badgeSettings = new BadgeSettings
    {
        Position = BadgePosition.TopRight,
        Offset = new Point(-5, -5)
    };
    
    if (unreadCount > 0)
    {
        badgeSettings.Type = BadgeType.Error;
        badgeView.BadgeText = unreadCount > 99 ? "99+" : unreadCount.ToString();
        badgeSettings.Animation = BadgeAnimation.Scale;
    }
    
    badgeView.Content = avatar;
    badgeView.BadgeSettings = badgeSettings;
    
    return badgeView;
}
```

### Pattern 3: Presence Status (Away/Busy/Available)

```csharp
public enum PresenceStatus
{
    Available,
    Away,
    Busy,
    Offline
}

public SfBadgeView CreatePresenceAvatar(string userName, PresenceStatus status)
{
    var badgeView = new SfBadgeView();
    
    var avatar = new SfAvatarView
    {
        ContentType = ContentType.Initials,
        AvatarName = userName,
        AvatarShape = AvatarShape.Circle,
        AvatarSize = AvatarSize.Medium
    };
    
    var badgeSettings = new BadgeSettings
    {
        Position = BadgePosition.BottomRight,
        Offset = new Point(-10, -10),
        Animation = BadgeAnimation.Scale
    };
    
    switch (status)
    {
        case PresenceStatus.Available:
            badgeSettings.Type = BadgeType.Success;
            badgeSettings.Icon = BadgeIcon.Available;
            break;
        case PresenceStatus.Away:
            badgeSettings.Type = BadgeType.Warning;
            badgeSettings.Icon = BadgeIcon.Away;
            break;
        case PresenceStatus.Busy:
            badgeSettings.Type = BadgeType.Error;
            badgeSettings.Icon = BadgeIcon.Busy;
            break;
        case PresenceStatus.Offline:
            badgeSettings.Type = BadgeType.Secondary;
            badgeSettings.Icon = BadgeIcon.None;
            break;
    }
    
    badgeView.Content = avatar;
    badgeView.BadgeSettings = badgeSettings;
    
    return badgeView;
}
```

### Pattern 4: Verified User Badge

```csharp
public SfBadgeView CreateVerifiedAvatar(string imagePath)
{
    var badgeView = new SfBadgeView();
    
    var avatar = new SfAvatarView
    {
        ContentType = ContentType.Custom,
        ImageSource = imagePath,
        AvatarShape = AvatarShape.Circle,
        AvatarSize = AvatarSize.Large,
        Stroke = Colors.Gold,
        StrokeThickness = 2
    };
    
    var badgeSettings = new BadgeSettings
    {
        Type = BadgeType.Info,
        Icon = BadgeIcon.Busy,  // Use appropriate verification icon
        Position = BadgePosition.BottomRight,
        Offset = new Point(-8, -8),
        Animation = BadgeAnimation.None
    };
    
    badgeView.Content = avatar;
    badgeView.BadgeSettings = badgeSettings;
    
    return badgeView;
}
```

## Complete Example: Chat List Item

```xaml
<ContentPage xmlns:badge="clr-namespace:Syncfusion.Maui.Core;assembly=Syncfusion.Maui.Core"
             xmlns:sfavatar="clr-namespace:Syncfusion.Maui.Core;assembly=Syncfusion.Maui.Core">
    
    <CollectionView ItemsSource="{Binding Contacts}">
        <CollectionView.ItemTemplate>
            <DataTemplate>
                <Grid Padding="15,10" ColumnDefinitions="Auto,*">
                    
                    <!-- Avatar with Badge -->
                    <badge:SfBadgeView Grid.Column="0" BadgeText="{Binding UnreadCount}">
                        <badge:SfBadgeView.Content>
                            <sfavatar:SfAvatarView 
                                ContentType="Custom"
                                ImageSource="{Binding ProfileImage}"
                                AvatarShape="Circle"
                                AvatarSize="Medium" />
                        </badge:SfBadgeView.Content>
                        <badge:SfBadgeView.BadgeSettings>
                            <badge:BadgeSettings 
                                Type="{Binding BadgeType}"
                                Position="TopRight"
                                Offset="-8,-8" />
                        </badge:SfBadgeView.BadgeSettings>
                    </badge:SfBadgeView>
                    
                    <!-- Contact Info -->
                    <VerticalStackLayout Grid.Column="1" Margin="15,0,0,0">
                        <Label Text="{Binding Name}" FontAttributes="Bold" />
                        <Label Text="{Binding LastMessage}" TextColor="Gray" />
                    </VerticalStackLayout>
                    
                </Grid>
            </DataTemplate>
        </CollectionView.ItemTemplate>
    </CollectionView>
    
</ContentPage>
```

## Best Practices

1. **Position consistently** - Use BottomRight for status, TopRight for counts
2. **Keep badges small** - Don't overwhelm the avatar
3. **Use appropriate colors** - Follow badge type conventions
4. **Animate status changes** - Use Scale animation for updates
5. **Handle large counts** - Show "99+" for numbers > 99
6. **Test visibility** - Ensure badges don't obscure important avatar content
7. **Respect theme** - Badge colors should work in light and dark modes

## Troubleshooting

### Issue: Badge Not Visible

**Solutions:**
- Check Offset values (negative values move badge inward)
- Verify BadgeSettings is set on BadgeView
- Ensure badge Type is specified
- Check if BadgeText or Icon is set

### Issue: Badge Positioning Wrong

**Solutions:**
- Adjust Offset property
- Try different Position values
- Account for avatar stroke thickness in offset
- Test on different screen sizes

### Issue: Badge Cuts Off

**Solutions:**
- Add margin/padding to parent container
- Reduce Offset magnitude
- Ensure avatar is not clipped by parent bounds

