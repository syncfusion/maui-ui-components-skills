# Character Display Types in .NET MAUI DigitalGauge

This guide covers how to display different types of content in the DigitalGauge: numbers, alphabetic characters, and special characters. The type of content you can display depends on the CharacterType (segment type) you choose.

## Overview

The DigitalGauge can display three categories of content:
- **Numbers**: 0-9 and related symbols (decimal points, commas, etc.)
- **Alphabets**: Uppercase letters A-Z
- **Special Characters**: @, #, $, %, *, &, etc. (requires dot matrix)

Use the `Text` property to set what content to display.

## Displaying Numbers

All segment types support numeric display (0-9). Numbers are the most universally supported character type across all display formats.

### Basic Number Display

```xml
<gauge:SfDigitalGauge Text="12345" />
```

```csharp
SfDigitalGauge digitalGauge = new SfDigitalGauge();
digitalGauge.Text = "12345";
```

### Number Formats and Patterns

#### Simple Numbers

```xml
<!-- Integer display -->
<gauge:SfDigitalGauge Text="987654321" 
                      CharacterType="SevenSegment" />

<!-- With leading zeros -->
<gauge:SfDigitalGauge Text="00123" 
                      CharacterType="SevenSegment" />
```

#### Numbers with Separators

```xml
<!-- Time format -->
<gauge:SfDigitalGauge Text="12:30:45" 
                      CharacterType="SevenSegment"
                      CharacterHeight="90"
                      CharacterWidth="60" />

<!-- Date format -->
<gauge:SfDigitalGauge Text="03-19-2026" 
                      CharacterType="SevenSegment" />

<!-- Grouped numbers -->
<gauge:SfDigitalGauge Text="12 34 56" 
                      CharacterType="SevenSegment" />
```

#### Decimal Numbers

```xml
<!-- Price display -->
<gauge:SfDigitalGauge Text="99.99" 
                      CharacterType="SevenSegment"
                      CharacterStroke="Green" />

<!-- Temperature -->
<gauge:SfDigitalGauge Text="72.5" 
                      CharacterType="SevenSegment"
                      CharacterHeight="80"
                      CharacterWidth="50" />
```

### Recommended Segment Type for Numbers

**Seven-Segment** is optimal for numeric-only displays:
- Most efficient rendering
- Classic digital display appearance
- Best for clocks, counters, meters

```xml
<gauge:SfDigitalGauge Text="12345" 
                      CharacterType="SevenSegment"
                      CharacterHeight="80"
                      CharacterWidth="50"
                      CharacterStroke="Red"
                      BackgroundColor="Black" />
```

### Complete Numeric Display Example

```csharp
public class NumericDisplayPage : ContentPage
{
    public NumericDisplayPage()
    {
        var layout = new VerticalStackLayout
        {
            Padding = new Thickness(20),
            Spacing = 20
        };
        
        // Counter
        layout.Add(CreateLabel("Counter"));
        layout.Add(new SfDigitalGauge
        {
            Text = "00987",
            CharacterType = DigitalGaugeCharacterType.SevenSegment,
            CharacterHeight = 80,
            CharacterWidth = 50,
            CharacterStroke = Colors.Lime
        });
        
        // Clock
        layout.Add(CreateLabel("Digital Clock"));
        layout.Add(new SfDigitalGauge
        {
            Text = DateTime.Now.ToString("HH:mm:ss"),
            CharacterType = DigitalGaugeCharacterType.SevenSegment,
            CharacterHeight = 100,
            CharacterWidth = 60,
            CharacterStroke = Colors.Red,
            StrokeWidth = 3
        });
        
        // Temperature
        layout.Add(CreateLabel("Temperature"));
        layout.Add(new SfDigitalGauge
        {
            Text = "72.5",
            CharacterType = DigitalGaugeCharacterType.SevenSegment,
            CharacterHeight = 90,
            CharacterWidth = 55,
            CharacterStroke = Colors.Orange
        });
        
        this.Content = layout;
    }
    
    private Label CreateLabel(string text)
    {
        return new Label
        {
            Text = text,
            FontSize = 18,
            Margin = new Thickness(0, 10, 0, 0)
        };
    }
}
```

## Displaying Alphabets

Alphabetic characters require 14-segment or 16-segment displays for clear rendering. Seven-segment can display some letters but with limited clarity.

### Segment Type Requirements

| Segment Type | Alphabet Support | Clarity |
|--------------|------------------|---------|
| Seven-Segment | Limited (A, C, E, F, H, L, P, U) | Low |
| Fourteen-Segment | Full uppercase | Good |
| Sixteen-Segment | Full uppercase | Excellent |
| Dot Matrix | Full (upper & lower) | Good |

### Basic Alphabet Display

#### With Sixteen-Segment (Recommended)

```xml
<gauge:SfDigitalGauge Text="SYNCFUSION" 
                      CharacterType="SixteenSegment" />
```

```csharp
SfDigitalGauge digitalGauge = new SfDigitalGauge();
digitalGauge.Text = "SYNCFUSION";
digitalGauge.CharacterType = DigitalGaugeCharacterType.SixteenSegment;
```

#### With Fourteen-Segment

```xml
<gauge:SfDigitalGauge Text="HELLO WORLD" 
                      CharacterType="FourteenSegment"
                      CharacterHeight="80"
                      CharacterWidth="60" />
```

### Alphanumeric Content

Mixing letters and numbers is common for status displays, product codes, and labels.

```xml
<!-- Product code -->
<gauge:SfDigitalGauge Text="ABC123XYZ" 
                      CharacterType="SixteenSegment"
                      CharacterHeight="70"
                      CharacterWidth="55" />

<!-- Status message -->
<gauge:SfDigitalGauge Text="STATUS OK" 
                      CharacterType="FourteenSegment"
                      CharacterHeight="80"
                      CharacterWidth="60"
                      CharacterStroke="Green" />

<!-- License plate -->
<gauge:SfDigitalGauge Text="ABC 1234" 
                      CharacterType="SixteenSegment"
                      CharacterHeight="90"
                      CharacterWidth="70" />
```

### Text Display Patterns

#### Short Messages

```xml
<VerticalStackLayout Spacing="15">
    
    <!-- Welcome message -->
    <gauge:SfDigitalGauge Text="WELCOME" 
                          CharacterType="SixteenSegment"
                          CharacterHeight="90"
                          CharacterWidth="70"
                          CharacterStroke="Blue" />
    
    <!-- Error message -->
    <gauge:SfDigitalGauge Text="ERROR" 
                          CharacterType="FourteenSegment"
                          CharacterHeight="90"
                          CharacterWidth="70"
                          CharacterStroke="Red"
                          StrokeWidth="4" />
    
    <!-- Success message -->
    <gauge:SfDigitalGauge Text="SUCCESS" 
                          CharacterType="SixteenSegment"
                          CharacterHeight="80"
                          CharacterWidth="60"
                          CharacterStroke="Green" />
    
</VerticalStackLayout>
```

#### Information Display

```csharp
public class InfoDisplay : ContentPage
{
    private SfDigitalGauge infoGauge;
    
    public InfoDisplay()
    {
        infoGauge = new SfDigitalGauge
        {
            CharacterType = DigitalGaugeCharacterType.SixteenSegment,
            CharacterHeight = 100,
            CharacterWidth = 75,
            CharacterSpacing = 8,
            CharacterStroke = Colors.Cyan,
            StrokeWidth = 3,
            BackgroundColor = Colors.Black
        };
        
        ShowMessage("LOADING");
        
        this.Content = infoGauge;
    }
    
    public void ShowMessage(string message)
    {
        infoGauge.Text = message.ToUpper(); // Ensure uppercase
    }
    
    public void ShowStatus(string status, Color color)
    {
        infoGauge.Text = status.ToUpper();
        infoGauge.CharacterStroke = color;
    }
}
```

### Best Practices for Text Display

1. **Always use uppercase**: Segment displays work best with uppercase letters
2. **Keep messages short**: 5-12 characters display best on most screens
3. **Use appropriate segment type**: 16-segment for maximum clarity
4. **Add spacing**: Use CharacterSpacing for better readability
5. **Consider contrast**: Choose colors that stand out against the background

### Complete Alphabet Display Example

```xml
<ContentPage xmlns:gauge="clr-namespace:Syncfusion.Maui.Gauges;assembly=Syncfusion.Maui.Gauges">
    
    <VerticalStackLayout Padding="20" Spacing="20">
        
        <Label Text="Text Display Examples" FontSize="24" />
        
        <!-- Brand name -->
        <gauge:SfDigitalGauge Text="SYNCFUSION" 
                              CharacterType="SixteenSegment"
                              CharacterHeight="100"
                              CharacterWidth="75"
                              CharacterSpacing="5"
                              CharacterStroke="Blue"
                              StrokeWidth="3" />
        
        <!-- Status indicator -->
        <gauge:SfDigitalGauge Text="ONLINE" 
                              CharacterType="FourteenSegment"
                              CharacterHeight="80"
                              CharacterWidth="60"
                              CharacterStroke="LimeGreen" />
        
        <!-- Product label -->
        <gauge:SfDigitalGauge Text="MODEL X" 
                              CharacterType="SixteenSegment"
                              CharacterHeight="70"
                              CharacterWidth="55"
                              CharacterStroke="Orange" />
        
    </VerticalStackLayout>
    
</ContentPage>
```

## Displaying Special Characters

Special characters (@, #, $, %, *, &, etc.) require the **EightCrossEightDotMatrix** character type. Segment-based displays cannot render most special characters.

### Requirements

**Must use:** `CharacterType="EightCrossEightDotMatrix"`

### Basic Special Character Display

```xml
<gauge:SfDigitalGauge Text="@ # $ % *" 
                      CharacterType="EightCrossEightDotMatrix" />
```

```csharp
SfDigitalGauge digitalGauge = new SfDigitalGauge();
digitalGauge.Text = "@ # $ % *";
digitalGauge.CharacterType = DigitalGaugeCharacterType.EightCrossEightDotMatrix;
```

### Common Special Character Patterns

#### Currency Symbols

```xml
<!-- Price with dollar sign -->
<gauge:SfDigitalGauge Text="$99.99" 
                      CharacterType="EightCrossEightDotMatrix"
                      CharacterHeight="80"
                      CharacterWidth="70"
                      CharacterStroke="Green" />

<!-- Euro price -->
<gauge:SfDigitalGauge Text="EUR 49.99" 
                      CharacterType="EightCrossEightDotMatrix"
                      CharacterHeight="70"
                      CharacterWidth="60" />
```

#### Email Addresses

```xml
<gauge:SfDigitalGauge Text="user@email.com" 
                      CharacterType="EightCrossEightDotMatrix"
                      CharacterHeight="60"
                      CharacterWidth="55"
                      CharacterSpacing="5" />
```

#### Mathematical Expressions

```xml
<gauge:SfDigitalGauge Text="50% OFF" 
                      CharacterType="EightCrossEightDotMatrix"
                      CharacterHeight="80"
                      CharacterWidth="70"
                      CharacterStroke="Red" />

<gauge:SfDigitalGauge Text="2 + 2 = 4" 
                      CharacterType="EightCrossEightDotMatrix"
                      CharacterHeight="70"
                      CharacterWidth="60" />
```

#### Social Media Handles

```xml
<gauge:SfDigitalGauge Text="@username" 
                      CharacterType="EightCrossEightDotMatrix"
                      CharacterHeight="70"
                      CharacterWidth="60"
                      CharacterStroke="Cyan" />

<gauge:SfDigitalGauge Text="#trending" 
                      CharacterType="EightCrossEightDotMatrix"
                      CharacterHeight="70"
                      CharacterWidth="60"
                      CharacterStroke="Blue" />
```

### Supported Special Characters

The dot matrix display supports a wide range of special characters:
- **Punctuation**: . , ; : ! ? ' "
- **Math**: + - * / = % < >
- **Currency**: $ € £ ¥
- **Symbols**: @ # & ( ) [ ] { }
- **Other**: ~ ` ^ _ | \ /

### Complete Special Character Example

```csharp
public class SpecialCharPage : ContentPage
{
    public SpecialCharPage()
    {
        var layout = new VerticalStackLayout
        {
            Padding = new Thickness(20),
            Spacing = 20
        };
        
        layout.Add(new Label { Text = "Special Characters", FontSize = 24 });
        
        // Price display
        layout.Add(new SfDigitalGauge
        {
            Text = "PRICE: $49.99",
            CharacterType = DigitalGaugeCharacterType.EightCrossEightDotMatrix,
            CharacterHeight = 70,
            CharacterWidth = 60,
            CharacterSpacing = 5,
            CharacterStroke = Colors.Yellow
        });
        
        // Email
        layout.Add(new SfDigitalGauge
        {
            Text = "contact@site.com",
            CharacterType = DigitalGaugeCharacterType.EightCrossEightDotMatrix,
            CharacterHeight = 60,
            CharacterWidth = 55,
            CharacterSpacing = 3,
            CharacterStroke = Colors.Cyan
        });
        
        // Discount
        layout.Add(new SfDigitalGauge
        {
            Text = "50% OFF!",
            CharacterType = DigitalGaugeCharacterType.EightCrossEightDotMatrix,
            CharacterHeight = 80,
            CharacterWidth = 70,
            CharacterSpacing = 6,
            CharacterStroke = Colors.Red,
            StrokeWidth = 3
        });
        
        this.Content = layout;
    }
}
```

## Text Property Usage

The `Text` property is the primary way to set display content.

### Setting Text in XAML

```xml
<gauge:SfDigitalGauge Text="HELLO" />
```

### Setting Text in C#

```csharp
// Direct assignment
digitalGauge.Text = "HELLO";

// From variable
string message = "STATUS OK";
digitalGauge.Text = message;

// Dynamic content
digitalGauge.Text = DateTime.Now.ToString("HH:mm:ss");
```

### Dynamic Text Updates

```csharp
public class LiveClockPage : ContentPage
{
    private SfDigitalGauge clockGauge;
    
    public LiveClockPage()
    {
        clockGauge = new SfDigitalGauge
        {
            CharacterType = DigitalGaugeCharacterType.SevenSegment,
            CharacterHeight = 120,
            CharacterWidth = 75,
            CharacterStroke = Colors.Red,
            StrokeWidth = 4,
            BackgroundColor = Colors.Black
        };
        
        this.Content = clockGauge;
        
        // Update every second
        Device.StartTimer(TimeSpan.FromSeconds(1), () =>
        {
            clockGauge.Text = DateTime.Now.ToString("HH:mm:ss");
            return true; // Continue timer
        });
    }
}
```

## Common Display Patterns

### Live Data Display

```csharp
public class SensorDisplay : ContentPage
{
    private SfDigitalGauge temperatureGauge;
    private SfDigitalGauge statusGauge;
    
    public SensorDisplay()
    {
        temperatureGauge = new SfDigitalGauge
        {
            CharacterType = DigitalGaugeCharacterType.SevenSegment,
            CharacterHeight = 100,
            CharacterWidth = 65,
            CharacterStroke = Colors.Orange
        };
        
        statusGauge = new SfDigitalGauge
        {
            CharacterType = DigitalGaugeCharacterType.SixteenSegment,
            CharacterHeight = 70,
            CharacterWidth = 55,
            CharacterStroke = Colors.Green
        };
        
        var layout = new VerticalStackLayout
        {
            Padding = new Thickness(20),
            Spacing = 20,
            Children =
            {
                new Label { Text = "Temperature", FontSize = 18 },
                temperatureGauge,
                new Label { Text = "Status", FontSize = 18 },
                statusGauge
            }
        };
        
        this.Content = layout;
        
        UpdateSensorData();
    }
    
    private void UpdateSensorData()
    {
        // Simulate sensor reading
        double temperature = 72.5;
        temperatureGauge.Text = $"{temperature:F1}";
        statusGauge.Text = "NORMAL";
    }
}
```

### Multi-Line Display Simulation

Since DigitalGauge displays single-line text, use multiple gauges for multi-line effects:

```xml
<VerticalStackLayout Spacing="10">
    <gauge:SfDigitalGauge Text="LINE ONE" 
                          CharacterType="SixteenSegment"
                          CharacterHeight="70"
                          CharacterWidth="55" />
    
    <gauge:SfDigitalGauge Text="LINE TWO" 
                          CharacterType="SixteenSegment"
                          CharacterHeight="70"
                          CharacterWidth="55" />
</VerticalStackLayout>
```

## Troubleshooting

### Characters Not Displaying

**Problem:** Some characters appear blank or incorrect.

**Solutions:**
1. Check if the character is supported by the current CharacterType
2. For special characters, use EightCrossEightDotMatrix
3. For clear letters, use SixteenSegment
4. Ensure text is uppercase (lowercase requires dot matrix)

### Text Too Long

**Problem:** Text gets cut off or doesn't fit.

**Solutions:**
1. Reduce CharacterWidth or CharacterSpacing
2. Shorten the text message
3. Use a ScrollView container
4. Split into multiple gauges

### Poor Readability

**Problem:** Characters are hard to read.

**Solutions:**
1. Increase CharacterHeight and CharacterWidth
2. Use appropriate CharacterType for content
3. Adjust CharacterStroke color for better contrast
4. Increase StrokeWidth for bolder segments
5. Set BackgroundColor for better visibility
