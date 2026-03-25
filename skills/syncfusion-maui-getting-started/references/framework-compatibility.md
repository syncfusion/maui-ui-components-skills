# .NET Framework Compatibility

Version compatibility matrix for Syncfusion .NET MAUI components across different .NET framework versions.

## Overview

Syncfusion MAUI controls are compatible with the latest .NET framework versions. This guide helps you choose the right Syncfusion version based on your .NET SDK version.

## Version Compatibility Matrix

### Current Support

| Syncfusion Version | .NET 6.0 | .NET 7.0 | .NET 8.0 | .NET 9.0 | .NET 10.0 |
|-------------------|:--------:|:--------:|:--------:|:--------:|:---------:|
| **>= v19.3.0**    | ✅       | ❌       | ❌       | ❌       | ❌        |
| **>= v23.2.4**    | ❌       | ✅       | ✅       | ❌       | ❌        |
| **>= v27.2.2**    | ❌       | ❌       | ✅       | ✅       | ❌        |
| **>= v31.2.10**   | ❌       | ❌       | ❌       | ✅       | ✅        |

**Legend:**
- ✅ = Supported and tested
- ❌ = Not supported

## Detailed Version Information

### .NET 10.0 (Latest - 2025)

**Syncfusion Version:** v31.2.10 or higher

**Features:**
- Latest performance improvements
- Enhanced hot reload
- Improved memory management
- Latest C# language features

**Recommended for:**
- New projects
- Maximum performance
- Latest framework features

**Installation:**
```bash
# Verify .NET version
dotnet --version
# Should show 10.x.x

# Install latest Syncfusion
dotnet add package Syncfusion.Maui.Core -v 31.2.10
```

### .NET 9.0 (LTS - 2024)

**Syncfusion Version:** v27.2.2 or v31.2.10 or higher

**Features:**
- Long-term support (LTS)
- Stable and mature
- Production-ready
- Extended support lifecycle

**Recommended for:**
- Production applications
- Long-term projects requiring stability
- Enterprise applications

**Installation:**
```bash
# Verify .NET version
dotnet --version
# Should show 9.x.x

# Install Syncfusion
dotnet add package Syncfusion.Maui.Core -v 27.2.2
# or
dotnet add package Syncfusion.Maui.Core -v 31.2.10
```

### .NET 8.0 (LTS - 2023)

**Syncfusion Version:** v23.2.4, v27.2.2, or higher (up to v31.1.x)

**Features:**
- Long-term support until 2026
- Stable for production
- Wide ecosystem support
- Performance optimizations

**Recommended for:**
- Existing projects on .NET 8
- Conservative upgrade path
- Projects requiring LTS

**Installation:**
```bash
# Verify .NET version
dotnet --version
# Should show 8.x.x

# Install Syncfusion
dotnet add package Syncfusion.Maui.Core -v 27.2.2
```

**End of Support:** November 2026

### .NET 7.0 (STS - 2022)

**Syncfusion Version:** v23.2.4 or v27.2.2

**Status:** Standard Term Support (ended May 2024)

**Features:**
- Short-term support
- Performance improvements over .NET 6
- New C# 11 features

**Recommended Action:**
- **Migrate to .NET 8 or .NET 9** for continued support
- Use for legacy projects only

**End of Support:** May 14, 2024 (already ended)

### .NET 6.0 (LTS - 2021)

**Syncfusion Version:** v19.3.0 through v23.1.x

**Status:** LTS (ending November 2024)

**Features:**
- First version with MAUI support
- Long-term support
- Mature and stable

**Recommended Action:**
- **Migrate to .NET 8 or .NET 9** before November 2024
- Remaining projects should plan upgrade

**End of Support:** November 12, 2024

## Choosing the Right Version

### Decision Matrix

| Your Situation | Recommended .NET | Recommended Syncfusion |
|---------------|------------------|------------------------|
| New project | .NET 9 (LTS) or .NET 10 | v31.2.10 or higher |
| Production app (stability priority) | .NET 9 (LTS) | v27.2.2 or v31.2.10 |
| Production app (latest features) | .NET 10 | v31.2.10 or higher |
| Existing .NET 8 project | .NET 8 (LTS) | v27.2.2 |
| Existing .NET 7 project | Upgrade to .NET 9 | v27.2.2 or v31.2.10 |
| Existing .NET 6 project | Upgrade to .NET 9 | v27.2.2 or v31.2.10 |

### LTS vs. STS Considerations

**Long-Term Support (LTS):**
- **Support Duration:** 3 years
- **Updates:** Security and critical bug fixes
- **Best for:** Production applications, enterprise projects
- **Current LTS:** .NET 8 and .NET 9

**Standard Term Support (STS):**
- **Support Duration:** 18 months
- **Updates:** Feature updates and bug fixes
- **Best for:** Projects okay with frequent upgrades
- **Current STS:** .NET 10

## Migration Paths

### From .NET 6 to .NET 9

**Step 1: Update Project File**

```xml
<!-- Before -->
<TargetFramework>net6.0</TargetFramework>

<!-- After -->
<TargetFramework>net9.0</TargetFramework>
```

**Step 2: Update Syncfusion Packages**

```bash
# Update to compatible version
dotnet add package Syncfusion.Maui.Core -v 27.2.2
dotnet add package Syncfusion.Maui.Sliders -v 27.2.2
```

**Step 3: Test Thoroughly**
- Run all unit tests
- Test on all target platforms
- Verify no breaking changes affect your code

**Step 4: Update CI/CD**
- Update SDK version in build pipelines
- Update Docker base images if applicable

### From .NET 8 to .NET 9/10

**Step 1: Update Project File**

```xml
<!-- Change from -->
<TargetFramework>net8.0</TargetFramework>

<!-- To -->
<TargetFramework>net9.0</TargetFramework>
<!-- or -->
<TargetFramework>net10.0</TargetFramework>
```

**Step 2: Update Syncfusion (if needed)**

.NET 8 applications can often use the same Syncfusion version (v27.2.2) for .NET 9, or upgrade to v31.2.10 for .NET 10.

```bash
dotnet add package Syncfusion.Maui.Core -v 31.2.10
```

**Step 3: Review Breaking Changes**
- Check [.NET release notes](https://learn.microsoft.com/en-us/dotnet/core/whats-new/)
- Review [Syncfusion release notes](https://help.syncfusion.com/maui/release-notes/)

## Verification

### Check Current .NET Version

```bash
dotnet --version
```

### Check SDK List

```bash
dotnet --list-sdks
```

### Check Project Target Framework

```bash
# In project directory
cat YourProject.csproj | grep TargetFramework
```

Or in PowerShell:
```powershell
Select-String -Path .\YourProject.csproj -Pattern "TargetFramework"
```

### Check Syncfusion Package Version

```bash
dotnet list package | grep Syncfusion
```

## Support Lifecycle

### .NET Support Dates

| Version | Release Date | End of Support | Type |
|---------|-------------|----------------|------|
| .NET 6.0 | Nov 2021 | Nov 2024 | LTS |
| .NET 7.0 | Nov 2022 | May 2024 | STS |
| .NET 8.0 | Nov 2023 | Nov 2026 | LTS |
| .NET 9.0 | Nov 2024 | Nov 2027 | LTS |
| .NET 10.0 | Nov 2025 | May 2027 | STS |

**Source:** [.NET Support Policy](https://dotnet.microsoft.com/platform/support/policy)

### Syncfusion Release Cycle

**Release Schedule:**
- **4 major releases per year** (Volume 1, 2, 3, 4)
- **Weekly patch releases** (critical fixes)
- **Service packs** between major releases

**Naming Convention:**
- Format: `vMM.V.B.P`
- Example: `v27.2.2`
  - `27` = Year (2027 minus 2000)
  - `2` = Volume (Volume 2)
  - `2` = Service pack/build

## Troubleshooting

### TargetFramework Mismatch

**Error:**
```
error NETSDK1045: The current .NET SDK does not support targeting .NET 9.0
```

**Solution:**
Install the correct .NET SDK version:
```bash
# Download from https://dotnet.microsoft.com/download
# Or use SDK installer
```

### Syncfusion Version Not Compatible

**Error:**
```
Package Syncfusion.Maui.Core 19.3.0 is not compatible with net9.0
```

**Solution:**
Use a compatible Syncfusion version:
```bash
dotnet add package Syncfusion.Maui.Core -v 27.2.2
```

### Multiple SDK Versions Installed

**Issue:** Using wrong SDK version

**Check global.json:**
```json
{
  "sdk": {
    "version": "9.0.100"
  }
}
```

**Or specify in command:**
```bash
dotnet --version
dotnet build --framework net9.0
```

## Best Practices

1. **Use LTS for production** - Prioritize .NET 8 or .NET 9 for production apps
2. **Match versions** - Keep all Syncfusion packages on the same version
3. **Test upgrades** - Thoroughly test after upgrading .NET or Syncfusion versions
4. **Plan migrations** - Don't wait until end-of-support
5. **Monitor releases** - Stay informed about new versions and security updates
6. **Document decisions** - Record why you chose specific versions for your project

## Resources

### Official Documentation

- [.NET Support Policy](https://dotnet.microsoft.com/platform/support/policy/dotnet-core)
- [Syncfusion Release Notes](https://help.syncfusion.com/maui/release-notes/)
- [.NET What's New](https://learn.microsoft.com/en-us/dotnet/core/whats-new/)

### Version Management Tools

- [.NET SDK Version Selector](https://dotnet.microsoft.com/download/visual-studio-sdks)
- [NuGet Package Explorer](https://github.com/NuGetPackageExplorer/NuGetPackageExplorer)

## Related Topics

- [System Requirements](system-requirements.md) - Development environment prerequisites
- [Upgrading](../upgrading/) - Upgrading Syncfusion to newer versions
- [Installation](installation-nuget.md) - Installing Syncfusion packages