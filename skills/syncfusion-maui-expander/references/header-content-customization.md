# Header and Content Customization

## Table of Contents
- [Overview](#overview)
- [Header Property](#header-property)
- [Content Property](#content-property)
- [Grid Layouts with Icons](#grid-layouts-with-icons)
- [Multi-Expander Layouts](#multi-expander-layouts)
- [Best Practices](#best-practices)
- [Common Pitfalls](#common-pitfalls)
- [Advanced Customization](#advanced-customization)

---

## Overview

The Syncfusion .NET MAUI Expander (SfExpander) is a layout control with two main sections:
- **Header:** Always visible section that users tap to expand/collapse
- **Content:** Collapsible section that appears/disappears

Both sections accept **any View** - you can load grids, stacks, images, icons, labels, or even complex custom views.

---

## Header Property

The `Header` property defines what appears in the always-visible header section.

### Basic Header

```xml
<syncfusion:SfExpander>
    <syncfusion:SfExpander.Header>
        <Grid>
            <Label Text="Click to expand" 
                   FontSize="16" 
                   VerticalOptions="Center"/>
        </Grid>
    </syncfusion:SfExpander.Header>
</syncfusion:SfExpander>
```

```csharp
var expander = new SfExpander();

var headerGrid = new Grid();
headerGrid.Children.Add(new Label 
{ 
    Text = "Click to expand", 
    FontSize = 16, 
    VerticalOptions = LayoutOptions.Center 
});

expander.Header = headerGrid;
```

### Header with Icon and Text

```xml
<syncfusion:SfExpander.Header>
    <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="40"/>
            <ColumnDefinition Width="*"/>
        </Grid.ColumnDefinitions>
        
        <!-- Icon -->
        <Label Text="&#xe701;" 
               FontSize="20" 
               FontFamily="MaterialIcons"
               VerticalOptions="Center"
               HorizontalOptions="Center"/>
        
        <!-- Text -->
        <Label Text="Invoice Details" 
               FontSize="16" 
               Grid.Column="1"
               VerticalOptions="Center"/>
    </Grid>
</syncfusion:SfExpander.Header>
```

### Header with Image

```xml
<syncfusion:SfExpander.Header>
    <Grid Padding="10">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="50"/>
            <ColumnDefinition Width="*"/>
        </Grid.ColumnDefinitions>
        
        <Image Source="user_avatar.png" 
               WidthRequest="40" 
               HeightRequest="40"
               Aspect="AspectFit"/>
        
        <StackLayout Grid.Column="1" VerticalOptions="Center">
            <Label Text="John Doe" FontSize="16" FontAttributes="Bold"/>
            <Label Text="john.doe@email.com" FontSize="12" TextColor="Gray"/>
        </StackLayout>
    </Grid>
</syncfusion:SfExpander.Header>
```

---

## Content Property

The `Content` property defines what appears in the expandable/collapsible section.

### Basic Content

```xml
<syncfusion:SfExpander>
    <syncfusion:SfExpander.Content>
        <Grid Padding="20">
            <Label Text="This is the expandable content" 
                   FontSize="14"/>
        </Grid>
    </syncfusion:SfExpander.Content>
</syncfusion:SfExpander>
```

### Content with Multiple Elements

```xml
<syncfusion:SfExpander.Content>
    <StackLayout Padding="15" Spacing="10">
        <Label Text="Name: John Doe" FontSize="14"/>
        <Label Text="Email: john@example.com" FontSize="14"/>
        <Label Text="Phone: (555) 123-4567" FontSize="14"/>
        <Label Text="Address: 123 Main St" FontSize="14"/>
    </StackLayout>
</syncfusion:SfExpander.Content>
```

### Content with Grid Layout

```xml
<syncfusion:SfExpander.Content>
    <Grid Padding="15" RowSpacing="8">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*"/>
            <ColumnDefinition Width="*"/>
        </Grid.ColumnDefinitions>
        
        <Label Text="Item" FontSize="14" FontAttributes="Bold"/>
        <Label Text="Price" FontSize="14" FontAttributes="Bold" Grid.Column="1" HorizontalOptions="End"/>
        
        <Label Text="Product A" FontSize="14" Grid.Row="1"/>
        <Label Text="$99.00" FontSize="14" Grid.Row="1" Grid.Column="1" HorizontalOptions="End"/>
        
        <Label Text="Product B" FontSize="14" Grid.Row="2"/>
        <Label Text="$149.00" FontSize="14" Grid.Row="2" Grid.Column="1" HorizontalOptions="End"/>
    </Grid>
</syncfusion:SfExpander.Content>
```

---

## Grid Layouts with Icons

Common pattern for professional-looking expanders with icons:

```xml
<syncfusion:SfExpander AnimationDuration="200" IsExpanded="True">
    <syncfusion:SfExpander.Header>
        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition Height="48"/>
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="35"/>
                <ColumnDefinition Width="*"/>
            </Grid.ColumnDefinitions>
            
            <!-- Icon using Unicode character -->
            <Label Text="&#xe703;" 
                   FontSize="16" 
                   Margin="14,2,2,2"
                   FontFamily='{OnPlatform Android=AccordionFontIcons.ttf#,
                                           WinUI=AccordionFontIcons.ttf#AccordionFontIcons,
                                           MacCatalyst=AccordionFontIcons,
                                           iOS=AccordionFontIcons}'
                   VerticalOptions="Center" 
                   VerticalTextAlignment="Center"/>
            
            <!-- Header Text -->
            <Label CharacterSpacing="0.25" 
                   FontFamily="Roboto-Regular" 
                   Text="Invoice Date" 
                   FontSize="14" 
                   Grid.Column="1" 
                   VerticalOptions="CenterAndExpand"/>
        </Grid>
    </syncfusion:SfExpander.Header>
    
    <syncfusion:SfExpander.Content>
        <Grid Padding="18,8,0,18">
            <Label CharacterSpacing="0.25" 
                   FontFamily="Roboto-Regular" 
                   Text="11:03 AM, 15 January 2019" 
                   FontSize="14" 
                   VerticalOptions="CenterAndExpand"/>
        </Grid>
    </syncfusion:SfExpander.Content>
</syncfusion:SfExpander>
```

### C# Implementation

```csharp
var expander = new SfExpander
{
    AnimationDuration = 200,
    IsExpanded = true
};

// Header with icon and text
var headerGrid = new Grid();
headerGrid.RowDefinitions.Add(new RowDefinition { Height = 48 });
headerGrid.ColumnDefinitions.Add(new ColumnDefinition { Width = 35 });
headerGrid.ColumnDefinitions.Add(new ColumnDefinition { Width = GridLength.Star });

var icon = new Label
{
    Text = "\uE703",
    FontSize = 16,
    Margin = new Thickness(14, 2, 2, 2),
    FontFamily = DeviceInfo.Platform == DevicePlatform.Android 
        ? "AccordionFontIcons.ttf#" 
        : "AccordionFontIcons",
    VerticalOptions = LayoutOptions.Center,
    VerticalTextAlignment = TextAlignment.Center
};
headerGrid.Children.Add(icon);

var headerLabel = new Label
{
    Text = "Invoice Date",
    FontSize = 14,
    CharacterSpacing = 0.25,
    FontFamily = "Roboto-Regular",
    VerticalOptions = LayoutOptions.CenterAndExpand
};
Grid.SetColumn(headerLabel, 1);
headerGrid.Children.Add(headerLabel);

expander.Header = headerGrid;

// Content
var contentGrid = new Grid { Padding = new Thickness(18, 8, 0, 18) };
contentGrid.Children.Add(new Label
{
    Text = "11:03 AM, 15 January 2019",
    FontSize = 14,
    CharacterSpacing = 0.25,
    FontFamily = "Roboto-Regular",
    VerticalOptions = LayoutOptions.CenterAndExpand
});

expander.Content = contentGrid;
```

---

## Multi-Expander Layouts

Create accordion-style layouts with multiple expanders:

```xml
<ScrollView>
    <StackLayout Spacing="8" Padding="8">
        
        <!-- Section 1 -->
        <Border StrokeShape="RoundRectangle 8" Stroke="#CAC4D0">
            <syncfusion:SfExpander AnimationDuration="200" IsExpanded="True">
                <syncfusion:SfExpander.Header>
                    <Grid Padding="12">
                        <Label Text="Personal Information" FontSize="16" FontAttributes="Bold"/>
                    </Grid>
                </syncfusion:SfExpander.Header>
                <syncfusion:SfExpander.Content>
                    <StackLayout Padding="15" Spacing="8">
                        <Label Text="Name: John Doe"/>
                        <Label Text="Email: john@example.com"/>
                        <Label Text="Phone: (555) 123-4567"/>
                    </StackLayout>
                </syncfusion:SfExpander.Content>
            </syncfusion:SfExpander>
        </Border>
        
        <!-- Section 2 -->
        <Border StrokeShape="RoundRectangle 8" Stroke="#CAC4D0">
            <syncfusion:SfExpander AnimationDuration="200" IsExpanded="False">
                <syncfusion:SfExpander.Header>
                    <Grid Padding="12">
                        <Label Text="Shipping Address" FontSize="16" FontAttributes="Bold"/>
                    </Grid>
                </syncfusion:SfExpander.Header>
                <syncfusion:SfExpander.Content>
                    <StackLayout Padding="15" Spacing="8">
                        <Label Text="123 Main Street"/>
                        <Label Text="New York, NY 10001"/>
                        <Label Text="United States"/>
                    </StackLayout>
                </syncfusion:SfExpander.Content>
            </syncfusion:SfExpander>
        </Border>
        
        <!-- Section 3 -->
        <Border StrokeShape="RoundRectangle 8" Stroke="#CAC4D0">
            <syncfusion:SfExpander AnimationDuration="200" IsExpanded="False">
                <syncfusion:SfExpander.Header>
                    <Grid Padding="12">
                        <Label Text="Payment Method" FontSize="16" FontAttributes="Bold"/>
                    </Grid>
                </syncfusion:SfExpander.Header>
                <syncfusion:SfExpander.Content>
                    <StackLayout Padding="15" Spacing="8">
                        <Label Text="Card ending in 1234"/>
                        <Label Text="Expires: 12/25"/>
                    </StackLayout>
                </syncfusion:SfExpander.Content>
            </syncfusion:SfExpander>
        </Border>
        
    </StackLayout>
</ScrollView>
```

---

## Best Practices

### 1. Always Use Layout Containers

Wrap content in Grid, StackLayout, or other layout controls - never use individual controls as direct children.

### 2. Set Appropriate Padding

Add padding to content for better visual spacing:
```xml
<syncfusion:SfExpander.Content>
    <Grid Padding="15">
        <!-- Content here -->
    </Grid>
</syncfusion:SfExpander.Content>
```

### 3. Platform-Specific Font Families

Use `OnPlatform` for icon fonts that require different paths per platform:
```xml
FontFamily='{OnPlatform Android=MyFont.ttf#,
                       WinUI=MyFont.ttf#MyFont,
                       MacCatalyst=MyFont,
                       iOS=MyFont}'
```

### 4. Consistent Height for Headers

Set fixed or consistent header heights for uniform appearance:
```xml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="48"/>
    </Grid.RowDefinitions>
    <!-- Header content -->
</Grid>
```

### 5. Use Borders for Visual Separation

Wrap expanders in Border controls for professional styling:
```xml
<Border StrokeShape="RoundRectangle 8" Stroke="#CAC4D0" StrokeThickness="1">
    <syncfusion:SfExpander>
        <!-- Expander content -->
    </syncfusion:SfExpander>
</Border>
```

---

## Common Pitfalls

### ❌ Pitfall 1: Label as Direct Child

**Problem:** Loading Label directly in Header or Content causes runtime exception.

```xml
<!-- WRONG - Will crash -->
<syncfusion:SfExpander.Header>
    <Label Text="Header"/>
</syncfusion:SfExpander.Header>
```

**Solution:** Wrap in a layout container.

```xml
<!-- CORRECT -->
<syncfusion:SfExpander.Header>
    <Grid>
        <Label Text="Header"/>
    </Grid>
</syncfusion:SfExpander.Header>
```

### ❌ Pitfall 2: Missing Padding

**Problem:** Content touches edges, looks cramped.

**Solution:** Add padding to content Grid/StackLayout.

```xml
<syncfusion:SfExpander.Content>
    <Grid Padding="15">
        <!-- Content with proper spacing -->
    </Grid>
</syncfusion:SfExpander.Content>
```

### ❌ Pitfall 3: Inconsistent Header Heights

**Problem:** Multiple expanders have varying header heights, looks unprofessional.

**Solution:** Set fixed RowDefinition height in all headers.

```xml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="48"/>
    </Grid.RowDefinitions>
</Grid>
```

---

## Advanced Customization

### Custom Header with Button

```xml
<syncfusion:SfExpander.Header>
    <Grid Padding="10">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*"/>
            <ColumnDefinition Width="Auto"/>
        </Grid.ColumnDefinitions>
        
        <Label Text="Settings" FontSize="16" VerticalOptions="Center"/>
        <Button Text="Edit" 
                Grid.Column="1" 
                WidthRequest="60" 
                HeightRequest="32"
                Clicked="OnEditClicked"/>
    </Grid>
</syncfusion:SfExpander.Header>
```

### Content with Nested Views

```xml
<syncfusion:SfExpander.Content>
    <StackLayout Padding="15" Spacing="10">
        
        <!-- Entry field -->
        <Entry Placeholder="Enter name" />
        
        <!-- Picker -->
        <Picker Title="Select option">
            <Picker.Items>
                <x:String>Option 1</x:String>
                <x:String>Option 2</x:String>
            </Picker.Items>
        </Picker>
        
        <!-- Switch -->
        <Grid>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="*"/>
                <ColumnDefinition Width="Auto"/>
            </Grid.ColumnDefinitions>
            <Label Text="Enable notifications" VerticalOptions="Center"/>
            <Switch Grid.Column="1"/>
        </Grid>
        
    </StackLayout>
</syncfusion:SfExpander.Content>
```

---

## Related Features

- **Getting Started:** See `getting-started.md` for basic setup
- **Animation and Events:** See `animation-events.md` for expand/collapse control
- **Appearance:** See `appearance-styling.md` for header styling options
