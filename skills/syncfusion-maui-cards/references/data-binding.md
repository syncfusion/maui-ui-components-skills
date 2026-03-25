# Data Binding with BindableLayout

## Table of Contents
- [Overview](#overview)
- [Setting Up ViewModel](#setting-up-viewmodel)
- [Populating CardLayout with Data](#populating-cardlayout-with-data)
- [Defining Card Appearance](#defining-card-appearance)
- [Complete Examples](#complete-examples)
- [Advanced Patterns](#advanced-patterns)
- [Best Practices](#best-practices)

## Overview

`SfCardLayout` supports data binding through .NET MAUI's `BindableLayout` feature. This allows you to dynamically generate cards from a data collection, making it easy to create data-driven card interfaces without manually creating each card.

**Key Benefits:**
- Automatic card generation from data sources
- Clean separation of data and UI
- MVVM pattern support
- Observable collection support for dynamic updates
- Simplified code maintenance

Since `SfCardLayout` is an extended class of `Layout<T>`, it inherits full BindableLayout capabilities.

## Setting Up ViewModel

### Step 1: Define Data Model

Create a simple model class that represents the data for each card:

```csharp
public class CardItem
{
    public string Title { get; set; }
    public string Description { get; set; }
    public Color BackgroundColor { get; set; }
    public string ImageUrl { get; set; }
}
```

**More Complex Model Example:**

```csharp
public class ProductCard
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Description { get; set; }
    public decimal Price { get; set; }
    public string ImageUrl { get; set; }
    public string Category { get; set; }
    public double Rating { get; set; }
    public bool InStock { get; set; }
}
```

### Step 2: Create ViewModel

Initialize a model collection in your ViewModel:

**Simple ViewModel:**

```csharp
public class ViewModel
{
    public ObservableCollection<string> Colors { get; set; }
    
    public ViewModel()
    {
        Colors = new ObservableCollection<string>
        {
            "Cyan",
            "Yellow",
            "Orange"
        };
    }
}
```

**MVVM ViewModel with INotifyPropertyChanged:**

```csharp
using System.Collections.ObjectModel;
using System.ComponentModel;

public class CardsViewModel : INotifyPropertyChanged
{
    private ObservableCollection<CardItem> _cards;
    
    public ObservableCollection<CardItem> Cards
    {
        get => _cards;
        set
        {
            _cards = value;
            OnPropertyChanged(nameof(Cards));
        }
    }
    
    public CardsViewModel()
    {
        LoadCards();
    }
    
    private void LoadCards()
    {
        Cards = new ObservableCollection<CardItem>
        {
            new CardItem
            {
                Title = "Morning Routine",
                Description = "Start your day right",
                BackgroundColor = Colors.Cyan
            },
            new CardItem
            {
                Title = "Work Tasks",
                Description = "Complete project milestones",
                BackgroundColor = Colors.Yellow
            },
            new CardItem
            {
                Title = "Evening Plans",
                Description = "Relax and unwind",
                BackgroundColor = Colors.Orange
            }
        };
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected virtual void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

### Step 3: Set BindingContext

Set the ViewModel instance as the BindingContext of your page.

**XAML:**

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:cards="clr-namespace:Syncfusion.Maui.Cards;assembly=Syncfusion.Maui.Cards"
             xmlns:local="clr-namespace:YourNamespace"
             x:Class="YourNamespace.CardPage">
    
    <ContentPage.BindingContext>
        <local:CardsViewModel/>
    </ContentPage.BindingContext>
    
    <!-- Your UI here -->
</ContentPage>
```

**C#:**

```csharp
public partial class CardPage : ContentPage
{
    public CardPage()
    {
        InitializeComponent();
        this.BindingContext = new CardsViewModel();
    }
}
```

## Populating CardLayout with Data

Use `BindableLayout.ItemsSource` to bind your data collection to the SfCardLayout.

**XAML:**

```xml
<cards:SfCardLayout BindableLayout.ItemsSource="{Binding Cards}"
                    HeightRequest="500"
                    SwipeDirection="Left"
                    BackgroundColor="#F0F0F0">
    <!-- ItemTemplate defined in next section -->
</cards:SfCardLayout>
```

**C#:**

```csharp
SfCardLayout cardLayout = new SfCardLayout
{
    HeightRequest = 500,
    SwipeDirection = CardSwipeDirection.Left,
    BackgroundColor = Color.FromArgb("#F0F0F0")
};

var viewModel = new CardsViewModel();
this.BindingContext = viewModel;

BindableLayout.SetItemsSource(cardLayout, viewModel.Cards);
```

## Defining Card Appearance

Use `BindableLayout.ItemTemplate` with a `DataTemplate` to define how each card should look.

### Simple Template

**XAML:**

```xml
<cards:SfCardLayout BindableLayout.ItemsSource="{Binding Colors}"
                    SwipeDirection="Left"
                    HeightRequest="300"
                    WidthRequest="300"
                    BackgroundColor="#F0F0F0">
    <BindableLayout.ItemTemplate>
        <DataTemplate>
            <cards:SfCardView BackgroundColor="{Binding}" CornerRadius="10">
                <Label Text="{Binding}" 
                       HorizontalOptions="Center" 
                       VerticalTextAlignment="Center"/>
            </cards:SfCardView>
        </DataTemplate>
    </BindableLayout.ItemTemplate>
</cards:SfCardLayout>
```

**C#:**

```csharp
SfCardLayout cardLayout = new SfCardLayout
{
    SwipeDirection = CardSwipeDirection.Left,
    BackgroundColor = Color.FromArgb("#F0F0F0"),
    HeightRequest = 300,
    WidthRequest = 300
};

this.BindingContext = new ViewModel();

DataTemplate dataTemplate = new DataTemplate(() =>
{
    SfCardView cardView = new SfCardView { CornerRadius = 10 };
    cardView.SetBinding(SfCardView.BackgroundColorProperty, ".");

    Label label = new Label
    {
        HorizontalOptions = LayoutOptions.Center,
        VerticalTextAlignment = TextAlignment.Center
    };
    label.SetBinding(Label.TextProperty, ".");
    
    cardView.Content = label;
    return cardView;
});

BindableLayout.SetItemTemplate(cardLayout, dataTemplate);
BindableLayout.SetItemsSource(cardLayout, ((ViewModel)BindingContext).Colors);

this.Content = cardLayout;
```

### Complex Template

**XAML:**

```xml
<cards:SfCardLayout BindableLayout.ItemsSource="{Binding Cards}"
                    SwipeDirection="Left"
                    HeightRequest="500"
                    BackgroundColor="#F0F0F0">
    <BindableLayout.ItemTemplate>
        <DataTemplate>
            <cards:SfCardView BackgroundColor="{Binding BackgroundColor}" 
                            CornerRadius="15"
                            Margin="10">
                <Grid Padding="20" RowSpacing="10">
                    <Grid.RowDefinitions>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="*"/>
                    </Grid.RowDefinitions>
                    
                    <Label Text="{Binding Title}" 
                           FontSize="24"
                           FontAttributes="Bold"
                           TextColor="White"
                           Grid.Row="0"/>
                    
                    <Label Text="{Binding Description}"
                           FontSize="16"
                           TextColor="White"
                           VerticalOptions="Center"
                           Grid.Row="1"/>
                </Grid>
            </cards:SfCardView>
        </DataTemplate>
    </BindableLayout.ItemTemplate>
</cards:SfCardLayout>
```

## Complete Examples

### Example 1: Task Cards

**Model:**

```csharp
public class TaskItem
{
    public string Title { get; set; }
    public string Description { get; set; }
    public DateTime DueDate { get; set; }
    public Priority Priority { get; set; }
    public bool IsCompleted { get; set; }
}

public enum Priority
{
    Low,
    Medium,
    High
}
```

**ViewModel:**

```csharp
public class TaskViewModel : INotifyPropertyChanged
{
    public ObservableCollection<TaskItem> Tasks { get; set; }
    
    public TaskViewModel()
    {
        Tasks = new ObservableCollection<TaskItem>
        {
            new TaskItem
            {
                Title = "Complete project proposal",
                Description = "Prepare and submit Q1 project proposal",
                DueDate = DateTime.Now.AddDays(3),
                Priority = Priority.High,
                IsCompleted = false
            },
            new TaskItem
            {
                Title = "Team meeting",
                Description = "Weekly sync with development team",
                DueDate = DateTime.Now.AddDays(1),
                Priority = Priority.Medium,
                IsCompleted = false
            },
            new TaskItem
            {
                Title = "Code review",
                Description = "Review pull requests from team members",
                DueDate = DateTime.Now,
                Priority = Priority.High,
                IsCompleted = false
            }
        };
    }
    
    public void AddTask(TaskItem task)
    {
        Tasks.Add(task);
    }
    
    public void RemoveTask(TaskItem task)
    {
        Tasks.Remove(task);
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected virtual void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

**XAML:**

```xml
<ContentPage.BindingContext>
    <local:TaskViewModel/>
</ContentPage.BindingContext>

<cards:SfCardLayout BindableLayout.ItemsSource="{Binding Tasks}"
                    SwipeDirection="Left"
                    ShowSwipedCard="True"
                    HeightRequest="550"
                    BackgroundColor="#F5F5F5">
    <BindableLayout.ItemTemplate>
        <DataTemplate>
            <cards:SfCardView BackgroundColor="White"
                            CornerRadius="12"
                            BorderWidth="1"
                            BorderColor="LightGray"
                            Margin="15">
                <!-- Indicator color based on priority -->
                <cards:SfCardView.IndicatorColor>
                    <Binding Path="Priority">
                        <Binding.Converter>
                            <local:PriorityToColorConverter/>
                        </Binding.Converter>
                    </Binding>
                </cards:SfCardView.IndicatorColor>
                <cards:SfCardView.IndicatorThickness>
                    <x:Double>6</x:Double>
                </cards:SfCardView.IndicatorThickness>
                <cards:SfCardView.IndicatorPosition>
                    <IndicatorPosition>Left</IndicatorPosition>
                </cards:SfCardView.IndicatorPosition>
                
                <Grid Padding="20" RowSpacing="10">
                    <Grid.RowDefinitions>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                    </Grid.RowDefinitions>
                    
                    <Label Text="{Binding Title}"
                           FontSize="20"
                           FontAttributes="Bold"
                           Grid.Row="0"/>
                    
                    <Label Text="{Binding Description}"
                           FontSize="14"
                           TextColor="Gray"
                           Grid.Row="1"/>
                    
                    <Label Grid.Row="2">
                        <Label.FormattedText>
                            <FormattedString>
                                <Span Text="Due: " FontAttributes="Bold"/>
                                <Span Text="{Binding DueDate, StringFormat='{0:MMM dd, yyyy}'}"/>
                            </FormattedString>
                        </Label.FormattedText>
                    </Label>
                    
                    <Label Text="{Binding Priority, StringFormat='Priority: {0}'}"
                           FontSize="12"
                           Grid.Row="3"/>
                </Grid>
            </cards:SfCardView>
        </DataTemplate>
    </BindableLayout.ItemTemplate>
</cards:SfCardLayout>
```

**Converter:**

```csharp
public class PriorityToColorConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        if (value is Priority priority)
        {
            return priority switch
            {
                Priority.High => Colors.Red,
                Priority.Medium => Colors.Orange,
                Priority.Low => Colors.Green,
                _ => Colors.Gray
            };
        }
        return Colors.Gray;
    }
    
    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        throw new NotImplementedException();
    }
}
```

### Example 2: Product Catalog

**Model:**

```csharp
public class Product
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Description { get; set; }
    public decimal Price { get; set; }
    public string ImageUrl { get; set; }
    public double Rating { get; set; }
    public bool InStock { get; set; }
}
```

**ViewModel:**

```csharp
public class ProductViewModel : INotifyPropertyChanged
{
    public ObservableCollection<Product> Products { get; set; }
    
    public ProductViewModel()
    {
        LoadProducts();
    }
    
    private async void LoadProducts()
    {
        // Simulate API call
        Products = new ObservableCollection<Product>
        {
            new Product
            {
                Id = 1,
                Name = "Wireless Headphones",
                Description = "Premium noise-cancelling headphones",
                Price = 299.99m,
                ImageUrl = "headphones.jpg",
                Rating = 4.5,
                InStock = true
            },
            new Product
            {
                Id = 2,
                Name = "Smart Watch",
                Description = "Fitness tracking and notifications",
                Price = 399.99m,
                ImageUrl = "smartwatch.jpg",
                Rating = 4.7,
                InStock = true
            }
        };
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected virtual void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

**XAML with Image Cards:**

```xml
<cards:SfCardLayout BindableLayout.ItemsSource="{Binding Products}"
                    SwipeDirection="Left"
                    HeightRequest="600"
                    WidthRequest="350"
                    BackgroundColor="#F0F0F0">
    <BindableLayout.ItemTemplate>
        <DataTemplate>
            <cards:SfCardView BackgroundColor="White"
                            CornerRadius="15"
                            Margin="10">
                <Grid>
                    <Grid.RowDefinitions>
                        <RowDefinition Height="250"/>
                        <RowDefinition Height="*"/>
                    </Grid.RowDefinitions>
                    
                    <!-- Product Image -->
                    <Image Source="{Binding ImageUrl}"
                           Aspect="AspectFill"
                           Grid.Row="0"/>
                    
                    <!-- Product Info -->
                    <VerticalStackLayout Padding="15" Spacing="8" Grid.Row="1">
                        <Label Text="{Binding Name}"
                               FontSize="20"
                               FontAttributes="Bold"/>
                        
                        <Label Text="{Binding Description}"
                               FontSize="14"
                               TextColor="Gray"
                               MaxLines="2"/>
                        
                        <HorizontalStackLayout Spacing="10">
                            <Label Text="⭐"
                                   FontSize="16"/>
                            <Label Text="{Binding Rating, StringFormat='{0:F1}'}"
                                   FontSize="16"
                                   VerticalOptions="Center"/>
                        </HorizontalStackLayout>
                        
                        <Label Text="{Binding Price, StringFormat='${0:F2}'}"
                               FontSize="24"
                               FontAttributes="Bold"
                               TextColor="Green"/>
                        
                        <Label Text="In Stock"
                               FontSize="12"
                               TextColor="Green"
                               IsVisible="{Binding InStock}"/>
                    </VerticalStackLayout>
                </Grid>
            </cards:SfCardView>
        </DataTemplate>
    </BindableLayout.ItemTemplate>
</cards:SfCardLayout>
```

## Advanced Patterns

### Dynamic Updates

ObservableCollection automatically updates the UI when items change:

```csharp
// Add new card
viewModel.Cards.Add(new CardItem 
{ 
    Title = "New Task",
    Description = "Just added"
});

// Remove card
viewModel.Cards.RemoveAt(0);

// Clear all
viewModel.Cards.Clear();

// Update existing item
viewModel.Cards[0].Title = "Updated Title";
viewModel.Cards[0].OnPropertyChanged(nameof(CardItem.Title));
```

## Best Practices

### 1. Use ObservableCollection

Always use `ObservableCollection<T>` for automatic UI updates:

```csharp
public ObservableCollection<CardItem> Cards { get; set; }  // ✓ Good
public List<CardItem> Cards { get; set; }  // ✗ Won't update UI automatically
```

### 2. Implement INotifyPropertyChanged

For item property changes to reflect in UI:

```csharp
public class CardItem : INotifyPropertyChanged
{
    private string _title;
    public string Title
    {
        get => _title;
        set
        {
            _title = value;
            OnPropertyChanged();
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged([CallerMemberName] string name = null) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
}
```

### 3. Optimize Performance

- Limit initial card count (lazy loading)
- Use simple DataTemplates
- Avoid heavy computations in templates
- Consider virtualization for large datasets

### 4. Handle Empty States

```xml
<Grid>
    <cards:SfCardLayout BindableLayout.ItemsSource="{Binding Cards}"
                        IsVisible="{Binding HasCards}"/>
    
    <Label Text="No cards available"
           IsVisible="{Binding HasNoCards}"
           HorizontalOptions="Center"
           VerticalOptions="Center"/>
</Grid>
```
