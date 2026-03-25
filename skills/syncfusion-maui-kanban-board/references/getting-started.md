# Getting Started with .NET MAUI Kanban Board

## Table of Contents
- [Overview](#overview)
- [Installation](#installation)
- [Handler Registration](#handler-registration)
- [Basic SfKanban Setup](#basic-sfkanban-setup)
- [Populating Data](#populating-data)
- [Running the Application](#running-the-application)
- [Troubleshooting](#troubleshooting)

## Overview

This guide walks through setting up the **Syncfusion .NET MAUI Kanban Board (SfKanban)** control from scratch. The SfKanban control is an efficient way to visualize workflows at each stage along their path to completion.

**What you'll learn:**
- How to install the Syncfusion.Maui.Kanban NuGet package
- How to register the Syncfusion core handler
- How to create your first kanban board
- How to populate the board with data
- How to define columns manually or automatically

**Video Tutorial:** For a quick visual guide, watch the [.NET MAUI Kanban Board video tutorial](https://youtu.be/Mq55vjT7ZEA).

## Installation

### Step 1: Create a New .NET MAUI Project

Choose your preferred IDE:

**Visual Studio 2022:**
1. Go to **File > New > Project**
2. Choose the **.NET MAUI App** template
3. Name the project and choose a location
4. Click **Next**, select the .NET framework version, and click **Create**

**Visual Studio Code:**
1. Press `Ctrl+Shift+P` to open the command palette
2. Type **.NET:New Project** and press Enter
3. Choose the **.NET MAUI App** template
4. Select the project location, type the project name, and press Enter
5. Choose **Create project**

**JetBrains Rider:**
1. Go to **File > New Solution**
2. Select **.NET (C#)** and choose the **.NET MAUI App** template
3. Enter the Project Name, Solution Name, and Location
4. Select the .NET framework version and click **Create**

### Step 2: Install the Syncfusion.Maui.Kanban NuGet Package

Choose your preferred method:

**Visual Studio 2022 or Rider:**
1. In **Solution Explorer**, right-click the project
2. Choose **Manage NuGet Packages**
3. Search for **Syncfusion.Maui.Kanban**
4. Install the latest version
5. Ensure dependencies are installed and the project is restored

**Visual Studio Code or Terminal:**
1. Open the integrated terminal (Ctrl + ` in VS Code)
2. Ensure you're in the project root directory (where the .csproj file is)
3. Run the following command:

```bash
dotnet add package Syncfusion.Maui.Kanban
```

4. Restore dependencies:

```bash
dotnet restore
```

**Package Details:**
- **Package Name:** Syncfusion.Maui.Kanban
- **NuGet URL:** [https://www.nuget.org/packages/Syncfusion.Maui.Kanban/](https://www.nuget.org/packages/Syncfusion.Maui.Kanban/)
- **Dependency:** Syncfusion.Maui.Core (installed automatically)

## Handler Registration

The **Syncfusion.Maui.Core** package is a dependent package for all Syncfusion .NET MAUI controls. You must register the Syncfusion core handler in your application.

### Register in MauiProgram.cs

Open `MauiProgram.cs` and add the following:

```csharp
using Microsoft.Maui;
using Microsoft.Maui.Hosting;
using Microsoft.Maui.Controls.Compatibility;
using Microsoft.Maui.Controls.Hosting;
using Microsoft.Maui.Controls.Xaml;
using Syncfusion.Maui.Core.Hosting;  // Add this namespace

namespace KanbanGettingStarted
{
    public static class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            var builder = MauiApp.CreateBuilder();
            builder
                .UseMauiApp<App>()
                .ConfigureSyncfusionCore()  // Register Syncfusion handler
                .ConfigureFonts(fonts =>
                {
                    fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
                });

            return builder.Build();
        }
    }
}
```

**Key Points:**
- Add the `using Syncfusion.Maui.Core.Hosting;` namespace
- Call `.ConfigureSyncfusionCore()` in the builder chain
- This must be done **once** in your application startup

**Why This is Required:**
The handler registration initializes Syncfusion controls and ensures they work correctly across all platforms (Android, iOS, Windows, macOS).

## Basic SfKanban Setup

### Step 1: Import the Namespace

To use the kanban control, import the `Syncfusion.Maui.Kanban` namespace.

**In XAML:**

Add the namespace declaration to your ContentPage:

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:kanban="clr-namespace:Syncfusion.Maui.Kanban;assembly=Syncfusion.Maui.Kanban"
             x:Class="KanbanGettingStarted.MainPage">
    
    <!-- Content here -->
    
</ContentPage>
```

**In C#:**

Add the using statement:

```csharp
using Syncfusion.Maui.Kanban;
```

### Step 2: Initialize SfKanban

**XAML Approach:**

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:kanban="clr-namespace:Syncfusion.Maui.Kanban;assembly=Syncfusion.Maui.Kanban"
             x:Class="KanbanGettingStarted.MainPage">
    
    <kanban:SfKanban x:Name="kanban"/>
    
</ContentPage>
```

**C# Approach (Code-Behind):**

In `MainPage.xaml.cs`:

```csharp
using Syncfusion.Maui.Kanban;

namespace KanbanGettingStarted
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            
            SfKanban kanban = new SfKanban();
            this.Content = kanban;
        }
    }
}
```

At this point, you have an empty kanban board. Next, we'll add data and columns.

## Populating Data

The kanban board requires two main components:
1. **Data source** (items to display as cards)
2. **Columns** (stages of the workflow)

### Using Default KanbanModel

The simplest approach is to use the built-in `KanbanModel` class.

**Step 1: Create a ViewModel**

Create `KanbanViewModel.cs`:

```csharp
using Syncfusion.Maui.Kanban;
using System.Collections.ObjectModel;

namespace KanbanGettingStarted
{
    public class KanbanViewModel
    {
        public ObservableCollection<KanbanModel> Cards { get; set; }
        
        public KanbanViewModel()
        {
            Cards = new ObservableCollection<KanbanModel>();
            
            Cards.Add(new KanbanModel
            {
                ID = 1,
                Title = "iOS - 1002",
                Category = "Open",
                Description = "Analyze customer requirements",
                IndicatorFill = Colors.Red,
                Tags = new List<string> { "Incident", "Customer" }
            });

            Cards.Add(new KanbanModel
            {
                ID = 6,
                Title = "Xamarin - 4576",
                Category = "Open",
                Description = "Show the retrieved data from the server in grid control",
                IndicatorFill = Colors.Green,
                Tags = new List<string> { "Story", "Customer" }
            });

            Cards.Add(new KanbanModel
            {
                ID = 13,
                Title = "UWP - 13",
                Category = "In Progress",
                Description = "Add responsive support to application",
                IndicatorFill = Colors.Brown,
                Tags = new List<string> { "Story", "Customer" }
            });

            Cards.Add(new KanbanModel
            {
                ID = 2543,
                Title = "iOS - 11",
                Category = "Code Review",
                Description = "Check login page validation",
                IndicatorFill = Colors.Brown,
                Tags = new List<string> { "Story", "Customer" }
            });

            Cards.Add(new KanbanModel
            {
                ID = 123,
                Title = "UWP - 21",
                Category = "Done",
                Description = "Check login page validation",
                IndicatorFill = Colors.Brown,
                Tags = new List<string> { "Story", "Customer" }
            });
        }
    }
}
```

**Step 2: Define Columns and Bind Data**

**XAML:**

```xml
<ContentPage xmlns:kanban="clr-namespace:Syncfusion.Maui.Kanban;assembly=Syncfusion.Maui.Kanban"
             xmlns:local="clr-namespace:KanbanGettingStarted">
    
    <kanban:SfKanban x:Name="kanban"
                     AutoGenerateColumns="False"
                     ItemsSource="{Binding Cards}">
        
        <kanban:SfKanban.Columns>
            <kanban:KanbanColumn Title="To Do" 
                                 Categories="Open" />
            <kanban:KanbanColumn Title="In Progress" 
                                 Categories="In Progress" />
            <kanban:KanbanColumn Title="Code Review" 
                                 Categories="Code Review" />
            <kanban:KanbanColumn Title="Done" 
                                 Categories="Done" />
        </kanban:SfKanban.Columns>
        
        <kanban:SfKanban.BindingContext>
            <local:KanbanViewModel />
        </kanban:SfKanban.BindingContext>
        
    </kanban:SfKanban>
    
</ContentPage>
```

**C#:**

```csharp
using Syncfusion.Maui.Kanban;

namespace KanbanGettingStarted
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            
            SfKanban kanban = new SfKanban();
            KanbanViewModel viewModel = new KanbanViewModel();
            
            kanban.AutoGenerateColumns = false;
            
            kanban.Columns.Add(new KanbanColumn
            {
                Title = "To Do",
                Categories = new List<object> { "Open" }
            });

            kanban.Columns.Add(new KanbanColumn
            {
                Title = "In Progress",
                Categories = new List<object> { "In Progress" }
            });

            kanban.Columns.Add(new KanbanColumn
            {
                Title = "Code Review",
                Categories = new List<object> { "Code Review" }
            });

            kanban.Columns.Add(new KanbanColumn
            {
                Title = "Done",
                Categories = new List<object> { "Done" }
            });

            kanban.ItemsSource = viewModel.Cards;
            this.Content = kanban;
        }
    }
}
```

**Key Points:**
- `AutoGenerateColumns="False"` - We manually define columns
- `ItemsSource` - Bound to the Cards collection
- `Categories` - Must match the `Category` property in KanbanModel
- Cards are automatically placed in columns based on their `Category` value

### Auto-Generating Columns

If you prefer to let the control generate columns automatically:

```xml
<kanban:SfKanban x:Name="kanban"
                 AutoGenerateColumns="True"
                 ItemsSource="{Binding Cards}">
    <kanban:SfKanban.BindingContext>
        <local:KanbanViewModel />
    </kanban:SfKanban.BindingContext>
</kanban:SfKanban>
```

**When to use:**
- Rapid prototyping
- Dynamic column creation based on data
- Simple workflows with no special column configuration

**When NOT to use:**
- Need custom column titles
- Want to control column order
- Need WIP limits or column-specific settings

## Running the Application

### Build and Run

**Visual Studio:**
- Press **F5** or click the **Run** button
- Select your target platform (Windows, Android, iOS, macOS)

**Visual Studio Code:**
- Press **F5** or run from the terminal:
  ```bash
  dotnet build
  dotnet run
  ```

**Rider:**
- Press **F5** or click the **Run** button
- Select your target platform

### What You Should See

Once the application runs, you should see:
- A kanban board with 4 columns (To Do, In Progress, Code Review, Done)
- Cards distributed across columns based on their Category
- Each card showing:
  - Title
  - Description
  - Color indicator
  - Tags

## Troubleshooting

### Issue: "SfKanban could not be found"

**Solution:**
1. Verify the NuGet package is installed
2. Ensure the namespace is imported correctly:
   ```xml
   xmlns:kanban="clr-namespace:Syncfusion.Maui.Kanban;assembly=Syncfusion.Maui.Kanban"
   ```
3. Clean and rebuild the solution

### Issue: "ConfigureSyncfusionCore not found"

**Solution:**
1. Ensure you've added the using statement:
   ```csharp
   using Syncfusion.Maui.Core.Hosting;
   ```
2. Verify Syncfusion.Maui.Core package is installed (should be automatic with Kanban package)
3. Restore NuGet packages: `dotnet restore`

### Issue: No cards appearing in kanban board

**Possible causes:**
1. **ItemsSource not bound** - Check the binding in XAML or code
2. **BindingContext not set** - Ensure ViewModel is set as BindingContext
3. **Category mismatch** - Verify card `Category` values match column `Categories`
4. **Empty data source** - Check that the Cards collection has items

**Debugging tips:**
```csharp
// Add to ViewModel constructor to verify data
public KanbanViewModel()
{
    Cards = new ObservableCollection<KanbanModel>();
    // ... populate cards
    System.Diagnostics.Debug.WriteLine($"Cards count: {Cards.Count}");
}
```

### Issue: Columns not appearing correctly

**Solution:**
1. If using `AutoGenerateColumns="False"`, ensure you've added columns to the `Columns` collection
2. Check that `Categories` property matches card `Category` values (case-sensitive)
3. Verify column definitions are inside `<kanban:SfKanban.Columns>` tag

### Issue: Build errors after installing NuGet package

**Solution (Rider specific):**
1. Open the terminal in Rider
2. Run manually:
   ```bash
   dotnet restore
   ```
3. Rebuild the project

## Next Steps

Now that you have a basic kanban board running:

1. **Customize cards** - See [cards.md](cards.md) for card templates and styling
2. **Configure columns** - See [columns.md](columns.md) for column sizing and WIP limits
3. **Add custom data** - See [data-binding.md](data-binding.md) for custom models
4. **Implement workflows** - See [workflows.md](workflows.md) for transition rules
5. **Handle events** - See [events.md](events.md) for drag-and-drop handling

## Sample Code Repository

For a complete working example, visit the official GitHub repository:
- **GitHub:** [SyncfusionExamples/GettingStarted_Kanban_MAUI](https://github.com/SyncfusionExamples/GettingStarted_Kanban_MAUI)

## API Reference

For detailed API documentation:
- **SfKanban:** [help.syncfusion.com/cr/maui/Syncfusion.Maui.Kanban.SfKanban.html](https://help.syncfusion.com/cr/maui/Syncfusion.Maui.Kanban.SfKanban.html)
- **KanbanModel:** [help.syncfusion.com/cr/maui/Syncfusion.Maui.Kanban.KanbanModel.html](https://help.syncfusion.com/cr/maui/Syncfusion.Maui.Kanban.KanbanModel.html)
- **KanbanColumn:** [help.syncfusion.com/cr/maui/Syncfusion.Maui.Kanban.KanbanColumn.html](https://help.syncfusion.com/cr/maui/Syncfusion.Maui.Kanban.KanbanColumn.html)