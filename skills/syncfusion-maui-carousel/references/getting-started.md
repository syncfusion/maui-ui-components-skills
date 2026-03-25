# Getting Started with .NET MAUI Carousel

This guide walks through the complete setup and basic implementation of the Syncfusion .NET MAUI Carousel (SfCarousel) control.

## Installation Steps

### Step 1: Create a New MAUI Project

**Visual Studio 2026:**
1. Go to **File > New > Project**
2. Choose the **.NET MAUI App** template
3. Name the project and choose a location
4. Click **Next**, select the .NET framework version, and click **Create**

**Visual Studio Code:**
1. Press `Ctrl+Shift+P` and type `.NET:New Project`
2. Choose the **.NET MAUI App** template
3. Select project location, type the project name, and press Enter
4. Choose **Create project**

**Command Line:**
```bash
dotnet new maui -n MyCarouselApp
cd MyCarouselApp
```

### Step 2: Install the Syncfusion MAUI Carousel NuGet Package

**Option 1 - NuGet Package Manager (Visual Studio):**
1. Right-click the project in **Solution Explorer**
2. Choose **Manage NuGet Packages**
3. Search for `Syncfusion.Maui.Carousel`
4. Install the latest version

**Option 2 - Package Manager Console:**
```powershell
Install-Package Syncfusion.Maui.Carousel
```

**Option 3 - .NET CLI:**
```bash
dotnet add package Syncfusion.Maui.Carousel
```

**Option 4 - Direct .csproj Edit:**
```xml
<ItemGroup>
  <PackageReference Include="Syncfusion.Maui.Carousel" Version="*" />
</ItemGroup>
```

After installation, run:
```bash
dotnet restore
```

### Step 3: Register the Handler

The `Syncfusion.Maui.Core` NuGet package is a required dependency for all Syncfusion .NET MAUI controls. Register the handler in `MauiProgram.cs`:

```csharp
using Microsoft.Maui;
using Microsoft.Maui.Hosting;
using Microsoft.Maui.Controls.Hosting;
using Syncfusion.Maui.Core.Hosting;

namespace CarouselSample
{
    public static class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            var builder = MauiApp.CreateBuilder();
            builder
                .UseMauiApp<App>()
                .ConfigureSyncfusionCore()  // ← Register Syncfusion handler
                .ConfigureFonts(fonts =>
                {
                    fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
                });

            return builder.Build();
        }
    }
}
```

**Key Point:** The `.ConfigureSyncfusionCore()` call is mandatory and must be added before the app runs.

### Step 4: Add Namespace

Add the carousel namespace to your XAML or C# file.

**XAML:**
```xml
xmlns:carousel="clr-namespace:Syncfusion.Maui.Carousel;assembly=Syncfusion.Maui.Carousel"
```

**C#:**
```csharp
using Syncfusion.Maui.Carousel;
```

### Step 5: Add a Basic Carousel

**XAML:**
```xml
<carousel:SfCarousel />
```

**C#:**
```csharp
SfCarousel carousel = new SfCarousel();
this.Content = carousel;
```

## Adding Carousel Items

There are two main approaches to populate carousel items:

### Approach 1: Through Data Binding (Recommended)

Create a model and ViewModel, then bind to the carousel.

**1. Create the Model:**
```csharp
public class CarouselModel
{
    public string Image { get; set; }

    public CarouselModel(string imageString)
    {
        Image = imageString;
    }
}
```

**2. Create the ViewModel:**
```csharp
using System.Collections.Generic;

public class CarouselViewModel
{
    public List<CarouselModel> ImageCollection { get; set; }

    public CarouselViewModel()
    {
        ImageCollection = new List<CarouselModel>
        {
            new CarouselModel("carousel_person1.png"),
            new CarouselModel("carousel_person2.png"),
            new CarouselModel("carousel_person3.png"),
            new CarouselModel("carousel_person4.png"),
            new CarouselModel("carousel_person5.png")
        };
    }
}
```

> **Note:** Image files should be added to the `Resources/Images` folder of your MAUI project.

**3. Bind to Carousel (XAML):**
```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:carousel="clr-namespace:Syncfusion.Maui.Carousel;assembly=Syncfusion.Maui.Carousel"
             xmlns:local="clr-namespace:CarouselSample"
             x:Class="CarouselSample.MainPage">
             
    <ContentPage.BindingContext>
        <local:CarouselViewModel/>
    </ContentPage.BindingContext>

    <ContentPage.Resources>
        <ResourceDictionary>
            <DataTemplate x:Key="itemTemplate">
                <Image Source="{Binding Image}" 
                       Aspect="AspectFit"/>
            </DataTemplate>
        </ResourceDictionary>
    </ContentPage.Resources>
    
    <ContentPage.Content>
        <carousel:SfCarousel x:Name="carousel"  
                             ItemTemplate="{StaticResource itemTemplate}" 
                             ItemsSource="{Binding ImageCollection}" />
    </ContentPage.Content>
</ContentPage>
```

**3. Bind to Carousel (C#):**
```csharp
using Syncfusion.Maui.Carousel;

namespace CarouselSample
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            
            var carouselViewModel = new CarouselViewModel();

            var carousel = new SfCarousel();

            var itemTemplate = new DataTemplate(() =>
            {
                var image = new Image();
                image.SetBinding(Image.SourceProperty, "Image");
                return image;
            });

            carousel.BindingContext = carouselViewModel;
            carousel.ItemTemplate = itemTemplate;
            carousel.SetBinding(SfCarousel.ItemsSourceProperty, "ImageCollection");

            this.Content = carousel;
        }
    }
}
```

### Approach 2: Through SfCarouselItem

Directly create carousel items without data binding.

```csharp
using Syncfusion.Maui.Carousel;
using System.Collections.ObjectModel;

namespace CarouselSample
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            
            var carousel = new SfCarousel();

            var carouselItems = new ObservableCollection<SfCarouselItem>
            {
                new SfCarouselItem { ImageName = "carousel_person1.png" },
                new SfCarouselItem { ImageName = "carousel_person2.png" },
                new SfCarouselItem { ImageName = "carousel_person3.png" },
                new SfCarouselItem { ImageName = "carousel_person4.png" },
                new SfCarouselItem { ImageName = "carousel_person5.png" }
            };

            carousel.ItemsSource = carouselItems;

            this.Content = carousel;
        }
    }
}
```

## Setting Item Dimensions

Use `ItemHeight` and `ItemWidth` properties to control the size of carousel items.

**XAML:**
```xml
<carousel:SfCarousel x:Name="carousel"
                     ItemTemplate="{StaticResource itemTemplate}" 
                     ItemsSource="{Binding ImageCollection}"
                     ItemHeight="170"
                     ItemWidth="270"/>
```

**C#:**
```csharp
var carousel = new SfCarousel
{
    ItemHeight = 170,
    ItemWidth = 270
};

carousel.ItemTemplate = itemTemplate;
carousel.SetBinding(SfCarousel.ItemsSourceProperty, "ImageCollection");
```

**Guidance:**
- **Mobile devices:** ItemHeight: 150-200, ItemWidth: 200-300
- **Tablets:** ItemHeight: 200-300, ItemWidth: 300-400
- **Desktop:** ItemHeight: 250-400, ItemWidth: 400-600

## Setting the Selected Item

Use the `SelectedIndex` property to specify which item should be selected initially or programmatically.

**XAML:**
```xml
<carousel:SfCarousel x:Name="carousel"
                     ItemTemplate="{StaticResource itemTemplate}" 
                     ItemsSource="{Binding ImageCollection}"
                     ItemHeight="170"
                     ItemWidth="270"
                     SelectedIndex="2"/>
```

**C#:**
```csharp
var carousel = new SfCarousel
{
    ItemHeight = 170,
    ItemWidth = 270,
    SelectedIndex = 2  // Zero-based index (3rd item)
};
```

**Default:** `SelectedIndex = 0` (first item)

## Complete Working Example

```csharp
// Model
public class CarouselModel
{
    public string Image { get; set; }
    public CarouselModel(string imageString) => Image = imageString;
}

// ViewModel
public class CarouselViewModel
{
    public List<CarouselModel> ImageCollection { get; set; }
    
    public CarouselViewModel()
    {
        ImageCollection = new List<CarouselModel>
        {
            new CarouselModel("carousel_person1.png"),
            new CarouselModel("carousel_person2.png"),
            new CarouselModel("carousel_person3.png"),
            new CarouselModel("carousel_person4.png"),
            new CarouselModel("carousel_person5.png")
        };
    }
}
```

```xml
<!-- MainPage.xaml -->
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:carousel="clr-namespace:Syncfusion.Maui.Carousel;assembly=Syncfusion.Maui.Carousel"
             xmlns:local="clr-namespace:CarouselSample"
             x:Class="CarouselSample.MainPage">
             
    <ContentPage.BindingContext>
        <local:CarouselViewModel/>
    </ContentPage.BindingContext>

    <ContentPage.Resources>
        <DataTemplate x:Key="itemTemplate">
            <Image Source="{Binding Image}" Aspect="AspectFit"/>
        </DataTemplate>
    </ContentPage.Resources>
    
    <carousel:SfCarousel ItemsSource="{Binding ImageCollection}"
                         ItemTemplate="{StaticResource itemTemplate}"
                         ItemHeight="170"
                         ItemWidth="270"
                         SelectedIndex="0"
                         HeightRequest="400"
                         WidthRequest="800"/>
</ContentPage>
```

## Troubleshooting

**Issue: Handler not registered error**
- **Solution:** Ensure `.ConfigureSyncfusionCore()` is called in `MauiProgram.cs`

**Issue: Items not displaying**
- **Solution:** Verify ItemsSource contains data and ItemTemplate is properly defined

**Issue: Images not loading**
- **Solution:** Check that image files are in `Resources/Images` folder and Build Action is set to `MauiImage`

**Issue: NuGet restore fails**
- **Solution:** Run `dotnet restore` or rebuild the project

## Next Steps

- Learn about populating data in different ways → [populating-data.md](populating-data.md)
- Explore view modes and layout options → [view-modes.md](view-modes.md)
- Customize animations → [animation.md](animation.md)
