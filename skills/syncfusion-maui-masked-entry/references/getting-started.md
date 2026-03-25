# Getting Started with .NET MAUI Masked Entry

This guide covers the installation, setup, and basic implementation of the Syncfusion .NET MAUI Masked Entry (SfMaskedEntry) control.

## Installation

### Step 1: Install the Syncfusion MAUI Inputs NuGet Package

**Via Visual Studio:**
1. Right-click your project in Solution Explorer → **Manage NuGet Packages**
2. Search for `Syncfusion.Maui.Inputs`
3. Install the latest stable version

**Via .NET CLI:**
```bash
dotnet add package Syncfusion.Maui.Inputs
```

**Via Package Manager Console:**
```powershell
Install-Package Syncfusion.Maui.Inputs
```

The package includes:
- SfMaskedEntry control
- All necessary dependencies (including Syncfusion.Maui.Core)

### Step 2: Register the Syncfusion Handler

In your `MauiProgram.cs` file, register the Syncfusion Core handler:

```csharp
using Microsoft.Maui;
using Microsoft.Maui.Hosting;
using Microsoft.Maui.Controls.Hosting;
using Syncfusion.Maui.Core.Hosting;

namespace YourApp
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

            return builder.Build();
        }
    }
}
```

**Important:** The `ConfigureSyncfusionCore()` method is required for all Syncfusion .NET MAUI controls.

## Basic Implementation

### Step 3: Add the Namespace

**In XAML:**
```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:editors="clr-namespace:Syncfusion.Maui.Inputs;assembly=Syncfusion.Maui.Inputs"
             x:Class="YourApp.MainPage">
    
    <!-- Your content here -->
    
</ContentPage>
```

**In C#:**
```csharp
using Syncfusion.Maui.Inputs;
```

### Step 4: Create Your First Masked Entry

**Basic Masked Entry (XAML):**
```xml
<editors:SfMaskedEntry x:Name="maskedEntry" />
```

**Basic Masked Entry (C#):**
```csharp
SfMaskedEntry maskedEntry = new SfMaskedEntry();
```

This creates a basic masked entry control without any mask pattern.

## Setting a Mask Pattern

### Simple Mask Example

Use the `Mask` property to define the input pattern:

```xml
<editors:SfMaskedEntry 
    WidthRequest="200"
    MaskType="Simple"
    Mask="00/00/0000"
    ClearButtonVisibility="WhileEditing" />
```

```csharp
SfMaskedEntry maskedEntry = new SfMaskedEntry
{
    WidthRequest = 200,
    MaskType = MaskedEntryMaskType.Simple,
    Mask = "00/00/0000",
    ClearButtonVisibility = ClearButtonVisibility.WhileEditing
};
```

**Result:** Accepts date input in MM/DD/YYYY format with automatic slash separators.

### RegEx Mask Example

For more flexible patterns, use RegEx mask type:

```xml
<editors:SfMaskedEntry 
    WidthRequest="200"
    MaskType="RegEx"
    Mask="[A-Za-z0-9._%-]+@[A-Za-z0-9]+\.[A-Za-z]{2,3}"
    ClearButtonVisibility="WhileEditing" />
```

```csharp
SfMaskedEntry maskedEntry = new SfMaskedEntry
{
    WidthRequest = 200,
    MaskType = MaskedEntryMaskType.RegEx,
    Mask = "[A-Za-z0-9._%-]+@[A-Za-z0-9]+\\.[A-Za-z]{2,3}",
    ClearButtonVisibility = ClearButtonVisibility.WhileEditing
};
```

**Result:** Validates email format as user types.

## Customizing the Prompt Character

The `PromptChar` defines what character appears in unfilled positions (default is underscore `_`):

```xml
<editors:SfMaskedEntry 
    WidthRequest="200"
    MaskType="Simple"
    Mask="00/00/0000"
    PromptChar="#" />
```

```csharp
SfMaskedEntry maskedEntry = new SfMaskedEntry
{
    WidthRequest = 200,
    MaskType = MaskedEntryMaskType.Simple,
    Mask = "00/00/0000",
    PromptChar = '#'
};
```

**Display:** `##/##/####` (instead of `__/__/____`)

**Common Prompt Characters:**
- `_` (underscore) - Default, widely recognized
- `#` (hash) - Alternative visual indicator
- `*` (asterisk) - For password-like fields
- ` ` (space) - Minimalist appearance

## Setting and Retrieving the Value

### Setting Initial Value

Use the `Value` property to pre-populate the masked entry:

```xml
<editors:SfMaskedEntry 
    WidthRequest="200"
    MaskType="Simple"
    Mask="00/00/0000"
    Value="12022024" />
```

```csharp
SfMaskedEntry maskedEntry = new SfMaskedEntry
{
    WidthRequest = 200,
    MaskType = MaskedEntryMaskType.Simple,
    Mask = "00/00/0000",
    Value = "12022024"
};
```

**Display:** `12/02/2024` (automatically formatted)

### Retrieving the Value

```csharp
// Get the current value
string currentValue = maskedEntry.Value?.ToString();

// Example: "12/02/2024" or formatted based on ValueMaskFormat
```

**Note:** The format of the returned value depends on the `ValueMaskFormat` property (covered in value-formatting.md).

## Complete Working Example

Here's a complete phone number input implementation:

**MainPage.xaml:**
```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:editors="clr-namespace:Syncfusion.Maui.Inputs;assembly=Syncfusion.Maui.Inputs"
             x:Class="MaskedEntryDemo.MainPage"
             Padding="20">

    <VerticalStackLayout Spacing="10">
        
        <Label 
            Text="Enter Phone Number" 
            FontSize="16"
            FontAttributes="Bold" />
        
        <editors:SfMaskedEntry 
            x:Name="phoneEntry"
            WidthRequest="250"
            MaskType="Simple"
            Mask="(000) 000-0000"
            Placeholder="(555) 123-4567"
            ClearButtonVisibility="WhileEditing"
            ValueChanged="OnPhoneValueChanged" />
        
        <Label 
            x:Name="resultLabel"
            Text="Value: "
            FontSize="14" />
            
    </VerticalStackLayout>
    
</ContentPage>
```

**MainPage.xaml.cs:**
```csharp
using Syncfusion.Maui.Inputs;

namespace MaskedEntryDemo
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
        }

        private void OnPhoneValueChanged(object sender, MaskedEntryValueChangedEventArgs e)
        {
            resultLabel.Text = $"Value: {e.NewValue}";
            
            if (e.IsMaskCompleted)
            {
                // Mask is fully completed
                resultLabel.TextColor = Colors.Green;
            }
            else
            {
                resultLabel.TextColor = Colors.Gray;
            }
        }
    }
}
```

## Common Initial Setup Patterns

### 1. Phone Number

```csharp
var phoneEntry = new SfMaskedEntry
{
    MaskType = MaskedEntryMaskType.Simple,
    Mask = "(000) 000-0000",
    Placeholder = "Enter phone number"
};
```

### 2. Date

```csharp
var dateEntry = new SfMaskedEntry
{
    MaskType = MaskedEntryMaskType.Simple,
    Mask = "00/00/0000",
    Placeholder = "MM/DD/YYYY",
    PromptChar = '_'
};
```

### 3. SSN

```csharp
var ssnEntry = new SfMaskedEntry
{
    MaskType = MaskedEntryMaskType.Simple,
    Mask = "000-00-0000",
    PasswordChar = '●',
    Placeholder = "123-45-6789"
};
```

### 4. Credit Card

```csharp
var cardEntry = new SfMaskedEntry
{
    MaskType = MaskedEntryMaskType.Simple,
    Mask = "0000-0000-0000-0000",
    Placeholder = "1234-5678-9012-3456"
};
```

## Troubleshooting

### Handler Not Registered Error
**Problem:** Exception about Syncfusion handler not registered  
**Solution:** Ensure `ConfigureSyncfusionCore()` is called in MauiProgram.cs

### Mask Not Applying
**Problem:** Typed characters don't match mask pattern  
**Solution:** Verify `MaskType` matches your mask pattern (Simple vs RegEx)

### Value Not Displaying
**Problem:** Set value doesn't appear in control  
**Solution:** Ensure value format matches mask pattern (e.g., "5551234567" for "(000) 000-0000")

### NuGet Package Issues
**Problem:** Package restore or version conflicts  
**Solution:** 
- Clear NuGet cache: `dotnet nuget locals all --clear`
- Ensure all Syncfusion packages use same version
- Update to latest stable version

## Next Steps

Now that you have a basic Masked Entry working:

1. **Learn Mask Types:** Explore Simple and RegEx mask elements → [mask-types.md](mask-types.md)
2. **Format Values:** Control how values are stored and displayed → [value-formatting.md](value-formatting.md)
3. **Add Validation:** Implement validation and handle events → [validation-and-events.md](validation-and-events.md)
4. **Customize Appearance:** Style your masked entry → [customization.md](customization.md)
5. **Advanced Features:** Culture support, passwords, and more → [advanced-features.md](advanced-features.md)

## Additional Resources

- [GitHub Samples](https://github.com/SyncfusionExamples/maui-maskedentry-samples)
- [API Documentation](https://help.syncfusion.com/cr/maui/Syncfusion.Maui.Inputs.SfMaskedEntry.html)
- [Video Tutorial](https://www.youtube.com/watch?v=yTbh1Jo95Vw)
