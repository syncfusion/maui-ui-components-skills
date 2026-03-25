# Getting Started with .NET MAUI Chips

This guide walks through the complete setup process for Syncfusion .NET MAUI Chips, from installation to creating your first working chip implementation.

## Step 1: Install the NuGet Package

### Visual Studio

1. Right-click your project in **Solution Explorer**
2. Select **Manage NuGet Packages**
3. Search for `Syncfusion.Maui.Core`
4. Install the latest version

### Visual Studio Code / CLI

```bash
dotnet add package Syncfusion.Maui.Core
dotnet restore
```

The `Syncfusion.Maui.Core` package is required as it's the core dependency for all Syncfusion .NET MAUI controls.

## Step 2: Register the Syncfusion Handler

In your `MauiProgram.cs` file, register the Syncfusion Core handler:

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
                .ConfigureSyncfusionCore()  // Register Syncfusion Core
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

**Critical:** The `.ConfigureSyncfusionCore()` call is required for all Syncfusion controls to function properly.

## Step 3: Add the Namespace

In your XAML file, add the Syncfusion Chips namespace:

```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:chip="clr-namespace:Syncfusion.Maui.Core;assembly=Syncfusion.Maui.Core"
             x:Class="YourApp.MainPage">
    <!-- Your content here -->
</ContentPage>
```

For C# code-behind:

```csharp
using Syncfusion.Maui.Core;
```

## Step 4: Create Your First SfChip

### Simple Individual Chip

**XAML:**
```xaml
<ContentPage.Content>
    <chip:SfChip x:Name="chip" 
                 Text="Hello Chip" />
</ContentPage.Content>
```

**C#:**
```csharp
SfChip chip = new SfChip
{
    Text = "Hello Chip"
};
Content = chip;
```

### Chip with Close Button

```xaml
<chip:SfChip Text="Removable Chip"
             ShowCloseButton="True"
             CloseButtonColor="Red" />
```

```csharp
SfChip chip = new SfChip
{
    Text = "Removable Chip",
    ShowCloseButton = true,
    CloseButtonColor = Colors.Red
};
```

## Step 5: Create Your First SfChipGroup

### Empty ChipGroup

**XAML:**
```xaml
<ContentPage.Content>
    <Grid>
        <chip:SfChipGroup />
    </Grid>
</ContentPage.Content>
```

**C#:**
```csharp
Grid grid = new Grid();
SfChipGroup chipGroup = new SfChipGroup();
grid.Children.Add(chipGroup);
this.Content = grid;
```

### ChipGroup with Layout

The ChipGroup requires a layout to arrange chips. Common layouts include `FlexLayout` and `StackLayout`.

**XAML with FlexLayout:**
```xaml
<ContentPage.Content>
    <Grid>
        <chip:SfChipGroup DisplayMemberPath="Name">
            <chip:SfChipGroup.ChipLayout>
                <FlexLayout HorizontalOptions="Start" 
                           VerticalOptions="Center" />
            </chip:SfChipGroup.ChipLayout>
        </chip:SfChipGroup>
    </Grid>
</ContentPage.Content>
```

**C#:**
```csharp
Grid grid = new Grid();
SfChipGroup chipGroup = new SfChipGroup
{
    DisplayMemberPath = "Name"
};

FlexLayout layout = new FlexLayout
{
    HorizontalOptions = LayoutOptions.Start,
    VerticalOptions = LayoutOptions.Center
};
chipGroup.ChipLayout = layout;

grid.Children.Add(chipGroup);
this.Content = grid;
```

## Step 6: Add Chips with Data Binding

### Create Model and ViewModel

**Model:**
```csharp
public class Person
{
    public string Name { get; set; }
}
```

**ViewModel:**
```csharp
using System.Collections.ObjectModel;
using System.ComponentModel;

public class ViewModel : INotifyPropertyChanged
{
    private ObservableCollection<Person> employees;
    
    public ObservableCollection<Person> Employees
    {
        get { return employees; }
        set 
        { 
            employees = value; 
            OnPropertyChanged(nameof(Employees)); 
        }
    }
    
    public ViewModel()
    {
        employees = new ObservableCollection<Person>();
        employees.Add(new Person { Name = "John" });
        employees.Add(new Person { Name = "James" });
        employees.Add(new Person { Name = "Linda" });
        employees.Add(new Person { Name = "Rose" });
        employees.Add(new Person { Name = "Mark" });
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

### Bind to ChipGroup

**XAML:**
```xaml
<ContentPage.BindingContext>
    <local:ViewModel />
</ContentPage.BindingContext>

<ContentPage.Content>
    <Grid>
        <chip:SfChipGroup ItemsSource="{Binding Employees}"
                         DisplayMemberPath="Name"
                         ChipPadding="8,8,0,0"
                         ChipBackground="White"
                         ChipTextColor="Black"
                         HorizontalOptions="Start"
                         VerticalOptions="Center">
            <chip:SfChipGroup.ChipLayout>
                <FlexLayout HorizontalOptions="Start" 
                           VerticalOptions="Center" />
            </chip:SfChipGroup.ChipLayout>
        </chip:SfChipGroup>
    </Grid>
</ContentPage.Content>
```

**C#:**
```csharp
this.BindingContext = new ViewModel();

Grid grid = new Grid();
SfChipGroup chipGroup = new SfChipGroup
{
    DisplayMemberPath = "Name",
    ChipPadding = new Thickness(8, 8, 0, 0),
    ChipBackground = Colors.White,
    ChipTextColor = Colors.Black,
    HorizontalOptions = LayoutOptions.Start,
    VerticalOptions = LayoutOptions.Center
};

FlexLayout layout = new FlexLayout
{
    HorizontalOptions = LayoutOptions.Start,
    VerticalOptions = LayoutOptions.Center
};
chipGroup.ChipLayout = layout;

chipGroup.SetBinding(SfChipGroup.ItemsSourceProperty, "Employees");
grid.Children.Add(chipGroup);
this.Content = grid;
```

## Complete Working Example

Here's a complete page with styled chips:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:chip="clr-namespace:Syncfusion.Maui.Core;assembly=Syncfusion.Maui.Core"
             xmlns:local="clr-namespace:YourApp"
             x:Class="YourApp.MainPage">
    
    <ContentPage.BindingContext>
        <local:ViewModel />
    </ContentPage.BindingContext>
    
    <ScrollView>
        <VerticalStackLayout Padding="20" Spacing="20">
            
            <!-- Single Chip Example -->
            <Label Text="Single Chip" FontAttributes="Bold" />
            <chip:SfChip Text="Category" 
                        Background="LightBlue"
                        TextColor="Black"
                        ShowCloseButton="True" />
            
            <!-- Chip Group Example -->
            <Label Text="Chip Group" FontAttributes="Bold" />
            <chip:SfChipGroup ItemsSource="{Binding Employees}"
                             DisplayMemberPath="Name"
                             ChipPadding="8,8,0,0"
                             ChipBackground="#E0E0E0"
                             ChipTextColor="Black"
                             ChipStroke="#9E9E9E"
                             ChipStrokeThickness="1">
                <chip:SfChipGroup.ChipLayout>
                    <FlexLayout Wrap="Wrap" 
                               HorizontalOptions="Start" 
                               VerticalOptions="Center" />
                </chip:SfChipGroup.ChipLayout>
            </chip:SfChipGroup>
            
        </VerticalStackLayout>
    </ScrollView>
</ContentPage>
```

## Common Setup Issues and Solutions

### Issue: Chips Not Appearing

**Solution:** Ensure you've called `.ConfigureSyncfusionCore()` in `MauiProgram.cs`.

### Issue: Chips Overlapping

**Solution:** Set `ChipPadding` property:
```xaml
<chip:SfChipGroup ChipPadding="8,8,0,0" ... />
```

### Issue: Layout Not Wrapping

**Solution:** Use `FlexLayout` with `Wrap` property:
```xaml
<chip:SfChipGroup.ChipLayout>
    <FlexLayout Wrap="Wrap" HorizontalOptions="Start" />
</chip:SfChipGroup.ChipLayout>
```

### Issue: Text Not Displaying from ItemsSource

**Solution:** Set `DisplayMemberPath` to the property name:
```xaml
<chip:SfChipGroup ItemsSource="{Binding Items}"
                 DisplayMemberPath="Name" />
```

## Next Steps

- **Chip Types:** Learn about Input, Choice, Filter, and Action chip types
- **Customization:** Explore styling options for colors, fonts, borders, and icons
- **Data Binding:** Advanced ItemsSource scenarios with images and complex objects
- **Events:** Handle user interactions with selection and click events
