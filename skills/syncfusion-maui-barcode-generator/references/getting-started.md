# Getting Started with Barcode Generators

## Table of Contents
- [Overview](#overview)
- [Installation](#installation)
  - [Visual Studio Setup](#visual-studio-setup)
  - [Visual Studio Code Setup](#visual-studio-code-setup)
  - [JetBrains Rider Setup](#jetbrains-rider-setup)
- [Handler Registration](#handler-registration)
- [Basic Implementation](#basic-implementation)
- [Symbology Configuration](#symbology-configuration)
- [Displaying Input Values](#displaying-input-values)
- [Complete Example](#complete-example)
- [Troubleshooting](#troubleshooting)

## Overview

This guide walks through the complete setup process for implementing Syncfusion .NET MAUI Barcode Generator (SfBarcodeGenerator) in your application. The Barcode Generator control allows you to create machine-readable barcodes for various use cases including retail, inventory management, shipping, and marketing.

**What you'll learn:**
- Installing the NuGet package across different IDEs
- Registering the required handler
- Creating your first barcode
- Setting different symbology types
- Displaying barcode values as text

## Installation

The installation process varies slightly by IDE. Choose the section that matches your development environment.

### Visual Studio Setup

#### Step 1: Create a New .NET MAUI Project

1. Go to **File > New > Project**
2. Choose the **.NET MAUI App** template
3. Name the project (e.g., "BarcodeApp")
4. Choose a location
5. Click **Next**
6. Select the .NET framework version (.NET 9 or later)
7. Click **Create**

#### Step 2: Install the Syncfusion Barcode NuGet Package

1. In **Solution Explorer**, right-click the project
2. Choose **Manage NuGet Packages**
3. Click the **Browse** tab
4. Search for `Syncfusion.Maui.Barcode`
5. Select **Syncfusion.Maui.Barcode** from the results
6. Click **Install**
7. Accept the license agreement
8. Wait for the package and dependencies to install
9. Ensure the project is restored (check Output window)

**Package URL:** https://www.nuget.org/packages/Syncfusion.Maui.Barcode/

### Visual Studio Code Setup

#### Step 1: Create a New .NET MAUI Project

1. Open the command palette: `Ctrl+Shift+P` (Windows/Linux) or `Cmd+Shift+P` (Mac)
2. Type **.NET:New Project** and press Enter
3. Choose the **.NET MAUI App** template
4. Select the project location
5. Type the project name (e.g., "BarcodeApp")
6. Press **Enter**
7. Choose **Create project**

#### Step 2: Install the Syncfusion Barcode NuGet Package

1. Press `Ctrl+`` (backtick) to open the integrated terminal
2. Ensure you're in the project root directory (where the .csproj file is located)
3. Run the installation command:

```bash
dotnet add package Syncfusion.Maui.Barcode
```

4. Restore dependencies:

```bash
dotnet restore
```

5. Verify installation by checking your .csproj file:

```xml
<ItemGroup>
  <PackageReference Include="Syncfusion.Maui.Barcode" Version="27.1.48" />
</ItemGroup>
```

### JetBrains Rider Setup

#### Step 1: Create a New .NET MAUI Project

1. Go to **File > New Solution**
2. Select **.NET (C#)**
3. Choose the **.NET MAUI App** template
4. Enter the **Project Name** (e.g., "BarcodeApp")
5. Enter the **Solution Name**
6. Choose the **Location**
7. Select the .NET framework version (.NET 9 or later)
8. Click **Create**

#### Step 2: Install the Syncfusion Barcode NuGet Package

1. In **Solution Explorer**, right-click the project
2. Choose **Manage NuGet Packages**
3. Click the **Browse** tab (or **Packages** tab)
4. Search for `Syncfusion.Maui.Barcode`
5. Select **Syncfusion.Maui.Barcode**
6. Click the **+** (Add) button or **Install**
7. Ensure dependencies are installed correctly

**Alternative: Terminal Installation**

1. Open the Terminal in Rider (View > Tool Windows > Terminal)
2. Navigate to project directory
3. Run:

```bash
dotnet add package Syncfusion.Maui.Barcode
dotnet restore
```

## Handler Registration

**CRITICAL:** The Syncfusion.Maui.Core NuGet is a dependent package for all Syncfusion controls. You **must** register the handler for Syncfusion Core in your MauiProgram.cs file.

### Locate MauiProgram.cs

In your project root, find and open `MauiProgram.cs`.

### Add Using Statement

At the top of the file, add:

```csharp
using Syncfusion.Maui.Core.Hosting;
```

### Register the Handler

In the `CreateMauiApp()` method, add `builder.ConfigureSyncfusionCore();`:

```csharp
using Syncfusion.Maui.Core.Hosting;

namespace BarcodeApp
{
    public static class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            var builder = MauiApp.CreateBuilder();

            // Register Syncfusion Core - REQUIRED
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

**Important:** `ConfigureSyncfusionCore()` must be called before other configuration methods.

## Basic Implementation

Now that the package is installed and the handler is registered, you can add a barcode to your application.

### Step 1: Add Namespace

In your XAML file (e.g., MainPage.xaml), add the barcode namespace:

```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:barcode="clr-namespace:Syncfusion.Maui.Barcode;assembly=Syncfusion.Maui.Barcode"
             x:Class="BarcodeApp.MainPage">
```

For C# code-behind or code-only pages:

```csharp
using Syncfusion.Maui.Barcode;
```

### Step 2: Initialize SfBarcodeGenerator

**XAML:**

```xaml
<barcode:SfBarcodeGenerator Value="https://www.syncfusion.com" 
                            HeightRequest="150"/>
```

**C#:**

```csharp
SfBarcodeGenerator barcode = new SfBarcodeGenerator();
barcode.HeightRequest = 150;
barcode.Value = "https://www.syncfusion.com";
this.Content = barcode;
```

**Result:** This creates a Code128 barcode (the default symbology) encoding the URL.

### Step 3: Build and Run

- **Visual Studio:** Press F5 or click the Run button
- **VS Code:** Press F5 or run `dotnet build` and `dotnet run`
- **Rider:** Click the Run button or press Shift+F10

You should see a barcode displayed in your application.

## Symbology Configuration

The default symbology is Code128, but you can change it to any supported type using the `Symbology` property.

### Setting QR Code Symbology

**XAML:**

```xaml
<barcode:SfBarcodeGenerator Value="https://www.syncfusion.com/" 
                            HeightRequest="350" 
                            WidthRequest="350">
    <barcode:SfBarcodeGenerator.Symbology>
        <barcode:QRCode />
    </barcode:SfBarcodeGenerator.Symbology>
</barcode:SfBarcodeGenerator>
```

**C#:**

```csharp
SfBarcodeGenerator barcode = new SfBarcodeGenerator();
barcode.HeightRequest = 350;
barcode.WidthRequest = 350;
barcode.Value = "https://www.syncfusion.com/";
barcode.Symbology = new QRCode();
this.Content = barcode;
```

### Available Symbology Types

**One-Dimensional:**
- `Code128` (default)
- `Code128A`, `Code128B`, `Code128C`
- `Code39`, `Code39Extended`
- `Code93`
- `Codabar`
- `UPCA`, `UPCE`
- `EAN8`, `EAN13`

**Two-Dimensional:**
- `QRCode`
- `DataMatrix`

**Example - UPC-A:**

```csharp
barcode.Symbology = new UPCA();
barcode.Value = "72527273070"; // 11 digits, check digit auto-added
```

**Example - Data Matrix:**

```csharp
barcode.Symbology = new DataMatrix();
barcode.Value = "Syncfusion MAUI Barcode";
```

## Displaying Input Values

By default, the barcode value is not displayed as text. Enable the `ShowText` property to display the input value below the barcode.

### Enable Text Display

**XAML:**

```xaml
<barcode:SfBarcodeGenerator Value="https://www.syncfusion.com/" 
                            ShowText="True" 
                            TextSpacing="15" 
                            HeightRequest="350" 
                            WidthRequest="350">
    <barcode:SfBarcodeGenerator.Symbology>
        <barcode:QRCode />
    </barcode:SfBarcodeGenerator.Symbology>
</barcode:SfBarcodeGenerator>
```

**C#:**

```csharp
SfBarcodeGenerator barcode = new SfBarcodeGenerator();
barcode.HeightRequest = 350;
barcode.WidthRequest = 350;
barcode.Value = "https://www.syncfusion.com/";
barcode.Symbology = new QRCode();
barcode.ShowText = true;  // Display the value as text
barcode.TextSpacing = 15;  // Space between barcode and text
this.Content = barcode;
```

**Properties:**
- `ShowText` - Boolean, enables/disables text display (default: false)
- `TextSpacing` - Double, space between barcode and text in pixels (default: 2)

## Complete Example

Here's a complete MainPage.xaml example with multiple barcodes:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:barcode="clr-namespace:Syncfusion.Maui.Barcode;assembly=Syncfusion.Maui.Barcode"
             x:Class="BarcodeApp.MainPage"
             BackgroundColor="{DynamicResource PageBackgroundColor}">

    <ScrollView>
        <VerticalStackLayout Padding="20" Spacing="20">
            
            <Label Text="Barcode Examples" 
                   FontSize="24" 
                   FontAttributes="Bold"
                   HorizontalOptions="Center"/>

            <!-- Code128 Barcode (Default) -->
            <Label Text="Code128 (Default):" FontSize="16"/>
            <barcode:SfBarcodeGenerator Value="CODE128EXAMPLE" 
                                        HeightRequest="100"
                                        ShowText="True"/>

            <!-- QR Code -->
            <Label Text="QR Code:" FontSize="16"/>
            <barcode:SfBarcodeGenerator Value="https://www.syncfusion.com" 
                                        HeightRequest="250" 
                                        WidthRequest="250"
                                        ShowText="True"
                                        HorizontalOptions="Center">
                <barcode:SfBarcodeGenerator.Symbology>
                    <barcode:QRCode />
                </barcode:SfBarcodeGenerator.Symbology>
            </barcode:SfBarcodeGenerator>

            <!-- UPC-A Product Code -->
            <Label Text="UPC-A Product Code:" FontSize="16"/>
            <barcode:SfBarcodeGenerator Value="72527273070" 
                                        HeightRequest="100"
                                        ShowText="True">
                <barcode:SfBarcodeGenerator.Symbology>
                    <barcode:UPCA Module="2"/>
                </barcode:SfBarcodeGenerator.Symbology>
            </barcode:SfBarcodeGenerator>

        </VerticalStackLayout>
    </ScrollView>

</ContentPage>
```

## Troubleshooting

### Issue: Barcode Control Not Rendering

**Solution:**
1. Verify `ConfigureSyncfusionCore()` is called in MauiProgram.cs
2. Check that the NuGet package is installed correctly
3. Ensure the namespace is imported: `xmlns:barcode="clr-namespace:Syncfusion.Maui.Barcode;assembly=Syncfusion.Maui.Barcode"`
4. Clean and rebuild the solution

### Issue: "The name 'SfBarcodeGenerator' does not exist in the current context"

**Solution:**
1. Add using statement: `using Syncfusion.Maui.Barcode;`
2. Verify the NuGet package is installed
3. Run `dotnet restore`
4. Rebuild the project

### Issue: Runtime Error - Handler Not Found

**Solution:**
Add the Syncfusion Core handler registration:

```csharp
builder.ConfigureSyncfusionCore();
```

### Issue: Invalid Value Error

**Solution:**
- Verify the value matches the symbology requirements:
  - UPC-A: 11 or 12 digits
  - EAN-13: 12 or 13 digits
  - Code39: Alphanumeric + special chars only
  - Codabar: Numeric + dash/colon/slash/plus only
- Check the symbology documentation for character restrictions

### Issue: Barcode Too Small or Not Visible

**Solution:**
1. Set explicit `HeightRequest` and `WidthRequest` (e.g., 150-350)
2. For 2D codes (QR, Data Matrix), make height and width equal for best results
3. Use the `Module` property to control bar/dot size:

```csharp
barcode.Symbology = new QRCode() { Module = 2 };
```

### Issue: NuGet Package Installation Fails

**Solution:**
1. Check internet connection
2. Clear NuGet cache: `dotnet nuget locals all --clear`
3. Verify package source includes NuGet.org
4. Try manual installation: `dotnet add package Syncfusion.Maui.Barcode --version <version>`

## Next Steps

Now that you have a basic barcode set up, explore:

1. **[One-Dimensional Barcodes](one-dimensional-barcodes.md)** - Learn about all 11 linear barcode types
2. **[Two-Dimensional Barcodes](two-dimensional-barcodes.md)** - QR Code and Data Matrix configuration
3. **[Customization](customization.md)** - Style barcodes with colors, fonts, and sizing
4. **[API Reference](api-reference.md)** - Complete property and method documentation

## Additional Resources

- [Video Tutorial](https://www.youtube.com/watch?v=WwdtIotODpE) - Getting started with .NET MAUI Barcode Generator
- [GitHub Samples](https://github.com/syncfusion/maui-demos/tree/master/MAUI/Barcode) - Example code and demos
- [API Documentation](https://help.syncfusion.com/cr/maui/Syncfusion.Maui.Barcode.html) - Complete API reference
- [Feature Tour](https://www.syncfusion.com/maui-controls/maui-barcodes) - Overview of all features
