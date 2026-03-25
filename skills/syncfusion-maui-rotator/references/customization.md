# Advanced Customization

## Table of Contents
- [Overview](#overview)
- [DataTemplateSelector](#datatemplateselector)
- [IsTextVisible](#istextvisible)
- [Background and Sizing](#background-and-sizing)
- [Loading Online Images](#loading-online-images)
- [Custom Layouts](#custom-layouts)
- [Complete Examples](#complete-examples)

## Overview

Beyond basic configuration, SfRotator supports advanced customization through:

- **DataTemplateSelector**: Choose templates dynamically based on data
- **IsTextVisible**: Display text captions with SfRotatorItem
- **Styling**: Background colors, sizing, and appearance
- **Online Images**: Load images from HTTP/HTTPS URLs
- **Custom Layouts**: Complex item templates with multiple elements

## DataTemplateSelector

DataTemplateSelector allows you to choose different DataTemplates based on the data object properties.

### Step 1: Define Templates in Resources

```xml
<ContentPage.Resources>
    <ResourceDictionary>
        <!-- Default template for regular items -->
        <DataTemplate x:Key="DefaultTemplate">
            <Grid>
                <Image Source="{Binding Image}"
                       HorizontalOptions="Center"
                       VerticalOptions="Center"/>
            </Grid>
        </DataTemplate>
        
        <!-- Special template for specific items -->
        <DataTemplate x:Key="SpecificTemplate">
            <Grid>
                <Image Source="{Binding Image}" Opacity="0.5"/>
                <Label Text="Not Available"
                       FontSize="50"
                       TextColor="Red"
                       HorizontalOptions="Center"
                       VerticalOptions="Center"/>
            </Grid>
        </DataTemplate>
    </ResourceDictionary>
</ContentPage.Resources>
```

### Step 2: Create DataTemplateSelector Class

```csharp
using Microsoft.Maui.Controls;

namespace YourApp
{
    public class RotatorDataTemplateSelector : DataTemplateSelector
    {
        public DataTemplate DefaultTemplate { get; set; }
        public DataTemplate SpecificTemplate { get; set; }
        
        protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
        {
            var rotatorModel = item as RotatorModel;
            
            if (rotatorModel == null)
                return DefaultTemplate;
            
            // Apply SpecificTemplate for certain conditions
            if (rotatorModel.Image == "image2.png" || rotatorModel.IsUnavailable)
            {
                return SpecificTemplate;
            }
            
            return DefaultTemplate;
        }
    }
}
```

### Step 3: Apply DataTemplateSelector in XAML

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:syncfusion="clr-namespace:Syncfusion.Maui.Rotator;assembly=Syncfusion.Maui.Rotator"
             xmlns:local="clr-namespace:YourApp"
             x:Class="YourApp.MainPage">
    
    <ContentPage.Resources>
        <ResourceDictionary>
            <DataTemplate x:Key="DefaultTemplate">
                <Grid>
                    <Image Source="{Binding Image}"
                           HorizontalOptions="Center"
                           VerticalOptions="Center"/>
                </Grid>
            </DataTemplate>
            
            <DataTemplate x:Key="SpecificTemplate">
                <Grid>
                    <Image Source="{Binding Image}" Opacity="0.5"/>
                    <Label Text="Not Available"
                           FontSize="50"
                           HorizontalOptions="Center"
                           VerticalOptions="Center"/>
                </Grid>
            </DataTemplate>
        </ResourceDictionary>
    </ContentPage.Resources>
    
    <ContentPage.Content>
        <syncfusion:SfRotator x:Name="rotator"
                              ItemsSource="{Binding ImageCollection}">
            <syncfusion:SfRotator.ItemTemplate>
                <local:RotatorDataTemplateSelector
                    DefaultTemplate="{StaticResource DefaultTemplate}"
                    SpecificTemplate="{StaticResource SpecificTemplate}"/>
            </syncfusion:SfRotator.ItemTemplate>
        </syncfusion:SfRotator>
    </ContentPage.Content>
</ContentPage>
```

### Step 4: DataTemplateSelector in C#

```csharp
using Microsoft.Maui.Controls;
using Syncfusion.Maui.Rotator;

public partial class MainPage : ContentPage
{
    DataTemplate defaultTemplate;
    DataTemplate specificTemplate;
    
    public MainPage()
    {
        InitializeComponent();
        this.BindingContext = new RotatorViewModel();
        
        SfRotator rotator = new SfRotator();
        
        // Define default template
        defaultTemplate = new DataTemplate(() =>
        {
            Grid grid = new Grid();
            Image image = new Image();
            image.SetBinding(Image.SourceProperty, "Image");
            grid.Children.Add(image);
            return grid;
        });
        
        // Define specific template
        specificTemplate = new DataTemplate(() =>
        {
            Grid grid = new Grid();
            Image image = new Image();
            Label label = new Label();
            
            image.SetBinding(Image.SourceProperty, "Image");
            image.Opacity = 0.5;
            
            label.Text = "Not Available";
            label.FontSize = 50;
            label.HorizontalOptions = LayoutOptions.Center;
            label.VerticalOptions = LayoutOptions.Center;
            
            grid.Children.Add(image);
            grid.Children.Add(label);
            return grid;
        });
        
        var imageCollection = new List<RotatorModel>
        {
            new RotatorModel("image1.png"),
            new RotatorModel("image2.png") { IsUnavailable = true },
            new RotatorModel("image3.png"),
            new RotatorModel("image4.png"),
            new RotatorModel("image5.png")
        };
        
        rotator.NavigationDirection = NavigationDirection.Horizontal;
        rotator.NavigationStripMode = NavigationStripMode.Thumbnail;
        rotator.BackgroundColor = Colors.White;
        rotator.ItemsSource = imageCollection;
        rotator.ItemTemplate = new RotatorDataTemplateSelector
        {
            DefaultTemplate = defaultTemplate,
            SpecificTemplate = specificTemplate
        };
        
        this.Content = rotator;
    }
}

// Enhanced Model
public class RotatorModel
{
    public string Image { get; set; }
    public bool IsUnavailable { get; set; }
    
    public RotatorModel(string image)
    {
        Image = image;
        IsUnavailable = false;
    }
}
```

### Advanced DataTemplateSelector Example

```csharp
public class ProductTemplateSelector : DataTemplateSelector
{
    public DataTemplate FeaturedTemplate { get; set; }
    public DataTemplate RegularTemplate { get; set; }
    public DataTemplate OutOfStockTemplate { get; set; }
    
    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        var product = item as ProductModel;
        
        if (product == null)
            return RegularTemplate;
        
        if (!product.InStock)
            return OutOfStockTemplate;
        
        if (product.IsFeatured)
            return FeaturedTemplate;
        
        return RegularTemplate;
    }
}

public class ProductModel
{
    public string Image { get; set; }
    public string Name { get; set; }
    public bool IsFeatured { get; set; }
    public bool InStock { get; set; }
    public decimal Price { get; set; }
}
```

## IsTextVisible

Display text captions below images when using SfRotatorItem collections.

**Property:** `IsTextVisible`  
**Type:** `bool`  
**Default:** `false`  
**Note:** Only works with `SfRotatorItem`, not custom ItemTemplates

### Basic Text Display

**XAML:**
```xml
<syncfusion:SfRotator x:Name="rotator"
                      BackgroundColor="#ececec"
                      IsTextVisible="True"
                      ItemsSource="{Binding ImageCollection}"
                      HeightRequest="400">
    <syncfusion:SfRotator.ItemTemplate>
        <DataTemplate>
            <Image Source="{Binding Image}"/>
        </DataTemplate>
    </syncfusion:SfRotator.ItemTemplate>
</syncfusion:SfRotator>
```

### Using with SfRotatorItem

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
            },
            new SfRotatorItem
            {
                Image = "bird4.png",
                ItemText = "Majestic Owl"
            }
        };
        
        rotator.ItemsSource = items;
        rotator.IsTextVisible = true;
        rotator.DotPlacement = DotsPlacement.OutSide;
        rotator.HeightRequest = 400;
        rotator.WidthRequest = 400;
        
        this.Content = rotator;
    }
}
```

**XAML Approach:**
```xml
<syncfusion:SfRotator BackgroundColor="#ececec"
                      IsTextVisible="True"
                      DotPlacement="Outside"
                      HeightRequest="400">
    <syncfusion:SfRotator.ItemsSource>
        <x:Array Type="{x:Type syncfusion:SfRotatorItem}">
            <syncfusion:SfRotatorItem Image="bird1.png" ItemText="Beautiful Hummingbird"/>
            <syncfusion:SfRotatorItem Image="bird2.png" ItemText="Colorful Parrot"/>
            <syncfusion:SfRotatorItem Image="bird3.png" ItemText="Elegant Eagle"/>
        </x:Array>
    </syncfusion:SfRotator.ItemsSource>
</syncfusion:SfRotator>
```

## Background and Sizing

### Background Color

**XAML:**
```xml
<syncfusion:SfRotator BackgroundColor="#F5F5F5"
                      ItemsSource="{Binding ImageCollection}">
    <!-- Content -->
</syncfusion:SfRotator>
```

**C#:**
```csharp
rotator.BackgroundColor = Color.FromArgb("#F5F5F5");
```

### Width and Height

**XAML:**
```xml
<syncfusion:SfRotator WidthRequest="600"
                      HeightRequest="400"
                      ItemsSource="{Binding ImageCollection}">
    <!-- Content -->
</syncfusion:SfRotator>
```

**C#:**
```csharp
rotator.WidthRequest = 600;
rotator.HeightRequest = 400;
```

### Responsive Sizing

```xml
<Grid>
    <syncfusion:SfRotator ItemsSource="{Binding ImageCollection}"
                          HorizontalOptions="FillAndExpand"
                          VerticalOptions="FillAndExpand">
        <!-- Content -->
    </syncfusion:SfRotator>
</Grid>
```

## Loading Online Images

Load images from HTTP/HTTPS URLs instead of local resources.

### Basic Online Image Loading

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
            new RotatorModel("https://example.com/images/photo3.jpg"),
            new RotatorModel("https://cdn.syncfusion.com/content/images/Images/Camtasia_Succinctly.png")
        };
    }
}
```

### Complete Online Image Example

**XAML:**
```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:syncfusion="clr-namespace:Syncfusion.Maui.Rotator;assembly=Syncfusion.Maui.Rotator"
             xmlns:local="clr-namespace:YourApp"
             x:Class="YourApp.MainPage">
    <ContentPage.BindingContext>
        <local:RotatorViewModel/>
    </ContentPage.BindingContext>
    
    <ContentPage.Content>
        <syncfusion:SfRotator ItemsSource="{Binding ImageCollection}"
                              NavigationStripMode="Dots"
                              NavigationDirection="Horizontal"
                              HeightRequest="450">
            <syncfusion:SfRotator.ItemTemplate>
                <DataTemplate>
                    <Image Source="{Binding Image}"
                           Aspect="AspectFill"/>
                </DataTemplate>
            </syncfusion:SfRotator.ItemTemplate>
        </syncfusion:SfRotator>
    </ContentPage.Content>
</ContentPage>
```

**C#:**
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

## Custom Layouts

Create complex item templates with multiple UI elements.

### Product Card Layout

```xml
<syncfusion:SfRotator ItemsSource="{Binding Products}"
                      HeightRequest="550">
    <syncfusion:SfRotator.ItemTemplate>
        <DataTemplate>
            <Grid RowDefinitions="*, Auto, Auto, Auto"
                  Padding="10">
                <!-- Product Image -->
                <Image Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       Grid.Row="0"/>
                
                <!-- Product Name -->
                <Label Text="{Binding Name}"
                       FontSize="24"
                       FontAttributes="Bold"
                       Margin="0,10,0,5"
                       Grid.Row="1"/>
                
                <!-- Description -->
                <Label Text="{Binding Description}"
                       FontSize="14"
                       TextColor="Gray"
                       MaxLines="2"
                       LineBreakMode="TailTruncation"
                       Grid.Row="2"/>
                
                <!-- Price and Button -->
                <Grid ColumnDefinitions="*, Auto"
                      Margin="0,10,0,0"
                      Grid.Row="3">
                    <Label Text="{Binding Price, StringFormat='${0:F2}'}"
                           FontSize="28"
                           FontAttributes="Bold"
                           TextColor="Green"
                           VerticalOptions="Center"
                           Grid.Column="0"/>
                    
                    <Button Text="Add to Cart"
                            BackgroundColor="Blue"
                            TextColor="White"
                            CornerRadius="5"
                            Grid.Column="1"/>
                </Grid>
            </Grid>
        </DataTemplate>
    </syncfusion:SfRotator.ItemTemplate>
</syncfusion:SfRotator>
```

### Testimonial Card Layout

```xml
<syncfusion:SfRotator ItemsSource="{Binding Testimonials}"
                      NavigationStripMode="Dots"
                      EnableAutoPlay="True"
                      NavigationDelay="6000"
                      HeightRequest="350">
    <syncfusion:SfRotator.ItemTemplate>
        <DataTemplate>
            <Frame CornerRadius="10"
                   Padding="20"
                   BackgroundColor="White"
                   HasShadow="True">
                <Grid RowDefinitions="Auto, *, Auto">
                    <!-- Quote Icon -->
                    <Label Text="&#x275D;"
                           FontSize="50"
                           TextColor="LightGray"
                           Grid.Row="0"/>
                    
                    <!-- Testimonial Text -->
                    <Label Text="{Binding Quote}"
                           FontSize="16"
                           LineHeight="1.5"
                           VerticalOptions="Center"
                           Grid.Row="1"
                           Margin="0,10"/>
                    
                    <!-- Author Info -->
                    <StackLayout Orientation="Horizontal"
                                 Spacing="10"
                                 Grid.Row="2">
                        <Image Source="{Binding AuthorPhoto}"
                               WidthRequest="50"
                               HeightRequest="50"
                               Aspect="AspectFill">
                            <Image.Clip>
                                <EllipseGeometry Center="25,25"
                                                 RadiusX="25"
                                                 RadiusY="25"/>
                            </Image.Clip>
                        </Image>
                        <StackLayout VerticalOptions="Center">
                            <Label Text="{Binding AuthorName}"
                                   FontAttributes="Bold"/>
                            <Label Text="{Binding AuthorTitle}"
                                   FontSize="12"
                                   TextColor="Gray"/>
                        </StackLayout>
                    </StackLayout>
                </Grid>
            </Frame>
        </DataTemplate>
    </syncfusion:SfRotator.ItemTemplate>
</syncfusion:SfRotator>
```

### News Card with Overlay

```xml
<syncfusion:SfRotator ItemsSource="{Binding NewsArticles}"
                      HeightRequest="400">
    <syncfusion:SfRotator.ItemTemplate>
        <DataTemplate>
            <Grid>
                <!-- Background Image -->
                <Image Source="{Binding ImageUrl}"
                       Aspect="AspectFill"/>
                
                <!-- Gradient Overlay -->
                <BoxView>
                    <BoxView.Background>
                        <LinearGradientBrush StartPoint="0,0" EndPoint="0,1">
                            <GradientStop Color="Transparent" Offset="0.0"/>
                            <GradientStop Color="#CC000000" Offset="1.0"/>
                        </LinearGradientBrush>
                    </BoxView.Background>
                </BoxView>
                
                <!-- Content -->
                <StackLayout VerticalOptions="End"
                             Padding="20">
                    <Label Text="{Binding Category}"
                           TextColor="Orange"
                           FontSize="12"
                           FontAttributes="Bold"/>
                    <Label Text="{Binding Title}"
                           TextColor="White"
                           FontSize="24"
                           FontAttributes="Bold"
                           Margin="0,5"/>
                    <Label Text="{Binding Summary}"
                           TextColor="White"
                           FontSize="14"
                           MaxLines="2"
                           LineBreakMode="TailTruncation"/>
                    <Label Text="{Binding PublishDate, StringFormat='{0:MMM dd, yyyy}'}"
                           TextColor="LightGray"
                           FontSize="12"
                           Margin="0,10,0,0"/>
                </StackLayout>
            </Grid>
        </DataTemplate>
    </syncfusion:SfRotator.ItemTemplate>
</syncfusion:SfRotator>
```

## Complete Examples

### Example 1: Full Product Showcase

```csharp
public class ProductShowcaseViewModel
{
    public ObservableCollection<ProductModel> Products { get; set; }
    
    public ProductShowcaseViewModel()
    {
        Products = new ObservableCollection<ProductModel>
        {
            new ProductModel
            {
                Name = "Premium Headphones",
                ImageUrl = "https://example.com/headphones.jpg",
                Description = "High-quality wireless headphones with noise cancellation",
                Price = 299.99m,
                IsFeatured = true,
                InStock = true
            },
            new ProductModel
            {
                Name = "Smart Watch",
                ImageUrl = "https://example.com/watch.jpg",
                Description = "Feature-rich smartwatch with fitness tracking",
                Price = 399.99m,
                IsFeatured = false,
                InStock = true
            }
        };
    }
}

public class ProductModel
{
    public string Name { get; set; }
    public string ImageUrl { get; set; }
    public string Description { get; set; }
    public decimal Price { get; set; }
    public bool IsFeatured { get; set; }
    public bool InStock { get; set; }
}
```

## Best Practices

1. **Image Optimization:**
   - Resize images appropriately for display size
   - Use compressed formats (WebP, optimized JPEGs)
   - Consider CDN for online images

2. **DataTemplateSelector:**
   - Keep template selection logic simple
   - Cache templates when possible
   - Test with various data scenarios

3. **Custom Layouts:**
   - Keep templates performant (avoid complex nesting)
   - Test on target devices
   - Consider different screen sizes

4. **Online Images:**
   - Implement error handling
   - Show loading indicators
   - Provide fallback images
   - Cache aggressively

## Common Issues

### Issue: DataTemplateSelector Not Working

**Check:**
1. Selector class inherits from `DataTemplateSelector`
2. `OnSelectTemplate` is overridden correctly
3. Templates are assigned to selector properties
4. Data model properties are accessible

### Issue: IsTextVisible Not Showing Text

**Solution:**
- `IsTextVisible` only works with `SfRotatorItem` collections
- For custom templates, add Label to ItemTemplate manually
- Verify `ItemText` property is set on SfRotatorItem

### Issue: Online Images Not Loading

**Check:**
1. Internet permission configured (platform-specific)
2. URLs are valid and accessible
3. HTTPS URLs preferred (HTTP may be blocked)
4. Consider timeout and retry logic
