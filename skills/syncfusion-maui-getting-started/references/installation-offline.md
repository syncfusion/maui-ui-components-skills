# Offline Installer

Complete guide for downloading and installing Syncfusion .NET MAUI using the offline installer.

## Overview

The Syncfusion .NET MAUI offline installer allows you to install the complete suite without requiring constant internet access. Available in both EXE and ZIP formats for Windows.

## When to Use Offline Installer

- **Corporate environments** with restricted internet access
- **Slow or unreliable** internet connections
- **Multiple installations** across different machines
- **Air-gapped systems** with no internet connectivity
- **Consistent environment setup** for teams

## Downloading the Installer

### Trial Version

**Option 1: Download Free Trial Setup**

1. Visit [Download Free Trial](https://www.syncfusion.com/downloads)
2. Select **.NET MAUI** platform
3. Complete the form or log in with your Syncfusion account
4. Download from the confirmation page
5. Trial installer is valid for **30 days**

**Option 2: Start Trial via NuGet.org**

If you've already used Syncfusion packages from NuGet.org:

1. Go to [Start Trial](https://www.syncfusion.com/account/manage-trials/start-trials)
2. Sign in with your Syncfusion account
3. Select **.NET MAUI** product to start trial
4. Access installer from [Trials & Downloads](https://www.syncfusion.com/account/manage-trials/downloads)
5. Click **More Download Options** for offline installer (EXE/ZIP)

**Trial Access:**
- Access trial installer anytime before expiration
- Generate unlock key from Trials & Downloads page
- Only latest version available for trial

### Licensed Version

1. Visit [License & Downloads](https://www.syncfusion.com/account/downloads)
2. View all licenses (active and expired) in your account
3. Click **Download** for latest installer
4. For older versions: Click **Downloads Older Versions**
5. For different formats: Click **More Downloads Options**
6. Choose between **EXE** or **ZIP** format

## Installation Steps

### UI Installation

**Step 1: Extract Installer**

1. Double-click the downloaded installer file
2. Installer Wizard automatically extracts the package
3. Wait for extraction to complete

**Step 2: Unlock Installer**

Choose one of two methods:

**Method A: Login to Install**
- Enter your Syncfusion email and password
- Click **Create an account** if you don't have one
- Click **Forgot Password** to reset password
- Click **Next**

**Method B: Use Unlock Key**
- Enter your platform and version-specific unlock key
- **Trial keys** are valid for 30 days only
- **Licensed keys** don't expire
- Generate unlock key: [KB Article](https://support.syncfusion.com/kb/article/2757/how-to-generate-syncfusion-setup-unlock-key-from-syncfusion-support-account)

**Step 3: Accept License Terms**

1. Read the License Terms and Privacy Policy
2. Check **"I agree to the License Terms and Privacy Policy"**
3. Click **Next**

**Step 4: Configure Installation Settings**

**Installation Locations:**
- **Install Location:** Where Syncfusion assemblies will be installed
  - Default: `C:\Program Files (x86)\Syncfusion\Essential Studio\MAUI\{version}`
- **Samples Location:** Where demo applications will be installed
  - Default: `C:\Users\{username}\Documents\Syncfusion\{version}\MAUI`

**Additional Settings:**

- ☑ **Install Demos** - Install Syncfusion sample applications
- ☑ **Configure Syncfusion controls in Visual Studio** - Add controls to VS toolbox
- ☑ **Configure Syncfusion Extensions in Visual Studio** - Add extensions to VS
- ☑ **Create Desktop Shortcut** - Shortcut for Syncfusion Control Panel
- ☑ **Create Start Menu Shortcut** - Start menu entry for Control Panel

Click **Next** or **Install** to proceed.

**Step 5: Uninstall Previous Versions (Optional)**

If previous versions are detected:
1. Installer shows **Uninstall Previous Version(s)** wizard
2. Select versions to uninstall (checkboxes)
3. Click **Proceed**
4. Confirm uninstall when prompted
5. Progress screen shows:
   - Uninstall progress (if selected)
   - Install progress

**Step 6: Monitor Installation**

- Installation progress bar shows current status
- Estimated time remaining displayed
- Components being installed listed

**Step 7: Complete Installation**

1. Completion screen appears when done
2. Shows both install and uninstall status (if applicable)
3. Click **Launch Control Panel** to open Syncfusion Control Panel
4. Click **Finish** to exit installer

### Silent Mode Installation (Command Line)

For automated or script-based installations.

**Preparation:**

1. Run the installer by double-clicking
2. Let it extract to Temp directory
3. Run `%temp%` to open Temp folder
4. Find `syncfusionessentialmaui_(version).exe` in one of the folders
5. Copy the extracted file to a local drive location
6. Exit the wizard

**Installation Command:**

Open Command Prompt as **Administrator** and run:

```cmd
"C:\Path\syncfusionessentialmaui_x.x.x.x.exe" /Install silent /UNLOCKKEY:"your-unlock-key" [options]
```

**Optional Arguments:**

```cmd
/log "C:\Logs\installation.log"
/InstallPath:C:\Syncfusion\x.x.x.x
/InstallSamples:true
/InstallAssemblies:true
/UninstallExistAssemblies:true
/InstallToolbox:true
/CreateShortcut:true
/CreateStartMenuShortcut:true
```

**Full Example:**

```cmd
"D:\Installers\syncfusionessentialmaui_27.2.2.exe" /Install silent /UNLOCKKEY:"ABC123-DEF456-GHI789" /log "C:\Logs\install.log" /InstallPath:C:\Syncfusion\27.2.2 /InstallSamples:true /InstallAssemblies:true /UninstallExistAssemblies:true /InstallToolbox:true /CreateShortcut:true /CreateStartMenuShortcut:true
```

**Arguments Explained:**

| Argument | Description | Values |
|----------|-------------|--------|
| `/Install silent` | Silent installation mode | Required |
| `/UNLOCKKEY` | Product unlock key | Required |
| `/log` | Log file path | Optional |
| `/InstallPath` | Installation directory | Optional |
| `/InstallSamples` | Install demo samples | true/false |
| `/InstallAssemblies` | Install assemblies | true/false |
| `/UninstallExistAssemblies` | Remove old versions | true/false |
| `/InstallToolbox` | Configure VS toolbox | true/false |
| `/CreateShortcut` | Desktop shortcut | true/false |
| `/CreateStartMenuShortcut` | Start menu shortcut | true/false |

### Silent Mode Uninstallation

**Preparation:** Same as installation preparation steps

**Uninstallation Command:**

```cmd
"C:\Path\syncfusionessentialmaui_x.x.x.x.exe" /uninstall silent
```

**Example:**

```cmd
"D:\Installers\syncfusionessentialmaui_27.2.2.exe" /uninstall silent
```

## Troubleshooting

### Controlled Folder Access Error

**Problem:**
Cannot install to Documents folder due to Windows security settings.

**Error Message:**
> Controlled folder access is enabled on your machine. The provided install or samples location is protected.

**Solution 1: Disable controlled folder access**
1. Open Windows Security
2. Go to **Virus & threat protection**
3. Click **Manage ransomware protection**
4. Turn off **Controlled folder access**
5. Install Syncfusion
6. Re-enable after installation

**Solution 2: Install to alternate location**
- Choose installation path outside protected folders
- Example: `C:\Syncfusion\` instead of `C:\Users\Documents\`

### Another Installation in Progress

**Problem:**
Installer blocked by active msiexec process.

**Error Message:**
> Another installation is in progress. You cannot start this installation without completing all other currently active installations.

**Solution:**
1. Open **Task Manager** (Ctrl+Shift+Esc)
2. Go to **Details** tab
3. Find `msiexec.exe` process
4. Right-click → **End task**
5. Retry Syncfusion installer
6. If issue persists, restart computer

### Extraction Fails

**Problem:**
Installer fails to extract files.

**Solutions:**
1. **Run as Administrator:** Right-click installer → Run as administrator
2. **Temp space:** Ensure enough space in `%temp%` directory
3. **Antivirus:** Temporarily disable antivirus
4. **Download again:** File may be corrupted

### Unlock Key Errors

**Problem: Wrong key type**

**Error:**
> Trial unlock key cannot unlock licensed installer

**Solution:**
- Use licensed key for licensed installer
- Use trial key for trial installer
- Keys are platform and version specific

**Problem: Expired trial key**

**Solution:**
- Generate new trial key (if trial not expired)
- Purchase license for continued use
- Contact sales: sales@syncfusion.com

## Post-Installation

### Verify Installation

1. Check installation directory:
   ```
   C:\Program Files (x86)\Syncfusion\Essential Studio\MAUI\{version}
   ```

2. Open Syncfusion Control Panel (from shortcut or Start menu)

3. Verify samples location:
   ```
   C:\Users\{username}\Documents\Syncfusion\{version}\MAUI
   ```

### Configure Visual Studio

If you didn't select toolbox configuration during install:

1. Open Syncfusion Control Panel
2. Go to **Utilities** → **Toolbox Configuration**
3. Select Visual Studio version
4. Click **Configure**

### Local NuGet Source

Add installed location as local NuGet source:

1. Open Visual Studio
2. **Tools** → **NuGet Package Manager** → **Package Manager Settings**
3. Go to **Package Sources**
4. Click **+** to add new source
5. Set:
   - **Name:** Syncfusion Local
   - **Source:** `C:\Program Files (x86)\Syncfusion\Essential Studio\MAUI\{version}\NuGet`
6. Click **Update**, then **OK**

## Best Practices

1. **Keep installer file:** Store installer for offline reinstallation
2. **Document unlock key:** Save unlock key in secure location
3. **Version consistency:** Use same version across team
4. **Test in staging:** Test installer in non-production environment first
5. **Log files:** Use `/log` parameter in silent mode for troubleshooting

## Related Topics

- [Installation Errors](installation-errors.md) - Detailed error troubleshooting
- [System Requirements](system-requirements.md) - Prerequisites verification
- [NuGet Installation](installation-nuget.md) - Alternative installation method