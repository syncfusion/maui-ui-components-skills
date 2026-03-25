# Getting Started with .NET MAUI DataForm

Complete guide to setting up and implementing your first DataForm in a .NET MAUI application.

## Table of Contents
- [Installation](#installation)
- [Handler Registration](#handler-registration)
- [Creating Data Objects](#creating-data-objects)
- [Adding DataForm to Your App](#adding-dataform-to-your-app)
- [DataForm in StackLayout](#dataform-in-stacklayout)
- [Editor Auto-Generation](#editor-auto-generation)
- [Label Customization](#label-customization)
- [Troubleshooting](#troubleshooting)

## Installation

### Step 1: Create a New .NET MAUI Project

#### Visual Studio
1. **File > New > Project**
2. Select **.NET MAUI App** template
3. Name your project and choose a location
4. Click **Next**, select .NET framework version, then **Create**

#### Visual Studio Code
1. Press `Ctrl+Shift+P` (or `Cmd+Shift+P` on Mac)
2. Type **.NET:New Project** and press Enter
3. Choose **.NET MAUI App** template
4. Select project location and enter project name

### Step 2: Install Syncfusion.Maui.DataForm NuGet Package

#### Visual Studio
1. In **Solution Explorer**, right-click the project
2. Choose **Manage NuGet Packages**
3. Search for `Syncfusion.Maui.DataForm`
4. Click **Install** on the latest version

#### Visual Studio Code
1. Press `Ctrl+` ` (backtick) to open integrated terminal
2. Ensure you're in the project root directory
3. Run: 
   ```bash
   dotnet add package Syncfusion.Maui.DataForm
   ```
4. Restore dependencies:
   ```bash
   dotnet restore
   ```
## Handler Registration

The `Syncfusion.Maui.Core` package is a dependency for all Syncfusion MAUI controls and must be registered in your application.

### Register in MauiProgram.cs

Open `MauiProgram.cs` and add the Syncfusion handler registration:

```csharp
using Syncfusion.Maui.Core.Hosting;

public static class MauiProgram
{
    public static MauiApp CreateMauiApp()
    {
        var builder = MauiApp.CreateBuilder();
        builder
            .UseMauiApp<App>()
            .ConfigureSyncfusionCore() // ← Add this line
            .ConfigureFonts(fonts =>
            {
                fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
                fonts.AddFont("OpenSans-Semibold.ttf", "OpenSansSemibold");
            });

        return builder.Build();
    }
}
```

**Important:** `ConfigureSyncfusionCore()` must be called once during app initialization. Without this, DataForm and other Syncfusion controls won't function properly.

## Creating Data Objects

DataForm automatically generates form editors from your data object's properties. Create a model class with properties representing form fields.

### Basic Data Object

```csharp
public class ContactInfo
{
    public string FirstName { get; set; }
    public string MiddleName { get; set; }
    public string LastName { get; set; }
    public string ContactNumber { get; set; }
    public string Email { get; set; }
    public string Address { get; set; }
    public DateTime? BirthDate { get; set; }
    public string GroupName { get; set; }
}
```

### ViewModel with Data Object

Create a ViewModel to hold your data object:

```csharp
public class DataFormViewModel
{
    public ContactInfo ContactInfo { get; set; }
    
    public DataFormViewModel()
    {
        // Initialize with empty or default values
        this.ContactInfo = new ContactInfo();
    }
}
```

**Why use a ViewModel?**
- Separates UI logic from data
- Enables data binding
- Makes testing easier
- Follows MVVM pattern best practices

## Adding DataForm to Your App

### XAML Approach (Recommended)

#### Step 1: Import the DataForm Namespace

Add the `dataForm` namespace to your ContentPage:

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:dataForm="clr-namespace:Syncfusion.Maui.DataForm;assembly=Syncfusion.Maui.DataForm"
             xmlns:local="clr-namespace:YourNamespace"
             x:Class="YourNamespace.MainPage">
```

#### Step 2: Set the BindingContext

```xml
<ContentPage.BindingContext>
    <local:DataFormViewModel/>
</ContentPage.BindingContext>
```

#### Step 3: Add the DataForm Control

```xml
<dataForm:SfDataForm x:Name="dataForm" 
                     DataObject="{Binding ContactInfo}"/>
```

#### Complete XAML Example

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:dataForm="clr-namespace:Syncfusion.Maui.DataForm;assembly=Syncfusion.Maui.DataForm"
             xmlns:local="clr-namespace:YourNamespace"
             x:Class="YourNamespace.MainPage">
    
    <ContentPage.BindingContext>
        <local:DataFormViewModel/>
    </ContentPage.BindingContext>
    
    <dataForm:SfDataForm x:Name="dataForm" 
                         DataObject="{Binding ContactInfo}"/>
</ContentPage>
```

### Code-Behind Approach

```csharp
using Syncfusion.Maui.DataForm;

public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        
        // Create ViewModel
        this.BindingContext = new DataFormViewModel();
        
        // Create and configure DataForm
        SfDataForm dataForm = new SfDataForm()
        {
            DataObject = new ContactInfo()
        };
        
        // Set as page content
        this.Content = dataForm;
    }
}
```

### With Explicit DataObject (No ViewModel)

If you don't need a ViewModel, bind directly to a data object:

```csharp
public partial class MainPage : ContentPage
{
    private ContactInfo contactInfo;
    
    public MainPage()
    {
        InitializeComponent();
        
        contactInfo = new ContactInfo();
        
        SfDataForm dataForm = new SfDataForm()
        {
            DataObject = contactInfo
        };
        
        this.Content = dataForm;
    }
}
```

## DataForm in StackLayout

When placing DataForm inside a `StackLayout`, you must set minimum size constraints to ensure proper rendering.

### Vertical StackLayout

Set `MinimumHeightRequest` (default: 300):

```xml
<VerticalStackLayout>
    <Label Text="Contact Form" FontSize="24"/>
    
    <dataForm:SfDataForm x:Name="dataForm" 
                         MinimumHeightRequest="500"
                         DataObject="{Binding ContactInfo}"/>
    
    <Button Text="Submit" Clicked="OnSubmitClicked"/>
</VerticalStackLayout>
```

**Code-Behind:**
```csharp
SfDataForm dataForm = new SfDataForm()
{
    DataObject = new ContactInfo(),
    MinimumHeightRequest = 500
};
```

### Horizontal StackLayout

Set `MinimumWidthRequest` (default: 300):

```xml
<HorizontalStackLayout>
    <dataForm:SfDataForm x:Name="dataForm" 
                         MinimumWidthRequest="450"
                         DataObject="{Binding ContactInfo}"/>
    
    <VerticalStackLayout>
        <Button Text="Save"/>
        <Button Text="Cancel"/>
    </VerticalStackLayout>
</HorizontalStackLayout>
```

**Code-Behind:**
```csharp
SfDataForm dataForm = new SfDataForm()
{
    DataObject = new ContactInfo(),
    MinimumWidthRequest = 450
};
```

### Best Practice: Use ScrollView or Grid

For better responsiveness, consider using `ScrollView` or `Grid` instead of `StackLayout`:

```xml
<ScrollView>
    <dataForm:SfDataForm x:Name="dataForm" 
                         DataObject="{Binding ContactInfo}"/>
</ScrollView>
```

## Editor Auto-Generation

DataForm automatically generates appropriate editors based on property types. Here's how different data types are mapped:

### Basic Type Mappings

| Property Type | Generated Editor | Input Control |
|--------------|------------------|---------------|
| `string` | Text | Entry |
| `int`, `double`, `float` | Numeric | SfNumericEntry |
| `bool` | CheckBox | CheckBox |
| `DateTime` | Date | DatePicker |
| `TimeSpan` | Time | TimePicker |
| `Enum` | Picker | Picker |

### Example with Multiple Types

```csharp
public class EmployeeInfo
{
    public string Name { get; set; }              // → Text editor
    public int Age { get; set; }                  // → Numeric editor
    public double Salary { get; set; }            // → Numeric editor
    public bool IsFullTime { get; set; }          // → CheckBox
    public DateTime JoinDate { get; set; }        // → DatePicker
    public TimeSpan WorkHours { get; set; }       // → TimePicker
    public Department Dept { get; set; }          // → Picker
}

public enum Department
{
    IT,
    HR,
    Finance,
    Marketing
}
```

This single data object will automatically generate 7 different types of editors!

## Label Customization

Customize how labels appear using the `DataFormItemManager`.

### Basic Label Customization

```csharp
public class CustomItemManager : DataFormItemManager
{
    public override void InitializeDataLabel(DataFormItem dataFormItem, Label label)
    {
        // Customize all labels
        label.Background = Colors.LightBlue;
        label.TextColor = Colors.DarkBlue;
        label.FontSize = 16;
        label.FontAttributes = FontAttributes.Bold;
        label.Padding = new Thickness(10, 5);
        label.Margin = new Thickness(0, 0, 10, 0);
    }
}

// Apply to DataForm
dataForm.ItemManager = new CustomItemManager();
```

### Add Required Field Indicator

```csharp
public class RequiredFieldItemManager : DataFormItemManager
{
    public override void InitializeDataLabel(DataFormItem dataFormItem, Label label)
    {
        // Create formatted text with red asterisk
        FormattedString formattedString = new FormattedString();
        formattedString.Spans.Add(new Span 
        { 
            Text = label.Text, 
            TextColor = Colors.Black 
        });
        formattedString.Spans.Add(new Span 
        { 
            Text = " *", 
            TextColor = Colors.Red,
            FontAttributes = FontAttributes.Bold
        });
        label.FormattedText = formattedString;
    }
}
```

### Conditional Label Styling

```csharp
public class ConditionalItemManager : DataFormItemManager
{
    public override void InitializeDataLabel(DataFormItem dataFormItem, Label label)
    {
        // Highlight specific fields
        if (dataFormItem.FieldName == "Email" || dataFormItem.FieldName == "PhoneNumber")
        {
            label.Background = Colors.Yellow;
            label.TextColor = Colors.DarkOrange;
            label.FontAttributes = FontAttributes.Bold;
        }
        else
        {
            label.TextColor = Colors.Gray;
        }
        
        label.VerticalOptions = LayoutOptions.Center;
        label.Padding = new Thickness(5);
    }
}
```

## Troubleshooting

### Issue: DataForm Doesn't Appear

**Symptoms:** The page displays but DataForm is invisible or empty.

**Solutions:**
1. Verify `ConfigureSyncfusionCore()` is called in `MauiProgram.cs`
2. Check that `DataObject` is not null
3. Ensure data object properties are public with getters and setters
4. If in StackLayout, set `MinimumHeightRequest` or `MinimumWidthRequest`

```csharp
// ❌ Wrong - properties are private
private string Name { get; set; }

// ✅ Correct - public properties
public string Name { get; set; }
```

### Issue: Handler Not Registered Error

**Error:** "Handler not found for view..."

**Solution:** Add `ConfigureSyncfusionCore()` to `MauiProgram.cs`:

```csharp
builder
    .UseMauiApp<App>()
    .ConfigureSyncfusionCore(); // ← Must include this
```

### Issue: NuGet Package Restore Fails

**Symptoms:** Build errors about missing Syncfusion references.

**Solutions:**
1. Run `dotnet restore` in terminal
2. Clean and rebuild solution
3. Check internet connection (package download)
4. Verify NuGet package source is configured

```bash
# Terminal commands
dotnet clean
dotnet restore
dotnet build
```

### Issue: AOT Compilation Errors (iOS/macOS)

**Error:** Binding issues when publishing in AOT mode.

**Solution:** Add `[Preserve]` attribute to your model class:

```csharp
using System.Diagnostics.CodeAnalysis;

[Preserve(AllMembers = true)]
public class ContactInfo
{
    public string FirstName { get; set; }
    public string Email { get; set; }
    // ... other properties
}
```

### Issue: DataForm in ScrollView Takes Full Height

**Symptoms:** DataForm expands to fill ScrollView, no scrolling works.

**Solution:** Set explicit height or use VerticalOptions:

```xml
<ScrollView>
    <dataForm:SfDataForm x:Name="dataForm" 
                         DataObject="{Binding ContactInfo}"
                         VerticalOptions="Start"/>
</ScrollView>
```

## Next Steps

Now that you have a basic DataForm running:

- **[Built-in Editors](built-in-editors.md)** - Learn about all available editor types
- **[Validation](validation.md)** - Add input validation to your forms
- **[Data Annotations](data-annotations.md)** - Configure forms using attributes
- **[Layout](layout.md)** - Organize fields in grid layouts
- **[Custom Editors](custom-editors.md)** - Create custom input controls

## Additional Resources

- [GitHub Sample - Getting Started](https://github.com/SyncfusionExamples/maui-dataform/tree/master/GettingStarted)
- [API Documentation](https://help.syncfusion.com/cr/maui/Syncfusion.Maui.DataForm.SfDataForm.html)
- [Component Overview](https://help.syncfusion.com/maui/dataform/overview)
