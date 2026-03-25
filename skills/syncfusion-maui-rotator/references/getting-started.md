# Getting Started with .NET MAUI Rotator

This guide covers the installation, and basic setup for implementing the Syncfusion .NET MAUI Rotator (SfRotator) control in your application.

## Step 1: Install NuGet Package

### Using Visual Studio

1. Right-click your project in Solution Explorer
2. Select **Manage NuGet Packages**
3. Search for **Syncfusion.Maui.Rotator**
4. Install the latest stable version
5. Accept the license agreement if prompted

### Using Package Manager Console

```bash
Install-Package Syncfusion.Maui.Rotator
```

### Using .NET CLI

```bash
dotnet add package Syncfusion.Maui.Rotator
```

### Using Visual Studio Code

```bash
dotnet add package Syncfusion.Maui.Rotator
dotnet restore
```

## Step 2: Register the Handler

**Critical Step:** The Syncfusion.Maui.Core package (a dependency) requires handler registration.

Open `MauiProgram.cs` and add the ConfigureSyncfusionCore() call:

```csharp
using Microsoft.Maui;
using Microsoft.Maui.Hosting;
using Microsoft.Maui.Controls.Hosting;
using Syncfusion.Maui.Core.Hosting;  // Add this namespace

namespace YourApp
{
    public static class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            var builder = MauiApp.CreateBuilder();
            builder
                .UseMauiApp<App>()
                .ConfigureSyncfusionCore()  // Register Syncfusion handlers
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

**Important:** Without this registration, you'll encounter runtime errors when using SfRotator.

## Step 3: Add Namespace

Add the SfRotator namespace to your XAML or C# file:

**XAML:**
```xml
xmlns:syncfusion="clr-namespace:Syncfusion.Maui.Rotator;assembly=Syncfusion.Maui.Rotator"
```

**C#:**
```csharp
using Syncfusion.Maui.Rotator;
```

## Step 4: Basic Implementation

### Minimal XAML Example

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:syncfusion="clr-namespace:Syncfusion.Maui.Rotator;assembly=Syncfusion.Maui.Rotator"
             x:Class="MyApp.MainPage">
    <ContentPage.Content>
        <syncfusion:SfRotator x:Name="rotator" />
    </ContentPage.Content>
</ContentPage>
```

### Minimal C# Example

```csharp
using Syncfusion.Maui.Rotator;

namespace MyApp
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            
            SfRotator rotator = new SfRotator();
            this.Content = rotator;
        }
    }
}
```

## Step 5: Adding Images

### Prepare Images

1. Place images in the `Resources/Images` folder of your project
2. Ensure Build Action is set to **MauiImage**
3. Supported formats: PNG, JPG, SVG, GIF

Example structure:
```
YourProject/
└── Resources/
    └── Images/
        ├── image1.png
        ├── image2.png
        ├── image3.png
        └── image4.png
```

### Using SfRotatorItem (Simple Approach)

```csharp
using Syncfusion.Maui.Rotator;

public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        
        SfRotator rotator = new SfRotator();
        
        List<SfRotatorItem> items = new List<SfRotatorItem>
        {
            new SfRotatorItem { Image = "image1.png" },
            new SfRotatorItem { Image = "image2.png" },
            new SfRotatorItem { Image = "image3.png" },
            new SfRotatorItem { Image = "image4.png" },
            new SfRotatorItem { Image = "image5.png" }
        };
        
        rotator.ItemsSource = items;
        rotator.HeightRequest = 300;
        
        this.Content = rotator;
    }
}
```

## Complete Working Example

### XAML with ViewModel Binding

**MainPage.xaml:**
```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:syncfusion="clr-namespace:Syncfusion.Maui.Rotator;assembly=Syncfusion.Maui.Rotator"
             xmlns:local="clr-namespace:MyApp"
             x:Class="MyApp.MainPage">
    <ContentPage.BindingContext>
        <local:RotatorViewModel/>
    </ContentPage.BindingContext>
    
    <ContentPage.Content>
        <syncfusion:SfRotator ItemsSource="{Binding ImageCollection}"
                              NavigationStripMode="Dots"
                              NavigationStripPosition="Bottom"
                              BackgroundColor="#ececec"
                              HeightRequest="400"
                              WidthRequest="400">
            <syncfusion:SfRotator.ItemTemplate>
                <DataTemplate>
                    <Image Source="{Binding Image}" 
                           HorizontalOptions="Center"
                           VerticalOptions="Center"/>
                </DataTemplate>
            </syncfusion:SfRotator.ItemTemplate>
        </syncfusion:SfRotator>
    </ContentPage.Content>
</ContentPage>
```

**RotatorViewModel.cs:**
```csharp
using System.Collections.ObjectModel;

namespace MyApp
{
    public class RotatorViewModel
    {
        public ObservableCollection<RotatorModel> ImageCollection { get; set; }
        
        public RotatorViewModel()
        {
            ImageCollection = new ObservableCollection<RotatorModel>
            {
                new RotatorModel("image1.png"),
                new RotatorModel("image2.png"),
                new RotatorModel("image3.png"),
                new RotatorModel("image4.png"),
                new RotatorModel("image5.png")
            };
        }
    }
}
```

**RotatorModel.cs:**
```csharp
namespace MyApp
{
    public class RotatorModel
    {
        public string Image { get; set; }
        
        public RotatorModel(string image)
        {
            Image = image;
        }
    }
}
```

## Troubleshooting

### Issue: "Handler not found" Error

**Solution:** Ensure `ConfigureSyncfusionCore()` is called in MauiProgram.cs before `UseMauiApp<App>()`

### Issue: Images Not Displaying

**Check:**
1. Images exist in `Resources/Images` folder
2. Build Action is set to `MauiImage`
3. File names match exactly (case-sensitive)
4. Image paths don't include "Resources/Images/" prefix (just filename)

### Issue: NuGet Package Not Installing

**Solution:**
- Clear NuGet cache: `dotnet nuget locals all --clear`
- Restart Visual Studio
- Check internet connection and NuGet.org availability
- Verify project targets .NET 9 or later

### Issue: Rotator Not Visible

**Check:**
1. HeightRequest or WidthRequest is set
2. Parent container has sufficient space
3. ItemsSource is not null or empty
4. ItemTemplate is properly configured

## Next Steps

- **Data Binding:** Learn advanced data binding techniques in `populating-data.md`
- **Navigation:** Configure navigation modes and styling in `navigation-modes.md`
- **Customization:** Explore layout and appearance options in other reference files
