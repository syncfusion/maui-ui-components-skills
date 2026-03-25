# Getting Started with .NET MAUI Color Picker

This guide walks through the complete setup and basic implementation of the Syncfusion .NET MAUI Color Picker (SfColorPicker) control.

## Installation Steps

### Step 1: Create or Open .NET MAUI Project

#### For New Projects:

**Visual Studio:**
1. Go to **File > New > Project**
2. Select **.NET MAUI App** template
3. Name your project and choose a location
4. Click **Next**, select .NET framework version
5. Click **Create**

**CLI:**
```bash
dotnet new maui -n MyColorPickerApp
cd MyColorPickerApp
```

### Step 2: Install Syncfusion MAUI Inputs NuGet Package

The Color Picker is part of the `Syncfusion.Maui.Inputs` package.

#### Option 1: Visual Studio NuGet Package Manager

1. Right-click the project in **Solution Explorer**
2. Select **Manage NuGet Packages**
3. Search for `Syncfusion.Maui.Inputs`
4. Select the latest stable version
5. Click **Install**
6. Accept license agreements
7. Wait for dependencies to install and project to restore

#### Option 2: Package Manager Console

```powershell
Install-Package Syncfusion.Maui.Inputs
```

#### Option 3: .NET CLI

```bash
dotnet add package Syncfusion.Maui.Inputs
```

#### Option 4: Edit .csproj Directly

```xml
<ItemGroup>
  <PackageReference Include="Syncfusion.Maui.Inputs" Version="33.1.44" />
</ItemGroup>
```

Then restore packages:
```bash
dotnet restore
```

**Note:** The `Syncfusion.Maui.Core` package is automatically installed as a dependency and is required for all Syncfusion .NET MAUI controls.

### Step 3: Register Syncfusion Handler

Open `MauiProgram.cs` and register the Syncfusion core handler:

```csharp
using Microsoft.Extensions.Logging;
using Syncfusion.Maui.Core.Hosting;

namespace ColorPickerGettingStarted
{
    public static class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            var builder = MauiApp.CreateBuilder();
            builder
                .UseMauiApp<App>()
                .ConfigureSyncfusionCore()  // ← Add this line
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

**Important:** The `.ConfigureSyncfusionCore()` call is mandatory for all Syncfusion .NET MAUI controls to work properly.

## Basic Implementation

### Step 4: Add Namespace

Import the Syncfusion Inputs namespace in your XAML or C# file.

#### XAML:

```xaml
<ContentPage 
    xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    xmlns:inputs="clr-namespace:Syncfusion.Maui.Inputs;assembly=Syncfusion.Maui.Inputs"
    xmlns:core="clr-namespace:Syncfusion.Maui.Core;assembly=Syncfusion.Maui.Core"
    x:Class="ColorPickerGettingStarted.MainPage">
    
    <!-- Content here -->
    
</ContentPage>
```

#### C#:

```csharp
using Syncfusion.Maui.Inputs;
```

### Step 5: Create Basic Color Picker

#### XAML Implementation:

```xaml
<ContentPage 
    xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    xmlns:inputs="clr-namespace:Syncfusion.Maui.Inputs;assembly=Syncfusion.Maui.Inputs"
    xmlns:core="clr-namespace:Syncfusion.Maui.Core;assembly=Syncfusion.Maui.Core"
    x:Class="ColorPickerGettingStarted.MainPage">

    <ContentPage.Content>
        <inputs:SfColorPicker x:Name="colorPicker"/>
    </ContentPage.Content>

</ContentPage>
```

#### C# Implementation:

```csharp
using Syncfusion.Maui.Inputs;

namespace ColorPickerGettingStarted
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            
            SfColorPicker colorPicker = new SfColorPicker();
            Content = colorPicker;
        }
    }
}
```

## Verification

After implementing the basic setup:

1. **Build the project**:
   ```bash
   dotnet build
   ```

2. **Run on your target platform**:
   - Android emulator/device
   - iOS simulator/device
   - Windows desktop
   - macOS desktop

3. **Expected Result**: A color picker button appears that opens a popup with spectrum view when clicked.

## Example Output

When you run the application, you should see:
- A compact button showing the currently selected color
- Click/tap the button to open the color picker popup
- Default spectrum mode with gradient color selection
- HEX/RGB input fields
- Alpha slider for transparency control
- Apply and Cancel buttons

## Complete Working Example

Here's a complete, working example with a preview:

### XAML:

```xaml
<ContentPage 
    xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    xmlns:inputs="clr-namespace:Syncfusion.Maui.Inputs;assembly=Syncfusion.Maui.Inputs"
    xmlns:core="clr-namespace:Syncfusion.Maui.Core;assembly=Syncfusion.Maui.Core"
    x:Class="ColorPickerGettingStarted.MainPage">

    <Grid Padding="20" RowDefinitions="Auto,*">
        
        <!-- Color Picker -->
        <inputs:SfColorPicker x:Name="colorPicker" 
                              Grid.Row="0"
                              SelectedColor="DodgerBlue"
                              ColorChanged="OnColorChanged"/>
        
        <!-- Color Preview -->
        <VerticalStackLayout Grid.Row="1" Margin="0,20,0,0">
            <Label Text="Selected Color Preview:" 
                   FontSize="16" 
                   Margin="0,0,0,10"/>
            <BoxView x:Name="previewBox"
                     BackgroundColor="DodgerBlue"
                     HeightRequest="150"
                     CornerRadius="10"/>
            <Label x:Name="colorLabel"
                   Text="#1E90FF"
                   FontSize="18"
                   FontAttributes="Bold"
                   HorizontalOptions="Center"
                   Margin="0,10,0,0"/>
        </VerticalStackLayout>
        
    </Grid>

</ContentPage>
```

### Code-behind:

```csharp
using Syncfusion.Maui.Inputs;

namespace ColorPickerGettingStarted
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
        }

        private void OnColorChanged(object sender, ColorChangedEventArgs e)
        {
            // Update preview box color
            previewBox.BackgroundColor = e.NewColor;
            
            // Update color label with HEX value
            colorLabel.Text = e.NewColor.ToHex();
        }
    }
}
```

## Troubleshooting

### Issue: "Handler not found" error

**Solution:** Ensure `.ConfigureSyncfusionCore()` is called in `MauiProgram.cs`:

```csharp
builder
    .UseMauiApp<App>()
    .ConfigureSyncfusionCore()  // Must be here
```

### Issue: NuGet package not restoring

**Solution:** Clean and restore:
```bash
dotnet clean
dotnet restore
dotnet build
```

### Issue: Namespace not found

**Solution:** Verify package installation:
```bash
dotnet list package | grep Syncfusion
```

Should show:
```
Syncfusion.Maui.Inputs    [version]
Syncfusion.Maui.Core      [version]
```

### Issue: Control not appearing

**Solution:** Check:
1. Handler registration in `MauiProgram.cs`
2. Namespace import is correct
3. Package is installed and restored
4. Build is successful without errors
