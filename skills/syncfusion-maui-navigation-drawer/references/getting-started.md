# Getting Started with Navigation Drawer

## Table of Contents
- [Step 1: Install NuGet Package](#step-1-install-nuget-package)
- [Step 2: Register Syncfusion Handler](#step-2-register-syncfusion-handler)
- [Step 3: Import Navigation Drawer Namespace](#step-3-import-navigation-drawer-namespace)
- [Step 4: Create Basic Navigation Drawer](#step-4-create-basic-navigation-drawer)
- [Step 5: Configure Drawer Size](#step-5-configure-drawer-size)
- [Step 6: Add Hamburger Menu for Toggling](#step-6-add-hamburger-menu-for-toggling)
- [Step 7: Add Drawer Content](#step-7-add-drawer-content)
- [Common Issues and Solutions](#common-issues-and-solutions)
- [Quick Reference](#quick-reference)
- [Next Steps](#next-steps)
- [Sample Code](#sample-code)

This guide covers the complete setup process for implementing the Syncfusion .NET MAUI Navigation Drawer (SfNavigationDrawer) in your application, from NuGet installation to creating your first working navigation drawer with a hamburger menu.

## Step 1: Install NuGet Package

### Via NuGet Package Manager (Visual Studio)

1. Right-click your project in **Solution Explorer**
2. Select **Manage NuGet Packages**
3. Search for **Syncfusion.Maui.NavigationDrawer**
4. Install the latest version
5. Ensure dependencies are restored

### Via Package Manager Console

```powershell
Install-Package Syncfusion.Maui.NavigationDrawer
```

### Via .NET CLI

```bash
dotnet add package Syncfusion.Maui.NavigationDrawer
```

## Step 2: Register Syncfusion Handler

The `Syncfusion.Maui.Core` NuGet is a required dependency. Register the Syncfusion handler in your `MauiProgram.cs` file:

```csharp
using Microsoft.Extensions.Logging;
using Syncfusion.Maui.Core.Hosting;

namespace NavigationDrawerGettingStarted
{
    public static class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            var builder = MauiApp.CreateBuilder();
            builder
                .UseMauiApp<App>()
                .ConfigureSyncfusionCore()  // ← Register Syncfusion handler
                .ConfigureFonts(fonts =>
                {
                    fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
                });

            return builder.Build();
        }
    }
}
```

**⚠️ Critical:** Always call `.ConfigureSyncfusionCore()` before other configurations.

## Step 3: Import Navigation Drawer Namespace

Add the NavigationDrawer namespace to your XAML or C# file.

### XAML

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:navigationDrawer="clr-namespace:Syncfusion.Maui.NavigationDrawer;assembly=Syncfusion.Maui.NavigationDrawer"
             x:Class="YourApp.MainPage">
    <!-- Content here -->
</ContentPage>
```

### C#

```csharp
using Syncfusion.Maui.NavigationDrawer;
```

## Step 4: Create Basic Navigation Drawer

### Minimal Implementation

The simplest drawer requires only a `ContentView` (mandatory):

**XAML:**

```xml
<navigationDrawer:SfNavigationDrawer x:Name="navigationDrawer">
    <navigationDrawer:SfNavigationDrawer.ContentView>
        <Grid>
            <Label Text="Main Content"
                   HorizontalOptions="Center"
                   VerticalOptions="Center"/>
        </Grid>
    </navigationDrawer:SfNavigationDrawer.ContentView>
</navigationDrawer:SfNavigationDrawer>
```

**C#:**

```csharp
SfNavigationDrawer navigationDrawer = new SfNavigationDrawer();
Grid grid = new Grid();
grid.Children.Add(new Label 
{ 
    Text = "Main Content",
    HorizontalOptions = LayoutOptions.Center,
    VerticalOptions = LayoutOptions.Center
});
navigationDrawer.ContentView = grid;
this.Content = navigationDrawer;
```

**⚠️ Important:** ContentView is **mandatory** when initializing SfNavigationDrawer. The app will not function without it.

## Step 5: Configure Drawer Size

Set the drawer width to make it visible when opened:

```xml
<navigationDrawer:SfNavigationDrawer x:Name="navigationDrawer">
    <navigationDrawer:SfNavigationDrawer.DrawerSettings>
        <navigationDrawer:DrawerSettings DrawerWidth="250"/>
    </navigationDrawer:SfNavigationDrawer.DrawerSettings>
    <navigationDrawer:SfNavigationDrawer.ContentView>
        <Grid/>
    </navigationDrawer:SfNavigationDrawer.ContentView>
</navigationDrawer:SfNavigationDrawer>
```

**C#:**

```csharp
navigationDrawer.DrawerSettings = new DrawerSettings
{
    DrawerWidth = 250
};
```

**💡 Note:** 
- Use `DrawerWidth` for Left/Right positions (default: Left)
- Use `DrawerHeight` for Top/Bottom positions
- Default position is Left if not specified

## Step 6: Add Hamburger Menu for Toggling

Create an interactive hamburger button to open/close the drawer.

### Complete XAML Example

```xml
<navigationDrawer:SfNavigationDrawer x:Name="navigationDrawer">
    <navigationDrawer:SfNavigationDrawer.DrawerSettings>
        <navigationDrawer:DrawerSettings DrawerWidth="250"/>
    </navigationDrawer:SfNavigationDrawer.DrawerSettings>
    
    <navigationDrawer:SfNavigationDrawer.ContentView>
        <Grid x:Name="mainContentView" 
              BackgroundColor="White" 
              RowDefinitions="Auto,*">
            
            <!-- Header with Hamburger Button -->
            <HorizontalStackLayout BackgroundColor="#6750A4" 
                                   Spacing="10" 
                                   Padding="5,0,0,0">
                <ImageButton x:Name="hamburgerButton"
                             HeightRequest="50"
                             WidthRequest="50"
                             HorizontalOptions="Start"
                             Source="hamburgericon.png"
                             BackgroundColor="#6750A4"
                             Clicked="hamburgerButton_Clicked"/>
                <Label x:Name="headerLabel" 
                       HeightRequest="50" 
                       HorizontalTextAlignment="Center" 
                       VerticalTextAlignment="Center" 
                       Text="Home" 
                       FontSize="16" 
                       TextColor="White" 
                       BackgroundColor="#6750A4"/>
            </HorizontalStackLayout>
            
            <!-- Main Content Area -->
            <Label Grid.Row="1" 
                   x:Name="contentLabel" 
                   VerticalOptions="Center" 
                   HorizontalOptions="Center" 
                   Text="Content View" 
                   FontSize="14" 
                   TextColor="Black"/>
        </Grid>
    </navigationDrawer:SfNavigationDrawer.ContentView>
</navigationDrawer:SfNavigationDrawer>
```

### Code-Behind Handler

```csharp
private void hamburgerButton_Clicked(object sender, EventArgs e)
{
    navigationDrawer.ToggleDrawer();
}
```

**💡 Asset Setup:** Place `hamburgericon.png` in your `Resources/Images` directory.

### Complete C# Implementation

```csharp
namespace NavigationDrawerGettingStarted;

public partial class MainPage : ContentPage
{
    SfNavigationDrawer navigationDrawer;
    
    public MainPage()
    {
        InitializeComponent();
        
        // Create drawer
        navigationDrawer = new SfNavigationDrawer();
        
        // Configure drawer settings
        navigationDrawer.DrawerSettings = new DrawerSettings
        {
            DrawerWidth = 250
        };
        
        // Create main content grid
        var grid = new Grid
        {
            RowDefinitions =
            {
                new RowDefinition { Height = new GridLength(1, GridUnitType.Auto) },
                new RowDefinition()
            },
            BackgroundColor = Colors.White
        };
        
        // Create header with hamburger button
        var layout = new HorizontalStackLayout
        { 
            BackgroundColor = Color.FromArgb("#6750A4"),
            Spacing = 10,
            Padding = new Thickness(5, 0, 0, 0)
        };
        
        var hamburgerButton = new ImageButton
        {
            HeightRequest = 50,
            WidthRequest = 50,
            HorizontalOptions = LayoutOptions.Start,
            BackgroundColor = Color.FromArgb("#6750A4"),
            Source = "hamburgericon.png"
        };
        hamburgerButton.Clicked += (s, e) => navigationDrawer.ToggleDrawer();
        
        var headerLabel = new Label
        {
            HeightRequest = 50,
            HorizontalTextAlignment = TextAlignment.Center,
            VerticalTextAlignment = TextAlignment.Center,
            Text = "Home",
            FontSize = 16,
            TextColor = Colors.White,
            BackgroundColor = Color.FromArgb("#6750A4")
        };
        
        layout.Children.Add(hamburgerButton);
        layout.Children.Add(headerLabel);
        
        // Create content label
        var contentLabel = new Label
        {
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.Center,
            Text = "Content View",
            FontSize = 14,
            TextColor = Colors.Black
        };
        
        grid.SetRow(layout, 0);
        grid.SetRow(contentLabel, 1);
        grid.Children.Add(layout);
        grid.Children.Add(contentLabel);
        
        navigationDrawer.ContentView = grid;
        this.Content = navigationDrawer;
    }
}
```

## Step 7: Add Drawer Content

Complete the drawer by adding header and content views.

```xml
<navigationDrawer:SfNavigationDrawer x:Name="navigationDrawer">
    <navigationDrawer:SfNavigationDrawer.DrawerSettings>
        <navigationDrawer:DrawerSettings DrawerWidth="250"
                                         DrawerHeaderHeight="160">
            
            <!-- Drawer Header -->
            <navigationDrawer:DrawerSettings.DrawerHeaderView>
                <Grid BackgroundColor="#6750A4" RowDefinitions="120,40">
                    <Image Source="user.png"
                           HeightRequest="110"
                           Margin="0,10,0,0"
                           BackgroundColor="#6750A4"
                           VerticalOptions="Center"
                           HorizontalOptions="Center"/>
                    <Label Text="James Pollock"
                           Grid.Row="1"
                           HorizontalTextAlignment="Center"
                           HorizontalOptions="Center"
                           FontSize="20"
                           TextColor="White"/>
                </Grid>
            </navigationDrawer:DrawerSettings.DrawerHeaderView>
            
            <!-- Drawer Content (Menu Items) -->
            <navigationDrawer:DrawerSettings.DrawerContentView>
                <CollectionView x:Name="collectionView" 
                                SelectionMode="Single"
                                SelectionChanged="collectionView_SelectionChanged">
                    <CollectionView.ItemsSource>
                        <x:Array Type="{x:Type x:String}">
                            <x:String>Home</x:String>
                            <x:String>Profile</x:String>
                            <x:String>Inbox</x:String>
                            <x:String>Outbox</x:String>
                            <x:String>Sent</x:String>
                            <x:String>Draft</x:String>
                        </x:Array>
                    </CollectionView.ItemsSource>
                    <CollectionView.ItemTemplate>
                        <DataTemplate>
                            <VerticalStackLayout HeightRequest="40">
                                <Label Margin="10,7,0,0"
                                       Text="{Binding}"
                                       FontSize="16"
                                       TextColor="Black"/>
                            </VerticalStackLayout>
                        </DataTemplate>
                    </CollectionView.ItemTemplate>
                </CollectionView>
            </navigationDrawer:DrawerSettings.DrawerContentView>
        </navigationDrawer:DrawerSettings>
    </navigationDrawer:SfNavigationDrawer.DrawerSettings>
    
    <!-- (ContentView from previous step) -->
</navigationDrawer:SfNavigationDrawer>
```

### Handle Menu Selection

```csharp
private void collectionView_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (e.CurrentSelection.FirstOrDefault() is string selectedItem)
    {
        // Update header text
        headerLabel.Text = selectedItem;
        
        // Update content
        contentLabel.Text = $"{selectedItem} Content";
        
        // Close drawer after selection
        navigationDrawer.ToggleDrawer();
    }
}
```

## Common Issues and Solutions

### Issue: "ContentView cannot be null"
**Solution:** Always set ContentView property before displaying the drawer. It's mandatory.

```csharp
// ✓ Correct
navigationDrawer.ContentView = new Grid();

// ✗ Wrong - will throw exception
this.Content = navigationDrawer; // ContentView not set
```

### Issue: Drawer not visible when opened
**Solution:** Set DrawerWidth (for left/right) or DrawerHeight (for top/bottom).

```csharp
navigationDrawer.DrawerSettings = new DrawerSettings
{
    DrawerWidth = 250  // Must be set for visibility
};
```

### Issue: Hamburger icon not showing
**Solution:** Ensure image file is in Resources/Images folder with proper build action.

1. Add `hamburgericon.png` to `Resources/Images/`
2. Build action should be **MauiImage**
3. Reference as `Source="hamburgericon.png"`

### Issue: ToggleDrawer() does nothing
**Solution:** Ensure DrawerSettings is properly configured before calling toggle methods.

```csharp
// Configure drawer first
navigationDrawer.DrawerSettings = new DrawerSettings { DrawerWidth = 250 };

// Then toggle will work
navigationDrawer.ToggleDrawer();
```

## Quick Reference

### Minimum Required Code (XAML)

```xml
<navigationDrawer:SfNavigationDrawer x:Name="navigationDrawer">
    <navigationDrawer:SfNavigationDrawer.ContentView>
        <Grid/>
    </navigationDrawer:SfNavigationDrawer.ContentView>
</navigationDrawer:SfNavigationDrawer>
```

### Minimum Required Code (C#)

```csharp
var drawer = new SfNavigationDrawer
{
    ContentView = new Grid()
};
this.Content = drawer;
```

### Toggle Methods

```csharp
// Method 1: ToggleDrawer()
navigationDrawer.ToggleDrawer();

// Method 2: IsOpen property
navigationDrawer.DrawerSettings.IsOpen = true;  // Open
navigationDrawer.DrawerSettings.IsOpen = false; // Close

// Method 3: Swipe gesture (enabled by default)
// User swipes from screen edge
```
