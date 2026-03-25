# Getting Started with .NET MAUI TimePicker

## Table of Contents
- [Step 1: Create a New .NET MAUI Project](#step-1-create-a-new-net-maui-project)
- [Step 2: Install the NuGet Package](#step-2-install-the-nuget-package)
- [Step 3: Register the Handler](#step-3-register-the-handler)
- [Step 4: Add the TimePicker to Your Page](#step-4-add-the-timepicker-to-your-page)
- [Basic Implementation Examples](#basic-implementation-examples)
- [Troubleshooting](#troubleshooting)

This guide covers the installation, and basic implementation of the Syncfusion .NET MAUI TimePicker (SfTimePicker) control.

## Step 1: Create a New .NET MAUI Project

### Using Visual Studio
1. Go to **File > New > Project**
2. Choose the **.NET MAUI App** template
3. Name the project and choose a location
4. Click **Next**
5. Select the .NET framework version (.NET 9 or later)
6. Click **Create**

### Using .NET CLI
```bash
dotnet new maui -n MyTimePickerApp
cd MyTimePickerApp
```

## Step 2: Install Syncfusion .NET MAUI Picker NuGet Package

### Using Visual Studio
1. In **Solution Explorer**, right-click the project
2. Choose **Manage NuGet Packages**
3. Search for **[Syncfusion.Maui.Picker](https://www.nuget.org/packages/Syncfusion.Maui.Picker/)**
4. Install the latest version
5. Ensure dependencies are installed correctly and project is restored

### Using .NET CLI
```bash
dotnet add package Syncfusion.Maui.Picker
```

### Using Package Manager Console
```powershell
Install-Package Syncfusion.Maui.Picker
```

## Step 3: Register the Syncfusion Handler

The **Syncfusion.Maui.Core** NuGet package is a dependency for all Syncfusion controls. You must register the handler in **MauiProgram.cs**.

**MauiProgram.cs:**
```csharp
using Syncfusion.Maui.Core.Hosting;

namespace MyTimePickerApp
{
    public static class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            var builder = MauiApp.CreateBuilder();

            // Register Syncfusion Core handler (REQUIRED)
            builder.ConfigureSyncfusionCore();
            
            builder
                .UseMauiApp<App>()
                .ConfigureFonts(fonts =>
                {
                    fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
                    fonts.AddFont("OpenSans-Semibold.ttf", "OpenSansSemibold");
                });

            return builder.Build();
        }
    }
}
```

**Important:** Without `ConfigureSyncfusionCore()`, the TimePicker control will not render properly.

## Step 4: Add SfTimePicker Control

### Import the Namespace

**XAML:**
```xml
xmlns:picker="clr-namespace:Syncfusion.Maui.Picker;assembly=Syncfusion.Maui.Picker"
```

**C#:**
```csharp
using Syncfusion.Maui.Picker;
```

### Basic Implementation

**XAML (MainPage.xaml):**
```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:picker="clr-namespace:Syncfusion.Maui.Picker;assembly=Syncfusion.Maui.Picker"
             x:Class="MyTimePickerApp.MainPage">

    <picker:SfTimePicker x:Name="timePicker" />

</ContentPage>
```

**C# (MainPage.xaml.cs):**
```csharp
using Syncfusion.Maui.Picker;

namespace MyTimePickerApp
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            
            SfTimePicker timePicker = new SfTimePicker();
            this.Content = timePicker;
        }
    }
}
```

## Setting Header View

Add and customize header text using the `PickerHeaderView`.

**XAML:**
```xml
<picker:SfTimePicker x:Name="timePicker">
    <picker:SfTimePicker.HeaderView>
        <picker:PickerHeaderView Text="Select Time" Height="40" />
    </picker:SfTimePicker.HeaderView>
</picker:SfTimePicker>
```

**C#:**
```csharp
SfTimePicker timePicker = new SfTimePicker();
timePicker.HeaderView = new PickerHeaderView()
{
    Text = "Select Time",
    Height = 40
};

this.Content = timePicker;
```

**Properties:**
- `Text` (string) - Header text to display
- `Height` (double) - Header height (set > 0 to make visible)
- `Background` (Brush) - Header background color
- `TextStyle` (PickerTextStyle) - Font, size, color, attributes

## Setting Footer View

Add validation buttons (OK and Cancel) using the `PickerFooterView`.

**XAML:**
```xml
<picker:SfTimePicker x:Name="timePicker">
    <picker:SfTimePicker.FooterView>
        <picker:PickerFooterView ShowOkButton="True" 
                                 Height="40"
                                 OkButtonText="Confirm"
                                 CancelButtonText="Cancel" />
    </picker:SfTimePicker.FooterView>
</picker:SfTimePicker>
```

**C#:**
```csharp
SfTimePicker timePicker = new SfTimePicker();
timePicker.FooterView = new PickerFooterView()
{
    ShowOkButton = true,
    Height = 40,
    OkButtonText = "Confirm",
    CancelButtonText = "Cancel"
};

this.Content = timePicker;
```

**Properties:**
- `ShowOkButton` (bool) - Show/hide OK button
- `Height` (double) - Footer height (set > 0 to make visible)
- `OkButtonText` (string) - Text for OK button (default: "OK")
- `CancelButtonText` (string) - Text for Cancel button (default: "Cancel")
- `Background` (Brush) - Footer background color

## Setting Height and Width

Customize the picker dimensions:

**XAML:**
```xml
<picker:SfTimePicker x:Name="timePicker" 
                     HeightRequest="280" 
                     WidthRequest="300" />
```

**C#:**
```csharp
SfTimePicker timePicker = new SfTimePicker()
{
    HeightRequest = 280,
    WidthRequest = 300
};

this.Content = timePicker;
```

## Setting Selected Time

Use the `SelectedTime` property to get or set the selected time value.

**XAML:**
```xml
<picker:SfTimePicker x:Name="timePicker" 
                     SelectedTime="09:30:00" />
```

**C#:**
```csharp
SfTimePicker timePicker = new SfTimePicker()
{
    SelectedTime = new TimeSpan(9, 30, 0)  // 9:30:00 AM
};

this.Content = timePicker;
```

**Getting the selected time:**
```csharp
private void OnGetTimeClicked(object sender, EventArgs e)
{
    TimeSpan? selectedTime = timePicker.SelectedTime;
    
    if (selectedTime.HasValue)
    {
        DisplayAlert("Selected Time", 
                     selectedTime.Value.ToString(@"hh\:mm\:ss"), 
                     "OK");
    }
}
```

**Property Details:**
- **Type:** `TimeSpan?` (nullable TimeSpan)
- **Default:** Current system time
- **Bindable:** Yes (supports two-way binding)

## Clear Selection

Clear the selected time by setting `SelectedTime` to `null`:

**XAML:**
```xml
<StackLayout>
    <picker:SfTimePicker x:Name="timePicker" 
                         SelectedTime="09:30:00" />
    
    <Button Text="Clear Selection" 
            Clicked="OnClearClicked" />
</StackLayout>
```

**C#:**
```csharp
private void OnClearClicked(object sender, EventArgs e)
{
    timePicker.SelectedTime = null;
}
```

## Complete Example

**MainPage.xaml:**
```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:picker="clr-namespace:Syncfusion.Maui.Picker;assembly=Syncfusion.Maui.Picker"
             x:Class="MyTimePickerApp.MainPage">

    <StackLayout Padding="20" Spacing="20">
        
        <Label Text="Appointment Time Picker" 
               FontSize="20" 
               FontAttributes="Bold"
               HorizontalOptions="Center" />
        
        <picker:SfTimePicker x:Name="timePicker"
                             HeightRequest="280"
                             WidthRequest="300"
                             SelectedTime="09:00:00"
                             Format="hh_mm_tt"
                             HorizontalOptions="Center">
            
            <picker:SfTimePicker.HeaderView>
                <picker:PickerHeaderView Text="Select Appointment Time" 
                                         Height="40" />
            </picker:SfTimePicker.HeaderView>
            
            <picker:SfTimePicker.FooterView>
                <picker:PickerFooterView ShowOkButton="True" 
                                         Height="40"
                                         OkButtonText="Confirm"
                                         CancelButtonText="Cancel" />
            </picker:SfTimePicker.FooterView>
            
        </picker:SfTimePicker>
        
        <Button Text="Get Selected Time" 
                Clicked="OnGetTimeClicked" />
        
        <Button Text="Clear Selection" 
                Clicked="OnClearClicked" />
        
    </StackLayout>

</ContentPage>
```

**MainPage.xaml.cs:**
```csharp
using Syncfusion.Maui.Picker;

namespace MyTimePickerApp
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
        }

        private void OnGetTimeClicked(object sender, EventArgs e)
        {
            if (timePicker.SelectedTime.HasValue)
            {
                DisplayAlert("Selected Time", 
                             timePicker.SelectedTime.Value.ToString(@"hh\:mm\:ss tt"), 
                             "OK");
            }
            else
            {
                DisplayAlert("No Selection", "Please select a time", "OK");
            }
        }

        private void OnClearClicked(object sender, EventArgs e)
        {
            timePicker.SelectedTime = null;
        }
    }
}
```

## Common Issues and Solutions

### Issue: TimePicker not displaying
**Solution:** Ensure `ConfigureSyncfusionCore()` is called in MauiProgram.cs

### Issue: Header/Footer not visible
**Solution:** Set `Height` property > 0 for HeaderView and FooterView

### Issue: NuGet package errors
**Solution:** 
- Clean and rebuild the solution
- Delete `bin` and `obj` folders
- Restore NuGet packages: `dotnet restore`

### Issue: Namespace not recognized
**Solution:** Ensure Syncfusion.Maui.Picker NuGet package is installed and project is restored

## Next Steps

Now that you have basic TimePicker setup, explore:
- **Formatting** - Configure time display formats (12/24 hour, seconds, milliseconds)
- **Picker Modes** - Use Dialog or RelativeDialog modes
- **Time Intervals** - Set intervals for hours, minutes, seconds
- **Time Restrictions** - Limit selectable times with min/max and blackout times
- **Events** - Handle time selection changes
- **Customization** - Style header, footer, and selection views
