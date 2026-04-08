# Installing Syncfusion MAUI NuGet Packages

## Table of Contents
- [Overview](#overview)
- [Installation Using Package Manager UI](#installation-using-package-manager-ui)
- [Installation Using .NET CLI](#installation-using-net-cli)
- [Installation Using Package Manager Console](#installation-using-package-manager-console)
- [Package Dependencies](#package-dependencies)
- [Version Management](#version-management)
- [Verifying Installation](#verifying-installation)
- [Common NuGet Packages](#common-nuget-packages)
- [Troubleshooting](#troubleshooting)

---

## Overview

NuGet is the package management system for .NET that allows you to easily add, update, and remove external libraries in your application. Syncfusion publishes all MAUI NuGet packages on [nuget.org](https://www.nuget.org/packages?q=Tag%3A%22Maui%22+Syncfusion), making them accessible without requiring the Syncfusion installer.

### Benefits of Using NuGet

- **No Installer Required:** Use Syncfusion components without installing full Syncfusion Suite
- **Easy Updates:** Update packages individually or all at once
- **Version Control:** Specify exact versions for consistency across teams
- **Dependency Management:** NuGet automatically handles package dependencies
- **Lightweight:** Only install packages you need

### When to Use NuGet vs Installer

**Use NuGet When:**
- You only need specific components
- Working in CI/CD pipelines
- Want latest packages quickly
- Need granular version control
- Working with .NET CLI or VS Code

**Use Installer When:**
- Need comprehensive component suite
- Want local package cache
- Prefer offline development
- Need sample applications and documentation locally

---

## Installation Using Package Manager UI

The Package Manager UI provides a visual interface for managing NuGet packages in Visual Studio.

### Step-by-Step Installation

**Step 1: Open Package Manager**

**Method A - From Solution Explorer:**
1. Right-click on your MAUI project or solution in Solution Explorer
2. Select **Manage NuGet Packages...**

**Method B - From Tools Menu:**
1. Go to **Tools** menu
2. Hover over **NuGet Package Manager**
3. Select **Manage NuGet Packages for Solution...**

**Step 2: Configure Package Source**

1. In the Manage NuGet Packages window, verify **Package source** dropdown shows **nuget.org**
2. If nuget.org is not available:
   - Go to **Tools → Options**
   - Navigate to **NuGet Package Manager → Package Sources**
   - Click **+** to add new source
   - Name: `nuget.org`
   - Source: `https://api.nuget.org/v3/index.json`
   - Click **Update**, then **OK**

**Step 3: Search for Packages**

1. Click the **Browse** tab
2. In the search box, type **"Syncfusion Maui"**
3. Browse available Syncfusion MAUI packages
4. Select the package you need (e.g., Syncfusion.Maui.Core, Syncfusion.Maui.DataGrid)

**Step 4: Install Package**

1. Select your desired package from the search results
2. On the right panel, review package information:
   - Description
   - Dependencies
   - Version history
3. Select the version (default is latest)
4. Click the **Install** button
5. Review and accept the license agreement
6. Wait for installation to complete

**Step 5: Verify Installation**

After installation succeeds:
- Package reference added to your .csproj file
- Dependencies automatically installed
- Ready to use component in your code

### Installing Multiple Packages

To install multiple Syncfusion packages:
1. Repeat the search and install process for each package
2. Or use **Manage NuGet Packages for Solution** to install across multiple projects

---

## Installation Using .NET CLI

The .NET Command Line Interface (CLI) allows package installation via terminal commands.

### Basic Syntax

```bash
dotnet add package <PackageName>
```

### Installation Steps

**Step 1: Navigate to Project Directory**

```bash
cd /path/to/your/maui/project
```

**Step 2: Install Package**

```bash
dotnet add package Syncfusion.Maui.Core
```

### Installing Specific Version

To install a specific version, use the `-v` or `--version` flag:

```bash
dotnet add package Syncfusion.Maui.Core -v 24.1.45
```

### Installing to Specific Project (in Solution)

If working with a solution containing multiple projects:

```bash
dotnet add ProjectName/ProjectName.csproj package Syncfusion.Maui.Core
```

### Common Installation Commands

**Core Package (Required for all Syncfusion components):**
```bash
dotnet add package Syncfusion.Maui.Core
```

**DataGrid:**
```bash
dotnet add package Syncfusion.Maui.DataGrid
```

**Charts:**
```bash
dotnet add package Syncfusion.Maui.Charts
```

**Scheduler:**
```bash
dotnet add package Syncfusion.Maui.Scheduler
```

**Calendar:**
```bash
dotnet add package Syncfusion.Maui.Calendar
```

**Inputs (ComboBox, Autocomplete, etc.):**
```bash
dotnet add package Syncfusion.Maui.Inputs
```

**ListView:**
```bash
dotnet add package Syncfusion.Maui.ListView
```

### Restoring Packages

After adding package references, restore all packages:

```bash
dotnet restore
```

This command:
- Downloads all referenced packages
- Resolves dependencies
- Updates lock files

### Verifying Installation via CLI

Check installed packages:

```bash
dotnet list package
```

Output shows all installed packages with versions.

### Updating Packages

Update a specific package to latest version:

```bash
dotnet add package Syncfusion.Maui.Core
```

Update all packages in project:

```bash
dotnet list package --outdated
dotnet add package <PackageName>  # For each outdated package
```

---

## Installation Using Package Manager Console

Package Manager Console provides PowerShell-based NuGet commands within Visual Studio.

### Opening Package Manager Console

1. In Visual Studio, go to **Tools** menu
2. Hover over **NuGet Package Manager**
3. Select **Package Manager Console**
4. Console appears at bottom of Visual Studio window

### Basic Installation Command

```powershell
Install-Package <PackageName>
```

### Installation Examples

**Install latest version:**
```powershell
Install-Package Syncfusion.Maui.Core
```

**Install specific version:**
```powershell
Install-Package Syncfusion.Maui.Core -Version 24.1.45
```

**Install to specific project (when multiple projects in solution):**
```powershell
Install-Package Syncfusion.Maui.Core -ProjectName MyMauiApp
```

### Common Package Manager Console Commands

**Install package:**
```powershell
Install-Package Syncfusion.Maui.DataGrid
```

**Update package to latest version:**
```powershell
Update-Package Syncfusion.Maui.DataGrid
```

**Update all packages in project:**
```powershell
Update-Package
```

**Uninstall package:**
```powershell
Uninstall-Package Syncfusion.Maui.DataGrid
```

**Get information about installed packages:**
```powershell
Get-Package
```

**Get available versions of a package:**
```powershell
Find-Package Syncfusion.Maui.Core -AllVersions
```

---

## Package Dependencies

### Automatic Dependency Resolution

NuGet automatically installs required dependencies. When you install a Syncfusion component package, NuGet:
1. Analyzes package dependencies
2. Downloads required dependency packages
3. Installs dependencies in correct order
4. Updates .csproj with all references

### Common Dependencies

Most Syncfusion MAUI packages depend on:
- **Syncfusion.Maui.Core** - Core functionality, themes, base classes
- **Microsoft.Maui.Controls** - .NET MAUI framework
- **Microsoft.Maui.Graphics** - Graphics abstractions

Some packages have additional dependencies:
- **PDF/Excel/Word components** - Syncfusion.Compression.Portable, Syncfusion.Pdf.Portable
- **AI-Powered components** - Microsoft.Extensions.AI, provider-specific packages

### Viewing Dependencies

**In Package Manager UI:**
- Select package
- Click **Dependencies** dropdown on right panel
- View all dependencies and their versions

**In .csproj file:**
```xml
<ItemGroup>
  <PackageReference Include="Syncfusion.Maui.DataGrid" Version="24.1.45" />
  <!-- Dependencies automatically managed by NuGet -->
</ItemGroup>
```

### Dependency Conflicts

If conflicts occur:
1. Update all Syncfusion packages to same version
2. Clear NuGet cache: `dotnet nuget locals all --clear`
3. Delete `bin` and `obj` folders
4. Restore packages: `dotnet restore`

---

## Version Management

### Versioning Strategy

Syncfusion uses semantic versioning: `Major.Minor.Patch.Build`
- **Example:** 24.1.45.0
  - **24** - Year (2024)
  - **1** - Volume (release number in year)
  - **45** - Service pack
  - **0** - Build number

### Installing Specific Versions

**.NET CLI:**
```bash
dotnet add package Syncfusion.Maui.Core -v 24.1.45
```

**Package Manager Console:**
```powershell
Install-Package Syncfusion.Maui.Core -Version 24.1.45
```

**Package Manager UI:**
1. Select package
2. Use **Version** dropdown to select specific version
3. Click **Install**

### Updating Packages

**Update to Latest Version:**

Via CLI:
```bash
dotnet add package Syncfusion.Maui.Core
```

Via Console:
```powershell
Update-Package Syncfusion.Maui.Core
```

**Update All Syncfusion Packages:**
```powershell
Update-Package -ProjectName YourProjectName
```

### Version Pinning

Lock to specific version in .csproj:

```xml
<PackageReference Include="Syncfusion.Maui.Core" Version="24.1.45" />
```

Allow minor updates only:
```xml
<PackageReference Include="Syncfusion.Maui.Core" Version="24.1.*" />
```

### Best Practices

- **Keep Versions Synchronized:** Use same version for all Syncfusion packages
- **Test Before Updating:** Test in development before updating production
- **Read Release Notes:** Review breaking changes before major updates
- **Use Version Control:** Commit updated packages to source control

---

## Verifying Installation

### Check .csproj File

Open your `.csproj` file to verify package references:

```xml
<ItemGroup>
  <PackageReference Include="Syncfusion.Maui.Core" Version="24.1.45" />
  <PackageReference Include="Syncfusion.Maui.DataGrid" Version="24.1.45" />
</ItemGroup>
```

### Check Via CLI

List all installed packages:

```bash
dotnet list package
```

Output example:
```
Project 'MyMauiApp' has the following package references
   [net8.0-android]:
   Top-level Package                      Version
   > Syncfusion.Maui.Core                 24.1.45
   > Syncfusion.Maui.DataGrid             24.1.45
```

### Build Project

Verify installation by building:

```bash
dotnet build
```

Successful build confirms packages installed correctly.

### Test in Code

Add a simple component to verify:

```csharp
using Syncfusion.Maui.Core;

// If this compiles, package is successfully installed
```

---

## Common NuGet Packages

### Core & Essential Packages

| Package | Purpose |
|---------|---------|
| **Syncfusion.Maui.Core** | Core functionality, themes, base controls (Required for all Syncfusion components) |
| **Syncfusion.Maui.Themes** | Additional theme resources |

### Data Components

| Package | Components |
|---------|------------|
| **Syncfusion.Maui.DataGrid** | DataGrid control |
| **Syncfusion.Maui.ListView** | ListView control |
| **Syncfusion.Maui.TreeView** | TreeView control |

### Visualization Components

| Package | Components |
|---------|------------|
| **Syncfusion.Maui.Charts** | Cartesian, Circular, Funnel, Pyramid, Polar charts |
| **Syncfusion.Maui.Gauges** | Linear and Radial gauges |
| **Syncfusion.Maui.Maps** | Interactive maps |
| **Syncfusion.Maui.Barcode** | Barcode generator |

### Editors & Inputs

| Package | Components |
|---------|------------|
| **Syncfusion.Maui.Inputs** | TextInputLayout, ComboBox, Autocomplete, MaskedEntry, etc. |
| **Syncfusion.Maui.Picker** | DatePicker, TimePicker, DateRangePicker |
| **Syncfusion.Maui.Sliders** | Slider, RangeSlider |

### Calendar & Scheduling

| Package | Components |
|---------|------------|
| **Syncfusion.Maui.Calendar** | Calendar control |
| **Syncfusion.Maui.Scheduler** | Scheduler/Appointment control |

### Navigation & Layout

| Package | Components |
|---------|------------|
| **Syncfusion.Maui.TabView** | Tab navigation |
| **Syncfusion.Maui.NavigationDrawer** | Slide-out navigation |
| **Syncfusion.Maui.Carousel** | Carousel control |
| **Syncfusion.Maui.Accordion** | Accordion/Expander |
| **Syncfusion.Maui.Popup** | Popup dialogs |

### Buttons & Indicators

| Package | Components |
|---------|------------|
| **Syncfusion.Maui.Buttons** | Button, Checkbox, RadioButton, Switch, SegmentedControl |
| **Syncfusion.Maui.BadgeView** | Badge notifications |
| **Syncfusion.Maui.ProgressBar** | Progress indicators |
| **Syncfusion.Maui.BusyIndicator** | Loading indicators |

---

## Troubleshooting

### Issue: Package Not Found

**Cause:** nuget.org not configured as package source

**Solution:**
1. Go to **Tools → Options → NuGet Package Manager → Package Sources**
2. Ensure nuget.org source exists: `https://api.nuget.org/v3/index.json`
3. Enable the source if disabled

### Issue: Version Conflict

**Symptoms:** Build errors mentioning version conflicts

**Solution:**
```bash
# Clear NuGet caches
dotnet nuget locals all --clear

# Delete bin and obj folders
rm -rf bin obj

# Restore packages
dotnet restore

# Rebuild
dotnet build
```

### Issue: Package Restore Failed

**Solution:**
1. Check internet connection
2. Verify nuget.org is accessible
3. Check firewall/proxy settings
4. Try manual restore: `dotnet restore --force`

### Issue: License Watermark Appears

**Cause:** License not registered or invalid

**Solution:**
Register license key in MauiProgram.cs before CreateMauiApp():
```csharp
// Read from environment variable - SECURITY BEST PRACTICE
string licenseKey = Environment.GetEnvironmentVariable("SYNCFUSION_LICENSE_KEY");
if (!string.IsNullOrEmpty(licenseKey))
{
    Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense(licenseKey);
}
```

See licensing-overview.md and licensing-register-key.md for details.

### Issue: Slow Package Download

**Solution:**
- Use package caching
- Download packages manually and use local NuGet source
- Or install full Syncfusion installer for offline access

---

## Next Steps

After installing NuGet packages:

1. **Register License:** See licensing-register-key.md
3. **Start Using Components:** Refer to component-specific documentation

For installation via installers, read installation-installers.md.