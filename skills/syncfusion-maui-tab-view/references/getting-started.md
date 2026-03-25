# Getting Started with .NET MAUI Tab View

## Table of Contents
- [NuGet Package Installation](#nuget-package-installation)
- [Register Syncfusion Handler](#register-syncfusion-handler)
- [Basic Tab View Implementation](#basic-tab-view-implementation)
- [Populating Tab Items Using Items Collection](#populating-tab-items-using-items-collection)
- [Data Binding with ItemsSource](#data-binding-with-itemssource)
- [HeaderItemTemplate Customization](#headeritemtemplate-customization)
- [ContentItemTemplate Customization](#contentitemtemplate-customization)
- [DataTemplateSelector for Conditional Templates](#datatemplateselector-for-conditional-templates)
- [Common Gotchas](#common-gotchas)
- [Next Steps](#next-steps)

---

This guide covers installation, setup, and basic implementation of Syncfusion .NET MAUI Tab View (SfTabView) control.

## NuGet Package Installation

### Step 1: Install Syncfusion.Maui.TabView Package

**Visual Studio:**
1. Right-click the project in Solution Explorer
2. Select **Manage NuGet Packages**
3. Search for `Syncfusion.Maui.TabView`
4. Install the latest version
5. Ensure dependencies are restored

**Package Manager Console:**
```powershell
Install-Package Syncfusion.Maui.TabView
```

**dotnet CLI:**
```bash
dotnet add package Syncfusion.Maui.TabView
```

The package includes dependency on `Syncfusion.Maui.Core` which is automatically installed.

## Register Syncfusion Handler

**Critical Step:** Register the Syncfusion Core handler in `MauiProgram.cs`:

```csharp
using Microsoft.Extensions.Logging;
using Syncfusion.Maui.Core.Hosting;

namespace TabViewGettingStarted
{
    public static class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            var builder = MauiApp.CreateBuilder();
            builder
                .UseMauiApp<App>()
                .ConfigureSyncfusionCore()  // Required for all Syncfusion controls
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

**Without `.ConfigureSyncfusionCore()`, the Tab View will not render properly.**

## Basic Tab View Implementation

### Initialize in XAML

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:tabView="clr-namespace:Syncfusion.Maui.TabView;assembly=Syncfusion.Maui.TabView"
             x:Class="TabViewGettingStarted.MainPage">
    
    <ContentPage.Content>
        <tabView:SfTabView x:Name="tabView" />
    </ContentPage.Content>
    
</ContentPage>
```

### Initialize in C#

```csharp
using Syncfusion.Maui.TabView;

namespace TabViewGettingStarted
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            
            SfTabView tabView = new SfTabView();
            this.Content = tabView;
        }
    }
}
```

## Populating Tab Items Using Items Collection

Add tabs directly to the `Items` property:

### XAML Approach

```xaml
<tabView:SfTabView x:Name="tabView">
    <tabView:SfTabView.Items>
        
        <tabView:SfTabItem Header="Call">
            <tabView:SfTabItem.Content>
                <Grid BackgroundColor="#E3F2FD">
                    <Label Text="Call List" 
                           HorizontalOptions="Center" 
                           VerticalOptions="Center"
                           FontSize="20" />
                </Grid>
            </tabView:SfTabItem.Content>
        </tabView:SfTabItem>
        
        <tabView:SfTabItem Header="Favorites">
            <tabView:SfTabItem.Content>
                <CollectionView>
                    <CollectionView.ItemsSource>
                        <x:Array Type="{x:Type x:String}">
                            <x:String>James</x:String>
                            <x:String>Richard</x:String>
                            <x:String>Michael</x:String>
                            <x:String>Alex</x:String>
                            <x:String>Clara</x:String>
                        </x:Array>
                    </CollectionView.ItemsSource>
                    <CollectionView.ItemTemplate>
                        <DataTemplate>
                            <Grid Margin="10,5" HeightRequest="40">
                                <Label VerticalOptions="Start"
                                       HorizontalOptions="Start"
                                       TextColor="#666666"
                                       FontSize="16"
                                       Text="{Binding}" />
                            </Grid>
                        </DataTemplate>
                    </CollectionView.ItemTemplate>
                </CollectionView>
            </tabView:SfTabItem.Content>
        </tabView:SfTabItem>
        
        <tabView:SfTabItem Header="Contacts">
            <tabView:SfTabItem.Content>
                <Grid BackgroundColor="#FFF3E0">
                    <Label Text="All Contacts" 
                           HorizontalOptions="Center" 
                           VerticalOptions="Center"
                           FontSize="20" />
                </Grid>
            </tabView:SfTabItem.Content>
        </tabView:SfTabItem>
        
    </tabView:SfTabView.Items>
</tabView:SfTabView>
```

### C# Approach

```csharp
using Syncfusion.Maui.TabView;

namespace TabViewGettingStarted
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            
            var tabView = new SfTabView();
            
            // First tab: Call
            var callTab = new SfTabItem { Header = "Call" };
            callTab.Content = new Grid
            {
                BackgroundColor = Color.FromArgb("#E3F2FD"),
                Children =
                {
                    new Label
                    {
                        Text = "Call List",
                        HorizontalOptions = LayoutOptions.Center,
                        VerticalOptions = LayoutOptions.Center,
                        FontSize = 20
                    }
                }
            };
            tabView.Items.Add(callTab);
            
            // Second tab: Favorites
            var favoritesTab = new SfTabItem { Header = "Favorites" };
            var collectionView = new CollectionView
            {
                ItemsSource = new string[] { "James", "Richard", "Michael", "Alex", "Clara" }
            };
            
            collectionView.ItemTemplate = new DataTemplate(() =>
            {
                var grid = new Grid
                {
                    Margin = new Thickness(10, 5),
                    HeightRequest = 40
                };
                var label = new Label
                {
                    VerticalOptions = LayoutOptions.Start,
                    HorizontalOptions = LayoutOptions.Start,
                    TextColor = Color.FromArgb("#666666"),
                    FontSize = 16
                };
                label.SetBinding(Label.TextProperty, ".");
                grid.Children.Add(label);
                return grid;
            });
            
            favoritesTab.Content = collectionView;
            tabView.Items.Add(favoritesTab);
            
            // Third tab: Contacts
            var contactsTab = new SfTabItem { Header = "Contacts" };
            contactsTab.Content = new Grid
            {
                BackgroundColor = Color.FromArgb("#FFF3E0"),
                Children =
                {
                    new Label
                    {
                        Text = "All Contacts",
                        HorizontalOptions = LayoutOptions.Center,
                        VerticalOptions = LayoutOptions.Center,
                        FontSize = 20
                    }
                }
            };
            tabView.Items.Add(contactsTab);
            
            this.Content = tabView;
        }
    }
}
```

## Data Binding with ItemsSource

For dynamic tab generation from collections, use `ItemsSource` with templates.

### Create Model Class

```csharp
using System.ComponentModel;

public class TabModel : INotifyPropertyChanged
{
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
    
    private string name;
    public string Name
    {
        get => name;
        set
        {
            name = value;
            OnPropertyChanged(nameof(Name));
        }
    }
}
```

### Create ViewModel

```csharp
using System.Collections.ObjectModel;
using System.ComponentModel;

public class TabItemsSourceViewModel : INotifyPropertyChanged
{
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
    
    private ObservableCollection<TabModel> tabItems;
    public ObservableCollection<TabModel> TabItems
    {
        get => tabItems;
        set
        {
            tabItems = value;
            OnPropertyChanged(nameof(TabItems));
        }
    }
    
    public TabItemsSourceViewModel()
    {
        TabItems = new ObservableCollection<TabModel>
        {
            new TabModel { Name = "Alexandar" },
            new TabModel { Name = "Gabriella" },
            new TabModel { Name = "Clara" },
            new TabModel { Name = "Tye" },
            new TabModel { Name = "Nora" },
            new TabModel { Name = "Sebastian" }
        };
    }
}
```

### Bind ItemsSource in XAML

```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:TabViewItemTemplateSample"
             xmlns:tabView="clr-namespace:Syncfusion.Maui.TabView;assembly=Syncfusion.Maui.TabView"
             x:Class="TabViewItemTemplateSample.MainPage">
    
    <ContentPage.BindingContext>
        <local:TabItemsSourceViewModel />
    </ContentPage.BindingContext>
    
    <tabView:SfTabView ItemsSource="{Binding TabItems}" />
    
</ContentPage>
```

### Bind ItemsSource in C#

```csharp
using Syncfusion.Maui.TabView;

namespace TabViewItemTemplateSample
{
    public partial class MainPage : ContentPage
    {
        TabItemsSourceViewModel model;
        SfTabView tabView;
        
        public MainPage()
        {
            InitializeComponent();
            
            model = new TabItemsSourceViewModel();
            this.BindingContext = model;
            
            tabView = new SfTabView();
            tabView.ItemsSource = model.TabItems;
            
            this.Content = tabView;
        }
    }
}
```

## HeaderItemTemplate Customization

Define custom UI for tab headers:

```xaml
<tabView:SfTabView ItemsSource="{Binding TabItems}">
    <tabView:SfTabView.HeaderItemTemplate>
        <DataTemplate>
            <Label Padding="5,10,10,10" 
                   Text="{Binding Name}"
                   FontSize="16"
                   TextColor="#333333" />
        </DataTemplate>
    </tabView:SfTabView.HeaderItemTemplate>
</tabView:SfTabView>
```

**C# Equivalent:**

```csharp
tabView.HeaderItemTemplate = new DataTemplate(() =>
{
    var nameLabel = new Label 
    { 
        Padding = new Thickness(5, 10, 10, 10),
        FontSize = 16,
        TextColor = Color.FromArgb("#333333")
    };
    nameLabel.SetBinding(Label.TextProperty, "Name");
    return nameLabel;
});
```

## ContentItemTemplate Customization

Define custom UI for tab content:

```xaml
<tabView:SfTabView ItemsSource="{Binding TabItems}">
    <tabView:SfTabView.HeaderItemTemplate>
        <DataTemplate>
            <Label Padding="5,10,10,10" Text="{Binding Name}" />
        </DataTemplate>
    </tabView:SfTabView.HeaderItemTemplate>
    
    <tabView:SfTabView.ContentItemTemplate>
        <DataTemplate>
            <Grid Padding="20">
                <StackLayout VerticalOptions="Center" HorizontalOptions="Center">
                    <Label Text="{Binding Name}" 
                           FontSize="24" 
                           FontAttributes="Bold"
                           HorizontalOptions="Center" />
                    <Label Text="Content for this tab" 
                           FontSize="16" 
                           TextColor="#666666"
                           HorizontalOptions="Center"
                           Margin="0,10,0,0" />
                </StackLayout>
            </Grid>
        </DataTemplate>
    </tabView:SfTabView.ContentItemTemplate>
</tabView:SfTabView>
```

**C# Equivalent:**

```csharp
tabView.ContentItemTemplate = new DataTemplate(() =>
{
    var grid = new Grid { Padding = new Thickness(20) };
    var stackLayout = new StackLayout
    {
        VerticalOptions = LayoutOptions.Center,
        HorizontalOptions = LayoutOptions.Center
    };
    
    var titleLabel = new Label
    {
        FontSize = 24,
        FontAttributes = FontAttributes.Bold,
        HorizontalOptions = LayoutOptions.Center
    };
    titleLabel.SetBinding(Label.TextProperty, "Name");
    
    var descLabel = new Label
    {
        Text = "Content for this tab",
        FontSize = 16,
        TextColor = Color.FromArgb("#666666"),
        HorizontalOptions = LayoutOptions.Center,
        Margin = new Thickness(0, 10, 0, 0)
    };
    
    stackLayout.Children.Add(titleLabel);
    stackLayout.Children.Add(descLabel);
    grid.Children.Add(stackLayout);
    
    return grid;
});
```

## DataTemplateSelector for Conditional Templates

Use `DataTemplateSelector` to apply different templates based on conditions:

### Define Model with Condition Property

```csharp
public class TabItemModel
{
    public string Title { get; set; }
    public bool IsImportant { get; set; }
}
```

### Create DataTemplateSelector

```csharp
public class TabHeaderTemplateSelector : DataTemplateSelector
{
    public DataTemplate NormalTemplate { get; set; }
    public DataTemplate ImportantTemplate { get; set; }
    
    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        var viewModel = item as TabItemModel;
        return viewModel?.IsImportant == true ? ImportantTemplate : NormalTemplate;
    }
}
```

### Apply in XAML

```xaml
<ContentPage.Resources>
    <DataTemplate x:Key="NormalHeaderTemplate">
        <Label Text="{Binding Title}" 
               VerticalTextAlignment="Center" 
               Margin="10" />
    </DataTemplate>
    
    <DataTemplate x:Key="ImportantHeaderTemplate">
        <StackLayout Orientation="Horizontal" Spacing="10" Margin="10">
            <Image Source="star.png" WidthRequest="16" HeightRequest="16" />
            <Label Text="{Binding Title}" 
                   FontAttributes="Bold" 
                   VerticalTextAlignment="Center"
                   TextColor="DarkGoldenrod" />
        </StackLayout>
    </DataTemplate>
    
    <local:TabHeaderTemplateSelector x:Key="TabHeaderTemplateSelector"
                                      NormalTemplate="{StaticResource NormalHeaderTemplate}"
                                      ImportantTemplate="{StaticResource ImportantHeaderTemplate}" />
</ContentPage.Resources>

<tabView:SfTabView ItemsSource="{Binding Tabs}" 
                   HeaderItemTemplate="{StaticResource TabHeaderTemplateSelector}" />
```

### ViewModel for Selector

```csharp
public class TabItemViewModel : INotifyPropertyChanged
{
    public ObservableCollection<TabItemModel> Tabs { get; }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    public TabItemViewModel()
    {
        Tabs = new ObservableCollection<TabItemModel>
        {
            new TabItemModel { Title = "Profile" },
            new TabItemModel { Title = "Notifications", IsImportant = true },
            new TabItemModel { Title = "Settings" }
        };
    }
    
    protected void OnPropertyChanged(string propertyName)
        => PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
}
```

## Common Gotchas

1. **Forgetting `.ConfigureSyncfusionCore()`** - Tabs won't render without this in MauiProgram.cs
2. **Empty Content** - Always set Content property for each SfTabItem
3. **Binding Errors** - Ensure model properties match DataTemplate bindings
4. **Missing Namespace** - Include `xmlns:tabView` namespace in XAML
5. **ObservableCollection Not Updating** - Use `ObservableCollection<T>` for dynamic tabs, not `List<T>`

## Next Steps

- **Tab Bar Configuration:** Learn about width modes, placement, height, and styling
- **Tab Item Configuration:** Customize headers with icons, fonts, and templates
- **Events:** Handle tab selection and implement custom navigation logic
- **Nested Tabs:** Create multi-level tab hierarchies
- **Visual Customization:** Apply themes, indicators, and Liquid Glass effects
