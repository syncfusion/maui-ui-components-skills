# Customization Options

The Syncfusion .NET MAUI Parallax View provides powerful customization options to fine-tune the parallax effect and adapt it to different UI designs and orientations.

## Speed Customization

The `Speed` property is the primary way to control the intensity and behavior of the parallax effect. It determines how fast the background Content scrolls relative to the foreground Source.

### Understanding Speed Values

The Speed property accepts a double value between **0.0** and **1.0**:

| Speed Value | Behavior | Effect |
|-------------|----------|--------|
| **0.0** | Background doesn't move at all | Static background (no parallax) |
| **0.3 - 0.5** | Background moves slowly | Subtle, elegant parallax effect (recommended) |
| **0.6 - 0.8** | Background moves moderately | Noticeable parallax motion |
| **1.0** | Background moves at same speed as foreground | No parallax effect (synchronized scrolling) |

**Default:** 0.5 (50% of foreground scroll speed)

**Recommended range:** 0.3 - 0.6 for most applications

### Setting Speed in XAML

```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    xmlns:parallax="clr-namespace:Syncfusion.Maui.ParallaxView;assembly=Syncfusion.Maui.ParallaxView"
    xmlns:list="clr-namespace:Syncfusion.Maui.ListView;assembly=Syncfusion.Maui.ListView"
    x:Class="ParallaxViewDemo.MainPage">
    
    <Grid>
        <!-- Subtle parallax with Speed=0.4 -->
        <parallax:SfParallaxView Source="{x:Reference listView}" 
                                  x:Name="parallaxView" 
                                  Speed="0.4">
            <parallax:SfParallaxView.Content>
                <Image Source="{Binding BackgroundImage}" 
                       BackgroundColor="Transparent"
                       HorizontalOptions="Fill" 
                       VerticalOptions="Fill" 
                       Aspect="AspectFill" />
            </parallax:SfParallaxView.Content>
        </parallax:SfParallaxView>
        
        <list:SfListView x:Name="listView" 
                         ItemsSource="{Binding Items}" 
                         BackgroundColor="Transparent"
                         ItemSize="100">
            <!-- ItemTemplate here -->
        </list:SfListView>
    </Grid>
</ContentPage>
```

### Setting Speed in C#

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
            
            ParallaxViewModel viewModel = new ParallaxViewModel();
            BindingContext = viewModel;
            
            // Create parallax view with custom speed
            SfParallaxView parallax = new SfParallaxView
            {
                Speed = 0.4 // Set custom speed
            };
            
            // Set background image
            Image backgroundImage = new Image
            {
                Source = viewModel.BackgroundImage,
                BackgroundColor = Colors.Transparent,
                HorizontalOptions = LayoutOptions.Fill,
                VerticalOptions = LayoutOptions.Fill,
                Aspect = Aspect.AspectFill
            };
            parallax.Content = backgroundImage;
            
            // Create ListView
            SfListView listView = new SfListView
            {
                ItemsSource = viewModel.Items,
                BackgroundColor = Colors.Transparent,
                ItemSize = 100
            };
            
            // Bind ListView as Source
            parallax.Source = listView;
            
            // Add to grid
            Grid grid = new Grid();
            grid.Children.Add(parallax);
            grid.Children.Add(listView);
            
            this.Content = grid;
        }
    }
}
```

### Speed Selection Guidelines

**For Hero Sections (Speed = 0.3 - 0.4):**
- Subtle, professional look
- Background barely moves, creating depth without distraction
- Ideal for marketing pages, landing screens

**For Content Readers (Speed = 0.4 - 0.5):**
- Balanced parallax effect
- Adds visual interest without overwhelming text
- Good for article readers, blog posts

**For Image Galleries (Speed = 0.5 - 0.7):**
- More pronounced parallax
- Highlights the scrolling motion
- Works well for visual-heavy content

**For Immersive Experiences (Speed = 0.7 - 0.8):**
- Strong parallax effect
- Background moves almost as fast as foreground
- Use sparingly for dramatic effect

### Dynamic Speed Changes

You can change the speed at runtime based on user preferences or context:

```csharp
// Adjust speed based on user preference
public void SetParallaxIntensity(string intensity)
{
    switch (intensity)
    {
        case "subtle":
            parallaxView.Speed = 0.3;
            break;
        case "normal":
            parallaxView.Speed = 0.5;
            break;
        case "intense":
            parallaxView.Speed = 0.7;
            break;
        case "none":
            parallaxView.Speed = 0.0; // Disable parallax
            break;
    }
}
```

## Orientation Customization

The `Orientation` property controls the scroll direction of the parallax effect. By default, it's set to Vertical, but you can change it to Horizontal for side-scrolling parallax experiences.

### Available Orientations

- **Vertical** (default) - Top-to-bottom scrolling
- **Horizontal** - Left-to-right scrolling

**Important:** The Source control's orientation must match the ParallaxView's orientation for the effect to work correctly.

### Horizontal Parallax Example

**XAML:**
```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    xmlns:parallax="clr-namespace:Syncfusion.Maui.ParallaxView;assembly=Syncfusion.Maui.ParallaxView"
    xmlns:list="clr-namespace:Syncfusion.Maui.ListView;assembly=Syncfusion.Maui.ListView"
    x:Class="ParallaxViewDemo.MainPage">
    
    <Grid>
        <!-- Horizontal Parallax View -->
        <parallax:SfParallaxView Source="{x:Reference listView}" 
                                  x:Name="parallaxView" 
                                  Orientation="Horizontal"
                                  Speed="0.5">
            <parallax:SfParallaxView.Content>
                <Image Source="{Binding BackgroundImage}" 
                       BackgroundColor="Transparent"
                       HorizontalOptions="Fill" 
                       VerticalOptions="Fill" 
                       Aspect="AspectFill" />
            </parallax:SfParallaxView.Content>
        </parallax:SfParallaxView>
        
        <!-- Horizontal ListView (Orientation MUST match) -->
        <list:SfListView x:Name="listView" 
                         Orientation="Horizontal"
                         ItemsSource="{Binding Items}" 
                         BackgroundColor="Transparent"
                         ItemSize="200">
            <list:SfListView.ItemTemplate>
                <DataTemplate>
                    <ViewCell>
                        <StackLayout Padding="10" WidthRequest="200">
                            <Image Source="{Binding ItemImage}" 
                                   HeightRequest="150" 
                                   Aspect="AspectFill" />
                            <Label Text="{Binding Name}" 
                                   FontSize="16" 
                                   TextColor="White" />
                        </StackLayout>
                    </ViewCell>
                </DataTemplate>
            </list:SfListView.ItemTemplate>
        </list:SfListView>
    </Grid>
</ContentPage>
```

**C#:**
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
            
            ParallaxViewModel viewModel = new ParallaxViewModel();
            BindingContext = viewModel;
            
            // Create horizontal parallax view
            SfParallaxView parallax = new SfParallaxView
            {
                Orientation = Syncfusion.Maui.ParallaxView.Orientation.Horizontal,
                Speed = 0.5
            };
            
            // Set background
            Image backgroundImage = new Image
            {
                Source = viewModel.BackgroundImage,
                BackgroundColor = Colors.Transparent,
                HorizontalOptions = LayoutOptions.Fill,
                VerticalOptions = LayoutOptions.Fill,
                Aspect = Aspect.AspectFill
            };
            parallax.Content = backgroundImage;
            
            // Create horizontal ListView
            SfListView listView = new SfListView
            {
                Orientation = ItemsLayoutOrientation.Horizontal,
                ItemsSource = viewModel.Items,
                BackgroundColor = Colors.Transparent,
                ItemSize = 200
            };
            
            // Bind ListView as Source
            parallax.Source = listView;
            
            // Add to grid
            Grid grid = new Grid();
            grid.Children.Add(parallax);
            grid.Children.Add(listView);
            
            this.Content = grid;
        }
    }
}
```

### Horizontal ScrollView Example

For simpler horizontal scrolling without a list:

```xaml
<Grid>
    <parallax:SfParallaxView Source="{x:Reference scrollView}" 
                              Orientation="Horizontal"
                              Speed="0.4">
        <parallax:SfParallaxView.Content>
            <Image Source="wide_background.jpg" Aspect="AspectFill" />
        </parallax:SfParallaxView.Content>
    </parallax:SfParallaxView>
    
    <ScrollView x:Name="scrollView" 
                Orientation="Horizontal" 
                BackgroundColor="Transparent">
        <HorizontalStackLayout Spacing="20" Padding="20">
            <Frame WidthRequest="300" HeightRequest="400">
                <Label Text="Card 1" />
            </Frame>
            <Frame WidthRequest="300" HeightRequest="400">
                <Label Text="Card 2" />
            </Frame>
            <Frame WidthRequest="300" HeightRequest="400">
                <Label Text="Card 3" />
            </Frame>
        </HorizontalStackLayout>
    </ScrollView>
</Grid>
```

## Combining Speed and Orientation

You can combine both properties for maximum customization:

```xaml
<!-- Fast horizontal parallax -->
<parallax:SfParallaxView Source="{x:Reference scrollView}" 
                          Orientation="Horizontal"
                          Speed="0.7">
    <parallax:SfParallaxView.Content>
        <Image Source="panorama.jpg" Aspect="AspectFill" />
    </parallax:SfParallaxView.Content>
</parallax:SfParallaxView>
```

```csharp
// Slow vertical parallax
var parallax = new SfParallaxView
{
    Orientation = Syncfusion.Maui.ParallaxView.Orientation.Vertical,
    Speed = 0.3,
    Content = new Image 
    { 
        Source = "background.jpg", 
        Aspect = Aspect.AspectFill 
    }
};
```

## Performance Considerations

### Optimizing for Smooth Scrolling

**Image Optimization:**
- Use appropriately sized images (don't load 4K images for mobile displays)
- Compress images to reduce memory usage
- Consider using cached images for better performance

**Speed Selection:**
- Lower speed values (0.3-0.5) generally perform better
- Higher speeds require more frequent background updates

**Content Complexity:**
- Keep Content views simple (avoid complex layouts with many nested views)
- Single images perform better than complex StackLayouts
- Avoid animations within the Content view

### Testing Recommendations

- Test on low-end devices to ensure smooth scrolling
- Monitor frame rates during scrolling
- Test with different image sizes and resolutions
- Verify performance on both Android and iOS platforms

## Common Customization Patterns

### Subtle Background Motion
```csharp
Speed = 0.3, Orientation = Vertical
```
Best for professional apps, news readers, documentation

### Standard Parallax Effect
```csharp
Speed = 0.5, Orientation = Vertical
```
Best for general use, balanced visual interest

### Dramatic Parallax
```csharp
Speed = 0.7, Orientation = Vertical
```
Best for visual-heavy apps, galleries, portfolios

### Horizontal Carousel
```csharp
Speed = 0.5, Orientation = Horizontal
```
Best for image galleries, product showcases

### Disable Parallax (Static Background)
```csharp
Speed = 0.0
```
Useful for accessibility or user preference settings

## Next Steps

- **[Custom Controls](custom-controls.md)** - Implement IParallaxView for custom scrollable controls
- **[Getting Started](getting-started.md)** - Review basic setup and installation
- **[Source Binding](source-binding.md)** - Learn about Source property and control integration

## Troubleshooting

### Issue: Parallax effect too subtle/strong
**Solution:** Adjust the Speed property. Start with 0.5 and increment/decrement by 0.1 until desired effect is achieved.

### Issue: Horizontal parallax not working
**Solution:** Ensure both the SfParallaxView.Orientation AND the Source control's orientation are set to Horizontal.

### Issue: Laggy scrolling with parallax
**Solution:** Reduce the Speed value, optimize Content images, or simplify the Content layout structure.

### Issue: Background moves at same speed as foreground
**Solution:** Verify Speed is not set to 1.0. Use values between 0.3-0.7 for visible parallax effect.
