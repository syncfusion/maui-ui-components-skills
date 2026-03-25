# Source Binding and Integration

The Source property is the key to creating the parallax effect. It represents the foreground scrollable control that drives the parallax motion of the background Content. This guide explains how to bind scrollable controls to create effective parallax experiences.

## Understanding the Source Property

The `Source` property must be set to a scrollable control that implements the `IParallaxView` interface. When the Source scrolls, the SfParallaxView automatically moves the Content (background) at a different speed based on the Speed property, creating the parallax effect.

**Requirements:**
- Source must be a scrollable view (has scrolling capability)
- Source must implement `IParallaxView` interface (or be a built-in supported control)
- Source should have a transparent background for the parallax effect to be visible

## Built-in Supported Controls

Syncfusion ParallaxView has built-in support for these controls - no additional configuration needed:

1. **.NET MAUI ScrollView** - Native MAUI scrollable container
2. **Syncfusion ListView (SfListView)** - Syncfusion's list control

Both of these controls already implement the `IParallaxView` interface, so you can bind them directly to the Source property.

## IParallaxView Interface

The IParallaxView interface provides the contract for scrollable controls to work with SfParallaxView:

```csharp
public interface IParallaxView
{
    // Total size of the scrollable content
    Size ScrollableContentSize { get; set; }
    
    // Event raised when scrolling occurs
    event EventHandler<ParallaxScrollingEventArgs> Scrolling;
}
```

For custom controls, you'll need to implement this interface. See [custom-controls.md](custom-controls.md) for details.

## ScrollView Integration

The simplest parallax implementation uses .NET MAUI's built-in ScrollView.

### Basic ScrollView Example

**XAML:**
```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    xmlns:parallax="clr-namespace:Syncfusion.Maui.ParallaxView;assembly=Syncfusion.Maui.ParallaxView"
    x:Class="ParallaxViewDemo.MainPage">
    
    <Grid>
        <!-- Parallax View with Image Background -->
        <parallax:SfParallaxView Source="{x:Reference scrollView}" x:Name="parallaxView">
            <parallax:SfParallaxView.Content>
                <Image Source="background_image.jpg" 
                       BackgroundColor="Transparent"
                       HorizontalOptions="Fill" 
                       VerticalOptions="Fill" 
                       Aspect="AspectFill" />
            </parallax:SfParallaxView.Content>
        </parallax:SfParallaxView>
        
        <!-- Foreground ScrollView (MUST be transparent) -->
        <ScrollView x:Name="scrollView" BackgroundColor="Transparent">
            <StackLayout Padding="20" Spacing="15">
                <Label Text="Article Title" 
                       FontSize="32" 
                       FontAttributes="Bold" 
                       TextColor="White" />
                <Label Text="Lorem ipsum dolor sit amet, consectetur adipiscing elit..." 
                       FontSize="16" 
                       TextColor="White" />
                <BoxView HeightRequest="500" Color="Transparent" />
                <Label Text="More content here..." 
                       FontSize="16" 
                       TextColor="White" />
                <BoxView HeightRequest="500" Color="Transparent" />
            </StackLayout>
        </ScrollView>
    </Grid>
</ContentPage>
```

**C#:**
```csharp
using Syncfusion.Maui.ParallaxView;

namespace ParallaxViewDemo
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            
            // Create parallax view
            var parallaxView = new SfParallaxView();
            
            // Set background content
            var backgroundImage = new Image
            {
                Source = "background_image.jpg",
                BackgroundColor = Colors.Transparent,
                HorizontalOptions = LayoutOptions.Fill,
                VerticalOptions = LayoutOptions.Fill,
                Aspect = Aspect.AspectFill
            };
            parallaxView.Content = backgroundImage;
            
            // Create scrollable content
            var scrollView = new ScrollView
            {
                BackgroundColor = Colors.Transparent,
                Content = new StackLayout
                {
                    Padding = new Thickness(20),
                    Spacing = 15,
                    Children =
                    {
                        new Label 
                        { 
                            Text = "Article Title", 
                            FontSize = 32, 
                            FontAttributes = FontAttributes.Bold,
                            TextColor = Colors.White 
                        },
                        new Label 
                        { 
                            Text = "Lorem ipsum dolor sit amet...", 
                            FontSize = 16,
                            TextColor = Colors.White 
                        }
                        // Add more content
                    }
                }
            };
            
            // Bind ScrollView as Source
            parallaxView.Source = scrollView;
            
            // Add to page
            var grid = new Grid();
            grid.Children.Add(parallaxView);
            grid.Children.Add(scrollView);
            this.Content = grid;
        }
    }
}
```

## ListView Integration

For data-driven scrollable content, use Syncfusion's SfListView with the parallax view.

### Complete ListView Example

**XAML:**
```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    xmlns:parallax="clr-namespace:Syncfusion.Maui.ParallaxView;assembly=Syncfusion.Maui.ParallaxView"
    xmlns:list="clr-namespace:Syncfusion.Maui.ListView;assembly=Syncfusion.Maui.ListView"
    x:Class="ParallaxViewDemo.MainPage">
    
    <Grid>
        <!-- Parallax View -->
        <parallax:SfParallaxView Source="{x:Reference listView}" x:Name="parallaxView">
            <parallax:SfParallaxView.Content>
                <Image Source="{Binding BackgroundImage}" 
                       BackgroundColor="Transparent"
                       HorizontalOptions="Fill" 
                       VerticalOptions="Fill" 
                       Aspect="AspectFill" />
            </parallax:SfParallaxView.Content>
        </parallax:SfParallaxView>
        
        <!-- ListView as Source -->
        <list:SfListView x:Name="listView" 
                         ItemsSource="{Binding Items}" 
                         BackgroundColor="Transparent"
                         ItemSize="100">
            <list:SfListView.ItemTemplate>
                <DataTemplate>
                    <ViewCell>
                        <Grid Padding="10" BackgroundColor="#80000000">
                            <Grid.ColumnDefinitions>
                                <ColumnDefinition Width="90" />
                                <ColumnDefinition Width="*" />
                            </Grid.ColumnDefinitions>
                            
                            <!-- Item Image -->
                            <Image Source="{Binding ItemImage}" 
                                   HeightRequest="80" 
                                   WidthRequest="80"
                                   Aspect="AspectFit"
                                   Grid.Column="0" 
                                   HorizontalOptions="Center"
                                   VerticalOptions="Center" />
                            
                            <!-- Item Details -->
                            <StackLayout Grid.Column="1" 
                                       VerticalOptions="Center" 
                                       Spacing="5">
                                <Label Text="{Binding Name}" 
                                       FontSize="18" 
                                       FontAttributes="Bold"
                                       TextColor="White" />
                                <Label Text="{Binding Author}" 
                                       FontSize="14" 
                                       TextColor="LightGray" />
                            </StackLayout>
                        </Grid>
                    </ViewCell>
                </DataTemplate>
            </list:SfListView.ItemTemplate>
        </list:SfListView>
    </Grid>
</ContentPage>
```

**C# (ViewModel):**
```csharp
using System.Collections.ObjectModel;
using System.Reflection;

namespace ParallaxViewDemo
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            BindingContext = new ParallaxViewModel();
        }
    }
    
    public class ParallaxViewModel
    {
        public ImageSource BackgroundImage { get; set; }
        public ObservableCollection<MusicItem> Items { get; set; }
        
        public ParallaxViewModel()
        {
            Assembly assembly = typeof(ParallaxViewModel).GetTypeInfo().Assembly;
            
            // Load background image from resources
            BackgroundImage = ImageSource.FromResource(
                "ParallaxViewDemo.Resources.Images.music_background.jpg", 
                assembly);
            
            // Create sample data
            Items = new ObservableCollection<MusicItem>
            {
                new MusicItem 
                { 
                    Name = "Thriller", 
                    Author = "Michael Jackson",
                    ItemImage = ImageSource.FromResource(
                        "ParallaxViewDemo.Resources.Images.album1.png", assembly)
                },
                new MusicItem 
                { 
                    Name = "Like a Prayer", 
                    Author = "Madonna",
                    ItemImage = ImageSource.FromResource(
                        "ParallaxViewDemo.Resources.Images.album2.png", assembly)
                },
                new MusicItem 
                { 
                    Name = "When Doves Cry", 
                    Author = "Prince",
                    ItemImage = ImageSource.FromResource(
                        "ParallaxViewDemo.Resources.Images.album3.png", assembly)
                },
                new MusicItem 
                { 
                    Name = "I Wanna Dance", 
                    Author = "Whitney Houston",
                    ItemImage = ImageSource.FromResource(
                        "ParallaxViewDemo.Resources.Images.album4.png", assembly)
                },
                new MusicItem 
                { 
                    Name = "Rolling in the Deep", 
                    Author = "Adele",
                    ItemImage = ImageSource.FromResource(
                        "ParallaxViewDemo.Resources.Images.album5.png", assembly)
                },
                // Add more items...
            };
        }
    }
    
    public class MusicItem
    {
        public string Name { get; set; }
        public string Author { get; set; }
        public ImageSource ItemImage { get; set; }
    }
}
```

### Programmatic ListView Binding

**C# (Complete):**
```csharp
using Syncfusion.Maui.ParallaxView;
using Syncfusion.Maui.ListView;

namespace ParallaxViewDemo
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            
            var viewModel = new ParallaxViewModel();
            BindingContext = viewModel;
            
            // Create parallax view
            var parallaxView = new SfParallaxView();
            
            // Set background
            var backgroundImage = new Image
            {
                Source = viewModel.BackgroundImage,
                BackgroundColor = Colors.Transparent,
                HorizontalOptions = LayoutOptions.Fill,
                VerticalOptions = LayoutOptions.Fill,
                Aspect = Aspect.AspectFill
            };
            parallaxView.Content = backgroundImage;
            
            // Create ListView
            var listView = new SfListView
            {
                ItemsSource = viewModel.Items,
                BackgroundColor = Colors.Transparent,
                ItemSize = 100,
                ItemTemplate = new DataTemplate(() =>
                {
                    var grid = new Grid { Padding = 10 };
                    grid.ColumnDefinitions.Add(new ColumnDefinition { Width = 90 });
                    grid.ColumnDefinitions.Add(new ColumnDefinition { Width = GridLength.Star });
                    
                    var itemImage = new Image
                    {
                        HeightRequest = 80,
                        WidthRequest = 80,
                        Aspect = Aspect.AspectFit
                    };
                    itemImage.SetBinding(Image.SourceProperty, "ItemImage");
                    Grid.SetColumn(itemImage, 0);
                    
                    var stackLayout = new StackLayout 
                    { 
                        VerticalOptions = LayoutOptions.Center,
                        Spacing = 5 
                    };
                    
                    var nameLabel = new Label
                    {
                        FontSize = 18,
                        FontAttributes = FontAttributes.Bold,
                        TextColor = Colors.White
                    };
                    nameLabel.SetBinding(Label.TextProperty, "Name");
                    
                    var authorLabel = new Label
                    {
                        FontSize = 14,
                        TextColor = Colors.LightGray
                    };
                    authorLabel.SetBinding(Label.TextProperty, "Author");
                    
                    stackLayout.Children.Add(nameLabel);
                    stackLayout.Children.Add(authorLabel);
                    Grid.SetColumn(stackLayout, 1);
                    
                    grid.Children.Add(itemImage);
                    grid.Children.Add(stackLayout);
                    
                    return new ViewCell { View = grid };
                })
            };
            
            // Bind ListView as Source
            parallaxView.Source = listView;
            
            // Add to grid
            var mainGrid = new Grid();
            mainGrid.Children.Add(parallaxView);
            mainGrid.Children.Add(listView);
            
            this.Content = mainGrid;
        }
    }
}
```

## Content Auto-Stretch Behavior

**Important:** The size of the Content view will automatically be stretched to match the size of the Source view. This means:

- You don't need to manually calculate or set Content dimensions
- The background will always cover the entire scrollable area
- The parallax effect works seamlessly across all scroll positions
- The Content maintains its aspect ratio while filling the available space

This auto-stretch behavior ensures that there are no gaps or visual glitches during scrolling, regardless of the Source content size.

## GitHub Sample Links

For complete working examples, check out the official Syncfusion sample repository:
[MAUI Parallax View Sample Demos](https://github.com/SyncfusionExamples/MAUI-Parallax-View-Sample-Demos)

## Known Issues and Workarounds

### Android Image Sizing Issue

**Issue:** In Android, when an image's pixel size cannot stretch to fit the Parallax View Source control during loading, it may result in a `Java.Lang.RuntimeException`.

**GitHub Issue:** [dotnet/maui#11230](https://github.com/dotnet/maui/issues/11230)

**Workaround:** Use images with appropriate resolution that can be scaled without quality loss. Ensure images are large enough to cover the expected scroll area without degradation.

**Best Practices:**
- Use high-resolution images for backgrounds (at least 2x the expected display size)
- Test on target Android devices to verify image loading
- Consider using vector graphics or scalable formats when possible
- Implement error handling for image loading failures

## Next Steps

- **[Customization](customization.md)** - Learn how to adjust speed and orientation for different effects
- **[Custom Controls](custom-controls.md)** - Implement IParallaxView for custom scrollable controls

## Troubleshooting

### Issue: Parallax effect not visible
**Cause:** Source control has an opaque background  
**Solution:** Set `BackgroundColor="Transparent"` on the Source control (ScrollView or ListView)

### Issue: Content doesn't cover entire scroll area
**Cause:** Content view is smaller than expected  
**Solution:** The Content should auto-stretch. Verify the Source has a defined size and the Content uses `HorizontalOptions="Fill"` and `VerticalOptions="Fill"`

### Issue: Jerky or laggy scrolling
**Cause:** Complex Content or large images  
**Solution:** Optimize images, reduce Content complexity, or adjust the Speed property to reduce motion intensity
