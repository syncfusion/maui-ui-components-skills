# Getting Started with .NET MAUI Radio Button

This guide walks you through setting up and configuring a Syncfusion .NET MAUI Radio Button (SfRadioButton) in your application, from installation to creating your first working radio button.

## Step 1: Create a New .NET MAUI Project

### Using Visual Studio

1. Open Visual Studio
2. Go to **File > New > Project**
3. Search for and select the **.NET MAUI App** template
4. Click **Next**
5. Name your project (e.g., `RadioButtonDemo`)
6. Choose a location for your project
7. Click **Next**
8. Select the .NET framework version (9.0 or later)
9. Click **Create**

### Using .NET CLI

```bash
dotnet new maui -n RadioButtonDemo
cd RadioButtonDemo
```

## Step 2: Install Syncfusion MAUI Buttons NuGet Package

### Using Visual Studio

1. In **Solution Explorer**, right-click the project
2. Select **Manage NuGet Packages**
3. Click the **Browse** tab
4. Search for `Syncfusion.Maui.Buttons`
5. Select the package from the results
6. Click **Install** and accept the license agreement
7. Wait for the package and its dependencies to install
8. Ensure the project is restored successfully

### Using .NET CLI

```bash
dotnet add package Syncfusion.Maui.Buttons
```

### Using Package Manager Console

```powershell
Install-Package Syncfusion.Maui.Buttons
```

## Step 3: Register the Syncfusion Handler

The `Syncfusion.Maui.Core` NuGet package is a dependency for all Syncfusion .NET MAUI controls. You must register the Syncfusion handler in your `MauiProgram.cs` file.

### Update MauiProgram.cs

Open `MauiProgram.cs` and add the Syncfusion configuration:

```csharp
using Microsoft.Extensions.Logging;
using Syncfusion.Maui.Core.Hosting;

namespace RadioButtonDemo
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

**Key Points:**
- Import the `Syncfusion.Maui.Core.Hosting` namespace
- Call `.ConfigureSyncfusionCore()` in the builder chain
- This registration is required for all Syncfusion MAUI controls

## Step 4: Add Your First Radio Button

### Import the Buttons Namespace

In your XAML or C# file, import the Syncfusion Buttons namespace:

**XAML:**
```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:buttons="clr-namespace:Syncfusion.Maui.Buttons;assembly=Syncfusion.Maui.Buttons"
             x:Class="RadioButtonDemo.MainPage">
```

**C#:**
```csharp
using Syncfusion.Maui.Buttons;
```

### Create a Basic Radio Button

#### XAML Implementation

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:buttons="clr-namespace:Syncfusion.Maui.Buttons;assembly=Syncfusion.Maui.Buttons"
             x:Class="RadioButtonDemo.MainPage">
    
    <VerticalStackLayout Padding="20" Spacing="10">
        <buttons:SfRadioButton x:Name="radioButton" Text="Option 1"/>
    </VerticalStackLayout>
    
</ContentPage>
```

#### C# Implementation

```csharp
using Syncfusion.Maui.Buttons;

namespace RadioButtonDemo
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            
            // Create a radio button
            SfRadioButton radioButton = new SfRadioButton
            {
                Text = "Option 1"
            };
            
            // Create a layout and add the radio button
            VerticalStackLayout layout = new VerticalStackLayout
            {
                Padding = new Thickness(20),
                Spacing = 10
            };
            layout.Children.Add(radioButton);
            
            this.Content = layout;
        }
    }
}
```

## Setting Caption Text

The caption text is displayed next to the radio button and describes its purpose. Set it using the `Text` property:

### XAML
```xml
<buttons:SfRadioButton x:Name="radioButton" Text="Premium Subscription"/>
```

### C#
```csharp
SfRadioButton radioButton = new SfRadioButton
{
    Text = "Premium Subscription"
};
```

## Changing the Radio Button State

Radio buttons have two primary states: **Checked** and **Unchecked**. Control the state using the `IsChecked` property:

### Setting Initial State

**XAML:**
```xml
<buttons:SfRadioButton Text="Option 1" IsChecked="True"/>
<buttons:SfRadioButton Text="Option 2" IsChecked="False"/>
```

**C#:**
```csharp
SfRadioButton option1 = new SfRadioButton
{
    Text = "Option 1",
    IsChecked = true  // Initially checked
};

SfRadioButton option2 = new SfRadioButton
{
    Text = "Option 2",
    IsChecked = false  // Initially unchecked
};
```

### Visual Feedback

When a radio button is checked, an inner circle appears within the button to provide visual feedback.

## Basic Radio Button Group

To create mutually exclusive radio buttons (where only one can be selected at a time), use `SfRadioGroup`:

### XAML Example

```xml
<ContentPage xmlns:buttons="clr-namespace:Syncfusion.Maui.Buttons;assembly=Syncfusion.Maui.Buttons">
    
    <VerticalStackLayout Padding="20" Spacing="10">
        <Label Text="Select Payment Method:" FontSize="16" FontAttributes="Bold"/>
        
        <buttons:SfRadioGroup x:Name="paymentGroup">
            <buttons:SfRadioButton Text="Credit Card"/>
            <buttons:SfRadioButton Text="Debit Card" IsChecked="True"/>
            <buttons:SfRadioButton Text="Net Banking"/>
        </buttons:SfRadioGroup>
    </VerticalStackLayout>
    
</ContentPage>
```

### C# Example

```csharp
// Create the radio group
SfRadioGroup paymentGroup = new SfRadioGroup();

// Create radio buttons
SfRadioButton creditCard = new SfRadioButton { Text = "Credit Card" };
SfRadioButton debitCard = new SfRadioButton { Text = "Debit Card", IsChecked = true };
SfRadioButton netBanking = new SfRadioButton { Text = "Net Banking" };

// Add radio buttons to the group
paymentGroup.Children.Add(creditCard);
paymentGroup.Children.Add(debitCard);
paymentGroup.Children.Add(netBanking);

// Create layout
VerticalStackLayout layout = new VerticalStackLayout
{
    Padding = new Thickness(20),
    Spacing = 10
};
layout.Children.Add(new Label 
{ 
    Text = "Select Payment Method:", 
    FontSize = 16, 
    FontAttributes = FontAttributes.Bold 
});
layout.Children.Add(paymentGroup);

this.Content = layout;
```

**Important:** Radio buttons within an `SfRadioGroup` are automatically mutually exclusive. When you select one, all others are deselected.

## Complete Working Example

Here's a complete example demonstrating a basic form with radio buttons:

### MainPage.xaml

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:buttons="clr-namespace:Syncfusion.Maui.Buttons;assembly=Syncfusion.Maui.Buttons"
             x:Class="RadioButtonDemo.MainPage"
             BackgroundColor="{DynamicResource PageBackgroundColor}">

    <ScrollView>
        <VerticalStackLayout Padding="30" Spacing="20">
            
            <Label Text="Survey Form" 
                   FontSize="24" 
                   FontAttributes="Bold"
                   HorizontalOptions="Center"/>
            
            <!-- Gender Selection -->
            <Label Text="Gender:" FontSize="16" FontAttributes="Bold"/>
            <buttons:SfRadioGroup x:Name="genderGroup">
                <buttons:SfRadioButton Text="Male"/>
                <buttons:SfRadioButton Text="Female" IsChecked="True"/>
                <buttons:SfRadioButton Text="Other"/>
            </buttons:SfRadioGroup>
            
            <!-- Age Group Selection -->
            <Label Text="Age Group:" FontSize="16" FontAttributes="Bold"/>
            <buttons:SfRadioGroup x:Name="ageGroup">
                <buttons:SfRadioButton Text="18-25"/>
                <buttons:SfRadioButton Text="26-35" IsChecked="True"/>
                <buttons:SfRadioButton Text="36-50"/>
                <buttons:SfRadioButton Text="50+"/>
            </buttons:SfRadioGroup>
            
            <Button Text="Submit" Clicked="OnSubmitClicked"/>
            
        </VerticalStackLayout>
    </ScrollView>

</ContentPage>
```

### MainPage.xaml.cs

```csharp
using Syncfusion.Maui.Buttons;

namespace RadioButtonDemo
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
        }

        private void OnSubmitClicked(object sender, EventArgs e)
        {
            string gender = GetSelectedRadioButtonText(genderGroup);
            string ageGroup = GetSelectedRadioButtonText(this.ageGroup);
            
            DisplayAlert("Form Submitted", 
                $"Gender: {gender}\nAge Group: {ageGroup}", 
                "OK");
        }

        private string GetSelectedRadioButtonText(SfRadioGroup group)
        {
            if (group.CheckedItem is SfRadioButton checkedButton)
            {
                return checkedButton.Text;
            }
            return "None";
        }
    }
}
```

## Troubleshooting

### Radio Button Not Appearing

**Issue:** The radio button doesn't show up in the UI.

**Solutions:**
1. Verify `ConfigureSyncfusionCore()` is called in `MauiProgram.cs`
2. Check that the Syncfusion.Maui.Buttons package is installed
3. Ensure the namespace is correctly imported
4. Clean and rebuild the project

### Multiple Radio Buttons Selected Simultaneously

**Issue:** More than one radio button is checked at the same time.

**Solution:** Ensure radio buttons are within an `SfRadioGroup` or share the same `GroupKey` for mutual exclusion.

### Build Errors After Adding Package

**Issue:** Build fails after adding Syncfusion package.

**Solutions:**
1. Restore NuGet packages (right-click solution → Restore NuGet Packages)
2. Clean the solution and rebuild
3. Verify .NET MAUI workload is installed
4. Check that all dependencies are compatible with your .NET version
