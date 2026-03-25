# Advanced Integration and Examples

## Table of Contents
- [Overview](#overview)
- [ListView in Popup](#listview-in-popup)
- [DataGrid Integration](#datagrid-integration)
- [MVVM Patterns](#mvvm-patterns)
- [Localization Support](#localization-support)
- [Liquid Glass Effect](#liquid-glass-effect)
- [Best Practices](#best-practices)

## Overview

This guide covers advanced integration scenarios for the .NET MAUI Popup, including:
- Displaying complex controls like ListView and DataGrid within popups
- Full MVVM implementation patterns with commands
- Localization for multi-language support
- Advanced styling with liquid glass effects

## ListView in Popup

### Basic ListView Integration

Display a Syncfusion ListView inside a popup's content template.

**XAML:**
```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sfListView="clr-namespace:Syncfusion.Maui.ListView;assembly=Syncfusion.Maui.ListView"
             xmlns:sfPopup="clr-namespace:Syncfusion.Maui.Popup;assembly=Syncfusion.Maui.Popup"
             xmlns:local="clr-namespace:PopupIntegration"
             x:Class="PopupIntegration.MainPage">
    <ContentPage.BindingContext>
        <local:ContactsViewModel x:Name="viewModel"/>
    </ContentPage.BindingContext>
    
    <StackLayout Padding="20">
        <Button Text="Show Contacts" 
                Clicked="OpenButton_Clicked"
                HorizontalOptions="Center"/>
        
        <sfPopup:SfPopup x:Name="sfPopup" 
                         HeaderTitle="Contacts"
                         ShowFooter="True"
                         HeightRequest="400" 
                         WidthRequest="350">
            <sfPopup:SfPopup.ContentTemplate>
                <DataTemplate>
                    <sfListView:SfListView ItemSize="60"
                                           ItemsSource="{Binding Items}">
                        <sfListView:SfListView.ItemTemplate>
                            <DataTemplate>                                
                                <Grid Padding="10">                                           
                                    <Grid.ColumnDefinitions>
                                        <ColumnDefinition Width="50" />
                                        <ColumnDefinition Width="*" />
                                        <ColumnDefinition Width="Auto" />
                                    </Grid.ColumnDefinitions>
                                    
                                    <Image Source="{Binding ContactImage}"
                                           VerticalOptions="Center"
                                           HorizontalOptions="Center"
                                           HeightRequest="40"
                                           WidthRequest="40"/>
                                    
                                    <Label Grid.Column="1"
                                           VerticalTextAlignment="Center"
                                           Text="{Binding ContactName}" 
                                           FontSize="16" />
                                    
                                    <Image Grid.Column="2" 
                                           Source="{Binding ContactType}"
                                           VerticalOptions="Center"
                                           HeightRequest="20"
                                           WidthRequest="20"/>
                                </Grid>                                   
                            </DataTemplate>
                        </sfListView:SfListView.ItemTemplate>
                    </sfListView:SfListView>
                </DataTemplate>
            </sfPopup:SfPopup.ContentTemplate>
        </sfPopup:SfPopup>
    </StackLayout>
</ContentPage>
```

**ViewModel:**
```csharp
using System.Collections.ObjectModel;

public class ContactsViewModel
{
    public ObservableCollection<Contact> Items { get; set; }
    
    public ContactsViewModel()
    {
        Items = new ObservableCollection<Contact>
        {
            new Contact { ContactName = "John Doe", ContactImage = "user1.png", ContactType = "phone.png" },
            new Contact { ContactName = "Jane Smith", ContactImage = "user2.png", ContactType = "email.png" },
            new Contact { ContactName = "Bob Johnson", ContactImage = "user3.png", ContactType = "phone.png" },
            // Add more contacts...
        };
    }
}

public class Contact
{
    public string ContactName { get; set; }
    public string ContactImage { get; set; }
    public string ContactType { get; set; }
}
```

**Code-Behind:**
```csharp
private void OpenButton_Clicked(object sender, EventArgs e)
{
    sfPopup.Show();
}
```

### ListView with Search

```xml
<sfPopup:SfPopup.ContentTemplate>
    <DataTemplate>
        <StackLayout>
            <Entry Placeholder="Search contacts..." 
                   Margin="10"
                   TextChanged="OnSearchTextChanged" />
            
            <sfListView:SfListView x:Name="listView"
                                   ItemSize="60"
                                   ItemsSource="{Binding FilteredItems}">
                <!-- Item template here -->
            </sfListView:SfListView>
        </StackLayout>
    </DataTemplate>
</sfPopup:SfPopup.ContentTemplate>
```

```csharp
private void OnSearchTextChanged(object sender, TextChangedEventArgs e)
{
    var viewModel = (ContactsViewModel)BindingContext;
    viewModel.FilterContacts(e.NewTextValue);
}
```

## DataGrid Integration

### Show Popup on Cell Tap

Display a popup when a DataGrid cell is tapped.

**XAML:**
```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sfDatagrid="clr-namespace:Syncfusion.Maui.DataGrid;assembly=Syncfusion.Maui.DataGrid"
             xmlns:sfPopup="clr-namespace:Syncfusion.Maui.Popup;assembly=Syncfusion.Maui.Popup"
             xmlns:local="clr-namespace:PopupIntegration"
             x:Class="PopupIntegration.DataGridPage">
    <ContentPage.BindingContext>
        <local:OrdersViewModel x:Name="viewModel" />
    </ContentPage.BindingContext>
    
    <Grid>
        <sfDatagrid:SfDataGrid x:Name="dataGrid"
                               ItemsSource="{Binding OrdersInfo}"
                               CellTapped="OnDataGridCellTapped"    
                               ColumnWidthMode="Fill" />
        
        <sfPopup:SfPopup x:Name="sfPopup" 
                         HeaderTitle="Cell Details" 
                         AutoSizeMode="Height"
                         WidthRequest="300"
                         ShowCloseButton="True">
            <sfPopup:SfPopup.ContentTemplate>
                <DataTemplate>
                    <StackLayout Padding="20" Spacing="10">
                        <Label Text="{Binding CellValue}" 
                               FontSize="16"
                               FontAttributes="Bold" />
                        <Label Text="{Binding CellInfo}" 
                               FontSize="14" />
                    </StackLayout>
                </DataTemplate>
            </sfPopup:SfPopup.ContentTemplate>
        </sfPopup:SfPopup>
    </Grid>
</ContentPage>
```

**Code-Behind:**
```csharp
using Syncfusion.Maui.DataGrid;

private void OnDataGridCellTapped(object sender, DataGridCellTappedEventArgs e)
{
    var rowIndex = e.RowColumnIndex.RowIndex;
    var columnIndex = e.RowColumnIndex.ColumnIndex;
    var cellValue = e.CellValue;
    
    // Set popup content
    sfPopup.BindingContext = new
    {
        CellValue = cellValue?.ToString() ?? "N/A",
        CellInfo = $"Row: {rowIndex}, Column: {columnIndex}"
    };
    
    sfPopup.Show();
}
```

### DataGrid Inside Popup

Display a full DataGrid within a popup for detailed data viewing.

```xml
<sfPopup:SfPopup x:Name="dataGridPopup"
                 HeaderTitle="Order Details"
                 IsFullScreen="True"
                 ShowCloseButton="True">
    <sfPopup:SfPopup.ContentTemplate>
        <DataTemplate>
            <sfDatagrid:SfDataGrid ItemsSource="{Binding OrderDetails}"
                                   AutoGenerateColumnsMode="None">
                <sfDatagrid:SfDataGrid.Columns>
                    <sfDatagrid:DataGridTextColumn MappingName="ProductName" 
                                                   HeaderText="Product" />
                    <sfDatagrid:DataGridTextColumn MappingName="Quantity" 
                                                   HeaderText="Qty" />
                    <sfDatagrid:DataGridTextColumn MappingName="Price" 
                                                   HeaderText="Price" 
                                                   Format="C2" />
                </sfDatagrid:SfDataGrid.Columns>
            </sfDatagrid:SfDataGrid>
        </DataTemplate>
    </sfPopup:SfPopup.ContentTemplate>
</sfPopup:SfPopup>
```

## MVVM Patterns

### Complete MVVM Implementation

Full MVVM pattern with commands and data binding.

**ViewModel:**
```csharp
using System.ComponentModel;
using System.Runtime.CompilerServices;
using System.Windows.Input;

public class PopupViewModel : INotifyPropertyChanged
{
    private bool _isPopupOpen;
    private string _popupTitle;
    private string _popupMessage;
    
    public bool IsPopupOpen
    {
        get => _isPopupOpen;
        set
        {
            _isPopupOpen = value;
            OnPropertyChanged();
        }
    }
    
    public string PopupTitle
    {
        get => _popupTitle;
        set
        {
            _popupTitle = value;
            OnPropertyChanged();
        }
    }
    
    public string PopupMessage
    {
        get => _popupMessage;
        set
        {
            _popupMessage = value;
            OnPropertyChanged();
        }
    }
    
    public ICommand OpenPopupCommand { get; }
    public ICommand ClosePopupCommand { get; }
    public ICommand AcceptCommand { get; }
    public ICommand DeclineCommand { get; }
    
    public PopupViewModel()
    {
        OpenPopupCommand = new Command(OpenPopup);
        ClosePopupCommand = new Command(ClosePopup);
        AcceptCommand = new Command(OnAccept);
        DeclineCommand = new Command(OnDecline);
    }
    
    private void OpenPopup()
    {
        PopupTitle = "Confirmation";
        PopupMessage = "Do you want to proceed with this action?";
        IsPopupOpen = true;
    }
    
    private void ClosePopup()
    {
        IsPopupOpen = false;
    }
    
    private void OnAccept()
    {
        // Handle accept action
        Console.WriteLine("User accepted");
        IsPopupOpen = false;
    }
    
    private void OnDecline()
    {
        // Handle decline action
        Console.WriteLine("User declined");
        IsPopupOpen = false;
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

**XAML:**
```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sfPopup="clr-namespace:Syncfusion.Maui.Popup;assembly=Syncfusion.Maui.Popup"
             xmlns:local="clr-namespace:PopupIntegration"
             x:Class="PopupIntegration.MVVMPage">
    <ContentPage.BindingContext>
        <local:PopupViewModel />
    </ContentPage.BindingContext>
    
    <StackLayout Padding="20">
        <Button Text="Show Popup"
                Command="{Binding OpenPopupCommand}"
                HorizontalOptions="Center" />
        
        <sfPopup:SfPopup IsOpen="{Binding IsPopupOpen}"
                         HeaderTitle="{Binding PopupTitle}"
                         Message="{Binding PopupMessage}"
                         ShowFooter="True"
                         AppearanceMode="TwoButton"
                         AcceptCommand="{Binding AcceptCommand}"
                         DeclineCommand="{Binding DeclineCommand}"
                         AcceptButtonText="Yes"
                         DeclineButtonText="No" />
    </StackLayout>
</ContentPage>
```

### Reusable Popup as Page

Create a reusable popup as a separate XAML file.

**PopupPage.xaml:**
```xml
<sfPopup:SfPopup xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="PopupIntegration.PopupPage"
                 xmlns:sfPopup="clr-namespace:Syncfusion.Maui.Popup;assembly=Syncfusion.Maui.Popup"
                 x:Name="sfPopup">
    <sfPopup:SfPopup.HeaderTemplate>
        <DataTemplate>
            <Grid BackgroundColor="#D32F2F" Padding="16">
                <Label Text="Error" 
                       TextColor="White"
                       FontSize="18"
                       FontAttributes="Bold" />
            </Grid>
        </DataTemplate>
    </sfPopup:SfPopup.HeaderTemplate>
    
    <sfPopup:SfPopup.ContentTemplate>
        <DataTemplate>
            <StackLayout Padding="20" Spacing="15">
                <Image Source="error_icon.png"
                       HeightRequest="60"
                       WidthRequest="60"
                       HorizontalOptions="Center" />
                
                <Label x:Name="errorMessage"
                       Text="{Binding Source={x:Reference sfPopup}, Path=BindingContext.ErrorMessage}"
                       FontSize="14"
                       HorizontalTextAlignment="Center" />
            </StackLayout>
        </DataTemplate>
    </sfPopup:SfPopup.ContentTemplate>
</sfPopup:SfPopup>
```

**PopupPage.xaml.cs:**
```csharp
using Syncfusion.Maui.Popup;

public partial class PopupPage : SfPopup
{
    public PopupPage()
    {
        InitializeComponent();
        ShowFooter = true;
        AppearanceMode = PopupButtonAppearanceMode.OneButton;
        AcceptButtonText = "OK";
    }
}
```

**Usage:**
```csharp
private void ShowErrorPopup(string errorMessage)
{
    var errorPopup = new PopupPage
    {
        BindingContext = new { ErrorMessage = errorMessage }
    };
    errorPopup.Show();
}
```

## Localization Support

### Setup Localization

The Syncfusion .NET MAUI Popup supports localization for button text and messages.

**1. Create Resource Files:**

Create `.resx` files for each language (e.g., `Resources.resx`, `Resources.es.resx`, `Resources.fr.resx`).

**Resources.resx (English):**
```xml
<data name="AcceptButtonText" xml:space="preserve">
  <value>OK</value>
</data>
<data name="DeclineButtonText" xml:space="preserve">
  <value>Cancel</value>
</data>
```

**Resources.es.resx (Spanish):**
```xml
<data name="AcceptButton" xml:space="preserve">
  <value>Aceptar</value>
</data>
<data name="DeclineButton" xml:space="preserve">
  <value>Cancelar</value>
</data>
```

**2. Use Localized Strings:**

```csharp
using System.Resources;
using System.Globalization;

public class LocalizationService
{
    private readonly ResourceManager _resourceManager;
    
    public LocalizationService()
    {
        _resourceManager = new ResourceManager(
            "YourApp.Resources", 
            typeof(LocalizationService).Assembly
        );
    }
    
    public string GetString(string key)
    {
        return _resourceManager.GetString(key, CultureInfo.CurrentUICulture) ?? key;
    }
}
```

**3. Apply to Popup:**

```csharp
private void ShowLocalizedPopup()
{
    var localization = new LocalizationService();
    
    sfPopup.HeaderTitle = localization.GetString("Title");
    sfPopup.Message = localization.GetString("Message");
    sfPopup.AcceptButtonText = localization.GetString("AcceptButtonText");
    sfPopup.DeclineButtonText = localization.GetString("DeclineButtonText");
    
    sfPopup.Show();
}
```

**4. Change Language Dynamically:**

```csharp
private void ChangeLanguage(string languageCode)
{
    CultureInfo.CurrentUICulture = new CultureInfo(languageCode);
    
    // Refresh popup text
    ShowLocalizedPopup();
}
```

## Liquid Glass Effect

Create a modern, translucent glass-like effect for the popup.

**XAML with Glass Effect:**
```xml
<sfPopup:SfPopup x:Name="glassPopup"
                 ShowHeader="False"
                 EnableLiquidGlassEffect="True" 
                 ShowFooter="False">    
</sfPopup:SfPopup>
```
Supported on macOS 26 or higher and iOS 26 or higher. This feature is available only in .NET 10.

## Best Practices

### 1. Performance Optimization

```csharp
// Reuse popup instances instead of creating new ones
private SfPopup _cachedPopup;

private SfPopup GetPopupInstance()
{
    if (_cachedPopup == null)
    {
        _cachedPopup = new SfPopup
        {
            ShowFooter = true,
            AppearanceMode = PopupButtonAppearanceMode.TwoButton
        };
    }
    return _cachedPopup;
}
```

### 2. Memory Management

```csharp
protected override void OnDisappearing()
{
    base.OnDisappearing();
    
    // Unsubscribe from events
    sfPopup.Opening -= OnPopupOpening;
    sfPopup.Closed -= OnPopupClosed;
    
    // Clear bindings if needed
    sfPopup.BindingContext = null;
}
```

### 3. Accessibility

```xml
<sfPopup:SfPopup x:Name="accessiblePopup"
                 AutomationId="MainPopup"
                 SemanticProperties.Description="Alert dialog">
    <sfPopup:SfPopup.ContentTemplate>
        <DataTemplate>
            <Label Text="Important message"
                   SemanticProperties.HeadingLevel="Level1" />
        </DataTemplate>
    </sfPopup:SfPopup.ContentTemplate>
</sfPopup:SfPopup>
```

### 4. Error Handling

```csharp
private async void ShowDataPopup()
{
    try
    {
        sfPopup.Show();
        
        var data = await LoadDataAsync();
        sfPopup.BindingContext = data;
    }
    catch (Exception ex)
    {
        sfPopup.Dismiss();
        await DisplayAlert("Error", $"Failed to load data: {ex.Message}", "OK");
    }
}
```

### 5. Testing Support

```csharp
// Make popups testable
public interface IPopupService
{
    Task<bool> ShowConfirmationAsync(string title, string message);
}

public class PopupService : IPopupService
{
    public async Task<bool> ShowConfirmationAsync(string title, string message)
    {
        var popup = new SfPopup
        {
            HeaderTitle = title,
            Message = message,
            ShowFooter = true,
            AppearanceMode = PopupButtonAppearanceMode.TwoButton
        };
        
        return await popup.ShowAsync();
    }
}
```

### 6. Responsive Design

```csharp
private void ConfigureResponsivePopup()
{
    var displayInfo = DeviceDisplay.MainDisplayInfo;
    var screenWidth = displayInfo.Width / displayInfo.Density;
    var screenHeight = displayInfo.Height / displayInfo.Density;
    
    if (screenWidth < 600) // Mobile
    {
        sfPopup.WidthRequest = screenWidth * 0.9;
        sfPopup.HeightRequest = screenHeight * 0.7;
    }
    else // Tablet/Desktop
    {
        sfPopup.WidthRequest = 500;
        sfPopup.HeightRequest = 400;
    }
}
```

### 7. Complex Content Loading

```csharp
private async void ShowComplexPopup()
{
    // Show popup with loading indicator first
    sfPopup.ContentTemplate = new DataTemplate(() => 
        new ActivityIndicator { IsRunning = true });
    sfPopup.Show();
    
    // Load complex content asynchronously
    var complexView = await LoadComplexViewAsync();
    
    // Update popup content
    sfPopup.ContentTemplate = new DataTemplate(() => complexView);
}
```
