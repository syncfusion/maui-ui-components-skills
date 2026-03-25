# Visual Studio Integration

## Overview

Syncfusion provides Visual Studio extension for accelerated .NET MAUI development with project templates, toolbox controls, and Essential UI Kit.

**Supported:** Visual Studio 2022 (17.3+)

## Extension Installation

### Download and Install

**Method 1: Visual Studio Marketplace**

1. Open Visual Studio 2022
2. **Extensions** → **Manage Extensions**
3. Search: "Syncfusion .NET MAUI"
4. Install **Syncfusion .NET MAUI Extensions**
5. Restart Visual Studio

**Method 2: Direct Download**

1. Visit: [Syncfusion Visual Studio Extensions page](https://www.syncfusion.com/downloads/support/directtrac/general)
2. Download installer for VS 2022
3. Run installer
4. Restart Visual Studio

**Method 3: Syncfusion Control Panel**

- If you have Syncfusion Essential Studio installed
- Use Control Panel → Extensions → Install VS Extension

### Verify Installation

**Extensions** → **Manage Extensions** → **Installed**

Look for: "Syncfusion .NET MAUI Extensions"

## Extension Features

### 1. Project Templates

Pre-configured project templates with Syncfusion NuGet packages.

**Create New Project:**

1. **File** → **New** → **Project**
2. Search: "Syncfusion MAUI"
3. Select: **Syncfusion .NET MAUI App**
4. Configure:
   - Project name
   - Location
   - Solution name
5. **Next** → Choose features:
   - Material or Cupertino theme
   - Sample pages (optional)
   - Navigation structure
6. **Create**

**What's Included:**
- Syncfusion.Maui.Core NuGet package
- Theme ResourceDictionary configured
- MauiProgram.cs with Syncfusion initialization
- Sample layouts (optional)

### 2. Toolbox Controls

Drag-and-drop Syncfusion controls from toolbox.

**Using Toolbox:**

1. Open XAML designer
2. **View** → **Toolbox** (Ctrl+Alt+X)
3. Find: **Syncfusion Controls for .NET MAUI**
4. Drag control to designer
5. XAML namespace added automatically

**Available Controls:**
- DataGrid
- Charts
- Scheduler
- Calendar
- ListView
- ComboBox
- ...and 50+ more

**Auto-Generated XAML:**
```xml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       ItemsSource="{Binding YourData}">
</syncfusion:SfDataGrid>
```

Namespace added automatically:
```xml
xmlns:syncfusion="clr-namespace:Syncfusion.Maui.DataGrid;assembly=Syncfusion.Maui.DataGrid"
```

### 3. Essential UI Kit

Pre-built application templates with beautiful UI screens.

**Access UI Kit:**

1. **File** → **New** → **Project**
2. Search: "Syncfusion Essential UI Kit"
3. Choose template category:
   - E-Commerce
   - Social
   - Banking
   - Healthcare
   - Restaurant

**Features:**
- 150+ pre-built screens
- Responsive layouts
- Theming support
- MVVM architecture
- Navigation configured

**Example Categories:**
- Login/signup pages
- Product catalogs
- Shopping carts
- Profile pages
- Dashboard screens
- Forms and data entry

### 4. Syncfusion Notifications

Receive update notifications for Syncfusion components.

**View Notifications:**

**Tools** → **Syncfusion** → **Notification**

**Notifications Include:**
- New version releases
- Security updates
- Feature updates
- Breaking changes

**Configure Notifications:**
- Enable/disable in extension settings
- Choose notification frequency

## Syncfusion Menu

**Tools** → **Syncfusion** menu provides:

- **Sample Browser:** Launch control samples
- **Release Notes:** View version history
- **Support:** Access technical support
- **License Registration:** Register license key
- **Check Updates:** Manual update check

## Template Studio

Advanced project scaffolding tool.

**Launch Template Studio:**

1. **File** → **New** → **Project**
2. Search: "Syncfusion .NET MAUI Template Studio"
3. Choose project type:
   - Blank app
   - Navigation app
   - Shell app
4. Select pages to include
5. Configure theme
6. Generate project

**Benefits:**
- Choose only features you need
- Customizable project structure
- Best practices built-in

## Troubleshooting

### Extension Not Visible

**Solution:**
- Verify Visual Studio 2022 version (17.3+)
- Check **Extensions** → **Manage Extensions** → **Installed**
- Reinstall extension

### Toolbox Controls Missing

**Solution:**
- Close/reopen XAML designer
- **View** → **Toolbox** → Right-click → **Reset Toolbox**
- Rebuild solution

### Project Template Not Found

**Solution:**
- Restart Visual Studio after installation
- Clear template cache: Delete `%USERPROFILE%\AppData\Roaming\Microsoft\VisualStudio\17.0_xxxxx\ProjectTemplatesCache`
- Repair extension

### Namespace Not Added Automatically

**Solution:**
- Manually add namespace:
```xml
xmlns:syncfusion="clr-namespace:Syncfusion.Maui.DataGrid;assembly=Syncfusion.Maui.DataGrid"
```
- Verify NuGet package installed

## Best Practices

✅ **Use Project Templates** for new projects (saves configuration time)

✅ **Keep Extension Updated** for latest features and fixes

✅ **Explore Essential UI Kit** before building custom screens

✅ **Enable Notifications** to stay informed of updates

❌ **Don't mix manual and template setup** (choose one approach)

## Alternative: Manual Setup

If you prefer not using extension:

**Install packages manually:**
```bash
dotnet add package Syncfusion.Maui.Core
```

**Configure MauiProgram.cs:**
```csharp
var builder = MauiApp.CreateBuilder();
builder.UseMauiApp<App>()
    .ConfigureSyncfusionCore();
```

See: installation-nuget.md

## Related Files

- **VS Code Integration:** vscode-integration.md
- **Installation:** installation-nuget.md