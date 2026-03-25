# Getting Started with .NET MAUI CheckBox

## Table of Contents
- [Step 1: Create a New MAUI Project](#step-1-create-a-new-maui-project)
- [Step 2: Install NuGet Package](#step-2-install-nuget-package)
- [Step 3: Register Handler](#step-3-register-handler)
- [Step 4: Basic Implementation](#step-4-basic-implementation)
- [Setting Caption Text](#setting-caption-text)
- [Understanding Three States](#understanding-three-states)
- [Single CheckBox Scenarios](#single-checkbox-scenarios)
- [Multiple CheckBox Groups](#multiple-checkbox-groups)
- [Intermediate State Pattern](#intermediate-state-pattern)

---

This guide walks you through setting up and configuring the Syncfusion .NET MAUI CheckBox (SfCheckBox) in your application.

## Step 1: Create a New MAUI Project

### Using Visual Studio:

1. Go to **File > New > Project**
2. Choose the **.NET MAUI App** template
3. Name the project (e.g., "CheckBoxDemo")
4. Choose a location and click **Next**
5. Select the .NET framework version (9.0 or later)
6. Click **Create**

### Using CLI:

```bash
dotnet new maui -n CheckBoxDemo
cd CheckBoxDemo
```

## Step 2: Install NuGet Package

The SfCheckBox control is part of the Syncfusion.Maui.Buttons package.

### Using Visual Studio:

1. In **Solution Explorer**, right-click the project
2. Choose **Manage NuGet Packages**
3. Search for **Syncfusion.Maui.Buttons**
4. Install the latest stable version
5. Ensure dependencies are restored

### Using Package Manager Console:

```powershell
Install-Package Syncfusion.Maui.Buttons
```

### Using CLI:

```bash
dotnet add package Syncfusion.Maui.Buttons
```

**Important**: The Syncfusion.Maui.Core package is automatically installed as a dependency.

## Step 3: Register Handler

Syncfusion controls require handler registration in your MauiProgram.cs file.

### MauiProgram.cs Configuration:

```csharp
using Microsoft.Extensions.Logging;
using Syncfusion.Maui.Core.Hosting;

namespace CheckBoxDemo
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

**Key point**: The `.ConfigureSyncfusionCore()` method must be called to register all Syncfusion MAUI control handlers.

## Step 4: Basic Implementation

### XAML Implementation:

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:buttons="clr-namespace:Syncfusion.Maui.Buttons;assembly=Syncfusion.Maui.Buttons"
             x:Class="CheckBoxDemo.MainPage">
    
    <StackLayout Padding="20">
        <buttons:SfCheckBox x:Name="checkBox"/>
    </StackLayout>
</ContentPage>
```

**Important**: Add the `xmlns:buttons` namespace declaration to use Syncfusion button controls.

### C# Implementation:

```csharp
using Syncfusion.Maui.Buttons;

namespace CheckBoxDemo
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            
            SfCheckBox checkBox = new SfCheckBox();
            this.Content = checkBox;
        }
    }
}
```

## Setting Caption Text

The `Text` property sets the caption displayed next to the checkbox.

### XAML:

```xml
<buttons:SfCheckBox x:Name="checkBox" 
                    Text="Accept terms and conditions" 
                    IsChecked="True"/>
```

### C#:

```csharp
SfCheckBox checkBox = new SfCheckBox
{
    Text = "Accept terms and conditions",
    IsChecked = true
};
this.Content = checkBox;
```

## Understanding Three States

The SfCheckBox supports three visual states:

| State | `IsChecked` Value | Description |
|-------|-------------------|-------------|
| **Checked** | `true` | Checkbox is selected (tick mark shown) |
| **Unchecked** | `false` | Checkbox is not selected (empty box) |
| **Indeterminate** | `null` | Checkbox is in an intermediate state (dash or partial mark) |

### Visual Representation:

```
Checked:       [✓]
Unchecked:     [ ]
Indeterminate: [─]
```

### Enabling Three-State Mode:

Set `IsThreeState="True"` to allow the indeterminate state:

```xml
<buttons:SfCheckBox x:Name="checkBox" 
                    Text="Select All" 
                    IsThreeState="True"
                    IsChecked="{x:Null}"/>
```

**Important**: When `IsThreeState="False"` and `IsChecked="null"`, the checkbox will display as unchecked.

## Single CheckBox Scenarios

Single checkboxes are commonly used for binary yes/no choices.

### Example 1: Terms of Service Agreement

```xml
<StackLayout Padding="20">
    <buttons:SfCheckBox x:Name="termsCheckBox" 
                        Text="I agree to the terms of service for this site" 
                        IsChecked="False"/>
    
    <Button Text="Submit" 
            IsEnabled="{Binding Source={x:Reference termsCheckBox}, Path=IsChecked}"
            Clicked="Submit_Clicked"/>
</StackLayout>
```

### Example 2: Remember Me (Login)

```xml
<StackLayout Padding="20" Spacing="10">
    <Entry Placeholder="Username"/>
    <Entry Placeholder="Password" IsPassword="True"/>
    
    <buttons:SfCheckBox x:Name="rememberMe" 
                        Text="Remember me" 
                        IsChecked="False"/>
    
    <Button Text="Log In" Clicked="Login_Clicked"/>
</StackLayout>
```

### Example 3: Newsletter Subscription

```xml
<buttons:SfCheckBox x:Name="newsletter" 
                    Text="Subscribe to our newsletter" 
                    IsChecked="True"/>
```

## Multiple CheckBox Groups

Multiple checkboxes allow users to select one or more non-mutually-exclusive options.

### Example: Pizza Toppings Selection

```xml
<StackLayout Padding="20">
    <Label Text="Pizza Toppings" 
           FontSize="18" 
           FontAttributes="Bold"
           Margin="0,10"/>
    
    <buttons:SfCheckBox x:Name="pepperoni" Text="Pepperoni"/>
    <buttons:SfCheckBox x:Name="beef" Text="Beef" IsChecked="True"/>
    <buttons:SfCheckBox x:Name="mushroom" Text="Mushrooms"/>
    <buttons:SfCheckBox x:Name="onion" Text="Onions" IsChecked="True"/>
</StackLayout>
```

### C# Implementation:

```csharp
StackLayout stackLayout = new StackLayout { Padding = 20 };

Label label = new Label
{
    Text = "Pizza Toppings",
    FontSize = 18,
    FontAttributes = FontAttributes.Bold,
    Margin = new Thickness(0, 10)
};

SfCheckBox pepperoni = new SfCheckBox { Text = "Pepperoni" };
SfCheckBox beef = new SfCheckBox { Text = "Beef", IsChecked = true };
SfCheckBox mushroom = new SfCheckBox { Text = "Mushrooms" };
SfCheckBox onion = new SfCheckBox { Text = "Onions", IsChecked = true };

stackLayout.Children.Add(label);
stackLayout.Children.Add(pepperoni);
stackLayout.Children.Add(beef);
stackLayout.Children.Add(mushroom);
stackLayout.Children.Add(onion);

this.Content = stackLayout;
```

## Intermediate State Pattern

The intermediate state is useful for parent-child checkbox hierarchies where a parent checkbox represents the state of multiple child checkboxes.

### Use Case: Select All with Child Options

**Behavior**:
- Parent is **checked** when all children are checked
- Parent is **unchecked** when all children are unchecked
- Parent is **indeterminate** when some (but not all) children are checked

### XAML:

```xml
<StackLayout Padding="20">
    <Label Text="Pizza Toppings" 
           FontSize="18" 
           Margin="10"/>
    
    <buttons:SfCheckBox x:Name="selectAll" 
                        Text="Select All" 
                        IsThreeState="True"
                        IsChecked="{x:Null}"
                        StateChanged="SelectAll_StateChanged"/>
    
    <buttons:SfCheckBox x:Name="pepperoni" 
                        Text="Pepperoni" 
                        Margin="30,0,0,0"
                        StateChanged="CheckBox_StateChanged"/>
    
    <buttons:SfCheckBox x:Name="beef" 
                        Text="Beef" 
                        IsChecked="True"
                        Margin="30,0,0,0"
                        StateChanged="CheckBox_StateChanged"/>
    
    <buttons:SfCheckBox x:Name="mushroom" 
                        Text="Mushrooms" 
                        Margin="30,0,0,0"
                        StateChanged="CheckBox_StateChanged"/>
    
    <buttons:SfCheckBox x:Name="onion" 
                        Text="Onions" 
                        IsChecked="True"
                        Margin="30,0,0,0"
                        StateChanged="CheckBox_StateChanged"/>
</StackLayout>
```

### C# Event Handlers:

```csharp
bool skip = false;

private void SelectAll_StateChanged(object sender, StateChangedEventArgs e)
{
    if (!skip)
    {
        skip = true;
        // Update all child checkboxes to match parent state
        pepperoni.IsChecked = e.IsChecked;
        beef.IsChecked = e.IsChecked;
        mushroom.IsChecked = e.IsChecked;
        onion.IsChecked = e.IsChecked;
        skip = false;
    }
}

private void CheckBox_StateChanged(object sender, StateChangedEventArgs e)
{
    if (!skip)
    {
        skip = true;
        
        // Check if all children are checked
        if (pepperoni.IsChecked == true && 
            beef.IsChecked == true && 
            mushroom.IsChecked == true && 
            onion.IsChecked == true)
        {
            selectAll.IsChecked = true;
        }
        // Check if all children are unchecked
        else if (pepperoni.IsChecked == false && 
                 beef.IsChecked == false && 
                 mushroom.IsChecked == false && 
                 onion.IsChecked == false)
        {
            selectAll.IsChecked = false;
        }
        // Some checked, some unchecked = indeterminate
        else
        {
            selectAll.IsChecked = null;
        }
        
        skip = false;
    }
}
```

**Important**: The `skip` flag prevents infinite recursion when updating checkboxes programmatically.

### Complete C# Setup:

```csharp
SfCheckBox selectAll, pepperoni, beef, mushroom, onion;

public MainPage()
{
    InitializeComponent();
    
    StackLayout stackLayout = new StackLayout { Padding = 20 };
    
    Label label = new Label
    {
        Text = "Pizza Toppings",
        FontSize = 18,
        Margin = new Thickness(10)
    };
    
    selectAll = new SfCheckBox
    {
        Text = "Select All",
        IsThreeState = true,
        IsChecked = null
    };
    selectAll.StateChanged += SelectAll_StateChanged;
    
    pepperoni = new SfCheckBox
    {
        Text = "Pepperoni",
        Margin = new Thickness(30, 0, 0, 0)
    };
    pepperoni.StateChanged += CheckBox_StateChanged;
    
    beef = new SfCheckBox
    {
        Text = "Beef",
        IsChecked = true,
        Margin = new Thickness(30, 0, 0, 0)
    };
    beef.StateChanged += CheckBox_StateChanged;
    
    mushroom = new SfCheckBox
    {
        Text = "Mushrooms",
        Margin = new Thickness(30, 0, 0, 0)
    };
    mushroom.StateChanged += CheckBox_StateChanged;
    
    onion = new SfCheckBox
    {
        Text = "Onions",
        IsChecked = true,
        Margin = new Thickness(30, 0, 0, 0)
    };
    onion.StateChanged += CheckBox_StateChanged;
    
    stackLayout.Children.Add(label);
    stackLayout.Children.Add(selectAll);
    stackLayout.Children.Add(pepperoni);
    stackLayout.Children.Add(beef);
    stackLayout.Children.Add(mushroom);
    stackLayout.Children.Add(onion);
    
    this.Content = stackLayout;
}
```

## Best Practices

1. **Namespace Import**: Always import `Syncfusion.Maui.Buttons` namespace
2. **Handler Registration**: Call `ConfigureSyncfusionCore()` before using any Syncfusion control
3. **Three-State**: Enable `IsThreeState` only when you need indeterminate state support
4. **Event Handling**: Use the `skip` flag pattern to prevent recursion in parent-child scenarios
5. **Accessibility**: Provide clear, descriptive text for all checkboxes
6. **Layout**: Use appropriate margins/padding for checkbox groups
7. **Validation**: Check `IsChecked.HasValue` before accessing `IsChecked.Value` to avoid null reference errors

## Common Gotchas

**Issue**: Checkbox doesn't appear or throws exception  
**Solution**: Ensure `ConfigureSyncfusionCore()` is called in MauiProgram.cs

**Issue**: Indeterminate state shows as unchecked  
**Solution**: Set `IsThreeState="True"` before setting `IsChecked="{x:Null}"`

**Issue**: Infinite loop with parent-child checkboxes  
**Solution**: Use a `skip` flag to prevent recursive event handling

**Issue**: NuGet package not found  
**Solution**: Ensure you have the correct NuGet source configured and check package name spelling
