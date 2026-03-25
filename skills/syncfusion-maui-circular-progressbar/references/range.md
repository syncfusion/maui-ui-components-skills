# Range Configuration in Circular ProgressBar

Define custom progress ranges for the Syncfusion .NET MAUI Circular ProgressBar to work with different value scales beyond the default 0-100 percentage.

## Overview

The range represents the entire span of the circular progress bar and is defined using:
- **Minimum**: Starting value of the range
- **Maximum**: Ending value of the range
- **Progress**: Current value within the range

**Default Range**: 0 to 100 (percentage)

## Default Range (0-100)

By default, progress values are specified between 0 and 100, representing percentages.

**XAML:**
```xml
<progressBar:SfCircularProgressBar Progress="75" />
```

**C#:**
```csharp
SfCircularProgressBar circularProgressBar = new SfCircularProgressBar 
{ 
    Progress = 75 // 75%
};
```

## Custom Range

You can define any custom range using the `Minimum` and `Maximum` properties.

### Minimum and Maximum Properties

- **Minimum**: Lower bound of the progress range
- **Maximum**: Upper bound of the progress range
- **Progress**: Must be between Minimum and Maximum

## Factor-Based Range (0-1)

Common for decimal progress values.

**XAML:**
```xml
<progressBar:SfCircularProgressBar Minimum="0" 
                                   Maximum="1" 
                                   Progress="0.5" />
```

**C#:**
```csharp
SfCircularProgressBar circularProgressBar = new SfCircularProgressBar
{
    Minimum = 0,
    Maximum = 1,
    Progress = 0.5 // 50% completion
};
```

### Converting Between Percentages and Factors

```csharp
// Convert percentage to factor
double percentage = 75;
double factor = percentage / 100.0; // 0.75

// Convert factor to percentage
double factorValue = 0.75;
double percent = factorValue * 100; // 75
```

## Custom Numeric Ranges

### Example 1: Temperature Range

Display temperature progress from 0°C to 100°C.

**XAML:**
```xml
<progressBar:SfCircularProgressBar Minimum="0" 
                                   Maximum="100" 
                                   Progress="72">
    <progressBar:SfCircularProgressBar.Content>
        <StackLayout>
            <Label Text="72°C" 
                   FontSize="24" 
                   FontAttributes="Bold"
                   HorizontalTextAlignment="Center" />
            <Label Text="Temperature" 
                   FontSize="12"
                   HorizontalTextAlignment="Center" />
        </StackLayout>
    </progressBar:SfCircularProgressBar.Content>
</progressBar:SfCircularProgressBar>
```

### Example 2: Score Range

Display game score from 0 to 1000.

**XAML:**
```xml
<progressBar:SfCircularProgressBar Minimum="0" 
                                   Maximum="1000" 
                                   Progress="750">
    <progressBar:SfCircularProgressBar.Content>
        <Label Text="750/1000" 
               FontSize="20"
               HorizontalTextAlignment="Center" />
    </progressBar:SfCircularProgressBar.Content>
</progressBar:SfCircularProgressBar>
```

**C#:**
```csharp
SfCircularProgressBar scoreProgressBar = new SfCircularProgressBar
{
    Minimum = 0,
    Maximum = 1000,
    Progress = 750
};
```

### Example 3: File Size Progress

Track file upload progress in megabytes.

```csharp
public partial class UploadPage : ContentPage
{
    private SfCircularProgressBar uploadProgressBar;
    private double totalFileSizeMB = 250; // 250 MB file
    
    public UploadPage()
    {
        InitializeComponent();
        
        uploadProgressBar = new SfCircularProgressBar
        {
            Minimum = 0,
            Maximum = totalFileSizeMB,
            Progress = 0
        };
        
        Content = uploadProgressBar;
    }
}
```

## Negative Ranges

You can use negative values in your range.

### Example: Temperature Range (Negative to Positive)

```xml
<progressBar:SfCircularProgressBar Minimum="-20" 
                                   Maximum="40" 
                                   Progress="15">
    <progressBar:SfCircularProgressBar.Content>
        <Label Text="15°C" 
               FontSize="20"
               HorizontalTextAlignment="Center" />
    </progressBar:SfCircularProgressBar.Content>
</progressBar:SfCircularProgressBar>
```

**C#:**
```csharp
SfCircularProgressBar temperatureBar = new SfCircularProgressBar
{
    Minimum = -20,  // -20°C
    Maximum = 40,   // 40°C
    Progress = 15   // Current: 15°C
};
```

## Time-Based Ranges

### Example 1: Hour Range (0-24)

```xml
<progressBar:SfCircularProgressBar Minimum="0" 
                                   Maximum="24" 
                                   Progress="14.5"
                                   StartAngle="270"
                                   EndAngle="270">
    <progressBar:SfCircularProgressBar.Content>
        <StackLayout>
            <Label Text="14:30" 
                   FontSize="24"
                   FontAttributes="Bold"
                   HorizontalTextAlignment="Center" />
            <Label Text="Hours" 
                   FontSize="12"
                   HorizontalTextAlignment="Center" />
        </StackLayout>
    </progressBar:SfCircularProgressBar.Content>
</progressBar:SfCircularProgressBar>
```

### Example 2: Minute Range (0-60)

```csharp
SfCircularProgressBar minuteProgressBar = new SfCircularProgressBar
{
    Minimum = 0,
    Maximum = 60,
    Progress = 45, // 45 minutes
    StartAngle = 270, // Start at top
    EndAngle = 270    // Full circle
};
```

## Calculating Progress Percentage

When using custom ranges, you may need to calculate the equivalent percentage.

### Helper Methods

```csharp
public static class ProgressCalculator
{
    /// <summary>
    /// Calculate percentage from custom range
    /// </summary>
    public static double GetPercentage(double current, double min, double max)
    {
        if (max == min) return 0;
        return ((current - min) / (max - min)) * 100;
    }
    
    /// <summary>
    /// Calculate progress value from percentage
    /// </summary>
    public static double GetProgressFromPercentage(double percentage, double min, double max)
    {
        return min + ((percentage / 100.0) * (max - min));
    }
    
    /// <summary>
    /// Check if value is within range
    /// </summary>
    public static bool IsInRange(double value, double min, double max)
    {
        return value >= min && value <= max;
    }
}

// Usage examples
double percentage = ProgressCalculator.GetPercentage(15, -20, 40); // 58.33%
double progress = ProgressCalculator.GetProgressFromPercentage(75, 0, 1000); // 750
bool isValid = ProgressCalculator.IsInRange(50, 0, 100); // true
```

## Complete Examples

### Example 1: Storage Usage Indicator

```xml
<StackLayout Padding="20" Spacing="20">
    <Label Text="Storage Usage" 
           FontSize="18" 
           FontAttributes="Bold"
           HorizontalTextAlignment="Center" />
    
    <progressBar:SfCircularProgressBar x:Name="storageProgressBar"
                                       Minimum="0"
                                       Maximum="512"
                                       Progress="387"
                                       ProgressFill="#FFFF9800"
                                       TrackFill="#33FF9800">
        <progressBar:SfCircularProgressBar.Content>
            <StackLayout Spacing="5">
                <Label Text="387 GB" 
                       FontSize="22"
                       FontAttributes="Bold"
                       HorizontalTextAlignment="Center" />
                <Label Text="of 512 GB" 
                       FontSize="14"
                       TextColor="Gray"
                       HorizontalTextAlignment="Center" />
            </StackLayout>
        </progressBar:SfCircularProgressBar.Content>
    </progressBar:SfCircularProgressBar>
</StackLayout>
```

### Example 2: Battery Level (Factor-Based)

```csharp
public class BatteryPage : ContentPage
{
    private SfCircularProgressBar batteryProgressBar;
    private Label batteryLabel;

    public BatteryPage()
    {
        InitializeComponent();

        batteryProgressBar = new SfCircularProgressBar
        {
            Minimum = 0,
            Maximum = 1,
            Progress = 0.67, // 67% battery
            TrackThickness = 10,
            ProgressThickness = 10,
            WidthRequest = 150,
            HeightRequest = 150,
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.Center
        };

        batteryLabel = new Label
        {
            Text = "Battery: 67%",
            HorizontalOptions = LayoutOptions.Center,
            Margin = new Thickness(0, 20, 0, 0),
            FontSize = 18,
            FontAttributes = FontAttributes.Bold
        };

        var updateButton = new Button
        {
            Text = "Update Battery",
            HorizontalOptions = LayoutOptions.Center
        };
        updateButton.Clicked += (s, e) =>
        {
            // Simulate new battery level
            var random = new Random();
            double newLevel = random.NextDouble(); // between 0 and 1
            batteryProgressBar.Progress = newLevel;
            UpdateBatteryDisplay();
        };

        Content = new StackLayout
        {
            Padding = 40,
            Spacing = 20,
            Children = { batteryProgressBar, batteryLabel, updateButton }
        };

        UpdateBatteryDisplay();
    }

    private void UpdateBatteryDisplay()
    {
        double percentage = batteryProgressBar.Progress * 100;
        batteryLabel.Text = $"Battery: {percentage:F0}%";

        if (percentage < 20)
        {
            batteryProgressBar.ProgressFill = new SolidColorBrush(Colors.Red);
            batteryLabel.TextColor = Colors.Red;
        }
        else if (percentage < 50)
        {
            batteryProgressBar.ProgressFill = new SolidColorBrush(Colors.Orange);
            batteryLabel.TextColor = Colors.Orange;
        }
        else
        {
            batteryProgressBar.ProgressFill = new SolidColorBrush(Colors.Green);
            batteryLabel.TextColor = Colors.Green;
        }
    }
}

```

## Best Practices

### Range Selection
1. **Use 0-100**: Default for percentages, most intuitive
2. **Use 0-1**: Factor-based calculations, decimal progress
3. **Use natural units**: Match your data's natural scale (MB, °C, scores)
4. **Avoid large ranges**: Very large numbers may cause precision issues

### Display Considerations
1. **Show actual values**: Display custom range values in center content
2. **Provide context**: Include units (MB, °C, points)
3. **Calculate percentages**: Show percentage alongside custom values
4. **Update consistently**: Ensure range matches data source

### Validation
1. **Clamp values**: Ensure progress stays within min/max
2. **Handle edge cases**: Check for equal min/max
3. **Validate input**: Confirm current value is within range
4. **Test boundaries**: Verify behavior at minimum and maximum

### Performance
1. **Integer ranges**: Use integers when possible for better performance
2. **Reasonable precision**: Avoid excessive decimal places
3. **Update frequency**: Consider range scale for update intervals

## Summary

Custom ranges allow the circular progress bar to work with any value scale:
- **Default (0-100)**: Standard percentage-based progress
- **Factor (0-1)**: Decimal-based calculations
- **Custom numeric**: Any range (scores, sizes, temperatures)
- **Negative values**: Supported for ranges like temperature
- **Time-based**: Hours, minutes, seconds

Choose the range that best matches your data's natural scale for intuitive, meaningful progress visualization.