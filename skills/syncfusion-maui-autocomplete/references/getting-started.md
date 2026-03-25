# Getting Started with .NET MAUI Autocomplete

This guide walks through the setup and basic implementation of the Syncfusion .NET MAUI Autocomplete (SfAutocomplete) control.

## Installation

### Step 1: Install NuGet Package

Install the `Syncfusion.Maui.Inputs` package:

**Package Manager Console:**
```powershell
Install-Package Syncfusion.Maui.Inputs
```

**\.NET CLI:**
```bash
dotnet add package Syncfusion.Maui.Inputs
```

**Visual Studio NuGet Manager:**
1. Right-click project → Manage NuGet Packages
2. Search for `Syncfusion.Maui.Inputs`
3. Install latest version

### Step 2: Register Syncfusion Handler

In `MauiProgram.cs`, register the Syncfusion core handler:

```csharp
using Syncfusion.Maui.Core.Hosting;

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
            });

        return builder.Build();
    }
}
```

**Important:** The `Syncfusion.Maui.Core` package is a dependency and will be installed automatically. The `ConfigureSyncfusionCore()` call registers handlers for all Syncfusion controls.

## Basic Implementation

### Step 1: Add Namespace

Add the Syncfusion namespace to your XAML or C# file:

**XAML:**
```xml
xmlns:editors="clr-namespace:Syncfusion.Maui.Inputs;assembly=Syncfusion.Maui.Inputs"
```

**C#:**
```csharp
using Syncfusion.Maui.Inputs;
```

### Step 2: Create Basic Autocomplete

**XAML:**
```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:editors="clr-namespace:Syncfusion.Maui.Inputs;assembly=Syncfusion.Maui.Inputs">
    
    <editors:SfAutocomplete x:Name="autocomplete"
                            WidthRequest="250"
                            HeightRequest="50" />
</ContentPage>
```

**C#:**
```csharp
SfAutocomplete autocomplete = new SfAutocomplete
{
    WidthRequest = 250,
    HeightRequest = 50
};

Content = autocomplete;
```

## Data Binding

### Simple String Collection

**C#:**
```csharp
autocomplete.ItemsSource = new List<string>
{
    "Facebook",
    "Twitter",
    "Instagram",
    "LinkedIn",
    "YouTube"
};
```

**XAML with ViewModel:**
```xml
<ContentPage.BindingContext>
    <local:SocialMediaViewModel />
</ContentPage.BindingContext>

<editors:SfAutocomplete ItemsSource="{Binding SocialMediaList}" />
```

### Complex Object Binding

Create a model and ViewModel:

```csharp
// Model
public class SocialMedia
{
    public string Name { get; set; }
    public int ID { get; set; }
}

// ViewModel
public class SocialMediaViewModel
{
    public ObservableCollection<SocialMedia> SocialMedias { get; set; }
    
    public SocialMediaViewModel()
    {
        SocialMedias = new ObservableCollection<SocialMedia>
        {
            new SocialMedia { Name = "Facebook", ID = 0 },
            new SocialMedia { Name = "Google Plus", ID = 1 },
            new SocialMedia { Name = "Instagram", ID = 2 },
            new SocialMedia { Name = "LinkedIn", ID = 3 },
            new SocialMedia { Name = "Skype", ID = 4 },
            new SocialMedia { Name = "Telegram", ID = 5 },
            new SocialMedia { Name = "Twitter", ID = 10 },
            new SocialMedia { Name = "WhatsApp", ID = 12 },
            new SocialMedia { Name = "YouTube", ID = 13 }
        };
    }
}
```

Bind to Autocomplete:

```xml
<editors:SfAutocomplete ItemsSource="{Binding SocialMedias}"
                        DisplayMemberPath="Name"
                        TextMemberPath="Name" />
```

## DisplayMemberPath and TextMemberPath

When binding complex objects, specify which properties to use:

- **DisplayMemberPath**: Property displayed in the dropdown list
- **TextMemberPath**: Property used for searching and displayed in input box

```xml
<editors:SfAutocomplete ItemsSource="{Binding SocialMedias}"
                        DisplayMemberPath="Name"
                        TextMemberPath="Name" />
```

**Different Paths Example:**
```xml
<!-- Display Name in dropdown, but search by ID -->
<editors:SfAutocomplete ItemsSource="{Binding SocialMedias}"
                        DisplayMemberPath="Name"
                        TextMemberPath="ID" />
```

**Default Behavior:**
- If both are empty/null: Uses class name with namespace
- If TextMemberPath is empty: Searching uses DisplayMemberPath

**AOT Publishing Note:** When publishing in AOT mode on iOS, add `[Preserve(AllMembers = true)]` to your model class:

```csharp
[Preserve(AllMembers = true)]
public class SocialMedia
{
    public string Name { get; set; }
    public int ID { get; set; }
}
```

## Placeholder

Display hint text when the control is empty:

```xml
<editors:SfAutocomplete Placeholder="Select a social media"
                        PlaceholderColor="Gray"
                        ItemsSource="{Binding SocialMedias}"
                        DisplayMemberPath="Name" />
```

```csharp
autocomplete.Placeholder = "Select a social media";
autocomplete.PlaceholderColor = Colors.Gray;
```

## Text Property

The `Text` property retrieves the user-submitted text:

```csharp
// Get current text
string currentText = autocomplete.Text;

// Set text programmatically
autocomplete.Text = "Facebook";
```

Default value: `string.Empty`

## AutomationId Support

The SfAutocomplete provides AutomationId support for UI automation frameworks. The control's AutomationId is used to generate unique IDs for inner elements:

```xml
<editors:SfAutocomplete AutomationId="EmployeeAutocomplete" />
```

**Generated AutomationIds:**
- **Entry**: `EmployeeAutocomplete Entry`
- **Clear Button**: `EmployeeAutocomplete Clear Button`

This enables stable, predictable identifiers for automated UI testing.

## Complete Example

```xml
<!-- MainPage.xaml -->
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:editors="clr-namespace:Syncfusion.Maui.Inputs;assembly=Syncfusion.Maui.Inputs"
             xmlns:local="clr-namespace:YourNamespace">
    
    <ContentPage.BindingContext>
        <local:SocialMediaViewModel />
    </ContentPage.BindingContext>
    
    <StackLayout Padding="20">
        <editors:SfAutocomplete x:Name="autocomplete"
                                WidthRequest="300"
                                HeightRequest="50"
                                Placeholder="Select a social media"
                                PlaceholderColor="Gray"
                                ItemsSource="{Binding SocialMedias}"
                                DisplayMemberPath="Name"
                                TextMemberPath="Name"
                                AutomationId="SocialMediaAutocomplete" />
    </StackLayout>
</ContentPage>
```

```csharp
// MainPage.xaml.cs
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
    }
}
```

## Next Steps

Now that you have a basic autocomplete working:

1. **Configure searching** - See [searching-filtering.md](searching-filtering.md) for TextSearchMode and custom filters
2. **Handle selection** - See [selection.md](selection.md) for single/multi-selection
3. **Customize UI** - See [ui-customization.md](ui-customization.md) for styling and templates
4. **Add advanced features** - See [advanced-features.md](advanced-features.md) for headers, highlighting, etc.

## Common Issues

**Issue:** Control not appearing
- **Solution:** Ensure `ConfigureSyncfusionCore()` is called in MauiProgram.cs

**Issue:** Items not displaying
- **Solution:** Set DisplayMemberPath for complex objects

**Issue:** AOT build fails on iOS
- **Solution:** Add `[Preserve(AllMembers = true)]` to model classes
