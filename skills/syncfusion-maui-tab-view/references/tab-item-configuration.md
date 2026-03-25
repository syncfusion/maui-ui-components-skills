# Tab Item Configuration

## Table of Contents
- [Header Content Configuration](#header-content-configuration)
- [Icon and Image Support](#icon-and-image-support)
- [Font and Text Styling](#font-and-text-styling)
- [Badge and Notification Support](#badge-and-notification-support)
- [Content Configuration](#content-configuration)
- [Disabled State](#disabled-state)
- [Complete Examples](#complete-examples)

---

Comprehensive guide for configuring individual tab items including headers, icons, text styling, and content in .NET MAUI Tab View.

## Header Content Configuration

### Basic Text Header

Set simple text headers using the `Header` property:

**XAML:**
```xaml
<tabView:SfTabItem Header="Home" />
<tabView:SfTabItem Header="Profile" />
<tabView:SfTabItem Header="Settings" />
```

**C#:**
```csharp
var homeTab = new SfTabItem { Header = "Home" };
var profileTab = new SfTabItem { Header = "Profile" };
var settingsTab = new SfTabItem { Header = "Settings" };
```

### Dynamic Header Updates

Update headers programmatically:

```csharp
// Access tab by index
tabView.Items[0].Header = "Updated Home";

// Update with binding
tabItem.SetBinding(SfTabItem.HeaderProperty, "TabTitle");
```

### Header Padding

Control spacing inside tab headers:

**XAML:**
```xaml
<tabView:SfTabView TabHeaderPadding="20,10,20,10" />
```

**C#:**
```csharp
tabView.TabHeaderPadding = new Thickness(20, 10, 20, 10);
```

## Icon and Image Support

### Adding Icons to Headers

Use the `ImageSource` property to add icons:

**XAML:**
```xaml
<tabView:SfTabView>
    <tabView:SfTabView.Items>
        <tabView:SfTabItem Header="Home" 
                           ImageSource="home.png" />
        <tabView:SfTabItem Header="Profile" 
                           ImageSource="user.png" />
        <tabView:SfTabItem Header="Settings" 
                           ImageSource="settings.png" />
    </tabView:SfTabView.Items>
</tabView:SfTabView>
```

**C#:**
```csharp
var homeTab = new SfTabItem
{
    Header = "Home",
    ImageSource = "home.png"
};
```

### ImagePosition Options

Control where the icon appears relative to text:

**Options:**
- `Left`: Icon to the left of text (default)
- `Top`: Icon above text
- `Right`: Icon to the right of text
- `Bottom`: Icon below text

**XAML Examples:**

```xaml
<!-- Icon on left -->
<tabView:SfTabItem Header="Home" 
                   ImageSource="home.png" 
                   ImagePosition="Left" />

<!-- Icon on top (common for mobile) -->
<tabView:SfTabItem Header="Home" 
                   ImageSource="home.png" 
                   ImagePosition="Top" />

<!-- Icon on right -->
<tabView:SfTabItem Header="Home" 
                   ImageSource="home.png" 
                   ImagePosition="Right" />

<!-- Icon on bottom -->
<tabView:SfTabItem Header="Home" 
                   ImageSource="home.png" 
                   ImagePosition="Bottom" />
```

**C#:**
```csharp
// Top position (vertical layout)
var tab = new SfTabItem
{
    Header = "Home",
    ImageSource = "home.png",
    ImagePosition = TabImagePosition.Top
};

// Left position (horizontal layout)
var tab2 = new SfTabItem
{
    Header = "Profile",
    ImageSource = "user.png",
    ImagePosition = TabImagePosition.Left
};
```

**⚠️ TabBarHeight Recommendation:**
- `ImagePosition="Left"` or `"Right"`: TabBarHeight = 48-56
- `ImagePosition="Top"` or `"Bottom"`: TabBarHeight = 72

### Icon-Only Tabs

Create tabs with only icons (no text):

**XAML:**
```xaml
<tabView:SfTabView TabBarHeight="56">
    <tabView:SfTabView.Items>
        <tabView:SfTabItem ImageSource="home.png" />
        <tabView:SfTabItem ImageSource="search.png" />
        <tabView:SfTabItem ImageSource="notifications.png" />
        <tabView:SfTabItem ImageSource="profile.png" />
    </tabView:SfTabView.Items>
</tabView:SfTabView>
```

## Font and Text Styling

### FontSize

Control text size in tab headers:

**XAML:**
```xaml
<tabView:SfTabItem Header="Home" FontSize="16" />
<tabView:SfTabItem Header="Large Text" FontSize="20" />
```

**C#:**
```csharp
tabItem.FontSize = 16;
```

### FontFamily

Apply custom fonts:

**XAML:**
```xaml
<tabView:SfTabItem Header="Home" 
                   FontFamily="OpenSans-Bold" />
```

**C#:**
```csharp
tabItem.FontFamily = "OpenSans-Bold";
```

### FontAttributes

Apply bold or italic styles:

**XAML:**
```xaml
<tabView:SfTabItem Header="Bold" FontAttributes="Bold" />
<tabView:SfTabItem Header="Italic" FontAttributes="Italic" />
<tabView:SfTabItem Header="Both" FontAttributes="Bold,Italic" />
```

**C#:**
```csharp
tabItem.FontAttributes = FontAttributes.Bold;
tabItem.FontAttributes = FontAttributes.Italic;
tabItem.FontAttributes = FontAttributes.Bold | FontAttributes.Italic;
```

### TextColor

Set header text color:

**XAML:**
```xaml
<tabView:SfTabItem Header="Colored" TextColor="#6200EE" />
```

**C#:**
```csharp
tabItem.TextColor = Color.FromArgb("#6200EE");
```

### Complete Text Styling Example

```xaml
<tabView:SfTabItem Header="FEATURED" 
                   FontSize="14"
                   FontFamily="OpenSans-Bold"
                   FontAttributes="Bold"
                   TextColor="White"
                   ImageSource="star.png"
                   ImagePosition="Left" />
```

## Badge and Notification Support

Badges notify users of new messages, notifications, or status updates on tab items.

### Adding Badge Text

Use the `BadgeText` property to display a badge with text or numbers:

**XAML:**
```xaml
<ContentPage xmlns:tabView="clr-namespace:Syncfusion.Maui.TabView;assembly=Syncfusion.Maui.TabView"
             xmlns:core="clr-namespace:Syncfusion.Maui.Core;assembly=Syncfusion.Maui.Core">
    <tabView:SfTabView>
        <tabView:SfTabView.Items>
            <tabView:SfTabItem Header="Messages" 
                              ImageSource="messages.png"
                              ImagePosition="Top"
                              BadgeText="12" />
            
            <tabView:SfTabItem Header="Notifications" 
                              ImageSource="bell.png"
                              ImagePosition="Top"
                              BadgeText="5" />
            
            <tabView:SfTabItem Header="Profile" 
                              ImageSource="user.png"
                              ImagePosition="Top" />
        </tabView:SfTabView.Items>
    </tabView:SfTabView>
</ContentPage>
```

**C#:**
```csharp
var messagesTab = new SfTabItem
{
    Header = "Messages",
    ImageSource = "messages.png",
    ImagePosition = TabImagePosition.Top,
    BadgeText = "12"
};

var notificationsTab = new SfTabItem
{
    Header = "Notifications",
    ImageSource = "bell.png",
    ImagePosition = TabImagePosition.Top,
    BadgeText = "5"
};

tabView.Items.Add(messagesTab);
tabView.Items.Add(notificationsTab);
```

### Customizing Badge Appearance

Use `BadgeSettings` to customize badge styling, colors, and positioning:

**XAML:**
```xaml
xmlns:core="clr-namespace:Syncfusion.Maui.Core;assembly=Syncfusion.Maui.Core"

<tabView:SfTabItem Header="Messages" BadgeText="15">
    <tabView:SfTabItem.BadgeSettings>
        <core:BadgeSettings Type="Error" 
                           FontSize="14" 
                           FontAttributes="Bold" />
    </tabView:SfTabItem.BadgeSettings>
</tabView:SfTabItem>
```

**C#:**
```csharp
using Syncfusion.Maui.Core;

var tabItem = new SfTabItem
{
    Header = "Messages",
    BadgeText = "15",
    BadgeSettings = new BadgeSettings
    {
        Type = BadgeType.Error,
        FontSize = 14,
        FontAttributes = FontAttributes.Bold
    }
};
```

### Badge Types

Use predefined badge types for different notification levels:

**XAML:**
```xaml
<!-- Error/Critical (Red background) -->
<tabView:SfTabItem Header="Urgent" BadgeText="3">
    <tabView:SfTabItem.BadgeSettings>
        <core:BadgeSettings Type="Error" FontSize="12" />
    </tabView:SfTabItem.BadgeSettings>
</tabView:SfTabItem>

<!-- Warning (Orange/Yellow background) -->
<tabView:SfTabItem Header="Pending" BadgeText="7">
    <tabView:SfTabItem.BadgeSettings>
        <core:BadgeSettings Type="Warning" FontSize="12" />
    </tabView:SfTabItem.BadgeSettings>
</tabView:SfTabItem>

<!-- Success (Green background) -->
<tabView:SfTabItem Header="Completed" BadgeText="✓">
    <tabView:SfTabItem.BadgeSettings>
        <core:BadgeSettings Type="Success" FontSize="14" />
    </tabView:SfTabItem.BadgeSettings>
</tabView:SfTabItem>

<!-- Info (Blue background) -->
<tabView:SfTabItem Header="Updates" BadgeText="New">
    <tabView:SfTabItem.BadgeSettings>
        <core:BadgeSettings Type="Info" FontSize="10" />
    </tabView:SfTabItem.BadgeSettings>
</tabView:SfTabItem>
```

### Custom Badge Styling

For complete customization, set `Type="None"` and define custom colors:

**XAML:**
```xaml
<tabView:SfTabItem Header="Custom" BadgeText="99+">
    <tabView:SfTabItem.BadgeSettings>
        <core:BadgeSettings Type="None"
                           Background="#FF6B6B"
                           TextColor="White"
                           Stroke="White"
                           StrokeThickness="2"
                           FontSize="12"
                           FontAttributes="Bold"
                           CornerRadius="10"
                           Offset="-5,-5" />
    </tabView:SfTabItem.BadgeSettings>
</tabView:SfTabItem>
```

**C#:**
```csharp
var customBadgeTab = new SfTabItem
{
    Header = "Custom",
    BadgeText = "99+",
    BadgeSettings = new BadgeSettings
    {
        Type = BadgeType.None,
        Background = Color.FromArgb("#FF6B6B"),
        TextColor = Colors.White,
        Stroke = Colors.White,
        StrokeThickness = 2,
        FontSize = 12,
        FontAttributes = FontAttributes.Bold,
        CornerRadius = 10,
        Offset = new Point(-5, -5)
    }
};
```

### Dynamic Badge Updates

Update badge text programmatically based on data changes:

```csharp
// Update badge count
tabView.Items[0].BadgeText = "20";

// Clear badge
tabView.Items[0].BadgeText = string.Empty;

// Show badge conditionally
if (hasNewMessages)
{
    messagesTab.BadgeText = messageCount.ToString();
}
else
{
    messagesTab.BadgeText = string.Empty;
}

// Update from ViewModel
public void UpdateNotificationBadge(int count)
{
    if (count > 0)
    {
        NotificationBadgeText = count > 99 ? "99+" : count.ToString();
    }
    else
    {
        NotificationBadgeText = string.Empty;
    }
}
```

### Badge with ItemsSource Binding

Use data binding to dynamically update badges:

**ViewModel:**
```csharp
public class TabItemModel : INotifyPropertyChanged
{
    private string _title;
    private string _icon;
    private string _badgeText;
    
    public string Title
    {
        get => _title;
        set
        {
            _title = value;
            OnPropertyChanged();
        }
    }
    
    public string Icon
    {
        get => _icon;
        set
        {
            _icon = value;
            OnPropertyChanged();
        }
    }
    
    public string BadgeText
    {
        get => _badgeText;
        set
        {
            _badgeText = value;
            OnPropertyChanged();
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}

public class MainViewModel
{
    public ObservableCollection<TabItemModel> TabItems { get; set; }
    
    public MainViewModel()
    {
        TabItems = new ObservableCollection<TabItemModel>
        {
            new TabItemModel { Title = "Home", Icon = "home.png", BadgeText = "" },
            new TabItemModel { Title = "Messages", Icon = "messages.png", BadgeText = "5" },
            new TabItemModel { Title = "Notifications", Icon = "bell.png", BadgeText = "12" }
        };
    }
    
    public void UpdateMessageBadge(int count)
    {
        TabItems[1].BadgeText = count > 0 ? count.ToString() : string.Empty;
    }
}
```

**XAML:**
```xaml
<tabView:SfTabView ItemsSource="{Binding TabItems}">
    <tabView:SfTabView.HeaderItemTemplate>
        <DataTemplate>
            <tabView:SfTabItem Header="{Binding Title}"
                              ImageSource="{Binding Icon}"
                              ImagePosition="Top"
                              BadgeText="{Binding BadgeText}">
                <tabView:SfTabItem.BadgeSettings>
                    <core:BadgeSettings Type="Error" 
                                       FontSize="12" 
                                       FontAttributes="Bold" />
                </tabView:SfTabItem.BadgeSettings>
            </tabView:SfTabItem>
        </DataTemplate>
    </tabView:SfTabView.HeaderItemTemplate>
</tabView:SfTabView>
```

### Badge Best Practices

1. **Use appropriate badge types:**
   - Error (red) for urgent/critical notifications
   - Warning (orange) for items needing attention
   - Success (green) for completed tasks
   - Info (blue) for general updates

2. **Keep badge text concise:**
   - Use numbers for counts: "5", "23"
   - Use "99+" for counts over 99
   - Use short text: "New", "!", "✓"

3. **Clear badges when viewed:**
   - Set BadgeText to empty string when user views content
   - Update in real-time when possible

4. **Test visibility:**
   - Ensure badge contrasts with tab background
   - Test with different TabBarPlacement positions
   - Verify badge doesn't obscure icon or text

## Content Configuration

### Simple Content

Set any view as tab content:

**XAML:**
```xaml
<tabView:SfTabItem Header="Home">
    <tabView:SfTabItem.Content>
        <Grid Padding="20">
            <Label Text="Welcome to Home" 
                   FontSize="24"
                   HorizontalOptions="Center"
                   VerticalOptions="Center" />
        </Grid>
    </tabView:SfTabItem.Content>
</tabView:SfTabItem>
```

**C#:**
```csharp
var tabItem = new SfTabItem
{
    Header = "Home",
    Content = new Grid
    {
        Padding = new Thickness(20),
        Children =
        {
            new Label
            {
                Text = "Welcome to Home",
                FontSize = 24,
                HorizontalOptions = LayoutOptions.Center,
                VerticalOptions = LayoutOptions.Center
            }
        }
    }
};
```

### Complex Content Layout

**XAML:**
```xaml
<tabView:SfTabItem Header="Profile">
    <tabView:SfTabItem.Content>
        <ScrollView>
            <StackLayout Padding="20" Spacing="15">
                <Frame CornerRadius="50" 
                       WidthRequest="100" 
                       HeightRequest="100"
                       HorizontalOptions="Center"
                       Padding="0">
                    <Image Source="avatar.png" Aspect="AspectFill" />
                </Frame>
                
                <Label Text="John Doe" 
                       FontSize="24" 
                       FontAttributes="Bold"
                       HorizontalOptions="Center" />
                
                <Label Text="john.doe@example.com" 
                       FontSize="16"
                       TextColor="Gray"
                       HorizontalOptions="Center" />
                
                <Button Text="Edit Profile" 
                        CornerRadius="20"
                        BackgroundColor="#6200EE"
                        TextColor="White" />
            </StackLayout>
        </ScrollView>
    </tabView:SfTabItem.Content>
</tabView:SfTabItem>
```

### Loading Content Page

Reference a separate ContentPage as tab content:

```csharp
public class HomePage : ContentPage
{
    public HomePage()
    {
        Content = new StackLayout
        {
            Children =
            {
                new Label { Text = "Home Page Content" }
            }
        };
    }
}

// In main page
var tabItem = new SfTabItem
{
    Header = "Home",
    Content = new HomePage().Content
};
```

## Disabled State

### IsEnabled Property

Disable tab interaction:

**XAML:**
```xaml
<tabView:SfTabItem Header="Disabled Tab" IsEnabled="False" />
```

**C#:**
```csharp
tabItem.IsEnabled = false;
```

### Visual Styling for Disabled State

Apply visual cues for disabled tabs:

```xaml
<tabView:SfTabItem Header="Premium Feature" 
                   IsEnabled="False"
                   TextColor="Gray"
                   ImageSource="lock.png">
    <tabView:SfTabItem.Content>
        <Grid>
            <Label Text="This feature requires a premium subscription" 
                   HorizontalOptions="Center"
                   VerticalOptions="Center"
                   TextColor="Gray" />
        </Grid>
    </tabView:SfTabItem.Content>
</tabView:SfTabItem>
```

### Conditional Enabling

Enable/disable tabs based on logic:

```csharp
// Enable after authentication
tabView.Items[2].IsEnabled = isUserAuthenticated;

// Enable after data load
tabView.Items[3].IsEnabled = hasData;
```

## Complete Examples

### Example 1: Mobile Bottom Navigation

```xaml
<tabView:SfTabView TabBarPlacement="Bottom" TabBarHeight="60">
    <tabView:SfTabView.Items>
        <tabView:SfTabItem Header="Home" 
                           ImageSource="home.png"
                           ImagePosition="Top"
                           FontSize="12"
                           TextColor="#666666">
            <tabView:SfTabItem.Content>
                <Grid BackgroundColor="White">
                    <Label Text="Home Content" 
                           HorizontalOptions="Center" 
                           VerticalOptions="Center" />
                </Grid>
            </tabView:SfTabItem.Content>
        </tabView:SfTabItem>
        
        <tabView:SfTabItem Header="Search" 
                           ImageSource="search.png"
                           ImagePosition="Top"
                           FontSize="12"
                           TextColor="#666666">
            <tabView:SfTabItem.Content>
                <Grid BackgroundColor="White">
                    <Label Text="Search Content" 
                           HorizontalOptions="Center" 
                           VerticalOptions="Center" />
                </Grid>
            </tabView:SfTabItem.Content>
        </tabView:SfTabItem>
        
        <tabView:SfTabItem Header="Profile" 
                           ImageSource="user.png"
                           ImagePosition="Top"
                           FontSize="12"
                           TextColor="#666666">
            <tabView:SfTabItem.Content>
                <Grid BackgroundColor="White">
                    <Label Text="Profile Content" 
                           HorizontalOptions="Center" 
                           VerticalOptions="Center" />
                </Grid>
            </tabView:SfTabItem.Content>
        </tabView:SfTabItem>
    </tabView:SfTabView.Items>
</tabView:SfTabView>
```

### Example 2: Rich Content Tab

```xaml
<tabView:SfTabItem Header="Dashboard" 
                   FontSize="16"
                   FontAttributes="Bold">
    <tabView:SfTabItem.Content>
        <ScrollView>
            <StackLayout Padding="20" Spacing="15">
                <!-- Stats Cards -->
                <Grid ColumnDefinitions="*,*" RowDefinitions="Auto" ColumnSpacing="10">
                    <Frame Grid.Column="0" CornerRadius="10" Padding="15">
                        <StackLayout>
                            <Label Text="Total Users" FontSize="12" TextColor="Gray" />
                            <Label Text="1,234" FontSize="24" FontAttributes="Bold" />
                        </StackLayout>
                    </Frame>
                    <Frame Grid.Column="1" CornerRadius="10" Padding="15">
                        <StackLayout>
                            <Label Text="Active Now" FontSize="12" TextColor="Gray" />
                            <Label Text="89" FontSize="24" FontAttributes="Bold" TextColor="#4CAF50" />
                        </StackLayout>
                    </Frame>
                </Grid>
                
                <!-- Chart Section -->
                <Frame CornerRadius="10" Padding="15">
                    <StackLayout>
                        <Label Text="Activity This Week" FontSize="16" FontAttributes="Bold" />
                        <!-- Chart control would go here -->
                        <Label Text="[Chart Placeholder]" 
                               HeightRequest="200"
                               HorizontalOptions="Center"
                               VerticalOptions="Center"
                               TextColor="Gray" />
                    </StackLayout>
                </Frame>
            </StackLayout>
        </ScrollView>
    </tabView:SfTabItem.Content>
</tabView:SfTabItem>
```

## Best Practices

1. **Icon + Text Positioning:**
   - Use `Top` or `Bottom` for mobile bottom navigation
   - Use `Left` for desktop/web-style tabs

2. **Font Sizes:**
   - Mobile: 10-12px for bottom nav labels
   - Desktop: 14-16px for top tab labels

3. **Touch Target Size:**
   - Ensure proper touch target size (minimum 44x44 points)
   - Use appropriate TabBarHeight or TabBarSize for icons and text

4. **Content Loading:**
   - Use lazy loading for complex tab content
   - Consider using ContentPage for each tab's content

5. **Disabled Tabs:**
   - Provide visual feedback (gray text, lock icon)
   - Consider showing tooltip/message explaining why disabled

6. **Accessibility:**
   - Provide meaningful header text (not just icons)
   - Ensure sufficient color contrast
   - Test with screen readers

## Common Issues

**Issue:** Icons not displaying  
**Solution:** Verify image files are in Resources/Images and Build Action is set to MauiImage

**Issue:** Header text truncated  
**Solution:** Increase TabBarHeight or use shorter labels, consider SizeToContent width mode

**Issue:** Icon and text not aligned properly  
**Solution:** Ensure TabBarHeight or TabBarSize is appropriate for the chosen ImagePosition (48-56 for Left/Right, 72+ for Top/Bottom)
