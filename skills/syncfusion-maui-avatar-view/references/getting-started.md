# Getting Started with .NET MAUI Avatar View

This guide walks you through setting up and configuring the Syncfusion .NET MAUI Avatar View (SfAvatarView) in your application.

## Step 1: Create a New .NET MAUI Project

### Using Visual Studio

1. Open Visual Studio
2. Select **File → New → Project**
3. Search for "**.NET MAUI App**" template
4. Select the template and click **Next**
5. Configure your project:
   - **Name:** Your project name (e.g., "AvatarViewDemo")
   - **Location:** Choose your preferred directory
   - Click **Next**
6. Select the .NET framework version (.NET 9 or later)
7. Click **Create**

### Using CLI

```bash
dotnet new maui -n AvatarViewDemo
cd AvatarViewDemo
```

## Step 2: Install Syncfusion MAUI Core NuGet Package

### Using Visual Studio

1. In **Solution Explorer**, right-click your project
2. Select **Manage NuGet Packages**
3. Click the **Browse** tab
4. Search for **Syncfusion.Maui.Core**
5. Select the package from the results
6. Click **Install** (install the latest stable version)
7. Accept the license agreement if prompted
8. Wait for package restoration to complete

### Using Package Manager Console

```powershell
Install-Package Syncfusion.Maui.Core
```

### Using .NET CLI

```bash
dotnet add package Syncfusion.Maui.Core
```

### Verify Installation

Check your `.csproj` file to confirm the package reference:

```xml
<ItemGroup>
  <PackageReference Include="Syncfusion.Maui.Core" Version="33.1.44" />
</ItemGroup>
```

## Step 3: Register the Syncfusion Handler

The Syncfusion.Maui.Core package requires handler registration in your `MauiProgram.cs` file.

### Open MauiProgram.cs

Locate the file in your project root.

### Add Using Directive

At the top of the file, add:

```csharp
using Syncfusion.Maui.Core.Hosting;
```

### Register the Handler

In the `CreateMauiApp()` method, add `.ConfigureSyncfusionCore()` to the builder chain:

```csharp
using Microsoft.Extensions.Logging;
using Syncfusion.Maui.Core.Hosting;

namespace AvatarViewDemo
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

**Important:** Place `.ConfigureSyncfusionCore()` after `.UseMauiApp<App>()` and before `.ConfigureFonts()`.

## Step 4: Add Avatar View to Your Page

### Import the Namespace

In your XAML file (e.g., `MainPage.xaml`), add the namespace declaration:

```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sfavatar="clr-namespace:Syncfusion.Maui.Core;assembly=Syncfusion.Maui.Core"
             x:Class="AvatarViewDemo.MainPage">
    
    <!-- Your content here -->
    
</ContentPage>
```

### Create a Basic Avatar View

Add the SfAvatarView control in your layout:

```xaml
<ContentPage xmlns:sfavatar="clr-namespace:Syncfusion.Maui.Core;assembly=Syncfusion.Maui.Core">
    <Grid>
        <sfavatar:SfAvatarView 
            HorizontalOptions="Center"
            VerticalOptions="Center" />
    </Grid>
</ContentPage>
```

This creates a default avatar view with the built-in vector image.

## Step 5: Add a Custom Image

To display a custom user image, you need to add an image file to your project and reference it.

### Add Image to Project

1. Add your image file (e.g., `user_profile.png`) to the `Resources/Images` folder
2. Ensure the build action is set to **MauiImage**
3. The image will be automatically processed by MAUI

### Display the Image

**XAML:**
```xaml
<sfavatar:SfAvatarView 
    ContentType="Custom"
    ImageSource="user_profile.png"
    WidthRequest="100"
    HeightRequest="100"
    CornerRadius="50"
    Stroke="Black"
    StrokeThickness="2"
    HorizontalOptions="Center"
    VerticalOptions="Center" />
```

**C#:**
```csharp
using Syncfusion.Maui.Core;

namespace AvatarViewDemo
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            
            var avatarView = new SfAvatarView
            {
                ContentType = ContentType.Custom,
                ImageSource = "user_profile.png",
                WidthRequest = 100,
                HeightRequest = 100,
                CornerRadius = 50,
                Stroke = Colors.Black,
                StrokeThickness = 2,
                HorizontalOptions = LayoutOptions.Center,
                VerticalOptions = LayoutOptions.Center
            };
            
            Content = avatarView;
        }
    }
}
```

## Complete Example: MainPage.xaml

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sfavatar="clr-namespace:Syncfusion.Maui.Core;assembly=Syncfusion.Maui.Core"
             x:Class="AvatarViewDemo.MainPage">

    <ScrollView>
        <VerticalStackLayout Spacing="25" Padding="30">
            
            <!-- Default Avatar -->
            <Label Text="Default Avatar" FontSize="18" FontAttributes="Bold" />
            <sfavatar:SfAvatarView 
                ContentType="Default"
                Background="CornflowerBlue"
                WidthRequest="80"
                HeightRequest="80"
                CornerRadius="40"
                HorizontalOptions="Center" />
            
            <!-- Custom Image Avatar -->
            <Label Text="Custom Image" FontSize="18" FontAttributes="Bold" />
            <sfavatar:SfAvatarView 
                ContentType="Custom"
                ImageSource="user_profile.png"
                WidthRequest="100"
                HeightRequest="100"
                CornerRadius="50"
                Stroke="Gray"
                StrokeThickness="2"
                HorizontalOptions="Center" />
            
            <!-- Initials Avatar -->
            <Label Text="Initials Avatar" FontSize="18" FontAttributes="Bold" />
            <sfavatar:SfAvatarView 
                ContentType="Initials"
                InitialsType="DoubleCharacter"
                AvatarName="John Doe"
                Background="OrangeRed"
                InitialsColor="White"
                WidthRequest="80"
                HeightRequest="80"
                CornerRadius="40"
                HorizontalOptions="Center" />
            
        </VerticalStackLayout>
    </ScrollView>

</ContentPage>
```

## Troubleshooting

### Issue: Handler Not Registered Error

**Error Message:**
```
Handler not registered for type Syncfusion.Maui.Core.SfAvatarView
```

**Solution:**
Ensure `.ConfigureSyncfusionCore()` is called in `MauiProgram.cs`:

```csharp
builder.UseMauiApp<App>()
       .ConfigureSyncfusionCore()  // Must be present
```

### Issue: Image Not Displaying

**Possible Causes:**
1. Image file not in `Resources/Images` folder
2. Build action not set to **MauiImage**
3. Incorrect file name or path

**Solution:**
- Verify the image exists in `Resources/Images/`
- Check the file name matches exactly (case-sensitive on some platforms)
- Rebuild the project

### Issue: NuGet Package Not Found

**Solution:**
- Check your NuGet package source settings
- Ensure you have internet connectivity
- Try clearing NuGet cache: `dotnet nuget locals all --clear`
- Verify package name: **Syncfusion.Maui.Core** (not Syncfusion.MAUI.Core)

### Issue: Namespace Not Found

**Error Message:**
```
The type or namespace name 'Syncfusion' could not be found
```

**Solution:**
- Verify the NuGet package is installed
- Clean and rebuild the solution
- Check the using directive: `using Syncfusion.Maui.Core;`
- Restart Visual Studio if necessary

## Best Practices

1. **Always register the handler** - Don't skip the `.ConfigureSyncfusionCore()` call
2. **Use appropriate image sizes** - Don't use unnecessarily large images for small avatars
3. **Test on multiple platforms** - Avatar rendering may vary slightly between platforms
4. **Consider fallbacks** - Provide initials or default avatars when images fail to load
5. **Optimize image resources** - Use compressed images to reduce app size

