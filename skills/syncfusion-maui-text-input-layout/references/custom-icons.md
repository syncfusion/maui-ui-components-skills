# Custom Icons and Views in .NET MAUI Text Input Layout

Learn how to add custom icons or views to the leading and trailing edges of text input fields.

## Overview

SfTextInputLayout supports adding custom views (icons, labels, buttons) at two positions:
- **Leading View** — Left edge (right edge in RTL)
- **Trailing View** — Right edge (left edge in RTL)

Each view can be positioned **Inside** or **Outside** the container.

## Leading View

Add an icon or custom view to the leading edge (left side) of the input.

### Basic Leading View

#### XAML

```xml
<inputLayout:SfTextInputLayout Hint="Birth Date"
                               LeadingViewPosition="Inside">
    <Entry />
    <inputLayout:SfTextInputLayout.LeadingView>
        <Label Text="📅" FontSize="20" />
    </inputLayout:SfTextInputLayout.LeadingView>
</inputLayout:SfTextInputLayout>
```

#### C#

```csharp
var inputLayout = new SfTextInputLayout
{
    Hint = "Birth Date",
    LeadingViewPosition = ViewPosition.Inside,
    LeadingView = new Label 
    { 
        Text = "📅",
        FontSize = 20 
    },
    Content = new Entry()
};
```

### Leading View Position

Control whether the leading view appears inside or outside the container border.

#### Inside Position (Default for Leading View: Outside)

```xml
<inputLayout:SfTextInputLayout Hint="Search"
                               LeadingViewPosition="Inside"
                               ContainerType="Filled">
    <Entry Placeholder="Search..." />
    <inputLayout:SfTextInputLayout.LeadingView>
        <Label Text="🔍" FontSize="18" />
    </inputLayout:SfTextInputLayout.LeadingView>
</inputLayout:SfTextInputLayout>
```

**Visual:** Icon appears within the input container

#### Outside Position

```xml
<inputLayout:SfTextInputLayout Hint="Email"
                               LeadingViewPosition="Outside"
                               ContainerType="Outlined">
    <Entry Keyboard="Email" />
    <inputLayout:SfTextInputLayout.LeadingView>
        <Label Text="✉️" FontSize="20" />
    </inputLayout:SfTextInputLayout.LeadingView>
</inputLayout:SfTextInputLayout>
```

```csharp
var inputLayout = new SfTextInputLayout
{
    Hint = "Email",
    LeadingViewPosition = ViewPosition.Outside,
    ContainerType = ContainerType.Outlined,
    LeadingView = new Label { Text = "✉️", FontSize = 20 },
    Content = new Entry { Keyboard = Keyboard.Email }
};
```

**Visual:** Icon appears before the container border

### Common Leading View Examples

#### Calendar Icon

```xml
<inputLayout:SfTextInputLayout Hint="Date of Birth"
                               LeadingViewPosition="Inside"
                               ContainerType="Outlined">
    <DatePicker />
    <inputLayout:SfTextInputLayout.LeadingView>
        <Label Text="&#x1F5D3;" FontSize="20" />
    </inputLayout:SfTextInputLayout.LeadingView>
</inputLayout:SfTextInputLayout>
```

#### User Icon

```xml
<inputLayout:SfTextInputLayout Hint="Username"
                               LeadingViewPosition="Inside">
    <Entry />
    <inputLayout:SfTextInputLayout.LeadingView>
        <Label Text="👤" FontSize="18" />
    </inputLayout:SfTextInputLayout.LeadingView>
</inputLayout:SfTextInputLayout>
```

#### Location Pin

```xml
<inputLayout:SfTextInputLayout Hint="Address"
                               LeadingViewPosition="Inside"
                               ContainerType="Outlined">
    <Entry />
    <inputLayout:SfTextInputLayout.LeadingView>
        <Label Text="📍" FontSize="20" />
    </inputLayout:SfTextInputLayout.LeadingView>
</inputLayout:SfTextInputLayout>
```

## Trailing View

Add an icon or custom view to the trailing edge (right side) of the input.

### Basic Trailing View

#### XAML

```xml
<inputLayout:SfTextInputLayout Hint="Birth Date"
                               TrailingViewPosition="Outside">
    <Entry />
    <inputLayout:SfTextInputLayout.TrailingView>
        <Label Text="📅" FontSize="20" />
    </inputLayout:SfTextInputLayout.TrailingView>
</inputLayout:SfTextInputLayout>
```

#### C#

```csharp
var inputLayout = new SfTextInputLayout
{
    Hint = "Birth Date",
    TrailingViewPosition = ViewPosition.Outside,
    TrailingView = new Label 
    { 
        Text = "📅",
        FontSize = 20 
    },
    Content = new Entry()
};
```

### Trailing View Position

Control whether the trailing view appears inside or outside the container.

#### Inside Position (Default for Trailing View)

```xml
<inputLayout:SfTextInputLayout Hint="Search"
                               TrailingViewPosition="Inside"
                               ContainerType="Filled">
    <Entry />
    <inputLayout:SfTextInputLayout.TrailingView>
        <Label Text="🔍" FontSize="18" />
    </inputLayout:SfTextInputLayout.TrailingView>
</inputLayout:SfTextInputLayout>
```

**Visual:** Icon appears within the input container (commonly used)

#### Outside Position

```xml
<inputLayout:SfTextInputLayout Hint="Phone"
                               TrailingViewPosition="Outside"
                               ContainerType="Outlined">
    <Entry Keyboard="Telephone" />
    <inputLayout:SfTextInputLayout.TrailingView>
        <Label Text="📞" FontSize="20" />
    </inputLayout:SfTextInputLayout.TrailingView>
</inputLayout:SfTextInputLayout>
```

**Visual:** Icon appears after the container border

### Common Trailing View Examples

#### Clear Button

```xml
<inputLayout:SfTextInputLayout x:Name="searchInput"
                               Hint="Search"
                               TrailingViewPosition="Inside">
    <Entry x:Name="searchEntry" TextChanged="OnSearchTextChanged" />
    <inputLayout:SfTextInputLayout.TrailingView>
        <Label Text="✖️" 
               FontSize="16"
               IsVisible="False"
               x:Name="clearButton">
            <Label.GestureRecognizers>
                <TapGestureRecognizer Tapped="OnClearTapped" />
            </Label.GestureRecognizers>
        </Label>
    </inputLayout:SfTextInputLayout.TrailingView>
</inputLayout:SfTextInputLayout>
```

```csharp
private void OnSearchTextChanged(object sender, TextChangedEventArgs e)
{
    clearButton.IsVisible = !string.IsNullOrEmpty(e.NewTextValue);
}

private void OnClearTapped(object sender, EventArgs e)
{
    searchEntry.Text = string.Empty;
}
```

#### Info Icon

```xml
<inputLayout:SfTextInputLayout Hint="Credit Card"
                               TrailingViewPosition="Inside"
                               ContainerType="Outlined">
    <Entry Keyboard="Numeric" />
    <inputLayout:SfTextInputLayout.TrailingView>
        <Label Text="ℹ️" FontSize="18">
            <Label.GestureRecognizers>
                <TapGestureRecognizer Tapped="OnInfoTapped" />
            </Label.GestureRecognizers>
        </Label>
    </inputLayout:SfTextInputLayout.TrailingView>
</inputLayout:SfTextInputLayout>
```

#### Microphone Icon

```xml
<inputLayout:SfTextInputLayout Hint="Voice Search"
                               TrailingViewPosition="Inside">
    <Entry />
    <inputLayout:SfTextInputLayout.TrailingView>
        <Label Text="🎤" FontSize="18">
            <Label.GestureRecognizers>
                <TapGestureRecognizer Tapped="OnVoiceSearchTapped" />
            </Label.GestureRecognizers>
        </Label>
    </inputLayout:SfTextInputLayout.TrailingView>
</inputLayout:SfTextInputLayout>
```

## Using Font Icons

Unicode characters and custom font icons provide scalable, customizable icons.

### Unicode Icons

```xml
<inputLayout:SfTextInputLayout Hint="Email"
                               LeadingViewPosition="Inside">
    <Entry Keyboard="Email" />
    <inputLayout:SfTextInputLayout.LeadingView>
        <Label Text="&#x2709;" FontSize="20" />
    </inputLayout:SfTextInputLayout.LeadingView>
</inputLayout:SfTextInputLayout>
```

Common Unicode values:
- `&#x1F5D3;` — Calendar 📅
- `&#x1F50D;` — Magnifying Glass 🔍
- `&#x2709;` — Envelope ✉
- `&#x1F4CD;` — Pin 📍
- `&#x1F464;` — User 👤
- `&#x1F512;` — Lock 🔒

### Custom Font Icons

First, register your icon font in `MauiProgram.cs`:

```csharp
builder.ConfigureFonts(fonts =>
{
    fonts.AddFont("MaterialIcons-Regular.ttf", "MaterialIcons");
});
```

Then use in XAML:

```xml
<inputLayout:SfTextInputLayout Hint="Location"
                               LeadingViewPosition="Inside">
    <Entry />
    <inputLayout:SfTextInputLayout.LeadingView>
        <Label Text="&#xe55f;" 
               FontFamily="MaterialIcons" 
               FontSize="20"
               TextColor="#6750A4" />
    </inputLayout:SfTextInputLayout.LeadingView>
</inputLayout:SfTextInputLayout>
```

## Using Image Icons

Use `Image` control for complex or colored icons.

```xml
<inputLayout:SfTextInputLayout Hint="Company"
                               LeadingViewPosition="Inside"
                               ContainerType="Outlined">
    <Entry />
    <inputLayout:SfTextInputLayout.LeadingView>
        <Image Source="company_icon.png" 
               WidthRequest="24" 
               HeightRequest="24" />
    </inputLayout:SfTextInputLayout.LeadingView>
</inputLayout:SfTextInputLayout>
```

## Both Leading and Trailing Views

Combine leading and trailing views for maximum functionality.

```xml
<inputLayout:SfTextInputLayout Hint="Password"
                               LeadingViewPosition="Inside"
                               TrailingViewPosition="Inside"
                               ContainerType="Outlined">
    <Entry x:Name="passwordEntry" IsPassword="True" />
    
    <inputLayout:SfTextInputLayout.LeadingView>
        <Label Text="🔒" FontSize="18" />
    </inputLayout:SfTextInputLayout.LeadingView>
    
    <inputLayout:SfTextInputLayout.TrailingView>
        <Label Text="👁️" FontSize="18">
            <Label.GestureRecognizers>
                <TapGestureRecognizer Tapped="OnTogglePassword" />
            </Label.GestureRecognizers>
        </Label>
    </inputLayout:SfTextInputLayout.TrailingView>
</inputLayout:SfTextInputLayout>
```

```csharp
private void OnTogglePassword(object sender, EventArgs e)
{
    passwordEntry.IsPassword = !passwordEntry.IsPassword;
}
```

## Event Handling

Add interactivity to icons using gesture recognizers.

### Tap Gesture

```xml
<inputLayout:SfTextInputLayout.TrailingView>
    <Label Text="ℹ️" FontSize="18">
        <Label.GestureRecognizers>
            <TapGestureRecognizer Tapped="OnIconTapped" />
        </Label.GestureRecognizers>
    </Label>
</inputLayout:SfTextInputLayout.TrailingView>
```

```csharp
private async void OnIconTapped(object sender, EventArgs e)
{
    await DisplayAlert("Information", "Additional help text here", "OK");
}
```

### Button as Icon

```xml
<inputLayout:SfTextInputLayout Hint="Search"
                               TrailingViewPosition="Inside">
    <Entry x:Name="searchEntry" />
    <inputLayout:SfTextInputLayout.TrailingView>
        <Button Text="Clear" 
                FontSize="12"
                BackgroundColor="Transparent"
                TextColor="#6750A4"
                Clicked="OnClearClicked" />
    </inputLayout:SfTextInputLayout.TrailingView>
</inputLayout:SfTextInputLayout>
```

```csharp
private void OnClearClicked(object sender, EventArgs e)
{
    searchEntry.Text = string.Empty;
    searchEntry.Focus();
}
```

## Custom Views

Use any .NET MAUI view as leading or trailing content.

### Custom Layout

```xml
<inputLayout:SfTextInputLayout Hint="Amount"
                               TrailingViewPosition="Inside">
    <Entry Keyboard="Numeric" />
    <inputLayout:SfTextInputLayout.TrailingView>
        <HorizontalStackLayout Spacing="5">
            <Label Text="USD" 
                   FontSize="14" 
                   TextColor="#757575"
                   VerticalOptions="Center" />
        </HorizontalStackLayout>
    </inputLayout:SfTextInputLayout.TrailingView>
</inputLayout:SfTextInputLayout>
```

### Multiple Icons

```xml
<inputLayout:SfTextInputLayout.TrailingView>
    <HorizontalStackLayout Spacing="10">
        <Label Text="🔍" FontSize="16">
            <Label.GestureRecognizers>
                <TapGestureRecognizer Tapped="OnSearchTapped" />
            </Label.GestureRecognizers>
        </Label>
        <Label Text="✖️" FontSize="16">
            <Label.GestureRecognizers>
                <TapGestureRecognizer Tapped="OnClearTapped" />
            </Label.GestureRecognizers>
        </Label>
    </HorizontalStackLayout>
</inputLayout:SfTextInputLayout.TrailingView>
```

## Best Practices

### Icon Selection

- Use universally recognized icons (search 🔍, calendar 📅, lock 🔒)
- Ensure icons match your app's visual style
- Consider color accessibility (sufficient contrast)
- Keep icon size consistent (typically 18-24dp)

### Positioning

- **Inside** for functional icons (clear, toggle, search)
- **Outside** for decorative or informational icons
- Leading for context (what type of data)
- Trailing for actions (what to do with data)

### Interactivity

- Add tap gestures only when action is clear
- Provide visual feedback on tap
- Don't make non-interactive icons look clickable
- Consider adding tooltips for complex actions

### Performance

- Avoid heavy views in leading/trailing positions
- Use labels with fonts over images when possible
- Don't perform expensive operations in tap handlers
- Cache resources for frequently used icons

### Accessibility

- Ensure icons have semantic meaning
- Don't rely solely on color to convey information
- Provide text alternatives when needed
- Test with screen readers

## Common Patterns

### Search Field

```xml
<inputLayout:SfTextInputLayout Hint="Search"
                               LeadingViewPosition="Inside"
                               TrailingViewPosition="Inside"
                               ContainerType="Filled">
    <Entry x:Name="searchBox" TextChanged="OnSearchTextChanged" />
    
    <inputLayout:SfTextInputLayout.LeadingView>
        <Label Text="🔍" FontSize="18" />
    </inputLayout:SfTextInputLayout.LeadingView>
    
    <inputLayout:SfTextInputLayout.TrailingView>
        <Label x:Name="clearIcon" 
               Text="✖️" 
               FontSize="14"
               IsVisible="False">
            <Label.GestureRecognizers>
                <TapGestureRecognizer Tapped="OnClearSearch" />
            </Label.GestureRecognizers>
        </Label>
    </inputLayout:SfTextInputLayout.TrailingView>
</inputLayout:SfTextInputLayout>
```

### Currency Input

```xml
<inputLayout:SfTextInputLayout Hint="Price"
                               LeadingViewPosition="Inside"
                               ContainerType="Outlined">
    <Entry Keyboard="Numeric" />
    <inputLayout:SfTextInputLayout.LeadingView>
        <Label Text="$" 
               FontSize="18" 
               TextColor="#424242"
               VerticalOptions="Center" />
    </inputLayout:SfTextInputLayout.LeadingView>
</inputLayout:SfTextInputLayout>
```

### Date Picker

```xml
<inputLayout:SfTextInputLayout Hint="Date"
                               LeadingViewPosition="Inside"
                               ContainerType="Outlined">
    <DatePicker />
    <inputLayout:SfTextInputLayout.LeadingView>
        <Label Text="📅" FontSize="20" />
    </inputLayout:SfTextInputLayout.LeadingView>
</inputLayout:SfTextInputLayout>
```

### Password with Strength Indicator

```xml
<inputLayout:SfTextInputLayout x:Name="passwordLayout"
                               Hint="Password"
                               TrailingViewPosition="Inside"
                               ContainerType="Outlined">
    <Entry x:Name="passwordField" 
           IsPassword="True" 
           TextChanged="OnPasswordChanged" />
    
    <inputLayout:SfTextInputLayout.TrailingView>
        <Label x:Name="strengthIndicator" 
               Text="●" 
               FontSize="20"
               TextColor="Gray" />
    </inputLayout:SfTextInputLayout.TrailingView>
</inputLayout:SfTextInputLayout>
```

```csharp
private void OnPasswordChanged(object sender, TextChangedEventArgs e)
{
    int strength = CalculatePasswordStrength(e.NewTextValue);
    strengthIndicator.TextColor = strength switch
    {
        >= 4 => Colors.Green,
        >= 2 => Colors.Orange,
        _ => Colors.Red
    };
}
```

## Platform Considerations

All features work consistently across:
- Windows
- iOS
- Android
- macOS

RTL (Right-to-Left) layouts automatically flip leading and trailing positions.
