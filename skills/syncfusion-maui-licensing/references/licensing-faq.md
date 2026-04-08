# Licensing FAQ and Advanced Topics

## Table of Contents
- [CI/CD License Validation](#cicd-license-validation)
- [Programmatic License Validation](#programmatic-license-validation)
- [Internet Connection Requirements](#internet-connection-requirements)
- [Upgrading from Trial to Licensed](#upgrading-from-trial-to-licensed)
- [NuGet.org User Registration](#nugetorg-user-registration)
- [General FAQ](#general-faq)

Frequently asked questions and advanced topics for Syncfusion .NET MAUI licensing.

## CI/CD License Validation

Validate Syncfusion licenses during Continuous Integration (CI) processes to ensure proper licensing and prevent licensing errors during deployment.

### Why Validate in CI/CD?

**Benefits:**
- Catch licensing errors early in the build process
- Fail builds if license validation fails
- Ensure proper licensing before deployment
- Prevent production licensing issues

**Use Cases:**
- Automated build pipelines
- Release validation
- Multi-environment deployments
- Team collaboration workflows

### LicenseKeyValidator Utility

Syncfusion provides a utility for validating license keys in CI environments.

#### Step 1: Download Utility

Download and extract: `LicenseKeyValidator.zip` // download link - https://s3.amazonaws.com/files2.syncfusion.com/Installs/LicenseKeyValidation/LicenseKeyValidator.zip

**Contents:**
```
LicenseKeyValidator/
├── LicenseKeyValidatorConsole.exe
└── LicenseKeyValidation.ps1
```

#### Step 2: Configure PowerShell Script

Open `LicenseKeyValidation.ps1` in a text editor:

```powershell
# Replace the parameters with the desired platform, version, and actual license key.

$result = & $PSScriptRoot"\LicenseKeyValidatorConsole.exe" /platform:"MAUI" /version:"26.2.4" /licensekey:"Your License Key"

Write-Host $result
```

**Update Parameters:**
- **Platform:** Set to `"MAUI"` for .NET MAUI
- **Version:** Set to your Syncfusion version (e.g., `"26.2.4"`)
- **License Key:** Replace `"Your License Key"` with your actual license key

**Example:**
```powershell
$result = & $PSScriptRoot"\LicenseKeyValidatorConsole.exe" /platform:"MAUI" /version:"26.2.4" /licensekey:"Mgo+DSMBaFt+QHJqVk1hXk5Hd0BLVGpAblJ3T2ZQdVt5ZDU7a15RRnVfR11gSH5Qd0FiWH5dcXE="

Write-Host $result
```

### Azure Pipelines (YAML)

Integrate license validation in Azure Pipelines YAML configuration.

#### Step 1: Create User-Defined Variable

1. Go to Azure Pipeline settings
2. Create variable: `LICENSE_VALIDATION`
3. Value: Path to LicenseKeyValidation.ps1 (e.g., `D:\LicenseKeyValidator\LicenseKeyValidation.ps1`)

Or use the actual path directly in the YAML.

#### Step 2: Add PowerShell Task

```yaml
pool:
  vmImage: 'windows-latest'

steps:
  - task: PowerShell@2
    inputs:
      targetType: 'filePath'
      filePath: $(LICENSE_VALIDATION)  # Or direct path: 'D:\LicenseKeyValidator\LicenseKeyValidation.ps1'
    displayName: 'Syncfusion License Validation'

  # Your other build steps
  - task: DotNetCoreCLI@2
    inputs:
      command: 'build'
      projects: '**/*.csproj'
    displayName: 'Build Projects'
```

**Complete Example:**
```yaml
trigger:
  - main

pool:
  vmImage: 'windows-latest'

variables:
  LICENSE_VALIDATION: 'D:\LicenseKeyValidator\LicenseKeyValidation.ps1'

steps:
  - task: PowerShell@2
    inputs:
      targetType: 'filePath'
      filePath: $(LICENSE_VALIDATION)
    displayName: 'Syncfusion License Validation'
    
  - task: NuGetToolInstaller@1
  
  - task: NuGetCommand@2
    inputs:
      restoreSolution: '**/*.sln'
    
  - task: DotNetCoreCLI@2
    inputs:
      command: 'build'
      projects: '**/*.csproj'
      configuration: 'Release'
```

If validation fails, the pipeline will fail and show the validation error.

### Azure Pipelines (Classic)

For classic Azure Pipelines editor:

#### Step 1: Create Variable

1. Open pipeline in classic editor
2. Go to Variables tab
3. Add variable:
   - Name: `LICENSE_VALIDATION`
   - Value: `D:\LicenseKeyValidator\LicenseKeyValidation.ps1`

#### Step 2: Add PowerShell Task

1. Click "+" to add task
2. Search for "PowerShell"
3. Add "PowerShell" task
4. Configure:
   - Type: **File Path**
   - Script Path: `$(LICENSE_VALIDATION)`
   - Display name: **Syncfusion License Validation**

5. Move task to run before build steps

The pipeline will now validate license before building.

### GitHub Actions

Integrate license validation in GitHub Actions workflows.

#### Step 1: Setup Workflow File

Create or edit `.github/workflows/build.yml`:

```yaml
name: Build and Validate License

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build:
    runs-on: windows-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v3
      
      - name: Syncfusion License Validation
        shell: pwsh
        run: |
          ./path/to/LicenseKeyValidator/LicenseKeyValidation.ps1
      
      - name: Setup .NET
        uses: actions/setup-dotnet@v3
        with:
          dotnet-version: '8.0.x'
      
      - name: Restore dependencies
        run: dotnet restore
      
      - name: Build
        run: dotnet build --configuration Release --no-restore
```

**Important:** 
- Update `./path/to/LicenseKeyValidator/LicenseKeyValidation.ps1` with actual path
- Ensure LicenseKeyValidator folder is committed to repository or downloaded in workflow
- Use `shell: pwsh` to run PowerShell

#### Alternative: Download Validator in Workflow

```yaml
steps:
  - name: Checkout code
    uses: actions/checkout@v3
  
  - name: Download License Validator
    run: |
      Invoke-WebRequest -Uri
       -OutFile "LicenseKeyValidator.zip"
      Expand-Archive -Path "LicenseKeyValidator.zip" -DestinationPath "."
    shell: pwsh
  
  - name: Configure License Validation
    run: |
      $content = Get-Content "LicenseKeyValidation.ps1"
      $content = $content -replace 'platform:".*?"', 'platform:"MAUI"'
      $content = $content -replace 'version:".*?"', 'version:"26.2.4"'
      $content = $content -replace 'licensekey:".*?"', 'licensekey:"${{ secrets.SYNCFUSION_LICENSE_KEY }}"'
      $content | Set-Content "LicenseKeyValidation.ps1"
    shell: pwsh
  
  - name: Validate License
    run: ./LicenseKeyValidation.ps1
    shell: pwsh
```

**Note:** Store license key in GitHub Secrets:
1. Go to repository Settings > Secrets and variables > Actions
2. Add secret: `SYNCFUSION_LICENSE_KEY` with your license key value

### Jenkins

Integrate license validation in Jenkins pipeline.

#### Step 1: Create Environment Variable

In Jenkinsfile or Jenkins configuration:

```groovy
pipeline {
    agent any
    
    environment {
        LICENSE_VALIDATION = 'D:\\LicenseKeyValidator\\LicenseKeyValidation.ps1'
    }
    
    stages {
        stage('Syncfusion License Validation') {
            steps {
                powershell script: "${LICENSE_VALIDATION}"
            }
        }
        
        stage('Build') {
            steps {
                bat 'dotnet build --configuration Release'
            }
        }
        
        stage('Test') {
            steps {
                bat 'dotnet test --no-build --configuration Release'
            }
        }
    }
}
```

**Alternative using Unix-style path:**
```groovy
environment {
    LICENSE_VALIDATION = 'path/to/LicenseKeyValidator/LicenseKeyValidation.ps1'
}

stages {
    stage('Syncfusion License Validation') {
        steps {
            sh 'pwsh ${LICENSE_VALIDATION}'
        }
    }
}
```

## Programmatic License Validation

Validate licenses programmatically using the `ValidateLicense()` method.

### ValidateLicense() Method

**Method Signature:**
```csharp
bool ValidateLicense(Platform platform)
bool ValidateLicense(Platform platform, out string validationMessage)
```

### Basic Usage

```csharp
using Syncfusion.Licensing;

// Register license key first
string licenseKey = Environment.GetEnvironmentVariable("SYNCFUSION_LICENSE_KEY");
if (!string.IsNullOrEmpty(licenseKey))
{   
  Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense(licenseKey);
}

// Validate the registered license key
bool isValid = SyncfusionLicenseProvider.ValidateLicense(Platform.MAUI);

if (isValid)
{
    Console.WriteLine("License is valid");
}
else
{
    Console.WriteLine("License validation failed");
}
```

### With Validation Message

```csharp
using Syncfusion.Licensing;

// Register license key
string licenseKey = Environment.GetEnvironmentVariable("SYNCFUSION_LICENSE_KEY");
if (!string.IsNullOrEmpty(licenseKey))
{   
  SyncfusionLicenseProvider.RegisterLicense(licenseKey);
}

// Validate and get detailed message
bool isValid = SyncfusionLicenseProvider.ValidateLicense(Platform.MAUI, out string validationMessage);

if (isValid)
{
    Console.WriteLine($"License is valid: {validationMessage}");
}
else
{
    Console.WriteLine($"License validation failed: {validationMessage}");
    // Handle invalid license (log, alert, fail deployment, etc.)
}
```

### Unit Test Validation

Create unit tests to validate licensing during automated testing.

#### Step 1: Create Unit Test Project

**Option 1: Visual Studio**
1. File → New → Project
2. Search for "Test"
3. Select test framework (MSTest, NUnit, xUnit)
4. Create project

**Option 2: CLI**
```bash
# MSTest
dotnet new mstest -n YourApp.LicenseTests

# NUnit
dotnet new nunit -n YourApp.LicenseTests

# xUnit
dotnet new xunit -n YourApp.LicenseTests
```

#### Step 2: Add Syncfusion References

```bash
cd YourApp.LicenseTests
dotnet add package Syncfusion.Licensing
dotnet add reference ../YourApp/YourApp.csproj
```

#### Step 3: Create License Validation Test

**MSTest Example:**
```csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using Syncfusion.Licensing;

namespace YourApp.LicenseTests
{
    [TestClass]
    public class SyncfusionLicenseTests
    {
        [TestMethod]
        public void TestSyncfusionMAUILicense()
        {
            var platform = Platform.MAUI;
            
            // Register the Syncfusion license key
            string licenseKey = Environment.GetEnvironmentVariable("SYNCFUSION_LICENSE_KEY");
            if (!string.IsNullOrEmpty(licenseKey))
            {   
                Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense(licenseKey);
            }

            // Validate license
            bool isValidLicense = SyncfusionLicenseProvider.ValidateLicense(
                platform, 
                out string validationMessage
            );
            
            // Assert license is valid
            Assert.IsTrue(
                isValidLicense, 
                $"Validation failed for {platform}. Validation Message: {validationMessage}"
            );

            // Log success message
            TestContext.WriteLine($"Platform {platform} is correctly licensed for version {typeof(SyncfusionLicenseProvider).Assembly.GetName().Version}");
        }
        
        public TestContext TestContext { get; set; }
    }
}
```

**NUnit Example:**
```csharp
using NUnit.Framework;
using Syncfusion.Licensing;

namespace YourApp.LicenseTests
{
    [TestFixture]
    public class SyncfusionLicenseTests
    {
        [Test]
        public void TestSyncfusionMAUILicense()
        {
            var platform = Platform.MAUI;
            
            // Register license
            string licenseKey = Environment.GetEnvironmentVariable("SYNCFUSION_LICENSE_KEY");
            if (!string.IsNullOrEmpty(licenseKey))
            {   
                SyncfusionLicenseProvider.RegisterLicense(licenseKey);
            }

            // Validate
            bool isValidLicense = SyncfusionLicenseProvider.ValidateLicense(
                platform, 
                out string validationMessage
            );
            
            // Assert
            Assert.That(
                isValidLicense, 
                Is.True, 
                $"Validation failed for {platform}. Validation Message: {validationMessage}"
            );

            // Log
            if (isValidLicense)
            {
                TestContext.Out.WriteLine($"Platform {platform} is correctly licensed for version {typeof(SyncfusionLicenseProvider).Assembly.GetName().Version}");
            }
        }
    }
}
```

#### Step 4: Run Tests

```bash
# Run all tests
dotnet test

# Run specific test
dotnet test --filter TestSyncfusionMAUILicense
```

### Success Output

When license validation passes:
```
✅ Test Passed: TestSyncfusionMAUILicense
Platform MAUI is correctly licensed for version 26.2.4.0
```

### Failure Output

When license validation fails:
```
❌ Test Failed: TestSyncfusionMAUILicense
Validation failed for MAUI. 
Validation Message: Invalid license key - version mismatch
```

### CI/CD Integration with Unit Tests

**Azure Pipelines:**
```yaml
- task: DotNetCoreCLI@2
  inputs:
    command: 'test'
    projects: '**/*LicenseTests.csproj'
  displayName: 'Run License Validation Tests'
```

**GitHub Actions:**
```yaml
- name: Run License Tests
  run: dotnet test YourApp.LicenseTests/YourApp.LicenseTests.csproj
```

**Jenkins:**
```groovy
stage('License Tests') {
    steps {
        bat 'dotnet test YourApp.LicenseTests\\YourApp.LicenseTests.csproj'
    }
}
```

## Internet Connection Requirements

### Does Licensing Require Internet?

**No.** Syncfusion license validation is performed **offline** during application execution.

**Key Points:**
- License validation happens locally within the application
- No network calls to external servers
- No data sent to Syncfusion servers during validation

### Deployment Without Internet

Applications registered with Syncfusion license keys can be deployed to systems without internet connectivity.

**Scenarios:**
- Air-gapped environments
- Offline kiosks
- Internal corporate networks
- Remote/isolated locations
- High-security environments

**Requirements:**
1. License key must be registered in application code
2. Syncfusion.Licensing.dll must be included in deployment
3. All Syncfusion assemblies must be deployed
4. No runtime internet access needed

### Online Operations

**What DOES require internet:**
- **Generating license keys** from Syncfusion account (one-time, during development)
- **Downloading Syncfusion installers** (one-time, during setup)
- **Accessing Syncfusion documentation** (optional, development-time)

**What does NOT require internet:**
- **License validation** at runtime (offline)
- **Running applications** with registered licenses (offline)
- **Deploying applications** to end users (offline)

## Upgrading from Trial to Licensed

### After Purchasing a License

You have two options for upgrading from trial to licensed version:

### Option 1: Replace License Key (Recommended)

If using NuGet packages:

**Step 1: Generate Licensed Key**
1. Go to [License & Downloads](https://www.syncfusion.com/account/downloads)
2. Select Platform: .NET MAUI
3. Select Version: Your current version
4. Generate license key

**Step 2: Update Application**
Replace trial key with licensed key:

```csharp
// Before (trial key)
// SyncfusionLicenseProvider.RegisterLicense("TRIAL_KEY_HERE");

// After (licensed key)
SyncfusionLicenseProvider.RegisterLicense("LICENSED_KEY_HERE");
```

**Step 3: Clean and Rebuild**
```bash
dotnet clean
dotnet build
```

**Step 4: Verify**
Run application. Trial warning should be gone.

**Benefits:**
- Quick and easy
- No reinstallation needed
- Works with NuGet packages
- Minimal code changes

### Option 2: Install Licensed Version

If using installer:

**Step 1: Uninstall Trial Version**
1. Go to Windows Settings → Apps
2. Find "Syncfusion Essential Studio" trial version
3. Uninstall

**Step 2: Download Licensed Installer**
1. Go to [License & Downloads](https://www.syncfusion.com/account/downloads)
2. Download licensed installer for .NET MAUI
3. Run installer
4. Use your unlock key during installation

**Step 3: Update Project References**
1. Remove old trial assembly references
2. Add references to licensed assemblies
3. Update paths if necessary

**Step 4: Remove License Key Registration (Optional)**
If using licensed installer assemblies (not NuGet), you can remove RegisterLicense() call:

```csharp
// Can be removed when using licensed installer
// SyncfusionLicenseProvider.RegisterLicense("KEY");
```

**Step 5: Clean and Rebuild**
```bash
dotnet clean
dotnet build
```

**Benefits:**
- No license key registration needed in code
- License embedded in assemblies
- Traditional reference model

### Important Notes

**License Key Not Required:**
- When referencing assemblies from **licensed installer**
- License is embedded in the assemblies themselves

**License Key Required:**
- When using **NuGet packages** from nuget.org (even with paid license)
- When using **trial installer** assemblies

**After Upgrade:**
- Trial warnings will be removed
- No expiration messages
- Full license benefits apply

## NuGet.org User Registration

If you installed Syncfusion packages directly from NuGet.org but don't have a Syncfusion account:

### Step 1: Create Syncfusion Account

1. Go to [Syncfusion Registration](https://www.syncfusion.com/account/register)
2. Fill in registration form:
   - Name
   - Email address
   - Password
   - Company (optional)
3. Verify email address
4. Log in to your new account

### Step 2: Start Free Trial

1. Navigate to [Start Trials](https://www.syncfusion.com/account/manage-trials/start-trials)
2. Find "Essential Studio for .NET MAUI"
3. Click "Start Trial"
4. Accept terms and conditions
5. Trial activated (30 days free)

### Step 3: Generate License Key

1. Go to [Trial & Downloads](https://www.syncfusion.com/account/manage-trials/downloads)
2. Find license key generation section
3. Select:
   - Platform: **.NET MAUI**
   - Version: **Your installed version** (check NuGet packages)
4. Click "Generate License Key"
5. Copy the generated key

### Step 4: Register in Application

See [license-registration.md](license-registration.md) for detailed registration instructions.

**Quick registration:**
```csharp
using Syncfusion.Licensing;

public App()
{
    string licenseKey = Environment.GetEnvironmentVariable("SYNCFUSION_LICENSE_KEY");
    if (!string.IsNullOrEmpty(licenseKey))
    {   
       Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense(licenseKey);
    }
    InitializeComponent();
}
```

### Step 5: Verify

Run application. Trial license warning should be removed (for 30 days).

### After Trial Expires

**Option 1: Purchase License**
1. Go to [Syncfusion Sales](https://www.syncfusion.com/sales/products)
2. Choose appropriate license
3. Complete purchase
4. Generate licensed key
5. Update application with new key

**Option 2: Continue Evaluation**
Contact Syncfusion support for trial extension eligibility.

## General FAQ

### Q: Where can I get a license key?

**A:** License keys can be generated from:
- **Licensed users:** [License & Downloads](https://www.syncfusion.com/account/downloads)
- **Trial users:** [Trial & Downloads](https://www.syncfusion.com/account/manage-trials/downloads)

See [license-generation.md](license-generation.md) for detailed instructions.

### Q: Do I need different keys for development and production?

**A:** No. You can use the same license key for development, testing, and production environments. However:
- Each developer should have their own trial/licensed key during development
- Production deployments can use a single registered key
- Ensure the key version matches your deployed package version

### Q: Can I use the same license key across multiple applications?

**A:** Yes, within the terms of your license. One license key can be used across multiple applications as long as:
- All applications use the same Syncfusion version
- All applications are on the same platform (MAUI)
- Usage complies with your license agreement terms

### Q: What happens if I don't register a license key?

**A:** If you're using NuGet packages or trial installer:
- You'll see "This application was built using a trial version" warning
- Warning appears at runtime in your application
- Application still works but shows licensing messages to end users

### Q: How do I know which version license key to use?

**A:** Check your NuGet package versions:
```bash
dotnet list package | findstr Syncfusion
```

Generate a license key for the exact version shown. See this [KB article](https://support.syncfusion.com/kb/article/7865/which-version-syncfusion-license-key-should-i-use-in-my-application) for more details.

### Q: Can I use a Blazor license key for MAUI?

**A:** No. License keys are platform-specific. You need a MAUI-specific license key for MAUI applications. Using a different platform's key will result in "Platform mismatch" error.

### Q: What's the difference between trial and licensed keys?

**A:**
- **Trial Key:** Valid for 30 days, free evaluation, expires after 30 days
- **Licensed Key:** Valid for your license subscription period, no expiration for active licenses, requires purchase

## Summary

**CI/CD Validation:**
- Use LicenseKeyValidator utility for automated validation
- Integrate in Azure Pipelines, GitHub Actions, or Jenkins
- Use ValidateLicense() method for programmatic checks
- Create unit tests for license validation

**Internet Requirements:**
- License validation is **offline** (no internet needed)
- Applications work in air-gapped environments
- Internet only needed for key generation (one-time)

**Upgrading from Trial:**
- Option 1: Replace trial key with licensed key (NuGet users)
- Option 2: Install licensed version (installer users)
- Licensed installer doesn't require key registration

**NuGet.org Users:**
- Register for Syncfusion account
- Start free trial (30 days)
- Generate and register license key
- Purchase after trial expires

**General Tips:**
- Keys are version and platform specific
- Use same key across multiple apps (within license terms)
- Validate before deployment to catch errors early
- Store keys securely (environment variables, configs)