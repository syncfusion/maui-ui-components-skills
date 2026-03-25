# Troubleshooting Common Issues

This guide covers common problems you might encounter when working with SfMarkdownViewer and their solutions.

## Installation and Setup Issues

### Handler Not Registered Error

**Symptom:**
```
System.InvalidOperationException: Handler not found for control 'SfMarkdownViewer'
```

**Cause:** Syncfusion handler not registered in `MauiProgram.cs`

**Solution:**

1. Open `MauiProgram.cs`
2. Add the `using` directive:
```csharp
using Syncfusion.Maui.Core.Hosting;
```

3. Add `.ConfigureSyncfusionCore()` to the builder:
```csharp
public static MauiApp CreateMauiApp()
{
    var builder = MauiApp.CreateBuilder();
    builder
        .UseMauiApp<App>()
        .ConfigureSyncfusionCore()  // Add this line
        .ConfigureFonts(fonts =>
        {
            fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
        });
    return builder.Build();
}
```

4. Clean and rebuild your project

---

### NuGet Package Not Found

**Symptom:**
```
Package 'Syncfusion.Maui.MarkdownViewer' not found
```

**Solutions:**

**Option 1: Clear NuGet Cache**
```bash
dotnet nuget locals all --clear
dotnet restore
```

**Option 2: Verify NuGet Sources**
```bash
dotnet nuget list source
```
Ensure `nuget.org` is in the list.

**Option 3: Manual Installation**
1. Visit [nuget.org](https://www.nuget.org/packages/Syncfusion.Maui.MarkdownViewer)
2. Download the `.nupkg` file
3. Add as local package source

**Option 4: Check Internet Connection**
- Verify you can access nuget.org
- Check firewall/proxy settings
- Try from a different network

---

### Namespace Not Found Error

**Symptom:**
```csharp
The type or namespace name 'Syncfusion' could not be found
```

**Solutions:**

1. **Verify package is installed:**
```bash
dotnet list package
```
Look for `Syncfusion.Maui.MarkdownViewer` in the list.

2. **Clean and rebuild:**
```bash
dotnet clean
dotnet build
```

3. **Check .csproj file:**
```xml
<ItemGroup>
  <PackageReference Include="Syncfusion.Maui.MarkdownViewer" Version="x.x.x" />
</ItemGroup>
```

4. **Restart IDE:**
- Close and reopen Visual Studio/VS Code/Rider
- Sometimes IDEs cache intellisense

---

## Content Display Issues

### Content Not Displaying

**Symptom:** MarkdownViewer renders but shows blank/empty

**Possible Causes and Solutions:**

**1. Source Not Set**
```csharp
// ❌ Wrong - Source not set
SfMarkdownViewer viewer = new SfMarkdownViewer();
Content = viewer;

// ✅ Correct - Source is set
SfMarkdownViewer viewer = new SfMarkdownViewer();
viewer.Source = "# Hello World";
Content = viewer;
```

**2. Empty or Null Content**
```csharp
// Check before setting
if (!string.IsNullOrWhiteSpace(markdownContent))
{
    viewer.Source = markdownContent;
}
else
{
    viewer.Source = "# No Content\n\nNo content available.";
}
```

**3. XAML CDATA Issue**
```xml
<!-- ❌ Wrong - Missing CDATA -->
<markdown:SfMarkdownViewer.Source>
    <x:String>
        # Hello
        Content with <special> characters
    </x:String>
</markdown:SfMarkdownViewer.Source>

<!-- ✅ Correct - Using CDATA -->
<markdown:SfMarkdownViewer.Source>
    <x:String>
        <![CDATA[
        # Hello
        Content with <special> characters
        ]]>
    </x:String>
</markdown:SfMarkdownViewer.Source>
```

**4. Async Loading Not Awaited**
```csharp
// ❌ Wrong - Not awaited
public MainPage()
{
    InitializeComponent();
    LoadContentAsync();  // Fire and forget
}

// ✅ Correct - Properly awaited
public MainPage()
{
    InitializeComponent();
    _ = LoadContentAsync();  // Explicit discard
}

private async Task LoadContentAsync()
{
    string content = await File.ReadAllTextAsync("file.md");
    markdownViewer.Source = content;
}
```

---

### Markdown Rendering Incorrectly

**Symptom:** Markdown displays as plain text or renders incorrectly

**Solutions:**

**1. Verify Markdown Syntax**
```markdown
<!-- ❌ Wrong syntax -->
##No space after hash
**Bold without closing asterisks

<!-- ✅ Correct syntax -->
## Space after hash
**Bold with closing asterisks**
```

**2. Check Line Breaks**
```markdown
<!-- Markdown requires blank line between elements -->

# Heading
Content here

## Another Heading
More content
```

**3. Escape Special Characters**
```markdown
Use backslash to escape: \* \_ \# \[ \]
```

---

### Images Not Loading

**Symptom:** Image markdown displays but image doesn't show

**Solutions:**

**1. Embedded Resources**
```csharp
// Place image in Resources/Images/logo.png
// Set Build Action: MauiImage

// Use in Markdown
string markdown = @"
![Logo](logo.png)
";
```

**2. Absolute URLs**
```markdown
![Image](https://example.com/image.png)
```

**3. Base64 Embedded Images**
```markdown
![Image](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAUA...)
```

**4. Local File Paths (Platform-Specific)**
```csharp
#if WINDOWS
string imagePath = @"C:\Images\logo.png";
#elif ANDROID
string imagePath = "/storage/emulated/0/Pictures/logo.png";
#elif IOS
string imagePath = Path.Combine(
    Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments),
    "logo.png"
);
#endif

string markdown = $"![Logo]({imagePath})";
```

---

## Styling Issues

### Styles Not Applying

**Symptom:** MarkdownStyleSettings or CSS not taking effect

**Solutions:**

**1. Verify Settings Are Set**
```csharp
// ❌ Wrong - Settings created but not assigned
var settings = new MarkdownStyleSettings { H1Color = "#FF0000" };

// ✅ Correct - Settings assigned to viewer
var settings = new MarkdownStyleSettings { H1Color = "#FF0000" };
markdownViewer.Settings = settings;
```

**2. Check Font Size Units**
```csharp
// ❌ Wrong - Missing "px" unit
H1FontSize = "32"

// ✅ Correct - Includes "px" unit
H1FontSize = "32px"
```

**3. Verify Color Format**
```csharp
// ✅ All valid formats
H1Color = "#FF5733"
H1Color = "#F57"
H1Color = "Red"
H1Color = "rgb(255, 87, 51)"
H1Color = "rgba(255, 87, 51, 0.8)"

// ❌ Invalid
H1Color = "255, 87, 51"  // Missing rgb()
```

**4. CSS Syntax Errors**
```css
/* ❌ Wrong - Missing semicolon */
body {
    color: #000000
    font-size: 16px
}

/* ✅ Correct */
body {
    color: #000000;
    font-size: 16px;
}
```

---

### CSS Not Overriding Properties

**Symptom:** Property settings override CSS when CSS should win

**Expected Behavior:** CSS should always take precedence

**Solution:**

```csharp
// This is actually correct behavior
var settings = new MarkdownStyleSettings
{
    H1Color = "#000000",  // Base color
    CssStyleRules = @"
        h1 {
            color: #FF0000;  // This should win and display red
        }
    "
};

// If CSS is not winning:
// 1. Check for CSS syntax errors
// 2. Verify selector matches (h1, not H1)
// 3. Check for conflicting !important rules
```

---

### Font Size Issues on Different Platforms

**Symptom:** Font sizes look different on iOS/Android/Windows

**Solution:**

Use consistent "px" units and test on all platforms:

```csharp
// Platform-specific adjustments if needed
#if ANDROID
var baseFontSize = "15px";
#elif IOS
var baseFontSize = "16px";
#elif WINDOWS
var baseFontSize = "14px";
#else
var baseFontSize = "16px";
#endif

markdownViewer.Settings = new MarkdownStyleSettings
{
    BodyFontSize = baseFontSize
};
```

---

## Content Loading Issues

### File Not Found When Loading

**Symptom:** `FileNotFoundException` when loading from file

**Solutions:**

**1. Verify File Path**
```csharp
string filePath = Path.Combine(FileSystem.AppDataDirectory, "document.md");

if (File.Exists(filePath))
{
    string content = await File.ReadAllTextAsync(filePath);
    markdownViewer.Source = content;
}
else
{
    await DisplayAlert("Error", $"File not found: {filePath}", "OK");
}
```

**2. Embedded Resource Path**
```csharp
// For file in Resources/Raw/UserGuide.md
using Stream stream = await FileSystem.OpenAppPackageFileAsync("UserGuide.md");

// For file in Resources/Raw/Docs/UserGuide.md
using Stream stream = await FileSystem.OpenAppPackageFileAsync("Docs/UserGuide.md");
```

**3. Case Sensitivity (Android/iOS)**
```csharp
// ❌ May fail on Android/iOS
"UserGuide.md" vs "userguide.md"

// ✅ Always use exact case
string fileName = "UserGuide.md";  // Match actual file name exactly
```

---

### URL Loading Fails

**Symptom:** Loading Markdown from URL fails or times out

**Solutions:**

**1. Check Network Permissions**

**Android:** Add to `AndroidManifest.xml`:
```xml
<uses-permission android:name="android.permission.INTERNET" />
```

**iOS:** Add to `Info.plist` (if loading non-HTTPS):
```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSAllowsArbitraryLoads</key>
    <true/>
</dict>
```

**2. Handle Timeouts**
```csharp
public async Task LoadFromUrlWithRetryAsync(string url, int maxRetries = 3)
{
    for (int i = 0; i < maxRetries; i++)
    {
        try
        {
            using HttpClient client = new HttpClient();
            client.Timeout = TimeSpan.FromSeconds(30);
            
            string content = await client.GetStringAsync(url);
            markdownViewer.Source = content;
            return;  // Success
        }
        catch (TaskCanceledException)
        {
            if (i == maxRetries - 1)
                markdownViewer.Source = "# Timeout\n\nFailed to load content.";
        }
        catch (HttpRequestException ex)
        {
            if (i == maxRetries - 1)
                markdownViewer.Source = $"# Error\n\n{ex.Message}";
        }
        
        await Task.Delay(1000);  // Wait before retry
    }
}
```

**3. Verify URL Format**
```csharp
// ✅ Correct URL formats for raw content
"https://raw.githubusercontent.com/user/repo/branch/file.md"
"https://gitlab.com/user/repo/-/raw/branch/file.md"

// ❌ Wrong - HTML page, not raw markdown
"https://github.com/user/repo/blob/branch/file.md"
```

---

### Special Characters Display Incorrectly

**Symptom:** Non-ASCII characters show as boxes or garbled text

**Solutions:**

**1. Use UTF-8 Encoding**
```csharp
// For files
string content = await File.ReadAllTextAsync(filePath, Encoding.UTF8);

// For streams
using StreamReader reader = new StreamReader(stream, Encoding.UTF8);
string content = await reader.ReadToEndAsync();
```

**2. Save Files as UTF-8**
- In Visual Studio: Save As → Encoding → UTF-8
- Ensure `.md` files are saved with UTF-8 encoding

**3. HTTP Content Encoding**
```csharp
using HttpClient client = new HttpClient();
client.DefaultRequestHeaders.AcceptCharset.Add(
    new StringWithQualityHeaderValue("utf-8")
);
```

---

## Performance Issues

### Slow Rendering of Large Documents

**Symptom:** App becomes unresponsive with large Markdown files

**Solutions:**

**1. Load Content Asynchronously**
```csharp
private async Task LoadLargeDocumentAsync()
{
    loadingIndicator.IsVisible = true;
    
    string content = await Task.Run(async () => 
        await File.ReadAllTextAsync("large-document.md")
    );
    
    markdownViewer.Source = content;
    loadingIndicator.IsVisible = false;
}
```

**2. Paginate Content**
```csharp
public void LoadSection(int sectionNumber)
{
    // Split large document into sections
    string[] sections = fullContent.Split("---");
    
    if (sectionNumber >= 0 && sectionNumber < sections.Length)
    {
        markdownViewer.Source = sections[sectionNumber];
    }
}
```

**3. Simplify CSS**
```css
/* Avoid expensive CSS operations */
/* ❌ Expensive */
* {
    box-shadow: 0 0 10px rgba(0,0,0,0.5);
}

/* ✅ More efficient */
img {
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}
```

---

## Platform-Specific Issues

### Scrollbar Not Visible on Mobile

**Symptom:** Custom scrollbar CSS doesn't work on Android/iOS

**Explanation:** `::-webkit-scrollbar` CSS primarily works on desktop platforms

**Solution:**

```csharp
// Apply scrollbar styles conditionally
#if WINDOWS || MACCATALYST
string scrollbarCss = @"
    ::-webkit-scrollbar { width: 10px; background: #EEE; }
    ::-webkit-scrollbar-thumb { background: #888; }
";
#else
string scrollbarCss = "";  // Native scrollbar on mobile
#endif

markdownViewer.Settings = new MarkdownStyleSettings
{
    CssStyleRules = scrollbarCss
};
```

---

### Different Rendering Across Platforms

**Symptom:** Markdown looks different on iOS vs Android vs Windows

**Solutions:**

**1. Test on All Platforms**
- Always test on target platforms
- Don't rely solely on emulators

**2. Use Platform-Agnostic CSS**
```css
/* Avoid platform-specific features */
/* Use standard CSS properties */
```

**3. Apply Platform-Specific Overrides**
```csharp
string GetPlatformCss()
{
#if ANDROID
    return "body { padding: 20px; }";
#elif IOS
    return "body { padding: 24px; }";
#elif WINDOWS
    return "body { padding: 16px; }";
#else
    return "body { padding: 20px; }";
#endif
}
```

---

## Build and Deployment Issues

### Build Errors After Package Update

**Symptom:** Project won't build after updating Syncfusion packages

**Solutions:**

**1. Update All Syncfusion Packages**
```bash
# Update all Syncfusion packages to same version
dotnet add package Syncfusion.Maui.Core --version x.x.x
dotnet add package Syncfusion.Maui.MarkdownViewer --version x.x.x
```

**2. Clean and Rebuild**
```bash
dotnet clean
dotnet restore
dotnet build
```

**3. Clear bin/obj Folders**
- Manually delete `bin` and `obj` folders
- Rebuild project

---

## Getting Further Help

If you continue to experience issues:

1. **Check Official Documentation:**
   - [Syncfusion MarkdownViewer Docs](https://help.syncfusion.com/maui/markdownviewer/overview)

2. **GitHub Sample:**
   - [Getting Started Sample](https://github.com/SyncfusionExamples/.NET_MAUI_Markdown_Viewer_GettingStarted)

3. **Community Support:**
   - [Syncfusion Community Forum](https://www.syncfusion.com/forums/maui)
   - Stack Overflow with tag `syncfusion-maui`

4. **Direct Support:**
   - [Syncfusion Support Portal](https://support.syncfusion.com/)
   - Include:
     - Platform (Android/iOS/Windows/macOS)
     - .NET version (e.g., .NET 9.0)
     - Visual Studio version (e.g., VS 2026 v18.0.0)
     - Syncfusion package version
     - Code sample
     - Error messages

---

## Quick Reference: Common Fixes

| Issue | Quick Fix |
|-------|-----------|
| Handler not found | Add `.ConfigureSyncfusionCore()` in `MauiProgram.cs` |
| Content not showing | Verify `Source` property is set |
| Styles not applying | Check font size includes "px" unit |
| File not loading | Verify file path and use `File.Exists()` |
| URL timeout | Increase `HttpClient.Timeout` |
| Special characters broken | Use `Encoding.UTF8` when reading files |
| CSS not working | Check for syntax errors, missing semicolons |
| Build fails | Clean, restore, rebuild |
| Package not found | Clear NuGet cache: `dotnet nuget locals all --clear` |
