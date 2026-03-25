# Installing Syncfusion MAUI via Installers

## Table of Contents
- [Overview](#overview)
- [Installer Types](#installer-types)
- [Web Installer](#web-installer)
- [Offline Installer](#offline-installer)
- [Mac Installer](#mac-installer)
- [Installer vs NuGet Comparison](#installer-vs-nuget-comparison)
- [Post-Installation Verification](#post-installation-verification)

---

## Overview

Syncfusion offers three types of installers for .NET MAUI components: Web Installer, Offline Installer, and Mac Installer. Each installer provides the complete Syncfusion MAUI component suite along with sample applications and documentation.

### When to Use Installers

**Choose Installers When:**
- You need the complete Syncfusion MAUI component suite
- You want sample applications and documentation installed locally
- You prefer offline development with local package cache
- You're starting a new comprehensive project
- You need to install on multiple machines (offline installer)

**Choose NuGet When:**
- You only need specific components
- Working in CI/CD pipelines
- Want to update individual packages easily
- Need granular version control
- Working primarily with .NET CLI or VS Code

---

## Installer Types

### Web Installer

**Description:** Small download that installs components via internet connection

**Pros:**
- Small initial download (~5-10 MB)
- Always gets latest version
- Minimal disk space for installer file

**Cons:**
- Requires active internet during installation
- Slower installation (downloads during install)

**Best For:** Single installations with reliable internet

---

### Offline Installer

**Description:** Complete package with all components bundled

**Pros:**
- No internet required during installation
- Fast installation
- Can install on multiple machines
- Ideal for restricted networks

**Cons:**
- Large download (~500 MB - 2 GB)
- Need to download new installer for updates

**Best For:** Corporate environments, offline development, multiple installations

---

### Mac Installer

**Description:** macOS-specific installer for Mac development

**Pros:**
- macOS-native installation
- Includes iOS/macOS sample applications
- Xcode integration support

**Cons:**
- macOS only
- Requires macOS 12+ (Monterey)

**Best For:** macOS developers building iOS/Mac apps

---

## Web Installer

### Downloading Web Installer

**Trial Version:**

1. Visit [Syncfusion Downloads](https://www.syncfusion.com/downloads) page
2. Select ".NET MAUI" platform
3. Complete the form or log in with your Syncfusion account
4. Download the web installer from confirmation page
5. For existing trial users, visit [Trials & Downloads](https://www.syncfusion.com/account/manage-trials/downloads)

**Licensed Version:**

1. Log in to your Syncfusion account
2. Navigate to [License & Downloads](https://www.syncfusion.com/account/downloads)
3. Find your .NET MAUI license
4. Click "Download" button
5. For older versions, select "Downloads Older Versions"

### Installing Web Installer

**Step 1: Run Installer**

1. Double-click downloaded installer file (e.g., `syncfusion_maui_installer.exe`)
2. If prompted by User Account Control, click "Yes"

**Step 2: Unlock Installer**

For trial version:
- Enter trial unlock key (generated from your account)
- Or log in with Syncfusion credentials

For licensed version:
- Log in with Syncfusion credentials

 **Step 3: Welcome Screen**

1. Read welcome information
2. Click "Next" to continue

**Step 4: License Agreement**

1. Review license agreement
2. Check "I accept the terms" if you agree
3. Click "Next"

**Step 5: Choose Installation Location**

1. Default location: `C:\Program Files (x86)\Syncfusion\Essential Studio\MAUI\{version}`
2. To change: Click "Browse" and select custom location
3. Click "Next"

**Step 6: Select Installation Options**

- **Install Syncfusion Components:** (Required) Core component libraries
- **Install Samples:** (Optional) Sample applications demonstrating component usage
- **Install Documentation:** (Optional) Offline help documentation

Click "Next"

**Step 7: Begin Installation**

1. Review installation summary
2. Click "Install"
3. Installation begins (downloads and installs components)
4. Wait for completion (may take 10-30 minutes depending on internet speed)

**Step 8: Complete Installation**

1. Installation complete message appears
2. Click "Finish"
3. Optionally launch sample browser

### Web Installer Components Installed

After installation, the following are available:

**Components:**
- All Syncfusion .NET MAUI NuGet packages (locally cached)
- Component library assemblies (.dll files)

**Samples:**
- Sample browser application
- Component-specific sample projects
- Integration examples

**Documentation:**
- Offline help documentation
- API reference
- Getting started guides

---

## Offline Installer

### Downloading Offline Installer

**Trial Version:**

1. Visit [Syncfusion Downloads](https://www.syncfusion.com/downloads)
2. Select ".NET MAUI" platform
3. Download offline installer (large file: ~500 MB - 2 GB)
4. Save to accessible location

**Licensed Version:**

1. Log in to [License & Downloads](https://www.syncfusion.com/account/downloads)
2. Click "More Download Options"
3. Select "Offline Installer"
4. Download complete installer package

### Installing Offline Installer

**Step 1: Extract Installer (if compressed)**

1. If downloaded as .zip, extract to folder
2. Navigate to extracted folder

**Step 2: Run Installer**

1. Double-click installer executable
2. Confirm User Account Control prompt

**Step 3-8: Same as Web Installer**

Follow steps 2-8 from Web Installer section above.

**Difference:** No internet download during installation—all components already included.

### Advantages of Offline Installer

- **No Internet Required:** Install on air-gapped or restricted networks
- **Faster Installation:** No download time during install
- **Multiple Installations:** Use same installer file on multiple machines
- **Version Control:** Specific version installer for consistency

### Use Cases

- Corporate environments with limited internet
- Installing on multiple developer machines
- Offline development scenarios
- Controlled version deployments

---

## Mac Installer

### Requirements

- macOS 12 (Monterey) or higher
- Xcode 13+ installed
- .NET 9.0 or .NET 10.0 SDK installed
- Syncfusion account (trial or licensed)

### Downloading Mac Installer

**Trial Version:**

1. Visit [Syncfusion Downloads](https://www.syncfusion.com/downloads)
2. Select ".NET MAUI" platform
3. Download Mac installer (.pkg or .dmg file)

**Licensed Version:**

1. Log in to [License & Downloads](https://www.syncfusion.com/account/downloads)
2. Select Mac installer from download options
3. Download .pkg/.dmg file

### Installing on macOS

**Step 1: Mount Installer (if .dmg)**

1. Double-click downloaded .dmg file
2. Installer volume mounts on desktop

**Step 2: Run Installer**

1. Double-click .pkg file
2. macOS may show security warning
3. If blocked, go to System Preferences → Security & Privacy
4. Click "Open Anyway" for the installer

**Step 3: Introduction Screen**

1. Read introduction
2. Click "Continue"

**Step 4: License Agreement**

1. Review license
2. Click "Continue" then "Agree"

**Step 5: Installation Type**

1. Standard Install: Installs all components
2. Custom Install: Choose specific components
3. Click "Install"

**Step 6: Authenticate**

1. Enter macOS admin password
2. Click "Install Software"

**Step 7: Installation Progress**

- Installer copies files and configures components
- Wait for completion (5-15 minutes)

**Step 8: Complete**

1. Installation success message
2. Click "Close"

### Mac Installer Components

**Installed Location:** `/Users/<username>/Library/Syncfusion/{version}/MAUI/`

**Components:**
- .NET MAUI component libraries
- iOS/macOS-specific samples
- Documentation
- NuGet package cache

### Mac-Specific Features

- Integration with Xcode projects
- iOS simulator sample running
- macOS Catalyst sample applications
- Code snippets for Visual Studio for Mac

---

## Installer vs NuGet Comparison

| Feature | Installer | NuGet Packages |
|---------|-----------|----------------|
| **Installation Method** | Executable file | Package Manager/CLI |
| **Internet Required** | Web: Yes, Offline: No | Yes (during install) |
| **Components Included** | All components | Select specific packages |
| **Samples** | Yes, included | No |
| **Documentation** | Offline available | Online only |
| **Local Package Cache** | Yes | Package cache folder |
| **Update Process** | Download new installer | Update individual packages |
| **Disk Space** | ~2-5 GB (full install) | ~100-500 MB (selected packages) |
| **Ideal For** | Comprehensive development | Targeted development |
| **CI/CD Friendly** | No | Yes |
| **Version Control** | Installer version | Per-package versioning |

### Recommendations

**Use Installer If:**
- New to Syncfusion MAUI (want samples and docs)
- Need comprehensive component suite
- Prefer offline development
- Want to explore all components

**Use NuGet If:**
- Know exactly which components you need
- Working in CI/CD pipelines
- Want flexible package updates
- Using .NET CLI or VS Code

---

## Post-Installation Verification

### Verify Installation Location

**Windows:**
```
C:\Program Files (x86)\Syncfusion\Essential Studio\MAUI\{version}\
```

**macOS:**
```
/Users/<username>/Library/Syncfusion/{version}/MAUI/
```

### Check Installed Components

Navigate to installation folder and verify:
- `/NuGet/` - Local NuGet packages
- `/Samples/` - Sample applications
- `/Documentation/` - Offline help files

### Launch Sample Browser

**Windows:**
1. Navigate to `{InstallPath}\Samples\`
2. Open sample browser solution in Visual Studio
3. Build and run to test installation

**macOS:**
1. Navigate to installation samples folder
2. Open sample project in Visual Studio for Mac
3. Select iOS simulator or Mac target
4. Run to verify installation

### Verify NuGet Package Access

**Visual Studio:**
1. Create or open a MAUI project
2. Right-click project → Manage NuGet Packages
3. Go to Settings → Package Sources
4. Verify Syncfusion local source exists (if configured by installer)
5. Browse for "Syncfusion.Maui" packages

### Test Component Integration

Create a simple MAUI project and add:

```xml
<ContentPage xmlns:buttons="clr-namespace:Syncfusion.Maui.Buttons;assembly=Syncfusion.Maui.Buttons">
    <buttons:SfButton Text="Test Button" />
</ContentPage>
```

If it compiles and runs, installation successful.

### Next Steps After Installation

1. **Register License:** Generate and register license key (see license-registration.md)
3. **Explore Samples:** Review sample browser applications
4. **Read Documentation:** Browse offline/online documentation
5. **Start Building:** Create your first Syncfusion MAUI application

---

## Troubleshooting Installation Issues

For installation errors and resolutions, see installation-errors.md.

For licensing issues, see licensing-errors.md.