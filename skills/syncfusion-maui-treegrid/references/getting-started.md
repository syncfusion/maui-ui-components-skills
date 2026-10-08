# Getting Started with TreeGrid

## Table of Contents
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Handler Registration](#handler-registration)
- [Creating Data Models](#creating-data-models)
- [Creating a ViewModel](#creating-a-viewmodel)
- [Basic TreeGrid Setup](#basic-treegrid-setup)
- [Troubleshooting](#troubleshooting)

## Prerequisites

Before proceeding, ensure the following are set up:

1. Install [.NET 9 SDK](https://dotnet.microsoft.com/en-us/download/dotnet/9.0) or later.
2. Set up a .NET MAUI environment with Visual Studio 2022 v17.12 or later, Visual Studio Code, or JetBrains Rider 2024.3 or later.
3. Ensure the .NET MAUI workloads are installed and configured.

## Installation

### Step 1: Create a New .NET MAUI Project

**Visual Studio:**
1. Go to **File > New > Project** and choose the **.NET MAUI App** template.
2. Name the project and choose a location, then click **Next**.
3. Select the .NET framework version and click **Create**.

**Visual Studio Code:**
1. Press `Ctrl+Shift+P`, type **.NET:New Project**, and press Enter.
2. Choose the **.NET MAUI App** template.
3. Select the project location, type the project name, and press **Enter**.

**JetBrains Rider:**
1. Go to **File > New Solution**, select .NET (C#), and choose the .NET MAUI App template.
2. Enter the Project Name, Solution Name, and Location.
3. Select the .NET framework version and click **Create**.

### Step 2: Install the Syncfusion MAUI Tree Grid NuGet Package

**Visual Studio / Rider:**
1. In **Solution Explorer**, right-click the project and choose **Manage NuGet Packages**.
2. Search for `Syncfusion.Maui.TreeGrid` and install the latest version.
3. Ensure the necessary dependencies are installed correctly and the project is restored.

**Visual Studio Code:**
1. Press `Ctrl+` + `` ` `` (backtick) to open the integrated terminal.
2. Ensure you are in the project root directory where your `.csproj` file is located.
3. Run `dotnet add package Syncfusion.Maui.TreeGrid`.
4. Run `dotnet restore` to ensure all dependencies are installed.

**NuGet Package:** `Syncfusion.Maui.TreeGrid`

## Handler Registration

To use Syncfusion controls, register the Syncfusion core handler in your application's startup configuration. In `MauiProgram.cs`, add the namespace:

```csharp
using Syncfusion.Maui.Core.Hosting;
```

Then register the Syncfusion core handler in the `CreateMauiApp` method:

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

**Important:** Forgetting to call `ConfigureSyncfusionCore()` will result in runtime errors when using the TreeGrid.

## Creating Data Models

Create a simple data model that supports hierarchical data through a `Children` collection. Implementing `INotifyPropertyChanged` enables automatic UI updates when property values change.

**EmployeeInfo.cs:**

```csharp
using System.Collections.ObjectModel;
using System.ComponentModel;

public class EmployeeInfo : INotifyPropertyChanged
{
    private string? _firstName;
    private string? _lastName;
    private int? _empId;
    private double? _salary;
    private string? _title;
    private ObservableCollection<EmployeeInfo> _children = new ObservableCollection<EmployeeInfo>();

    public string? FirstName
    {
        get { return _firstName; }
        set { _firstName = value; OnPropertyChanged(nameof(FirstName)); }
    }

    public string? LastName
    {
        get { return _lastName; }
        set { _lastName = value; OnPropertyChanged(nameof(LastName)); }
    }

    public int? EmpId
    {
        get { return _empId; }
        set { _empId = value; OnPropertyChanged(nameof(EmpId)); }
    }

    public double? Salary
    {
        get { return _salary; }
        set { _salary = value; OnPropertyChanged(nameof(Salary)); }
    }

    public string? Title
    {
        get { return _title; }
        set { _title = value; OnPropertyChanged(nameof(Title)); }
    }

    public ObservableCollection<EmployeeInfo> Children
    {
        get { return _children; }
        set { _children = value; OnPropertyChanged(nameof(Children)); }
    }

    public event PropertyChangedEventHandler? PropertyChanged;

    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

**Why INotifyPropertyChanged:**
- Enables automatic UI updates when property values change
- Required for two-way data binding
- Essential for edit and live-update scenarios

## Creating a ViewModel

Create a ViewModel that populates the hierarchical employee data:

**EmployeeInfoViewModel.cs:**

```csharp
using System.Collections.ObjectModel;

public class EmployeeInfoViewModel
{
    public ObservableCollection<EmployeeInfo> PersonDetails { get; set; }

    public EmployeeInfoViewModel()
    {
        PersonDetails = CreateEmployeeData();
    }

    private ObservableCollection<EmployeeInfo> CreateEmployeeData()
    {
        var employeeList = new ObservableCollection<EmployeeInfo>();

        var childCollection1 = new ObservableCollection<EmployeeInfo>
        {
            new EmployeeInfo { FirstName = "Robert", LastName = "Fuller", EmpId = 1008, Salary = 120000, Title = "Design Engineer" },
            new EmployeeInfo { FirstName = "Janet", LastName = "Leverling", EmpId = 1009, Salary = 100000, Title = "Engineering Manager" }
        };

        var childCollection2 = new ObservableCollection<EmployeeInfo>
        {
            new EmployeeInfo { FirstName = "Nancy", LastName = "Davolio", EmpId = 1011, Salary = 85000, Title = "Accounts Supervisor" },
            new EmployeeInfo { FirstName = "Margaret", LastName = "Peacock", EmpId = 1012, Salary = 32000, Title = "Accounts Representative" }
        };

        employeeList.Add(new EmployeeInfo
        {
            FirstName = "Sean", LastName = "Jacobson", EmpId = 1001, Salary = 200000, Title = "General Manager", Children = childCollection1
        });
        employeeList.Add(new EmployeeInfo
        {
            FirstName = "Phyllis", LastName = "Allen", EmpId = 1002, Salary = 45000, Title = "Accounts Manager", Children = childCollection2
        });

        return employeeList;
    }
}
```

## Basic TreeGrid Setup

### Import the Namespace

**XAML:**

```xaml
xmlns:syncfusion="clr-namespace:Syncfusion.Maui.TreeGrid;assembly=Syncfusion.Maui.TreeGrid"
```

**C#:**

```csharp
using Syncfusion.Maui.TreeGrid;
```

### XAML Implementation

Set the `BindingContext` to the ViewModel and bind the `ItemsSource` and `ChildPropertyName`:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:syncfusion="clr-namespace:Syncfusion.Maui.TreeGrid;assembly=Syncfusion.Maui.TreeGrid"
             xmlns:local="clr-namespace:YourNamespace"
             x:Class="YourNamespace.MainPage">

    <ContentPage.BindingContext>
        <local:EmployeeInfoViewModel />
    </ContentPage.BindingContext>

    <syncfusion:SfTreeGrid x:Name="treeGrid"
                           ItemsSource="{Binding PersonDetails}"
                           ChildPropertyName="Children" />

</ContentPage>
```

**Key Points:**
- Import the namespace: `xmlns:syncfusion="clr-namespace:Syncfusion.Maui.TreeGrid;assembly=Syncfusion.Maui.TreeGrid"`
- `ChildPropertyName="Children"` establishes the hierarchical relationship by pointing to the nested collection property.

### C# Code-Behind Implementation

```csharp
using Syncfusion.Maui.TreeGrid;

namespace YourNamespace
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();

            EmployeeInfoViewModel viewModel = new EmployeeInfoViewModel();
            SfTreeGrid treeGrid = new SfTreeGrid();
            treeGrid.ItemsSource = viewModel.PersonDetails;
            treeGrid.ChildPropertyName = "Children";
            this.Content = treeGrid;
        }
    }
}
```

## Troubleshooting

| Problem | Cause | Solution |
|---------|-------|----------|
| Control does not render / runtime error | Syncfusion handler not registered | Call `builder.ConfigureSyncfusionCore()` in `MauiProgram.cs` |
| No hierarchy shown, all rows flat | `ChildPropertyName` not set or misspelled | Ensure `ChildPropertyName` matches the nested collection property name exactly |
| NuGet restore fails | Wrong package name or .NET version | Verify the package is `Syncfusion.Maui.TreeGrid` and .NET 9 SDK is installed |
| Column headers show property names instead of friendly text | `HeaderText` not set | Set `HeaderText` on each column, or handle the `AutoGeneratingColumn` event |
