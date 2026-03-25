# MVVM Patterns in .NET MAUI ListView

## Table of Contents
- [Overview](#overview)
- [MVVM Architecture with ListView](#mvvm-architecture-with-listview)
- [Commands](#commands)
- [Data Binding](#data-binding)
- [Handling User Actions in MVVM](#handling-user-actions-in-mvvm)
- [Complete MVVM Example](#complete-mvvm-example)

## Overview

MVVM (Model-View-ViewModel) is the recommended pattern for .NET MAUI applications. It separates UI (View) from business logic (ViewModel) and data (Model).

**Benefits:**
- Clean separation of concerns
- Testable business logic
- Reusable ViewModels
- Data binding for automatic UI updates

## MVVM Architecture with ListView

### Model

Data entity with property change notification:

```csharp
using System.ComponentModel;
using System.Runtime.CompilerServices;

public class Product : INotifyPropertyChanged
{
    private string name;
    private decimal price;
    private int quantity;
    private bool isAvailable;
    
    public int Id { get; set; }
    
    public string Name
    {
        get => name;
        set
        {
            if (name != value)
            {
                name = value;
                OnPropertyChanged();
            }
        }
    }
    
    public decimal Price
    {
        get => price;
        set
        {
            if (price != value)
            {
                price = value;
                OnPropertyChanged();
                OnPropertyChanged(nameof(FormattedPrice));
            }
        }
    }
    
    public int Quantity
    {
        get => quantity;
        set
        {
            if (quantity != value)
            {
                quantity = value;
                OnPropertyChanged();
                OnPropertyChanged(nameof(IsInStock));
            }
        }
    }
    
    public bool IsAvailable
    {
        get => isAvailable;
        set
        {
            if (isAvailable != value)
            {
                isAvailable = value;
                OnPropertyChanged();
            }
        }
    }
    
    // Computed properties
    public string FormattedPrice => $"${Price:F2}";
    public bool IsInStock => Quantity > 0;
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

### ViewModel

Business logic and data management:

```csharp
using System.Collections.ObjectModel;
using System.ComponentModel;
using System.Runtime.CompilerServices;
using System.Windows.Input;

public class ProductViewModel : INotifyPropertyChanged
{
    private ObservableCollection<Product> products;
    private Product selectedProduct;
    private bool isRefreshing;
    private string searchText;
    
    public ObservableCollection<Product> Products
    {
        get => products;
        set
        {
            products = value;
            OnPropertyChanged();
        }
    }
    
    public Product SelectedProduct
    {
        get => selectedProduct;
        set
        {
            if (selectedProduct != value)
            {
                selectedProduct = value;
                OnPropertyChanged();
                OnProductSelected();
            }
        }
    }
    
    public bool IsRefreshing
    {
        get => isRefreshing;
        set
        {
            isRefreshing = value;
            OnPropertyChanged();
        }
    }
    
    public string SearchText
    {
        get => searchText;
        set
        {
            if (searchText != value)
            {
                searchText = value;
                OnPropertyChanged();
                FilterProducts();
            }
        }
    }
    
    // Commands
    public ICommand DeleteCommand { get; }
    public ICommand AddToCartCommand { get; }
    
    public ProductViewModel()
    {
        Products = new ObservableCollection<Product>();
    
        DeleteCommand = new Command<Product>(DeleteProduct);
        AddToCartCommand = new Command<Product>(AddToCart);
        
        LoadProducts();
    }
    
    private void LoadProducts()
    {
        // Load initial data
        Products.Add(new Product { Id = 1, Name = "Laptop", Price = 999.99m, Quantity = 5 });
        Products.Add(new Product { Id = 2, Name = "Mouse", Price = 29.99m, Quantity = 20 });
        Products.Add(new Product { Id = 3, Name = "Keyboard", Price = 79.99m, Quantity = 15 });
    }
    
    private async Task RefreshData()
    {
        IsRefreshing = true;
        
        try
        {
            // Simulate API call
            await Task.Delay(1000);
            
            // Reload data
            Products.Clear();
            LoadProducts();
        }
        finally
        {
            IsRefreshing = false;
        }
    }
    
    private void DeleteProduct(Product product)
    {
        if (product != null)
        {
            Products.Remove(product);
        }
    }
    
    private void AddToCart(Product product)
    {
        if (product != null && product.IsInStock)
        {
            // Add to cart logic
            System.Diagnostics.Debug.WriteLine($"Added {product.Name} to cart");
        }
    }
    
    private void OnProductSelected()
    {
        if (SelectedProduct != null)
        {
            // Navigate to detail page or show details
            System.Diagnostics.Debug.WriteLine($"Selected: {SelectedProduct.Name}");
        }
    }
    
    private void FilterProducts()
    {
        // Implement filtering logic
        // Use DataSource.Filter for ListView
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

### View

XAML with data binding:

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:syncfusion="clr-namespace:Syncfusion.Maui.ListView;assembly=Syncfusion.Maui.ListView"
             xmlns:local="clr-namespace:YourApp.ViewModels"
             x:Class="YourApp.Views.ProductListPage">
    
    <ContentPage.BindingContext>
        <local:ProductViewModel />
    </ContentPage.BindingContext>
    
    <Grid RowDefinitions="Auto, *">
        <!-- Search Bar -->
        <SearchBar Grid.Row="0"
                   Placeholder="Search products..."
                   Text="{Binding SearchText}" />
        
        <!-- Product List -->
        <syncfusion:SfListView Grid.Row="1"
                               ItemsSource="{Binding Products}"
                               SelectedItem="{Binding SelectedProduct, Mode=TwoWay}"
                               SelectionMode="Single"
                               IsBusy="{Binding IsRefreshing}"
                               ItemSize="100">
            
            <syncfusion:SfListView.ItemTemplate>
                <DataTemplate>
                    <SwipeView>
                        <SwipeView.RightItems>
                            <SwipeItems>
                                <SwipeItem Text="Delete"
                                           BackgroundColor="Red"
                                           Command="{Binding Source={RelativeSource AncestorType={x:Type local:ProductViewModel}}, Path=DeleteCommand}"
                                           CommandParameter="{Binding .}" />
                            </SwipeItems>
                        </SwipeView.RightItems>
                        
                        <Grid Padding="15,10" ColumnSpacing="15">
                            <Grid.ColumnDefinitions>
                                <ColumnDefinition Width="80" />
                                <ColumnDefinition Width="*" />
                                <ColumnDefinition Width="Auto" />
                            </Grid.ColumnDefinitions>
                            
                            <Frame Grid.Column="0"
                                   Padding="0"
                                   CornerRadius="8"
                                   IsClippedToBounds="True">
                                <Image Source="{Binding ImageUrl}"
                                       Aspect="AspectFill"
                                       HeightRequest="80"
                                       WidthRequest="80" />
                            </Frame>
                            
                            <StackLayout Grid.Column="1" VerticalOptions="Center">
                                <Label Text="{Binding Name}"
                                       FontAttributes="Bold"
                                       FontSize="16" />
                                <Label Text="{Binding FormattedPrice}"
                                       FontSize="14"
                                       TextColor="Green" />
                                <Label Text="{Binding Quantity, StringFormat='Stock: {0}'}"
                                       FontSize="12"
                                       TextColor="Gray" />
                            </StackLayout>
                            
                            <Button Grid.Column="2"
                                    Text="Add"
                                    Command="{Binding Source={RelativeSource AncestorType={x:Type local:ProductViewModel}}, Path=AddToCartCommand}"
                                    CommandParameter="{Binding .}"
                                    VerticalOptions="Center"
                                    IsEnabled="{Binding IsInStock}" />
                        </Grid>
                    </SwipeView>
                </DataTemplate>
            </syncfusion:SfListView.ItemTemplate>
        </syncfusion:SfListView>
    </Grid>
</ContentPage>
```

## Commands

### TapCommand

Execute command when item is tapped:

```xml
<syncfusion:SfListView TapCommand="{Binding ItemTappedCommand}"
                       TapCommandParameter="{Binding .}" />
```

```csharp
public ICommand ItemTappedCommand { get; }

public ProductViewModel()
{
    ItemTappedCommand = new Command<object>((item) =>
    {
        var product = item as Product;
        if (product != null)
        {
            // Handle tap
            NavigateToDetails(product);
        }
    });
}
```

### LongPressCommand

Execute command on long press:

```xml
<syncfusion:SfListView LongPressCommand="{Binding ItemLongPressCommand}"
                       LongPressCommandParameter="{Binding .}" />
```

```csharp
public ICommand ItemLongPressCommand { get; }

public ProductViewModel()
{
    ItemLongPressCommand = new Command<object>((item) =>
    {
        var product = item as Product;
        if (product != null)
        {
            // Show context menu or options
            ShowContextMenu(product);
        }
    });
}
```

### ItemTapped Event vs Command

**Use Command when:**
- Following MVVM pattern strictly
- Need testable unit tests
- Reusing logic across multiple views

**Use Event when:**
- Need access to View-specific objects
- Performing navigation with parameters
- Simpler one-time implementations

```csharp
// Code-behind (Event approach)
private void OnItemTapped(object sender, ItemTappedEventArgs e)
{
    var product = e.DataItem as Product;
    Navigation.PushAsync(new ProductDetailPage(product));
}

// ViewModel (Command approach)
public ICommand NavigateToDetailCommand { get; }

public ProductViewModel(INavigationService navigationService)
{
    NavigateToDetailCommand = new Command<Product>((product) =>
    {
        navigationService.NavigateToAsync<ProductDetailViewModel>(product);
    });
}
```

## Data Binding

### ObservableCollection

Always use `ObservableCollection<T>` for automatic UI updates:

```csharp
// ✓ CORRECT
public ObservableCollection<Product> Products { get; set; }

// ❌ WRONG
public List<Product> Products { get; set; }
```

### Two-Way Binding

Enable UI changes to update ViewModel:

```xml
<syncfusion:SfListView SelectedItem="{Binding SelectedProduct, Mode=TwoWay}"
                       SelectedItems="{Binding SelectedProducts, Mode=TwoWay}" />
```

### Binding to Nested Properties

```xml
<Label Text="{Binding SelectedProduct.Name}" />
<Label Text="{Binding SelectedProduct.FormattedPrice}" />
```

### Binding to ViewModel Commands from ItemTemplate

```xml
<DataTemplate>
    <Grid>
        <Button Text="Delete"
                Command="{Binding Source={RelativeSource AncestorType={x:Type local:ProductViewModel}}, Path=DeleteCommand}"
                CommandParameter="{Binding .}" />
    </Grid>
</DataTemplate>
```

## Handling User Actions in MVVM

### Pattern 1: Command in ItemTemplate

```xml
<syncfusion:SfListView.ItemTemplate>
    <DataTemplate>
        <Grid>
            <Button Text="Favorite"
                    Command="{Binding Source={x:Reference listView}, Path=BindingContext.ToggleFavoriteCommand}"
                    CommandParameter="{Binding .}" />
        </Grid>
    </DataTemplate>
</syncfusion:SfListView.ItemTemplate>
```

```csharp
public ICommand ToggleFavoriteCommand { get; }

public ProductViewModel()
{
    ToggleFavoriteCommand = new Command<Product>((product) =>
    {
        if (product != null)
        {
            product.IsFavorite = !product.IsFavorite;
        }
    });
}
```

### Pattern 2: Selection Changed

```csharp
// In code-behind
listView.SelectionChanged += (sender, e) =>
{
    var viewModel = BindingContext as ProductViewModel;
    viewModel?.OnSelectionChanged(e.AddedItems, e.RemovedItems);
};

// In ViewModel
public void OnSelectionChanged(IList addedItems, IList removedItems)
{
    // Handle selection change
    UpdateSelectionCount();
    EnableDisableActions();
}
```

### Pattern 3: ItemTapped with Navigation

```csharp
// In code-behind
private void OnItemTapped(object sender, ItemTappedEventArgs e)
{
    if (e.DataItem is Product product)
    {
        var viewModel = BindingContext as ProductViewModel;
        viewModel?.NavigateToProduct(product);
    }
}

// In ViewModel
public async Task NavigateToProduct(Product product)
{
    await navigationService.NavigateToAsync<ProductDetailViewModel>(product.Id);
}
```

## Complete MVVM Example

Full implementation with master-detail pattern:

```csharp
// Model
public class Product : INotifyPropertyChanged
{
    private string name;
    private decimal price;
    private bool isFavorite;
    
    public int Id { get; set; }
    
    public string Name
    {
        get => name;
        set { name = value; OnPropertyChanged(); }
    }
    
    public decimal Price
    {
        get => price;
        set { price = value; OnPropertyChanged(); OnPropertyChanged(nameof(FormattedPrice)); }
    }
    
    public bool IsFavorite
    {
        get => isFavorite;
        set { isFavorite = value; OnPropertyChanged(); }
    }
    
    public string FormattedPrice => $"${Price:F2}";
    
    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged([CallerMemberName] string name = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
    }
}

// ViewModel
public class ProductListViewModel : INotifyPropertyChanged
{
    private ObservableCollection<Product> products;
    private ObservableCollection<Product> filteredProducts;
    private Product selectedProduct;
    private string searchText;
    private bool hasSelection;
    
    public ObservableCollection<Product> Products
    {
        get => products;
        set { products = value; OnPropertyChanged(); }
    }
    
    public ObservableCollection<Product> FilteredProducts
    {
        get => filteredProducts;
        set { filteredProducts = value; OnPropertyChanged(); }
    }
    
    public Product SelectedProduct
    {
        get => selectedProduct;
        set
        {
            selectedProduct = value;
            OnPropertyChanged();
            HasSelection = value != null;
        }
    }
    
    public string SearchText
    {
        get => searchText;
        set
        {
            searchText = value;
            OnPropertyChanged();
            ApplyFilter();
        }
    }
    
    public bool HasSelection
    {
        get => hasSelection;
        set { hasSelection = value; OnPropertyChanged(); }
    }
    
    public ICommand DeleteCommand { get; }
    public ICommand ToggleFavoriteCommand { get; }
    public ICommand AddToCartCommand { get; }
    
    public ProductListViewModel()
    {
        Products = new ObservableCollection<Product>();
        FilteredProducts = new ObservableCollection<Product>();
        
        DeleteCommand = new Command<Product>(Delete);
        ToggleFavoriteCommand = new Command<Product>(ToggleFavorite);
        AddToCartCommand = new Command<Product>(AddToCart, CanAddToCart);
        
        LoadData();
    }
    
    private void LoadData()
    {
        Products.Add(new Product { Id = 1, Name = "Laptop", Price = 999.99m });
        Products.Add(new Product { Id = 2, Name = "Mouse", Price = 29.99m });
        Products.Add(new Product { Id = 3, Name = "Keyboard", Price = 79.99m });
        
        ApplyFilter();
    }
    
    private void Delete(Product product)
    {
        Products.Remove(product);
        ApplyFilter();
    }
    
    private void ToggleFavorite(Product product)
    {
        product.IsFavorite = !product.IsFavorite;
    }
    
    private void AddToCart(Product product)
    {
        // Add to cart
    }
    
    private bool CanAddToCart(Product product)
    {
        return product != null && product.Price > 0;
    }
    
    private void ApplyFilter()
    {
        FilteredProducts.Clear();
        
        var filtered = string.IsNullOrWhiteSpace(SearchText)
            ? Products
            : Products.Where(p => p.Name.Contains(SearchText, StringComparison.OrdinalIgnoreCase));
        
        foreach (var product in filtered)
        {
            FilteredProducts.Add(product);
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged([CallerMemberName] string name = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
    }
}
```

## Best Practices

1. **Always implement INotifyPropertyChanged** on Models and ViewModels
2. **Use ObservableCollection<T>** for collections bound to ListView
3. **Prefer Commands over Events** for testability
4. **Keep ViewModels testable** - avoid View-specific dependencies
5. **Use async/await** for long-running operations
6. **Implement proper error handling** in commands
7. **Use RelativeSource** or x:Reference for ItemTemplate command binding
8. **Validate CanExecute** for commands that depend on state

## Troubleshooting

**Issue:** ListView not updating when items change
→ Use ObservableCollection<T>, implement INotifyPropertyChanged

**Issue:** Command not executing from ItemTemplate
→ Use RelativeSource binding to ViewModel

**Issue:** SelectedItem not updating ViewModel
→ Use Mode=TwoWay in binding

**Issue:** Memory leaks
→ Unsubscribe from events, dispose ViewModels properly
