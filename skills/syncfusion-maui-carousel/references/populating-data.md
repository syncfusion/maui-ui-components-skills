# Populating Data in .NET MAUI Carousel

The SfCarousel control supports multiple ways to populate items, including data binding with ItemsSource and direct item creation using SfCarouselItem.

## Through Data Binding (Recommended)

Data binding allows you to populate carousel items from a collection using MVVM pattern with ItemsSource and ItemTemplate.

### Creating a Model

Define a model class that represents your carousel data:

```csharp
namespace CarouselSample
{
    public class CarouselModel
    {
        private string _image;

        public string Image
        {
            get { return _image; }
            set { _image = value; }
        }

        public CarouselModel(string imageString)
        {
            Image = imageString;
        }
    }
}
```

**For more complex data:**
```csharp
public class ProductModel
{
    public string ProductImage { get; set; }
    public string ProductName { get; set; }
    public string Description { get; set; }
    public decimal Price { get; set; }
    
    public ProductModel(string image, string name, string desc, decimal price)
    {
        ProductImage = image;
        ProductName = name;
        Description = desc;
        Price = price;
    }
}
```

### Creating a ViewModel

Populate the collection in a ViewModel:

```csharp
using System.Collections.Generic;

namespace CarouselSample
{
    public class CarouselViewModel
    {
        private List<CarouselModel> imageCollection = new List<CarouselModel>();
        
        public List<CarouselModel> ImageCollection
        {
            get { return imageCollection; }
            set { imageCollection = value; }
        }

        public CarouselViewModel()
        {
            ImageCollection.Add(new CarouselModel("carousel_person1.png"));
            ImageCollection.Add(new CarouselModel("carousel_person2.png"));
            ImageCollection.Add(new CarouselModel("carousel_person3.png"));
            ImageCollection.Add(new CarouselModel("carousel_person4.png"));
            ImageCollection.Add(new CarouselModel("carousel_person5.png"));
        }
    }
}
```

**Using ObservableCollection (for dynamic updates):**
```csharp
using System.Collections.ObjectModel;

public class CarouselViewModel
{
    public ObservableCollection<CarouselModel> ImageCollection { get; set; }

    public CarouselViewModel()
    {
        ImageCollection = new ObservableCollection<CarouselModel>
        {
            new CarouselModel("carousel_person1.png"),
            new CarouselModel("carousel_person2.png"),
            new CarouselModel("carousel_person3.png"),
            new CarouselModel("carousel_person4.png"),
            new CarouselModel("carousel_person5.png")
        };
    }
    
    public void AddItem(string imagePath)
    {
        ImageCollection.Add(new CarouselModel(imagePath));
    }
}
```

**Why ObservableCollection:** Changes to the collection automatically update the carousel UI.

### Binding with Custom Template

Use ItemTemplate to define how each item should appear:

**XAML:**
```xml
<?xml version="1.0" encoding="utf-8" ?>
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
                             ItemsSource="{Binding ImageCollection}"
                             ItemHeight="170"
                             ItemWidth="270"
                             ItemSpacing="2" 
                             HeightRequest="400" 
                             WidthRequest="800" />
    </ContentPage.Content>
</ContentPage>
```

**C#:**
```csharp
using Syncfusion.Maui.Carousel;
using System.Collections.Generic;

namespace CarouselSample
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            
            var carouselViewModel = new CarouselViewModel();

            var carousel = new SfCarousel
            {
                ItemHeight = 170,
                ItemWidth = 270,
                ItemSpacing = 2,
                HeightRequest = 400,
                WidthRequest = 800
            };

            var itemTemplate = new DataTemplate(() =>
            {
                var grid = new Grid();
                var image = new Image();
                image.SetBinding(Image.SourceProperty, "Image");
                grid.Children.Add(image);
                return grid;
            });

            carousel.BindingContext = carouselViewModel;
            carousel.ItemTemplate = itemTemplate;
            carousel.SetBinding(SfCarousel.ItemsSourceProperty, "ImageCollection");

            this.Content = carousel;
        }
    }
}
```

### Complex Custom Templates

For rich UI with multiple elements:

```xml
<DataTemplate x:Key="productTemplate">
    <Grid Padding="10">
        <Frame CornerRadius="15" 
               HasShadow="True" 
               BackgroundColor="White"
               BorderColor="LightGray">
            <StackLayout Spacing="10">
                <Image Source="{Binding ProductImage}" 
                       HeightRequest="200"
                       Aspect="AspectFill"/>
                <Label Text="{Binding ProductName}" 
                       FontSize="20" 
                       FontAttributes="Bold"
                       HorizontalOptions="Center"/>
                <Label Text="{Binding Description}" 
                       FontSize="14"
                       TextColor="Gray"
                       MaxLines="2"
                       LineBreakMode="TailTruncation"/>
                <Label Text="{Binding Price, StringFormat='${0:F2}'}"
                       FontSize="18"
                       TextColor="Green"
                       FontAttributes="Bold"
                       HorizontalOptions="Center"/>
            </StackLayout>
        </Frame>
    </Grid>
</DataTemplate>
```

```xml
<carousel:SfCarousel ItemsSource="{Binding Products}"
                     ItemTemplate="{StaticResource productTemplate}"
                     ItemHeight="350"
                     ItemWidth="280"/>
```

## Through SfCarouselItem

Create carousel items directly without data binding.

### Using ItemContent Property

Provide custom views for each item:

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
            
            var carousel = new SfCarousel
            {
                ItemHeight = 170,
                ItemWidth = 270,
                ItemSpacing = 2,
                HeightRequest = 400,
                WidthRequest = 800
            };

            var carouselItems = new ObservableCollection<SfCarouselItem>();

            carouselItems.Add(new SfCarouselItem 
            { 
                ItemContent = new Image 
                { 
                    Source = "carousel_person1.png", 
                    Aspect = Aspect.Fill 
                } 
            });
            carouselItems.Add(new SfCarouselItem 
            { 
                ItemContent = new Image 
                { 
                    Source = "carousel_person2.png", 
                    Aspect = Aspect.Fill 
                } 
            });
            carouselItems.Add(new SfCarouselItem 
            { 
                ItemContent = new Image 
                { 
                    Source = "carousel_person3.png", 
                    Aspect = Aspect.Fill 
                } 
            });
            carouselItems.Add(new SfCarouselItem 
            { 
                ItemContent = new Image 
                { 
                    Source = "carousel_person4.png", 
                    Aspect = Aspect.Fill 
                } 
            });
            carouselItems.Add(new SfCarouselItem 
            { 
                ItemContent = new Image 
                { 
                    Source = "carousel_person5.png", 
                    Aspect = Aspect.Fill 
                } 
            });

            carousel.ItemsSource = carouselItems;
            this.Content = carousel;
        }
    }
}
```

**ItemContent accepts any View:**
```csharp
carouselItems.Add(new SfCarouselItem
{
    ItemContent = new StackLayout
    {
        Children =
        {
            new Image { Source = "product1.png", HeightRequest = 150 },
            new Label { Text = "Product Name", FontSize = 18 }
        }
    }
});
```

### Using ImageName Property (Simple Images)

For displaying just images, use the ImageName property:

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
            
            var carousel = new SfCarousel
            {
                ItemHeight = 170,
                ItemWidth = 270,
                ItemSpacing = 2,
                HeightRequest = 400,
                WidthRequest = 800
            };

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

**When to use ImageName:**
- Quick prototypes
- Simple image galleries
- No additional UI elements needed

**When to use ItemContent:**
- Complex layouts per item
- Multiple UI elements (images + text + buttons)
- Different view types per item

## Data Binding vs Direct Items

| Aspect | Data Binding (ItemsSource + ItemTemplate) | Direct Items (SfCarouselItem) |
|--------|-------------------------------------------|-------------------------------|
| **MVVM Support** | ✅ Yes | ❌ No |
| **Testability** | ✅ High | ⚠️ Low |
| **Maintainability** | ✅ High | ⚠️ Medium |
| **Dynamic Updates** | ✅ With ObservableCollection | ⚠️ Manual management |
| **Template Reuse** | ✅ Yes | ❌ No |
| **Use Case** | Production apps | Quick prototypes |

**Recommendation:** Use data binding for production code; use direct items for quick tests or simple cases.

## Dynamic Data Loading

### Loading Data Asynchronously

```csharp
public class CarouselViewModel : INotifyPropertyChanged
{
    public ObservableCollection<CarouselModel> ImageCollection { get; set; }
    
    public CarouselViewModel()
    {
        ImageCollection = new ObservableCollection<CarouselModel>();
        LoadDataAsync();
    }
    
    private async Task LoadDataAsync()
    {
        // Simulate async data loading
        var images = await FetchImagesFromApiAsync();
        
        foreach (var image in images)
        {
            ImageCollection.Add(new CarouselModel(image));
        }
    }
    
    private async Task<List<string>> FetchImagesFromApiAsync()
    {
        await Task.Delay(1000); // Simulate network call
        return new List<string>
        {
            "carousel_person1.png",
            "carousel_person2.png",
            "carousel_person3.png"
        };
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
}
```

### Adding Items Dynamically

```csharp
public void AddNewItem(string imagePath)
{
    ImageCollection.Add(new CarouselModel(imagePath));
}

public void RemoveItem(int index)
{
    if (index >= 0 && index < ImageCollection.Count)
    {
        ImageCollection.RemoveAt(index);
    }
}

public void ClearAll()
{
    ImageCollection.Clear();
}
```

## Edge Cases and Best Practices

**Empty Collection:**
```csharp
if (ImageCollection == null || ImageCollection.Count == 0)
{
    // Show placeholder or empty state
    carousel.ItemsSource = new List<SfCarouselItem>
    {
        new SfCarouselItem 
        { 
            ItemContent = new Label 
            { 
                Text = "No items to display",
                HorizontalOptions = LayoutOptions.Center,
                VerticalOptions = LayoutOptions.Center
            } 
        }
    };
}
```

**Large Datasets:**
- Use `EnableVirtualization="True"` for collections >50 items
- Implement load more functionality
- See [advanced-features.md](advanced-features.md) for details

**Image Optimization:**
- Resize images before adding to carousel
- Use appropriate image formats (WebP, PNG, JPEG)
- Consider lazy loading for remote images

**Memory Management:**
```csharp
protected override void OnDisappearing()
{
    base.OnDisappearing();
    // Clear references if needed
    ImageCollection?.Clear();
}
```

## Troubleshooting

**Items not displaying:**
→ Ensure ItemTemplate binds to correct property names
→ Verify ItemsSource collection has data (`Count > 0`)
→ Check ItemHeight/ItemWidth are set appropriately

**Template not applying:**
→ Confirm DataTemplate is in Resources dictionary
→ Verify `ItemTemplate="{StaticResource templateKey}"` matches resource key
→ Check binding paths match model property names (case-sensitive)

**ObservableCollection not updating UI:**
→ Ensure you're using `ObservableCollection<T>`, not `List<T>`
→ Modify collection on UI thread: `Device.BeginInvokeOnMainThread()`
→ Implement `INotifyPropertyChanged` in ViewModel if binding to collection itself
