# Populating Data in .NET MAUI Rotator

## Table of Contents
- [Overview](#overview)
- [Data Source Options](#data-source-options)
- [Using SfRotatorItem](#using-sfrotatoritem)
- [Using ItemTemplate with Models](#using-itemtemplate-with-models)
- [Loading Online Images](#loading-online-images)
- [Observable Collections](#observable-collections)
- [Best Practices](#best-practices)

## Overview

The SfRotator control supports multiple approaches for populating data:

1. **SfRotatorItem Collection**: Simple approach using built-in item class
2. **ItemTemplate with Custom Models**: Flexible approach using DataTemplate and custom data models
3. **Online Images**: Loading images from HTTP/HTTPS URLs

## Data Source Options

The `ItemsSource` property accepts any `IEnumerable` collection:
- `List<T>`
- `ObservableCollection<T>`
- `Array`
- `IList`
- Custom collections implementing `IEnumerable`

## Using SfRotatorItem

### Basic SfRotatorItem Implementation

The simplest approach using the built-in `SfRotatorItem` class:

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
            new SfRotatorItem { Image = "image4.png" }
        };
        
        rotator.ItemsSource = items;
        rotator.HeightRequest = 300;
        
        this.Content = rotator;
    }
}
```

### SfRotatorItem with Text

Add text captions using the `ItemText` property:

```csharp
List<SfRotatorItem> items = new List<SfRotatorItem>
{
    new SfRotatorItem 
    { 
        Image = "bird1.png",
        ItemText = "Beautiful Hummingbird"
    },
    new SfRotatorItem 
    { 
        Image = "bird2.png",
        ItemText = "Colorful Parrot"
    },
    new SfRotatorItem 
    { 
        Image = "bird3.png",
        ItemText = "Elegant Eagle"
    }
};

rotator.ItemsSource = items;
rotator.IsTextVisible = true; // Show text panel
```

**Note:** `IsTextVisible` only works with `SfRotatorItem` collections, not custom ItemTemplates.

## Using ItemTemplate with Models

For more flexibility and data binding, use custom models with ItemTemplate.

### Step 1: Create a Data Model

```csharp
namespace YourApp.Models
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

### Step 2: Create a ViewModel

```csharp
using System.Collections.ObjectModel;

namespace YourApp.ViewModels
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

### Step 3: Configure ItemTemplate in XAML

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:syncfusion="clr-namespace:Syncfusion.Maui.Rotator;assembly=Syncfusion.Maui.Rotator"
             xmlns:local="clr-namespace:YourApp.ViewModels"
             x:Class="YourApp.MainPage">
    <ContentPage.BindingContext>
        <local:RotatorViewModel/>
    </ContentPage.BindingContext>
    
    <ContentPage.Content>
        <syncfusion:SfRotator ItemsSource="{Binding ImageCollection}"
                              NavigationStripMode="Thumbnail"
                              NavigationStripPosition="Bottom"
                              NavigationDelay="2000"
                              SelectedIndex="2"
                              BackgroundColor="#ececec"
                              HeightRequest="500"
                              WidthRequest="500">
            <syncfusion:SfRotator.ItemTemplate>
                <DataTemplate>
                    <Image Source="{Binding Image}"/>
                </DataTemplate>
            </syncfusion:SfRotator.ItemTemplate>
        </syncfusion:SfRotator>
    </ContentPage.Content>
</ContentPage>
```

### Step 4: ItemTemplate in C#

```csharp
using Syncfusion.Maui.Rotator;

public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        
        SfRotator rotator = new SfRotator();
        
        var imageCollection = new List<RotatorModel>
        {
            new RotatorModel("image1.png"),
            new RotatorModel("image2.png"),
            new RotatorModel("image3.png"),
            new RotatorModel("image4.png")
        };
        
        // Define ItemTemplate
        var itemTemplate = new DataTemplate(() =>
        {
            var grid = new Grid();
            var image = new Image();
            image.SetBinding(Image.SourceProperty, "Image");
            grid.Children.Add(image);
            return grid;
        });
        
        rotator.ItemTemplate = itemTemplate;
        rotator.ItemsSource = imageCollection;
        rotator.NavigationStripMode = NavigationStripMode.Thumbnail;
        
        this.Content = rotator;
    }
}
```

## Advanced Data Models

### Model with Multiple Properties

```csharp
public class ProductModel
{
    public string ProductName { get; set; }
    public string ImageUrl { get; set; }
    public string Description { get; set; }
    public decimal Price { get; set; }
    
    public ProductModel(string name, string imageUrl, string description, decimal price)
    {
        ProductName = name;
        ImageUrl = imageUrl;
        Description = description;
        Price = price;
    }
}
```

### Complex ItemTemplate with Multiple Bindings

```xml
<syncfusion:SfRotator ItemsSource="{Binding Products}">
    <syncfusion:SfRotator.ItemTemplate>
        <DataTemplate>
            <Grid RowDefinitions="*, Auto, Auto">
                <!-- Product Image -->
                <Image Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       Grid.Row="0"/>
                
                <!-- Product Name -->
                <Label Text="{Binding ProductName}"
                       FontSize="20"
                       FontAttributes="Bold"
                       TextColor="White"
                       BackgroundColor="#80000000"
                       Padding="10"
                       Grid.Row="1"/>
                
                <!-- Price -->
                <Label Text="{Binding Price, StringFormat='${0:F2}'}"
                       FontSize="18"
                       TextColor="Green"
                       FontAttributes="Bold"
                       HorizontalOptions="End"
                       Padding="10"
                       Grid.Row="2"/>
            </Grid>
        </DataTemplate>
    </syncfusion:SfRotator.ItemTemplate>
</syncfusion:SfRotator>
```

## Loading Online Images

### From HTTP/HTTPS URLs

```csharp
public class RotatorViewModel
{
    public ObservableCollection<RotatorModel> ImageCollection { get; set; }
    
    public RotatorViewModel()
    {
        ImageCollection = new ObservableCollection<RotatorModel>
        {
            new RotatorModel("https://example.com/images/photo1.jpg"),
            new RotatorModel("https://example.com/images/photo2.jpg"),
            new RotatorModel("https://example.com/images/photo3.jpg")
        };
    }
}
```

### XAML Configuration for Online Images

```xml
<syncfusion:SfRotator ItemsSource="{Binding ImageCollection}"
                      NavigationStripMode="Dots"
                      HeightRequest="400">
    <syncfusion:SfRotator.ItemTemplate>
        <DataTemplate>
            <Image Source="{Binding Image}"
                   Aspect="AspectFill">
                <!-- Optional: Loading indicator -->
                <Image.Behaviors>
                    <!-- Add custom loading behavior if needed -->
                </Image.Behaviors>
            </Image>
        </DataTemplate>
    </syncfusion:SfRotator.ItemTemplate>
</syncfusion:SfRotator>
```

### Complete Online Image Example

```csharp
using System.Collections.ObjectModel;

public class RotatorViewModel
{
    public ObservableCollection<RotatorModel> ImageCollection { get; set; }
    
    public RotatorViewModel()
    {
        ImageCollection = new ObservableCollection<RotatorModel>
        {
            new RotatorModel("https://cdn.syncfusion.com/content/images/Images/Camtasia_Succinctly.png"),
            new RotatorModel("https://cdn.syncfusion.com/content/images/Images/SQL_Queries_Succinctly.png"),
            new RotatorModel("https://upload.wikimedia.org/wikipedia/commons/0/0c/Cow_female_black_white.jpg"),
            new RotatorModel("https://cdn.syncfusion.com/content/images/Images/Keystonejs_Succinctly.png")
        };
    }
}

public class RotatorModel
{
    public string Image { get; set; }
    
    public RotatorModel(string imageString)
    {
        Image = imageString;
    }
}
```

## Observable Collections

### Dynamic Data Updates

Use `ObservableCollection` for data that changes at runtime:

```csharp
using System.Collections.ObjectModel;
using System.ComponentModel;

public class RotatorViewModel : INotifyPropertyChanged
{
    private ObservableCollection<RotatorModel> _imageCollection;
    public ObservableCollection<RotatorModel> ImageCollection
    {
        get => _imageCollection;
        set
        {
            _imageCollection = value;
            OnPropertyChanged(nameof(ImageCollection));
        }
    }
    
    public RotatorViewModel()
    {
        ImageCollection = new ObservableCollection<RotatorModel>();
        LoadImages();
    }
    
    private void LoadImages()
    {
        ImageCollection.Add(new RotatorModel("image1.png"));
        ImageCollection.Add(new RotatorModel("image2.png"));
        ImageCollection.Add(new RotatorModel("image3.png"));
    }
    
    public void AddImage(string imagePath)
    {
        ImageCollection.Add(new RotatorModel(imagePath));
    }
    
    public void RemoveImage(int index)
    {
        if (index >= 0 && index < ImageCollection.Count)
        {
            ImageCollection.RemoveAt(index);
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

### Adding Items Dynamically

```csharp
// In your page or view model
public void OnAddImageClicked(object sender, EventArgs e)
{
    var viewModel = (RotatorViewModel)BindingContext;
    viewModel.AddImage("newimage.png");
}
```

## Best Practices

### 1. Choose the Right Approach

- **Use SfRotatorItem** for simple image galleries without complex data binding
- **Use ItemTemplate** when you need custom layouts or multiple data properties
- **Use ObservableCollection** when data changes dynamically

### 2. Image Optimization

```csharp
// Optimize image loading for better performance
<Image Source="{Binding Image}"
       Aspect="AspectFill"
       CachingEnabled="True"
       CacheValidity="7"/>
```

### 3. Handle Empty States

```csharp
public RotatorViewModel()
{
    ImageCollection = new ObservableCollection<RotatorModel>();
    
    if (ImageCollection.Count == 0)
    {
        // Add placeholder or default image
        ImageCollection.Add(new RotatorModel("placeholder.png"));
    }
}
```

### 4. Error Handling for Online Images

```csharp
public class RotatorModel
{
    public string Image { get; set; }
    public string FallbackImage { get; set; } = "error_placeholder.png";
    
    public RotatorModel(string image, string fallback = null)
    {
        Image = image;
        if (!string.IsNullOrEmpty(fallback))
            FallbackImage = fallback;
    }
}
```

### 5. Memory Management

For large image collections:
- Implement virtualization if possible
- Dispose of unused images
- Use appropriate image sizes (avoid full resolution if not needed)
- Consider lazy loading for online images

## Common Issues

### Issue: ItemsSource Not Updating

**Solution:** Ensure you're using `ObservableCollection` and implementing `INotifyPropertyChanged`

### Issue: Images Not Binding

**Check:**
- Property names match exactly in binding expressions
- Data context is set correctly
- ItemTemplate is properly configured

### Issue: Performance with Many Items

**Solutions:**
- Optimize image sizes
- Enable image caching
- Limit autoplay when many items are present
- Consider pagination for very large datasets
