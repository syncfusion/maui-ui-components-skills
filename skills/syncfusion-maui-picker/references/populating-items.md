# Populating Items and Data Binding

Learn how to populate the Syncfusion .NET MAUI Picker with data using data binding and ObservableCollection.

## Table of Contents
- [Basic Data Binding](#basic-data-binding)
- [Multi-Column Items](#multi-column-items)
- [Item Text Style Customization](#item-text-style-customization)
- [Custom Item Templates](#custom-item-templates)
- [MVVM Data Binding](#mvvm-data-binding)

## Basic Data Binding

The Picker is a data-bound control that requires binding a collection to the `ItemsSource` property of `PickerColumn`.

### Create a Data Source

**ItemInfo.cs:**
```csharp
using System.Collections.ObjectModel;

public class ItemInfo
{
    private ObservableCollection<object> dataSource;

    public ObservableCollection<object> DataSource
    {
        get { return dataSource; }
        set { dataSource = value; }
    }

    public ItemInfo()
    {
        dataSource = new ObservableCollection<object>()
        {
            "Pink", "Green", "Blue", "Yellow", 
            "Orange", "Purple", "SkyBlue", "PaleGreen"
        };
    }
}
```

### Bind to Picker

**XAML:**
```xml
<ContentPage xmlns:local="clr-namespace:YourNamespace">
    
    <picker:SfPicker x:Name="picker">
        <picker:SfPicker.Columns>
            <picker:PickerColumn ItemsSource="{Binding DataSource}" />
        </picker:SfPicker.Columns>
    </picker:SfPicker>

    <ContentPage.BindingContext>
        <local:ItemInfo />
    </ContentPage.BindingContext>

</ContentPage>
```

**C#:**
```csharp
ItemInfo itemInfo = new ItemInfo();

PickerColumn pickerColumn = new PickerColumn()
{
    ItemsSource = itemInfo.DataSource,
    SelectedIndex = 1,
};

picker.Columns.Add(pickerColumn);
```

### Simple String Collection

**C#:**
```csharp
ObservableCollection<object> cityNames = new ObservableCollection<object>();
cityNames.Add("Chennai");
cityNames.Add("Mumbai");
cityNames.Add("Delhi");
cityNames.Add("Kolkata");
cityNames.Add("Bangalore");
cityNames.Add("Hyderabad");
cityNames.Add("Pune");
cityNames.Add("Ahmedabad");
cityNames.Add("Jaipur");
cityNames.Add("Lucknow");

PickerColumn pickerColumn = new PickerColumn()
{
    HeaderText = "Select City",
    ItemsSource = cityNames,
    SelectedIndex = 1,
};

picker.Columns.Add(pickerColumn);
```

## Multi-Column Items

Populate multiple columns with independent data sources.

**Example: Country and City Picker**

```csharp
// Country column
ObservableCollection<object> countryNames = new ObservableCollection<object>
{
    "Canada", "United States", "India", "United Kingdom", 
    "Australia", "Germany", "France", "Japan", "China", "Brazil"
};

PickerColumn countryColumn = new PickerColumn()
{
    HeaderText = "Select Country",
    ItemsSource = countryNames,
    SelectedIndex = 1,
};

// City column
ObservableCollection<object> cityNames = new ObservableCollection<object>
{
    "Chennai", "Mumbai", "Delhi", "Kolkata", "Bangalore", 
    "Hyderabad", "Pune", "Ahmedabad", "Jaipur", "Lucknow"
};

PickerColumn cityColumn = new PickerColumn()
{
    HeaderText = "Select City",
    ItemsSource = cityNames,
    SelectedIndex = 1,
};

picker.Columns.Add(countryColumn);
picker.Columns.Add(cityColumn);
```

**XAML:**
```xml
<picker:SfPicker x:Name="picker">
    <picker:SfPicker.Columns>
        <picker:PickerColumn HeaderText="Select Country"
                            ItemsSource="{Binding Countries}"
                            SelectedIndex="1" />
        <picker:PickerColumn HeaderText="Select City"
                            ItemsSource="{Binding Cities}"
                            SelectedIndex="1" />
    </picker:SfPicker.Columns>
</picker:SfPicker>
```

## Item Text Style Customization

Customize the appearance of selected and unselected items.

### Selected Item Customization

Style the currently selected item using `SelectedTextStyle`.

**XAML:**
```xml
<picker:SfPicker x:Name="picker">
    <picker:SfPicker.SelectedTextStyle>
        <picker:PickerTextStyle FontSize="16" 
                               FontAttributes="Bold" 
                               TextColor="White"/>
    </picker:SfPicker.SelectedTextStyle>
</picker:SfPicker>
```

**C#:**
```csharp
picker.SelectedTextStyle.FontSize = 16;
picker.SelectedTextStyle.FontAttributes = FontAttributes.Bold;
picker.SelectedTextStyle.TextColor = Colors.White;
```

### Unselected Item Customization

Style the non-selected items using `TextStyle`.

**XAML:**
```xml
<picker:SfPicker x:Name="picker">
    <picker:SfPicker.TextStyle>
        <picker:PickerTextStyle FontSize="16" 
                               FontAttributes="Italic" 
                               TextColor="Black"/>
    </picker:SfPicker.TextStyle>
</picker:SfPicker>
```

**C#:**
```csharp
picker.TextStyle.FontSize = 16;
picker.TextStyle.FontAttributes = FontAttributes.Italic;
picker.TextStyle.TextColor = Colors.Black;
```

### Complete Text Style Example

```csharp
SfPicker picker = new SfPicker()
{
    // Selected item style
    SelectedTextStyle = new PickerTextStyle()
    {
        FontSize = 18,
        FontAttributes = FontAttributes.Bold,
        TextColor = Colors.DarkBlue
    },
    
    // Unselected item style
    TextStyle = new PickerTextStyle()
    {
        FontSize = 14,
        FontAttributes = FontAttributes.None,
        TextColor = Colors.Gray
    }
};
```

## Custom Item Templates

Create custom visual layouts for picker items using `ItemTemplate`.

### Basic Custom Template

**XAML:**
```xml
<Grid>
    <Grid.Resources>
        <DataTemplate x:Key="customView">
            <Grid>
                <Label HorizontalTextAlignment="Center" 
                       VerticalTextAlignment="Center" 
                       TextColor="Red" 
                       Text="{Binding Data}"/>    
            </Grid>
        </DataTemplate>
    </Grid.Resources>
    
    <picker:SfPicker x:Name="picker" 
                     ItemTemplate="{StaticResource customView}">
        <picker:SfPicker.Columns>
            <picker:PickerColumn ItemsSource="{Binding DataSource}" />
        </picker:SfPicker.Columns>
    </picker:SfPicker>
</Grid>
```

### Custom Template in C#

```csharp
DataTemplate customView = new DataTemplate(() =>
{
    Grid grid = new Grid
    {
        Padding = new Thickness(0, 1, 0, 1),
    };

    Label label = new Label
    {
        HorizontalOptions = LayoutOptions.Center,
        VerticalOptions = LayoutOptions.Center,
        HorizontalTextAlignment = TextAlignment.Center,
        VerticalTextAlignment = TextAlignment.Center,
        TextColor = Colors.Black,
    };

    label.SetBinding(Label.TextProperty, new Binding("Data"));
    grid.Children.Add(label);
    return grid;
});

picker.ItemTemplate = customView;
```

### Advanced Custom Template with Images

```xml
<Grid.Resources>
    <DataTemplate x:Key="imageItemTemplate">
        <Grid Padding="5">
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="40"/>
                <ColumnDefinition Width="*"/>
            </Grid.ColumnDefinitions>
            
            <Image Grid.Column="0" 
                   Source="{Binding Icon}" 
                   HeightRequest="30" 
                   WidthRequest="30"/>
            
            <Label Grid.Column="1" 
                   Text="{Binding Name}" 
                   VerticalOptions="Center"
                   FontSize="14"/>
        </Grid>
    </DataTemplate>
</Grid.Resources>

<picker:SfPicker ItemTemplate="{StaticResource imageItemTemplate}">
    <!-- Configuration -->
</picker:SfPicker>
```

**Model for Image Template:**
```csharp
public class ItemWithIcon
{
    public string Name { get; set; }
    public string Icon { get; set; }
}

// Data source
ObservableCollection<ItemWithIcon> items = new ObservableCollection<ItemWithIcon>
{
    new ItemWithIcon { Name = "Home", Icon = "home.png" },
    new ItemWithIcon { Name = "Settings", Icon = "settings.png" },
    new ItemWithIcon { Name = "Profile", Icon = "profile.png" }
};
```

## MVVM Data Binding

Implement clean data binding patterns using the MVVM architecture.

### ViewModel

```csharp
using System.Collections.ObjectModel;
using System.ComponentModel;
using System.Runtime.CompilerServices;

public class PickerViewModel : INotifyPropertyChanged
{
    private ObservableCollection<string> colors;
    private string selectedColor;

    public ObservableCollection<string> Colors
    {
        get => colors;
        set
        {
            colors = value;
            OnPropertyChanged();
        }
    }

    public string SelectedColor
    {
        get => selectedColor;
        set
        {
            selectedColor = value;
            OnPropertyChanged();
        }
    }

    public PickerViewModel()
    {
        Colors = new ObservableCollection<string>
        {
            "Red", "Blue", "Green", "Yellow", 
            "Orange", "Purple", "Pink", "Cyan"
        };
        
        SelectedColor = "Red";
    }

    public event PropertyChangedEventHandler PropertyChanged;

    protected virtual void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

### View (XAML)

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:picker="clr-namespace:Syncfusion.Maui.Picker;assembly=Syncfusion.Maui.Picker"
             xmlns:local="clr-namespace:YourNamespace"
             x:Class="YourNamespace.PickerPage">
    
    <ContentPage.BindingContext>
        <local:PickerViewModel />
    </ContentPage.BindingContext>
    
    <picker:SfPicker x:Name="picker"
                     HeightRequest="280"
                     WidthRequest="300">
        
        <picker:SfPicker.HeaderView>
            <picker:PickerHeaderView Text="Select Color" Height="40" />
        </picker:SfPicker.HeaderView>
        
        <picker:SfPicker.Columns>
            <picker:PickerColumn HeaderText="Colors" 
                                ItemsSource="{Binding Colors}"
                                SelectedItem="{Binding SelectedColor, Mode=TwoWay}" />
        </picker:SfPicker.Columns>
        
        <picker:SfPicker.FooterView>
            <picker:PickerFooterView ShowOkButton="True" Height="40" />
        </picker:SfPicker.FooterView>
        
    </picker:SfPicker>
    
</ContentPage>
```

### Complex Object Binding with MVVM

**Model:**
```csharp
public class Country
{
    public string Name { get; set; }
    public string Code { get; set; }
    public string Flag { get; set; }
}
```

**ViewModel:**
```csharp
public class CountryPickerViewModel : INotifyPropertyChanged
{
    private ObservableCollection<Country> countries;
    private Country selectedCountry;

    public ObservableCollection<Country> Countries
    {
        get => countries;
        set
        {
            countries = value;
            OnPropertyChanged();
        }
    }

    public Country SelectedCountry
    {
        get => selectedCountry;
        set
        {
            selectedCountry = value;
            OnPropertyChanged();
        }
    }

    public CountryPickerViewModel()
    {
        Countries = new ObservableCollection<Country>
        {
            new Country { Name = "United States", Code = "US", Flag = "us.png" },
            new Country { Name = "Canada", Code = "CA", Flag = "ca.png" },
            new Country { Name = "United Kingdom", Code = "UK", Flag = "uk.png" }
        };
    }

    public event PropertyChangedEventHandler PropertyChanged;

    protected virtual void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

**View:**
```xml
<picker:SfPicker>
    <picker:SfPicker.Columns>
        <picker:PickerColumn HeaderText="Country" 
                            DisplayMemberPath="Name"
                            ItemsSource="{Binding Countries}"
                            SelectedItem="{Binding SelectedCountry, Mode=TwoWay}" />
    </picker:SfPicker.Columns>
</picker:SfPicker>
```

## Dynamic Data Updates

Handle dynamic data changes using `ObservableCollection`.

```csharp
public class DynamicPickerViewModel : INotifyPropertyChanged
{
    public ObservableCollection<string> Items { get; set; }

    public DynamicPickerViewModel()
    {
        Items = new ObservableCollection<string> { "Item 1", "Item 2", "Item 3" };
    }

    public void AddItem(string item)
    {
        Items.Add(item);
        // Picker automatically updates
    }

    public void RemoveItem(string item)
    {
        Items.Remove(item);
        // Picker automatically updates
    }

    public void ClearItems()
    {
        Items.Clear();
        // Picker automatically updates
    }

    public event PropertyChangedEventHandler PropertyChanged;
}
```

## Best Practices

1. **Use ObservableCollection** for data that changes dynamically
2. **Implement INotifyPropertyChanged** for proper MVVM binding
3. **Use DisplayMemberPath** for complex objects to show specific properties
4. **Keep item text concise** to fit within column width
5. **Test with various data sizes** to ensure performance
6. **Provide default SelectedIndex** for better UX
7. **Handle empty data sources** gracefully

## Troubleshooting

### Issue: Items not displaying

**Solution:**
- Verify `ItemsSource` is not null or empty
- Check binding context is set correctly
- Ensure `ObservableCollection` is properly initialized
- Verify column is added to picker's `Columns` collection

### Issue: Selected item not updating in ViewModel

**Solution:**
- Ensure `Mode=TwoWay` is set on binding
- Implement `INotifyPropertyChanged` correctly
- Verify property setter calls `OnPropertyChanged`

### Issue: Dynamic updates not reflecting

**Solution:**
- Use `ObservableCollection` instead of `List`
- Don't replace the entire collection, modify it
- Ensure binding context is set before adding items
