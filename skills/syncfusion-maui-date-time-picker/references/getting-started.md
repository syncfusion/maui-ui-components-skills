# Getting Started with DateTimePicker

This guide walks you through setting up and implementing the Syncfusion .NET MAUI DateTimePicker (SfDateTimePicker) control in your application.

## Step 1: Create a New .NET MAUI Project

### Using Visual Studio

1. Open Visual Studio 2022 (v17.13+)
2. Go to **File > New > Project**
3. Select **.NET MAUI App** template
4. Name your project (e.g., "DateTimePickerApp")
5. Choose a location and click **Next**
6. Select the .NET framework version (.NET 9 or later)
7. Click **Create**

### Using Command Line

```bash
dotnet new maui -n DateTimePickerApp
cd DateTimePickerApp
```

## Step 2: Install the Syncfusion.Maui.Picker NuGet Package

### Using Visual Studio

1. In **Solution Explorer**, right-click the project
2. Choose **Manage NuGet Packages**
3. Select the **Browse** tab
4. Search for `Syncfusion.Maui.Picker`
5. Select the package and click **Install**
6. Accept the license agreement
7. Wait for installation to complete

### Using .NET CLI

```bash
dotnet add package Syncfusion.Maui.Picker
```

### Using Package Manager Console

```powershell
Install-Package Syncfusion.Maui.Picker
```

**Note**: The `Syncfusion.Maui.Core` package is automatically installed as a dependency.

## Step 3: Register the Syncfusion Handler

The Syncfusion.Maui.Core NuGet package is a required dependency for all Syncfusion .NET MAUI controls. You must register the handler in `MauiProgram.cs`.

### Update MauiProgram.cs

Open `MauiProgram.cs` and modify it as follows:

```csharp
using Syncfusion.Maui.Core.Hosting;

namespace DateTimePickerApp
{
    public static class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            var builder = MauiApp.CreateBuilder();
            
            builder
                .UseMauiApp<App>()
                .ConfigureSyncfusionCore()  // Register Syncfusion handler
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

**Key changes:**
- Add `using Syncfusion.Maui.Core.Hosting;`
- Call `.ConfigureSyncfusionCore()` after `.UseMauiApp<App>()`

## Step 4: Add DateTimePicker to Your Page

### XAML Implementation

Open `MainPage.xaml` and add the DateTimePicker:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:picker="clr-namespace:Syncfusion.Maui.Picker;assembly=Syncfusion.Maui.Picker"
             x:Class="DateTimePickerApp.MainPage">

    <VerticalStackLayout Padding="30,0" Spacing="25">
        
        <Label 
            Text="Select Date and Time"
            FontSize="18"
            FontAttributes="Bold"
            HorizontalOptions="Center" />
        
        <picker:SfDateTimePicker 
            x:Name="dateTimePicker"
            HorizontalOptions="Center" />
        
        <Button 
            Text="Get Selected DateTime"
            Clicked="OnGetDateTimeClicked"
            HorizontalOptions="Center" />
        
        <Label 
            x:Name="resultLabel"
            HorizontalOptions="Center" />
            
    </VerticalStackLayout>

</ContentPage>
```

**Key steps:**
1. Add namespace: `xmlns:picker="clr-namespace:Syncfusion.Maui.Picker;assembly=Syncfusion.Maui.Picker"`
2. Use the picker: `<picker:SfDateTimePicker />`

### Code-Behind (MainPage.xaml.cs)

```csharp
namespace DateTimePickerApp
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
        }

        private void OnGetDateTimeClicked(object sender, EventArgs e)
        {
            if (dateTimePicker.SelectedDate.HasValue)
            {
                resultLabel.Text = $"Selected: {dateTimePicker.SelectedDate.Value:f}";
            }
            else
            {
                resultLabel.Text = "No date selected";
            }
        }
    }
}
```

### Pure C# Implementation

Alternatively, you can create the DateTimePicker entirely in C#:

```csharp
using Syncfusion.Maui.Picker;

namespace DateTimePickerApp
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            
            var layout = new VerticalStackLayout
            {
                Padding = new Thickness(30, 0),
                Spacing = 25
            };

            var titleLabel = new Label
            {
                Text = "Select Date and Time",
                FontSize = 18,
                FontAttributes = FontAttributes.Bold,
                HorizontalOptions = LayoutOptions.Center
            };

            var dateTimePicker = new SfDateTimePicker
            {
                HorizontalOptions = LayoutOptions.Center,
                SelectedDate = DateTime.Now
            };

            var resultLabel = new Label
            {
                HorizontalOptions = LayoutOptions.Center
            };

            var button = new Button
            {
                Text = "Get Selected DateTime",
                HorizontalOptions = LayoutOptions.Center
            };

            button.Clicked += (s, e) =>
            {
                if (dateTimePicker.SelectedDate.HasValue)
                {
                    resultLabel.Text = $"Selected: {dateTimePicker.SelectedDate.Value:f}";
                }
            };

            layout.Children.Add(titleLabel);
            layout.Children.Add(dateTimePicker);
            layout.Children.Add(button);
            layout.Children.Add(resultLabel);

            Content = layout;
        }
    }
}
```

## Step 5: Run the Application

### Visual Studio
1. Select target platform (Android, iOS, Windows, macOS)
2. Press **F5** or click **Start Debugging**
3. The app will build and launch

### Command Line
```bash
# Windows
dotnet build -t:Run -f net9.0-windows10.0.19041.0

# Android
dotnet build -t:Run -f net9.0-android

# iOS (requires Mac)
dotnet build -t:Run -f net9.0-ios

# macOS
dotnet build -t:Run -f net9.0-maccatalyst
```

## Minimal Working Example

Here's the simplest possible implementation:

**XAML:**
```xaml
<ContentPage xmlns:picker="clr-namespace:Syncfusion.Maui.Picker;assembly=Syncfusion.Maui.Picker">
    <picker:SfDateTimePicker />
</ContentPage>
```

**C#:**
```csharp
using Syncfusion.Maui.Picker;

Content = new SfDateTimePicker();
```

## Setting Initial Value

Set the initial selected date and time:

```xaml
<picker:SfDateTimePicker 
    SelectedDate="2024-03-15 14:30:00" />
```

```csharp
var picker = new SfDateTimePicker
{
    SelectedDate = new DateTime(2024, 3, 15, 14, 30, 0)
};
```

## Common Initial Configuration

A typical configuration might look like:

```xaml
<picker:SfDateTimePicker 
    x:Name="appointmentPicker"
    SelectedDate="{Binding AppointmentDateTime}"
    MinimumDate="{Binding Today}"
    DateFormat="dd_MMM_yyyy"
    TimeFormat="h_mm_tt"
    SelectionChanged="OnDateTimeSelectionChanged" />
```

## Troubleshooting

### Issue: Control Not Appearing
**Solution**: Ensure you called `.ConfigureSyncfusionCore()` in `MauiProgram.cs`

### Issue: Namespace Not Recognized
**Solution**: Verify the NuGet package is installed and the project is restored:
```bash
dotnet restore
```

### Issue: Build Errors After Installation
**Solution**: Clean and rebuild:
```bash
dotnet clean
dotnet build
```

### Issue: Handler Not Registered Error
**Solution**: Add the using statement and handler registration:
```csharp
using Syncfusion.Maui.Core.Hosting;
builder.ConfigureSyncfusionCore();
```

## Next Steps

Now that you have a basic DateTimePicker working:

1. **Formatting**: Customize date and time display formats
2. **Picker Modes**: Use Dialog or RelativeDialog for better UX
3. **Customization**: Style headers, footers, and selection views
4. **Date Restrictions**: Set min/max dates and blackout periods
5. **Intervals**: Configure time slots and date intervals
6. **Events**: Handle selection changes and dialog events
7. **Localization**: Support multiple languages