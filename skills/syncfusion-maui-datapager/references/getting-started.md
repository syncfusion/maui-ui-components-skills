# Getting Started with DataPager

## Table of Contents
- [Prerequisites](#prerequisites)
- [Step 1: Create a .NET MAUI Project](#step-1-create-a-net-maui-project)
- [Step 2: Install the NuGet Package](#step-2-install-the-nuget-package)
- [Step 3: Register the Syncfusion Handler](#step-3-register-the-syncfusion-handler)
- [Step 4: Import the DataPager Namespace](#step-4-import-the-datapager-namespace)
- [Step 5: Define Model and ViewModel](#step-5-define-model-and-viewmodel)
- [Step 6: Add the DataPager with DataGrid](#step-6-add-the-datapager-with-datagrid)
- [Source-to-PagedSource Binding Pattern](#source-to-pagedsource-binding-pattern)
- [Troubleshooting](#troubleshooting)

## Prerequisites

Before proceeding, ensure the following are set up:

1. Install [.NET 9 SDK](https://dotnet.microsoft.com/en-us/download/dotnet/9.0) or later.
2. Set up a .NET MAUI environment with Visual Studio 2022 v17.12 or later, Visual Studio Code, or JetBrains Rider 2024.3 or later.
3. Ensure the .NET MAUI workloads are installed and configured.

## Step 1: Create a .NET MAUI Project

**Visual Studio:**
1. Go to **File > New > Project** and choose the **.NET MAUI App** template.
2. Name the project and choose a location. Then, click **Next.**
3. Select the .NET framework version and click **Create.**

**Visual Studio Code:**
1. Open the command palette by pressing `Ctrl+Shift+P` and type **.NET:New Project** and press Enter.
2. Choose the **.NET MAUI App** template.
3. Select the project location, type the project name, and press **Enter.**
4. Then choose **Create project.**

**JetBrains Rider:**
1. Go to **File > New Solution**, select .NET (C#), and choose the .NET MAUI App template.
2. Enter the Project Name, Solution Name, and Location.
3. Select the .NET framework version and click **Create.**

## Step 2: Install the NuGet Package

**Visual Studio / Rider:**
1. In **Solution Explorer**, right-click the project and choose **Manage NuGet Packages.**
2. Search for `Syncfusion.Maui.DataPager` and install the latest version.
3. Ensure the necessary dependencies are installed correctly, and the project is restored.

**Visual Studio Code:**
1. Press `Ctrl+` ` (backtick) to open the integrated terminal.
2. Ensure you're in the project root directory where your `.csproj` file is located.
3. Run the command `dotnet add package Syncfusion.Maui.DataPager`.
4. To ensure all dependencies are installed, run `dotnet restore`.

**NuGet Package:** `Syncfusion.Maui.DataPager`

## Step 3: Register the Syncfusion Handler

To use Syncfusion controls, register the Syncfusion core handler in your application's startup configuration in `MauiProgram.cs` (located at the root of your project):

```csharp
using Syncfusion.Maui.Core.Hosting;

// Inside CreateMauiApp method:
builder.ConfigureSyncfusionCore();
```

**Important:** Forgetting to call `ConfigureSyncfusionCore()` will result in runtime errors when using the DataPager.

## Step 4: Import the DataPager Namespace

Add the following namespace in your XAML or C#:

**XAML:**

```xaml
xmlns:pager="clr-namespace:Syncfusion.Maui.DataPager;assembly=Syncfusion.Maui.DataPager"
```

**C#:**

```csharp
using Syncfusion.Maui.DataPager;
```

## Step 5: Define Model and ViewModel

Create a data model class. Save it as `OrderInfo.cs`:

```csharp
public class OrderInfo
{
    private string? orderID;
    private string? customerID;
    private string? customer;
    private string? shipCity;
    private string? shipCountry;

    public string? OrderID
    {
        get { return orderID; }
        set { this.orderID = value; }
    }

    public string? CustomerID
    {
        get { return customerID; }
        set { this.customerID = value; }
    }

    public string? ShipCountry
    {
        get { return shipCountry; }
        set { this.shipCountry = value; }
    }

    public string? Customer
    {
        get { return this.customer; }
        set { this.customer = value; }
    }

    public string? ShipCity
    {
        get { return shipCity; }
        set { this.shipCity = value; }
    }

    public OrderInfo(string orderId, string customerId, string country, string customer, string shipCity)
    {
        this.OrderID = orderId;
        this.CustomerID = customerId;
        this.Customer = customer;
        this.ShipCountry = country;
        this.ShipCity = shipCity;
    }
}
```

Next, create a data repository (view model) class that manages a collection of `OrderInfo` objects. Save it as `OrderInfoRepository.cs`:

```csharp
public class OrderInfoRepository
{
    private ObservableCollection<OrderInfo> orders;
    public ObservableCollection<OrderInfo> Orders
    {
        get { return orders; }
        set { this.orders = value; }
    }

    public OrderInfoRepository()
    {
        orders = new ObservableCollection<OrderInfo>();
        this.GenerateOrders();
    }

    public void GenerateOrders()
    {
        orders.Add(new OrderInfo("1001", "Maria Anders", "Germany", "ALFKI", "Berlin"));
        orders.Add(new OrderInfo("1002", "Ana Trujillo", "Mexico", "ANATR", "Mexico D.F."));
        orders.Add(new OrderInfo("1003", "Ant Fuller", "Mexico", "ANTON", "Mexico D.F."));
        orders.Add(new OrderInfo("1004", "Thomas Hardy", "UK", "AROUT", "London"));
        orders.Add(new OrderInfo("1005", "Tim Adams", "Sweden", "BERGS", "London"));
        orders.Add(new OrderInfo("1006", "Hanna Moos", "Germany", "BLAUS", "Mannheim"));
        orders.Add(new OrderInfo("1007", "Andrew Fuller", "France", "BLONP", "Strasbourg"));
        orders.Add(new OrderInfo("1008", "Martin King", "Spain", "BOLID", "Madrid"));
        orders.Add(new OrderInfo("1009", "Lenny Lin", "France", "BONAP", "Marsiella"));
    }
}
```

A simple view model exposes the repository:

```csharp
public class OrderInfoViewModel
{
    private OrderInfoRepository orderRepository;

    public ObservableCollection<OrderInfo> Orders
    {
        get { return orderRepository.Orders; }
    }

    public OrderInfoViewModel()
    {
        orderRepository = new OrderInfoRepository();
    }
}
```

## Step 6: Add the DataPager with DataGrid

Create a `SfDataPager` instance and bind your data collection to the `Source` property. Then bind the `PagedSource` to a data display control like `SfDataGrid`.

**XAML:**

```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataPagerDemo"
             xmlns:syncfusion="clr-namespace:Syncfusion.Maui.DataGrid;assembly=Syncfusion.Maui.DataGrid"
             xmlns:pager="clr-namespace:Syncfusion.Maui.DataPager;assembly=Syncfusion.Maui.DataPager"
             x:Class="DataPagerDemo.MainPage">

    <ContentPage.BindingContext>
        <local:OrderInfoViewModel x:Name="viewModel"/>
    </ContentPage.BindingContext>

    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>
        <Border Grid.Row="1" Padding="5">
            <pager:SfDataPager x:Name="dataPager"
                               PageSize="15"
                               NumericButtonCount="10"
                               Source="{Binding Orders}">
            </pager:SfDataPager>
        </Border>
        <syncfusion:SfDataGrid x:Name="dataGrid"
                               Grid.Row="0"
                               ItemsSource="{Binding Source={x:Reference dataPager}, Path=PagedSource}">
        </syncfusion:SfDataGrid>
    </Grid>
</ContentPage>
```

**C# (programmatic):**

```csharp
using Syncfusion.Maui.DataGrid;
using Syncfusion.Maui.DataPager;

public partial class MainPage : ContentPage
{
    // Note: The XAML approach above is the recommended method. This C# example demonstrates
    // programmatic creation for scenarios where declarative XAML markup is not available.
    public MainPage()
    {
        InitializeComponent();
        SfDataPager dataPager = new SfDataPager();
        OrderInfoViewModel viewModel = new OrderInfoViewModel();
        dataPager.PageSize = 15;
        dataPager.NumericButtonCount = 10;
        dataPager.Source = viewModel.Orders;

        SfDataGrid dataGrid = new SfDataGrid();
        dataGrid.ItemsSource = dataPager.PagedSource;

        Border border = new Border();
        border.Padding = new Thickness(5);
        border.Content = dataPager;

        Grid grid = new Grid();
        grid.RowDefinitions.Add(new RowDefinition() { Height = GridLength.Star });
        grid.RowDefinitions.Add(new RowDefinition() { Height = GridLength.Auto });
        grid.Children.Add(dataGrid);
        grid.Children.Add(border);
        grid.SetRow(dataGrid, 0);
        grid.SetRow(border, 1);
        this.Content = grid;
    }
}
```

## Source-to-PagedSource Binding Pattern

The DataPager follows a two-binding pattern:

1. **Input:** Bind the full data collection to `SfDataPager.Source`. The pager internally creates `SfDataPager.PagedSource`.
2. **Output:** Bind `SfDataPager.PagedSource` to the `ItemsSource` of a display control (e.g., `SfDataGrid`).

This keeps the display control unaware of paging logic — it only sees the current page's data.

**Key properties:**
- `Source`: The full data collection (ObservableCollection, List, etc.).
- `PagedSource`: The paged view — bind this to a display control.
- `PageSize`: Number of items per page. Must not be 0 (throws `ArgumentException`).
- `NumericButtonCount`: Number of numeric buttons to display.

## Troubleshooting

| Problem | Cause | Solution |
|---|---|---|
| Runtime error when rendering the DataPager | `ConfigureSyncfusionCore()` not called | Register the handler in `MauiProgram.cs` |
| `ArgumentException` thrown | `PageSize` set to 0 | Set `PageSize` to a positive integer |
| DataGrid shows no data | `PagedSource` not bound to `ItemsSource` | Bind `ItemsSource` to `{Binding Source={x:Reference dataPager}, Path=PagedSource}` |
| Numeric buttons not visible | `NumericButtonCount` too low or `DisplayMode` excludes numeric | Set `NumericButtonCount` to a positive value and use a `DisplayMode` that includes `Numeric` |
| Pager layout overflows screen | Too many numeric buttons for available width | Reduce `NumericButtonCount` or use `NumericButtonsGenerateMode="Auto"` |
