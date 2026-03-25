# Getting Started with TreeView

This guide walks you through setting up and creating your first Syncfusion .NET MAUI TreeView (SfTreeView) control.

## Table of Contents
- [Step 1: Create a New Project](#step-1-create-a-new-project)
- [Step 2: Install NuGet Package](#step-2-install-nuget-package)
- [Step 3: Register Handler](#step-3-register-handler)
- [Step 4: Add Basic TreeView](#step-4-add-basic-treeview)
- [Step 5: Run the Application](#step-5-run-the-application)
- [Next Steps](#next-steps)

---

## Step 1: Create a New Project

### Using Visual Studio

1. Go to **File > New > Project**
2. Search for **.NET MAUI App** template
3. Enter the **Project Name** and **Location**
4. Select the **.NET framework version**
5. Click **Create**

---

## Step 2: Install NuGet Package

### Using NuGet Package Manager

1. In **Solution Explorer**, right-click the project
2. Choose **Manage NuGet Packages**
3. Search for **[Syncfusion.Maui.TreeView](https://www.nuget.org/packages/Syncfusion.Maui.TreeView/)**
4. Install the latest stable version
5. Ensure all dependencies are installed correctly

### Using Package Manager Console

```powershell
Install-Package Syncfusion.Maui.TreeView
```

### Using .NET CLI

```bash
dotnet add package Syncfusion.Maui.TreeView
```

### Verify Installation

After installation, ensure the project is restored:

```bash
dotnet restore
```

---

## Step 3: Register Handler

The [Syncfusion.Maui.Core](https://www.nuget.org/packages/Syncfusion.Maui.Core) package is a dependency for all Syncfusion MAUI controls. You must register the Syncfusion handler in your `MauiProgram.cs` file.

### Update MauiProgram.cs

```csharp
using Microsoft.Maui.Controls.Hosting;
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

            // Register Syncfusion handler
            builder.ConfigureSyncfusionCore();
            
            return builder.Build();
        }
    }
}
```

**Key Points:**
- Add `using Syncfusion.Maui.Core.Hosting;` at the top
- Call `builder.ConfigureSyncfusionCore();` before `builder.Build()`
- This registration is required for all Syncfusion MAUI controls

---

## Step 4: Add Basic TreeView

### Import Namespace

Add the TreeView namespace to your XAML or C# file.

**XAML:**

```xml
xmlns:syncfusion="clr-namespace:Syncfusion.Maui.TreeView;assembly=Syncfusion.Maui.TreeView"
```

**C#:**

```csharp
using Syncfusion.Maui.TreeView;
```

### Create Simple TreeView

#### XAML Approach

```xml
<ContentPage   
    xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    xmlns:syncfusion="clr-namespace:Syncfusion.Maui.TreeView;assembly=Syncfusion.Maui.TreeView"
    x:Class="GettingStarted.MainPage">

    <syncfusion:SfTreeView x:Name="treeView" />
</ContentPage>
```

#### C# Approach

```csharp
using Syncfusion.Maui.TreeView;

namespace GettingStarted
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            
            SfTreeView treeView = new SfTreeView();
            this.Content = treeView;
        }
    }
}
```

---

## Step 5: Run the Application

### Build and Run

Press **F5** or click the **Run** button to build and run the application.

### Expected Result

You should see an empty TreeView control displayed in your application. At this point, the TreeView is initialized but contains no data.

---

## Adding Sample Data

To see the TreeView in action, let's add some sample data using the unbound mode:

### XAML with Unbound Data

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:syncfusion="clr-namespace:Syncfusion.Maui.TreeView;assembly=Syncfusion.Maui.TreeView"
             xmlns:treeviewengine="clr-namespace:Syncfusion.TreeView.Engine;assembly=Syncfusion.Maui.TreeView"
             x:Class="GettingStarted.MainPage">
    
    <syncfusion:SfTreeView x:Name="treeView">
        <syncfusion:SfTreeView.Nodes>
            <treeviewengine:TreeViewNode Content="Australia">
                <treeviewengine:TreeViewNode.ChildNodes>
                    <treeviewengine:TreeViewNode Content="New South Wales">
                        <treeviewengine:TreeViewNode.ChildNodes>
                            <treeviewengine:TreeViewNode Content="Sydney"/>
                        </treeviewengine:TreeViewNode.ChildNodes>
                    </treeviewengine:TreeViewNode>
                </treeviewengine:TreeViewNode.ChildNodes>
            </treeviewengine:TreeViewNode>
            <treeviewengine:TreeViewNode Content="United States of America">
                <treeviewengine:TreeViewNode.ChildNodes>
                    <treeviewengine:TreeViewNode Content="New York"/>
                    <treeviewengine:TreeViewNode Content="California">
                        <treeviewengine:TreeViewNode.ChildNodes>
                            <treeviewengine:TreeViewNode Content="San Francisco"/>
                        </treeviewengine:TreeViewNode.ChildNodes>
                    </treeviewengine:TreeViewNode>
                </treeviewengine:TreeViewNode.ChildNodes>
            </treeviewengine:TreeViewNode>
        </syncfusion:SfTreeView.Nodes>
    </syncfusion:SfTreeView>
</ContentPage>
```

### C# with Unbound Data

```csharp
using Syncfusion.Maui.TreeView;
using Syncfusion.TreeView.Engine;

namespace GettingStarted
{
    public class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            
            SfTreeView treeView = new SfTreeView();

            var australia = new TreeViewNode() { Content = "Australia" };
            var nsw = new TreeViewNode() { Content = "New South Wales" };
            var sydney = new TreeViewNode() { Content = "Sydney" };
            australia.ChildNodes.Add(nsw);
            nsw.ChildNodes.Add(sydney);
 
            var usa = new TreeViewNode() { Content = "United States of America" };
            var newYork = new TreeViewNode() { Content = "New York" };
            var california = new TreeViewNode() { Content = "California" };
            var sanFrancisco = new TreeViewNode() { Content = "San Francisco" };
            usa.ChildNodes.Add(newYork);
            usa.ChildNodes.Add(california);
            california.ChildNodes.Add(sanFrancisco);
            
            treeView.Nodes.Add(australia);
            treeView.Nodes.Add(usa);

            this.Content = treeView;
        }
    }
}
```

---

## Common Issues and Solutions

### Issue 1: TreeView Not Displaying

**Solution:** Ensure you've called `builder.ConfigureSyncfusionCore()` in `MauiProgram.cs`.

### Issue 2: NuGet Package Not Restoring

**Solution:** Manually run `dotnet restore` in the terminal.

### Issue 3: Build Errors After Installation

**Solution:** Clean and rebuild the solution:
```bash
dotnet clean
dotnet build
```

### Issue 4: Missing Assembly References

**Solution:** Verify that both `Syncfusion.Maui.TreeView` and `Syncfusion.Maui.Core` packages are installed.

---

## Next Steps

Now that you have a basic TreeView running, explore these topics:

1. **Data Binding:** Learn how to populate TreeView with data from ViewModels → [data-binding.md](data-binding.md)
2. **Selection:** Implement single and multiple selection → [selection.md](selection.md)
3. **Templates:** Customize the appearance of tree items → [templating.md](templating.md)
4. **Expand/Collapse:** Control node expansion behavior → [expand-collapse.md](expand-collapse.md)

---

## Sample Projects

- [Unbound Mode Sample](https://github.com/SyncfusionExamples/populate-the-nodes-in-unbound-mode-in-.net-maui-treeview)
- [Bound Mode Sample](https://github.com/SyncfusionExamples/data-binding-in-.net-maui-treeview)

---

**Key Takeaways:**
- Always register Syncfusion handler in `MauiProgram.cs`
- TreeView supports both bound (data binding) and unbound (manual nodes) modes
- Start with simple examples and gradually add complexity
- Refer to [data-binding.md](data-binding.md) for production-ready data population patterns
