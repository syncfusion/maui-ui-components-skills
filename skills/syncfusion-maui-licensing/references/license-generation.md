# License Key Generation

Step-by-step guide for generating Syncfusion .NET MAUI license keys from your account.

## Where to Generate License Keys

License keys can be generated from two sections of the Syncfusion website, depending on your license type:

### For Licensed Users
**URL:** [License & Downloads](https://www.syncfusion.com/account/downloads)

Use this if you have:
- Purchased a Syncfusion license
- Active paid subscription
- Valid commercial license

### For Trial Users
**URL:** [Trial & Downloads](https://www.syncfusion.com/account/manage-trials/downloads)

Use this if you have:
- Started a 30-day trial
- Active evaluation license
- Free trial account

## Generation Process

### Step 1: Log In
1. Navigate to [syncfusion.com](https://www.syncfusion.com)
2. Click "Login" in the top right
3. Enter your Syncfusion account credentials

### Step 2: Access Downloads Section
- **Licensed users:** Go to [License & Downloads](https://www.syncfusion.com/account/downloads)
- **Trial users:** Go to [Trial & Downloads](https://www.syncfusion.com/account/manage-trials/downloads)

### Step 3: Generate Key
1. Look for the license key generation section
2. Select **Platform:** .NET MAUI
3. Select **Version:** Match your installed package version (e.g., 26.2.4)
4. Click "Generate License Key" or "Get License Key"
5. Copy the generated key

### Step 4: Use in Application
Register the key in your MAUI application code (see license-registration.md for details).

## Using Claim License Key

Syncfusion provides a convenient "Claim License Key" feature that simplifies key generation.

### Accessing Claim License Key

**Option 1: From Licensing Warning**
When you see the licensing warning message in your app:
```
This application was built using a trial version of Syncfusion Essential Studio...
```
Click the **"Claim License"** button in the warning dialog.

**Option 2: Direct URL**
Navigate to the Claim License Key page directly (available from your account dashboard).

### How Claim License Key Works

The system automatically detects your account status and provides the appropriate license key:

## License Key Scenarios

### Scenario 1: Active License

**Account Status:** You have a valid, active paid license.

**What Happens:**
- System generates a full license key immediately
- No expiration date on the key
- Valid for your license subscription period

**Display:**
```
✅ Active License Found
Platform: .NET MAUI
Version: 26.2.4
License Key: [YOUR-LICENSE-KEY]
Status: Valid
```

**Actions:**
1. Copy the license key
2. Register in your application
3. No limitations or warnings

### Scenario 2: Active Trial

**Account Status:** You have an active 30-day trial.

**What Happens:**
- System generates a trial license key
- Key includes expiration date (30 days from trial start)
- Valid until trial expires

**Display:**
```
⏰ Active Trial License
Platform: .NET MAUI
Version: 26.2.4
License Key: [YOUR-TRIAL-LICENSE-KEY]
Status: Valid
Expires: [DATE] (X days remaining)
```

**Actions:**
1. Copy the trial license key
2. Register in your application
3. Key works until expiration date
4. Purchase license before expiration or trial warning will appear

### Scenario 3: Expired License

**Account Status:** Your license subscription has expired.

**What Happens:**
- System generates a **temporary 5-day license key**
- Key is valid for 5 days only
- Gives you time to renew subscription

**Display:**
```
⚠️ Expired License - Temporary Key
Platform: .NET MAUI
Version: 26.2.4
License Key: [TEMPORARY-LICENSE-KEY]
Status: Temporary (5 days)
Expires: [DATE]
Action Required: Renew subscription
```

**Actions:**
1. Use temporary key for immediate needs (valid 5 days)
2. Renew your license subscription from [account page](https://www.syncfusion.com/account)
3. After renewal, generate new permanent license key
4. Replace temporary key with renewed license key

**Renewal Process:**
1. Go to [Syncfusion account renewal page](https://www.syncfusion.com/sales/products)
2. Complete renewal purchase
3. Return to [License & Downloads](https://www.syncfusion.com/account/downloads)
4. Generate new license key for latest version
5. Update key in your application

### Scenario 4: No Trial or License

**Account Status:** You have a Syncfusion account but no active trial or license.

**What Happens:**
- System offers options to get a license
- Can start a free 30-day trial
- Can purchase a license

**Display:**
```
ℹ️ No Active License or Trial
You don't have an active license or trial.

Options:
[Start Free Trial] - Get 30-day evaluation license
[Purchase License] - Buy commercial license
```

**Actions:**

**Option A: Start Trial**
1. Click "Start Free Trial"
2. Navigate to [Start Trials](https://www.syncfusion.com/account/manage-trials/start-trials)
3. Select Essential Studio for .NET MAUI
4. Start trial (30 days)
5. Return to [Trial & Downloads](https://www.syncfusion.com/account/manage-trials/downloads)
6. Generate trial license key

**Option B: Purchase License**
1. Click "Purchase License"
2. Navigate to [Sales Page](https://www.syncfusion.com/sales/products)
3. Choose appropriate license type
4. Complete purchase
5. Return to [License & Downloads](https://www.syncfusion.com/account/downloads)
6. Generate full license key

### Scenario 5: Expired Trial

**Account Status:** Your 30-day trial has expired.

**What Happens:**
- Trial license key no longer works
- Application shows trial expired errors
- Must purchase license to continue

**Display:**
```
❌ Trial Expired
Your 30-day trial has ended.

To continue using Syncfusion components:
[Purchase License] - Get commercial license
```

**Actions:**
1. Purchase a license from [Sales Page](https://www.syncfusion.com/sales/products)
2. After purchase, generate new license key
3. Update application with new license key

## Version and Platform Selection

### Selecting the Correct Version

**Critical:** License keys are version-specific. Select the exact version you're using.

**How to Check Your Version:**

**Option 1: NuGet Package Manager**
1. Open NuGet Package Manager in Visual Studio
2. Look at installed Syncfusion.Maui.* packages
3. Note the version number (e.g., 26.2.4)

**Option 2: Project File (.csproj)**
```xml
<PackageReference Include="Syncfusion.Maui.Core" Version="26.2.4" />
```

**Option 3: packages.config (if using)**
```xml
<package id="Syncfusion.Maui.Core" version="26.2.4" />
```

**Then:** Generate license key for that exact version (26.2.4 in this example).

### Selecting the Correct Platform

**For .NET MAUI applications:**
- Platform: **.NET MAUI**
- NOT: Blazor, WPF, WinForms, Xamarin.Forms

**Platform Selection:**
1. In license generation page, look for platform dropdown
2. Select ".NET MAUI" or "MAUI"
3. Generate key

### Handling Multiple Versions

**Scenario:** Your project uses multiple Syncfusion package versions.

**Problem:** License keys only work for exact version match.

**Solution:**
1. Standardize all Syncfusion packages to same version
2. Update all packages to latest version
3. Generate license key for that unified version

**Example Fix:**
```xml
<!-- Before (Mismatched) -->
<PackageReference Include="Syncfusion.Maui.Core" Version="26.2.4" />
<PackageReference Include="Syncfusion.Maui.DataGrid" Version="26.1.0" /> ❌

<!-- After (Matched) -->
<PackageReference Include="Syncfusion.Maui.Core" Version="26.2.4" />
<PackageReference Include="Syncfusion.Maui.DataGrid" Version="26.2.4" /> ✅
```

Now generate key for version 26.2.4.

## For NuGet.org Users Without Account

If you installed Syncfusion packages from NuGet.org but don't have a Syncfusion account:

### Step 1: Register for Account
1. Go to [Syncfusion Registration](https://www.syncfusion.com/account/register)
2. Fill in your details
3. Verify email address
4. Log in to your new account

### Step 2: Start Trial
1. Navigate to [Start Trials](https://www.syncfusion.com/account/manage-trials/start-trials)
2. Select "Essential Studio for .NET MAUI"
3. Click "Start Trial" (30 days free)

### Step 3: Generate Key
1. Go to [Trial & Downloads](https://www.syncfusion.com/account/manage-trials/downloads)
2. Generate license key for .NET MAUI platform
3. Select your installed version
4. Copy the generated key

### Step 4: Register in App
Use the key in your application code (see license-registration.md).

## Knowledge Base Resources

### Version-Specific Key Generation
For detailed instructions on generating keys for specific versions:
[How to generate license key for licensed products](https://support.syncfusion.com/kb/article/7898/how-to-generate-license-key-for-licensed-products)

### Which Version Key to Use
To determine which version of license key to use in your application:
[Which version Syncfusion license key should I use in my application](https://support.syncfusion.com/kb/article/7865/which-version-syncfusion-license-key-should-i-use-in-my-application)

## Troubleshooting

### Problem: Can't Find License Key Generation Option

**Solutions:**
- Ensure you're logged in to your Syncfusion account
- Verify you have an active license or trial
- For licensed users, check [License & Downloads](https://www.syncfusion.com/account/downloads)
- For trial users, check [Trial & Downloads](https://www.syncfusion.com/account/manage-trials/downloads)

### Problem: Platform Not Listed

**Solutions:**
- Ensure you selected correct section (License vs Trial downloads)
- Verify your license/trial includes .NET MAUI platform
- Contact Syncfusion support if platform missing

### Problem: Version Not Listed

**Solutions:**
- Your license may not cover that version
- Try generating for closest available version
- Update to a version covered by your license
- Renew license if needed for latest versions

### Problem: Generated Key Doesn't Work

**Verify:**
- Key is for exact version you're using
- Key is for MAUI platform (not other platforms)
- Key is properly registered in code (see license-registration.md)
- No typos when copying key

## Best Practices

1. **Generate keys immediately** when starting new projects
2. **Save keys securely** (password manager, environment variables)
3. **Don't commit keys to public repositories** (use .gitignore or config files)
4. **Regenerate keys** when upgrading Syncfusion versions
5. **Use environment-specific keys** for dev vs production if needed
6. **Document key generation** process for team members

## Summary

- Licensed users generate keys from [License & Downloads](https://www.syncfusion.com/account/downloads)
- Trial users generate keys from [Trial & Downloads](https://www.syncfusion.com/account/manage-trials/downloads)
- Use "Claim License Key" feature for quick access
- Select exact version and MAUI platform
- Active licenses get permanent keys
- Active trials get 30-day keys
- Expired licenses get temporary 5-day keys
- NuGet.org users need to create Syncfusion account first