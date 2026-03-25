# Layout and Container Controls Migration: Xamarin.Forms to .NET MAUI

Migration guide for layout and container controls from Xamarin.Forms to .NET MAUI.

## Table of Contents
- [SfBackdropPage Migration](#sfbackdroppage-migration)
- [SfCardView Migration](#sfcardview-migration)
- [SfParallaxView Migration](#sfparallaxview-migration)
- [SfPopup Migration](#sfpopup-migration)
- [Obsolete Controls](#obsolete-controls)

## SfBackdropPage Migration

### Namespace Changes

```csharp
// Xamarin
using Syncfusion.XForms.Backdrop;

// MAUI
using Syncfusion.Maui.Backdrop;
```

### Migration Example

**Xamarin:**
```xml
<backdrop:SfBackdropPage BackLayerRevealOption="Auto">
    <backdrop:SfBackdropPage.BackLayer>
        <Grid><!-- Navigation content --></Grid>
    </backdrop:SfBackdropPage.BackLayer>
    <backdrop:SfBackdropPage.FrontLayer>
        <Grid><!-- Main content --></Grid>
    </backdrop:SfBackdropPage.FrontLayer>
</backdrop:SfBackdropPage>
```

**.NET MAUI:**
```xml
<backdrop:SfBackdropPage BackLayerRevealOption="Auto">
    <backdrop:SfBackdropPage.BackLayer>
        <Grid><!-- Navigation content --></Grid>
    </backdrop:SfBackdropPage.BackLayer>
    <backdrop:SfBackdropPage.FrontLayer>
        <Grid><!-- Main content --></Grid>
    </backdrop:SfBackdropPage.FrontLayer>
</backdrop:SfBackdropPage>
```

APIs largely unchanged.

## SfCardView Migration

### Namespace Changes

```csharp
// Xamarin
using Syncfusion.XForms.Cards;

// MAUI
using Syncfusion.Maui.Cards;
```

### Migration Example

**Xamarin:**
```xml
<cards:SfCardView CornerRadius="10"
                  HasShadow="True">
    <Grid>
        <!-- Card content -->
    </Grid>
</cards:SfCardView>
```

**.NET MAUI:**
```xml
<cards:SfCardView CornerRadius="10">
    <Grid>
        <!-- Card content -->
    </Grid>
</cards:SfCardView>
```

APIs largely unchanged.

## SfParallaxView Migration

### Namespace Changes

```csharp
// Xamarin
using Syncfusion.XForms.ParallaxView;

// MAUI
using Syncfusion.Maui.ParallaxView;
```

### Migration Example

**Xamarin:**
```xml
<parallax:SfParallaxView>
    <parallax:SfParallaxView.Content>
        <Image Source="background.jpg"/>
    </parallax:SfParallaxView.Content>
    <parallax:SfParallaxView.Source>
        <ScrollView>
            <!-- Scrollable content -->
        </ScrollView>
    </parallax:SfParallaxView.Source>
</parallax:SfParallaxView>
```

**.NET MAUI:**
```xml
<parallax:SfParallaxView>
    <parallax:SfParallaxView.Content>
        <Image Source="background.jpg"/>
    </parallax:SfParallaxView.Content>
    <parallax:SfParallaxView.Source>
        <ScrollView>
            <!-- Scrollable content -->
        </ScrollView>
    </parallax:SfParallaxView.Source>
</parallax:SfParallaxView>
```

APIs largely unchanged.

## SfPopup Migration

### Component Rename

| Xamarin | MAUI |
|---------|------|
| SfPopUpLayout | SfPopup |

### Namespace Changes

```csharp
// Xamarin
using Syncfusion.XForms.PopupLayout;

// MAUI
using Syncfusion.Maui.Popup;
```

### Migration Example

**Xamarin:**
```xml
<popupLayout:SfPopupLayout IsOpen="{Binding ShowPopup}">
    <popupLayout:SfPopupLayout.PopupView>
        <popupLayout:PopupView>
            <popupLayout:PopupView.ContentTemplate>
                <DataTemplate>
                    <!-- Popup content -->
                </DataTemplate>
            </popupLayout:PopupView.ContentTemplate>
        </popupLayout:PopupView>
    </popupLayout:SfPopupLayout.PopupView>
</popupLayout:SfPopupLayout>
```

**.NET MAUI:**
```xml
<popup:SfPopup IsOpen="{Binding ShowPopup}">
    <popup:SfPopup.ContentTemplate>
        <DataTemplate>
            <!-- Popup content -->
        </DataTemplate>
    </popup:SfPopup.ContentTemplate>
</popup:SfPopup>
```

Structure simplified in MAUI.

## Obsolete Controls

### SfBorder - Use Native MAUI Border

**Xamarin:**
```xml
<border:SfBorder CornerRadius="10"
                 BorderColor="Blue"
                 BorderWidth="2">
    <Label Text="Content"/>
</border:SfBorder>
```

**.NET MAUI (Native Control):**
```xml
<Border Stroke="Blue"
        StrokeThickness="2"
        StrokeShape="RoundRectangle 10">
    <Label Text="Content"/>
</Border>
```

**Why Obsolete:** .NET MAUI includes a native Border control with equivalent functionality.

### SfGradientView - Use Native MAUI Gradients

**Xamarin:**
```xml
<gradient:SfGradientView>
    <gradient:SfGradientView.BackgroundBrush>
        <gradient:SfLinearGradientBrush>
            <gradient:SfGradientStop Color="Red" Offset="0"/>
            <gradient:SfGradientStop Color="Blue" Offset="1"/>
        </gradient:SfLinearGradientBrush>
    </gradient:SfGradientView.BackgroundBrush>
</gradient:SfGradientView>
```

**.NET MAUI (Native Gradients):**
```xml
<Grid>
    <Grid.Background>
        <LinearGradientBrush StartPoint="0,0" EndPoint="1,1">
            <GradientStop Color="Red" Offset="0.0"/>
            <GradientStop Color="Blue" Offset="1.0"/>
        </LinearGradientBrush>
    </Grid.Background>
</Grid>
```

**Why Obsolete:** .NET MAUI has built-in gradient support.

## Common Migration Patterns

### Popup State Management

**Xamarin:**
```csharp
popupLayout.IsOpen = true;
popupLayout.Show();
```

**.NET MAUI:**
```csharp
popup.IsOpen = true;
popup.Show();
```

## Troubleshooting

### Issue: SfPopUpLayout not found

**Solution:** Renamed to `SfPopup`:
```csharp
// Change
using Syncfusion.XForms.PopupLayout;
SfPopupLayout popup = new SfPopupLayout();

// To
using Syncfusion.Maui.Popup;
SfPopup popup = new SfPopup();
```

### Issue: SfBorder not available

**Solution:** Use native MAUI `Border`:
```xml
<!-- Change -->
<syncfusion:SfBorder ...>

<!-- To -->
<Border ...>
```

### Issue: SfGradientView not available

**Solution:** Use native MAUI gradients:
```xml
<Grid>
    <Grid.Background>
        <LinearGradientBrush>...</LinearGradientBrush>
    </Grid.Background>
</Grid>
```

## Next Steps

1. Update NuGet packages for layout controls
2. Update namespaces
3. Replace SfPopUpLayout → SfPopup
4. Replace SfBorder with native Border
5. Replace SfGradientView with native gradients
6. Test layout rendering and interactions
