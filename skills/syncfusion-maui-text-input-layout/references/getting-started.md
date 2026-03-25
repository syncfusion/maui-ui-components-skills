# Getting Started with .NET MAUI Text Input Layout

This guide walks you through setting up and implementing the Syncfusion .NET MAUI Text Input Layout (SfTextInputLayout) control from scratch.

## Installation Steps

### Step 1: Create a New .NET MAUI Project

#### Using Visual Studio 2026

1. Open Visual Studio 2026
2. Click **File → New → Project**
3. Search for and select **.NET MAUI App** template
4. Click **Next**
5. Enter a **Project name** and choose a **Location**
6. Click **Next**
7. Select **.NET 9** or later as the framework
8. Click **Create**

#### Using Visual Studio Code

1. Open the Command Palette: **Ctrl+Shift+P** (Windows/Linux) or **Cmd+Shift+P** (Mac)
2. Type and select: **.NET: New Project**
3. Choose **.NET MAUI App** template
4. Select project location and enter project name
5. Click **Create project**

#### Using CLI

```bash
dotnet new maui -n MyMauiApp
cd MyMauiApp
```

### Step 2: Install Syncfusion.Maui.Core NuGet Package

The Text Input Layout control is part of the **Syncfusion.Maui.Core** package.

#### Using Visual Studio

1. Right-click your project in **Solution Explorer**
2. Select **Manage NuGet Packages**
3. Click the **Browse** tab
4. Search for `Syncfusion.Maui.Core`
5. Select the package and click **Install**
6. Accept the license agreement

#### Using Visual Studio Code

1. Press **Ctrl+`** (backtick) to open the integrated terminal
2. Run:
   ```bash
   dotnet add package Syncfusion.Maui.Core
   ```
3. Verify installation:
   ```bash
   dotnet restore
   ```

#### Using CLI

```bash
dotnet add package Syncfusion.Maui.Core
dotnet restore
```

**Note:** `Syncfusion.Maui.Core` is a dependency package required for all Syncfusion .NET MAUI controls.

### Step 3: Register Syncfusion Handler

Open **MauiProgram.cs** and register the Syncfusion Core handler.

```csharp
using Microsoft.Extensions.Logging;
using Syncfusion.Maui.Core.Hosting;  // Add this namespace

namespace MyMauiApp
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

**Important:** Always call `.ConfigureSyncfusionCore()` before building the app.

### Step 4: Add Namespace

Add the Syncfusion namespace to your XAML or C# files.

#### XAML Namespace

Open your **MainPage.xaml** and add the namespace:

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:inputLayout="clr-namespace:Syncfusion.Maui.Core;assembly=Syncfusion.Maui.Core"
             x:Class="MyMauiApp.MainPage">
    
    <!-- Your content here -->
    
</ContentPage>
```

#### C# Using Statement

For code-behind or C# pages:

```csharp
using Syncfusion.Maui.Core;
```

## Basic Implementation

### Example 1: Simple Text Input with Hint

The most basic implementation adds a floating hint label to an Entry control.

#### XAML

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:inputLayout="clr-namespace:Syncfusion.Maui.Core;assembly=Syncfusion.Maui.Core"
             x:Class="MyMauiApp.MainPage">
    
    <VerticalStackLayout Padding="20" Spacing="20">
        
        <inputLayout:SfTextInputLayout Hint="Name">
            <Entry />
        </inputLayout:SfTextInputLayout>
        
    </VerticalStackLayout>
    
</ContentPage>
```

#### C#

```csharp
using Syncfusion.Maui.Core;

public class MainPage : ContentPage
{
    public MainPage()
    {
        var inputLayout = new SfTextInputLayout
        {
            Hint = "Name",
            Content = new Entry()
        };

        Content = new VerticalStackLayout
        {
            Padding = 20,
            Spacing = 20,
            Children = { inputLayout }
        };
    }
}
```

**Behavior:**
- When the Entry is unfocused and empty, the hint displays inside the input
- When focused, the hint animates to the top position
- When unfocused with text, the hint remains at the top

### Example 2: Adding Helper Text

Helper text provides additional guidance below the input field.

#### XAML

```xml
<inputLayout:SfTextInputLayout Hint="Email"
                               HelperText="We'll never share your email">
    <Entry Keyboard="Email" />
</inputLayout:SfTextInputLayout>
```

#### C#

```csharp
var emailInput = new SfTextInputLayout
{
    Hint = "Email",
    HelperText = "We'll never share your email",
    Content = new Entry { Keyboard = Keyboard.Email }
};
```

### Example 3: Password Field with Visibility Toggle

Enable password visibility toggling with a single property.

#### XAML

```xml
<inputLayout:SfTextInputLayout Hint="Password"
                               EnablePasswordVisibilityToggle="True">
    <Entry IsPassword="True" />
</inputLayout:SfTextInputLayout>
```

#### C#

```csharp
var passwordInput = new SfTextInputLayout
{
    Hint = "Password",
    EnablePasswordVisibilityToggle = true,
    Content = new Entry { IsPassword = true }
};
```

**Note:** Password visibility toggle only works with `Entry` controls, not `Editor`.

### Example 4: Multi-Line Input with Editor

Use `Editor` for multi-line text input.

#### XAML

```xml
<inputLayout:SfTextInputLayout Hint="Comments"
                               HelperText="Share your thoughts">
    <Editor AutoSize="TextChanges" />
</inputLayout:SfTextInputLayout>
```

#### C#

```csharp
var commentsInput = new SfTextInputLayout
{
    Hint = "Comments",
    HelperText = "Share your thoughts",
    Content = new Editor { AutoSize = EditorAutoSizeOption.TextChanges }
};
```

**Important:** Always set `AutoSize="TextChanges"` on Editor to ensure proper height adjustment.

## Complete Working Example

Here's a complete login form example:

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:inputLayout="clr-namespace:Syncfusion.Maui.Core;assembly=Syncfusion.Maui.Core"
             x:Class="MyMauiApp.LoginPage"
             Title="Login">
    
    <ScrollView>
        <VerticalStackLayout Padding="30" Spacing="25" VerticalOptions="Center">
            
            <Label Text="Welcome Back" 
                   FontSize="28" 
                   FontAttributes="Bold"
                   HorizontalOptions="Center" />
            
            <inputLayout:SfTextInputLayout Hint="Email"
                                           HelperText="Enter your email address">
                <Entry Keyboard="Email" 
                       Placeholder="you@example.com" />
            </inputLayout:SfTextInputLayout>
            
            <inputLayout:SfTextInputLayout Hint="Password"
                                           EnablePasswordVisibilityToggle="True"
                                           HelperText="At least 8 characters">
                <Entry IsPassword="True" />
            </inputLayout:SfTextInputLayout>
            
            <Button Text="Sign In" 
                    BackgroundColor="#6750A4" 
                    TextColor="White"
                    HeightRequest="50"
                    CornerRadius="8" />
            
        </VerticalStackLayout>
    </ScrollView>
    
</ContentPage>
```

## Run Your Application

### Visual Studio

1. Select your target platform (Android, iOS, Windows, macOS) from the toolbar
2. Press **F5** or click the **Play** button to run

### Visual Studio Code

1. Open Command Palette: **Ctrl+Shift+P**
2. Select **.NET MAUI: Debug**
3. Choose target platform

### CLI

```bash
# Android
dotnet build -t:Run -f net9.0-android

# iOS
dotnet build -t:Run -f net9.0-ios

# Windows
dotnet build -t:Run -f net9.0-windows10.0.19041.0

# macOS
dotnet build -t:Run -f net9.0-maccatalyst
```

## Verifying Installation

You should see:
1. A text input field with a hint label
2. When you tap/click the input, the hint animates to the top
3. For password fields, an eye icon appears to toggle visibility

## Common Issues and Solutions

### Issue: "The type or namespace 'Syncfusion' could not be found"

**Solution:**
1. Ensure `Syncfusion.Maui.Core` NuGet package is installed
2. Run `dotnet restore`
3. Clean and rebuild the project

### Issue: Control doesn't appear

**Solution:**
1. Verify `.ConfigureSyncfusionCore()` is called in `MauiProgram.cs`
2. Ensure namespace is correctly added to XAML/C#
3. Check that you're setting the `Content` property of SfTextInputLayout

### Issue: Hint label doesn't animate

**Solution:**
1. Ensure `ShowHint` property is not set to `false`
2. Check that input view (Entry/Editor) is properly set as Content
3. Verify the control has sufficient space to display

### Issue: Password toggle doesn't appear

**Solution:**
1. Confirm `EnablePasswordVisibilityToggle="True"`
2. Ensure you're using `Entry` (not `Editor`)
3. Set `IsPassword="True"` on the Entry

## Next Steps

Now that you have a basic Text Input Layout working, explore:

- **Container Types** — Learn about Filled, Outlined, and None styles
- **Assistive Labels** — Add error messages, character counters
- **Custom Icons** — Add leading and trailing icons
- **Visual States** — Customize colors for Normal, Focused, and Error states
- **Advanced Features** — RTL support, custom fonts, events

Refer to other reference files for detailed guidance on each topic.
