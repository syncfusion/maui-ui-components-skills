# Container Types in .NET MAUI Text Input Layout

## Table of Contents
- [Overview](#overview)
- [Filled Container](#filled-container)
- [Outlined Container](#outlined-container)
- [None Container](#none-container)
- [Container Background](#container-background)
- [Custom Padding](#custom-padding)
- [Choosing a Container Type](#choosing-a-container-type)

## Overview

Container types create visual distinction between input fields and other content. The **ContainerType** property supports three styles:

- **Filled** — Background fill with bottom stroke
- **Outlined** — Border around the entire container
- **None** — Minimal style with bottom line only

Each type serves different design needs and visual hierarchies.

## Filled Container

The Filled container adds a background color to the input area with a stroke at the bottom edge. This creates a subtle, elevated appearance.

### Basic Usage

#### XAML

```xml
<inputLayout:SfTextInputLayout Hint="Name"
                               HelperText="Enter your name"
                               ContainerType="Filled">
    <Entry Text="John" />
</inputLayout:SfTextInputLayout>
```

#### C#

```csharp
var inputLayout = new SfTextInputLayout
{
    Hint = "Name",
    HelperText = "Enter your name",
    ContainerType = ContainerType.Filled,
    Content = new Entry { Text = "John" }
};
```

### Visual Behavior

- **Unfocused:** Background is filled, bottom stroke is subtle
- **Focused:** Background remains filled, bottom stroke becomes prominent
- **Error:** Background remains, bottom stroke turns red

### When to Use

- Dense layouts where inputs need subtle separation
- Material Design 2 style applications
- When you want less visual weight than Outlined
- Forms with many fields in limited space

### Customizing Filled Background

Use **ContainerBackground** to change the fill color:

```xml
<inputLayout:SfTextInputLayout Hint="Name"
                               ContainerType="Filled"
                               ContainerBackground="#FFF2F2"
                               HelperText="Enter your name">
    <Entry />
</inputLayout:SfTextInputLayout>
```

```csharp
var inputLayout = new SfTextInputLayout
{
    Hint = "Name",
    ContainerType = ContainerType.Filled,
    ContainerBackground = Color.FromArgb("#FFF2F2"),
    HelperText = "Enter your name",
    Content = new Entry()
};
```

## Outlined Container

The Outlined container draws a rounded border around the entire input field, creating clear boundaries.

### Basic Usage

#### XAML

```xml
<inputLayout:SfTextInputLayout Hint="Name"
                               HelperText="Enter your name"
                               ContainerType="Outlined">
    <Entry Text="John" />
</inputLayout:SfTextInputLayout>
```

#### C#

```csharp
var inputLayout = new SfTextInputLayout
{
    Hint = "Name",
    HelperText = "Enter your name",
    ContainerType = ContainerType.Outlined,
    Content = new Entry { Text = "John" }
};
```

### Visual Behavior

- **Unfocused:** Border is visible with default color
- **Focused:** Border becomes thicker and more prominent
- **Error:** Border turns red
- **Hint Label:** Floats over the top border, creating a gap

### Customizing Corner Radius

Use **OutlineCornerRadius** to adjust the roundness:

```xml
<inputLayout:SfTextInputLayout Hint="Name"
                               ContainerType="Outlined"
                               OutlineCornerRadius="8">
    <Entry />
</inputLayout:SfTextInputLayout>
```

```csharp
var inputLayout = new SfTextInputLayout
{
    Hint = "Name",
    ContainerType = ContainerType.Outlined,
    OutlineCornerRadius = 8,
    Content = new Entry()
};
```

**Common Corner Radius Values:**
- `4` — Subtle rounding (default)
- `8` — Medium rounding (recommended)
- `12` — Pronounced rounding
- `16` — Highly rounded
- `0` — Sharp corners (no rounding)

### When to Use

- Material Design 3 style applications
- Forms that need clear field separation
- When background contrast is important
- Modern, clean UI designs
- Accessibility-focused layouts (clear boundaries)

### Outlined with Background Color

You can combine Outlined with a background color:

```xml
<inputLayout:SfTextInputLayout Hint="Username"
                               ContainerType="Outlined"
                               ContainerBackground="#F5F5F5"
                               OutlineCornerRadius="12"
                               Stroke="#6750A4">
    <Entry />
</inputLayout:SfTextInputLayout>
```

## None Container

The None container provides a minimal appearance with only a bottom line.

### Basic Usage

#### XAML

```xml
<inputLayout:SfTextInputLayout Hint="Name"
                               HelperText="Enter your name"
                               ContainerType="None">
    <Entry Text="John" />
</inputLayout:SfTextInputLayout>
```

#### C#

```csharp
var inputLayout = new SfTextInputLayout
{
    Hint = "Name",
    HelperText = "Enter your name",
    ContainerType = ContainerType.None,
    Content = new Entry { Text = "John" }
};
```

### Visual Behavior

- **Unfocused:** Only bottom line is visible
- **Focused:** Bottom line becomes thicker
- **Error:** Bottom line turns red
- **No Background:** Transparent background

### When to Use

- Minimalist designs
- When you want maximum content visibility
- Legacy or simple form layouts
- When background space is limited
- Mobile-first designs with tight spacing

## Container Background

The **ContainerBackground** property sets the fill color for Filled and Outlined containers.

### Filled Container Background

```xml
<inputLayout:SfTextInputLayout Hint="Email"
                               ContainerType="Filled"
                               ContainerBackground="#E6EEF9"
                               Stroke="#0450C2">
    <Entry />
</inputLayout:SfTextInputLayout>
```

```csharp
var inputLayout = new SfTextInputLayout
{
    Hint = "Email",
    ContainerType = ContainerType.Filled,
    ContainerBackground = Color.FromArgb("#E6EEF9"),
    Stroke = Color.FromArgb("#0450C2"),
    Content = new Entry()
};
```

### Outlined Container Background

```xml
<inputLayout:SfTextInputLayout Hint="Phone"
                               ContainerType="Outlined"
                               ContainerBackground="#E6EEF9"
                               Stroke="#0450C2"
                               OutlineCornerRadius="10">
    <Entry Keyboard="Telephone" />
</inputLayout:SfTextInputLayout>
```

```csharp
var inputLayout = new SfTextInputLayout
{
    Hint = "Phone",
    ContainerType = ContainerType.Outlined,
    ContainerBackground = Color.FromArgb("#E6EEF9"),
    Stroke = Color.FromArgb("#0450C2"),
    OutlineCornerRadius = 10,
    Content = new Entry { Keyboard = Keyboard.Telephone }
};
```

**Note:** ContainerBackground has no effect on **None** container type.

## Custom Padding

Use **InputViewPadding** to add space around the input view inside the container.

### Basic Padding

```xml
<inputLayout:SfTextInputLayout Hint="Name"
                               InputViewPadding="10"
                               ContainerType="Outlined">
    <Entry />
</inputLayout:SfTextInputLayout>
```

### Asymmetric Padding

```xml
<inputLayout:SfTextInputLayout Hint="Message"
                               InputViewPadding="0,5,0,5"
                               ContainerType="Outlined"
                               HelperText="Top and bottom padding is 5">
    <Entry />
</inputLayout:SfTextInputLayout>
```

```csharp
var inputLayout = new SfTextInputLayout
{
    Hint = "Message",
    InputViewPadding = new Thickness(0, 5, 0, 5), // Left, Top, Right, Bottom
    ContainerType = ContainerType.Outlined,
    HelperText = "Top and bottom padding is 5",
    Content = new Entry()
};
```

### Use Cases for Padding

- **Compact layouts:** Reduce padding to `5` or less
- **Spacious forms:** Increase padding to `15-20`
- **Vertical spacing:** Use `InputViewPadding="0,10,0,10"` for height
- **Icon compensation:** Adjust padding when using LeadingView/TrailingView

## Choosing a Container Type

### Decision Guide

**Choose Filled when:**
- You need a subtle, less prominent input style
- Working with dense layouts (many fields)
- Following Material Design 2 guidelines
- Background contrast is already strong
- You want a modern, elevated appearance

**Choose Outlined when:**
- You need clear field boundaries
- Following Material Design 3 guidelines
- Accessibility is a priority (clear visual separation)
- Forms have varied background colors
- You want maximum clarity and readability

**Choose None when:**
- You want a minimalist appearance
- Working with limited screen space
- Creating simple, single-field forms
- Following a custom design system
- Background provides sufficient context

### Comparison Example

```xml
<VerticalStackLayout Spacing="25" Padding="20">
    
    <!-- Filled -->
    <inputLayout:SfTextInputLayout Hint="Filled Style"
                                   ContainerType="Filled"
                                   HelperText="Background with bottom line">
        <Entry Text="Example text" />
    </inputLayout:SfTextInputLayout>
    
    <!-- Outlined -->
    <inputLayout:SfTextInputLayout Hint="Outlined Style"
                                   ContainerType="Outlined"
                                   OutlineCornerRadius="8"
                                   HelperText="Border around container">
        <Entry Text="Example text" />
    </inputLayout:SfTextInputLayout>
    
    <!-- None -->
    <inputLayout:SfTextInputLayout Hint="None Style"
                                   ContainerType="None"
                                   HelperText="Only bottom line">
        <Entry Text="Example text" />
    </inputLayout:SfTextInputLayout>
    
</VerticalStackLayout>
```

## Best Practices

1. **Consistency:** Use the same container type throughout your app/form
2. **Hierarchy:** Use different types to indicate importance if needed
3. **Backgrounds:** Ensure sufficient contrast between container and page background
4. **Spacing:** Increase vertical spacing between Outlined containers
5. **Corner Radius:** Keep corner radius consistent with other UI elements
6. **Padding:** Adjust padding based on form density and device size
7. **Mobile:** Outlined works best on mobile for touch targets
8. **Desktop:** Filled or None can work well on larger screens

## Platform Considerations

All container types are supported on:
- Windows
- iOS
- Android
- macOS

Rendering may have subtle platform-specific differences, but functionality is consistent across all platforms.
