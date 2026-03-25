# Custom Views for .NET MAUI Shimmer

## Table of Contents
- [Overview](#overview)
- [CustomView with BoxView](#customview-with-boxview)
- [ShimmerView and ShapeType](#shimmerview-and-shapetype)
- [Full Custom Layout Example](#full-custom-layout-example)
- [Tips and Gotchas](#tips-and-gotchas)

---

## Overview

When none of the 7 built-in types match your UI, use `SfShimmer.CustomView` to define your own shimmer layout. Any view placed in `CustomView` will be animated with the shimmer wave effect.

Two approaches:
1. **`BoxView` approach** — Use standard MAUI `BoxView` elements with a gray background. Simple but less semantic.
2. **`ShimmerView` approach** — Use the dedicated `ShimmerView` control, which supports `ShapeType` for circle, rectangle, and rounded rectangle shapes. Recommended for most cases.

---

## CustomView with BoxView

Place standard MAUI layout elements inside `SfShimmer.CustomView`. The shimmer wave automatically animates over them.

### XAML

```xml
<shimmer:SfShimmer>
    <shimmer:SfShimmer.CustomView>
        <Grid Padding="10" ColumnSpacing="15" RowSpacing="10">
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto" />
                <RowDefinition Height="Auto" />
                <RowDefinition Height="Auto" />
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="Auto" />
                <ColumnDefinition Width="Auto" />
            </Grid.ColumnDefinitions>

            <!-- Title line -->
            <BoxView BackgroundColor="Gray"
                     Grid.Row="0" Grid.ColumnSpan="2"
                     HeightRequest="10" WidthRequest="280"
                     HorizontalOptions="Start" VerticalOptions="Start" />

            <!-- Subtitle line -->
            <BoxView BackgroundColor="Gray"
                     Grid.Row="1" Grid.ColumnSpan="2"
                     HeightRequest="10" WidthRequest="240"
                     HorizontalOptions="Start" VerticalOptions="Start" />

            <!-- Thumbnail block -->
            <BoxView BackgroundColor="Gray"
                     Grid.Row="2" Grid.RowSpan="4" />

            <!-- Body lines -->
            <BoxView BackgroundColor="Gray"
                     Grid.Row="2" Grid.Column="1"
                     HeightRequest="10" WidthRequest="190"
                     HorizontalOptions="Start" VerticalOptions="Start" />
        </Grid>
    </shimmer:SfShimmer.CustomView>
</shimmer:SfShimmer>
```

---

## ShimmerView and ShapeType

`ShimmerView` is a dedicated control for custom shimmer layouts. It participates natively in the wave animation and supports three shapes via `ShapeType`:

| `ShimmerShapeType` | Renders As |
|---|---|
| `Rectangle` | Sharp-cornered rectangle (default) |
| `RoundedRectangle` | Rectangle with rounded corners |
| `Circle` | Circular placeholder |

### XAML

```xml
<shimmer:SfShimmer>
    <shimmer:SfShimmer.CustomView>
        <Grid Padding="10" ColumnSpacing="15" RowSpacing="10">
            <Grid.RowDefinitions>
                <RowDefinition Height="10" />
                <RowDefinition Height="10" />
                <RowDefinition Height="Auto" />
                <RowDefinition Height="Auto" />
                <RowDefinition Height="Auto" />
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="Auto" />
                <ColumnDefinition Width="Auto" />
            </Grid.ColumnDefinitions>

            <!-- Wide title line -->
            <shimmer:ShimmerView Grid.Row="0" Grid.ColumnSpan="2"
                                 HorizontalOptions="Start" WidthRequest="300" />

            <!-- Narrower subtitle line -->
            <shimmer:ShimmerView Grid.Row="1" Grid.ColumnSpan="2"
                                 HorizontalOptions="Start" WidthRequest="250" />

            <!-- Circle avatar spanning multiple rows -->
            <shimmer:ShimmerView Grid.Row="2" Grid.RowSpan="3"
                                 ShapeType="Circle" />

            <!-- Body text lines next to the avatar -->
            <shimmer:ShimmerView Grid.Row="2" Grid.Column="1"
                                 ShapeType="Rectangle"
                                 HeightRequest="10" WidthRequest="180"
                                 HorizontalOptions="Start" VerticalOptions="Start" />

            <shimmer:ShimmerView Grid.Row="3" Grid.Column="1"
                                 ShapeType="Rectangle"
                                 HeightRequest="10" WidthRequest="160"
                                 HorizontalOptions="Start" VerticalOptions="Start" />

            <!-- Rounded tag/badge line -->
            <shimmer:ShimmerView Grid.Row="4" Grid.Column="1"
                                 ShapeType="RoundedRectangle"
                                 HeightRequest="10" WidthRequest="140"
                                 HorizontalOptions="Start" VerticalOptions="Start" />
        </Grid>
    </shimmer:SfShimmer.CustomView>
</shimmer:SfShimmer>
```

### C# (equivalent)

```csharp
SfShimmer shimmer = new SfShimmer();

var grid = new Grid
{
    Padding = 10,
    ColumnSpacing = 15,
    RowSpacing = 10,
    VerticalOptions = LayoutOptions.Fill,
    RowDefinitions = new RowDefinitionCollection
    {
        new RowDefinition { Height = 10 },
        new RowDefinition { Height = 10 },
        new RowDefinition { Height = GridLength.Auto },
        new RowDefinition { Height = GridLength.Auto },
        new RowDefinition { Height = GridLength.Auto },
    },
    ColumnDefinitions = new ColumnDefinitionCollection
    {
        new ColumnDefinition { Width = GridLength.Auto },
        new ColumnDefinition { Width = GridLength.Auto },
    }
};

var titleLine = new ShimmerView { HorizontalOptions = LayoutOptions.Start, WidthRequest = 300 };
var subtitleLine = new ShimmerView { HorizontalOptions = LayoutOptions.Start, WidthRequest = 250 };
var avatar = new ShimmerView { ShapeType = ShimmerShapeType.Circle };
var bodyLine1 = new ShimmerView
{
    ShapeType = ShimmerShapeType.Rectangle,
    HeightRequest = 10, WidthRequest = 180,
    HorizontalOptions = LayoutOptions.Start, VerticalOptions = LayoutOptions.Start
};
var tag = new ShimmerView
{
    ShapeType = ShimmerShapeType.RoundedRectangle,
    HeightRequest = 10, WidthRequest = 140,
    HorizontalOptions = LayoutOptions.Start, VerticalOptions = LayoutOptions.Start
};

grid.Add(titleLine, 0, 0); Grid.SetColumnSpan(titleLine, 2);
grid.Add(subtitleLine, 0, 1); Grid.SetColumnSpan(subtitleLine, 2);
grid.Add(avatar, 0, 2); Grid.SetRowSpan(avatar, 3);
grid.Add(bodyLine1, 1, 2);
grid.Add(tag, 1, 4);

shimmer.CustomView = grid;
this.Content = shimmer;
```

---

## Full Custom Layout Example

A card-style shimmer mimicking a user profile with header text, avatar, and detail lines:

```xml
<shimmer:SfShimmer VerticalOptions="Fill">
    <shimmer:SfShimmer.CustomView>
        <Grid Padding="16" RowSpacing="12" ColumnSpacing="12">
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto" />
                <RowDefinition Height="Auto" />
                <RowDefinition Height="80" />
                <RowDefinition Height="Auto" />
                <RowDefinition Height="Auto" />
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="80" />
                <ColumnDefinition Width="*" />
            </Grid.ColumnDefinitions>

            <!-- Header lines -->
            <shimmer:ShimmerView Grid.Row="0" Grid.ColumnSpan="2"
                                 ShapeType="Rectangle" HeightRequest="14" WidthRequest="260"
                                 HorizontalOptions="Start" />
            <shimmer:ShimmerView Grid.Row="1" Grid.ColumnSpan="2"
                                 ShapeType="Rectangle" HeightRequest="10" WidthRequest="200"
                                 HorizontalOptions="Start" />

            <!-- Circle avatar -->
            <shimmer:ShimmerView Grid.Row="2" Grid.RowSpan="2"
                                 ShapeType="Circle" HeightRequest="72" WidthRequest="72" />

            <!-- Name and subtitle -->
            <shimmer:ShimmerView Grid.Row="2" Grid.Column="1"
                                 ShapeType="Rectangle" HeightRequest="12" WidthRequest="160"
                                 HorizontalOptions="Start" VerticalOptions="Start" />
            <shimmer:ShimmerView Grid.Row="3" Grid.Column="1"
                                 ShapeType="RoundedRectangle" HeightRequest="10" WidthRequest="120"
                                 HorizontalOptions="Start" VerticalOptions="Start" />

            <!-- Footer tag -->
            <shimmer:ShimmerView Grid.Row="4" Grid.ColumnSpan="2"
                                 ShapeType="RoundedRectangle" HeightRequest="10" WidthRequest="180"
                                 HorizontalOptions="Start" />
        </Grid>
    </shimmer:SfShimmer.CustomView>

    <!-- Actual content shown when IsActive = false -->
    <shimmer:SfShimmer.Content>
        <StackLayout Padding="16">
            <Label Text="John Doe" FontSize="18" />
            <Label Text="Senior Developer" />
        </StackLayout>
    </shimmer:SfShimmer.Content>
</shimmer:SfShimmer>
```

---

## Tips and Gotchas

- **`ShimmerView` vs `BoxView`:** Prefer `ShimmerView` — it integrates with the shimmer animation system more directly. `BoxView` works but is less semantic.
- **Size your `ShimmerView` explicitly:** Without `HeightRequest`/`WidthRequest`, shapes may collapse to zero size inside a `Grid.Auto` row/column.
- **Circle shape:** The `Circle` shape type ignores `HeightRequest`/`WidthRequest` if not set — always provide both for predictable sizing.
- **`CustomView` and `Type` are mutually exclusive:** Setting `CustomView` overrides `Type`. Use one or the other.
- **`RepeatCount` works with `CustomView`:** The entire custom view will repeat `RepeatCount` times vertically.
