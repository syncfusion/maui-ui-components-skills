# Layout Customization

## Table of Contents
- [Overview](#overview)
- [Appearance Modes](#appearance-modes)
- [Header Customization](#header-customization)
- [Footer Customization](#footer-customization)
- [Content Customization](#content-customization)
- [Show/Hide Regions](#showhide-regions)
- [Overlay Customization](#overlay-customization)
- [Complete Examples](#complete-examples)

## Overview

The .NET MAUI Popup provides extensive layout customization options to create popups that match your app's design. You can:
- Choose between one-button or two-button footer layouts
- Customize header, footer, and content with templates
- Show or hide individual regions
- Control heights and styling
- Customize button text and behavior

## Appearance Modes

The popup supports two appearance modes for the footer layout.

### OneButton Mode

Displays a single accept button in the footer.

**XAML:**
```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sfPopup="clr-namespace:Syncfusion.Maui.Popup;assembly=Syncfusion.Maui.Popup"
             x:Class="PopupLayout.MainPage">
    <StackLayout Padding="20">
        <Button x:Name="clickToShowPopup" 
                Text="Show Popup" 
                VerticalOptions="Start" 
                HorizontalOptions="Center" 
                Clicked="ClickToShowPopup_Clicked" />
        
        <sfPopup:SfPopup x:Name="sfPopup" 
                         AppearanceMode="OneButton" 
                         ShowFooter="True">
        </sfPopup:SfPopup>
    </StackLayout>
</ContentPage>
```

**C#:**
```csharp
using Syncfusion.Maui.Popup;

public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        sfPopup.ShowFooter = true;
        sfPopup.AppearanceMode = PopupButtonAppearanceMode.OneButton;
    }

    private void ClickToShowPopup_Clicked(object sender, EventArgs e)
    {
        sfPopup.Show();
    }
}
```

### TwoButton Mode

Displays both accept and decline buttons in the footer.

**XAML:**
```xml
<sfPopup:SfPopup x:Name="sfPopup" 
                 AppearanceMode="TwoButton" 
                 ShowFooter="True">
</sfPopup:SfPopup>
```

**C#:**
```csharp
sfPopup.ShowFooter = true;
sfPopup.AppearanceMode = PopupButtonAppearanceMode.TwoButton;
sfPopup.Show();
```

## Header Customization

### Header Title

Set a simple text header using the `HeaderTitle` property.

**XAML:**
```xml
<sfPopup:SfPopup x:Name="sfPopup" 
                 HeaderTitle="Alert Message">
</sfPopup:SfPopup>
```

**C#:**
```csharp
sfPopup.HeaderTitle = "Confirmation";
sfPopup.Show();
```

### Header Height

Customize the header height using the `HeaderHeight` property.

**XAML:**
```xml
<sfPopup:SfPopup x:Name="sfPopup" 
                 HeaderHeight="100">
</sfPopup:SfPopup>
```

**C#:**
```csharp
sfPopup.HeaderHeight = 150;
sfPopup.Show();
```

### Custom Header Template

Create a fully custom header using the `HeaderTemplate` property.

**XAML:**
```xml
<sfPopup:SfPopup x:Name="sfPopup">
    <sfPopup:SfPopup.HeaderTemplate>
        <DataTemplate>
            <Grid BackgroundColor="#6750A4" Padding="16">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="Auto" />
                </Grid.ColumnDefinitions>
                
                <Image Source="info_icon.png"
                       HeightRequest="24"
                       WidthRequest="24"
                       VerticalOptions="Center" />
                
                <Label Grid.Column="1"
                       Text="Custom Header"
                       TextColor="White"
                       FontSize="18"
                       FontAttributes="Bold"
                       VerticalTextAlignment="Center"
                       Margin="10,0,0,0" />
                
                <Image Grid.Column="2"
                       Source="close_icon.png"
                       HeightRequest="20"
                       WidthRequest="20"
                       VerticalOptions="Center">
                    <Image.GestureRecognizers>
                        <TapGestureRecognizer Tapped="OnCloseIconTapped" />
                    </Image.GestureRecognizers>
                </Image>
            </Grid>
        </DataTemplate>
    </sfPopup:SfPopup.HeaderTemplate>
</sfPopup:SfPopup>
```

**C#:**
```csharp
using Syncfusion.Maui.Popup;

public partial class MainPage : ContentPage
{
    DataTemplate headerTemplateView;
    Label headerContent;

    public MainPage()
    {
        InitializeComponent();
        
        headerTemplateView = new DataTemplate(() =>
        {
            headerContent = new Label
            {
                Text = "Customized Header",
                FontAttributes = FontAttributes.Bold,
                BackgroundColor = Color.FromArgb("#6750A4"),
                FontSize = 16,
                HorizontalTextAlignment = TextAlignment.Center,
                VerticalTextAlignment = TextAlignment.Center,
                TextColor = Colors.White,
                Padding = 16
            };
            return headerContent;
        });

        sfPopup.HeaderTemplate = headerTemplateView;
    }

    private void ClickToShowPopup_Clicked(object sender, EventArgs e)
    {
        sfPopup.Show();
    }
}
```

### Header with Gradient Background

```xml
<sfPopup:SfPopup.HeaderTemplate>
    <DataTemplate>
        <Grid>
            <BoxView>
                <BoxView.Background>
                    <LinearGradientBrush StartPoint="0,0" EndPoint="1,1">
                        <GradientStop Color="#6750A4" Offset="0.0" />
                        <GradientStop Color="#9C27B0" Offset="1.0" />
                    </LinearGradientBrush>
                </BoxView.Background>
            </BoxView>
            
            <Label Text="Gradient Header"
                   TextColor="White"
                   FontSize="18"
                   FontAttributes="Bold"
                   HorizontalOptions="Center"
                   VerticalOptions="Center" />
        </Grid>
    </DataTemplate>
</sfPopup:SfPopup.HeaderTemplate>
```

## Footer Customization

### Footer Height

Customize the footer height using the `FooterHeight` property.

**XAML:**
```xml
<sfPopup:SfPopup x:Name="sfPopup" 
                 ShowFooter="True" 
                 FooterHeight="80">
</sfPopup:SfPopup>
```

**C#:**
```csharp
sfPopup.ShowFooter = true;
sfPopup.FooterHeight = 100;
sfPopup.Show();
```

### Button Text Customization

Customize the text for accept and decline buttons.

**XAML:**
```xml
<sfPopup:SfPopup x:Name="sfPopup" 
                 ShowFooter="True" 
                 AppearanceMode="TwoButton"
                 AcceptButtonText="Confirm" 
                 DeclineButtonText="Cancel">
</sfPopup:SfPopup>
```

**C#:**
```csharp
sfPopup.ShowFooter = true;
sfPopup.AppearanceMode = PopupButtonAppearanceMode.TwoButton;
sfPopup.AcceptButtonText = "Yes, Delete";
sfPopup.DeclineButtonText = "No, Keep";
sfPopup.Show();
```

### Custom Footer Template

Create a fully custom footer using the `FooterTemplate` property.

**XAML:**
```xml
<sfPopup:SfPopup x:Name="sfPopup" ShowFooter="True">
    <sfPopup:SfPopup.FooterTemplate>
        <DataTemplate>
            <Grid BackgroundColor="#F5F5F5" 
                  Padding="16" 
                  ColumnSpacing="10">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>
                
                <Button Grid.Column="0"
                        Text="Option 1"
                        BackgroundColor="#4CAF50"
                        TextColor="White"
                        Clicked="OnOption1Clicked" />
                
                <Button Grid.Column="1"
                        Text="Option 2"
                        BackgroundColor="#2196F3"
                        TextColor="White"
                        Clicked="OnOption2Clicked" />
                
                <Button Grid.Column="2"
                        Text="Cancel"
                        BackgroundColor="#9E9E9E"
                        TextColor="White"
                        Clicked="OnCancelClicked" />
            </Grid>
        </DataTemplate>
    </sfPopup:SfPopup.FooterTemplate>
</sfPopup:SfPopup>
```

**C#:**
```csharp
DataTemplate footerTemplateView;

public MainPage()
{
    InitializeComponent();
    
    sfPopup.ShowFooter = true;
    
    footerTemplateView = new DataTemplate(() =>
    {
        var grid = new Grid
        {
            BackgroundColor = Color.FromArgb("#6750A4"),
            Padding = 16,
            ColumnDefinitions =
            {
                new ColumnDefinition { Width = GridLength.Star },
                new ColumnDefinition { Width = GridLength.Star }
            }
        };
        
        var acceptButton = new Button
        {
            Text = "Accept",
            BackgroundColor = Colors.White,
            TextColor = Color.FromArgb("#6750A4")
        };
        acceptButton.Clicked += (s, e) => sfPopup.Dismiss();
        
        var declineButton = new Button
        {
            Text = "Decline",
            BackgroundColor = Colors.Transparent,
            TextColor = Colors.White,
            BorderColor = Colors.White,
            BorderWidth = 1
        };
        declineButton.Clicked += (s, e) => sfPopup.Dismiss();
        
        grid.Add(declineButton, 0, 0);
        grid.Add(acceptButton, 1, 0);
        
        return grid;
    });

    sfPopup.FooterTemplate = footerTemplateView;
}
```

### Footer with Icon Buttons

```xml
<sfPopup:SfPopup.FooterTemplate>
    <DataTemplate>
        <FlexLayout Direction="Row" 
                    JustifyContent="SpaceEvenly" 
                    Padding="10"
                    BackgroundColor="White">
            <Button ImageSource="thumbs_up.png" 
                    BackgroundColor="#4CAF50"
                    WidthRequest="50"
                    HeightRequest="50"
                    CornerRadius="25"
                    Clicked="OnApproveClicked" />
            
            <Button ImageSource="thumbs_down.png" 
                    BackgroundColor="#F44336"
                    WidthRequest="50"
                    HeightRequest="50"
                    CornerRadius="25"
                    Clicked="OnRejectClicked" />
            
            <Button ImageSource="info.png" 
                    BackgroundColor="#2196F3"
                    WidthRequest="50"
                    HeightRequest="50"
                    CornerRadius="25"
                    Clicked="OnInfoClicked" />
        </FlexLayout>
    </DataTemplate>
</sfPopup:SfPopup.FooterTemplate>
```

## Content Customization

### Message Property

Use the `Message` property for simple text content.

**XAML:**
```xml
<sfPopup:SfPopup x:Name="sfPopup" 
                 HeaderTitle="Alert"
                 Message="This is a simple message">
</sfPopup:SfPopup>
```

**C#:**
```csharp
sfPopup.HeaderTitle = "Information";
sfPopup.Message = "Your changes have been saved successfully.";
sfPopup.Show();
```

### Custom Content Template

Create custom content using the `ContentTemplate` property.

**XAML:**
```xml
<sfPopup:SfPopup x:Name="sfPopup" ShowFooter="True">
    <sfPopup:SfPopup.ContentTemplate>
        <DataTemplate>
            <StackLayout Padding="20" Spacing="15">
                <Label Text="Enter your details:" 
                       FontSize="16" 
                       FontAttributes="Bold" />
                
                <Entry Placeholder="Name" />
                <Entry Placeholder="Email" 
                       Keyboard="Email" />
                <Entry Placeholder="Phone" 
                       Keyboard="Telephone" />
                
                <Editor Placeholder="Comments" 
                        HeightRequest="100" />
            </StackLayout>
        </DataTemplate>
    </sfPopup:SfPopup.ContentTemplate>
</sfPopup:SfPopup>
```

**C#:**
```csharp
DataTemplate contentTemplateView;

public MainPage()
{
    InitializeComponent();
    
    contentTemplateView = new DataTemplate(() =>
    {
        var label = new Label
        {
            Text = "This is a customized view for SfPopup",
            BackgroundColor = Colors.SkyBlue,
            HorizontalTextAlignment = TextAlignment.Center,
            VerticalTextAlignment = TextAlignment.Center,
            Padding = 20
        };
        return label;
    });

    sfPopup.ContentTemplate = contentTemplateView;
}
```

### Content with Image and Text

```xml
<sfPopup:SfPopup.ContentTemplate>
    <DataTemplate>
        <StackLayout Padding="20" Spacing="15">
            <Image Source="success_icon.png"
                   HeightRequest="80"
                   WidthRequest="80"
                   HorizontalOptions="Center" />
            
            <Label Text="Success!"
                   FontSize="24"
                   FontAttributes="Bold"
                   HorizontalTextAlignment="Center"
                   TextColor="#4CAF50" />
            
            <Label Text="Your operation completed successfully."
                   FontSize="14"
                   HorizontalTextAlignment="Center"
                   TextColor="#757575" />
        </StackLayout>
    </DataTemplate>
</sfPopup:SfPopup.ContentTemplate>
```

### Content with Data Binding

```xml
<sfPopup:SfPopup x:Name="popup">
    <sfPopup:SfPopup.ContentTemplate>
        <DataTemplate>
            <StackLayout Padding="20" BindingContext="{Binding Source={x:Reference popup}}">
                <Label Text="{Binding BindingContext.Title}" 
                       FontSize="18" 
                       FontAttributes="Bold" />
                <Label Text="{Binding BindingContext.Description}" 
                       FontSize="14" 
                       Margin="0,10,0,0" />
            </StackLayout>
        </DataTemplate>
    </sfPopup:SfPopup.ContentTemplate>
</sfPopup:SfPopup>
```

## Show/Hide Regions

### Disable Header

Hide the header region using the `ShowHeader` property.

**XAML:**
```xml
<sfPopup:SfPopup x:Name="sfPopup" 
                 ShowHeader="False"
                 ShowFooter="True">
</sfPopup:SfPopup>
```

**C#:**
```csharp
sfPopup.ShowHeader = false;
sfPopup.ShowFooter = true;
sfPopup.Show();
```

### Enable Footer

Show the footer region using the `ShowFooter` property.

**XAML:**
```xml
<sfPopup:SfPopup x:Name="sfPopup" 
                 ShowFooter="True">
</sfPopup:SfPopup>
```

**C#:**
```csharp
sfPopup.ShowFooter = true;
sfPopup.Show();
```

### Enable Close Button

Show a close button in the header using the `ShowCloseButton` property.

**XAML:**
```xml
<sfPopup:SfPopup x:Name="sfPopup" 
                 ShowCloseButton="True">
</sfPopup:SfPopup>
```

**C#:**
```csharp
sfPopup.ShowCloseButton = true;
sfPopup.Show();
```

### Minimal Popup (No Header or Footer)

```xml
<sfPopup:SfPopup x:Name="minimalPopup"
                 ShowHeader="False"
                 ShowFooter="False">
    <sfPopup:SfPopup.ContentTemplate>
        <DataTemplate>
            <Frame BackgroundColor="White" 
                   CornerRadius="10" 
                   Padding="20"
                   HasShadow="True">
                <StackLayout>
                    <Label Text="Clean, Minimal Design"
                           FontSize="16"
                           HorizontalTextAlignment="Center" />
                    <Button Text="Close" 
                            Clicked="OnCloseClicked"
                            Margin="0,10,0,0" />
                </StackLayout>
            </Frame>
        </DataTemplate>
    </sfPopup:SfPopup.ContentTemplate>
</sfPopup:SfPopup>
```

## Overlay Customization

### Disable Overlay Background

Remove or customize the overlay behind the popup.

**XAML:**
```xml
<sfPopup:SfPopup x:Name="popup" 
                 ShowOverlayAlways="False"
                 IsOpen="True">
</sfPopup:SfPopup>
```

**C#:**
```csharp
// Disable overlay - allows background interaction
popup.ShowOverlayAlways = false;
popup.Show();
```

**Use Cases:**
- **ShowOverlayAlways="True"** (default): Modal behavior, blocks background
- **ShowOverlayAlways="False"**: Non-modal, allows background interaction

### Transparent Overlay

To block interaction without visible overlay, keep `ShowOverlayAlways="True"` and customize the overlay color to transparent in styles:

```xml
<sfPopup:SfPopup x:Name="popup" 
                 ShowOverlayAlways="True">
    <!-- Overlay is present but can be styled as transparent -->
</sfPopup:SfPopup>
```

## Complete Examples

### Alert Dialog

```xml
<sfPopup:SfPopup x:Name="alertPopup"
                 HeaderTitle="Alert"
                 Message="This operation cannot be undone."
                 ShowFooter="True"
                 AppearanceMode="OneButton"
                 AcceptButtonText="OK">
</sfPopup:SfPopup>
```

### Confirmation Dialog

```xml
<sfPopup:SfPopup x:Name="confirmPopup"
                 HeaderTitle="Confirm Delete"
                 Message="Are you sure you want to delete this item?"
                 ShowFooter="True"
                 AppearanceMode="TwoButton"
                 AcceptButtonText="Delete"
                 DeclineButtonText="Cancel">
</sfPopup:SfPopup>
```

### Custom Login Form

```xml
<sfPopup:SfPopup x:Name="loginPopup"
                 HeaderTitle="Login"
                 ShowFooter="True"
                 AppearanceMode="OneButton"
                 AcceptButtonText="Sign In"
                 WidthRequest="350"
                 HeightRequest="300">
    <sfPopup:SfPopup.ContentTemplate>
        <DataTemplate>
            <StackLayout Padding="20" Spacing="15">
                <Entry Placeholder="Username" />
                <Entry Placeholder="Password" 
                       IsPassword="True" />
                <CheckBox />
                <Label Text="Remember me" 
                       VerticalOptions="Center" />
            </StackLayout>
        </DataTemplate>
    </sfPopup:SfPopup.ContentTemplate>
</sfPopup:SfPopup>
```

### Progress Indicator Popup

```xml
<sfPopup:SfPopup x:Name="progressPopup"
                 ShowHeader="False"
                 ShowFooter="False"
                 WidthRequest="250"
                 HeightRequest="150">
    <sfPopup:SfPopup.ContentTemplate>
        <DataTemplate>
            <StackLayout Padding="30" 
                         Spacing="20"
                         HorizontalOptions="Center"
                         VerticalOptions="Center">
                <ActivityIndicator IsRunning="True"
                                   Color="#6750A4"
                                   HeightRequest="50"
                                   WidthRequest="50" />
                <Label Text="Loading..."
                       HorizontalTextAlignment="Center"
                       FontSize="16" />
            </StackLayout>
        </DataTemplate>
    </sfPopup:SfPopup.ContentTemplate>
</sfPopup:SfPopup>
```

### Rating Popup

```xml
<sfPopup:SfPopup x:Name="ratingPopup"
                 HeaderTitle="Rate Our App"
                 ShowFooter="True"
                 AppearanceMode="TwoButton"
                 AcceptButtonText="Submit"
                 DeclineButtonText="Later">
    <sfPopup:SfPopup.ContentTemplate>
        <DataTemplate>
            <StackLayout Padding="20" Spacing="10">
                <Label Text="How would you rate your experience?"
                       FontSize="14"
                       HorizontalTextAlignment="Center" />
                
                <FlexLayout Direction="Row"
                            JustifyContent="Center"
                            Margin="0,10,0,10">
                    <Label Text="⭐" FontSize="32" />
                    <Label Text="⭐" FontSize="32" />
                    <Label Text="⭐" FontSize="32" />
                    <Label Text="⭐" FontSize="32" />
                    <Label Text="⭐" FontSize="32" />
                </FlexLayout>
                
                <Editor Placeholder="Additional feedback (optional)"
                        HeightRequest="80" />
            </StackLayout>
        </DataTemplate>
    </sfPopup:SfPopup.ContentTemplate>
</sfPopup:SfPopup>
```

## Best Practices

1. **Keep Content Concise:**
   - Use clear, brief messages
   - Avoid overwhelming users with too much content

2. **Use Appropriate Appearance Mode:**
   - OneButton for informational alerts
   - TwoButton for confirmations requiring user choice

3. **Customize Button Text:**
   - Make button labels action-oriented ("Delete", "Save", "Continue")
   - Avoid generic labels when specific actions are clearer

4. **Consistent Styling:**
   - Match popup styling to your app's theme
   - Use consistent colors, fonts, and spacing

5. **Template Reusability:**
   - Create reusable `DataTemplate` resources
   - Share templates across multiple popups

6. **Accessibility:**
   - Ensure sufficient color contrast
   - Use readable font sizes
   - Provide clear button labels
