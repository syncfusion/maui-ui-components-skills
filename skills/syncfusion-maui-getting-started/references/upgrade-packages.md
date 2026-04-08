# Upgrading Syncfusion Packages

## Overview

Keep Syncfusion .NET MAUI components up-to-date for bug fixes, new features, and performance improvements.

**Upgrade Frequency:** Major releases quarterly, patch releases as needed

## Version Compatibility

### Syncfusion Versioning

**Format:** `Major.Minor.Patch.Build`

**Example:** `25.1.35.0`
- `25` - Year (2025)
- `1` - Quarter (Q1)
- `35` - Week
- `0` - Build

### Framework Compatibility

| Syncfusion Version | .NET MAUI Version | .NET Version |
|--------------------|-------------------|--------------|
| 24.1.x+ | .NET MAUI 8 | .NET 8.0+ |
| 23.1.x - 23.2.x | .NET MAUI 7/8 | .NET 7.0+ |
| 22.1.x - 22.2.x | .NET MAUI 6 | .NET 6.0+ |
| 20.2.x+ | .NET MAUI 6 RC+ | .NET 6.0+ |

**Rule:** Match Syncfusion version to .NET MAUI/SDK version.

## Checking Current Version

### In .csproj File

```xml
<ItemGroup>
  <PackageReference Include="Syncfusion.Maui.Core" Version="24.2.9" />
  <PackageReference Include="Syncfusion.Maui.DataGrid" Version="24.2.9" />
</ItemGroup>
```

### Via CLI

```bash
dotnet list package
```

**Output:**
```
Project 'MyApp' has the following package references
   [net8.0-android]:
   Top-level Package                  Requested
   > Syncfusion.Maui.Core            24.2.9
   > Syncfusion.Maui.DataGrid        24.2.9
```

## Upgrade Methods

### Method 1: Visual Studio NuGet Manager

**Steps:**
1. Right-click project → **Manage NuGet Packages**
2. Go to **Updates** tab
3. Select Syncfusion packages to update
4. Click **Update**
5. Accept license if prompted
6. Rebuild solution

**Benefits:**
- Visual interface
- See available versions
- Update multiple packages at once

### Method 2: .NET CLI

**Update all Syncfusion packages:**

```bash
dotnet add package Syncfusion.Maui.Core --version 25.1.35
dotnet add package Syncfusion.Maui.DataGrid --version 25.1.35
```

**Update to latest version:**

```bash
dotnet add package Syncfusion.Maui.Core
```

(Omit `--version` to get latest)

### Method 3: Edit .csproj Directly

**Before:**
```xml
<PackageReference Include="Syncfusion.Maui.Core" Version="24.2.9" />
```

**After:**
```xml
<PackageReference Include="Syncfusion.Maui.Core" Version="25.1.35" />
```

**Then restore:**
```bash
dotnet restore
```

### Method 4: Package Manager Console

**In Visual Studio:**

```powershell
Update-Package Syncfusion.Maui.Core -Version 25.1.35
Update-Package Syncfusion.Maui.DataGrid -Version 25.1.35
```

**Update all Syncfusion packages:**
```powershell
Update-Package Syncfusion.Maui.* -Version 25.1.35
```

## Version Uniformity

**CRITICAL:** All Syncfusion packages MUST use same version.

**❌ INCORRECT:**
```xml
<PackageReference Include="Syncfusion.Maui.Core" Version="24.2.9" />
<PackageReference Include="Syncfusion.Maui.DataGrid" Version="25.1.35" />
```

**✅ CORRECT:**
```xml
<PackageReference Include="Syncfusion.Maui.Core" Version="25.1.35" />
<PackageReference Include="Syncfusion.Maui.DataGrid" Version="25.1.35" />
```

**Why:** Mixed versions cause runtime errors and API mismatches.

## Post-Upgrade Steps

### 1. Clean and Rebuild

```bash
dotnet clean
dotnet build
```

**In Visual Studio:**
- **Build** → **Clean Solution**
- **Build** → **Rebuild Solution**

### 2. Clear Caches (if issues persist)

**Delete bin/ and obj/ folders:**
```bash
rm -rf bin obj
```

**Clear NuGet cache:**
```bash
dotnet nuget locals all --clear
```

### 3. Update License Key (if applicable)

**If upgrading from trial to paid, or renewing license:**

Update license key in **MauiProgram.cs:**

```csharp
// Read from environment variable - SECURITY BEST PRACTICE
string licenseKey = Environment.GetEnvironmentVariable("SYNCFUSION_LICENSE_KEY");
if (!string.IsNullOrEmpty(licenseKey))
{
    Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense(licenseKey);
}
```

See: license-registration.md

### 4. Test Critical Functionality

- Run app on all target platforms
- Test features using upgraded components
- Check for deprecated API warnings

## Breaking Changes

### Major Version Changes

When upgrading across major versions (e.g., 24.x → 25.x), review release notes for breaking changes.

**Common Breaking Changes:**
- API renames
- Property changes
- Removed properties/methods
- Namespace changes

**Example (Hypothetical):**
```csharp
// Old (24.x)
dataGrid.SelectionMode = SelectionMode.Single;

// New (25.x)
dataGrid.SelectionMode = DataGridSelectionMode.Single;
```

### Finding Breaking Changes

**Syncfusion Release Notes:**
- Visit: [help.syncfusion.com](https://help.syncfusion.com) → Release Notes
- Search for ".NET MAUI" → Select version
- Review "Breaking Changes" section

**Compiler Warnings:**
After upgrade, check for warnings:
```
Warning: 'PropertyName' is obsolete: 'Use NewPropertyName instead'
```

## Rollback if Needed

**If upgrade causes issues, rollback:**

```bash
dotnet add package Syncfusion.Maui.Core --version 24.2.9
```

Or edit .csproj to previous version, then:
```bash
dotnet restore
dotnet clean
dotnet build
```

## Upgrade Best Practices

✅ **Increment one major version at a time** (24.x → 25.x, not 23.x → 25.x)

✅ **Read release notes** before upgrading

✅ **Test in development** before production

✅ **Keep all packages same version**

✅ **Backup project** or use version control

❌ **Don't skip testing** after upgrade

❌ **Don't mix Syncfusion versions**

## Automated Upgrade Checking

### Dependabot (GitHub)

**.github/dependabot.yml:**
```yaml
version: 2
updates:
  - package-ecosystem: "nuget"
    directory: "/"
    schedule:
      interval: "weekly"
    allow:
      - dependency-name: "Syncfusion.Maui.*"
```

### Renovate (GitLab/GitHub)

**renovate.json:**
```json
{
  "packageRules": [
    {
      "matchPackagePatterns": ["^Syncfusion\\.Maui\\."],
      "groupName": "Syncfusion MAUI packages",
      "schedule": ["before 3am on Monday"]
    }
  ]
}
```

## Troubleshooting Upgrades

### Error: Version Conflict

**Message:** `Package 'Syncfusion.Maui.Core 25.1.35' has a dependency on 'Syncfusion.Licensing 25.1.35'`

**Solution:** Update ALL Syncfusion packages to same version.

### Error: License Invalid

**Message:** `The included Syncfusion license is invalid`

**Solution:**
- Verify license key matches version
- Generate new key if version > license coverage
- See: license-generation.md

### Build Errors After Upgrade

**Solution:**
1. Check release notes for breaking changes
2. Update deprecated APIs
3. Clean and rebuild
4. Clear NuGet cache
5. Delete bin/obj folders

### App Crashes at Runtime

**Solution:**
- Check for missing configurations
- Verify `ConfigureSyncfusionCore()` called
- Review xaml namespace declarations
- Test on physical device (not just emulator)

## Release Channels

**Stable Releases:** Quarterly (recommended)
**Patch Releases:** As needed (bug fixes)
**Preview Releases:** Beta features (not recommended for production)

**Stick to stable releases** for production apps.

## Staying Informed

**Subscribe to Updates:**
- Syncfusion blog
- GitHub repository releases
- NuGet.org package pages

**Check Regularly:**
```bash
dotnet list package --outdated
```

**Output shows available updates:**
```
Project 'MyApp' has the following updates to its packages
   [net8.0-android]:
   Top-level Package           Requested   Latest
   > Syncfusion.Maui.Core     24.2.9      25.1.35
```

## Related Files

- **Installation:** installation-nuget.md
- **Licensing:** licensing-overview.md
- **Errors:** installation-errors.md