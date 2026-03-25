# Getting Started with Smart DataGrid

## Overview

The Smart DataGrid requires several setup steps including NuGet package installation, handler registration, and AI service configuration. This guide walks through each step to get a functional Smart DataGrid running in your .NET MAUI application.

## Step 1: Create a New .NET MAUI Project

### Visual Studio
1. Go to **File > New > Project**
2. Search for and select **.NET MAUI App** template
3. Enter your project name and choose a location
4. Select the .NET framework version (9.0 recommended)
5. Click **Create**

### Visual Studio Code
1. Open Command Palette (Ctrl+Shift+P)
2. Type **.NET: New Project**
3. Select **.NET MAUI App** template
4. Choose project location and name
5. Click **Create project**

### JetBrains Rider
1. Go to **File > New Solution**
2. Select .NET (C#) and choose **.NET MAUI App**
3. Enter Project Name, Solution Name, and Location
4. Select .NET framework version
5. Click **Create**

## Step 2: Install Required NuGet Packages

The Smart DataGrid is part of the `Syncfusion.Maui.SmartComponents` package, which depends on `Syncfusion.Maui.Core`.

1. Right-click your project in **Solution Explorer**
2. Select **Manage NuGet Packages**
3. Search for `Syncfusion.Maui.SmartComponents`
4. Install the latest version
5. Wait for all dependencies to restore

**Package Details:**
- `Syncfusion.Maui.SmartComponents` - Main Smart Components package
- `Syncfusion.Maui.Core` - Core Syncfusion MAUI utilities (auto-installed)

## Step 3: Register Syncfusion Core Handler

In `MauiProgram.cs`, register the Syncfusion core handler by calling `ConfigureSyncfusionCore()`:

```csharp
using Microsoft.Maui;
using Microsoft.Maui.Hosting;
using Syncfusion.Maui.Core.Hosting;

namespace GettingStarted
{
    public class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            var builder = MauiApp.CreateBuilder();
            builder
                .UseMauiApp<App>()
                .ConfigureFonts(fonts =>
                {
                    fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
                });

            // Register Syncfusion core
            builder.ConfigureSyncfusionCore();
            
            return builder.Build();
        }
    }
}
```

## Step 4: Configure Azure OpenAI Service

The Smart DataGrid requires an AI service for natural language processing. Configure Azure OpenAI in `MauiProgram.cs`:

```csharp
using Microsoft.Maui;
using Microsoft.Maui.Hosting;
using Syncfusion.Maui.Core.Hosting;
using Syncfusion.Maui.SmartComponents.Hosting;
using Azure.AI.OpenAI;
using Azure;

namespace GettingStarted
{
    public class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            var builder = MauiApp.CreateBuilder();
            builder
                .UseMauiApp<App>()
                .ConfigureFonts(fonts =>
                {
                    fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
                });

            builder.ConfigureSyncfusionCore();

            // Get Azure credentials (replace with your actual values)
            string key = "<YOUR-AZURE-KEY>";
            Uri azureEndPoint = new Uri("<YOUR-AZURE-ENDPOINT>");
            string deploymentName = "<YOUR-DEPLOYMENT-NAME>";

            // Initialize Azure OpenAI client
            AzureOpenAIClient azureOpenAIClient = new AzureOpenAIClient(azureEndPoint, new AzureKeyCredential(key));
            IChatClient azureChatClient = azureOpenAIClient.GetChatClient(deploymentName).AsIChatClient();

            // Register AI service with dependency injection
            builder.Services.AddChatClient(azureChatClient);
            
            // Configure Syncfusion AI services
            builder.ConfigureSyncfusionAIServices();

            return builder.Build();
        }
    }
}
```

**Azure OpenAI Prerequisites:**
- Azure subscription with OpenAI resource
- API key from Azure portal (Settings > Keys and Endpoint)
- Endpoint URL (https://your-resource.openai.azure.com/)
- Deployment name of your GPT model

## Step 5: Create Data Model

Create the `OrderInfo` class to represent data for the Smart DataGrid:

```csharp
public class OrderInfo
{
    public string OrderID { get; set; }
    public string CustomerID { get; set; }
    public string Customer { get; set; }
    public string ShipCountry { get; set; }
    public string ShipCity { get; set; }

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

## Step 6: Create ViewModel with Data Repository

Create `OrderInfoRepository` to provide sample data:

```csharp
using System.Collections.ObjectModel;

public class OrderInfoRepository
{
    public ObservableCollection<OrderInfo> OrderInfoCollection { get; set; }

    public OrderInfoRepository()
    {
        OrderInfoCollection = new ObservableCollection<OrderInfo>();
        GenerateOrders();
    }

    private void GenerateOrders()
    {
        OrderInfoCollection.Add(new OrderInfo("1001", "ALFKI", "Germany", "Maria Anders", "Berlin"));
        OrderInfoCollection.Add(new OrderInfo("1002", "ANATR", "Mexico", "Ana Trujillo", "Mexico D.F."));
        OrderInfoCollection.Add(new OrderInfo("1003", "ANTON", "Mexico", "Ant Fuller", "Mexico D.F."));
        OrderInfoCollection.Add(new OrderInfo("1004", "AROUT", "UK", "Thomas Hardy", "London"));
        OrderInfoCollection.Add(new OrderInfo("1005", "BERGS", "Sweden", "Tim Adams", "London"));
        OrderInfoCollection.Add(new OrderInfo("1006", "BLAUS", "Germany", "Hanna Moos", "Mannheim"));
        OrderInfoCollection.Add(new OrderInfo("1007", "BLONP", "France", "Andrew Fuller", "Strasbourg"));
        OrderInfoCollection.Add(new OrderInfo("1008", "BOLID", "Spain", "Martin King", "Madrid"));
        OrderInfoCollection.Add(new OrderInfo("1009", "BONAP", "France", "Lenny Lin", "Marsiella"));
        OrderInfoCollection.Add(new OrderInfo("1010", "BOTTM", "Canada", "John Carter", "Lenny Lin"));
    }
}
```

## Step 7: Add Smart DataGrid to UI

In `MainPage.xaml`, add the Smart DataGrid with data binding:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:syncfusion="clr-namespace:Syncfusion.Maui.SmartComponents;assembly=Syncfusion.Maui.SmartComponents"
             xmlns:sfdatagrid="clr-namespace:Syncfusion.Maui.DataGrid;assembly=Syncfusion.Maui.DataGrid"
             xmlns:local="clr-namespace:GettingStarted"
             x:Class="GettingStarted.MainPage">

    <ContentPage.BindingContext>
        <local:OrderInfoRepository x:Name="viewModel" />
    </ContentPage.BindingContext>

    <ContentPage.Content>
        <syncfusion:SfSmartDataGrid x:Name="dataGrid"
                                   ItemsSource="{Binding OrderInfoCollection}">
            <syncfusion:SfSmartDataGrid.Columns>
                <sfdatagrid:DataGridTextColumn HeaderText="Order ID" 
                                      MappingName="OrderID" 
                                      Width="120"/>
                <sfdatagrid:DataGridTextColumn HeaderText="Customer ID"
                                      MappingName="CustomerID"
                                      Width="120" />
                <sfdatagrid:DataGridTextColumn HeaderText="Customer"
                                      MappingName="Customer"
                                      Width="150" />
                <sfdatagrid:DataGridTextColumn HeaderText="Ship Country"
                                      MappingName="ShipCountry"
                                      Width="120" />
                <sfdatagrid:DataGridTextColumn HeaderText="Ship City"
                                      MappingName="ShipCity"
                                      Width="120" />
            </syncfusion:SfSmartDataGrid.Columns>
        </syncfusion:SfSmartDataGrid>
    </ContentPage.Content>
</ContentPage>
```

In `MainPage.xaml.cs`, initialize the grid:

```csharp
namespace GettingStarted
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
        }
    }
}
```

## Step 8: Test AI-Powered Features

Press **F5** to run the application. Once loaded:

1. Click the **AI Assistant** button (default assist button)
2. Type a natural language command in the input field, such as:
   - "Sort by OrderID descending"
   - "Filter ShipCountry equals Germany"
   - "Group by ShipCountry"
   - "Highlight rows where OrderID > 1005 color LightPink"
3. Press **Enter** or click **Send**
4. Observe the Smart DataGrid update based on the command

## Common Issues and Troubleshooting

### Issue: "Syncfusion.Maui.SmartComponents not found"
**Solution:** Verify NuGet package installation. Check NuGet.org for latest version and re-install if needed.

### Issue: Azure OpenAI credentials not working
**Solution:** 
- Verify API key and endpoint from Azure portal
- Check deployment name matches your GPT model deployment
- Ensure your Azure subscription is active

### Issue: "ConfigureSyncfusionAIServices is not available"
**Solution:** Verify you've added the using statement for `Syncfusion.Maui.SmartComponents.Hosting`

### Issue: Smart DataGrid shows empty without data
**Solution:**
- Check ItemsSource binding matches ViewModel property
- Verify OrderInfoCollection has items (use breakpoint)
- Ensure column MappingName matches OrderInfo property names