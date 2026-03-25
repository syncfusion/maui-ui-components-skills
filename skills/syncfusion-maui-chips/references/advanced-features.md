# Advanced Features in .NET MAUI Chips

## Table of Contents
- [Applying Font Icons to Chips](#applying-font-icons-to-chips)
- [DataTemplateSelector](#datatemplateselector)
- [Liquid Glass Support](#liquid-glass-support)
- [IsSelected Property](#isselected-property)
- [AOT Publishing](#aot-publishing)
- [Platform-Specific Customizations](#platform-specific-customizations)

## Applying Font Icons to Chips

Use custom icon fonts (like Material Icons, Font Awesome) instead of image files for scalable, themeable icons.

### Step 1: Add Font File to Project

1. Add your `.ttf` font file to the `Resources/Fonts` folder
2. Register the font in `MauiProgram.cs`:

```csharp
public static class MauiProgram
{
    public static MauiApp CreateMauiApp()
    {
        var builder = MauiApp.CreateBuilder();
        builder
            .UseMauiApp<App>()
            .ConfigureSyncfusionCore()
            .ConfigureFonts(fonts =>
            {
                fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
                fonts.AddFont("materialdesignicons-webfont.ttf", "MaterialIcons");
            });

        return builder.Build();
    }
}
```

### Step 2: Create Helper Class

```csharp
public static class FontIcons
{
    // Material Design Icons Unicode characters
    public const string Home = "\uf02d0";
    public const string Settings = "\uf0493";
    public const string User = "\uf004a";
    public const string Email = "\uf01ee";
    public const string Phone = "\uf0403";
    public const string Calendar = "\uf00ed";
    public const string Location = "\uf0357";
    public const string Camera = "\uf00f1";
    public const string Delete = "\uf01b4";
    public const string Edit = "\uf0225";
}
```

### Step 3: Use in SfChip

```xaml
<chip:SfChip Text="Home"
             ShowIcon="True"
             FontFamily="MaterialIcons"
             ImageSource="{x:Static local:FontIcons.Home}"
             ImageSize="24" />
```

**Important:** When using font icons:
- Set `ShowIcon="True"`
- Set `FontFamily` to your registered font name
- Use `ImageSource` with the Unicode character
- The font icon will be rendered as text, not an image

### Step 4: Model with Font Icons

```csharp
public class MenuItem
{
    public string Name { get; set; }
    public string Icon { get; set; }  // Unicode character
}

public class MenuViewModel : INotifyPropertyChanged
{
    public ObservableCollection<MenuItem> MenuItems { get; set; }
    
    public MenuViewModel()
    {
        MenuItems = new ObservableCollection<MenuItem>
        {
            new MenuItem { Name = "Home", Icon = FontIcons.Home },
            new MenuItem { Name = "Settings", Icon = FontIcons.Settings },
            new MenuItem { Name = "Profile", Icon = FontIcons.User }
        };
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
}
```

### Complete Font Icon Example

```xaml
<chip:SfChipGroup ItemsSource="{Binding MenuItems}"
                  DisplayMemberPath="Name"
                  ImageMemberPath="Icon"
                  ShowIcon="True"
                  ChipImageSize="20"
                  ChipFontFamily="MaterialIcons"
                  ChipBackground="#F5F5F5"
                  ChipTextColor="#333333">
    <chip:SfChipGroup.ChipLayout>
        <FlexLayout Wrap="Wrap" HorizontalOptions="Start" />
    </chip:SfChipGroup.ChipLayout>
</chip:SfChipGroup>
```

### Benefits of Font Icons

- **Scalable:** No pixelation at any size
- **Themeable:** Change color with TextColor property
- **Lightweight:** Smaller file size than images
- **Consistent:** Same appearance across platforms

## DataTemplateSelector

Use `DataTemplateSelector` to display different chip styles based on data properties.

### Scenario: Priority-Based Styling

**Model:**
```csharp
public class Task
{
    public string Title { get; set; }
    public TaskPriority Priority { get; set; }
}

public enum TaskPriority
{
    Low,
    Medium,
    High,
    Critical
}
```

### Creating DataTemplateSelector

```csharp
public class TaskChipTemplateSelector : DataTemplateSelector
{
    public DataTemplate LowPriorityTemplate { get; set; }
    public DataTemplate MediumPriorityTemplate { get; set; }
    public DataTemplate HighPriorityTemplate { get; set; }
    public DataTemplate CriticalPriorityTemplate { get; set; }
    
    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        var task = item as Task;
        
        if (task == null)
            return LowPriorityTemplate;
        
        return task.Priority switch
        {
            TaskPriority.Low => LowPriorityTemplate,
            TaskPriority.Medium => MediumPriorityTemplate,
            TaskPriority.High => HighPriorityTemplate,
            TaskPriority.Critical => CriticalPriorityTemplate,
            _ => LowPriorityTemplate
        };
    }
}
```

### Using DataTemplateSelector in XAML

```xaml
<ContentPage.Resources>
    <ResourceDictionary>
        <!-- Define templates -->
        <DataTemplate x:Key="LowTemplate">
            <chip:SfChip Text="{Binding Title}"
                        Background="LightGreen"
                        TextColor="DarkGreen" />
        </DataTemplate>
        
        <DataTemplate x:Key="MediumTemplate">
            <chip:SfChip Text="{Binding Title}"
                        Background="LightYellow"
                        TextColor="DarkOrange" />
        </DataTemplate>
        
        <DataTemplate x:Key="HighTemplate">
            <chip:SfChip Text="{Binding Title}"
                        Background="LightCoral"
                        TextColor="DarkRed" />
        </DataTemplate>
        
        <DataTemplate x:Key="CriticalTemplate">
            <chip:SfChip Text="{Binding Title}"
                        Background="Red"
                        TextColor="White"
                        ShowIcon="True"
                        ImageSource="warning.png" />
        </DataTemplate>
        
        <!-- Define selector -->
        <local:TaskChipTemplateSelector x:Key="TaskSelector"
            LowPriorityTemplate="{StaticResource LowTemplate}"
            MediumPriorityTemplate="{StaticResource MediumTemplate}"
            HighPriorityTemplate="{StaticResource HighTemplate}"
            CriticalPriorityTemplate="{StaticResource CriticalTemplate}" />
    </ResourceDictionary>
</ContentPage.Resources>

<chip:SfChipGroup ItemsSource="{Binding Tasks}"
                  ItemTemplate="{StaticResource TaskSelector}">
    <chip:SfChipGroup.ChipLayout>
        <FlexLayout Wrap="Wrap" />
    </chip:SfChipGroup.ChipLayout>
</chip:SfChipGroup>
```

### Category-Based Templates

```csharp
public class CategoryChipSelector : DataTemplateSelector
{
    public DataTemplate ElectronicsTemplate { get; set; }
    public DataTemplate ClothingTemplate { get; set; }
    public DataTemplate BooksTemplate { get; set; }
    public DataTemplate DefaultTemplate { get; set; }
    
    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        var product = item as Product;
        
        if (product == null)
            return DefaultTemplate;
        
        return product.Category.ToLower() switch
        {
            "electronics" => ElectronicsTemplate,
            "clothing" => ClothingTemplate,
            "books" => BooksTemplate,
            _ => DefaultTemplate
        };
    }
}
```

## Liquid Glass Support

Liquid Glass is a visual effect that creates a translucent, frosted glass appearance.

**Note:** Liquid Glass support requires additional Syncfusion packages and is platform-specific. Check Syncfusion documentation for current implementation details.

### Enabling Liquid Glass

```xaml
<!-- Example implementation (check Syncfusion docs for current syntax) -->
<chip:SfChip Text="Translucent Chip"
             Background="Transparent">
    <!-- Apply liquid glass effect via platform-specific settings -->
</chip:SfChip>
```

### Use Cases

- Modern, translucent UI designs
- Overlay chips on images or complex backgrounds
- Premium app aesthetics

## IsSelected Property

The `IsSelected` property allows programmatic control over chip selection in Choice and Filter types.

## AOT Publishing

When publishing with Ahead-of-Time (AOT) compilation (especially on iOS), follow these guidelines:

### Add Preserve Attribute

```csharp
using System.ComponentModel;
using Microsoft.Maui.Controls;

[Preserve(AllMembers = true)]
public class Person : INotifyPropertyChanged
{
    public string Name { get; set; }
    public string Email { get; set; }
    public string Image { get; set; }
    
    public event PropertyChangedEventHandler PropertyChanged;
}
```

**Why:** AOT compilation may strip unused code. The `[Preserve]` attribute ensures all properties remain available for runtime binding.

### ViewModel Preservation

```csharp
[Preserve(AllMembers = true)]
public class ViewModel : INotifyPropertyChanged
{
    public ObservableCollection<Person> Employees { get; set; }
    
    public event PropertyChangedEventHandler PropertyChanged;
}
```

### Ensure DisplayMemberPath Works

Without `[Preserve]`, DisplayMemberPath bindings may fail in AOT:

```csharp
// ❌ May fail in AOT without [Preserve]
public class Person
{
    public string Name { get; set; }
}

// ✅ Works in AOT
[Preserve(AllMembers = true)]
public class Person
{
    public string Name { get; set; }
}
```

### Project Configuration

Add to `.csproj` for iOS:

```xml
<PropertyGroup Condition="'$(TargetFramework)' == 'net9.0-ios'">
    <MtouchLink>SdkOnly</MtouchLink>
    <PublishTrimmed>true</PublishTrimmed>
    <TrimMode>partial</TrimMode>
</PropertyGroup>
```

## Platform-Specific Customizations

### iOS-Specific Styling

```csharp
#if IOS
using Microsoft.Maui.Controls.PlatformConfiguration.iOSSpecific;

public void ApplyiOSStyles()
{
    chip.On<Microsoft.Maui.Controls.PlatformConfiguration.iOS>()
        .SetPrefersLargeTitles(true);
}
#endif
```

### Android-Specific Elevation

```csharp
#if ANDROID
public void ApplyAndroidElevation()
{
    // Apply Material Design elevation
    chip.Shadow = new Shadow
    {
        Brush = Brush.Black,
        Opacity = 0.3f,
        Radius = 10,
        Offset = new Point(0, 2)
    };
}
#endif
```

### Platform Detection for Icons

```csharp
public string GetPlatformIcon()
{
    if (DeviceInfo.Platform == DevicePlatform.Android)
        return "android_icon.png";
    else if (DeviceInfo.Platform == DevicePlatform.iOS)
        return "ios_icon.png";
    else if (DeviceInfo.Platform == DevicePlatform.WinUI)
        return "windows_icon.png";
    else
        return "default_icon.png";
}
```

### Responsive Sizing

```csharp
public double GetChipSize()
{
    var displayInfo = DeviceDisplay.MainDisplayInfo;
    var density = displayInfo.Density;
    
    // Adjust chip size based on screen density
    if (density >= 3.0)
        return 40; // High DPI
    else if (density >= 2.0)
        return 36; // Medium DPI
    else
        return 32; // Low DPI
}
```

### Platform-Specific Fonts

```csharp
public string GetPlatformFont()
{
    if (DeviceInfo.Platform == DevicePlatform.Android)
        return "sans-serif-medium";
    else if (DeviceInfo.Platform == DevicePlatform.iOS)
        return "Helvetica-Bold";
    else if (DeviceInfo.Platform == DevicePlatform.WinUI)
        return "Segoe UI Semibold";
    else
        return "OpenSans-Semibold";
}
```

## Performance Tips

### Virtualization for Large Collections

For collections with 100+ items, consider:

```csharp
// Load in batches
public async Task LoadCategoriesAsync()
{
    const int batchSize = 50;
    var allCategories = await GetCategoriesFromDatabaseAsync();
    
    for (int i = 0; i < allCategories.Count; i += batchSize)
    {
        var batch = allCategories.Skip(i).Take(batchSize);
        foreach (var category in batch)
        {
            Categories.Add(category);
        }
        
        await Task.Delay(100); // Allow UI to update
    }
}
```

### Image Optimization

```csharp
// Cache images
private static Dictionary<string, ImageSource> imageCache = new();

public ImageSource GetCachedImage(string imageName)
{
    if (!imageCache.ContainsKey(imageName))
    {
        imageCache[imageName] = ImageSource.FromFile(imageName);
    }
    
    return imageCache[imageName];
}
