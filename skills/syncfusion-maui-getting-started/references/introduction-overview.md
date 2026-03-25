# Introduction to Syncfusion .NET MAUI

## Table of Contents
- [What is Syncfusion . NET MAUI](#what-is-syncfusion-net-maui)
- [Cross-Platform Capabilities](#cross-platform-capabilities)
- [Component Ecosystem Overview](#component-ecosystem-overview)
- [Supported Platforms](#supported-platforms)
- [System Requirements](#system-requirements)
- [Framework Compatibility](#framework-compatibility)
- [Development Environment Setup](#development-environment-setup)
- [How to Use This User Guide](#how-to-use-this-user-guide)

---

## What is Syncfusion .NET MAUI

Syncfusion Essential Studio for .NET MAUI is a comprehensive collection of .NET MAUI components designed for building modern mobile and desktop applications from a single shared codebase. Built on Microsoft's .NET Multi-platform App UI (.NET MAUI) framework, it provides high-performance, feature-rich UI controls that work seamlessly across multiple platforms.

### Key Features

- **70+ Professional Components:** Complete suite of grids, charts, gauges, editors, calendars, schedulers, and more
- **Single Codebase:** Write once, deploy to Android, iOS, macOS (Mac Catalyst), and Windows (WinUI 3)
- **Native Performance:** Platform-specific optimizations ensure smooth, responsive user experiences
- **Modern Design:** Material Design and Cupertino (iOS-style) themes with automatic light/dark mode support
- **AI-Powered Components:** Smart Components with integrated AI service support
- **Enterprise-Ready:** Production-tested, WCAG-compliant, with comprehensive documentation and support

### Why Choose Syncfusion .NET MAUI

- **Accelerated Development:** Pre-built, production-ready components reduce development time
- **Consistent Design:** Uniform look-and-feel across all platforms with theme support
- **Extensive Customization:** Flexible APIs, styling options, and templating capabilities
- **Professional Support:** Access to knowledge base, forums, and dedicated support team
- **Regular Updates:** Frequent releases with new features, improvements, and bug fixes

---

## Cross-Platform Capabilities

### Supported Platforms

Syncfusion .NET MAUI components support development for:

1. **Android** - Native Android applications (API 21+)
2. **iOS** - Native iOS applications (iOS 12.2+)
3. **macOS** - Mac desktop applications via Mac Catalyst (macOS 12+)
4. **Windows** - Windows desktop applications via WinUI 3 (Windows 10 1809+, Windows 11)

### Architecture

.NET MAUI uses a single project system where:
- **Shared Code:** Business logic, UI layouts, and component usage in one codebase
- **Platform-Specific:** Platform-specific code when needed (handled by .NET MAUI)
- **Resource Management:** Unified resource system for images, fonts, and assets
- **Native APIs:** Access to platform-specific APIs when required

### Cross-Platform Development Benefits

- **Code Reusability:** 90%+ code sharing across platforms
- **Faster Time-to-Market:** Build for all platforms simultaneously
- **Consistent UX:** Uniform user experience with platform-specific adaptations
- **Easier Maintenance:** Single codebase reduces maintenance overhead
- **Cost-Effective:** One development team can target multiple platforms

---

## Component Ecosystem Overview

### Data Visualization Components

**Charts**
- Cartesian Charts (Line, Column, Bar, Area, Spline, Step, Range, etc.)
- Circular Charts (Pie, Doughnut, Radial Bar)
- Funnel and Pyramid Charts
- Polar Charts (Polar, Radar)

**Gauges**
- Linear Gauge (horizontal/vertical progress indicators)
- Radial Gauge (circular progress, speedometers)

**Maps**
- Interactive maps with bubble, shape, and marker layers
- Data visualization on geographical maps

**Barcode Generator**
- QR Code, DataMatrix, Code128, and 40+ barcode types

### Grids & Data Components

**DataGrid**
- High-performance data grid with virtualization
- Sorting, filtering, grouping, and aggregation
- Inline editing and cell templates
- Excel-like features

**ListView**
- Itemized list display with grouping
- Pull-to-refresh and load-more
- Item templates and layouts

**TreeView**
- Hierarchical data display
- Expand/collapse nodes
- Checkboxes and templates

### Editors & Inputs

**TextInputLayout** - Material-design text input with floating labels  
**ComboBox** - Dropdown selection with autocomplete  
**Autocomplete** - Incremental search with suggestions  
**DatePicker** - Date selection control  
**TimePicker** - Time selection control  
**DateRangePicker** - Select date ranges  
**NumericEntry** - Numeric input with formatting  
**MaskedEntry** - Input with custom masks  
**Rating** - Star rating input  
**Slider** - Value selection via slider  
**Range Slider** - Select value ranges

### Calendars & Scheduling

**Calendar** - Month/day/year view with selection  
**Scheduler** - Appointment scheduling with day/week/month/Agenda/timeline views  
**DateRangePicker** - Select start and end dates

### Navigation & Layout

**TabView** - Tabbed navigation  
**NavigationDrawer** - Slide-out navigation menu  
**Carousel** - Swipeable item carousel  
**Accordion** - Expandable/collapsible panels  
**TreeView** - Hierarchical navigation  
**Popup** - Modal and non-modal popups  
**Shimmer** - Loading placeholders

### Buttons & Indicators

**Button** - Customizable buttons with icons, styles  
**Checkbox** - Checkboxes with intermediate states  
**RadioButton** - Radio button selection  
**Switch** - Toggle switch  
**SegmentedControl** - Segmented button group  
**Badge** - Notification badges  
**BusyIndicator** - Loading indicators  
**ProgressBar** - Progress indication  
**AvatarView** - User avatars

### Additional Components

**SignaturePad** - Capture signatures  
**EffectsView** - Visual effects (blur, shadow, glow)  
**Backdrop** - Material backdrop layer  
**Chips** - Material chips/tags  
**PullToRefresh** - Refresh gesture  
**PDF Viewer** - Display and annotate PDFs  
**Image Editor** - Edit images with filters and effects

---

## Supported Platforms

### Platform Details

| Platform | Minimum Version | Recommended Version | Notes |
|----------|----------------|---------------------|-------|
| **Android** | API 21 (Android 5.0) | API 31+ (Android 12+) | Targets Android devices |
| **iOS** | iOS 12.2 | iOS 15+ | Targets iPhones, iPads |
| **macOS** | macOS 12 (Monterey) | macOS 13+ (Ventura) | Via Mac Catalyst |
| **Windows** | Windows 10 1809 | Windows 11 | Via WinUI 3 |
|  | Windows Server 2016+ | Windows Server 2022 | Server applications |

### Platform Support Matrix

All Syncfusion .NET MAUI components support all four platforms (Android, iOS, Mac Catalyst, WinUI) unless otherwise specified in component-specific documentation.

### Platform-Specific Considerations

**Android:**
- Requires Android SDK installed
- Min API level 21 (Lollipop)
- Supports all Android form factors (phone, tablet, TV)

**iOS:**
- Requires macOS with Xcode for building
- Supports iPhone and iPad
- iOS 12.2+ required

**macOS:**
- Mac Catalyst translates iOS apps to macOS
- macOS 12+ (Monterey or later)
- Full desktop capabilities

**Windows:**
- Windows 10 version 1809 or higher
- WinUI 3 (Windows App SDK)
- Windows 11 recommended for best performance

---

## System Requirements

### Hardware Requirements

**Minimum:**
- Processor: x86 or x64
- RAM: 4 GB
- Hard disk: 210 GB free space (for full development environment)

**Recommended:**
- Processor: x64 multi-core
- RAM: 16 GB
- Hard disk: SSD with 250 GB+ free space
- Graphics: Dedicated GPU for emulator performance

### Operating System Requirements

**Windows Development:**
- Windows 11 version 21H2 or higher (Home, Pro, Enterprise, Education)
- Windows 10 version 1909 or higher (Home, Professional, Enterprise, Education)
- Windows Server 2022 (Standard, Datacenter)
- Windows Server 2019 (Standard, Datacenter)
- Windows Server 2016 (Standard, Datacenter)

**macOS Development:**
- macOS 12 (Monterey) or higher
- Xcode 13+ (for iOS/macOS development)

### Development Tools

**Visual Studio 2022 (Windows/Mac):**
- Visual Studio 2022 17.8.0 or later
- .NET MAUI workload installed
- Android SDK (for Android development)
- Xcode (macOS only, for iOS/macOS development)

**Visual Studio 2026:**
- Visual Studio 2026 18.0.0 or later
- Latest .NET MAUI tooling

**Visual Studio Code:**
- VS Code with .NET MAUI extension
- .NET SDK 6.0+ installed
- Platform SDKs installed separately

---

## Framework Compatibility

### Supported .NET Versions

Syncfusion .NET MAUI components support:
- **.NET 9.0** - Current recommended version
- **.NET 10.0** - Latest version

### Version Compatibility Matrix

| Syncfusion Version | .NET 6.0 | .NET 7.0 | .NET 8.0 | .NET 9.0 | .NET 10.0 |
|-------------------|----------|----------|----------|----------|-----------|
| >= v19.3.0 | Yes | No | No | No | No |
| >= v23.2.4 | No | Yes | Yes | No | No |
| >= v27.2.2 | No | No | Yes | Yes | No |
| >= v31.2.10 | No | No | No | Yes | Yes |

### Recommendations

- **Use .NET 9.0 or .NET 10.0:** Latest Syncfusion versions target current .NET releases
- **Match Versions:** Ensure Syncfusion package version aligns with your .NET SDK version
- **LTS Support:** For long-term projects, use .NET LTS (Long-Term Support) versions
- **Stay Updated:** Newer .NET versions provide performance improvements and new features

---

## Development Environment Setup

### Windows Setup

1. **Install Visual Studio 2022 (17.8.0+):**
   - Download from visualstudio.microsoft.com
   - Select ".NET Multi-platform App UI development" workload during installation
   - Include Android SDK and Android emulators

2. **Install .NET SDK:**
   - .NET 9.0 or .NET 10.0 SDK
   - Verify installation: `dotnet --version`

3. **Configure Android Development:**
   - Install Android SDK via Visual Studio
   - Set up Android emulator or connect physical device
   - Enable USB debugging on device

4. **Install Syncfusion Components:**
   - Via NuGet Package Manager
   - Or install full Syncfusion installer

### macOS Setup

1. **Install Visual Studio for Mac (2022 8.10+):**
   - Download from visualstudio.microsoft.com/vs/mac
   - Select .NET MAUI workload

2. **Install Xcode (13+):**
   - Required for iOS and macOS development
   - Install from Mac App Store
   - Open Xcode once to accept license

3. **Install .NET SDK:**
   - .NET 9.0 or .NET 10.0
   - Verify: `dotnet --version`

4. **Install Syncfusion Components:**
   - Via NuGet Package Manager
   - Or use Mac installer

### Visual Studio Code Setup

1. **Install VS Code:**
   - Download from code.visualstudio.com

2. **Install .NET MAUI Extension:**
   - Open Extensions panel (Ctrl+Shift+X / Cmd+Shift+X)
   - Search for ".NET MAUI"
   - Install official .NET MAUI extension

3. **Install .NET SDK:**
   - .NET 9.0 or .NET 10.0 SDK

4. **Install Platform SDKs:**
   - Android SDK for Android development
   - Xcode for iOS/macOS development (macOS only)

5. **Install Syncfusion Components:**
   - Via .NET CLI: `dotnet add package Syncfusion.Maui.Core`

---

## How to Use This User Guide

### For New Users

1. **Start Here:** Read this introduction to understand Syncfusion .NET MAUI capabilities
2. **Installation:** Follow installation guides to set up NuGet packages or installers
3. **Licensing:** Generate and register your license key
4. **Themes:** Apply Material or Cupertino themes to your application
5. **Component-Specific Documentation:** Navigate to specific component skills for implementation details

### For Experienced Developers

- **Search:** Use semantic search to find specific features and APIs quickly
- **Quick Reference:** Jump directly to component-specific documentation
- **API Reference:** Detailed class and member documentation available
- **Code Examples:** Sample browser contains real-world usage examples

### Best Practices for Reading

- **Getting Started Sections:** Always read component-specific "Getting Started" sections end-to-end
- **Integration:** Use provided code examples to accelerate integration
- **Search for Features:** Use search to find detailed information on specific features
- **API Reference:** Consult API docs for detailed technical information

### Additional Resources

- **Knowledge Base:** Frequently asked questions and solutions
- **Forums:** Community discussions and peer support
- **Support Tickets:** Direct support from Syncfusion team
- **Feedback Portal:** Request new features or improvements

---

## Quick Start Checklist

✅ Verify system meets minimum requirements  
✅ Install Visual Studio 2022 17.8.0+ with .NET MAUI workload  
✅ Install .NET 9.0 or .NET 10.0 SDK  
✅ Install platform-specific SDKs (Android SDK, Xcode)  
✅ Install Syncfusion NuGet packages  
✅ Generate and register license key  
✅ Apply theme (MaterialLight/MaterialDark/CupertinoLight/CupertinoDark)  
✅ Start building with Syncfusion components!

For detailed installation instructions, read the installation-nuget.md or installation-installers.md reference files.