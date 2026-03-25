# Popup Positioning

## Table of Contents
- [Overview](#overview)
- [Center Positioning](#center-positioning)
- [Absolute Positioning](#absolute-positioning)
- [Relative Positioning](#relative-positioning)
- [Full-Screen Mode](#full-screen-mode)
- [MVVM Positioning](#mvvm-positioning)
- [Auto-Close Duration](#auto-close-duration)
- [Position Over Action Bar](#position-over-action-bar)
- [Returning Results](#returning-results)
- [Integration Examples](#integration-examples)

## Overview

The Syncfusion .NET MAUI Popup provides flexible positioning options to display popups exactly where you need them:

| Method | Description | Use Case |
|--------|-------------|----------|
| Center | Display at screen center | Alerts, confirmations |
| Absolute | Display at x,y coordinates | Fixed position overlays |
| Relative to View | Display near a specific control | Tooltips, context menus |
| Absolute Relative | Offset from relative position | Fine-tuned positioning |
| Full-Screen | Fill entire screen | Modal forms, full overlays |

## Center Positioning

Display the popup at the center of the screen using either the `IsOpen` property or the `Show()` method.

### Using IsOpen Property

**XAML:**
```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sfPopup="clr-namespace:Syncfusion.Maui.Popup;assembly=Syncfusion.Maui.Popup"
             x:Class="PopupPositioning.MainPage">
    <StackLayout Padding="20">
        <Button x:Name="clickToShowPopup" 
                Text="Show Popup" 
                VerticalOptions="Start" 
                HorizontalOptions="Center" 
                Clicked="ClickToShowPopup_Clicked" />
        
        <sfPopup:SfPopup x:Name="sfPopup" />
    </StackLayout>
</ContentPage>
```

**C#:**
```csharp
private void ClickToShowPopup_Clicked(object sender, EventArgs e)
{
    // Display popup at the center of the view
    sfPopup.IsOpen = true;
}
```

### Using Show() Method

```csharp
private void ClickToShowPopup_Clicked(object sender, EventArgs e)
{
    // Display popup at the center using Show() method
    sfPopup.Show();
}
```

Both methods produce the same result - a popup centered on the screen.

## Absolute Positioning

Display the popup at specific x and y coordinates on the screen.

### Using Show(x, y) Method

```csharp
private void ClickToShowPopup_Clicked(object sender, EventArgs e)
{
    // Display popup at x=50, y=200
    sfPopup.Show(50, 200);
}
```

### Using StartX and StartY Properties

**XAML:**
```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sfPopup="clr-namespace:Syncfusion.Maui.Popup;assembly=Syncfusion.Maui.Popup"
             x:Class="PopupPositioning.MainPage">
    <StackLayout>
        <Button x:Name="clickToShowPopup" 
                Text="Show Popup" 
                Margin="0,30,0,30"
                HorizontalOptions="Center"
                Clicked="ClickToShowPopup_Clicked" />
        
        <sfPopup:SfPopup x:Name="sfPopup" 
                         StartX="50" 
                         StartY="200" />
    </StackLayout>
</ContentPage>
```

**C#:**
```csharp
private void ClickToShowPopup_Clicked(object sender, EventArgs e)
{
    // IsOpen uses the StartX and StartY properties
    sfPopup.IsOpen = true;
}
```

**C# Alternative:**
```csharp
// Set properties programmatically
sfPopup.StartX = 100;
sfPopup.StartY = 300;
sfPopup.Show();
```

### Dynamic Positioning Based on Screen Size

```csharp
private void ClickToShowPopup_Clicked(object sender, EventArgs e)
{
    // Get screen dimensions
    var displayInfo = DeviceDisplay.MainDisplayInfo;
    var screenWidth = displayInfo.Width / displayInfo.Density;
    var screenHeight = displayInfo.Height / displayInfo.Density;
    
    // Position in bottom-right corner with margin
    double xPos = screenWidth - 320 - 20; // popup width + margin
    double yPos = screenHeight - 200 - 20; // popup height + margin
    
    sfPopup.Show(xPos, yPos);
}
```

## Relative Positioning

Display the popup relative to any view on the screen using the `ShowRelativeToView()` method.

### Relative Position Options

The `PopupRelativePosition` enum provides 8 positioning options:

| Position | Description |
|----------|-------------|
| `AlignBottom` | At the bottom of the given view |
| `AlignBottomLeft` | At the bottom left position of the given view |
| `AlignBottomRight` | At the bottom right position of the given view |
| `AlignToLeftOf` | To the left of the given view |
| `AlignTop` | At the top of the given view |
| `AlignTopLeft` | At the top left position of the given view |
| `AlignTopRight` | At the top right position of the given view |
| `AlignToRightOf` | To the right of the given view |

### Basic Relative Positioning

**XAML:**
```xml
<ContentPage.Content>       
    <StackLayout VerticalOptions="Start" 
                 HorizontalOptions="Center" 
                 Padding="20">
        <Button x:Name="clickToShowPopup" 
                Text="Show Popup Below Me"
                VerticalOptions="Start" 
                HorizontalOptions="Start"
                Clicked="ClickToShowPopup_Clicked" />   
        
        <sfPopup:SfPopup x:Name="sfPopup" />
    </StackLayout>
</ContentPage.Content>
```

**C#:**
```csharp
private void ClickToShowPopup_Clicked(object sender, EventArgs e)
{
    // Display popup at the bottom of the button
    sfPopup.ShowRelativeToView(
        clickToShowPopup, 
        PopupRelativePosition.AlignBottom
    );
}
```

### All Relative Positions Example

```csharp
// Show popup at the bottom of the given view
sfPopup.ShowRelativeToView(targetView, PopupRelativePosition.AlignBottom);

// Show popup at the bottom left position of the given view
sfPopup.ShowRelativeToView(targetView, PopupRelativePosition.AlignBottomLeft);

// Show popup at the bottom right position of the given view
sfPopup.ShowRelativeToView(targetView, PopupRelativePosition.AlignBottomRight);

// Show popup to the left of the given view
sfPopup.ShowRelativeToView(targetView, PopupRelativePosition.AlignToLeftOf);

// Show popup at the top of the given view
sfPopup.ShowRelativeToView(targetView, PopupRelativePosition.AlignTop);

// Show popup at the top left position of the given view
sfPopup.ShowRelativeToView(targetView, PopupRelativePosition.AlignTopLeft);

// Show popup at the top right position of the given view
sfPopup.ShowRelativeToView(targetView, PopupRelativePosition.AlignTopRight);

// Show popup to the right of the given view
sfPopup.ShowRelativeToView(targetView, PopupRelativePosition.AlignToRightOf);
```

### Absolute Relative Positioning

Display the popup with an offset from the relative position:

```csharp
private void ClickToShowPopup_Clicked(object sender, EventArgs e)
{
    // Show popup at bottom of button with offset
    // xOffset: 10 pixels to the right
    // yOffset: 10 pixels down from the bottom
    sfPopup.ShowRelativeToView(
        clickToShowPopup, 
        PopupRelativePosition.AlignBottom, 
        10, 
        10
    );
}
```

**Understanding Offsets:**

The relative position acts as the origin point (0, 0):
- **Positive X**: Moves popup to the right
- **Negative X**: Moves popup to the left
- **Positive Y**: Moves popup down
- **Negative Y**: Moves popup up

```csharp
// Example: Position above button with slight offset
sfPopup.ShowRelativeToView(
    clickToShowPopup, 
    PopupRelativePosition.AlignTop,
    -20,  // 20 pixels to the left
    -10   // 10 pixels further up
);
```

### Context Menu Example

```csharp
private void OnItemLongPress(object sender, EventArgs e)
{
    var button = sender as Button;
    
    // Create context menu popup
    sfPopup.HeaderTitle = "Options";
    sfPopup.ShowFooter = false;
    sfPopup.WidthRequest = 150;
    sfPopup.HeightRequest = 200;
    
    // Show menu to the right of the tapped item
    sfPopup.ShowRelativeToView(
        button, 
        PopupRelativePosition.AlignRight,
        5,  // Small gap from the button
        0
    );
}
```

## Full-Screen Mode

Display the popup covering the entire screen.

### Using IsFullScreen Property

**XAML:**
```xml
<sfPopup:SfPopup x:Name="sfPopup" 
                 IsFullScreen="True">
</sfPopup:SfPopup>
```

**C#:**
```csharp
private void ClickToShowPopup_Clicked(object sender, EventArgs e)
{
    sfPopup.IsFullScreen = true;
    sfPopup.Show();
}
```

### Using Show(bool) Method

```csharp
private void ClickToShowPopup_Clicked(object sender, EventArgs e)
{
    // Show popup in full-screen mode
    sfPopup.Show(isFullScreen: true);
}
```

### Full-Screen Modal Dialog Example

```xml
<sfPopup:SfPopup x:Name="fullScreenPopup"
                 IsFullScreen="True"
                 ShowCloseButton="True">
    <sfPopup:SfPopup.ContentTemplate>
        <DataTemplate>
            <StackLayout Padding="20">
                <Label Text="Full-Screen Form" 
                       FontSize="24" 
                       FontAttributes="Bold" 
                       Margin="0,0,0,20" />
                
                <Entry Placeholder="Name" Margin="0,0,0,10" />
                <Entry Placeholder="Email" Margin="0,0,0,10" />
                <Editor Placeholder="Message" 
                        HeightRequest="150" 
                        Margin="0,0,0,20" />
                
                <Button Text="Submit" 
                        BackgroundColor="#6750A4" 
                        TextColor="White" />
            </StackLayout>
        </DataTemplate>
    </sfPopup:SfPopup.ContentTemplate>
</sfPopup:SfPopup>
```

## MVVM Positioning

Position popups using data binding in MVVM pattern.

### Basic MVVM Example

**XAML:**
```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sfPopup="clr-namespace:Syncfusion.Maui.Popup;assembly=Syncfusion.Maui.Popup"
             xmlns:local="clr-namespace:PopupPositioning"
             x:Class="PopupPositioning.MainPage">
    <ContentPage.BindingContext>
        <local:ViewModel/>
    </ContentPage.BindingContext>
    
    <StackLayout>
        <Button Text="Show Popup" 
                Margin="0,30,0,30"
                HorizontalOptions="Center"
                Command="{Binding ShowPopupCommand}" />
        
        <Label x:Name="relativeView" 
               Text="Popup will appear below this label" 
               VerticalOptions="StartAndExpand" 
               HorizontalOptions="Center" 
               HorizontalTextAlignment="Center" 
               Padding="20"
               FontSize="14" 
               BackgroundColor="#6750A4" 
               TextColor="White"/>
        
        <sfPopup:SfPopup IsOpen="{Binding DisplayPopup}"
                         RelativeView="{x:Reference relativeView}" 
                         RelativePosition="AlignBottom"
                         AbsoluteX="0" 
                         AbsoluteY="5">
        </sfPopup:SfPopup>
    </StackLayout>
</ContentPage>
```

**ViewModel:**
```csharp
using System.ComponentModel;
using System.Runtime.CompilerServices;
using System.Windows.Input;

public class ViewModel : INotifyPropertyChanged
{
    private bool displayPopup;
    
    public bool DisplayPopup
    {
        get => displayPopup;
        set
        {
            displayPopup = value;
            OnPropertyChanged();
        }
    }
    
    public ICommand ShowPopupCommand { get; }
    
    public ViewModel()
    {
        DisplayPopup = false;
        ShowPopupCommand = new Command(ShowPopup);
    }
    
    private void ShowPopup()
    {
        DisplayPopup = true;
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

### Dynamic Relative View in MVVM

```csharp
// ViewModel with dynamic positioning
public class ViewModel : INotifyPropertyChanged
{
    private View currentRelativeView;
    private PopupRelativePosition position;
    
    public View CurrentRelativeView
    {
        get => currentRelativeView;
        set
        {
            currentRelativeView = value;
            OnPropertyChanged();
        }
    }
    
    public PopupRelativePosition Position
    {
        get => position;
        set
        {
            position = value;
            OnPropertyChanged();
        }
    }
    
    public void ShowPopupNearButton(Button button)
    {
        CurrentRelativeView = button;
        Position = PopupRelativePosition.AlignBottom;
        DisplayPopup = true;
    }
}
```

## Auto-Close Duration

Automatically close the popup after a specified time.

### Using AutoCloseDuration Property

**XAML:**
```xml
<sfPopup:SfPopup x:Name="toastPopup" 
                 IsOpen="True"
                 AutoCloseDuration="3000">
    <sfPopup:SfPopup.ContentTemplate>
        <DataTemplate>
            <Label Text="This popup will close in 3 seconds"
                   Padding="20"
                   HorizontalTextAlignment="Center" />
        </DataTemplate>
    </sfPopup:SfPopup.ContentTemplate>
</sfPopup:SfPopup>
```

**C#:**
```csharp
public void ShowToastNotification(string message)
{
    toastPopup.Message = message;
    toastPopup.AutoCloseDuration = 3000; // 3 seconds
    toastPopup.Show();
}
```

### Toast-Style Notification Example

```csharp
public void ShowSuccessToast()
{
    var popup = new SfPopup
    {
        ShowHeader = false,
        ShowFooter = false,
        HeightRequest = 60,
        WidthRequest = 300,
        AutoCloseDuration = 2000,
        ContentTemplate = new DataTemplate(() =>
        {
            return new Grid
            {
                BackgroundColor = Color.FromArgb("#4CAF50"),
                Padding = 15,
                Children =
                {
                    new Label
                    {
                        Text = "✓ Operation completed successfully",
                        TextColor = Colors.White,
                        VerticalTextAlignment = TextAlignment.Center
                    }
                }
            };
        })
    };
    
    popup.Show();
}
```

## Position Over Action Bar

Control whether the popup considers the action bar when positioning.

### Using IgnoreActionBar Property

**XAML:**
```xml
<sfPopup:SfPopup x:Name="popup"
                 IgnoreActionBar="True"> 
</sfPopup:SfPopup>
```

**C#:**
```csharp
// Position at top of screen, overlapping action bar
sfPopup.IgnoreActionBar = true;
sfPopup.Show(0, 0);
```

**When to Use:**
- **True**: Popup can overlap the action bar (useful for full-screen overlays)
- **False**: Popup positioning accounts for action bar height (default behavior)

## Returning Results

Get user feedback asynchronously using `ShowAsync()`.

### Using ShowAsync() Method

```csharp
private async void OnConfirmDelete_Clicked(object sender, EventArgs e)
{
    sfPopup.HeaderTitle = "Confirm Delete";
    sfPopup.Message = "Are you sure you want to delete this item?";
    sfPopup.ShowFooter = true;
    sfPopup.AppearanceMode = PopupButtonAppearanceMode.TwoButton;
    sfPopup.AcceptButtonText = "Delete";
    sfPopup.DeclineButtonText = "Cancel";
    
    // Wait for user response
    bool userConfirmed = await sfPopup.ShowAsync();
    
    if (userConfirmed)
    {
        // User clicked Accept button
        await DeleteItem();
    }
    else
    {
        // User clicked Decline button or closed popup
        Console.WriteLine("Delete cancelled");
    }
}
```

### Static Show Method with Result

```csharp
private async void OnAskQuestion_Clicked(object sender, EventArgs e)
{
    bool answer = await SfPopup.Show(
        title: "Question",
        message: "Would you like to save your changes?",
        acceptText: "Save",
        declineText: "Discard"
    );
    
    if (answer)
    {
        await SaveChanges();
    }
}
```

## Integration Examples

### Show Popup on DataGrid Cell Tap

```csharp
private void OnDataGridCellTapped(object sender, DataGridCellTappedEventArgs e)
{
    // Get tapped cell position
    var cellBounds = e.RowColumnIndex;
    
    sfPopup.HeaderTitle = "Cell Details";
    sfPopup.Message = $"Row: {cellBounds.RowIndex}, Column: {cellBounds.ColumnIndex}";
    sfPopup.Show();
}
```

### Show Popup on ListView Item Tap

```csharp
private void OnListViewItemTapped(object sender, ItemTappedEventArgs e)
{
    var tappedItem = e.Item as MyModel;
    var listViewItem = sender as ListView;
    
    sfPopup.HeaderTitle = tappedItem.Title;
    sfPopup.ContentTemplate = new DataTemplate(() =>
    {
        return new Label
        {
            Text = tappedItem.Description,
            Padding = 20
        };
    });
    
    // Show popup near the tapped item
    sfPopup.Show();
}
```

### Switch-Controlled Popup

```xml
<StackLayout Padding="20">
    <Switch x:Name="popupSwitch" 
            IsToggled="False" 
            VerticalOptions="Start" 
            HorizontalOptions="Center"/>
    
    <sfPopup:SfPopup x:Name="sfPopup"
                     IsOpen="{Binding Source={x:Reference popupSwitch}, Path=IsToggled}"/>
</StackLayout>
```

## Best Practices

1. **Choose the Right Positioning Method:**
   - Use center positioning for alerts and confirmations
   - Use relative positioning for context menus and tooltips
   - Use absolute positioning for fixed overlays
   - Use full-screen for modal forms

2. **Consider Screen Size:**
   - Test positioning on different device sizes
   - Use relative positioning to adapt to layouts
   - Avoid hardcoded coordinates when possible

3. **Provide Clear Dismissal Options:**
   - Add close button for non-critical popups
   - Use footer buttons for actions requiring user choice
   - Enable overlay tap to close for informational popups

4. **Use Auto-Close for Transient Messages:**
   - Toast notifications: 2-3 seconds
   - Success messages: 2-4 seconds
   - Warning messages: 4-6 seconds

5. **MVVM-Friendly Patterns:**
   - Bind `IsOpen` to ViewModel properties
   - Use commands for showing/hiding popups
   - Keep positioning logic in ViewModels when possible
