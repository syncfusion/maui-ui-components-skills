# Progress and Visual Indicators Migration: Xamarin.Forms to .NET MAUI

Migration guide for progress and visual indicator controls from Xamarin.Forms to .NET MAUI.

## Table of Contents
- [SfLinearProgressBar Migration](#sflinearprogressbar-migration)
- [SfCircularProgressBar Migration](#sfcircularprogressbar-migration)
- [SfStepProgressBar Migration](#sfstepprogressbar-migration)
- [SfShimmer Migration](#sfshimmer-migration)
- [SfBarcodeGenerator Migration](#sfbarcodegenerator-migration)

## SfLinearProgressBar Migration

### Namespace Changes

```csharp
// Xamarin
using Syncfusion.XForms.ProgressBar;

// MAUI
using Syncfusion.Maui.ProgressBar;
```

### Migration Example

**Xamarin:**
```xml
<progressBar:SfLinearProgressBar Progress="75"
                                 AnimationDuration="1000"
                                 TrackColor="LightGray"
                                 ProgressColor="Blue"/>
```

**.NET MAUI:**
```xml
<progressBar:SfLinearProgressBar Progress="75"
                                 AnimationDuration="1000"
                                 TrackFill="LightGray"
                                 ProgressFill="Blue"/>
```

**Property Changes:**

| Xamarin | MAUI |
|---------|------|
| `TrackColor` | `TrackFill` |
| `ProgressColor` | `ProgressFill` |

## SfCircularProgressBar Migration

### Namespace Changes

```csharp
// Same as Linear
using Syncfusion.Maui.ProgressBar;
```

### Migration Example

**Xamarin:**
```xml
<progressBar:SfCircularProgressBar Progress="60"
                                   TrackColor="LightGray"
                                   ProgressColor="Green"/>
```

**.NET MAUI:**
```xml
<progressBar:SfCircularProgressBar Progress="60"
                                   TrackFill="LightGray"
                                   ProgressFill="Green"/>
```

## SfStepProgressBar Migration

### Namespace Changes

```csharp
// Xamarin
using Syncfusion.XForms.ProgressBar;

// MAUI
using Syncfusion.Maui.ProgressBar;
```

### Migration Example

**Xamarin:**
```xml
<progressBar:SfStepProgressBar ItemsSource="{Binding Steps}"
                               ActiveStepIndex="2"/>
```

**.NET MAUI:**
```xml
<progressBar:SfStepProgressBar ItemsSource="{Binding Steps}"
                               ActiveStepIndex="2"/>
```

APIs largely unchanged.

## SfShimmer Migration

### Namespace Changes

```csharp
// Xamarin
using Syncfusion.XForms.Shimmer;

// MAUI
using Syncfusion.Maui.Shimmer;
```

### Migration Example

**Xamarin:**
```xml
<shimmer:SfShimmer IsActive="True"
                   Type="CirclePersona"/>
```

**.NET MAUI:**
```xml
<shimmer:SfShimmer IsActive="True"
                   Type="CirclePersona"/>
```

APIs largely unchanged.

## SfBarcodeGenerator Migration

### Component Rename

| Xamarin | MAUI |
|---------|------|
| SfBarcode | SfBarcodeGenerator |

### Namespace Changes

```csharp
// Xamarin
using Syncfusion.SfBarcode.XForms;

// MAUI
using Syncfusion.Maui.Barcode;
```

### Migration Example

**Xamarin:**
```xml
<barcode:SfBarcode Value="1234567890"
                   BarcodeFormat="Code128"/>
```

**.NET MAUI:**
```xml
<barcode:SfBarcodeGenerator Value="1234567890"
                            Symbology="Code128"/>
```

**Property Changes:**

| Xamarin | MAUI |
|---------|------|
| `BarcodeFormat` | `Symbology` |

## Common Migration Patterns

### Progress Value Updates

**Xamarin:**
```csharp
progressBar.Progress = 50;
progressBar.AnimationDuration = 500;
```

**.NET MAUI:**
```csharp
progressBar.Progress = 50;
progressBar.AnimationDuration = 500;
```

### Color to Brush Migration

**Xamarin:**
```csharp
progressBar.ProgressColor = Color.Blue;
```

**.NET MAUI:**
```csharp
progressBar.ProgressFill = Colors.Blue;
// Or with brush
progressBar.ProgressFill = new SolidColorBrush(Colors.Blue);
```

## Troubleshooting

### Issue: Sf Barcode not found

**Solution:** Renamed to `SfBarcodeGenerator`:
```csharp
// Change
SfBarcode barcode = new SfBarcode();

// To
SfBarcodeGenerator barcode = new SfBarcodeGenerator();
```

### Issue: TrackColor/ProgressColor not found

**Solution:** Use Fill properties:
```csharp
// Change
progressBar.TrackColor = Color.Gray;
progressBar.ProgressColor = Color.Blue;

// To
progressBar.TrackFill = Colors.Gray;
progressBar.ProgressFill = Colors.Blue;
```

## Next Steps

1. Update NuGet packages: `Syncfusion.Maui.ProgressBar`, `Syncfusion.Maui.Barcode`
2. Update namespaces
3. Replace SfBarcode → SfBarcodeGenerator
4. Update Color → Fill properties
5. Test progress animations
