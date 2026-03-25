# Tooltips in .NET MAUI Sunburst Chart

## Table of Contents
- [Overview](#overview)
- [Enabling Tooltips](#enabling-tooltips)
- [Tooltip Customization](#tooltip-customization)
- [Custom Tooltip Templates](#custom-tooltip-templates)
- [Complete Examples](#complete-examples)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

Tooltips provide additional segment information when users tap on chart segments. By default, tooltips display the segment's category name and value, but they can be extensively customized with styling options and custom templates for rich data presentations.

**Default tooltip behavior:**
- Displays category name from GroupMemberPath
- Shows numeric value from ValueMemberPath
- Appears on tap/hover
- Auto-dismisses after duration or on next interaction

## Enabling Tooltips

Enable tooltips using the `EnableTooltip` property.

**Property:**
- **EnableTooltip**: Enables/disables tooltip display
- **Default**: `False`

**XAML:**
```xml
<sunburst:SfSunburstChart EnableTooltip="True"
                          ItemsSource="{Binding DataSource}"
                          ValueMemberPath="EmployeesCount">
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Country"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Department"/>
    </sunburst:SfSunburstChart.Levels>
</sunburst:SfSunburstChart>
```

**C#:**
```csharp
SfSunburstChart sunburst = new SfSunburstChart();
sunburst.ItemsSource = (new SunburstViewModel()).DataSource;
sunburst.ValueMemberPath = "EmployeesCount";
sunburst.EnableTooltip = true;
// ... configure levels
this.Content = sunburst;
```

**What displays:**
- First line: Category name (e.g., "USA", "Sales")
- Second line: Value (e.g., "150")

## Tooltip Customization

Customize tooltip appearance using the `TooltipSettings` property with `SunburstTooltipSettings`.

### Available Properties

**Visual Properties:**
- **Background**: Tooltip background color (Brush)
- **TextColor**: Text color (Color)
- **FontFamily**: Font family name (string)
- **FontSize**: Font size in device-independent units (float)
- **FontAttributes**: Font style - Bold, Italic, None

**Behavior Properties:**
- **Duration**: Display duration in milliseconds (int)
- **Margin**: Space around tooltip content (Thickness)

### Basic Customization Example

**XAML:**
```xml
<sunburst:SfSunburstChart EnableTooltip="True"
                          ItemsSource="{Binding DataSource}"
                          ValueMemberPath="Sales">
    
    <sunburst:SfSunburstChart.TooltipSettings>
        <sunburst:SunburstTooltipSettings   
            Background="White" 
            TextColor="Black"  
            FontSize="14" 
            FontAttributes="Bold" 
            Duration="5000"/>
    </sunburst:SfSunburstChart.TooltipSettings>
    
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Region"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Product"/>
    </sunburst:SfSunburstChart.Levels>
</sunburst:SfSunburstChart>
```

**C#:**
```csharp
SfSunburstChart sunburst = new SfSunburstChart();
sunburst.EnableTooltip = true;

SunburstTooltipSettings tooltipSettings = new SunburstTooltipSettings()
{
    TextColor = Colors.Black,
    Background = Brush.White,
    FontSize = 14,
    FontAttributes = FontAttributes.Bold,
    Duration = 5000
};
sunburst.TooltipSettings = tooltipSettings;

// ... configure data and levels
this.Content = sunburst;
```

### Style Variations

**Dark Theme Tooltip:**
```xml
<sunburst:SunburstTooltipSettings   
    Background="#2D2D2D" 
    TextColor="White"  
    FontSize="13" 
    FontAttributes="None"
    Duration="3000"/>
```

**Accent Color Tooltip:**
```xml
<sunburst:SunburstTooltipSettings   
    Background="{StaticResource PrimaryColor}" 
    TextColor="White"  
    FontSize="12" 
    FontAttributes="Bold"
    Duration="4000"/>
```

**Subtle Light Tooltip:**
```xml
<sunburst:SunburstTooltipSettings   
    Background="#F5F5F5" 
    TextColor="#333333"  
    FontSize="12" 
    FontFamily="Segoe UI"
    Duration="3500"/>
```

**High Contrast Tooltip:**
```xml
<sunburst:SunburstTooltipSettings   
    Background="Black" 
    TextColor="Yellow"  
    FontSize="14" 
    FontAttributes="Bold"
    Duration="5000"/>
```

### Duration Guidelines

**Quick glimpse:**
```xml
<sunburst:SunburstTooltipSettings Duration="2000"/>
<!-- 2 seconds - for simple data -->
```

**Standard:**
```xml
<sunburst:SunburstTooltipSettings Duration="3000"/>
<!-- 3 seconds - default recommended -->
```

**Extended:**
```xml
<sunburst:SunburstTooltipSettings Duration="5000"/>
<!-- 5 seconds - for complex information -->
```

**Persistent:**
```xml
<sunburst:SunburstTooltipSettings Duration="10000"/>
<!-- 10 seconds - requires dismissal or timeout -->
```

### Margin Customization

Add spacing around tooltip content:

**XAML:**
```xml
<sunburst:SunburstTooltipSettings Margin="10,5,10,5"/>
<!-- Left: 10, Top: 5, Right: 10, Bottom: 5 -->
```

**C#:**
```csharp
tooltipSettings.Margin = new Thickness(10, 5, 10, 5);
```

**Uniform margin:**
```xml
<sunburst:SunburstTooltipSettings Margin="8"/>
<!-- 8 units on all sides -->
```

## Custom Tooltip Templates

Create rich, custom tooltip layouts using the `TooltipTemplate` property with DataTemplate.

### Template Binding Context

The template's binding context provides access to:
- **Item**: Array of values (Item[0] = category, Item[1] = value, etc.)
- **Fill**: Segment color
- Custom data model properties via data context

### Basic Custom Template Example

**XAML:**
```xml
<sunburst:SfSunburstChart EnableTooltip="True" 
                          ItemsSource="{Binding DataSource}"
                          ValueMemberPath="Sales"
                          TooltipTemplate="{StaticResource TooltipTemplate}">
    
    <sunburst:SfSunburstChart.Resources>
        <ResourceDictionary>
            <DataTemplate x:Key="TooltipTemplate">
                <StackLayout Orientation="Horizontal" Padding="10">
                    <Rectangle HeightRequest="30" 
                             WidthRequest="8" 
                             Fill="{Binding Fill}"/>
                    <StackLayout Orientation="Vertical" Spacing="2" Margin="10,0,0,0">
                        <Label Text="{Binding Item[0]}" 
                             TextColor="White" 
                             FontSize="14" 
                             FontAttributes="Bold"/>
                        <Label Text="{Binding Item[1], StringFormat='Sales: ${0:N0}'}" 
                             TextColor="LightGray" 
                             FontSize="12"/>
                    </StackLayout>
                </StackLayout>
            </DataTemplate>
        </ResourceDictionary>
    </sunburst:SfSunburstChart.Resources>
    
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Region"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Country"/>
    </sunburst:SfSunburstChart.Levels>
</sunburst:SfSunburstChart>
```

**C#:**
```csharp
SfSunburstChart sunburst = new SfSunburstChart();
sunburst.EnableTooltip = true;
sunburst.TooltipTemplate = (DataTemplate)sunburst.Resources["TooltipTemplate"];
// ... configure data and levels
this.Content = sunburst;
```

### Advanced Template Examples

**Example 1: Card Style with Icon**

```xml
<DataTemplate x:Key="CardTooltip">
    <Border BackgroundColor="White" 
            StrokeThickness="0"
            Padding="15">
        <Border.Shadow>
            <Shadow Brush="Black" Opacity="0.3" Radius="10"/>
        </Border.Shadow>
        <StackLayout Spacing="8">
            <HorizontalStackLayout Spacing="10">
                <Image Source="info_icon.png" 
                     HeightRequest="24" 
                     WidthRequest="24"/>
                <Label Text="{Binding Item[0]}" 
                     FontSize="16" 
                     FontAttributes="Bold"
                     TextColor="Black"/>
            </HorizontalStackLayout>
            <BoxView HeightRequest="2" 
                   BackgroundColor="{Binding Fill}"/>
            <Label Text="{Binding Item[1], StringFormat='Count: {0}'}" 
                 FontSize="14"
                 TextColor="#555"/>
        </StackLayout>
    </Border>
</DataTemplate>
```

**Example 2: Multi-Line Information**

```xml
<DataTemplate x:Key="DetailedTooltip">
    <StackLayout BackgroundColor="#2C3E50" 
                Padding="12"
                Spacing="4">
        <Label Text="{Binding Item[0]}" 
             TextColor="White" 
             FontSize="15" 
             FontAttributes="Bold"/>
        <Label Text="{Binding Item[1], StringFormat='Value: {0:N2}'}" 
             TextColor="#ECF0F1" 
             FontSize="13"/>
        <Label Text="Tap to drill down" 
             TextColor="#95A5A6" 
             FontSize="11" 
             FontAttributes="Italic"/>
    </StackLayout>
</DataTemplate>
```

**Example 3: Percentage Display**

```xml
<DataTemplate x:Key="PercentageTooltip">
    <Grid BackgroundColor="White" 
         Padding="10">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto"/>
            <ColumnDefinition Width="*"/>
        </Grid.ColumnDefinitions>
        
        <Ellipse Fill="{Binding Fill}" 
                WidthRequest="12" 
                HeightRequest="12"
                Grid.Row="0" 
                Grid.Column="0"
                Margin="0,0,8,0"/>
        <Label Text="{Binding Item[0]}" 
             Grid.Row="0" 
             Grid.Column="1"
             TextColor="Black" 
             FontSize="14" 
             FontAttributes="Bold"/>
        
        <Label Text="{Binding Item[1], StringFormat='{0:P1}'}" 
             Grid.Row="1" 
             Grid.Column="1"
             TextColor="#666" 
             FontSize="20"
             FontAttributes="Bold"/>
    </Grid>
</DataTemplate>
```

**Example 4: Hierarchical Path Display**

```xml
<DataTemplate x:Key="PathTooltip">
    <StackLayout BackgroundColor="#1E1E1E" 
                Padding="12"
                Spacing="6">
        <!-- Assuming Item[0], Item[1], Item[2] are hierarchy levels -->
        <Label Text="{Binding Item[0], StringFormat='Region: {0}'}" 
             TextColor="#FFD700" 
             FontSize="11"/>
        <Label Text="{Binding Item[1], StringFormat='Country: {0}'}" 
             TextColor="#FFA500" 
             FontSize="11"/>
        <Label Text="{Binding Item[2]}" 
             TextColor="White" 
             FontSize="14" 
             FontAttributes="Bold"/>
        <BoxView HeightRequest="1" 
               BackgroundColor="#555"/>
        <Label Text="{Binding Item[3], StringFormat='Total: {0:N0}'}" 
             TextColor="#4CAF50" 
             FontSize="13"
             FontAttributes="Bold"/>
    </StackLayout>
</DataTemplate>
```

### Template Best Practices

**For glass effect compatibility:**
Set background to Transparent when using liquid glass effect:

```xml
<DataTemplate x:Key="GlassTooltip">
    <StackLayout BackgroundColor="Transparent" Padding="12">
        <!-- Tooltip content -->
    </StackLayout>
</DataTemplate>
```

**Performance considerations:**
- Keep templates simple for smooth rendering
- Avoid complex nested layouts
- Don't use heavy images or animations
- Test on actual devices

## Complete Examples

### Example 1: Professional Business Tooltip

```xml
<sunburst:SfSunburstChart EnableTooltip="True"
                          ItemsSource="{Binding EmployeeData}"
                          ValueMemberPath="Count">
    
    <sunburst:SfSunburstChart.TooltipSettings>
        <sunburst:SunburstTooltipSettings   
            Background="#0078D4" 
            TextColor="White"  
            FontSize="13" 
            FontFamily="Segoe UI"
            FontAttributes="None"
            Duration="3500"
            Margin="10,6,10,6"/>
    </sunburst:SfSunburstChart.TooltipSettings>
    
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Department"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Team"/>
    </sunburst:SfSunburstChart.Levels>
</sunburst:SfSunburstChart>
```

### Example 2: Dark Mode with Custom Template

```xml
<sunburst:SfSunburstChart EnableTooltip="True"
                          ItemsSource="{Binding SalesData}"
                          ValueMemberPath="Revenue"
                          TooltipTemplate="{StaticResource DarkTemplate}">
    
    <sunburst:SfSunburstChart.Resources>
        <ResourceDictionary>
            <DataTemplate x:Key="DarkTemplate">
                <Border BackgroundColor="#1A1A1A" 
                       StrokeThickness="1"
                       Stroke="#333"
                       Padding="12">
                    <StackLayout Spacing="6">
                        <HorizontalStackLayout Spacing="8">
                            <Rectangle Fill="{Binding Fill}" 
                                     WidthRequest="4" 
                                     HeightRequest="20"/>
                            <Label Text="{Binding Item[0]}" 
                                 TextColor="White" 
                                 FontSize="14" 
                                 FontAttributes="Bold"/>
                        </HorizontalStackLayout>
                        <Label Text="{Binding Item[1], StringFormat='Revenue: ${0:N0}'}" 
                             TextColor="#AAA" 
                             FontSize="12"/>
                    </StackLayout>
                </Border>
            </DataTemplate>
        </ResourceDictionary>
    </sunburst:SfSunburstChart.Resources>
    
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Product"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Category"/>
    </sunburst:SfSunburstChart.Levels>
</sunburst:SfSunburstChart>
```

### Example 3: Mobile-Optimized Tooltip

```xml
<sunburst:SfSunburstChart EnableTooltip="True"
                          ItemsSource="{Binding Data}"
                          ValueMemberPath="Value">
    
    <sunburst:SfSunburstChart.TooltipSettings>
        <sunburst:SunburstTooltipSettings   
            Background="Black" 
            TextColor="White"  
            FontSize="16" 
            FontAttributes="Bold"
            Duration="2500"
            Margin="12"/>
    </sunburst:SfSunburstChart.TooltipSettings>
    
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Category"/>
    </sunburst:SfSunburstChart.Levels>
</sunburst:SfSunburstChart>
```

## Best Practices

### Tooltip Content

**Keep it concise:**
- Show essential information only
- Avoid long text that requires scrolling
- Use abbreviations when appropriate
- Consider 2-3 lines maximum

**Format numbers:**
- Use appropriate formats (currency, percentage, etc.)
- Round to meaningful precision
- Add units where applicable

**Provide context:**
- Show category hierarchy when relevant
- Include percentage of total if helpful
- Add comparison data when meaningful

### Visual Design

**Contrast:**
- Ensure high contrast between text and background
- Test in both light and dark environments
- Consider accessibility guidelines (WCAG AA minimum)

**Sizing:**
- Mobile: 14-16pt font for readability
- Tablet: 12-14pt font
- Desktop: 11-13pt font
- Adjust for target audience (larger for seniors)

**Duration:**
- Simple data: 2-3 seconds
- Complex information: 4-5 seconds
- Very detailed: 5-7 seconds
- Never exceed 10 seconds

### Custom Templates

**Performance:**
- Keep templates lightweight
- Avoid complex layouts
- Don't nest too many containers
- Test on lower-end devices

**Consistency:**
- Match your app's design language
- Use consistent colors and fonts
- Align with other UI elements
- Follow platform conventions

**Responsiveness:**
- Test various content lengths
- Handle missing data gracefully
- Ensure tooltips don't overflow screen
- Consider different screen sizes

## Troubleshooting

**Issue:** Tooltips not appearing
- **Solution:** Verify `EnableTooltip="True"` is set
- Ensure you're tapping/hovering on actual segments
- Check that segments aren't too small to tap
- Verify touch input is working (test other interactions)

**Issue:** Tooltip text not readable
- **Solution:** Increase FontSize property
- Adjust TextColor for better contrast with Background
- Use FontAttributes="Bold" for emphasis
- Test on actual devices, not just emulator

**Issue:** Tooltip disappears too quickly
- **Solution:** Increase Duration value (e.g., 5000 for 5 seconds)
- Consider user reading speed
- Balance between helpful and annoying
- Test with actual users if possible

**Issue:** Custom template not displaying
- **Solution:** Verify TooltipTemplate is properly bound
- Check ResourceDictionary key matches
- Ensure DataTemplate syntax is correct
- Test with simple template first, then add complexity

**Issue:** Template background conflicts with glass effect
- **Solution:** Set template background to Transparent
- The glass effect provides its own background
- Remove any BackgroundColor properties from template root
- Test with EnableLiquidGlassEffect property

**Issue:** Tooltip shows wrong information
- **Solution:** Verify Item[0], Item[1] bindings in template
- Item[0] is category name, Item[1] is value
- Check data model for correct property values
- Test with simple default tooltip first

**Issue:** Tooltip positioned incorrectly or off-screen
- **Solution:** This is automatically handled by the control
- Ensure chart has adequate margin in container
- Reduce Margin property in TooltipSettings if needed
- Test on different screen sizes

**Issue:** Tooltip appears behind other UI elements
- **Solution:** Tooltips should appear in front by default
- Check Z-index of overlapping elements
- Verify no absolute-positioned elements covering chart
- Test in isolated layout first
