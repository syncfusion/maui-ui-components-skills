# Getting Started with Linear ProgressBar

This guide covers the complete setup process for the Syncfusion .NET MAUI Linear ProgressBar control, including installation, configuration, and creating your first progress bar.

## Installation Steps

### Step 1: Create a New .NET MAUI Project

#### Visual Studio

1. Go to **File > New > Project**
2. Choose the **.NET MAUI App** template
3. Name the project and choose a location
4. Click **Next**
5. Select the .NET framework version
6. Click **Create**

#### Visual Studio Code

1. Open the command palette: `Ctrl+Shift+P`
2. Type **.NET:New Project** and press Enter
3. Choose the **.NET MAUI App** template
4. Select the project location
5. Type the project name and press **Enter**
6. Choose **Create project**

#### JetBrains Rider

1. Go to **File > New Solution**
2. Select **.NET (C#)**
3. Choose the **.NET MAUI App** template
4. Enter the Project Name, Solution Name, and Location
5. Select the .NET framework version
6. Click **Create**

### Step 2: Install the Syncfusion NuGet Package

The Linear ProgressBar is part of the `Syncfusion.Maui.ProgressBar` package.

#### Visual Studio / JetBrains Rider

1. In **Solution Explorer**, right-click the project
2. Choose **Manage NuGet Packages**
3. Search for **Syncfusion.Maui.ProgressBar**
4. Install the latest version
5. Ensure all dependencies are installed correctly

#### Visual Studio Code

1. Press `Ctrl + `` (backtick) to open the integrated terminal
2. Ensure you're in the project root directory (where .csproj is located)
3. Run the command:
   ```bash
   dotnet add package Syncfusion.Maui.ProgressBar
   ```
4. Restore dependencies:
   ```bash
   dotnet restore
   ```

### Step 3: Register the Syncfusion Core Handler

**CRITICAL**: This step is required for all Syncfusion .NET MAUI controls.

The `Syncfusion.Maui.Core` NuGet is a dependent package that's automatically installed. You must register its handler in **MauiProgram.cs**:

```csharp
using Syncfusion.Maui.Core.Hosting;

namespace YourProjectName
{
    public static class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            var builder = MauiApp.CreateBuilder();

            // Register Syncfusion Core Handler - REQUIRED!
            builder.ConfigureSyncfusionCore();
            
            builder
                .UseMauiApp<App>()
                .ConfigureFonts(fonts =>
                {
                    fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
                    fonts.AddFont("OpenSans-Semibold.ttf", "OpenSansSemibold");
                });

            return builder.Build();
        }
    }
}
```

**Common Mistake**: Forgetting to call `ConfigureSyncfusionCore()` will cause runtime errors when using the control.

### Step 4: Add the Linear ProgressBar Control

#### Import the Namespace

**In XAML:**

Add the namespace declaration to your page:

```xaml
xmlns:progressBar="clr-namespace:Syncfusion.Maui.ProgressBar;assembly=Syncfusion.Maui.ProgressBar"
```

**In C#:**

Add the using statement:

```csharp
using Syncfusion.Maui.ProgressBar;
```

#### Create Your First Progress Bar

**XAML Example:**

```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:progressBar="clr-namespace:Syncfusion.Maui.ProgressBar;assembly=Syncfusion.Maui.ProgressBar"
             x:Class="YourNamespace.MainPage">
    
    <StackLayout Padding="20" Spacing="20">
        <!-- Basic progress bar at 75% -->
        <progressBar:SfLinearProgressBar Progress="75"/>
    </StackLayout>
    
</ContentPage>
```

**C# Example:**

```csharp
using Syncfusion.Maui.ProgressBar;

public class MainPage : ContentPage
{
    public MainPage()
    {
        var stackLayout = new StackLayout
        {
            Padding = new Thickness(20),
            Spacing = 20
        };

        // Create progress bar at 75%
        var linearProgressBar = new SfLinearProgressBar 
        { 
            Progress = 75 
        };
        
        stackLayout.Children.Add(linearProgressBar);
        this.Content = stackLayout;
    }
}
```

## Understanding Progress Values

By default, progress values should be specified between **0 and 100**:
- `Progress = 0` → Empty (0%)
- `Progress = 50` → Half filled (50%)
- `Progress = 100` → Completely filled (100%)

To use a different range (e.g., 0 to 1 for factor values):

```csharp
var progressBar = new SfLinearProgressBar
{
    Minimum = 0,
    Maximum = 1,
    Progress = 0.75  // 75%
};
```

## Basic Configuration Examples

### Example 1: Simple Progress Bar

```xaml
<progressBar:SfLinearProgressBar Progress="50" />
```

### Example 2: Indeterminate Loading Indicator

When you don't know how long a task will take:

```xaml
<progressBar:SfLinearProgressBar IsIndeterminate="True"/>
```

### Example 3: Segmented Progress

For multi-step processes:

```xaml
<progressBar:SfLinearProgressBar SegmentCount="4" Progress="75"/>
```

### Example 4: Custom Colors

```xaml
<progressBar:SfLinearProgressBar Progress="75" 
                                 TrackFill="#33ffbe06" 
                                 ProgressFill="#FFffbe06"/>
```

## Running Your First Progress Bar

1. **Build** the project to ensure all packages are restored
2. **Run** the application on your target platform
3. You should see a horizontal progress bar at 75% completion

### Expected Output

You'll see a rectangular progress bar with:
- A light gray track (background)
- A colored progress indicator filled to 75%
- Smooth animation as it renders

## Quick Verification Checklist

Before proceeding, verify:

- ✅ .NET 9 SDK or later is installed
- ✅ Project created successfully
- ✅ `Syncfusion.Maui.ProgressBar` NuGet package installed
- ✅ `ConfigureSyncfusionCore()` called in MauiProgram.cs
- ✅ Namespace imported correctly
- ✅ Progress bar displays when app runs

## Next Steps

Now that you have a basic progress bar working:

1. **Explore States** - Learn about determinate, indeterminate, and buffer states
2. **Customize Appearance** - Change colors, sizes, and corner radius
3. **Add Animations** - Configure smooth transitions with easing effects
4. **Handle Events** - Respond to progress changes and completion
5. **Implement Segments** - Show multi-step progress visualization

## Common Issues

**Issue**: "The type or namespace name 'Syncfusion' could not be found"
- **Solution**: Ensure NuGet package is installed and project is restored

**Issue**: Progress bar doesn't appear
- **Solution**: Verify `ConfigureSyncfusionCore()` is called in MauiProgram.cs

**Issue**: Runtime error when creating progress bar
- **Solution**: Check that the handler registration is done before `UseMauiApp<App>()`

**Issue**: Progress bar shows but doesn't render correctly
- **Solution**: Ensure you're using .NET MAUI-compatible project template, not Xamarin.Forms

## Additional Resources

- **Official Docs**: [Getting Started Guide](https://help.syncfusion.com/maui/linearprogressbar/getting-started)
- **GitHub Samples**: [Complete Examples](https://github.com/SyncfusionExamples/Getting-started-with-.NET-MAUI-Linear-ProgressBar-control-)
- **Video Tutorial**: Available on the official documentation page