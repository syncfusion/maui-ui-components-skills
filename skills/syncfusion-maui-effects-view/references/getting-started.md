# Getting Started with .NET MAUI Effects View

Learn how to install, configure, and implement the Syncfusion .NET MAUI Effects View (SfEffectsView) in your application.

## Overview

The SfEffectsView is a container control that adds modern visual effects to any MAUI view through touch interactions. It provides effects like ripple, highlight, selection, scaling, and rotation that render based on user interaction or programmatic triggers.

## Step 1: Create a .NET MAUI Project

### Using Visual Studio:

1. Open Visual Studio
2. Go to **File > New > Project**
3. Search for ".NET MAUI App" template
4. Select the template and click **Next**
5. Enter project name (e.g., "EffectsViewDemo")
6. Choose a location and click **Next**
7. Select the .NET framework version (9.0 or later)
8. Click **Create**

### Using CLI:

```bash
dotnet new maui -n EffectsViewDemo
cd EffectsViewDemo
```

## Step 2: Install Syncfusion.Maui.Core NuGet Package

The SfEffectsView control is part of the **Syncfusion.Maui.Core** package.

### Option A: Visual Studio NuGet Package Manager

1. In **Solution Explorer**, right-click the project
2. Select **Manage NuGet Packages**
3. Click the **Browse** tab
4. Search for `Syncfusion.Maui.Core`
5. Select the package and click **Install**
6. Accept the license agreement
7. Wait for installation to complete

### Option B: Package Manager Console

```powershell
Install-Package Syncfusion.Maui.Core
```

### Option C: .NET CLI

```bash
dotnet add package Syncfusion.Maui.Core
```

### Option D: Manual .csproj Edit

Add to your `.csproj` file:

```xml
<ItemGroup>
  <PackageReference Include="Syncfusion.Maui.Core" Version="27.*" />
</ItemGroup>
```

Then restore packages:
```bash
dotnet restore
```

## Step 3: Register the Handler

The Syncfusion.Maui.Core NuGet is a dependency for all Syncfusion .NET MAUI controls. You must register the handler in `MauiProgram.cs`.

### Edit MauiProgram.cs:

```csharp
using Microsoft.Extensions.Logging;
using Syncfusion.Maui.Core.Hosting;  // ← Add this using

namespace EffectsViewDemo
{
    public static class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            var builder = MauiApp.CreateBuilder();
            builder
                .UseMauiApp<App>()
                .ConfigureSyncfusionCore()  // ← Register Syncfusion handlers
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

**Important:** The `ConfigureSyncfusionCore()` call is required. Without it, the control will not render properly and you may see errors at runtime.

## Step 4: Add SfEffectsView to Your Page

### XAML Implementation

1. Open `MainPage.xaml`
2. Add the namespace declaration
3. Add the SfEffectsView control

**MainPage.xaml:**

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:effectsView="clr-namespace:Syncfusion.Maui.Core;assembly=Syncfusion.Maui.Core"
             x:Class="EffectsViewDemo.MainPage">

    <VerticalStackLayout Padding="30" Spacing="25">
        
        <Label Text="Tap the button below to see the ripple effect"
               FontSize="18"
               HorizontalOptions="Center" />

        <effectsView:SfEffectsView HorizontalOptions="Center"
                                    VerticalOptions="Center"
                                    TouchDownEffects="Ripple">
            <Border BackgroundColor="#6200EE"
                    Padding="40,15"
                    StrokeThickness="0">
                <Border.StrokeShape>
                    <RoundRectangle CornerRadius="8" />
                </Border.StrokeShape>
                <Label Text="TAP ME"
                       TextColor="White"
                       FontSize="16"
                       FontAttributes="Bold"
                       HorizontalOptions="Center"
                       VerticalOptions="Center" />
            </Border>
        </effectsView:SfEffectsView>
        
    </VerticalStackLayout>

</ContentPage>
```

### C# Implementation

Alternatively, you can create the control entirely in code-behind:

**MainPage.xaml.cs:**

```csharp
using Syncfusion.Maui.Core;

namespace EffectsViewDemo
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            CreateEffectsView();
        }

        private void CreateEffectsView()
        {
            // Create the effects view
            var effectsView = new SfEffectsView
            {
                TouchDownEffects = SfEffects.Ripple,
                HorizontalOptions = LayoutOptions.Center,
                VerticalOptions = LayoutOptions.Center
            };

            // Create the content (a styled button)
            var border = new Border
            {
                BackgroundColor = Color.FromArgb("#6200EE"),
                Padding = new Thickness(40, 15),
                StrokeThickness = 0,
                StrokeShape = new RoundRectangle { CornerRadius = 8 },
                Content = new Label
                {
                    Text = "TAP ME",
                    TextColor = Colors.White,
                    FontSize = 16,
                    FontAttributes = FontAttributes.Bold,
                    HorizontalOptions = LayoutOptions.Center,
                    VerticalOptions = LayoutOptions.Center
                }
            };

            // Set the border as the effects view content
            effectsView.Content = border;

            // Add to page layout
            var layout = new VerticalStackLayout
            {
                Padding = new Thickness(30),
                Spacing = 25
            };

            layout.Add(new Label
            {
                Text = "Tap the button below to see the ripple effect",
                FontSize = 18,
                HorizontalOptions = LayoutOptions.Center
            });

            layout.Add(effectsView);

            Content = layout;
        }
    }
}
```

## Adding Content to SfEffectsView

The SfEffectsView is a container control. You can wrap any MAUI view or layout:

### Wrapping an Image

```xaml
<effectsView:SfEffectsView TouchDownEffects="Ripple">
    <Image Source="profile.png" 
           WidthRequest="100" 
           HeightRequest="100" />
</effectsView:SfEffectsView>
```

### Wrapping a Label

```xaml
<effectsView:SfEffectsView TouchDownEffects="Highlight"
                            HighlightBackground="#E0E0E0">
    <Label Text="Tap this text" 
           FontSize="20"
           Padding="20" />
</effectsView:SfEffectsView>
```

### Wrapping a Complex Layout

```xaml
<effectsView:SfEffectsView TouchDownEffects="Ripple">
    <Grid Padding="15" BackgroundColor="White">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="60" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>

        <Image Grid.RowSpan="2" 
               Source="avatar.png" 
               WidthRequest="50" 
               HeightRequest="50"
               Margin="5" />
        
        <Label Grid.Column="1" 
               Text="John Doe" 
               FontSize="16" 
               FontAttributes="Bold" />
        
        <Label Grid.Row="1" 
               Grid.Column="1" 
               Text="Software Engineer" 
               FontSize="12" 
               TextColor="Gray" />
    </Grid>
</effectsView:SfEffectsView>
```

## Basic Ripple Effect Example

The ripple effect is the most popular effect, providing Material Design-style feedback:

```xaml
<Border HorizontalOptions="Center" 
        VerticalOptions="Center">
    <Border.StrokeShape>
        <RoundRectangle CornerRadius="18" />
    </Border.StrokeShape>
    <Border.Background>
        <LinearGradientBrush EndPoint="1,0">
            <GradientStop Color="#4E54C8" Offset="0.0" />
            <GradientStop Color="#8F94FB" Offset="1.0" />
        </LinearGradientBrush>
    </Border.Background>
    
    <effectsView:SfEffectsView>
        <Grid>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="90" />
                <ColumnDefinition Width="*"/>
            </Grid.ColumnDefinitions>

            <Image Source="profile.png" 
                   Margin="7" 
                   VerticalOptions="Center"
                   WidthRequest="72" 
                   HeightRequest="72" />
            
            <StackLayout Grid.Column="1" 
                         VerticalOptions="Center">
                <Label Text="Laura Steffi" 
                       Margin="10,0,10,0" 
                       FontSize="18"
                       TextColor="White" />
                <Label Text="Data Science Analyst" 
                       Margin="10,0,10,0" 
                       FontSize="12"
                       TextColor="White" />
            </StackLayout>
        </Grid>
    </effectsView:SfEffectsView>
</Border>
```

**Result:** When you tap anywhere on the card, you'll see a ripple animation emanating from the touch point.

## Common Getting Started Issues

### Issue 1: Handler Not Registered Error

**Error:** `Handler not found for view Syncfusion.Maui.Core.SfEffectsView`

**Solution:** Ensure you've called `ConfigureSyncfusionCore()` in `MauiProgram.cs`:

```csharp
builder
    .UseMauiApp<App>()
    .ConfigureSyncfusionCore()  // ← This line is required
```

### Issue 2: Effects Not Appearing

**Problem:** Control renders but no effects show on tap.

**Solutions:**
- Verify `TouchDownEffects`, `TouchUpEffects`, or `LongPressEffects` is set (not `None`)
- Ensure the control has content (a child view)
- Check that the parent container isn't clipping the effect (`ClipToBounds="False"`)

### Issue 3: NuGet Package Restore Fails

**Solution:** Run restore manually:

```bash
dotnet restore
```

Or in Visual Studio: Right-click solution → **Restore NuGet Packages**

### Issue 4: Namespace Not Found

**Error:** `The type or namespace name 'Syncfusion' could not be found`

**Solution:** Ensure:
1. NuGet package is installed
2. Using directive is present: `using Syncfusion.Maui.Core;` (C#)
3. Namespace declaration is correct: `xmlns:effectsView="clr-namespace:Syncfusion.Maui.Core;assembly=Syncfusion.Maui.Core"` (XAML)
