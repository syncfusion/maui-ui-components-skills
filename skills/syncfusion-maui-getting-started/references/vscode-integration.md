# Visual Studio Code Integration

## Overview

Syncfusion provides Visual Studio Code extension for .NET MAUI development with project creation, code snippets, and Essential UI Kit support.

**Supported:** VS Code with .NET MAUI extension installed

## Prerequisites

Before installing Syncfusion extension:

**Required:**
- Visual Studio Code
- .NET 9 SDK or later
- .NET MAUI workload:
  ```bash
  dotnet workload install maui
  ```
- C# extension for VS Code

## Extension Installation

### Download and Install

**Method 1: VS Code Marketplace**

1. Open VS Code
2. **View** → **Extensions** (Ctrl+Shift+X)
3. Search: "Syncfusion .NET MAUI"
4. Install: **Syncfusion .NET MAUI Extension**
5. Reload VS Code

**Method 2: Command Palette**

1. Press: **Ctrl+Shift+P** (Cmd+Shift+P on Mac)
2. Type: "Extensions: Install Extensions"
3. Search: "Syncfusion .NET MAUI"
4. Install

**Method 3: Direct Download**

- Visit VS Code Marketplace online
- Download .vsix file
- Install via: **Extensions** → **...** → **Install from VSIX**

### Verify Installation

**View** → **Extensions** → Search "Syncfusion"

Look for: "Syncfusion .NET MAUI Extension" (Installed)

## Extension Features

### 1. Project Creation

Create Syncfusion .NET MAUI projects directly from VS Code.

**Create New Project:**

1. **Ctrl+Shift+P** (Command Palette)
2. Type: "Syncfusion .NET MAUI: Create New Project"
3. Select project template:
   - Blank App
   - Shell App
   - Essential UI Kit
4. Choose location
5. Enter project name
6. Select theme (Material/Cupertino)

**Generated Project Includes:**
- Syncfusion.Maui.Core package
- Theme configuration
- MauiProgram.cs with Syncfusion setup
- Basic project structure

**Example Output:**

```
MyMauiApp/
├── MauiProgram.cs
├── App.xaml
├── MainPage.xaml
└── Platforms/
```

### 2. Code Snippets

IntelliSense-powered snippets for Syncfusion controls.

**Using Snippets:**

In XAML file:
1. Type snippet prefix: `sf-`
2. Select control from IntelliSense
3. Snippet expands with template

**Available Snippets:**

| Snippet | Control |
|---------|---------|
| `sf-datagrid` | SfDataGrid |
| `sf-chart` | SfCartesianChart |
| `sf-calendar` | SfCalendar |
| `sf-scheduler` | SfScheduler |
| `sf-listview` | SfListView |
| `sf-combobox` | SfComboBox |

**Example: DataGrid Snippet**

Type: `sf-datagrid` → Tab

**Expands to:**
```xml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       ItemsSource="{Binding Items}"
                       AutoGenerateColumnsMode="None">
    <syncfusion:SfDataGrid.Columns>
        <syncfusion:DataGridTextColumn MappingName="Name" HeaderText="Name"/>
    </syncfusion:SfDataGrid.Columns>
</syncfusion:SfDataGrid>
```

**C# Snippets:**

In .cs files:

| Snippet | Code |
|---------|------|
| `sf-usingsyncfusion` | Add Syncfusion namespaces |
| `sf-configuremauiapp` | ConfigureSyncfusionCore() |
| `sf-themedictionary` | Theme ResourceDictionary |

### 3. Essential UI Kit Templates

Access pre-built screen templates.

**Add UI Kit Screen:**

1. **Ctrl+Shift+P**
2. "Syncfusion MAUI: Add Essential UI Kit"
3. Select category:
   - E-Commerce
   - Social
   - Banking
   - Forms
4. Choose specific screen
5. Screen added to project

**Benefits:**
- Production-ready screens
- MVVM structure
- Responsive layouts

### 4. IntelliSense Support

Auto-completion for Syncfusion control properties.

**Features:**
- Property suggestions
- Event handler templates
- Documentation tooltips
- Required attribute warnings

**Example:**
```xml
<syncfusion:SfDataGrid Auto
```
IntelliSense shows:
- AutoGenerateColumnsMode
- AutoSizeColumnsMode
- AutoFitMode

## Building and Running

### Build Project

**Terminal in VS Code:**
```bash
dotnet build
```

### Run on Android

```bash
dotnet build -t:Run -f net8.0-android
```

### Run on iOS (Mac only)

```bash
dotnet build -t:Run -f net8.0-ios
```

### Run on Windows

```bash
dotnet build -t:Run -f net8.0-windows10.0.19041.0
```

## Debugging

**Launch Configuration (.vscode/launch.json):**

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": ".NET MAUI (Android)",
      "type": "coreclr",
      "request": "launch",
      "preLaunchTask": "android-build",
      "program": "${workspaceFolder}/bin/Debug/net8.0-android/YourApp.dll"
    }
  ]
}
```

**Set Breakpoints:**
- Click left margin in C# files
- Breakpoint appears as red dot
- Run with F5

## Troubleshooting

### Extension Not Working

**Solution:**
- Verify .NET MAUI workload installed:
  ```bash
  dotnet workload list
  ```
- Reinstall workload if missing:
  ```bash
  dotnet workload install maui
  ```

### Snippets Not Appearing

**Solution:**
- Check file extension is `.xaml`
- Verify C# extension active
- Reload VS Code window

### Project Creation Fails

**Solution:**
- Check .NET SDK version:
  ```bash
  dotnet --version
  ```
- Must be .NET 9.0 or later
- Check write permissions in target directory

### Build Errors

**Solution:**
- Restore packages:
  ```bash
  dotnet restore
  ```
- Clean and rebuild:
  ```bash
  dotnet clean
  dotnet build
  ```

## VS Code vs Visual Studio

**Use VS Code when:**
- Cross-platform development (Mac/Linux)
- Lightweight environment preferred
- Command-line focused workflow

**Use Visual Studio when:**
- Windows primary platform
- Need visual designer
- Prefer GUI tools

Both support Syncfusion .NET MAUI development fully.

## Best Practices

✅ **Use Code Snippets** for faster control insertion

✅ **Leverage IntelliSense** for property discovery

✅ **Install C# extension** for full IDE experience

✅ **Use integrated terminal** for build commands

❌ **Don't skip .NET MAUI workload** installation

## Command Summary

**Create Project:**
```bash
Ctrl+Shift+P → "Syncfusion .NET MAUI: Create New Project"
```

**Add UI Kit:**
```bash
Ctrl+Shift+P → "Syncfusion MAUI: Add Essential UI Kit"
```

**Build:**
```bash
dotnet build
```

**Run:**
```bash
dotnet build -t:Run -f net8.0-android
```

## Related Files

- **Visual Studio Integration:** visual-studio-integration.md
- **Installation:** installation-nuget.md
- **Project Setup:** introduction-overview.md