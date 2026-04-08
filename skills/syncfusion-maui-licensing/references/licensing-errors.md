# Licensing Errors and Troubleshooting

## Table of Contents
- [Current Version Errors (20.3.0.x+)](#current-version-errors-2030x)
- [Legacy Version Errors (16.2.0.x - 20.3.0.x)](#legacy-version-errors-1620x---2030x)
- [Assembly Loading Issues](#assembly-loading-issues)
- [Troubleshooting Guide](#troubleshooting-guide)

Comprehensive guide for resolving Syncfusion .NET MAUI licensing errors with solutions for each error type.

## Current Version Errors (20.3.0.x+)

These errors apply to Syncfusion Essential Studio version 20.3.0.x and later.

### Error 1: License Key Not Registered / Trial Expired

**Error Message:**
```
This application was built using a trial version of Syncfusion Essential Studio. 
You should include the valid license key to remove the license validation message permanently.
```

**When This Occurs:**
- No license key has been registered in your application
- Trial license key has expired (after 30 days)
- License key registration code is missing or commented out

**Solution:**

**Step 1: Generate Valid License Key**
- **Licensed users:** [License & Downloads](https://www.syncfusion.com/account/downloads)
- **Trial users:** [Trial & Downloads](https://www.syncfusion.com/account/manage-trials/downloads)
- **Alternative:** Click "Claim License" button in the warning message, then generate key from [Claim License Key page](https://help.syncfusion.com/maui/licensing/how-to-generate#claim-license-key)

**Step 2: Register in Application**

In App.xaml.cs:
```csharp
using Syncfusion.Licensing;

public App()
{
    // Register before InitializeComponent
   string licenseKey = Environment.GetEnvironmentVariable("SYNCFUSION_LICENSE_KEY");
   if (!string.IsNullOrEmpty(licenseKey))
   {   
      Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense(licenseKey);
   }
    InitializeComponent();
    MainPage = new AppShell();
}
```

OR in MauiProgram.cs:
```csharp
using Syncfusion.Licensing;

public static MauiApp CreateMauiApp()
{
    // Register before ConfigureSyncfusionCore
   string licenseKey = Environment.GetEnvironmentVariable("SYNCFUSION_LICENSE_KEY");
   if (!string.IsNullOrEmpty(licenseKey))
   {   
      SyncfusionLicenseProvider.RegisterLicense(licenseKey);
   }
    
    var builder = MauiApp.CreateBuilder();
    builder.UseMauiApp<App>().ConfigureSyncfusionCore();
    return builder.Build();
}
```

**Step 3: Clean and Rebuild**
```bash
dotnet clean
dotnet build
```

**See Also:** [license-registration.md](license-registration.md) for detailed registration instructions.

### Error 2: Invalid Key

**Error Message:**
```
The included Syncfusion license key is invalid.
```

**When This Occurs:**
- License key format is incorrect
- Using license key from different version
- Using license key from different platform
- License key is corrupted or incomplete

**Solution:**

**Step 1: Verify License Key**
- Check key is complete (no missing characters)
- Ensure key is enclosed in double quotes
- Verify no extra spaces or line breaks

**Step 2: Check Version Match**
Your NuGet packages:
```xml
<PackageReference Include="Syncfusion.Maui.Core" Version="26.2.4" />
```

Your license key version MUST match: **26.2.4**

If mismatch:
- **Option A:** Generate new license key for version 26.2.4
- **Option B:** Update NuGet packages to match your license key version

**Step 3: Check Platform Match**
For .NET MAUI applications, license key MUST be for **.NET MAUI** platform.

Verify when generating key:
- Platform selection: ".NET MAUI" or "MAUI"
- NOT: Blazor, WPF, Xamarin.Forms, etc.

**Step 4: Regenerate License Key**
1. Go to [License & Downloads](https://www.syncfusion.com/account/downloads) or [Trial & Downloads](https://www.syncfusion.com/account/manage-trials/downloads)
2. Select Platform: **.NET MAUI**
3. Select Version: **Your installed version** (e.g., 26.2.4)
4. Generate new license key
5. Copy and register in application

**Step 5: Register Correctly**
```csharp
// Correct format
SyncfusionLicenseProvider.RegisterLicense("Mgo+DSMBaFt+QHJqVk1hXk5Hd0BLVGpAblJ3T2ZQdVt5ZDU7a15RRnVfR11gSH5Qd0FiWH5dcXE=");

// Common mistakes to avoid:
// ❌ Missing quotes
// SyncfusionLicenseProvider.RegisterLicense(YOUR LICENSE KEY);

// ❌ Missing part of key
// SyncfusionLicenseProvider.RegisterLicense("Mgo+DSM...");

// ❌ Extra spaces
// SyncfusionLicenseProvider.RegisterLicense(" YOUR KEY ");
```

## Legacy Version Errors (16.2.0.x - 20.3.0.x)

These errors apply to Syncfusion Essential Studio versions 16.2.0.x through 20.3.0.x.

### Error 3: License Key Not Registered (Legacy)

**Error Message:**
```
This application was built using a trial version of Syncfusion Essential Studio. 
Please include a valid license to permanently remove this license validation message. 
You can also obtain a free 30 day evaluation license to temporarily remove this 
message during the evaluation period. Please refer to this help topic for more information.
```

**Solution:**
Same as [Error 1](#error-1-license-key-not-registered--trial-expired) above.

### Error 4: Invalid Key (Legacy)

**Error Message:**
```
The included Syncfusion license is invalid. 
Please refer to this help topic for more information.
```

**Solution:**
Same as [Error 2](#error-2-invalid-key) above.

### Error 5: Trial Expired (Legacy)

**Error Message:**
```
Your Syncfusion trial license has expired. 
Please refer to this help topic for more information.
```

**When This Occurs:**
- Trial license key has expired (30 days have passed since generation)
- Still using expired trial key

**Solution:**

**Option A: Purchase License**
1. Purchase license from [Syncfusion Sales](https://www.syncfusion.com/sales/products)
2. After purchase, generate licensed key from [License & Downloads](https://www.syncfusion.com/account/downloads)
3. Replace trial key with licensed key in application

**Option B: Request Trial Extension (If Eligible)**
1. Contact Syncfusion support for trial extension eligibility
2. If approved, generate new trial key
3. Update application with new trial key

### Error 6: Platform Mismatch (Legacy)

**Error Message:**
```
The included Syncfusion license is invalid (Platform mismatch). 
Please refer to this help topic for more information.
```

**When This Occurs:**
- Using license key from different platform
- Example: Using Blazor key for MAUI app

**Solution:**

**Step 1: Identify Current Key Platform**
Check which platform your current key was generated for.

**Step 2: Generate Correct Platform Key**
1. Go to license generation page
2. Select Platform: **.NET MAUI** (not Blazor, WPF, etc.)
3. Select your version
4. Generate new key

**Step 3: Update Application**
Replace the platform-mismatched key with MAUI-specific key:

```csharp
// Replace this (Blazor key example)
// SyncfusionLicenseProvider.RegisterLicense("BLAZOR_KEY_HERE");

// With this (MAUI key)
SyncfusionLicenseProvider.RegisterLicense("MAUI_KEY_HERE");
```

### Error 7: Version Mismatch (Legacy)

**Error Message:**
```
The included Syncfusion license ({Registered Version}) is invalid for version {Required version}. 
Please refer to this help topic for more information.
```

**Example:**
```
The included Syncfusion license (25.1.35) is invalid for version 26.2.4.
```

**When This Occurs:**
- License key version doesn't match NuGet package version
- Example: Using 25.1.35 key with 26.2.4 packages

**Solution:**

**Option A: Update License Key (Recommended)**
1. Check your installed package version:
   ```xml
   <PackageReference Include="Syncfusion.Maui.Core" Version="26.2.4" />
   ```
2. Generate license key for version 26.2.4
3. Update RegisterLicense() with new key

**Option B: Downgrade Packages**
If your license doesn't cover the newer version:
1. Identify licensed version (e.g., 25.1.35)
2. Downgrade all Syncfusion packages to that version:
   ```xml
   <PackageReference Include="Syncfusion.Maui.Core" Version="25.1.35" />
   ```
3. Clean and rebuild

**Important:** All Syncfusion packages must be same version.

**Check All Packages:**
```bash
# List all Syncfusion packages
dotnet list package | findstr Syncfusion
```

Ensure they all show the same version number.

**Update All to Same Version:**
```bash
# Update all Syncfusion packages to specific version
dotnet add package Syncfusion.Maui.Core -v 26.2.4
dotnet add package Syncfusion.Maui.DataGrid -v 26.2.4
# ... etc for all packages
```

See [KB: How to generate license key for licensed products](https://support.syncfusion.com/kb/article/7898/how-to-generate-license-key-for-licensed-products) for detailed version generation instructions.

## Assembly Loading Issues

### Error 8: Could Not Load Syncfusion.Licensing.dll

**Error Message:**
```
Could not load Syncfusion.Licensing.dll assembly version [VERSION]
```

**When This Occurs:**
- Syncfusion.Licensing.dll is not referenced
- Assembly version mismatch
- DLL not copied to output directory
- Deployment missing the assembly

**Solution:**

**Step 1: Verify NuGet Package Installation**
Ensure Syncfusion.Licensing package is installed:

```xml
<!-- Should appear in .csproj -->
<PackageReference Include="Syncfusion.Licensing" Version="26.2.4" />
```

If missing, install it:
```bash
dotnet add package Syncfusion.Licensing
```

**Step 2: Check Assembly Version Match**
All Syncfusion assemblies should be same version:

```xml
<!-- All should be same version -->
<PackageReference Include="Syncfusion.Maui.Core" Version="26.2.4" />
<PackageReference Include="Syncfusion.Licensing" Version="26.2.4" />
<PackageReference Include="Syncfusion.Maui.DataGrid" Version="26.2.4" />
```

**Step 3: Set Copy Local to True**

For traditional assembly references (not NuGet):

1. Right-click Syncfusion.Licensing.dll in Solution Explorer
2. Select Properties
3. Set "Copy Local" to **True**

In .csproj file:
```xml
<Reference Include="Syncfusion.Licensing">
  <HintPath>..\packages\Syncfusion.Licensing.26.2.4\lib\net6.0\Syncfusion.Licensing.dll</HintPath>
  <Private>True</Private> <!-- This is Copy Local -->
</Reference>
```

**Step 4: Verify in Output Directory**

After build, check output folder (bin\Debug\net8.0-android, etc.):

```
bin/
  Debug/
    net8.0-android/
      YourApp.dll
      Syncfusion.Licensing.dll ← Should be present
      Syncfusion.Maui.Core.dll
      ...
```

If Syncfusion.Licensing.dll is missing:
- Check Copy Local setting
- Try Clean and Rebuild
- Verify NuGet package restore completed

**Step 5: Deployment Checklist**

When deploying, ensure:
- ✅ Syncfusion.Licensing.dll is in deployment package
- ✅ All Syncfusion assemblies are same version
- ✅ Dependencies are included
- ✅ Target platform matches assembly platform

**For Build Servers:**
- Restore NuGet packages: `dotnet restore`
- Verify all packages downloaded
- Check build output includes Syncfusion.Licensing.dll

See [KB: How to resolve "Could not load" errors](https://www.syncfusion.com/kb/4808/how-to-resolve-server-error-could-not-load-or-assembly-when-publishing-an-application) for additional troubleshooting.

## Troubleshooting Guide

### Diagnostic Checklist

When encountering licensing errors, verify:

**1. License Key Basics**
- [ ] License key is registered in code
- [ ] Registration happens before InitializeComponent() or ConfigureSyncfusionCore()
- [ ] License key is enclosed in double quotes
- [ ] No typos or missing characters in key

**2. Version Matching**
- [ ] All Syncfusion packages are same version
- [ ] License key version matches package version
- [ ] Check: `dotnet list package | findstr Syncfusion`

**3. Platform Matching**
- [ ] License key is for .NET MAUI platform
- [ ] Not using Blazor, WPF, or other platform keys

**4. Assembly References**
- [ ] Syncfusion.Licensing package is installed
- [ ] Using statement included: `using Syncfusion.Licensing;`
- [ ] Syncfusion.Licensing.dll in output folder

**5. Key Validity**
- [ ] Key is not expired (for trial licenses)
- [ ] Key is from correct account (Licensed vs Trial)
- [ ] Key was generated recently (not from old version)

### Common Scenarios and Quick Fixes

#### Scenario 1: Just Installed Syncfusion, Getting Trial Warning

**Quick Fix:**
```csharp
// 1. Generate key: https://www.syncfusion.com/account/manage-trials/downloads
// 2. Add to App.xaml.cs before InitializeComponent():
SyncfusionLicenseProvider.RegisterLicense("YOUR TRIAL KEY");
```

#### Scenario 2: Upgraded Syncfusion Version, Now Getting Error

**Quick Fix:**
```bash
# 1. Check new version
dotnet list package | findstr Syncfusion

# 2. Generate new license key for that version
# 3. Update RegisterLicense() with new key
# 4. Clean and rebuild
dotnet clean
dotnet build
```

#### Scenario 3: Works Locally But Fails in Deployment

**Quick Fix:**
1. Check Syncfusion.Licensing.dll is in deployment package
2. Set Copy Local = True for all Syncfusion assemblies
3. Verify license key is in deployed code (not conditionally excluded)

#### Scenario 4: Multiple Syncfusion Package Versions

**Quick Fix:**
```bash
# Update all to latest version
$version = "26.2.4"
dotnet add package Syncfusion.Maui.Core -v $version
dotnet add package Syncfusion.Maui.DataGrid -v $version
# ... for all Syncfusion packages

# Then generate license key for that version
```

### Step-by-Step Troubleshooting Process

**Step 1: Identify the Error**
- Note exact error message
- Check if error shows version or platform mismatch

**Step 2: Verify Package Versions**
```bash
dotnet list package | findstr Syncfusion
```
All should be same version. If not, standardize them.

**Step 3: Check License Key**
- Platform: Should be "MAUI"
- Version: Should match package version exactly

**Step 4: Regenerate if Necessary**
1. Go to appropriate downloads page (Licensed or Trial)
2. Select correct platform and version
3. Generate new key
4. Update in code

**Step 5: Verify Registration Location**
```csharp
// In App.xaml.cs - BEFORE InitializeComponent
public App()
{
    SyncfusionLicenseProvider.RegisterLicense("KEY"); // ← Here
    InitializeComponent(); // After license
}

// OR in MauiProgram.cs - BEFORE ConfigureSyncfusionCore
public static MauiApp CreateMauiApp()
{
    SyncfusionLicenseProvider.RegisterLicense("KEY"); // ← Here
    var builder = MauiApp.CreateBuilder();
    builder.UseMauiApp<App>().ConfigureSyncfusionCore(); // After license
}
```

**Step 6: Clean and Rebuild**
```bash
dotnet clean
dotnet build
```

**Step 7: Test**
Run application and verify no licensing errors appear.

### Getting Help

If errors persist after troubleshooting:

1. **Check Syncfusion Knowledge Base**
   - [Licensing FAQ](https://help.syncfusion.com/maui/licensing/licensing-faq)
   - [License Generation](https://support.syncfusion.com/kb/article/7898)

2. **Contact Syncfusion Support**
   - [Support Portal](https://support.syncfusion.com/)
   - Include: Error message, Syncfusion version, platform details

3. **Community Forums**
   - Search existing issues
   - Post with full error details and environment info

## Summary

**Most Common Causes:**
1. No license key registered → Register in App.xaml.cs or MauiProgram.cs
2. Version mismatch → Generate key for exact package version
3. Platform mismatch → Use MAUI-specific license key
4. Invalid/expired key → Regenerate from account portal
5. Missing Syncfusion.Licensing.dll → Install package, set Copy Local

**Key Reminders:**
- Register license **before** InitializeComponent() or ConfigureSyncfusionCore()
- License keys are version AND platform specific
- All Syncfusion packages must be same version
- Trial licenses expire after 30 days
- Syncfusion.Licensing.dll must be referenced and deployed