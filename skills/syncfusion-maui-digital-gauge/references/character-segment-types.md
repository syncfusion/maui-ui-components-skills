# Character Segment Types in .NET MAUI DigitalGauge

## Table of Contents
- [Overview](#overview)
- [Seven-Segment Display](#seven-segment-display)
- [Fourteen-Segment Display](#fourteen-segment-display)
- [Sixteen-Segment Display](#sixteen-segment-display)
- [Eight Cross Eight Dot Matrix Display](#eight-cross-eight-dot-matrix-display)
- [Comparison and Recommendations](#comparison-and-recommendations)
- [Choosing the Right Segment Type](#choosing-the-right-segment-type)
- [Switching Between Segment Types](#switching-between-segment-types)

## Overview

The DigitalGauge control supports four different character segment types, each optimized for different display requirements. The segment type determines how characters are rendered and which characters can be displayed clearly.

The four segment types are:
- **SevenSegment**: 7 line segments forming characters
- **FourteenSegment**: 14 line segments for better letter clarity
- **SixteenSegment**: 16 line segments for optimal letter rendering
- **EightCrossEightDotMatrix**: 8×8 grid of dots for maximum flexibility

Use the `CharacterType` property to set the segment type.

## Seven-Segment Display

The seven-segment display is the most common digital display format, consisting of 7 line segments arranged in a figure-eight pattern. It's ideal for numeric displays.

### Best For
- Numeric values (0-9)
- Limited uppercase letters (A, C, E, F, H, L, P, U, etc.)
- Simple displays where only numbers are needed
- Calculator-style displays
- Digital clocks showing only numbers

### Limitations
- Many letters are not clearly distinguishable
- No support for lowercase letters
- Limited special character support

### XAML Implementation

```xml
<gauge:SfDigitalGauge Text="12345" 
                      CharacterType="SevenSegment" />
```

### C# Implementation

```csharp
SfDigitalGauge digitalGauge = new SfDigitalGauge();
digitalGauge.Text = "12345";
digitalGauge.CharacterType = DigitalGaugeCharacterType.SevenSegment;
```

### Complete Example

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:gauge="clr-namespace:Syncfusion.Maui.Gauges;assembly=Syncfusion.Maui.Gauges">
    
    <VerticalStackLayout Padding="20" Spacing="20">
        
        <!-- Seven-segment numeric display -->
        <gauge:SfDigitalGauge Text="987654321" 
                              CharacterType="SevenSegment"
                              CharacterHeight="80"
                              CharacterWidth="50" />
        
        <!-- Seven-segment clock display -->
        <gauge:SfDigitalGauge Text="12:30:45" 
                              CharacterType="SevenSegment"
                              CharacterHeight="100"
                              CharacterWidth="60"
                              CharacterStroke="Red" />
        
        <!-- Seven-segment counter -->
        <gauge:SfDigitalGauge Text="00000" 
                              CharacterType="SevenSegment"
                              CharacterHeight="70"
                              CharacterWidth="45" />
        
    </VerticalStackLayout>
    
</ContentPage>
```

### Use Cases
- Digital clocks
- Numeric counters
- Temperature displays
- Speed displays
- Score counters
- Calculator displays

## Fourteen-Segment Display

The fourteen-segment display extends the seven-segment display with additional diagonal and vertical segments, providing better support for alphabetic characters.

### Best For
- Alphanumeric displays (numbers and letters)
- Status messages with text
- Product codes with mixed characters
- License plate displays
- Ticker displays with text

### Advantages Over Seven-Segment
- Clear display of all uppercase letters
- Better character differentiation
- More readable text messages

### XAML Implementation

```xml
<gauge:SfDigitalGauge Text="HELLO" 
                      CharacterType="FourteenSegment" />
```

### C# Implementation

```csharp
SfDigitalGauge digitalGauge = new SfDigitalGauge();
digitalGauge.Text = "HELLO";
digitalGauge.CharacterType = DigitalGaugeCharacterType.FourteenSegment;
```

### Complete Example

```xml
<ContentPage xmlns:gauge="clr-namespace:Syncfusion.Maui.Gauges;assembly=Syncfusion.Maui.Gauges">
    
    <VerticalStackLayout Padding="20" Spacing="20">
        
        <!-- Status message -->
        <gauge:SfDigitalGauge Text="STATUS OK" 
                              CharacterType="FourteenSegment"
                              CharacterHeight="80"
                              CharacterWidth="60"
                              CharacterStroke="Green" />
        
        <!-- Product code -->
        <gauge:SfDigitalGauge Text="ABC123XYZ" 
                              CharacterType="FourteenSegment"
                              CharacterHeight="70"
                              CharacterWidth="55" />
        
        <!-- Alert message -->
        <gauge:SfDigitalGauge Text="WARNING" 
                              CharacterType="FourteenSegment"
                              CharacterHeight="90"
                              CharacterWidth="70"
                              CharacterStroke="Orange"
                              StrokeWidth="3" />
        
    </VerticalStackLayout>
    
</ContentPage>
```

### C# Dynamic Example

```csharp
public class StatusDisplay : ContentPage
{
    private SfDigitalGauge statusGauge;
    
    public StatusDisplay()
    {
        statusGauge = new SfDigitalGauge
        {
            CharacterType = DigitalGaugeCharacterType.FourteenSegment,
            CharacterHeight = 90,
            CharacterWidth = 70,
            CharacterSpacing = 8,
            StrokeWidth = 3
        };
        
        UpdateStatus("READY");
        
        this.Content = statusGauge;
    }
    
    private void UpdateStatus(string status)
    {
        statusGauge.Text = status;
        
        // Change color based on status
        statusGauge.CharacterStroke = status switch
        {
            "READY" => Colors.Green,
            "ERROR" => Colors.Red,
            "WARNING" => Colors.Orange,
            _ => Colors.White
        };
    }
}
```

### Use Cases
- Status indicators
- Alphanumeric identifiers
- Short text messages
- Product information displays
- License plate recognition displays

## Sixteen-Segment Display

The sixteen-segment display provides the clearest rendering of alphabetic characters by using 16 individual segments. This is the best choice when text clarity is paramount.

### Best For
- Clear text display
- Mixed alphanumeric content
- Brand names and labels
- Informational text displays
- Any scenario requiring maximum text clarity

### Advantages
- Clearest letter representation
- Best readability for text
- All uppercase letters display clearly
- Good for longer text strings

### XAML Implementation

```xml
<gauge:SfDigitalGauge Text="SYNCFUSION" 
                      CharacterType="SixteenSegment" />
```

### C# Implementation

```csharp
SfDigitalGauge digitalGauge = new SfDigitalGauge();
digitalGauge.Text = "SYNCFUSION";
digitalGauge.CharacterType = DigitalGaugeCharacterType.SixteenSegment;
```

### Complete Example

```xml
<ContentPage xmlns:gauge="clr-namespace:Syncfusion.Maui.Gauges;assembly=Syncfusion.Maui.Gauges">
    
    <VerticalStackLayout Padding="20" Spacing="20">
        
        <!-- Company name -->
        <gauge:SfDigitalGauge Text="SYNCFUSION" 
                              CharacterType="SixteenSegment"
                              CharacterHeight="100"
                              CharacterWidth="75"
                              CharacterStroke="Blue"
                              StrokeWidth="3" />
        
        <!-- Information display -->
        <gauge:SfDigitalGauge Text="WELCOME USER" 
                              CharacterType="SixteenSegment"
                              CharacterHeight="80"
                              CharacterWidth="60"
                              CharacterSpacing="10" />
        
        <!-- Product label -->
        <gauge:SfDigitalGauge Text="MODEL X2024" 
                              CharacterType="SixteenSegment"
                              CharacterHeight="70"
                              CharacterWidth="55"
                              CharacterStroke="Cyan" />
        
    </VerticalStackLayout>
    
</ContentPage>
```

## Eight Cross Eight Dot Matrix Display

The 8×8 dot matrix display uses a grid of 64 dots to form characters. This provides maximum flexibility and supports special characters that segment-based displays cannot render.

### Best For
- Special characters (@, #, $, %, *, etc.)
- Lowercase letters (if needed)
- Custom symbols
- Icons and simple graphics
- Maximum character set support

### Advantages
- Supports all characters (numbers, letters, special chars)
- Can display lowercase letters
- Most flexible display type
- Good for custom characters

### Limitations
- Characters may appear less sharp than segment displays
- Requires more processing
- May look pixelated at small sizes

### XAML Implementation

```xml
<gauge:SfDigitalGauge Text="@ # $ % *" 
                      CharacterType="EightCrossEightDotMatrix" />
```

### C# Implementation

```csharp
SfDigitalGauge digitalGauge = new SfDigitalGauge();
digitalGauge.Text = "@ # $ % *";
digitalGauge.CharacterType = DigitalGaugeCharacterType.EightCrossEightDotMatrix;
```

### Complete Example

```xml
<ContentPage xmlns:gauge="clr-namespace:Syncfusion.Maui.Gauges;assembly=Syncfusion.Maui.Gauges">
    
    <VerticalStackLayout Padding="20" Spacing="20">
        
        <!-- Special characters -->
        <gauge:SfDigitalGauge Text="@ # $ % * &amp;" 
                              CharacterType="EightCrossEightDotMatrix"
                              CharacterHeight="80"
                              CharacterWidth="70"
                              CharacterSpacing="8" />
        
        <!-- Email display -->
        <gauge:SfDigitalGauge Text="user@email.com" 
                              CharacterType="EightCrossEightDotMatrix"
                              CharacterHeight="60"
                              CharacterWidth="55"
                              CharacterSpacing="5" />
        
        <!-- Mixed content -->
        <gauge:SfDigitalGauge Text="Price: $99.99" 
                              CharacterType="EightCrossEightDotMatrix"
                              CharacterHeight="70"
                              CharacterWidth="60"
                              CharacterStroke="Yellow" />
        
    </VerticalStackLayout>
    
</ContentPage>
```

### Use Cases
- Email addresses
- URLs
- Prices with currency symbols
- Mathematical expressions
- Social media handles
- Any content with special characters

## Comparison and Recommendations

| Segment Type | Segments | Numbers | Uppercase | Lowercase | Special Chars | Best Use Case |
|--------------|----------|---------|-----------|-----------|---------------|---------------|
| Seven | 7 | ✓✓✓ | Limited | ✗ | ✗ | Numeric displays, clocks |
| Fourteen | 14 | ✓✓✓ | ✓✓ | ✗ | ✗ | Alphanumeric, status messages |
| Sixteen | 16 | ✓✓✓ | ✓✓✓ | ✗ | ✗ | Clear text, labels |
| Dot Matrix | 64 dots | ✓✓ | ✓✓ | ✓✓ | ✓✓✓ | Special chars, flexible content |

### Performance Considerations
- Seven-segment: Fastest rendering
- Fourteen-segment: Fast rendering
- Sixteen-segment: Fast rendering
- Dot matrix: Slightly slower, more memory

## Choosing the Right Segment Type

### Choose Seven-Segment When:
- Displaying only numbers
- Building digital clocks or timers
- Creating counters or odometers
- Performance is critical
- Classic digital display look is desired

### Choose Fourteen-Segment When:
- Displaying alphanumeric codes
- Showing status messages
- Mixed number and letter content
- Good balance between clarity and compatibility

### Choose Sixteen-Segment When:
- Text clarity is most important
- Displaying brand names
- Longer text strings
- Professional appearance is needed
- Maximum readability for letters

### Choose Dot Matrix When:
- Special characters are required
- Displaying email addresses or URLs
- Flexible character support is needed
- Lowercase letters may be used
- Custom or unique characters needed

## Switching Between Segment Types

You can dynamically change the segment type at runtime:

### XAML with Picker

```xml
<VerticalStackLayout Padding="20" Spacing="20">
    
    <Picker x:Name="segmentPicker" 
            Title="Select Segment Type"
            SelectedIndexChanged="OnSegmentTypeChanged">
        <Picker.ItemsSource>
            <x:Array Type="{x:Type x:String}">
                <x:String>SevenSegment</x:String>
                <x:String>FourteenSegment</x:String>
                <x:String>SixteenSegment</x:String>
                <x:String>EightCrossEightDotMatrix</x:String>
            </x:Array>
        </Picker.ItemsSource>
    </Picker>
    
    <gauge:SfDigitalGauge x:Name="dynamicGauge" 
                          Text="HELLO 123"
                          CharacterHeight="90"
                          CharacterWidth="70" />
    
</VerticalStackLayout>
```

### C# Event Handler

```csharp
private void OnSegmentTypeChanged(object sender, EventArgs e)
{
    var picker = (Picker)sender;
    var selectedType = picker.SelectedItem.ToString();
    
    dynamicGauge.CharacterType = selectedType switch
    {
        "SevenSegment" => DigitalGaugeCharacterType.SevenSegment,
        "FourteenSegment" => DigitalGaugeCharacterType.FourteenSegment,
        "SixteenSegment" => DigitalGaugeCharacterType.SixteenSegment,
        "EightCrossEightDotMatrix" => DigitalGaugeCharacterType.EightCrossEightDotMatrix,
        _ => DigitalGaugeCharacterType.SevenSegment
    };
}
```

### Pure C# Implementation

```csharp
public class SegmentTypeSwitcher : ContentPage
{
    private SfDigitalGauge gauge;
    private Button sevenBtn, fourteenBtn, sixteenBtn, dotMatrixBtn;
    
    public SegmentTypeSwitcher()
    {
        gauge = new SfDigitalGauge
        {
            Text = "TEST 123",
            CharacterHeight = 100,
            CharacterWidth = 75,
            CharacterSpacing = 8,
            CharacterType = DigitalGaugeCharacterType.SevenSegment
        };
        
        sevenBtn = CreateButton("7-Segment", DigitalGaugeCharacterType.SevenSegment);
        fourteenBtn = CreateButton("14-Segment", DigitalGaugeCharacterType.FourteenSegment);
        sixteenBtn = CreateButton("16-Segment", DigitalGaugeCharacterType.SixteenSegment);
        dotMatrixBtn = CreateButton("Dot Matrix", DigitalGaugeCharacterType.EightCrossEightDotMatrix);
        
        var layout = new VerticalStackLayout
        {
            Padding = new Thickness(20),
            Spacing = 15
        };
        
        layout.Add(new Label { Text = "Select Segment Type", FontSize = 20 });
        layout.Add(gauge);
        layout.Add(new HorizontalStackLayout
        {
            Spacing = 10,
            Children = { sevenBtn, fourteenBtn, sixteenBtn, dotMatrixBtn }
        });
        
        this.Content = layout;
    }
    
    private Button CreateButton(string text, DigitalGaugeCharacterType type)
    {
        var button = new Button { Text = text };
        button.Clicked += (s, e) => gauge.CharacterType = type;
        return button;
    }
}
```
