# Getting Started with DataGrid

## Table of Contents
- [Installation](#installation)
- [Handler Registration](#handler-registration)
- [Basic DataGrid Setup](#basic-datagrid-setup)
- [Creating Data Models](#creating-data-models)
- [Data Binding](#data-binding)
- [Binding with Different Data Sources](#binding-with-different-data-sources)
- [Initial Configuration](#initial-configuration)
- [Troubleshooting](#troubleshooting)

## Installation

### Step 1: Create a New .NET MAUI Project

**Visual Studio:**
1. Go to **File > New > Project**
2. Choose **.NET MAUI App** template
3. Name the project and choose a location
4. Click **Next**, select the .NET framework version, and click **Create**

**Visual Studio Code:**
1. Press `Ctrl+Shift+P` (or `Cmd+Shift+P` on Mac)
2. Type **.NET:New Project** and press Enter
3. Choose **.NET MAUI App** template
4. Select project location, enter project name, and press **Enter**
5. Choose **Create project**

**JetBrains Rider:**
1. Go to **File > New Solution**
2. Select .NET (C#) and choose **.NET MAUI App** template
3. Enter Project Name, Solution Name, and Location
4. Select the .NET framework version and click **Create**

### Step 2: Install the NuGet Package

**Visual Studio / Rider:**
1. In **Solution Explorer**, right-click the project
2. Choose **Manage NuGet Packages**
3. Search for `Syncfusion.Maui.DataGrid`
4. Install the latest version
5. Ensure dependencies are installed correctly

**Visual Studio Code:**
1. Press `Ctrl+`` (backtick) to open integrated terminal
2. Ensure you're in the project root directory
3. Run: `dotnet add package Syncfusion.Maui.DataGrid`
4. Run: `dotnet restore` to ensure all dependencies are installed

**NuGet Package:** `Syncfusion.Maui.DataGrid`

**Dependency:** `Syncfusion.Maui.Core` (automatically installed)

## Handler Registration

The `Syncfusion.Maui.Core` package is a required dependency for all Syncfusion MAUI controls. You must register the handler in `MauiProgram.cs`.

**MauiProgram.cs:**

```csharp
using Microsoft.Maui;
using Microsoft.Maui.Hosting;
using Microsoft.Maui.Controls.Hosting;
using Syncfusion.Maui.Core.Hosting;

namespace YourNamespace
{
    public static class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            var builder = MauiApp.CreateBuilder();
            builder
                .UseMauiApp<App>()
                .ConfigureFonts(fonts =>
                {
                    fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
                    fonts.AddFont("OpenSans-Semibold.ttf", "OpenSansSemibold");
                });

            // Register Syncfusion handler
            builder.ConfigureSyncfusionCore();
            
            return builder.Build();
        }
    }
}
```

**Important:** Forgetting to call `ConfigureSyncfusionCore()` will result in runtime errors when using the DataGrid.

## Basic DataGrid Setup

### XAML Implementation

**MainPage.xaml:**

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:syncfusion="clr-namespace:Syncfusion.Maui.DataGrid;assembly=Syncfusion.Maui.DataGrid"
             x:Class="YourNamespace.MainPage">
    
    <syncfusion:SfDataGrid x:Name="dataGrid" />
    
</ContentPage>
```

**Key Points:**
- Import the namespace: `xmlns:syncfusion="clr-namespace:Syncfusion.Maui.DataGrid;assembly=Syncfusion.Maui.DataGrid"`
- Use the control: `<syncfusion:SfDataGrid />`

### C# Code-Behind Implementation

**MainPage.xaml.cs:**

```csharp
using Syncfusion.Maui.DataGrid;

namespace YourNamespace
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            
            SfDataGrid dataGrid = new SfDataGrid();
            this.Content = dataGrid;
        }
    }
}
```

## Creating Data Models

### Simple Data Model

Create a data model class to represent your data structure:

**OrderInfo.cs:**

```csharp
using System.ComponentModel;

namespace YourNamespace
{
    public class OrderInfo : INotifyPropertyChanged
    {
        private int orderID;
        private string customerID;
        private string shipCountry;
        private decimal freight;
        private DateTime orderDate;

        public int OrderID
        {
            get { return orderID; }
            set
            {
                orderID = value;
                OnPropertyChanged(nameof(OrderID));
            }
        }

        public string CustomerID
        {
            get { return customerID; }
            set
            {
                customerID = value;
                OnPropertyChanged(nameof(CustomerID));
            }
        }

        public string ShipCountry
        {
            get { return shipCountry; }
            set
            {
                shipCountry = value;
                OnPropertyChanged(nameof(ShipCountry));
            }
        }

        public decimal Freight
        {
            get { return freight; }
            set
            {
                freight = value;
                OnPropertyChanged(nameof(Freight));
            }
        }

        public DateTime OrderDate
        {
            get { return orderDate; }
            set
            {
                orderDate = value;
                OnPropertyChanged(nameof(OrderDate));
            }
        }

        public event PropertyChangedEventHandler PropertyChanged;

        protected void OnPropertyChanged(string propertyName)
        {
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
        }
    }
}
```

**Why INotifyPropertyChanged:**
- Enables automatic UI updates when property values change
- Required for two-way data binding
- Essential for edit scenarios

### ViewModel with Sample Data

**OrderViewModel.cs:**

```csharp
using System.Collections.ObjectModel;
using System.ComponentModel;

namespace YourNamespace
{
    public class OrderViewModel : INotifyPropertyChanged
    {
        private ObservableCollection<OrderInfo> orderInfoCollection;

        public ObservableCollection<OrderInfo> OrderInfoCollection
        {
            get { return orderInfoCollection; }
            set
            {
                orderInfoCollection = value;
                OnPropertyChanged(nameof(OrderInfoCollection));
            }
        }

        public OrderViewModel()
        {
            OrderInfoCollection = new ObservableCollection<OrderInfo>();
            GenerateOrders();
        }

        private void GenerateOrders()
        {
            OrderInfoCollection.Add(new OrderInfo
            {
                OrderID = 1001,
                CustomerID = "ALFKI",
                ShipCountry = "Germany",
                Freight = 32.38m,
                OrderDate = new DateTime(2024, 1, 15)
            });

            OrderInfoCollection.Add(new OrderInfo
            {
                OrderID = 1002,
                CustomerID = "FRANS",
                ShipCountry = "Mexico",
                Freight = 11.61m,
                OrderDate = new DateTime(2024, 1, 16)
            });

            OrderInfoCollection.Add(new OrderInfo
            {
                OrderID = 1003,
                CustomerID = "MEREP",
                ShipCountry = "Canada",
                Freight = 45.34m,
                OrderDate = new DateTime(2024, 1, 17)
            });

            OrderInfoCollection.Add(new OrderInfo
            {
                OrderID = 1004,
                CustomerID = "BONAP",
                ShipCountry = "France",
                Freight = 58.17m,
                OrderDate = new DateTime(2024, 1, 18)
            });

            OrderInfoCollection.Add(new OrderInfo
            {
                OrderID = 1005,
                CustomerID = "EASTC",
                ShipCountry = "USA",
                Freight = 22.98m,
                OrderDate = new DateTime(2024, 1, 19)
            });
        }

        public event PropertyChangedEventHandler PropertyChanged;

        protected void OnPropertyChanged(string propertyName)
        {
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
        }
    }
}
```

## Data Binding

### Binding in XAML

**MainPage.xaml:**

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:syncfusion="clr-namespace:Syncfusion.Maui.DataGrid;assembly=Syncfusion.Maui.DataGrid"
             xmlns:local="clr-namespace:YourNamespace"
             x:Class="YourNamespace.MainPage">
    
    <ContentPage.BindingContext>
        <local:OrderViewModel />
    </ContentPage.BindingContext>
    
    <syncfusion:SfDataGrid x:Name="dataGrid"
                           ItemsSource="{Binding OrderInfoCollection}"
                           AutoGenerateColumnsMode="Reset" />
    
</ContentPage>
```

### Binding in Code-Behind

**MainPage.xaml.cs:**

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        
        OrderViewModel viewModel = new OrderViewModel();
        this.BindingContext = viewModel;
        
        dataGrid.ItemsSource = viewModel.OrderInfoCollection;
    }
}
```

**Or directly:**

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        
        OrderViewModel viewModel = new OrderViewModel();
        dataGrid.SetBinding(SfDataGrid.ItemsSourceProperty, new Binding("OrderInfoCollection", source: viewModel));
    }
}
```

## Binding with Different Data Sources

### Binding with ObservableCollection

**Recommended for most scenarios:**

```csharp
public class OrderViewModel
{
    public ObservableCollection<OrderInfo> Orders { get; set; }
    
    public OrderViewModel()
    {
        Orders = new ObservableCollection<OrderInfo>();
        // Add items...
    }
}
```

**In XAML:**
```xml
<syncfusion:SfDataGrid ItemsSource="{Binding Orders}" />
```

**Benefits:**
- Automatically notifies the grid when items are added, removed, or cleared
- Implements `INotifyCollectionChanged`
- UI updates automatically

### Binding with List<T>

```csharp
public class OrderViewModel
{
    public List<OrderInfo> Orders { get; set; }
    
    public OrderViewModel()
    {
        Orders = new List<OrderInfo>();
        // Add items...
    }
}
```

```csharp
// Add item
dataGrid.ItemsSource = viewModel.Orders; // Force refresh
```

### Binding with IEnumerable

The DataGrid supports any collection implementing `IEnumerable`:

```csharp
public IEnumerable<OrderInfo> GetOrders()
{
    return new List<OrderInfo>
    {
        new OrderInfo { OrderID = 1, CustomerID = "ALFKI" },
        new OrderInfo { OrderID = 2, CustomerID = "FRANS" }
    };
}

// Binding
dataGrid.ItemsSource = GetOrders();
```

**All data operations (sorting, filtering, grouping) are supported.**

### Binding with DataTable

The DataGrid supports binding to `DataTable`:

```csharp
using System.Data;

public DataTable GetOrdersDataTable()
{
    DataTable table = new DataTable("Orders");
    
    table.Columns.Add("OrderID", typeof(int));
    table.Columns.Add("CustomerID", typeof(string));
    table.Columns.Add("ShipCountry", typeof(string));
    table.Columns.Add("Freight", typeof(decimal));
    
    table.Rows.Add(1001, "ALFKI", "Germany", 32.38);
    table.Rows.Add(1002, "FRANS", "Mexico", 11.61);
    table.Rows.Add(1003, "MEREP", "Canada", 45.34);
    
    return table;
}

// Binding
DataTable ordersTable = GetOrdersDataTable();
dataGrid.ItemsSource = ordersTable;
```

**DataTable Limitations:**
- Custom sorting is NOT supported
- `SfDataGrid.View.Filter` is NOT supported
- `LiveDataUpdateMode` (AllowDataShaping, AllowSummaryUpdate) is NOT supported

**UI Auto-Refresh:** The DataGrid automatically refreshes when rows are added, removed, or cleared from a DataTable.

### Binding with Dynamic Objects

The DataGrid supports binding to dynamic data objects using `ExpandoObject`:

```csharp
using System.Dynamic;
using System.Collections.ObjectModel;

public class EmployeeViewModel
{
    public ObservableCollection<dynamic> Employees { get; set; }
    
    public EmployeeViewModel()
    {
        Employees = new ObservableCollection<dynamic>();
        GenerateEmployees();
    }
    
    private void GenerateEmployees()
    {
        for (int i = 1; i <= 10; i++)
        {
            dynamic employee = new ExpandoObject();
            employee.EmployeeID = i;
            employee.EmployeeName = $"Employee {i}";
            employee.ContactID = i + 100;
            Employees.Add(employee);
        }
    }
}
```

**Manual Column Definition (Square Brackets Required):**

```xml
<syncfusion:SfDataGrid ItemsSource="{Binding Employees}"
                       AutoGenerateColumnsMode="None">
    <syncfusion:SfDataGrid.Columns>
        <syncfusion:DataGridNumericColumn MappingName="[EmployeeID]" 
                                          HeaderText="ID" />
        <syncfusion:DataGridTextColumn MappingName="[EmployeeName]" 
                                       HeaderText="Name" />
        <syncfusion:DataGridNumericColumn MappingName="[ContactID]" 
                                          HeaderText="Contact" />
    </syncfusion:SfDataGrid.Columns>
</syncfusion:SfDataGrid>
```

**Auto-Generation with Event Handler:**

```xml
<syncfusion:SfDataGrid ItemsSource="{Binding Employees}"
                       AutoGeneratingColumn="DataGrid_AutoGeneratingColumn" />
```

```csharp
private void DataGrid_AutoGeneratingColumn(object sender, DataGridAutoGeneratingColumnEventArgs e)
{
    // Add square brackets for dynamic properties
    e.Column.MappingName = "[" + e.Column.MappingName + "]";
}
```

**Dynamic Object Limitations:**
- `LiveDataUpdateMode` - AllowDataShaping and AllowSummaryUpdate are NOT supported

### Binding Complex Properties

The DataGrid supports binding to nested properties:

**Model with Complex Property:**

```csharp
public class Customer
{
    public string CustomerID { get; set; }
    public string ContactName { get; set; }
}

public class OrderInfo
{
    public int OrderID { get; set; }
    public Customer Customer { get; set; } // Complex property
    public string ShipCity { get; set; }
}
```

**Bind Using Dot Notation:**

```xml
<syncfusion:SfDataGrid AutoGenerateColumnsMode="None"
                       ItemsSource="{Binding Orders}">
    <syncfusion:SfDataGrid.Columns>
        <syncfusion:DataGridNumericColumn MappingName="OrderID" />
        <syncfusion:DataGridTextColumn MappingName="Customer.CustomerID" />
        <syncfusion:DataGridTextColumn MappingName="Customer.ContactName" />
        <syncfusion:DataGridTextColumn MappingName="ShipCity" />
    </syncfusion:SfDataGrid.Columns>
</syncfusion:SfDataGrid>
```

**Binding Indexers and Dictionaries:**

For complex types like indexers or dictionaries, set `UseBindingValue="True"`:

```xml
<syncfusion:SfDataGrid AutoGenerateColumnsMode="None"
                       ItemsSource="{Binding Orders}">
    <syncfusion:SfDataGrid.Columns>
        <syncfusion:DataGridTextColumn MappingName="OrderID" 
                                       UseBindingValue="True" />
        <syncfusion:DataGridTextColumn MappingName="CustomerID" 
                                       UseBindingValue="True" />
    </syncfusion:SfDataGrid.Columns>
</syncfusion:SfDataGrid>
```

**Complex Property Limitations:**
- `LiveDataUpdateMode` - AllowDataShaping and AllowSummaryUpdate are NOT supported

## Initial Configuration

### Basic Configuration Options

```xml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       ItemsSource="{Binding Orders}"
                       AutoGenerateColumnsMode="Reset"
                       AllowEditing="False"
                       AllowFiltering="False"
                       SelectionMode="Single"
                       NavigationMode="Cell"
                       ColumnWidthMode="Auto"
                       HeaderRowHeight="50"
                       RowHeight="40">
</syncfusion:SfDataGrid>
```

**Common Properties:**

| Property | Default | Description |
|----------|---------|-------------|
| `AutoGenerateColumnsMode` | Reset | Controls column auto-generation (None, Reset, ResetAll, RetainOld) |
| `AllowEditing` | False | Enable/disable cell editing |
| `AllowFiltering` | False | Enable/disable filtering |
| `SelectionMode` | None | Selection mode (None, Single, Multiple, SingleDeselect, Extended) |
| `NavigationMode` | Cell | Navigation mode (Cell, Row, Any) |
| `ColumnWidthMode` | None | Column width calculation (Auto, Fill, FitByCell, FitByHeader, LastColumnFill, None) |
| `HeaderRowHeight` | 55 | Height of header row |
| `RowHeight` | 47 | Default row height |

## Troubleshooting

### DataGrid Not Appearing

**Problem:** DataGrid control is not visible in the app.

**Solutions:**
1. Verify handler is registered: Ensure `ConfigureSyncfusionCore()` is called in `MauiProgram.cs`
2. Check NuGet package: Verify `Syncfusion.Maui.DataGrid` is installed
3. Check namespace: Ensure correct XML namespace is imported
4. Verify ItemsSource: Ensure data source is not null and contains data

### Columns Not Displaying

**Problem:** No columns appear in the grid.

**Solutions:**
1. Check `AutoGenerateColumnsMode`: Set to `Reset` or `ResetAll` for auto-generation
2. Verify ItemsSource has data
3. For manual columns, ensure `AutoGenerateColumnsMode="None"`
4. Check property names in `MappingName` match model properties exactly

### Data Not Updating

**Problem:** Changes to data are not reflected in the grid.

**Solutions:**
1. Use `ObservableCollection<T>` instead of `List<T>`
2. Implement `INotifyPropertyChanged` in your data model
3. For `List<T>`, reassign ItemsSource after changes:
   ```csharp
   dataGrid.ItemsSource = null;
   dataGrid.ItemsSource = myList;
   ```

### Handler Not Registered Error

**Problem:** Runtime error: "Handler not registered for control."

**Solution:**
Add `ConfigureSyncfusionCore()` in `MauiProgram.cs`:

```csharp
builder.ConfigureSyncfusionCore();
```

### NuGet Package Restore Issues

**Problem:** Build errors related to Syncfusion packages.

**Solutions:**
1. Clean and rebuild solution
2. Run `dotnet restore` in terminal
3. Delete `bin` and `obj` folders, then rebuild
4. Verify NuGet package source is configured correctly

### Dynamic Property Binding Not Working

**Problem:** Dynamic object properties not displaying.

**Solution:**
Add square brackets to `MappingName`:
```xml
<syncfusion:DataGridTextColumn MappingName="[PropertyName]" />
```

Or use `AutoGeneratingColumn` event:
```csharp
private void DataGrid_AutoGeneratingColumn(object sender, DataGridAutoGeneratingColumnEventArgs e)
{
    e.Column.MappingName = "[" + e.Column.MappingName + "]";
}
```

## Next Steps

After setting up the basic DataGrid:

1. **Configure Columns** - Read [columns.md](columns.md) to learn about column types and customization
2. **Enable Editing** - Read [editing.md](editing.md) to allow users to edit data
3. **Add Sorting/Filtering** - Read [sorting-filtering.md](sorting-filtering.md) for data manipulation
4. **Optimize Performance** - Read [paging-virtualization.md](paging-virtualization.md) for large datasets
5. **Customize Appearance** - Read [styling-customization.md](styling-customization.md) for theming
