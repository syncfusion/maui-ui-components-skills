# Getting Started with .NET MAUI Numeric Entry

This guide covers the complete setup and basic implementation of the Syncfusion .NET MAUI Numeric Entry control (SfNumericEntry).

## Step 1: Create a New .NET MAUI Project

### Using Visual Studio 2026

1. Open Visual Studio 2026
2. Click **File > New > Project**
3. Search for **.NET MAUI App** template
4. Select **.NET MAUI App** and click **Next**
5. Enter project name (e.g., "NumericEntryApp")
6. Choose location and click **Next**
7. Select .NET framework version (.NET 9.0 or later)
8. Click **Create**

### Using Visual Studio Code

1. Open Command Palette: **Ctrl+Shift+P** (Windows/Linux) or **Cmd+Shift+P** (Mac)
2. Type **.NET:New Project** and press Enter
3. Select **.NET MAUI App** template
4. Choose project location
5. Enter project name (e.g., "NumericEntryApp")
6. Click **Create project**

### Using .NET CLI

```bash
dotnet new maui -n NumericEntryApp
cd NumericEntryApp
```

## Step 2: Install Syncfusion.Maui.Inputs NuGet Package

### Using Visual Studio 2026

1. Right-click the project in **Solution Explorer**
2. Select **Manage NuGet Packages**
3. Click the **Browse** tab
4. Search for **Syncfusion.Maui.Inputs**
5. Select the package and click **Install**
6. Accept the license agreement
7. Wait for installation to complete

### Using Visual Studio Code

1. Press **Ctrl+`** to open integrated terminal
2. Navigate to project root directory
3. Run the command:

```bash
dotnet add package Syncfusion.Maui.Inputs
```

4. Restore dependencies:

```bash
dotnet restore
```

### Using Package Manager Console

```powershell
Install-Package Syncfusion.Maui.Inputs
```

## Step 3: Register Syncfusion Handler

**CRITICAL STEP:** You must register the Syncfusion handler in `MauiProgram.cs` before using any Syncfusion controls.

### Open MauiProgram.cs

Located in the project root directory.

### Add Using Statement

Add this at the top of the file:

```csharp
using Syncfusion.Maui.Core.Hosting;
```

### Call ConfigureSyncfusionCore()

Modify the `CreateMauiApp()` method:

```csharp
using Microsoft.Maui;
using Microsoft.Maui.Hosting;
using Microsoft.Maui.Controls.Hosting;
using Syncfusion.Maui.Core.Hosting;

namespace NumericEntryApp
{
    public static class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            var builder = MauiApp.CreateBuilder();
            builder
                .UseMauiApp<App>()
                .ConfigureSyncfusionCore()  // ← REQUIRED: Register handler
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

**Without this step, the Numeric Entry control will not render properly.**

## Step 4: Add Namespace

### XAML Namespace

Add the Syncfusion namespace to your XAML page:

```xml
xmlns:editors="clr-namespace:Syncfusion.Maui.Inputs;assembly=Syncfusion.Maui.Inputs"
```

**Complete XAML example:**

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:editors="clr-namespace:Syncfusion.Maui.Inputs;assembly=Syncfusion.Maui.Inputs"
             x:Class="NumericEntryApp.MainPage">
    
    <!-- Your content here -->
    
</ContentPage>
```

### C# Using Statement

Add the using directive at the top of your C# file:

```csharp
using Syncfusion.Maui.Inputs;
```

## Step 5: Add Basic Numeric Entry

### XAML Implementation

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:editors="clr-namespace:Syncfusion.Maui.Inputs;assembly=Syncfusion.Maui.Inputs"
             x:Class="NumericEntryApp.MainPage">
    
    <VerticalStackLayout Padding="20" Spacing="10">
        <Label Text="Enter Amount:" FontSize="16" />
        
        <editors:SfNumericEntry x:Name="numericEntry"
                                WidthRequest="200"
                                HeightRequest="40" />
    </VerticalStackLayout>
    
</ContentPage>
```

### C# Implementation

```csharp
using Syncfusion.Maui.Inputs;

namespace NumericEntryApp;

public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        
        // Create Numeric Entry programmatically
        var numericEntry = new SfNumericEntry
        {
            WidthRequest = 200,
            HeightRequest = 40
        };
        
        // Add to layout
        var layout = new VerticalStackLayout
        {
            Padding = 20,
            Spacing = 10
        };
        
        layout.Children.Add(new Label 
        { 
            Text = "Enter Amount:", 
            FontSize = 16 
        });
        layout.Children.Add(numericEntry);
        
        Content = layout;
    }
}
```

## Editing Values

The Numeric Entry control allows numeric input and restricts alphabetic characters. The value is validated and updated when:

1. **Enter key is pressed**
2. **Control loses focus** (user clicks outside or tabs away)

### Basic Value Entry

```xml
<editors:SfNumericEntry WidthRequest="200"
                        HorizontalOptions="Center"
                        VerticalOptions="Center" />
```

**Behavior:**
- User can type numeric digits (0-9)
- Decimal point and minus sign allowed
- Alphabetic characters automatically blocked
- Value updates on Enter or focus loss

### Set Initial Value

```xml
<editors:SfNumericEntry WidthRequest="200"
                        Value="1234.56" />
```

```csharp
var numericEntry = new SfNumericEntry
{
    WidthRequest = 200,
    Value = 1234.56
};
```

## Change Number Format

Use the `CustomFormat` property to display values in different formats. Default value is `null` (no specific format).

### Currency Format

```xml
<editors:SfNumericEntry WidthRequest="200"
                        Value="12.5"
                        CustomFormat="C2" />
```

```csharp
var numericEntry = new SfNumericEntry
{
    WidthRequest = 200,
    Value = 12.5,
    CustomFormat = "C2"  // Displays as "$12.50" (US culture)
};
```

**Result:** $12.50

### Percentage Format

```xml
<editors:SfNumericEntry WidthRequest="200"
                        Value="0.75"
                        CustomFormat="P0" />
```

**Result:** 75%

### Decimal Format with Thousands Separator

```xml
<editors:SfNumericEntry WidthRequest="200"
                        Value="123456.789"
                        CustomFormat="N2" />
```

**Result:** 123,456.79

### Custom Format with Precision

```xml
<editors:SfNumericEntry WidthRequest="200"
                        Value="12.5"
                        CustomFormat="0.000" />
```

**Result:** 12.500

## Accept Null Values

By default, Numeric Entry allows **null** values. When the user clears the input, the Value property becomes `null`.

### Allow Null (Default Behavior)

```xml
<editors:SfNumericEntry WidthRequest="200"
                        AllowNull="True"
                        Placeholder="Enter value" />
```

**Behavior:**
- Clear button sets value to null
- Placeholder text appears when null
- Value property is nullable (double?)

### Disallow Null

```xml
<editors:SfNumericEntry WidthRequest="200"
                        Value="100"
                        AllowNull="False" />
```

```csharp
var numericEntry = new SfNumericEntry
{
    WidthRequest = 200,
    Value = 100,
    AllowNull = false
};
```

**Behavior:**
- When cleared, value returns to **0** (or Minimum if set)
- Placeholder never shows
- Value property always has a numeric value

### Null with Minimum Value

```xml
<editors:SfNumericEntry WidthRequest="200"
                        Minimum="10"
                        AllowNull="False" />
```

**Behavior:** When cleared, value returns to **10** (the Minimum value)

## UI Customization

### Placeholder Text and Color

```xml
<editors:SfNumericEntry WidthRequest="200"
                        Placeholder="Enter amount"
                        PlaceholderColor="Gray" />
```

```csharp
numericEntry.Placeholder = "Enter amount";
numericEntry.PlaceholderColor = Colors.Gray;
```

### Text Color

```xml
<editors:SfNumericEntry WidthRequest="200"
                        TextColor="Blue" />
```

```csharp
numericEntry.TextColor = Colors.Blue;
```

### Font Properties

```xml
<editors:SfNumericEntry WidthRequest="200"
                        FontFamily="Arial"
                        FontSize="18"
                        FontAttributes="Bold" />
```

```csharp
numericEntry.FontFamily = "Arial";
numericEntry.FontSize = 18;
numericEntry.FontAttributes = FontAttributes.Bold;
```

**FontAttributes options:**
- `None` - Regular text
- `Bold` - Bold text
- `Italic` - Italic text
- `Bold, Italic` - Both bold and italic

### Font Auto-Scaling

```xml
<editors:SfNumericEntry WidthRequest="200"
                        FontAutoScalingEnabled="True" />
```

```csharp
numericEntry.FontAutoScalingEnabled = true;
```

**When enabled:** Font size scales based on platform accessibility settings

## Caret and Selection Control

### Set Cursor Position

```csharp
// Place cursor at position 3 (zero-based index)
numericEntry.CursorPosition = 3;
```

**Use case:** Position cursor after specific digits or at end of input

### Select Text Range

```csharp
// Select 2 characters starting at cursor position
numericEntry.SelectionLength = 2;

// Example: Select characters 2-4
numericEntry.CursorPosition = 2;
numericEntry.SelectionLength = 2;
```

**Use case:** Highlight portion of number for replacement

## Methods

### Focus() - Set Focus Programmatically

```csharp
// Give focus to Numeric Entry (opens keyboard)
numericEntry.Focus();
```

**When to use:**
- Auto-focus on page load
- Focus after validation error
- Navigate between inputs programmatically

### Unfocus() - Remove Focus

```csharp
// Remove focus from Numeric Entry (closes keyboard)
numericEntry.Unfocus();
```

**When to use:**
- Hide keyboard after validation
- Move focus to submit button
- Cancel input operation

## Events

### Focused Event

Fired when the Numeric Entry **gains focus**.

```xml
<editors:SfNumericEntry x:Name="numericEntry"
                        WidthRequest="200"
                        Focused="NumericEntry_Focused" />
```

```csharp
private void NumericEntry_Focused(object sender, FocusEventArgs e)
{
    // Handle focus gained
    Console.WriteLine("Numeric Entry gained focus");
    
    // Example: Change border color
    var entry = sender as SfNumericEntry;
    if (entry != null)
    {
        entry.Stroke = Colors.Blue;
    }
}
```

**Use cases:**
- Highlight control when focused
- Show contextual help
- Log user interaction
- Update UI state

### Unfocused Event

Fired when the Numeric Entry **loses focus**.

```xml
<editors:SfNumericEntry x:Name="numericEntry"
                        WidthRequest="200"
                        Unfocused="NumericEntry_Unfocused" />
```

```csharp
private void NumericEntry_Unfocused(object sender, FocusEventArgs e)
{
    // Handle focus lost
    Console.WriteLine("Numeric Entry lost focus");
    
    var entry = sender as SfNumericEntry;
    if (entry != null)
    {
        entry.Stroke = Colors.Gray;
    }
}
```

**Use cases:**
- Validate input after editing
- Restore default appearance
- Save draft data
- Trigger calculations

## Complete Example

Here's a complete working example combining multiple features:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:editors="clr-namespace:Syncfusion.Maui.Inputs;assembly=Syncfusion.Maui.Inputs"
             x:Class="NumericEntryApp.MainPage">
    
    <ScrollView>
        <VerticalStackLayout Padding="20" Spacing="20">
            
            <!-- Currency Input -->
            <StackLayout Spacing="5">
                <Label Text="Price ($):" FontSize="14" FontAttributes="Bold" />
                <editors:SfNumericEntry x:Name="priceEntry"
                                        WidthRequest="250"
                                        CustomFormat="C2"
                                        Value="99.99"
                                        AllowNull="False"
                                        Placeholder="$0.00"
                                        PlaceholderColor="LightGray"
                                        TextColor="Black"
                                        FontSize="16"
                                        Focused="OnEntryFocused"
                                        Unfocused="OnEntryUnfocused" />
            </StackLayout>
            
            <!-- Decimal Input -->
            <StackLayout Spacing="5">
                <Label Text="Quantity:" FontSize="14" FontAttributes="Bold" />
                <editors:SfNumericEntry x:Name="quantityEntry"
                                        WidthRequest="250"
                                        CustomFormat="N0"
                                        Value="1"
                                        AllowNull="False" />
            </StackLayout>
            
            <!-- Percentage Input -->
            <StackLayout Spacing="5">
                <Label Text="Discount (%):" FontSize="14" FontAttributes="Bold" />
                <editors:SfNumericEntry x:Name="discountEntry"
                                        WidthRequest="250"
                                        CustomFormat="P0"
                                        Value="0.15"
                                        AllowNull="True"
                                        Placeholder="No discount" />
            </StackLayout>
            
            <Button Text="Focus Price Entry" 
                    Clicked="OnFocusClicked" />
            
        </VerticalStackLayout>
    </ScrollView>
</ContentPage>
```

```csharp
using Syncfusion.Maui.Inputs;

namespace NumericEntryApp;

public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
    }
    
    private void OnEntryFocused(object sender, FocusEventArgs e)
    {
        var entry = sender as SfNumericEntry;
        if (entry != null)
        {
            entry.Stroke = Colors.Blue;
        }
    }
    
    private void OnEntryUnfocused(object sender, FocusEventArgs e)
    {
        var entry = sender as SfNumericEntry;
        if (entry != null)
        {
            entry.Stroke = Colors.Gray;
        }
    }
    
    private void OnFocusClicked(object sender, EventArgs e)
    {
        priceEntry.Focus();
    }
}
```

## Troubleshooting

### Control Not Rendering

**Problem:** Numeric Entry doesn't appear or shows as blank box

**Solution:** 
1. Verify `ConfigureSyncfusionCore()` is called in MauiProgram.cs
2. Check namespace is correctly added
3. Rebuild project
4. Clean and rebuild solution

### NuGet Package Not Found

**Problem:** Syncfusion.Maui.Inputs package not found

**Solution:**
1. Check internet connection
2. Verify NuGet package source is enabled
3. Try clearing NuGet cache: `dotnet nuget locals all --clear`
4. Manually download from [NuGet.org](https://www.nuget.org/packages/Syncfusion.Maui.Inputs)

### Value Not Updating

**Problem:** Value doesn't change when typing

**Solution:**
- Value updates only on Enter key or focus loss (by design)
- For real-time updates, see **basic-features.md** for `ValueChangeMode` property

## Next Steps

- **Basic Features:** Read [basic-features.md](basic-features.md) for placeholder, clear button, events, and borders
- **Formatting:** Read [formatting.md](formatting.md) for detailed format options and culture support
- **Restrictions:** Read [restrictions.md](restrictions.md) for min/max validation and editable control
- **UpDown Buttons:** Read [updown-buttons.md](updown-buttons.md) for increment/decrement functionality
