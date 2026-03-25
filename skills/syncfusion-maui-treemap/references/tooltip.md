# Tooltip in .NET MAUI TreeMap

Tooltips provide additional information when users interact with leaf items in the TreeMap. They enhance usability by displaying details without cluttering the visualization.

## Overview

Tooltips appear when users hover over or tap leaf items, showing contextual information about the data point.

**Key Properties:**
- `ShowToolTip`: Enable/disable tooltips
- `ToolTipTemplate`: Custom tooltip layout (DataTemplate)

**Basic Setup:**

```xml
<treemap:SfTreeMap DataSource="{Binding Data}"
                   PrimaryValuePath="Size"
                   ShowToolTip="True">
    <treemap:SfTreeMap.LeafItemSettings>
        <treemap:TreeMapLeafItemSettings LabelPath="Name" />
    </treemap:SfTreeMap.LeafItemSettings>
</treemap:SfTreeMap>
```

## Enabling Tooltips

### ShowToolTip Property

Controls whether tooltips display on interaction.

**Enable Tooltips:**
```xml
<treemap:SfTreeMap DataSource="{Binding PopulationData}"
                   PrimaryValuePath="Population"
                   ShowToolTip="True">
    <treemap:SfTreeMap.LeafItemSettings>
        <treemap:TreeMapLeafItemSettings LabelPath="Country" />
    </treemap:SfTreeMap.LeafItemSettings>
</treemap:SfTreeMap>
```

**C# Implementation:**
```csharp
treeMap.ShowToolTip = true;
treeMap.PrimaryValuePath = "Population";
treeMap.LeafItemSettings = new TreeMapLeafItemSettings
{
    LabelPath = "Country"
};
```

**Result:** When users hover/tap a leaf item, a tooltip displays showing the country name and population value.

### Disable Tooltips (Default)

```xml
<treemap:SfTreeMap ShowToolTip="False" />
```

```csharp
treeMap.ShowToolTip = false;
```

## Default Tooltip Behavior

When `ShowToolTip="True"` without a custom template, the TreeMap displays a default tooltip with:
- Label text (from `LabelPath` property)
- Primary value (from `PrimaryValuePath` property)

**Example:**

```csharp
public class Country
{
    public string Name { get; set; }
    public long Population { get; set; }
}

// TreeMap setup
treeMap.DataSource = countries;
treeMap.PrimaryValuePath = "Population";
treeMap.ShowToolTip = true;
treeMap.LeafItemSettings = new TreeMapLeafItemSettings { LabelPath = "Name" };
```

**Default Tooltip Display:**
```
Name: United States
Population: 331,000,000
```

The format automatically includes both label and value in a readable format.

## Custom Tooltip Templates

Create custom tooltip layouts using `ToolTipTemplate` with a `DataTemplate`.

### Basic Custom Template

**XAML:**
```xml
<treemap:SfTreeMap DataSource="{Binding PopulationData}"
                   PrimaryValuePath="Population"
                   ShowToolTip="True">
    <treemap:SfTreeMap.LeafItemSettings>
        <treemap:TreeMapLeafItemSettings LabelPath="Country" />
    </treemap:SfTreeMap.LeafItemSettings>
    
    <treemap:SfTreeMap.ToolTipTemplate>
        <DataTemplate>
            <VerticalStackLayout Padding="10"
                                 BackgroundColor="Black"
                                 Opacity="0.9">
                <Label Text="{Binding Data.Country}"
                       TextColor="White"
                       FontSize="16"
                       FontAttributes="Bold" />
                <Label Text="{Binding Data.Population, StringFormat='Population: {0:N0}'}"
                       TextColor="White"
                       FontSize="14" />
            </VerticalStackLayout>
        </DataTemplate>
    </treemap:SfTreeMap.ToolTipTemplate>
</treemap:SfTreeMap>
```

**Key Points:**
- Access data via `{Binding Data.PropertyName}`
- Use `StringFormat` for number/date formatting
- Apply styling (colors, fonts, padding) directly

### Multi-Property Tooltip

Display multiple data properties in a single tooltip.

**Example Model:**
```csharp
public class City
{
    public string Name { get; set; }
    public long Population { get; set; }
    public double Area { get; set; }  // in square miles
    public string Country { get; set; }
}
```

**XAML Template:**
```xml
<treemap:SfTreeMap DataSource="{Binding Cities}"
                   PrimaryValuePath="Population"
                   ShowToolTip="True">
    <treemap:SfTreeMap.LeafItemSettings>
        <treemap:TreeMapLeafItemSettings LabelPath="Name" />
    </treemap:SfTreeMap.LeafItemSettings>
    
    <treemap:SfTreeMap.ToolTipTemplate>
        <DataTemplate>
            <Frame BackgroundColor="#2C2C2C"
                   BorderColor="#4CAF50"
                   CornerRadius="8"
                   Padding="12">
                <VerticalStackLayout Spacing="5">
                    <Label Text="{Binding Data.Name}"
                           TextColor="White"
                           FontSize="16"
                           FontAttributes="Bold" />
                    <BoxView Color="#4CAF50" 
                             HeightRequest="1" />
                    <Label Text="{Binding Data.Population, StringFormat='Population: {0:N0}'}"
                           TextColor="LightGray"
                           FontSize="13" />
                    <Label Text="{Binding Data.Area, StringFormat='Area: {0:N2} sq mi'}"
                           TextColor="LightGray"
                           FontSize="13" />
                    <Label Text="{Binding Data.Country}"
                           TextColor="LightGray"
                           FontSize="13"
                           FontAttributes="Italic" />
                </VerticalStackLayout>
            </Frame>
        </DataTemplate>
    </treemap:SfTreeMap.ToolTipTemplate>
</treemap:SfTreeMap>
```

**Result:** A styled tooltip showing city name (bold), separator line, population, area, and country.

### Styled Tooltip with Icons

```xml
<treemap:SfTreeMap DataSource="{Binding SalesData}"
                   PrimaryValuePath="Revenue"
                   ShowToolTip="True">
    <treemap:SfTreeMap.LeafItemSettings>
        <treemap:TreeMapLeafItemSettings LabelPath="Product" />
    </treemap:SfTreeMap.LeafItemSettings>
    
    <treemap:SfTreeMap.ToolTipTemplate>
        <DataTemplate>
            <Frame BackgroundColor="White"
                   BorderColor="Gray"
                   CornerRadius="10"
                   Padding="15"
                   HasShadow="True">
                <Grid RowDefinitions="Auto,Auto,Auto" 
                      ColumnDefinitions="Auto,*"
                      RowSpacing="8"
                      ColumnSpacing="10">
                    
                    <!-- Product Name -->
                    <Label Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"
                           Text="{Binding Data.Product}"
                           TextColor="#212121"
                           FontSize="16"
                           FontAttributes="Bold" />
                    
                    <!-- Revenue -->
                    <Label Grid.Row="1" Grid.Column="0"
                           Text="💰"
                           FontSize="16" />
                    <Label Grid.Row="1" Grid.Column="1"
                           Text="{Binding Data.Revenue, StringFormat='Revenue: ${0:N0}'}"
                           TextColor="#424242"
                           FontSize="14" />
                    
                    <!-- Units Sold -->
                    <Label Grid.Row="2" Grid.Column="0"
                           Text="📦"
                           FontSize="16" />
                    <Label Grid.Row="2" Grid.Column="1"
                           Text="{Binding Data.UnitsSold, StringFormat='Units: {0:N0}'}"
                           TextColor="#424242"
                           FontSize="14" />
                </Grid>
            </Frame>
        </DataTemplate>
    </treemap:SfTreeMap.ToolTipTemplate>
</treemap:SfTreeMap>
```

### Conditional Formatting in Tooltip

Use value converters or calculated properties to format tooltip content conditionally.

**Example with Calculated Property:**
```csharp
public class Product
{
    public string Name { get; set; }
    public decimal Revenue { get; set; }
    public decimal Target { get; set; }
    
    public string PerformanceStatus => Revenue >= Target ? "✅ Meets Target" : "⚠️ Below Target";
    public Color StatusColor => Revenue >= Target ? Colors.Green : Colors.Red;
}
```

**XAML Template:**
```xml
<treemap:SfTreeMap.ToolTipTemplate>
    <DataTemplate>
        <Frame BackgroundColor="White" Padding="12" CornerRadius="8">
            <VerticalStackLayout Spacing="6">
                <Label Text="{Binding Data.Name}"
                       FontSize="16"
                       FontAttributes="Bold" />
                <Label Text="{Binding Data.Revenue, StringFormat='Revenue: ${0:N0}'}"
                       FontSize="14" />
                <Label Text="{Binding Data.Target, StringFormat='Target: ${0:N0}'}"
                       FontSize="14" />
                <Label Text="{Binding Data.PerformanceStatus}"
                       TextColor="{Binding Data.StatusColor}"
                       FontSize="14"
                       FontAttributes="Bold" />
            </VerticalStackLayout>
        </Frame>
    </DataTemplate>
</treemap:SfTreeMap.ToolTipTemplate>
```

## Tooltip with Images

Display images within tooltips for enhanced visual context.

**Example Model:**
```csharp
public class Employee
{
    public string Name { get; set; }
    public string Department { get; set; }
    public decimal Salary { get; set; }
    public string PhotoUrl { get; set; }
}
```

**XAML Template:**
```xml
<treemap:SfTreeMap.ToolTipTemplate>
    <DataTemplate>
        <Frame BackgroundColor="White"
               CornerRadius="12"
               Padding="15"
               HasShadow="True">
            <Grid ColumnDefinitions="Auto,*" ColumnSpacing="12">
                <!-- Employee Photo -->
                <Frame Grid.Column="0"
                       WidthRequest="60"
                       HeightRequest="60"
                       CornerRadius="30"
                       Padding="0"
                       IsClippedToBounds="True">
                    <Image Source="{Binding Data.PhotoUrl}"
                           Aspect="AspectFill" />
                </Frame>
                
                <!-- Employee Details -->
                <VerticalStackLayout Grid.Column="1" Spacing="4">
                    <Label Text="{Binding Data.Name}"
                           TextColor="#212121"
                           FontSize="16"
                           FontAttributes="Bold" />
                    <Label Text="{Binding Data.Department}"
                           TextColor="#757575"
                           FontSize="13" />
                    <Label Text="{Binding Data.Salary, StringFormat='${0:N0}'}"
                           TextColor="#424242"
                           FontSize="14"
                           FontAttributes="Bold" />
                </VerticalStackLayout>
            </Grid>
        </Frame>
    </DataTemplate>
</treemap:SfTreeMap.ToolTipTemplate>
```

## Tooltip Data Context

The tooltip `DataTemplate` receives a binding context with the data object accessible via `{Binding Data}`.

**Structure:**
```csharp
// Tooltip binding context
{
    Data = yourDataObject  // The actual item from DataSource
}
```

**Accessing Properties:**
```xml
<!-- If your model is Country with Name and Population properties -->
<Label Text="{Binding Data.Name}" />
<Label Text="{Binding Data.Population}" />
```

**Important:** Always use `Data.` prefix when binding to your model properties in the tooltip template.

## String Formatting

Use `StringFormat` to format numeric and date values in tooltips.

### Number Formatting

```xml
<!-- Thousands separator -->
<Label Text="{Binding Data.Population, StringFormat='{0:N0}'}" />
<!-- Result: 1,234,567 -->

<!-- Currency -->
<Label Text="{Binding Data.Revenue, StringFormat='${0:N2}'}" />
<!-- Result: $1,234.56 -->

<!-- Percentage -->
<Label Text="{Binding Data.GrowthRate, StringFormat='{0:P2}'}" />
<!-- Result: 12.34% -->

<!-- Custom decimal places -->
<Label Text="{Binding Data.Area, StringFormat='{0:F2} sq mi'}" />
<!-- Result: 123.45 sq mi -->
```

### Date Formatting

```xml
<!-- Short date -->
<Label Text="{Binding Data.Date, StringFormat='{0:d}'}" />
<!-- Result: 3/20/2026 -->

<!-- Long date -->
<Label Text="{Binding Data.Date, StringFormat='{0:D}'}" />
<!-- Result: Friday, March 20, 2026 -->

<!-- Custom format -->
<Label Text="{Binding Data.Date, StringFormat='{0:MMM dd, yyyy}'}" />
<!-- Result: Mar 20, 2026 -->
```

## Practical Examples

### Example 1: Simple Sales Tooltip

```xml
<treemap:SfTreeMap DataSource="{Binding SalesData}"
                   PrimaryValuePath="Amount"
                   ShowToolTip="True">
    <treemap:SfTreeMap.LeafItemSettings>
        <treemap:TreeMapLeafItemSettings LabelPath="ProductName" />
    </treemap:SfTreeMap.LeafItemSettings>
    
    <treemap:SfTreeMap.ToolTipTemplate>
        <DataTemplate>
            <Frame BackgroundColor="#424242" 
                   Padding="12" 
                   CornerRadius="6">
                <VerticalStackLayout Spacing="5">
                    <Label Text="{Binding Data.ProductName}"
                           TextColor="White"
                           FontSize="15"
                           FontAttributes="Bold" />
                    <Label Text="{Binding Data.Amount, StringFormat='Sales: ${0:N0}'}"
                           TextColor="LightGray"
                           FontSize="13" />
                </VerticalStackLayout>
            </Frame>
        </DataTemplate>
    </treemap:SfTreeMap.ToolTipTemplate>
</treemap:SfTreeMap>
```

### Example 2: Portfolio Performance Tooltip

```csharp
public class Stock
{
    public string Symbol { get; set; }
    public string Company { get; set; }
    public decimal Value { get; set; }
    public decimal ChangePercent { get; set; }
    
    public Color ChangeColor => ChangePercent >= 0 ? Colors.Green : Colors.Red;
    public string ChangeIndicator => ChangePercent >= 0 ? "▲" : "▼";
}
```

```xml
<treemap:SfTreeMap.ToolTipTemplate>
    <DataTemplate>
        <Frame BackgroundColor="White"
               BorderColor="LightGray"
               CornerRadius="10"
               Padding="14"
               HasShadow="True">
            <VerticalStackLayout Spacing="6">
                <Label Text="{Binding Data.Symbol}"
                       TextColor="#212121"
                       FontSize="18"
                       FontAttributes="Bold" />
                <Label Text="{Binding Data.Company}"
                       TextColor="#757575"
                       FontSize="13" />
                <Label Text="{Binding Data.Value, StringFormat='${0:N2}'}"
                       TextColor="#424242"
                       FontSize="15"
                       FontAttributes="Bold" />
                <HorizontalStackLayout Spacing="5">
                    <Label Text="{Binding Data.ChangeIndicator}"
                           TextColor="{Binding Data.ChangeColor}"
                           FontSize="14" />
                    <Label Text="{Binding Data.ChangePercent, StringFormat='{0:F2}%'}"
                           TextColor="{Binding Data.ChangeColor}"
                           FontSize="14"
                           FontAttributes="Bold" />
                </HorizontalStackLayout>
            </VerticalStackLayout>
        </Frame>
    </DataTemplate>
</treemap:SfTreeMap.ToolTipTemplate>
```

### Example 3: Geographic Tooltip with Multiple Metrics

```csharp
public class Region
{
    public string Name { get; set; }
    public long Population { get; set; }
    public double Area { get; set; }
    public double GDP { get; set; }
    
    public double PopulationDensity => Population / Area;
    public double GDPPerCapita => GDP / Population;
}
```

```xml
<treemap:SfTreeMap.ToolTipTemplate>
    <DataTemplate>
        <Frame BackgroundColor="#F5F5F5"
               BorderColor="#2196F3"
               CornerRadius="12"
               Padding="16"
               HasShadow="True">
            <VerticalStackLayout Spacing="8">
                <Label Text="{Binding Data.Name}"
                       TextColor="#212121"
                       FontSize="17"
                       FontAttributes="Bold"
                       HorizontalOptions="Center" />
                
                <BoxView Color="#2196F3" 
                         HeightRequest="2"
                         HorizontalOptions="Fill" />
                
                <Grid RowDefinitions="Auto,Auto,Auto,Auto" 
                      ColumnDefinitions="120,*"
                      RowSpacing="5">
                    <Label Grid.Row="0" Grid.Column="0" 
                           Text="Population:" TextColor="#616161" FontSize="13" />
                    <Label Grid.Row="0" Grid.Column="1" 
                           Text="{Binding Data.Population, StringFormat='{0:N0}'}" 
                           TextColor="#212121" FontSize="13" FontAttributes="Bold" />
                    
                    <Label Grid.Row="1" Grid.Column="0" 
                           Text="Area:" TextColor="#616161" FontSize="13" />
                    <Label Grid.Row="1" Grid.Column="1" 
                           Text="{Binding Data.Area, StringFormat='{0:N0} sq mi'}" 
                           TextColor="#212121" FontSize="13" FontAttributes="Bold" />
                    
                    <Label Grid.Row="2" Grid.Column="0" 
                           Text="GDP:" TextColor="#616161" FontSize="13" />
                    <Label Grid.Row="2" Grid.Column="1" 
                           Text="{Binding Data.GDP, StringFormat='${0:N0}B'}" 
                           TextColor="#212121" FontSize="13" FontAttributes="Bold" />
                    
                    <Label Grid.Row="3" Grid.Column="0" 
                           Text="GDP/Capita:" TextColor="#616161" FontSize="13" />
                    <Label Grid.Row="3" Grid.Column="1" 
                           Text="{Binding Data.GDPPerCapita, StringFormat='${0:N0}'}" 
                           TextColor="#212121" FontSize="13" FontAttributes="Bold" />
                </Grid>
            </VerticalStackLayout>
        </Frame>
    </DataTemplate>
</treemap:SfTreeMap.ToolTipTemplate>
```

## Troubleshooting

### Issue 1: Tooltips Not Appearing

**Symptoms:** No tooltip shows on hover/tap

**Solutions:**
1. Verify `ShowToolTip="True"` is set
2. Ensure `LabelPath` is defined in LeafItemSettings
3. Check that data source is properly bound
4. Confirm tooltip is not being clipped by parent container

```xml
<!-- Correct minimal setup -->
<treemap:SfTreeMap ShowToolTip="True" 
                   PrimaryValuePath="Value">
    <treemap:SfTreeMap.LeafItemSettings>
        <treemap:TreeMapLeafItemSettings LabelPath="Name" />
    </treemap:SfTreeMap.LeafItemSettings>
</treemap:SfTreeMap>
```

### Issue 2: Custom Tooltip Not Showing Data

**Symptoms:** Tooltip appears but shows blank or wrong data

**Solutions:**
1. Ensure binding uses `{Binding Data.PropertyName}` format
2. Verify property names match your data model (case-sensitive)
3. Check for binding errors in output window
4. Test with default tooltip first to verify data is available

```xml
<!-- Correct binding -->
<Label Text="{Binding Data.Name}" />

<!-- Wrong: Missing 'Data.' prefix -->
<Label Text="{Binding Name}" />
```

### Issue 3: Tooltip Layout Issues

**Symptoms:** Tooltip cut off, too large, or poorly formatted

**Solutions:**
1. Set explicit `WidthRequest` on Frame if too wide
2. Use `MaximumWidthRequest` to prevent overflow
3. Adjust padding and spacing for better fit
4. Test on different screen sizes

```xml
<Frame Padding="12" 
       MaximumWidthRequest="300"
       CornerRadius="8">
    <VerticalStackLayout Spacing="6">
        <!-- Content -->
    </VerticalStackLayout>
</Frame>
```

### Issue 4: StringFormat Not Working

**Symptoms:** Numbers/dates not formatting correctly

**Solutions:**
1. Use correct format specifier syntax
2. Ensure property is correct data type (not string)
3. Check for XAML syntax errors (quotes, braces)

```xml
<!-- Correct -->
<Label Text="{Binding Data.Value, StringFormat='{0:N0}'}" />

<!-- Wrong: Missing curly braces around format -->
<Label Text="{Binding Data.Value, StringFormat='0:N0'}" />
```

### Issue 5: Tooltip Appears Behind Other Elements

**Symptoms:** Tooltip hidden or partially obscured

**Solutions:**
1. Tooltips should automatically appear on top (z-index)
2. Check parent container clipping settings
3. Ensure TreeMap has adequate space in layout

### Issue 6: Tooltip Disappears Too Quickly

**Symptoms:** Tooltip flickers or vanishes immediately

**Solutions:**
- This is controlled by the TreeMap's internal tooltip timing
- Ensure mouse/touch isn't leaving the leaf item boundary
- On touch devices, tap and hold to keep tooltip visible

## Related Topics

- [Leaf Item Customization](leaf-item-customization.md) - Styling leaf items
- [Selection and Interaction](selection-interaction.md) - User interaction features
- [Getting Started](getting-started.md) - Basic TreeMap setup

## Summary

- Enable tooltips with `ShowToolTip="True"`
- Default tooltip shows label and value automatically
- Create custom tooltips using `ToolTipTemplate` with `DataTemplate`
- Access data properties via `{Binding Data.PropertyName}` in templates
- Use `StringFormat` to format numbers, currency, percentages, and dates
- Style tooltips with Frame, colors, fonts, padding, and shadows
- Display multiple properties, images, and conditional formatting
- Tooltips enhance usability without cluttering the TreeMap visualization
- Always test tooltip visibility and data binding with output window
- Use calculated properties in model for conditional content/colors
