# Interactive and Drawing Controls Migration: Xamarin.Forms to .NET MAUI

Migration guide for interactive and drawing controls from Xamarin.Forms to .NET MAUI.

## Table of Contents
- [SfSignaturePad Migration](#sfsignaturepad-migration)
- [SfChat Migration](#sfchat-migration)
- [SfSlider Migration](#sfslider-migration)
- [SfRangeSlider Migration](#sfrangeslider-migration)
- [SfRadialMenu Migration](#sfradialmenu-migration)
- [Pull-to-Refresh Migration](#pull-to-refresh-migration)

## SfSignaturePad Migration

### Namespace Changes

```csharp
// Xamarin
using Syncfusion.SfSignaturePad.XForms;

// MAUI
using Syncfusion.Maui.SignaturePad;
```

### Migration Example

**Xamarin:**
```xaml
<signaturePad:SfSignaturePad MinimumStrokeThickness="1"
                             MaximumStrokeThickness="4"
                             StrokeColor="Blue"/>
```

**.NET MAUI:**
```xaml
<signaturePad:SfSignaturePad MinimumStrokeThickness="1"
                             MaximumStrokeThickness="4"
                             StrokeColor="Blue"/>
```

APIs largely unchanged.

## SfChat Migration

### Namespace Changes

```csharp
// Xamarin
using Syncfusion.XForms.Chat;

// MAUI
using Syncfusion.Maui.Chat;
```

### Migration Example

**Xamarin:**
```xaml
<chat:SfChat Messages="{Binding Messages}"
             CurrentUser="{Binding CurrentUser}"/>
```

**.NET MAUI:**
```xaml
<chat:SfChat Messages="{Binding Messages}"
             CurrentUser="{Binding CurrentUser}"/>
```

APIs largely unchanged.

## SfSlider Migration

### Important Note

In Xamarin, `SfRangeSlider` handled both single-value and range selection. In MAUI, this is split:
- **SfSlider** - Single value selection
- **SfRangeSlider** - Range selection (two thumbs)

### Namespace Changes

```csharp
// Xamarin
using Syncfusion.SfSlider.XForms;

// MAUI
using Syncfusion.Maui.Sliders;
```

### Migration Example

**Xamarin (Single Value):**
```xaml
<slider:SfRangeSlider Minimum="0"
                      Maximum="100"
                      Value="50"/>
```

**.NET MAUI SfSlider:**
```xaml
<sliders:SfSlider Minimum="0"
                  Maximum="100"
                  Value="50"/>
```

## SfRangeSlider Migration

### Migration Example

**Xamarin:**
```xaml
<slider:SfRangeSlider Minimum="0"
                      Maximum="100"
                      RangeStart="25"
                      RangeEnd="75"/>
```

**.NET MAUI:**
```xaml
<sliders:SfRangeSlider Minimum="0"
                       Maximum="100"
                       RangeStart="25"
                       RangeEnd="75"/>
```

## SfRadialMenu Migration

### Namespace Changes

```csharp
// Xamarin
using Syncfusion.SfRadialMenu.XForms;

// MAUI
using Syncfusion.Maui.RadialMenu;
```

### Migration Example

**Xamarin:**
```xaml
<radialMenu:SfRadialMenu CenterButtonText="Menu"
                         RimRadius="150">
    <radialMenu:SfRadialMenuItem Text="Item 1"/>
    <radialMenu:SfRadialMenuItem Text="Item 2"/>
</radialMenu:SfRadialMenu>
```

**.NET MAUI:**
```xaml
<radialMenu:SfRadialMenu CenterButtonText="Menu"
                         RimRadius="150">
    <radialMenu:SfRadialMenuItem Text="Item 1"/>
    <radialMenu:SfRadialMenuItem Text="Item 2"/>
</radialMenu:SfRadialMenu>
```

APIs largely unchanged.

## Pull-to-Refresh Migration

### Namespace Changes

```csharp
// Xamarin
using Syncfusion.SfPullToRefresh.XForms;

// MAUI
using Syncfusion.Maui.PullToRefresh;
```

### Migration Example

**Xamarin:**
```xaml
<pullToRefresh:SfPullToRefresh IsRefreshing="{Binding IsRefreshing}"
                               RefreshCommand="{Binding RefreshCommand}">
    <pullToRefresh:SfPullToRefresh.PullableContent>
        <ListView ItemsSource="{Binding Items}"/>
    </pullToRefresh:SfPullToRefresh.PullableContent>
</pullToRefresh:SfPullToRefresh>
```

**.NET MAUI:**
```xaml
<pullToRefresh:SfPullToRefresh IsRefreshing="{Binding IsRefreshing}"
                               RefreshCommand="{Binding RefreshCommand}">
    <pullToRefresh:SfPullToRefresh.PullableContent>
        <ListView ItemsSource="{Binding Items}"/>
    </pullToRefresh:SfPullToRefresh.PullableContent>
</pullToRefresh:SfPullToRefresh>
```

APIs largely unchanged.

## Common Migration Patterns

### Signature Capture

**Both Xamarin and MAUI:**
```csharp
var imageStream = await signaturePad.ToImageStreamAsync();
// Save or display image
```

### Chat Message Handling

**Both Xamarin and MAUI:**
```csharp
chat.Messages.Add(new TextMessage
{
    Author = currentUser,
    Text = "Hello!",
    DateTime = DateTime.Now
});
```

### Slider Value Changed

**Xamarin:**
```csharp
slider.ValueChanged += (s, e) =>
{
    var value = e.Value;
};
```

**.NET MAUI:**
```csharp
slider.ValueChanged += (s, e) =>
{
    var value = e.NewValue;
};
```

## Troubleshooting

### Issue: Single value SfRangeSlider migration

**Solution:** Use `SfSlider` for single values:
```xaml
<!-- Xamarin (single value) -->
<slider:SfRangeSlider Value="50"/>

<!-- MAUI -->
<sliders:SfSlider Value="50"/>
```

### Issue: Range selection migration

**Solution:** Continue using `SfRangeSlider`:
```xaml
<!-- Both Xamarin and MAUI -->
<sliders:SfRangeSlider RangeStart="25" RangeEnd="75"/>
```

## Next Steps

1. Update NuGet packages: `Syncfusion.Maui.Sliders`, `Syncfusion.Maui.SignaturePad`, etc.
2. Update namespaces
3. Split single-value sliders to `SfSlider`
4. Keep range sliders as `SfRangeSlider`
5. Test touch interactions and gestures
