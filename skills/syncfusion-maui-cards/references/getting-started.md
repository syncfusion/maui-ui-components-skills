# Getting Started with .NET MAUI Cards

This guide covers the installation, and initial setup required to start using Syncfusion .NET MAUI Cards (SfCards) control in your .NET MAUI application.

## Step 1: Create a New .NET MAUI Project

### Using Visual Studio

1. Go to **File > New > Project**
2. Choose the **.NET MAUI App** template
3. Name the project and choose a location
4. Click **Next**
5. Select the .NET framework version (9.0 or later)
6. Click **Create**

### Using CLI

```bash
dotnet new maui -n MyCardApp
cd MyCardApp
```

## Step 2: Install Syncfusion.Maui.Cards NuGet Package

### Using Visual Studio

1. In **Solution Explorer**, right-click the project
2. Choose **Manage NuGet Packages**
3. Search for `Syncfusion.Maui.Cards`
4. Install the latest version
5. Ensure all dependencies are installed correctly

### Using Package Manager Console

```powershell
Install-Package Syncfusion.Maui.Cards
```

### Using .NET CLI

```bash
dotnet add package Syncfusion.Maui.Cards
```

## Step 3: Register the Syncfusion Handler

**CRITICAL:** The `Syncfusion.Maui.Core` NuGet is a dependent package for all Syncfusion controls. You MUST register the handler in `MauiProgram.cs`.

**File:** `MauiProgram.cs`

```csharp
using Syncfusion.Maui.Core.Hosting;

namespace MyCardApp
{
    public static class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            var builder = MauiApp.CreateBuilder();
            
            // Register Syncfusion Core - REQUIRED!
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

**Important:** Without `ConfigureSyncfusionCore()`, the cards control will not work.

## Step 4: Add .NET MAUI Cards Control

### Import the Namespace

Add the namespace to your XAML or C# file:

**XAML:**
```xml
xmlns:cards="clr-namespace:Syncfusion.Maui.Cards;assembly=Syncfusion.Maui.Cards"
```

**C#:**
```csharp
using Syncfusion.Maui.Cards;
```

### Create Your First Card

**XAML Implementation:**

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:cards="clr-namespace:Syncfusion.Maui.Cards;assembly=Syncfusion.Maui.Cards"
             x:Class="MyCardApp.MainPage">

    <cards:SfCardView>
        <Label Text="CardView" 
               Background="PeachPuff" 
               HorizontalTextAlignment="Center" 
               VerticalTextAlignment="Center"/>
    </cards:SfCardView>

</ContentPage>
```

**C# Implementation:**

```csharp
using Syncfusion.Maui.Cards;

namespace MyCardApp
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            
            SfCardView cardView = new SfCardView();
            
            cardView.Content = new Label
            {
                Text = "CardView",
                HorizontalTextAlignment = TextAlignment.Center,
                VerticalTextAlignment = TextAlignment.Center,
                BackgroundColor = Colors.PeachPuff
            };
            
            this.Content = cardView;
        }
    }
}
```

## Basic Card with Swipe-to-Dismiss

Enable swipe-to-dismiss functionality to create a dismissible card:

**XAML:**
```xml
<cards:SfCardView SwipeToDismiss="True">
    <Label Text="Swipe me left or right!" 
           Background="MediumPurple" 
           HorizontalTextAlignment="Center" 
           VerticalTextAlignment="Center"/>
</cards:SfCardView>
```

**C#:**
```csharp
SfCardView cardView = new SfCardView
{
    SwipeToDismiss = true,
    Content = new Label
    {
        Text = "Swipe me left or right!",
        HorizontalTextAlignment = TextAlignment.Center,
        VerticalTextAlignment = TextAlignment.Center,
        BackgroundColor = Colors.MediumPurple
    }
};
```

**Note:** SwipeToDismiss only works for standalone SfCardView, not when it's a child of SfCardLayout.

## Creating a Simple Card Layout (Stack)

Create multiple stacked cards with swipe navigation:

**XAML:**
```xml
<cards:SfCardLayout HeightRequest="500" BackgroundColor="#F0F0F0">

    <cards:SfCardView CornerRadius="10">
        <Label Text="Peach" 
               BackgroundColor="PeachPuff" 
               VerticalTextAlignment="Center" 
               HorizontalTextAlignment="Center"/>
    </cards:SfCardView>

    <cards:SfCardView CornerRadius="10">
        <Label Text="MediumPurple" 
               BackgroundColor="MediumPurple" 
               VerticalTextAlignment="Center" 
               HorizontalTextAlignment="Center"/>
    </cards:SfCardView>

    <cards:SfCardView CornerRadius="10">
        <Label Text="LightPink" 
               BackgroundColor="LightPink" 
               VerticalTextAlignment="Center" 
               HorizontalTextAlignment="Center"/>
    </cards:SfCardView>

</cards:SfCardLayout>
```

**C#:**
```csharp
SfCardLayout cardLayout = new SfCardLayout
{
    HeightRequest = 500,
    BackgroundColor = Color.FromArgb("#F0F0F0")
};

// Add children cards
cardLayout.Children.Add(new SfCardView 
{ 
    Content = new Label 
    { 
        Text = "Peach", 
        BackgroundColor = Colors.PeachPuff, 
        HorizontalTextAlignment = TextAlignment.Center, 
        VerticalTextAlignment = TextAlignment.Center 
    }, 
    CornerRadius = 10 
});

cardLayout.Children.Add(new SfCardView 
{ 
    Content = new Label 
    { 
        Text = "MediumPurple", 
        BackgroundColor = Colors.MediumPurple, 
        HorizontalTextAlignment = TextAlignment.Center, 
        VerticalTextAlignment = TextAlignment.Center 
    },
    CornerRadius = 10 
});

cardLayout.Children.Add(new SfCardView 
{ 
    Content = new Label 
    { 
        Text = "LightPink", 
        BackgroundColor = Colors.LightPink, 
        HorizontalTextAlignment = TextAlignment.Center, 
        VerticalTextAlignment = TextAlignment.Center 
    },
    CornerRadius = 10 
});

this.Content = cardLayout;
```

## Quick Verification

Run your application and verify:

1. **Card appears** - You should see the card with your content
2. **Swipe works** (if enabled) - Swipe left or right to dismiss
3. **Stack navigation** (for CardLayout) - Swipe to reveal next card

## Troubleshooting Common Issues

### Issue: Cards not appearing

**Solution:** Ensure you called `ConfigureSyncfusionCore()` in MauiProgram.cs

### Issue: NuGet package not found

**Solution:** Check your NuGet package source includes nuget.org

### Issue: SwipeToDismiss not working in CardLayout

**Explanation:** This is by design. SwipeToDismiss only works for standalone SfCardView, not when it's a child of SfCardLayout. In CardLayout, use SwipeDirection for navigation.

### Issue: Namespace not found

**Solution:** Ensure Syncfusion.Maui.Cards NuGet package is installed and project is restored.

## Next Steps

Now that you have cards working:

- Explore **card-views.md** for single card features
- Explore **card-layouts.md** for card stack features  
- See **customization.md** for styling options
- Check **events.md** for interaction handling
- Review **data-binding.md** for dynamic cards
