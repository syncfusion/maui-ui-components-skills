# Getting Started with .NET MAUI DatePicker

This guide covers the complete setup and basic implementation of the Syncfusion .NET MAUI DatePicker (SfDatePicker) control.

## Step 1: Create a New .NET MAUI Project

1. Go to **File > New > Project** and choose the **.NET MAUI App** template
2. Name the project and choose a location. Then click **Next**
3. Select the .NET framework version and click **Create**

## Step 2: Install the Syncfusion .NET MAUI Picker NuGet Package

### Option A: NuGet Package Manager

1. In **Solution Explorer**, right-click the project and choose **Manage NuGet Packages**
2. Search for `Syncfusion.Maui.Picker` and install the latest version
3. Ensure the necessary dependencies are installed correctly, and the project is restored

### Option B: Package Manager Console

```powershell
Install-Package Syncfusion.Maui.Picker
```

### Option C: .NET CLI

```bash
dotnet add package Syncfusion.Maui.Picker
```

## Step 3: Register the Handler

The `Syncfusion.Maui.Core` NuGet is a dependent package for all Syncfusion controls of .NET MAUI. In the **MauiProgram.cs** file, register the handler for Syncfusion core.

```csharp
using Syncfusion.Maui.Core.Hosting;

namespace GettingStarted
{
    public static class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            var builder = MauiApp.CreateBuilder();

            builder.ConfigureSyncfusionCore(); // Register Syncfusion handler
            
            builder
                .UseMauiApp<App>()
                .ConfigureFonts(fonts =>
                {
                    fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
                });

            return builder.Build();
        }
    }
}
```

## Step 4: Add .NET MAUI DatePicker Control

### XAML Implementation

1. Import the `Syncfusion.Maui.Picker` namespace
2. Initialize `SfDatePicker`

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:picker="clr-namespace:Syncfusion.Maui.Picker;assembly=Syncfusion.Maui.Picker"
             x:Class="MyApp.MainPage">
    
    <picker:SfDatePicker x:Name="datePicker" />
    
</ContentPage>
```

### C# Implementation

```csharp
using Syncfusion.Maui.Picker;

namespace MyApp
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            
            SfDatePicker picker = new SfDatePicker();
            this.Content = picker;
        }
    }
}
```

## Setting Header

Add header text to the DatePicker using the `Text` property in `PickerHeaderView`. Enable the header by setting the `Height` property to a value greater than 0.

### XAML

```xml
<picker:SfDatePicker x:Name="datePicker">
    <picker:SfDatePicker.HeaderView>
        <picker:PickerHeaderView Text="Date Picker" Height="40" />
    </picker:SfDatePicker.HeaderView>
</picker:SfDatePicker>
```

### C#

```csharp
SfDatePicker datePicker = new SfDatePicker();
datePicker.HeaderView = new PickerHeaderView()
{
    Text = "Date Picker",
    Height = 40,
};

this.Content = datePicker;
```

## Setting Footer

Customize validation buttons (OK and Cancel) using `OkButtonText` and `CancelButtonText` properties in `PickerFooterView`. Enable the OK button using the `ShowOkButton` property.

### XAML

```xml
<picker:SfDatePicker x:Name="datePicker">
    <picker:SfDatePicker.FooterView>
        <picker:PickerFooterView ShowOkButton="True" Height="40" />
    </picker:SfDatePicker.FooterView>
</picker:SfDatePicker>
```

### C#

```csharp
SfDatePicker datePicker = new SfDatePicker();
datePicker.FooterView = new PickerFooterView()
{  
    ShowOkButton = true,
    Height = 40,
};

this.Content = datePicker;
```

## Setting Height and Width

Customize the height and width of the DatePicker using the `HeightRequest` and `WidthRequest` properties.

### XAML

```xml
<picker:SfDatePicker x:Name="datePicker" 
                     HeightRequest="280" 
                     WidthRequest="300">
</picker:SfDatePicker>
```

### C#

```csharp
SfDatePicker datePicker = new SfDatePicker()
{
    HeightRequest = 280,
    WidthRequest = 300,
};

this.Content = datePicker;
```

## Setting Selected Date

Select a date using the `SelectedDate` property. The default value is the current date.

### XAML

```xml
<picker:SfDatePicker x:Name="datePicker" 
                     SelectedDate="9/7/2023">
</picker:SfDatePicker>
```

### C#

```csharp
SfDatePicker datePicker = new SfDatePicker()
{
    SelectedDate = new DateTime(2023, 9, 7).Date,
};

this.Content = datePicker;
```

## Clear Selection

Clear the selected date by setting the `SelectedDate` property to `null`.

### XAML

```xml
<picker:SfDatePicker x:Name="datePicker" />
```

### C# (Code-behind)

```csharp
this.datePicker.SelectedDate = null;
```

## Complete Example

Here's a complete example combining all the basic features:

### XAML

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:picker="clr-namespace:Syncfusion.Maui.Picker;assembly=Syncfusion.Maui.Picker"
             x:Class="MyApp.MainPage">
    
    <StackLayout Padding="20">
        <Label Text="Select Appointment Date" 
               FontSize="18" 
               Margin="0,0,0,10"/>
        
        <picker:SfDatePicker x:Name="datePicker"
                             HeightRequest="280"
                             WidthRequest="300"
                             SelectedDate="2023/09/15">
            <picker:SfDatePicker.HeaderView>
                <picker:PickerHeaderView Text="Date Picker" Height="40" />
            </picker:SfDatePicker.HeaderView>
            
            <picker:SfDatePicker.FooterView>
                <picker:PickerFooterView ShowOkButton="True" 
                                         Height="40" 
                                         OkButtonText="OK"
                                         CancelButtonText="Cancel"/>
            </picker:SfDatePicker.FooterView>
        </picker:SfDatePicker>
        
        <Button Text="Clear Date" 
                Clicked="ClearDate_Clicked" 
                Margin="0,20,0,0"/>
        
        <Label x:Name="selectedDateLabel" 
               Text="No date selected" 
               Margin="0,20,0,0"/>
    </StackLayout>
    
</ContentPage>
```

### C# Code-behind

```csharp
using Syncfusion.Maui.Picker;

namespace MyApp
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            
            // Subscribe to SelectionChanged event
            datePicker.SelectionChanged += OnDatePickerSelectionChanged;
        }
        
        private void OnDatePickerSelectionChanged(object sender, DatePickerSelectionChangedEventArgs e)
        {
            if (e.NewValue != null)
            {
                selectedDateLabel.Text = $"Selected Date: {e.NewValue:dd/MM/yyyy}";
            }
            else
            {
                selectedDateLabel.Text = "No date selected";
            }
        }
        
        private void ClearDate_Clicked(object sender, EventArgs e)
        {
            datePicker.SelectedDate = null;
        }
    }
}
```

## Video Tutorial

To get started quickly with .NET MAUI DatePicker, check out the [official video tutorial](https://youtu.be/PeQf-5pPCWo?si=KlwcdMcLN634G_AA).

## Next Steps

- **Formatting** - Learn about date format options (20+ predefined formats)
- **Picker Modes** - Explore Dialog and RelativeDialog display modes
- **Customization** - Customize headers, footers, and selection views
- **Date Restrictions** - Set min/max dates and blackout dates
- **Events** - Handle SelectionChanged and other events

## Common Issues

### DatePicker not visible
**Problem:** The DatePicker doesn't appear in the UI.
**Solution:** Ensure `ConfigureSyncfusionCore()` is called in MauiProgram.cs before the app is built.

### NuGet package conflicts
**Problem:** Build errors related to package versions.
**Solution:** Ensure all Syncfusion packages are the same version. Update all packages to the latest version.

### SelectedDate not updating
**Problem:** The SelectedDate property doesn't change when selecting a date.
**Solution:** When using Dialog mode with ShowOkButton enabled, the SelectedDate is confirmed only when the OK button is tapped. Set `IsSelectionImmediate="True"` for immediate updates.

## Additional Resources

- [Official Documentation](https://help.syncfusion.com/maui/datepicker/getting-started)
- [API Reference](https://help.syncfusion.com/cr/maui/Syncfusion.Maui.Picker.SfDatePicker.html)
- [Sample Browser](https://github.com/syncfusion/maui-demos/tree/master/MAUI/Picker)
