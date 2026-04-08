# License Key Registration

Complete guide for registering Syncfusion .NET MAUI license keys in your application.

## Registration Method

All license key registration uses the same method:

```csharp
string licenseKey = Environment.GetEnvironmentVariable("SYNCFUSION_LICENSE_KEY");
if (!string.IsNullOrEmpty(licenseKey))
{   
    Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense(licenseKey);
}
```

**Requirements:**
- Must be called before any Syncfusion control is initialized
- Requires `Syncfusion.Licensing.dll` reference
- License key must be enclosed in double quotes
- Only needs to be called once during application startup

## Registration Locations

You can register the license key in two locations. Choose the approach that best fits your project structure.

### Option 1: Register in App.xaml.cs

Register the license key in the **App.xaml.cs** constructor before `InitializeComponent()`.

#### When App Constructor Exists

```csharp
using Syncfusion.Licensing;

namespace YourAppNamespace
{
    public partial class App : Application
    {
        public App()
        {
            // Register Syncfusion license
            string licenseKey = Environment.GetEnvironmentVariable("SYNCFUSION_LICENSE_KEY");
            if (!string.IsNullOrEmpty(licenseKey))
            {   
                Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense(licenseKey);
            }
            
            InitializeComponent();
            
            MainPage = new AppShell();
        }
    }
}
```

#### When App Constructor Doesn't Exist

If your App.xaml.cs doesn't have a constructor, create one:

```csharp
using Syncfusion.Licensing;

namespace YourAppNamespace
{
    public partial class App : Application
    {
        // Create the constructor
        public App()
        {
            // Register Syncfusion license
            string licenseKey = Environment.GetEnvironmentVariable("SYNCFUSION_LICENSE_KEY");
            if (!string.IsNullOrEmpty(licenseKey))
            {   
                Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense(licenseKey);
            }
            
            InitializeComponent();
            
            MainPage = new AppShell();
        }
    }
}
```

**Key Points:**
- Registration happens **before** `InitializeComponent()`
- Ensures license is registered before any XAML is loaded
- Good for simple application setups

### Option 2: Register in MauiProgram.cs

Register the license key in **MauiProgram.cs** within the `CreateMauiApp` method.

#### Basic Registration

```csharp
using Syncfusion.Licensing;

namespace YourAppNamespace
{
    public static class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            // Register the Syncfusion license key
            string licenseKey = Environment.GetEnvironmentVariable("SYNCFUSION_LICENSE_KEY");
            if (!string.IsNullOrEmpty(licenseKey))
            {   
                Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense(licenseKey);
            }
     
            var builder = MauiApp.CreateBuilder();
            builder
                .UseMauiApp<App>()
                .ConfigureSyncfusionCore()
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

**Key Points:**
- Registration happens **before** `ConfigureSyncfusionCore()`
- Ensures license is set up during application builder configuration
- Recommended for projects with dependency injection or complex setup

#### With ConfigureSyncfusionCore

If you're using `ConfigureSyncfusionCore()` to register Syncfusion services:

```csharp
using Syncfusion.Licensing;
using Syncfusion.Maui.Core.Hosting;

namespace YourAppNamespace
{
    public static class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            // Register license BEFORE ConfigureSyncfusionCore
            string licenseKey = Environment.GetEnvironmentVariable("SYNCFUSION_LICENSE_KEY");
            if (!string.IsNullOrEmpty(licenseKey))
            {   
                Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense(licenseKey);
            }
     
            var builder = MauiApp.CreateBuilder();
            builder
                .UseMauiApp<App>()
                .ConfigureSyncfusionCore() // Syncfusion setup
                .ConfigureFonts(fonts =>
                {
                    fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
                });

            return builder.Build();
        }
    }
}
```

## Which Location to Choose?

### Use App.xaml.cs When:
- You have a simple application structure
- You're not using complex dependency injection
- You prefer keeping setup in the App class
- Your team is more familiar with App.xaml.cs patterns

### Use MauiProgram.cs When:
- You're using `ConfigureSyncfusionCore()`
- You have dependency injection setup
- You want centralized configuration
- You're following modern MAUI patterns

**Both approaches work correctly.** Choose based on your project needs and team preferences.

## Important: Syncfusion.Licensing.dll Reference

The `Syncfusion.Licensing` namespace requires the **Syncfusion.Licensing.dll** assembly.

### When Using NuGet Packages

The `Syncfusion.Licensing` NuGet package should be automatically installed as a dependency when you install any Syncfusion MAUI package.

**Verify it's installed:**
1. Open NuGet Package Manager
2. Look for "Syncfusion.Licensing" in installed packages
3. It should be automatically included

**If missing, install manually:**
```powershell
Install-Package Syncfusion.Licensing
```

Or via .NET CLI:
```bash
dotnet add package Syncfusion.Licensing
```

### Adding Using Statement

At the top of your file (App.xaml.cs or MauiProgram.cs):

```csharp
using Syncfusion.Licensing;
```

This allows you to use the shorter syntax:
```csharp
string licenseKey = Environment.GetEnvironmentVariable("SYNCFUSION_LICENSE_KEY");
if (!string.IsNullOrEmpty(licenseKey))
{   
    SyncfusionLicenseProvider.RegisterLicense(licenseKey);
}
```

Without the using statement, use the fully qualified name:
```csharp
string licenseKey = Environment.GetEnvironmentVariable("SYNCFUSION_LICENSE_KEY");
if (!string.IsNullOrEmpty(licenseKey))
{   
    Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense(licenseKey);
}
```

## Complete Examples

### Example 1: Simple MAUI App (App.xaml.cs)

**App.xaml.cs:**
```csharp
using Syncfusion.Licensing;

namespace SimpleMAUIApp
{
    public partial class App : Application
    {
        public App()
        {
            // Register Syncfusion license
            string licenseKey = Environment.GetEnvironmentVariable("SYNCFUSION_LICENSE_KEY");
            if (!string.IsNullOrEmpty(licenseKey))
            {   
                SyncfusionLicenseProvider.RegisterLicense(licenseKey);
            }
            
            InitializeComponent();
            
            MainPage = new MainPage();
        }
    }
}
```

**MainPage.xaml:**
```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:syncfusion="clr-namespace:Syncfusion.Maui.Buttons;assembly=Syncfusion.Maui.Buttons"
             x:Class="SimpleMAUIApp.MainPage">
    <VerticalStackLayout>
        <syncfusion:SfButton Text="Click Me" />
    </VerticalStackLayout>
</ContentPage>
```

### Example 2: MAUI App with ConfigureSyncfusionCore (MauiProgram.cs)

**MauiProgram.cs:**
```csharp
using Microsoft.Extensions.Logging;
using Syncfusion.Licensing;
using Syncfusion.Maui.Core.Hosting;

namespace ModernMAUIApp
{
    public static class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            // Register license before configuring Syncfusion
            string licenseKey = Environment.GetEnvironmentVariable("Mgo+DSMBaFt+QHJqVk1hXk5Hd0BLVGpAblJ3T2ZQdVt5ZDU7a15RRnVfR11gSH5Qd0FiWH5dcXE=");
            if (!string.IsNullOrEmpty(licenseKey))
            {   
                SyncfusionLicenseProvider.RegisterLicense(licenseKey);
            }
     
            var builder = MauiApp.CreateBuilder();
            builder
                .UseMauiApp<App>()
                .ConfigureSyncfusionCore()
                .ConfigureFonts(fonts =>
                {
                    fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
                });

#if DEBUG
            builder.Logging.AddDebug();
#endif

            return builder.Build();
        }
    }
}
```

**App.xaml.cs:**
```csharp
namespace ModernMAUIApp
{
    public partial class App : Application
    {
        public App()
        {
            InitializeComponent();
            MainPage = new AppShell();
        }
    }
}
```

### Example 3: Multiple Projects in Solution

When you have multiple MAUI projects using Syncfusion components:

**Shared Licensing Class (SharedLicensing.cs):**
```csharp
using Syncfusion.Licensing;

namespace MyCompany.Shared
{
    public static class SyncfusionLicensing
    {
        // Store license key in one place
        private const string LicenseKey = "Mgo+DSMBaFt+QHJqVk1hXk5Hd0BLVGpAblJ3T2ZQdVt5ZDU7a15RRnVfR11gSH5Qd0FiWH5dcXE=";
        
        public static void RegisterLicense()
        {
            if (!string.IsNullOrEmpty(licenseKey))
            {   
                SyncfusionLicenseProvider.RegisterLicense(licenseKey);
            }
        }
    }
}
```

**Project 1 - MauiProgram.cs:**
```csharp
using MyCompany.Shared;

namespace Project1
{
    public static class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            // Use shared licensing
            SyncfusionLicensing.RegisterLicense();
     
            var builder = MauiApp.CreateBuilder();
            builder.UseMauiApp<App>().ConfigureSyncfusionCore();
            return builder.Build();
        }
    }
}
```

**Project 2 - MauiProgram.cs:**
```csharp
using MyCompany.Shared;

namespace Project2
{
    public static class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            // Use shared licensing
            SyncfusionLicensing.RegisterLicense();
     
            var builder = MauiApp.CreateBuilder();
            builder.UseMauiApp<App>().ConfigureSyncfusionCore();
            return builder.Build();
        }
    }
}
```

## Using Environment Variables or Configuration

For better security and flexibility, store license keys outside of source code.

### Option 1: Environment Variables

**Set environment variable:**
```bash
# Windows
setx SYNCFUSION_LICENSE_KEY "YOUR LICENSE KEY"

# macOS/Linux
export SYNCFUSION_LICENSE_KEY="YOUR LICENSE KEY"
```

**Use in code:**
```csharp
using Syncfusion.Licensing;
using System;

namespace YourApp
{
    public static class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            // Get license key from environment variable
            string licenseKey = Environment.GetEnvironmentVariable("SYNCFUSION_LICENSE_KEY");
            
            if (!string.IsNullOrEmpty(licenseKey))
            {
                SyncfusionLicenseProvider.RegisterLicense(licenseKey);
            }
            else
            {
                throw new InvalidOperationException("Syncfusion license key not found in environment variables");
            }
     
            var builder = MauiApp.CreateBuilder();
            builder.UseMauiApp<App>().ConfigureSyncfusionCore();
            return builder.Build();
        }
    }
}
```

### Option 2: appsettings.json Configuration

**appsettings.json:**
```json
{
  "Syncfusion": {
    "LicenseKey": "YOUR LICENSE KEY"
  }
}
```

**MauiProgram.cs:**
```csharp
using Microsoft.Extensions.Configuration;
using Syncfusion.Licensing;
using System.Reflection;

namespace YourApp
{
    public static class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            var builder = MauiApp.CreateBuilder();
            
            // Load configuration
            var assembly = Assembly.GetExecutingAssembly();
            using var stream = assembly.GetManifestResourceStream("YourApp.appsettings.json");
            
            var config = new ConfigurationBuilder()
                .AddJsonStream(stream)
                .Build();
            
            // Register license from configuration
            string licenseKey = config["Syncfusion:LicenseKey"];
            SyncfusionLicenseProvider.RegisterLicense(licenseKey);
            
            builder.UseMauiApp<App>().ConfigureSyncfusionCore();
            return builder.Build();
        }
    }
}
```

**Important:** Add appsettings.json as embedded resource in .csproj:
```xml
<ItemGroup>
  <EmbeddedResource Include="appsettings.json" />
</ItemGroup>
```

### Option 3: User Secrets (Development Only)

For development, use .NET User Secrets:

**Initialize user secrets:**
```bash
dotnet user-secrets init
dotnet user-secrets set "Syncfusion:LicenseKey" "YOUR LICENSE KEY"
```

**Use in code:**
```csharp
#if DEBUG
using Microsoft.Extensions.Configuration;
#endif
using Syncfusion.Licensing;

namespace YourApp
{
    public static class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            var builder = MauiApp.CreateBuilder();
            
#if DEBUG
            // Load user secrets in development
            builder.Configuration.AddUserSecrets<App>();
            string licenseKey = builder.Configuration["Syncfusion:LicenseKey"];
#else
            // Use environment variable in production
            string licenseKey = Environment.GetEnvironmentVariable("SYNCFUSION_LICENSE_KEY");
#endif
            
            SyncfusionLicenseProvider.RegisterLicense(licenseKey);
            
            builder.UseMauiApp<App>().ConfigureSyncfusionCore();
            return builder.Build();
        }
    }
}
```

## Verification

After registration, your application should run without licensing warnings.

### Success Indicators
- ✅ No "trial version" warning message
- ✅ No "invalid license" error dialog
- ✅ Syncfusion controls render correctly
- ✅ No licensing errors in output/debug logs

### If You Still See Errors

See [licensing-errors.md](licensing-errors.md) for troubleshooting:
- Version mismatch
- Platform mismatch
- Invalid key format
- Missing Syncfusion.Licensing.dll

## Best Practices

### Security
1. **Don't commit license keys to public repositories**
   - Use .gitignore for config files with keys
   - Use environment variables or secrets
   
2. **Use different keys for different environments**
   - Development: Individual developer trial/licensed keys
   - Production: Deployment-specific licensed key

### Code Organization
1. **Register early in application lifecycle**
   - Before any Syncfusion control initialization
   - In App constructor or MauiProgram setup

2. **Register only once**
   - One call during startup is sufficient
   - Don't call multiple times

3. **Centralize for multi-project solutions**
   - Create shared licensing class
   - Reduce duplication

### Deployment
1. **Ensure Syncfusion.Licensing.dll is included**
   - Set Copy Local = True
   - Verify in output/deployment folder

2. **Test in deployment environment**
   - Verify license validation works
   - Check for any platform-specific issues

## Common Mistakes to Avoid

❌ **Registering after InitializeComponent()**
```csharp
public App()
{
    InitializeComponent(); // Wrong order!
    SyncfusionLicenseProvider.RegisterLicense("KEY"); // Too late
}
```

✅ **Register before InitializeComponent()**
```csharp
public App()
{
    SyncfusionLicenseProvider.RegisterLicense("KEY"); // Correct!
    InitializeComponent();
}
```

❌ **Not including Syncfusion.Licensing reference**
```csharp
// Missing: using Syncfusion.Licensing;
SyncfusionLicenseProvider.RegisterLicense("KEY"); // Error: Not found
```

✅ **Include proper using statement**
```csharp
using Syncfusion.Licensing; // Correct!
SyncfusionLicenseProvider.RegisterLicense("KEY");
```

❌ **Using wrong license key format**
```csharp
SyncfusionLicenseProvider.RegisterLicense(YOUR LICENSE KEY); // No quotes!
```

✅ **Enclose in double quotes**
```csharp
SyncfusionLicenseProvider.RegisterLicense("YOUR LICENSE KEY"); // Correct!
```

## Summary

- Use `SyncfusionLicenseProvider.RegisterLicense("YOUR KEY")` to register
- Register in App.xaml.cs constructor OR MauiProgram.cs
- Always register **before** InitializeComponent() or ConfigureSyncfusionCore()
- Requires Syncfusion.Licensing.dll reference
- Enclose license key in double quotes
- Register only once during application startup
- Consider using environment variables or configuration for security