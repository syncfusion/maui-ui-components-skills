# Licensing Overview

Understanding the Syncfusion .NET MAUI licensing system, requirements, and when license keys are needed.

## Introduction

Starting with version **20.2.0.x**, Syncfusion introduced a new licensing system for Essential Studio. These changes apply to all evaluators and paid customers who use Syncfusion assemblies from:
- **NuGet packages** from nuget.org
- **Evaluation installer** (trial version)

If you use either of these sources, you must register a corresponding platform and version license key in your projects.

## License Key Requirement Matrix

| Source of Assemblies | License Key Required? | Where to Get Key |
|---------------------|----------------------|------------------|
| **NuGet package from nuget.org** | ✅ Yes | [Licensed users](https://www.syncfusion.com/account/downloads) or [Trial users](https://www.syncfusion.com/account/manage-trials/downloads) |
| **Trial installer** | ✅ Yes | [Trial downloads](https://www.syncfusion.com/account/manage-trials/downloads) |
| **Licensed installer** | ❌ No | Not applicable (license embedded in assemblies) |

## What Happens Without License Registration?

If you don't register a license key when required, you'll see this error message in your application:

```
This application was built using a trial version of Syncfusion Essential Studio. 
Please include a valid license to permanently remove this license validation message. 
You can also obtain a free 30 day evaluation license to temporarily remove this 
message during the evaluation period.
```

This message appears at runtime and indicates that either:
- No license key has been registered
- The registered license key is invalid
- The license key has expired (for trial licenses)

## Unlock Key vs License Key

**Important:** These are two different keys with different purposes.

### Unlock Key
- **Purpose:** Unlocks the Syncfusion installer during installation
- **Used when:** Installing Syncfusion Essential Studio on your development machine
- **Location:** Entered during installer setup
- **Not used in code**

### License Key
- **Purpose:** Validates your application's use of Syncfusion components at runtime
- **Used when:** Running applications that use NuGet packages or trial installer assemblies
- **Location:** Registered in your application code using RegisterLicense() method
- **Required in code** when using NuGet or trial installer

**Key Difference:** The unlock key is for the installer, the license key is for your application code.

See this [knowledge base article](https://support.syncfusion.com/kb/article/7863/difference-between-the-unlock-key-and-licensing-key) for more details.

## License Key Characteristics

### Version Specific

License keys are tied to a specific version of Syncfusion Essential Studio:
- Each major.minor.patch version requires its own license key
- A key for version 26.2.4 will NOT work for 26.1.0 or 27.0.0
- All Syncfusion packages in your project should use the same version

**Example:**
```
License Key Version: 26.2.4
✅ Works with: Syncfusion.Maui.Core 26.2.4
❌ Fails with: Syncfusion.Maui.Core 26.1.0
❌ Fails with: Syncfusion.Maui.Core 27.0.0
```

### Platform Specific

License keys are tied to a specific platform:
- .NET MAUI requires a MAUI-specific license key
- Cannot use Blazor, WPF, or other platform keys for MAUI

**Example:**
```
License Key Platform: MAUI
✅ Works with: .NET MAUI applications
❌ Fails with: Blazor applications
❌ Fails with: WPF applications
```

When generating a license key, ensure you select:
1. The correct platform (.NET MAUI)
2. The exact version you're using in your project

## Build Server Licensing

Licensing requirements for build servers depend on the source of assemblies:

### NuGet Packages

**Scenario:** Build server uses Syncfusion NuGet packages from nuget.org

**Requirements:**
- ✅ License key registration IS required
- No need to install Syncfusion installer on build server
- Use any developer license to generate keys for build environments
- Register the key in your application code

**Steps:**
1. Reference Syncfusion NuGet packages in your project
2. Generate license key using any developer license
3. Register key in application code (App.xaml.cs or MauiProgram.cs)
4. Build server will validate the registered key during build

### Trial Installer

**Scenario:** Build server uses assemblies from trial installer

**Requirements:**
- ✅ License key registration IS required
- Trial installer must be installed on build server
- Register trial license key in application
- 30-day trial period applies

**Note:** Trial license warnings will appear if key is not registered or has expired.

### Licensed Installer

**Scenario:** Build server uses assemblies from licensed installer

**Requirements:**
- ❌ License key registration NOT required
- Licensed installer embeds license in assemblies
- No code changes needed

**Steps:**
1. [Download](https://help.syncfusion.com/maui/installation/web-installer/how-to-download#download-the-license-version) licensed installer
2. [Install](https://help.syncfusion.com/maui/installation/web-installer/how-to-install) on build server
3. Reference Syncfusion assemblies from installation directory
4. Build normally - no license key registration needed

## When to Use Which Approach

### Use NuGet Packages When:
- You want easy dependency management
- You're working in cloud-based build environments
- You don't want to install Syncfusion on every developer machine
- You're comfortable managing license keys in code

**Trade-off:** Requires license key registration in application code.

### Use Licensed Installer When:
- You want to avoid license key management in code
- You have control over build server environment
- You can install software on development machines
- You prefer traditional assembly references

**Trade-off:** Requires installer on each machine.

## Trial vs Licensed Licensing

### Trial License
- **Duration:** 30 days from generation
- **Purpose:** Evaluation of Syncfusion components
- **Renewal:** Can request new trial if expired
- **Limitations:** Time-limited, may show expiration warnings

**Generation:** [Trial & Downloads](https://www.syncfusion.com/account/manage-trials/downloads)

### Licensed License
- **Duration:** Perpetual or subscription-based
- **Purpose:** Production use of Syncfusion components
- **Renewal:** Depends on your license subscription
- **Limitations:** None (for active license)

**Generation:** [License & Downloads](https://www.syncfusion.com/account/downloads)

### Upgrading from Trial to Licensed

After purchasing a license:
1. Generate new license key from [License & Downloads](https://www.syncfusion.com/account/downloads)
2. Replace trial key with licensed key in RegisterLicense() call
3. Clean and rebuild project
4. Trial warnings will be removed

**OR**

1. Uninstall trial version
2. Install licensed version from [License & Downloads](https://www.syncfusion.com/account/downloads)
3. If using licensed installer, no key registration needed

## Offline License Validation

**Important:** Syncfusion license validation does NOT require an internet connection.

- Validation happens **offline** during application execution
- License keys are validated locally within the application
- Apps can be deployed to systems without internet access

This means:
- Your application works in air-gapped environments
- No network latency affects license validation
- No privacy concerns about sending data to external servers
- Deployment doesn't require internet connectivity

## Best Practices

### For Development
1. Generate license key for the exact version and platform you're using
2. Register license key early in application startup
3. Use the same version for all Syncfusion NuGet packages
4. Keep license keys secure (don't commit to public repositories)

### For Build/Deployment
1. For NuGet packages: Register license key in code
2. For licensed installer: Install on build server, no key needed
3. Validate licensing in CI/CD pipeline to catch errors early
4. Ensure Syncfusion.Licensing.dll is included in deployment

### For Team Collaboration
1. Each developer can use their own trial/licensed key
2. All keys should be for same version and platform
3. Consider using environment variables or config files for keys
4. Document licensing requirements in project README

## Common Questions

**Q: Can I use one license key for multiple versions?**
A: No, license keys are version-specific. Each version requires its own key.

**Q: Can I use one license key for multiple platforms?**
A: No, license keys are platform-specific. MAUI, Blazor, WPF each need separate keys.

**Q: What if my trial expires?**
A: You'll see trial expired errors. Purchase a license or request trial extension if eligible.

**Q: Can I share my license key with my team?**
A: For development, each developer should have their own license. For deployment, one key can be used in the deployed application.

## Summary

- License key registration is required for NuGet packages and trial installer only
- License keys are version AND platform specific
- Validation happens offline (no internet required)
- License key ≠ Unlock key (different purposes)
- Build server requirements depend on assembly source
- Trial licenses expire after 30 days; licensed keys don't