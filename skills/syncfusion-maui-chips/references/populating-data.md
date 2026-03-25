# Populating Data in .NET MAUI Chips

## Table of Contents
- [Overview](#overview)
- [Populating with Business Objects](#populating-with-business-objects)
- [Populating with SfChip Items](#populating-with-sfchip-items)
- [InputView for Dynamic Creation](#inputview-for-dynamic-creation)
- [Data Binding Best Practices](#data-binding-best-practices)

## Overview

The .NET MAUI Chips control supports two primary methods for populating data:

1. **Business Objects:** Bind a collection of objects using `ItemsSource` and `DisplayMemberPath`
2. **SfChip Items:** Directly add `SfChip` instances to the `Items` collection

## Populating with Business Objects

Use `ItemsSource` to bind a collection of business objects to the chip group. This is the recommended approach for data-driven applications.

### Basic Setup

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
using System.Runtime.CompilerServices;

public class ViewModel : INotifyPropertyChanged
{
    private ObservableCollection<Person> employees;
    
    public ObservableCollection<Person> Employees
    {
        get { return employees; }
        set 
        { 
            employees = value; 
            OnPropertyChanged(); 
        }
    }
    
    public ViewModel()
    {
        Employees = new ObservableCollection<Person>
        {
            new Person { Name = "John" },
            new Person { Name = "James" },
            new Person { Name = "Linda" },
            new Person { Name = "Rose" },
            new Person { Name = "Mark" }
        };
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

**XAML:**
```xaml
<ContentPage.BindingContext>
    <local:ViewModel />
</ContentPage.BindingContext>

<chip:SfChipGroup ItemsSource="{Binding Employees}"
                  DisplayMemberPath="Name"
                  ChipPadding="8,8,0,0"
                  ChipBackground="White"
                  ChipTextColor="Black">
    <chip:SfChipGroup.ChipLayout>
        <FlexLayout Wrap="Wrap" HorizontalOptions="Start" />
    </chip:SfChipGroup.ChipLayout>
</chip:SfChipGroup>
```

**C#:**
```csharp
this.BindingContext = new ViewModel();

SfChipGroup chipGroup = new SfChipGroup
{
    DisplayMemberPath = "Name",
    ChipPadding = new Thickness(8, 8, 0, 0),
    ChipBackground = Colors.White,
    ChipTextColor = Colors.Black
};
chipGroup.SetBinding(SfChipGroup.ItemsSourceProperty, "Employees");

FlexLayout layout = new FlexLayout
{
    Wrap = FlexWrap.Wrap,
    HorizontalOptions = LayoutOptions.Start
};
chipGroup.ChipLayout = layout;
```

### Key Properties

**ItemsSource:** The collection of business objects to display as chips.

**DisplayMemberPath:** The property name to use as the chip text. Must match a public property in your model.

```xaml
<chip:SfChipGroup ItemsSource="{Binding Products}"
                  DisplayMemberPath="ProductName" />
```

### Binding Icons with ImageMemberPath

To display icons from your business objects, use `ImageMemberPath` along with `ShowIcon`.

**Model with Image:**
```csharp
public class Person
{
    public string Name { get; set; }
    public string Image { get; set; }
}
```

**ViewModel:**
```csharp
public class ViewModel : INotifyPropertyChanged
{
    private ObservableCollection<Person> employees;
    
    public ObservableCollection<Person> Employees
    {
        get { return employees; }
        set 
        { 
            employees = value; 
            OnPropertyChanged(); 
        }
    }
    
    public ViewModel()
    {
        Employees = new ObservableCollection<Person>
        {
            new Person { Name = "John", Image = "john.png" },
            new Person { Name = "James", Image = "james.png" },
            new Person { Name = "Linda", Image = "linda.png" },
            new Person { Name = "Rose", Image = "rose.png" },
            new Person { Name = "Mark", Image = "mark.png" }
        };
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

**XAML:**
```xaml
<chip:SfChipGroup ItemsSource="{Binding Employees}"
                  DisplayMemberPath="Name"
                  ImageMemberPath="Image"
                  ShowIcon="True"
                  ChipImageSize="30"
                  ChipPadding="8,8,0,0">
    <chip:SfChipGroup.ChipLayout>
        <FlexLayout Wrap="Wrap" HorizontalOptions="Start" />
    </chip:SfChipGroup.ChipLayout>
</chip:SfChipGroup>
```

**C#:**
```csharp
SfChipGroup chipGroup = new SfChipGroup
{
    DisplayMemberPath = "Name",
    ImageMemberPath = "Image",
    ShowIcon = true,
    ChipImageSize = 30,
    ChipPadding = new Thickness(8, 8, 0, 0)
};
chipGroup.SetBinding(SfChipGroup.ItemsSourceProperty, "Employees");
```

### Complex Model Example

**Model:**
```csharp
public class Product
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Category { get; set; }
    public decimal Price { get; set; }
    public string ImageUrl { get; set; }
    public bool IsAvailable { get; set; }
}
```

**ViewModel:**
```csharp
public class ProductViewModel : INotifyPropertyChanged
{
    private ObservableCollection<Product> products;
    private ObservableCollection<Product> selectedProducts;
    
    public ObservableCollection<Product> Products
    {
        get { return products; }
        set 
        { 
            products = value; 
            OnPropertyChanged(); 
        }
    }
    
    public ObservableCollection<Product> SelectedProducts
    {
        get { return selectedProducts; }
        set 
        { 
            selectedProducts = value; 
            OnPropertyChanged(); 
        }
    }
    
    public ProductViewModel()
    {
        Products = new ObservableCollection<Product>
        {
            new Product 
            { 
                Id = 1, 
                Name = "Laptop", 
                Category = "Electronics",
                Price = 999.99m,
                ImageUrl = "laptop.png",
                IsAvailable = true
            },
            new Product 
            { 
                Id = 2, 
                Name = "Phone", 
                Category = "Electronics",
                Price = 599.99m,
                ImageUrl = "phone.png",
                IsAvailable = true
            },
            new Product 
            { 
                Id = 3, 
                Name = "Headphones", 
                Category = "Audio",
                Price = 149.99m,
                ImageUrl = "headphones.png",
                IsAvailable = false
            }
        };
        
        SelectedProducts = new ObservableCollection<Product>();
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

**XAML:**
```xaml
<chip:SfChipGroup ItemsSource="{Binding Products}"
                  DisplayMemberPath="Name"
                  ImageMemberPath="ImageUrl"
                  ShowIcon="True"
                  ChipType="Filter"
                  SelectionChanged="ChipGroup_SelectionChanged">
    <!-- Visual states for selection -->
</chip:SfChipGroup>
```

### Dynamic Collection Updates

Use `ObservableCollection<T>` to automatically update chips when the collection changes.

**Adding Items:**
```csharp
private void AddEmployee_Clicked(object sender, EventArgs e)
{
    var viewModel = this.BindingContext as ViewModel;
    viewModel.Employees.Add(new Person { Name = "New Employee" });
}
```

**Removing Items:**
```csharp
private void RemoveEmployee_Clicked(object sender, EventArgs e)
{
    var viewModel = this.BindingContext as ViewModel;
    if (viewModel.Employees.Count > 0)
    {
        viewModel.Employees.RemoveAt(viewModel.Employees.Count - 1);
    }
}
```

**Clearing Items:**
```csharp
private void ClearEmployees_Clicked(object sender, EventArgs e)
{
    var viewModel = this.BindingContext as ViewModel;
    viewModel.Employees.Clear();
}
```

## Populating with SfChip Items

Directly add `SfChip` instances to the `Items` collection for more granular control over individual chips.

### Basic Items Collection

**XAML:**
```xaml
<chip:SfChipGroup ChipType="Choice">
    <chip:SfChipGroup.Items>
        <chip:SfChip Text="Extra Small" Background="LightBlue" />
        <chip:SfChip Text="Small" Background="LightGreen" />
        <chip:SfChip Text="Medium" Background="LightYellow" />
        <chip:SfChip Text="Large" Background="LightCoral" />
        <chip:SfChip Text="Extra Large" Background="LightPink" />
    </chip:SfChipGroup.Items>
</chip:SfChipGroup>
```

**C#:**
```csharp
SfChipGroup chipGroup = new SfChipGroup
{
    ChipType = SfChipsType.Choice
};

chipGroup.Items.Add(new SfChip { Text = "Extra Small", Background = Colors.LightBlue });
chipGroup.Items.Add(new SfChip { Text = "Small", Background = Colors.LightGreen });
chipGroup.Items.Add(new SfChip { Text = "Medium", Background = Colors.LightYellow });
chipGroup.Items.Add(new SfChip { Text = "Large", Background = Colors.LightCoral });
chipGroup.Items.Add(new SfChip { Text = "Extra Large", Background = Colors.LightPink });
```

### Individually Styled Chips

```xaml
<chip:SfChipGroup>
    <chip:SfChipGroup.Items>
        <chip:SfChip Text="Category 1"
                     Background="#E3F2FD"
                     TextColor="#1565C0"
                     ShowIcon="True"
                     ImageSource="icon1.png" />
        
        <chip:SfChip Text="Category 2"
                     Background="#F3E5F5"
                     TextColor="#6A1B9A"
                     ShowIcon="True"
                     ImageSource="icon2.png" />
        
        <chip:SfChip Text="Category 3"
                     Background="#E8F5E9"
                     TextColor="#2E7D32"
                     ShowIcon="True"
                     ImageSource="icon3.png" />
    </chip:SfChipGroup.Items>
</chip:SfChipGroup>
```

### Dynamic Items Addition

```csharp
private void AddChip_Clicked(object sender, EventArgs e)
{
    var chip = new SfChip
    {
        Text = $"Chip {chipGroup.Items.Count + 1}",
        Background = Colors.LightBlue,
        TextColor = Colors.Black,
        ShowCloseButton = true
    };
    
    chip.CloseButtonClicked += (s, args) =>
    {
        chipGroup.Items.Remove(chip);
    };
    
    chipGroup.Items.Add(chip);
}
```

## InputView for Dynamic Creation

Use `InputView` to allow users to create chips at runtime, typically with the Input chip type.

### Entry-Based Input

**XAML:**
```xaml
<chip:SfChipGroup ItemsSource="{Binding Tags}"
                  DisplayMemberPath="Name"
                  ChipType="Input"
                  ChipPadding="8,8,0,0">
    <chip:SfChipGroup.InputView>
        <Entry x:Name="entry"
               Placeholder="Enter tag name"
               VerticalOptions="Center"
               HeightRequest="40"
               WidthRequest="120"
               Completed="Entry_Completed" />
    </chip:SfChipGroup.InputView>
    
    <chip:SfChipGroup.ChipLayout>
        <FlexLayout Wrap="Wrap" HorizontalOptions="Start" />
    </chip:SfChipGroup.ChipLayout>
</chip:SfChipGroup>
```

**Code-behind:**
```csharp
private void Entry_Completed(object sender, EventArgs e)
{
    var viewModel = this.BindingContext as ViewModel;
    var tagName = (sender as Entry).Text;
    
    if (!string.IsNullOrWhiteSpace(tagName))
    {
        viewModel.Tags.Add(new Tag { Name = tagName });
        entry.Text = string.Empty;
        entry.Focus();
    }
}
```

### Button-Based Input

```xaml
<chip:SfChipGroup ItemsSource="{Binding Categories}"
                  DisplayMemberPath="Name"
                  ChipType="Input">
    <chip:SfChipGroup.InputView>
        <HorizontalStackLayout Spacing="5">
            <Entry x:Name="categoryEntry"
                   Placeholder="New category"
                   WidthRequest="100"
                   HeightRequest="40" />
            <Button Text="+"
                    HeightRequest="40"
                    WidthRequest="40"
                    Clicked="AddCategory_Clicked" />
        </HorizontalStackLayout>
    </chip:SfChipGroup.InputView>
</chip:SfChipGroup>
```

```csharp
private void AddCategory_Clicked(object sender, EventArgs e)
{
    var viewModel = this.BindingContext as ViewModel;
    var categoryName = categoryEntry.Text;
    
    if (!string.IsNullOrWhiteSpace(categoryName))
    {
        viewModel.Categories.Add(new Category { Name = categoryName });
        categoryEntry.Text = string.Empty;
    }
}
```

### Custom InputView

```xaml
<chip:SfChipGroup ItemsSource="{Binding Contacts}"
                  DisplayMemberPath="Name"
                  ChipType="Input">
    <chip:SfChipGroup.InputView>
        <Border Stroke="Gray"
                StrokeThickness="1"
                Padding="5"
                HeightRequest="40">
            <HorizontalStackLayout>
                <Image Source="add_contact.png" 
                       HeightRequest="24" 
                       WidthRequest="24" />
                <Entry x:Name="contactEntry"
                       Placeholder="Add contact"
                       WidthRequest="100"
                       VerticalOptions="Center"
                       Completed="ContactEntry_Completed" />
            </HorizontalStackLayout>
        </Border>
    </chip:SfChipGroup.InputView>
</chip:SfChipGroup>
```

## Data Binding Best Practices

### Use ObservableCollection

Always use `ObservableCollection<T>` for automatic UI updates when data changes.

```csharp
// ✅ Correct
public ObservableCollection<Person> Employees { get; set; }

// ❌ Incorrect - won't update UI automatically
public List<Person> Employees { get; set; }
```

### Implement INotifyPropertyChanged

Ensure your ViewModel implements `INotifyPropertyChanged` for property change notifications.

```csharp
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
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

### AOT Publishing Considerations

When publishing in AOT (Ahead-of-Time) mode on iOS, add the `[Preserve]` attribute to your model class to maintain `DisplayMemberPath` binding.

```csharp
using Microsoft.Maui.Controls;

[Preserve(AllMembers = true)]
public class Person
{
    public string Name { get; set; }
    public string Image { get; set; }
}
```

**Why:** AOT compilation may strip unused properties. The `[Preserve]` attribute ensures all members remain available for runtime binding.

### Property Naming

Ensure property names in `DisplayMemberPath` and `ImageMemberPath` match exactly (case-sensitive).

```csharp
// Model
public class Product
{
    public string ProductName { get; set; }  // Note the capitalization
}

// XAML - must match exactly
<chip:SfChipGroup ItemsSource="{Binding Products}"
                  DisplayMemberPath="ProductName" />
```

### Null Safety

Check for null or empty collections before accessing items.

```csharp
private void ProcessSelection()
{
    var viewModel = this.BindingContext as ViewModel;
    
    if (viewModel?.Employees != null && viewModel.Employees.Count > 0)
    {
        // Safe to process
        var firstEmployee = viewModel.Employees[0];
    }
}
```

### Performance Tips

**Virtualization:** For large collections, consider loading data in batches.

```csharp
public async Task LoadEmployeesAsync()
{
    var employees = await GetEmployeesFromDatabaseAsync();
    
    // Add in batches
    foreach (var employee in employees.Take(20))
    {
        Employees.Add(employee);
    }
}
```

**Lazy Loading:** Load images on demand rather than all at once.

```csharp
public class Person
{
    private string imagePath;
    
    public string Name { get; set; }
    
    public ImageSource Image
    {
        get
        {
            if (string.IsNullOrEmpty(imagePath))
                return null;
            
            return ImageSource.FromFile(imagePath);
        }
    }
    
    public void SetImagePath(string path)
    {
        imagePath = path;
    }
}
```

### Complete Example

Here's a complete working example with all best practices:

```csharp
// Model
[Preserve(AllMembers = true)]
public class Tag
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Color { get; set; }
}

// ViewModel
public class TagViewModel : INotifyPropertyChanged
{
    private ObservableCollection<Tag> tags;
    
    public ObservableCollection<Tag> Tags
    {
        get { return tags; }
        set 
        { 
            tags = value; 
            OnPropertyChanged(); 
        }
    }
    
    public TagViewModel()
    {
        Tags = new ObservableCollection<Tag>
        {
            new Tag { Id = 1, Name = "Important", Color = "Red" },
            new Tag { Id = 2, Name = "Work", Color = "Blue" },
            new Tag { Id = 3, Name = "Personal", Color = "Green" }
        };
    }
    
    public void AddTag(string name, string color)
    {
        var newId = Tags.Count > 0 ? Tags.Max(t => t.Id) + 1 : 1;
        Tags.Add(new Tag { Id = newId, Name = name, Color = color });
    }
    
    public void RemoveTag(Tag tag)
    {
        if (tag != null && Tags.Contains(tag))
        {
            Tags.Remove(tag);
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

```xaml
<ContentPage.BindingContext>
    <local:TagViewModel />
</ContentPage.BindingContext>

<chip:SfChipGroup ItemsSource="{Binding Tags}"
                  DisplayMemberPath="Name"
                  ChipType="Input"
                  ChipPadding="8,8,0,0"
                  ItemRemoved="ChipGroup_ItemRemoved">
    <chip:SfChipGroup.InputView>
        <Entry x:Name="tagEntry"
               Placeholder="Add tag"
               Completed="TagEntry_Completed" />
    </chip:SfChipGroup.InputView>
</chip:SfChipGroup>
```

```csharp
private void TagEntry_Completed(object sender, EventArgs e)
{
    var viewModel = this.BindingContext as TagViewModel;
    var tagName = tagEntry.Text;
    
    if (!string.IsNullOrWhiteSpace(tagName))
    {
        viewModel.AddTag(tagName, "Gray");
        tagEntry.Text = string.Empty;
    }
}

private void ChipGroup_ItemRemoved(object sender, ItemRemovedEventArgs e)
{
    var viewModel = this.BindingContext as TagViewModel;
    var removedTag = e.RemovedItem as Tag;
    viewModel.RemoveTag(removedTag);
}
```
