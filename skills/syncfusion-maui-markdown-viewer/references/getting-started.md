# Getting Started with .NET MAUI MarkdownViewer

This guide walks you through the initial setup and basic usage of the SfMarkdownViewer control, from installation to displaying your first Markdown content in a .NET MAUI application.

## Step 1: Create a New .NET MAUI Project

### Visual Studio 2026

1. Go to **File > New > Project**
2. Search for and select **.NET MAUI App** template
3. Click **Next**
4. Enter your project name (e.g., "MarkdownViewerDemo")
5. Choose a location for your project
6. Click **Next**
7. Select the .NET framework version (9.0 or later)
8. Click **Create**

### Visual Studio Code

1. Open the command palette: `Ctrl+Shift+P` (Windows/Linux) or `Cmd+Shift+P` (Mac)
2. Type **.NET:New Project** and press **Enter**
3. Choose **.NET MAUI App** template
4. Select the project location
5. Type the project name and press **Enter**
6. Choose **Create project**

### JetBrains Rider

1. Go to **File > New Solution**
2. Select **.NET (C#)** from the left panel
3. Choose **.NET MAUI App** template
4. Enter the **Project Name** and **Solution Name**
5. Choose the **Location**
6. Select the .NET framework version (9.0 or later)
7. Click **Create**

### Command Line

```bash
dotnet new maui -n MarkdownViewerDemo
cd MarkdownViewerDemo
```

## Step 2: Install the NuGet Package

### Visual Studio 2026

1. In **Solution Explorer**, right-click your project
2. Select **Manage NuGet Packages**
3. Click the **Browse** tab
4. Search for `Syncfusion.Maui.MarkdownViewer`
5. Select the package from the results
6. Click **Install**
7. Accept any license agreements
8. Wait for the package and dependencies to install

### Visual Studio Code

1. Press `Ctrl+` (backtick) to open the integrated terminal
2. Navigate to your project directory (if not already there)
3. Run:
```bash
dotnet add package Syncfusion.Maui.MarkdownViewer
```
4. Restore dependencies:
```bash
dotnet restore
```

### JetBrains Rider

1. In **Solution Explorer**, right-click your project
2. Select **Manage NuGet Packages**
3. Search for `Syncfusion.Maui.MarkdownViewer`
4. Select the package and click **Install**
5. If restore doesn't happen automatically, open the terminal and run:
```bash
dotnet restore
```

### Command Line

```bash
dotnet add package Syncfusion.Maui.MarkdownViewer
dotnet restore
```

## Step 3: Register the Syncfusion Handler

The `Syncfusion.Maui.Core` package is a required dependency for all Syncfusion MAUI controls. You must register the Syncfusion handler in your `MauiProgram.cs` file.

### Update MauiProgram.cs

Open `MauiProgram.cs` and add the Syncfusion namespace and registration:

```csharp
using Microsoft.Extensions.Logging;
using Syncfusion.Maui.Core.Hosting;  // Add this namespace

namespace MarkdownViewerDemo
{
    public static class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            var builder = MauiApp.CreateBuilder();
            builder
                .UseMauiApp<App>()
                .ConfigureSyncfusionCore()  // Add this line
                .ConfigureFonts(fonts =>
                {
                    fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
                    fonts.AddFont("OpenSans-Semibold.ttf", "OpenSansSemibold");
                });

#if DEBUG
            builder.Logging.AddDebug();
#endif

            return builder.Build();
        }
    }
}
```

**Important:** The `ConfigureSyncfusionCore()` call must be added after `UseMauiApp<App>()` and before `ConfigureFonts()`.

## Step 4: Add MarkdownViewer to Your Page

### XAML Approach

Open `MainPage.xaml` and add the MarkdownViewer control:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:markdown="clr-namespace:Syncfusion.Maui.MarkdownViewer;assembly=Syncfusion.Maui.MarkdownViewer"
             x:Class="MarkdownViewerDemo.MainPage">

    <markdown:SfMarkdownViewer />

</ContentPage>
```

**Key points:**
- Add the `xmlns:markdown` namespace declaration
- Use `<markdown:SfMarkdownViewer />` to add the control

### C# Approach

If you prefer code-behind, update `MainPage.xaml.cs`:

```csharp
using Syncfusion.Maui.MarkdownViewer;

namespace MarkdownViewerDemo
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            
            SfMarkdownViewer markdownViewer = new SfMarkdownViewer();
            Content = markdownViewer;
        }
    }
}
```

## Step 5: Add Markdown Content

Now add some Markdown content to display in the viewer.

### XAML with Inline Content

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:markdown="clr-namespace:Syncfusion.Maui.MarkdownViewer;assembly=Syncfusion.Maui.MarkdownViewer"
             x:Class="MarkdownViewerDemo.MainPage">

    <markdown:SfMarkdownViewer>
        <markdown:SfMarkdownViewer.Source>
            <x:String>
                <![CDATA[
# Welcome to MarkdownViewer

The **MarkdownViewer** is a UI control for .NET MAUI that renders Markdown content with full formatting support.

## Key Features

- Standard Markdown syntax support
- Flexible input sources
- Customizable appearance
- Smooth scrolling
- Cross-platform compatibility

## Supported Elements

### Text Formatting
- **Bold text**
- *Italic text*
- ~~Strikethrough~~
- `Inline code`

### Lists

**Unordered List:**
- Item 1
- Item 2
- Item 3

**Ordered List:**
1. First item
2. Second item
3. Third item

### Tables

| Feature | Description |
|---------|-------------|
| Headings | H1 through H6 |
| Lists | Ordered and unordered |
| Tables | With headers and data |
| Links | External and internal |

### Code Blocks

```csharp
public class Example
{
    public string Name { get; set; }
}
```

---

**Note:** This is just a sample of what MarkdownViewer can render!
                ]]>
            </x:String>
        </markdown:SfMarkdownViewer.Source>
    </markdown:SfMarkdownViewer>

</ContentPage>
```

**Important:** Use `<![CDATA[...]]>` to wrap multiline Markdown content in XAML to avoid XML parsing issues.

### C# with String Content

```csharp
using Syncfusion.Maui.MarkdownViewer;

namespace MarkdownViewerDemo
{
    public partial class MainPage : ContentPage
    {
        private const string markdownContent = @"
# Welcome to MarkdownViewer

The **MarkdownViewer** is a UI control for .NET MAUI that renders Markdown content with full formatting support.

## Key Features

- Standard Markdown syntax support
- Flexible input sources
- Customizable appearance
- Smooth scrolling
- Cross-platform compatibility

## Sample Table

| Column 1 | Column 2 | Column 3 |
|----------|----------|----------|
| Data 1   | Data 2   | Data 3   |
| Data 4   | Data 5   | Data 6   |
";

        public MainPage()
        {
            InitializeComponent();
            
            SfMarkdownViewer markdownViewer = new SfMarkdownViewer();
            markdownViewer.Source = markdownContent;
            Content = markdownViewer;
        }
    }
}
```

**Tip:** Use verbatim string literals (`@"..."`) in C# to preserve line breaks and avoid escaping special characters.

## Step 6: Run Your Application

### Visual Studio 2026
1. Select your target platform from the debug dropdown (Android, iOS, Windows, macOS)
2. Click the **Run** button (F5) or select **Debug > Start Debugging**

### Visual Studio Code
1. Press `F5` or select **Run > Start Debugging**
2. Choose your target platform if prompted

### JetBrains Rider
1. Select your target platform from the run configurations
2. Click the **Run** button or press `Shift+F10`

### Command Line

```bash
# For Windows
dotnet build -t:Run -f net9.0-windows10.0.19041.0

# For Android (device/emulator must be running)
dotnet build -t:Run -f net9.0-android

# For iOS (Mac only, simulator must be available)
dotnet build -t:Run -f net9.0-ios
```

## Verification

When your app runs, you should see:
- Properly formatted headings at different sizes
- Bold and italic text rendered correctly
- Lists with proper indentation
- Tables with borders and formatting
- Clickable links (if any)
- Code blocks with monospace font
- Smooth scrolling when content is long

## Common First-Run Issues

### Handler Not Registered Error

**Error:** `Handler not found for control 'SfMarkdownViewer'`

**Solution:** Ensure you've added `.ConfigureSyncfusionCore()` in `MauiProgram.cs`. The call must be placed after `UseMauiApp<App>()`.

### NuGet Package Not Found

**Error:** `Package 'Syncfusion.Maui.MarkdownViewer' not found`

**Solution:** 
1. Check your internet connection
2. Clear NuGet cache: `dotnet nuget locals all --clear`
3. Restore packages: `dotnet restore`
4. Try installing again

### Namespace Not Found

**Error:** `The type or namespace name 'Syncfusion' could not be found`

**Solution:**
1. Verify the NuGet package is installed
2. Clean and rebuild your project
3. Restart your IDE
4. Check that the package reference exists in your `.csproj` file

### Content Not Displaying

**Issue:** MarkdownViewer renders but shows no content

**Solution:**
1. Verify `Source` property is set with valid Markdown content
2. Check for XAML syntax errors in the `<![CDATA[...]]>` block
3. Ensure the content string is not empty or null
4. For file sources, verify the file path is correct

## Next Steps

Now that you have a working MarkdownViewer:

- **Load content from different sources** → See [loading-content.md](loading-content.md)
- **Customize appearance** → See [appearance-customization.md](appearance-customization.md)
- **Apply custom CSS styling** → See [custom-css-styling.md](custom-css-styling.md)
- **Retrieve content programmatically** → See [content-retrieval.md](content-retrieval.md)
