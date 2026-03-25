# Installation Errors and Solutions

## Common Installation Errors

This guide describes the most common installation errors encountered when installing Syncfusion .NET MAUI components, their causes, and step-by-step solutions.

## Table of Contents
- [Unlocking License Installer with Trial Key](#unlocking-license-installer-with-trial-key)
- [License Has Expired](#license-has-expired)
- [Unable to Find Valid License or Trial](#unable-to-find-valid-license-or-trial)
- [Another Installation in Progress](#another-installation-in-progress)
- [Controlled Folder Access Error](#controlled-folder-access-error)
- [NuGet Package Restore Failures](#nuget-package-restore-failures)
- [Version Conflict Errors](#version-conflict-errors)

---

## Unlocking License Installer with Trial Key

### Error Message

```
Sorry, the provided unlock key is a trial unlock key and cannot be used to unlock 
the licensed version of our Essential Studio for .NET MAUI installer.
```

### Cause

You are attempting to use a **trial unlock key** to unlock a **licensed installer**, or vice versa.

### Solution

**Step 1: Identify Your License Type**
- Check your Syncfusion account to determine if you have a trial or paid license

**Step 2: Generate Correct Unlock Key**

For **Licensed Version:**
1. Log in to [Syncfusion Account](https://www.syncfusion.com/account/downloads)
2. Navigate to License & Downloads
3. Generate licensed unlock key

For **Trial Version:**
1. Log in to [Syncfusion Account](https://www.syncfusion.com/account/manage-trials)
2. Navigate to Trials & Downloads
3. Generate trial unlock key

**Step 3: Download Matching Installer**
- For **trial key**: Download trial installer
- For **licensed key**: Download licensed installer

**Step 4: Unlock with Correct Key**
- Enter the appropriate unlock key matching your installer type

### Prevention

Always verify your license type before downloading the installer:
- Trial users → Use trial unlock key with trial installer
- Licensed users → Use licensed unlock key with licensed installer

---

## License Has Expired

### Error Message

**Online Installer:**
```
Your license for Syncfusion Essential Studio for .NET MAUI has been expired since {date}. 
Please renew your subscription and try again.
```

**Offline Installer:**
```
License expired on {date}. Please renew to continue.
```

### Cause

Your Syncfusion license or trial period has expired.

### Solutions

**Option 1: Renew Subscription (for Existing License Holders)**

1. Visit [My Renewals](https://www.syncfusion.com/account/my-renewals)
2. Select .NET MAUI product
3. Complete renewal process
4. Download new installer after renewal

**Option 2: Purchase New License**

1. Visit [Syncfusion Products](https://www.syncfusion.com/sales/products)
2. Select .NET MAUI components
3. Complete purchase
4. Access downloads from your account

**Option 3: Extend Trial Period**

If your trial has expired:
- Contact sales team: sales@syncfusion.com
- Request trial extension (typically granted once)

**Option 4: Contact Sales Team**

Email: sales@syncfusion.com for:
- License renewal assistance
- Pricing questions
- Volume licensing options

### After Resolving License

1. Download latest installer from your account
2. Generate new unlock key (if needed)
3. Uninstall old version (optional)
4. Install new version with valid license

---

## Unable to Find Valid License or Trial

### Error Message

**Offline Installer:**
```
Sorry, we are unable to find a valid license or trial for Essential Studio 
for .NET MAUI under your account.
```

**Online Installer:**
```
No valid license or active trial found for this account.
```

### Possible Causes

1. **Trial period has expired**
2. **No license or active trial exists**
3. **Not the license holder** (license assigned to different user)
4. **License not yet assigned** by account administrator

### Solutions

**Scenario 1: Trial Expired**

Start a new trial (if eligible):
1. Visit [Start Trials](https://www.syncfusion.com/account/manage-trials/start-trials)
2. Select .NET MAUI platform
3. Begin new 30-day trial
4. Download installer

**Note:** Trial can only be started once per email/account. If previously used, purchase license required.

**Scenario 2: No License Exists**

Purchase a license:
1. Visit [Syncfusion Products](https://www.syncfusion.com/sales/products)
2. Select .NET MAUI
3. Complete purchase
4. Download installer from account

**Scenario 3: License Assigned to Different User**

Contact your organization's Syncfusion account administrator:
- Request license assignment
- Verify you're added as licensed user
- Once assigned, download installer

**Scenario 4: License Purchase Not Processed**

Email client relations team:
- Email: clientrelations@syncfusion.com
- Subject: "License Access Issue - .NET MAUI"
- Include: Order number, account email

**Scenario 5: Corporate License**

If your organization has corporate license:
1. Contact IT/Admin team
2. Request Syncfusion account access
3. Get assigned to company license
4. Access downloads after assignment

### Verification Steps

After resolving:
1. Log out of Syncfusion account
2. Log back in
3. Navigate to [License & Downloads](https://www.syncfusion.com/account/downloads)
4. Verify .NET MAUI license appears
5. Download installer

---

## Another Installation in Progress

### Error Message

```
Another installation is in progress. You cannot start this installation without completing 
all other currently active installations. Click cancel to end this installer or retry after 
currently active installation have completed.
```

### Cause

Windows Installer (msiexec.exe) is running another installation, preventing Syncfusion installer from starting.

### Solution (Method 1: End Installation Process)

**Step 1: Open Task Manager**
- Press `Ctrl + Shift + Esc`
- Or right-click taskbar → Task Manager

**Step 2: Navigate to Details Tab**
- Click "Details" tab (Windows 10/11)
- Or "Processes" tab in older Windows

**Step 3: Find msiexec.exe**
- Scroll to find `msiexec.exe` processes
- There may be multiple instances

**Step 4: End Process**
- Right-click each `msiexec.exe`
- Select "End Task"
- Confirm if prompted
- End all msiexec.exe instances

**Step 5: Retry Installation**
- Close and reopen Syncfusion installer
- Attempt installation again

### Solution (Method 2: Restart Computer)

If ending processes doesn't work:

**Step 1: Save All Work**
- Save open documents/projects

**Step 2: Restart**
- Restart Windows

**Step 3: Retry Installation**
- After restart, run Syncfusion installer
- Should proceed without error

### Solution (Method 3: Force Stop Windows Installer Service)

**Step 1: Open Services**
- Press `Win + R`
- Type `services.msc`
- Click OK

**Step 2: Find Windows Installer**
- Scroll to find "Windows Installer" service

**Step 3: Stop Service**
- Right-click "Windows Installer"
- Select "Stop"
- Wait for service to stop

**Step 4: Retry Installation**
- Run Syncfusion installer
- Windows Installer service will auto-start

### Prevention

- Complete or cancel other installations before starting Syncfusion installer
- Don't run multiple installers simultaneously
- Wait for Windows updates to complete before installing

---

## Controlled Folder Access Error

### Error Message

**Offline Installer:**
```
Controlled folder access is enabled on your machine. The provided install or samples 
location (e.g., Public Documents) is protected by the controlled folder access settings.
```

**Online Installer:**
```
Controlled folder access is enabled on your machine. The provided install, samples, or 
download location (e.g., Public Documents) is protected by the controlled folder access settings.
```

### Cause

Windows Controlled Folder Access (part of Windows Security/Defender) is preventing installer from writing to protected folders like Documents.

### Solution 1: Change Installation Directory

**Step 1: Choose Custom Location**
- During installation, select custom installation path
- Choose non-protected directory (e.g., `C:\Syncfusion\`)
- Avoid: Documents, Desktop, Pictures, Downloads

**Step 2: Complete Installation**
- Install to custom directory
- Samples and components install successfully

### Solution 2: Temporarily Disable Controlled Folder Access

**Step 1: Open Windows Security**
- Press `Win + I` (Settings)
- Go to "Privacy & Security" → "Windows Security"
- Click "Virus & threat protection"

**Step 2: Access Controlled Folder Settings**
- Scroll to "Ransomware protection"
- Click "Manage ransomware protection"

**Step 3: Disable Controlled Folder Access**
- Toggle "Controlled folder access" to **Off**

**Step 4: Install Syncfusion**
- Run Syncfusion installer
- Complete installation

**Step 5: Re-enable Controlled Folder Access**
- After installation completes
- Return to ransomware protection settings
- Toggle "Controlled folder access" back to **On**

### Solution 3: Allow Syncfusion Installer Through Controlled Folders

**Step 1: Open Controlled Folder Access Settings**
- Follow steps above to open "Manage ransomware protection"

**Step 2: Allow an App**
- Click "Allow an app through Controlled folder access"
- Click "Add an allowed app"

**Step 3: Add Syncfusion Installer**
- Browse to Syncfusion installer executable
- Select and add it

**Step 4: Install**
- Run installer (now allowed to access protected folders)

### Recommendation

For best security:
- **Option 1 (Recommended):** Install to custom non-protected directory
- **Option 2:** Temporarily disable, then re-enable after installation
- **Option 3:** Allow installer specifically (more secure than disabling)

---

## NuGet Package Restore Failures

### Error Symptoms

- Build errors mentioning missing packages
- "Package restore failed"
- Components not found during compilation

### Common Causes & Solutions

**Cause 1: No Internet Connection**

**Solution:**
- Connect to internet
- Retry build/restore
- Or use offline NuGet package source

**Cause 2: NuGet Source Not Configured**

**Solution:**
```bash
# Add nuget.org source
dotnet nuget add source https://api.nuget.org/v3/index.json -n nuget.org
```

**Or in Visual Studio:**
1. Tools → Options → NuGet Package Manager → Package Sources
2. Add nuget.org if missing
3. Ensure it's enabled

**Cause 3: Corrupted NuGet Cache**

**Solution:**
```bash
# Clear all NuGet caches
dotnet nuget locals all --clear

# Restore packages
dotnet restore
```

**Cause 4: Firewall/Proxy Blocking**

**Solution:**
- Configure proxy in NuGet.config
- Or add nuget.org to firewall whitelist

**NuGet.config proxy example:**
```xml
<configuration>
  <config>
    <add key="http_proxy" value="http://proxy.address:port" />
  </config>
</configuration>
```

---

## Version Conflict Errors

### Error Symptoms

- "Version conflict detected"
- "Could not load assembly"
- Multiple versions of same package referenced

### Solution 1: Synchronize Package Versions

Ensure all Syncfusion packages use same version:

```xml
<!-- In .csproj - all should match -->
<PackageReference Include="Syncfusion.Maui.Core" Version="24.1.45" />
<PackageReference Include="Syncfusion.Maui.DataGrid" Version="24.1.45" />
<PackageReference Include="Syncfusion.Maui.Charts" Version="24.1.45" />
```

### Solution 2: Clean and Rebuild

```bash
# Delete build artifacts
rm -rf bin obj

# Clear NuGet cache
dotnet nuget locals all --clear

# Restore packages
dotnet restore

# Rebuild
dotnet build
```

### Solution 3: Update All Packages

```bash
# Update all Syncfusion packages
dotnet list package --outdated
dotnet add package Syncfusion.Maui.Core  # Updates to latest
# Repeat for all packages
```

---

## Getting Additional Help

If errors persist after trying these solutions:

**1. Syncfusion Support**
- Create support ticket: https://support.syncfusion.com/support/tickets/create
- Include: Error message, steps taken, system details

**2. Knowledge Base**
- Search: https://support.syncfusion.com/kb
- Many errors documented with solutions

**3. Forums**
- Community support: https://www.syncfusion.com/forums/maui

**4. Direct Contact**
- Technical: clientrelations@syncfusion.com
- Sales: sales@syncfusion.com