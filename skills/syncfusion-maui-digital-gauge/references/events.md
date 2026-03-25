# Events in .NET MAUI DigitalGauge

This guide covers event handling in the DigitalGauge control, specifically the `TextChanged` event that fires when the displayed text is updated.

## Overview

The DigitalGauge control provides the `TextChanged` event, which is triggered whenever the `Text` property value changes. This event is useful for:
- Responding to text updates
- Validating displayed content
- Synchronizing with other UI elements
- Logging display changes
- Triggering animations or effects

## TextChanged Event

The `TextChanged` event fires when the `Text` property of the DigitalGauge is modified, either programmatically or through data binding.

### Event Signature

```csharp
public event EventHandler<DigitalGaugeTextChangedEventArgs> TextChanged;
```

### Event Arguments

The event provides `DigitalGaugeTextChangedEventArgs` which contains:
- **OldText**: The previous text value
- **NewText**: The current (new) text value

## XAML Implementation

### Basic Event Wiring

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:gauge="clr-namespace:Syncfusion.Maui.Gauges;assembly=Syncfusion.Maui.Gauges"
             x:Class="MyApp.MainPage">
    
    <gauge:SfDigitalGauge x:Name="digitalGauge"
                          Text="HELLO"
                          TextChanged="OnDigitalGaugeTextChanged"
                          CharacterType="SixteenSegment"
                          CharacterHeight="90"
                          CharacterWidth="70" />
    
</ContentPage>
```

### Code-Behind Event Handler

```csharp
using Syncfusion.Maui.Gauges;

namespace MyApp
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
        }
        
        private void OnDigitalGaugeTextChanged(object sender, DigitalGaugeTextChangedEventArgs e)
        {
            // Handle the event
            string oldValue = e.OldText;
            string newValue = e.NewText;
            
            Console.WriteLine($"Text changed from '{oldValue}' to '{newValue}'");
        }
    }
}
```

### Complete XAML Example

```xml
<ContentPage xmlns:gauge="clr-namespace:Syncfusion.Maui.Gauges;assembly=Syncfusion.Maui.Gauges">
    
    <VerticalStackLayout Padding="20" Spacing="20">
        
        <Label Text="Digital Gauge with TextChanged Event" 
               FontSize="20" />
        
        <gauge:SfDigitalGauge x:Name="statusGauge"
                              Text="READY"
                              TextChanged="OnStatusChanged"
                              CharacterType="SixteenSegment"
                              CharacterHeight="90"
                              CharacterWidth="70"
                              CharacterSpacing="8"
                              CharacterStroke="Green"
                              StrokeWidth="3" />
        
        <Label x:Name="statusLabel" 
               Text="Status: Ready"
               FontSize="16" />
        
        <Button Text="Set to RUNNING" 
                Clicked="OnSetRunning" />
        
        <Button Text="Set to COMPLETE" 
                Clicked="OnSetComplete" />
        
    </VerticalStackLayout>
    
</ContentPage>
```

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
    }
    
    private void OnStatusChanged(object sender, DigitalGaugeTextChangedEventArgs e)
    {
        statusLabel.Text = $"Status changed: {e.OldText} → {e.NewText}";
        
        // Update color based on new status
        var gauge = (SfDigitalGauge)sender;
        gauge.CharacterStroke = e.NewText switch
        {
            "READY" => Colors.Green,
            "RUNNING" => Colors.Blue,
            "COMPLETE" => Colors.Lime,
            _ => Colors.White
        };
    }
    
    private void OnSetRunning(object sender, EventArgs e)
    {
        statusGauge.Text = "RUNNING";
    }
    
    private void OnSetComplete(object sender, EventArgs e)
    {
        statusGauge.Text = "COMPLETE";
    }
}
```

## C# Implementation

### Programmatic Event Subscription

```csharp
using Syncfusion.Maui.Gauges;

public class EventDemoPage : ContentPage
{
    private SfDigitalGauge digitalGauge;
    private Label eventLog;
    
    public EventDemoPage()
    {
        // Create gauge
        digitalGauge = new SfDigitalGauge
        {
            Text = "12345",
            CharacterType = DigitalGaugeCharacterType.SevenSegment,
            CharacterHeight = 100,
            CharacterWidth = 65,
            CharacterStroke = Colors.Red
        };
        
        // Subscribe to event
        digitalGauge.TextChanged += OnTextChanged;
        
        // Create event log label
        eventLog = new Label
        {
            Text = "Event log will appear here",
            FontSize = 14
        };
        
        // Create layout
        var layout = new VerticalStackLayout
        {
            Padding = new Thickness(20),
            Spacing = 20,
            Children = { digitalGauge, eventLog }
        };
        
        this.Content = layout;
        
        // Trigger some text changes
        StartCounter();
    }
    
    private void OnTextChanged(object sender, DigitalGaugeTextChangedEventArgs e)
    {
        eventLog.Text = $"Text changed from '{e.OldText}' to '{e.NewText}'";
    }
    
    private void StartCounter()
    {
        int counter = 0;
        Device.StartTimer(TimeSpan.FromSeconds(1), () =>
        {
            counter++;
            digitalGauge.Text = counter.ToString("D5");
            return counter < 10; // Stop after 10 updates
        });
    }
    
    protected override void OnDisappearing()
    {
        base.OnDisappearing();
        
        // Unsubscribe from event
        digitalGauge.TextChanged -= OnTextChanged;
    }
}
```

### Lambda Expression Handler

```csharp
digitalGauge.TextChanged += (sender, e) =>
{
    Console.WriteLine($"Text: {e.OldText} → {e.NewText}");
};
```

### Multiple Event Handlers

You can subscribe multiple handlers to the same event:

```csharp
public class MultiHandlerPage : ContentPage
{
    private SfDigitalGauge gauge;
    
    public MultiHandlerPage()
    {
        gauge = new SfDigitalGauge
        {
            Text = "START",
            CharacterType = DigitalGaugeCharacterType.SixteenSegment,
            CharacterHeight = 90,
            CharacterWidth = 70
        };
        
        // Subscribe multiple handlers
        gauge.TextChanged += LogTextChange;
        gauge.TextChanged += UpdateColor;
        gauge.TextChanged += NotifyUser;
        
        this.Content = gauge;
    }
    
    private void LogTextChange(object sender, DigitalGaugeTextChangedEventArgs e)
    {
        Console.WriteLine($"[LOG] {e.OldText} → {e.NewText}");
    }
    
    private void UpdateColor(object sender, DigitalGaugeTextChangedEventArgs e)
    {
        var gauge = (SfDigitalGauge)sender;
        gauge.CharacterStroke = e.NewText.Length > 5 ? Colors.Red : Colors.Green;
    }
    
    private void NotifyUser(object sender, DigitalGaugeTextChangedEventArgs e)
    {
        DisplayAlert("Text Changed", $"New value: {e.NewText}", "OK");
    }
}
```

## Event Patterns and Use Cases

### Validation

Validate the text and revert if invalid:

```csharp
private void OnTextChanged(object sender, DigitalGaugeTextChangedEventArgs e)
{
    var gauge = (SfDigitalGauge)sender;
    
    // Only allow numeric values
    if (!IsNumeric(e.NewText))
    {
        // Revert to old value
        gauge.TextChanged -= OnTextChanged; // Temporarily unsubscribe
        gauge.Text = e.OldText;
        gauge.TextChanged += OnTextChanged; // Resubscribe
        
        DisplayAlert("Invalid Input", "Only numeric values allowed", "OK");
    }
}

private bool IsNumeric(string text)
{
    return text.All(c => char.IsDigit(c) || c == '.' || c == ':' || c == ' ');
}
```

### Synchronization

Sync the gauge with other UI elements:

```csharp
public class SyncPage : ContentPage
{
    private SfDigitalGauge gauge;
    private Entry textEntry;
    private Label displayLabel;
    
    public SyncPage()
    {
        gauge = new SfDigitalGauge
        {
            Text = "SYNC",
            CharacterType = DigitalGaugeCharacterType.SixteenSegment,
            CharacterHeight = 90,
            CharacterWidth = 70
        };
        
        textEntry = new Entry
        {
            Placeholder = "Enter text",
            MaxLength = 10
        };
        
        displayLabel = new Label
        {
            Text = "Current: SYNC",
            FontSize = 16
        };
        
        // Sync entry to gauge
        textEntry.TextChanged += (s, e) =>
        {
            gauge.Text = e.NewValue?.ToUpper() ?? "";
        };
        
        // Sync gauge to label
        gauge.TextChanged += (s, e) =>
        {
            displayLabel.Text = $"Current: {e.NewText}";
        };
        
        var layout = new VerticalStackLayout
        {
            Padding = new Thickness(20),
            Spacing = 20,
            Children = { gauge, textEntry, displayLabel }
        };
        
        this.Content = layout;
    }
}
```

### Logging and Analytics

Track text changes for analytics:

```csharp
public class AnalyticsPage : ContentPage
{
    private SfDigitalGauge gauge;
    private List<string> changeHistory = new List<string>();
    
    public AnalyticsPage()
    {
        gauge = new SfDigitalGauge
        {
            Text = "START",
            CharacterType = DigitalGaugeCharacterType.FourteenSegment,
            CharacterHeight = 90,
            CharacterWidth = 70
        };
        
        gauge.TextChanged += LogChange;
        
        this.Content = gauge;
    }
    
    private void LogChange(object sender, DigitalGaugeTextChangedEventArgs e)
    {
        string logEntry = $"{DateTime.Now:HH:mm:ss} - Changed from '{e.OldText}' to '{e.NewText}'";
        changeHistory.Add(logEntry);
        
        Console.WriteLine(logEntry);
        
        // Send to analytics service
        AnalyticsService.TrackEvent("DigitalGauge_TextChanged", new Dictionary<string, string>
        {
            { "OldValue", e.OldText },
            { "NewValue", e.NewText },
            { "ChangeCount", changeHistory.Count.ToString() }
        });
    }
}
```

### Animation Triggers

Trigger animations when text changes:

```csharp
public class AnimatedGaugePage : ContentPage
{
    private SfDigitalGauge gauge;
    
    public AnimatedGaugePage()
    {
        gauge = new SfDigitalGauge
        {
            Text = "READY",
            CharacterType = DigitalGaugeCharacterType.SixteenSegment,
            CharacterHeight = 90,
            CharacterWidth = 70,
            CharacterStroke = Colors.Green
        };
        
        gauge.TextChanged += OnTextChangedAnimate;
        
        this.Content = gauge;
    }
    
    private async void OnTextChangedAnimate(object sender, DigitalGaugeTextChangedEventArgs e)
    {
        var gauge = (SfDigitalGauge)sender;
        
        // Flash animation
        await gauge.ScaleTo(1.2, 100);
        await gauge.ScaleTo(1.0, 100);
        
        // Fade effect
        await gauge.FadeTo(0.5, 50);
        await gauge.FadeTo(1.0, 50);
    }
}
```

### Conditional Styling

Change appearance based on text content:

```csharp
private void OnTextChanged(object sender, DigitalGaugeTextChangedEventArgs e)
{
    var gauge = (SfDigitalGauge)sender;
    
    // Style based on content
    if (e.NewText.Contains("ERROR"))
    {
        gauge.CharacterStroke = Colors.Red;
        gauge.StrokeWidth = 4;
    }
    else if (e.NewText.Contains("WARNING"))
    {
        gauge.CharacterStroke = Colors.Orange;
        gauge.StrokeWidth = 3;
    }
    else if (e.NewText.Contains("SUCCESS") || e.NewText.Contains("OK"))
    {
        gauge.CharacterStroke = Colors.Green;
        gauge.StrokeWidth = 2;
    }
    else
    {
        gauge.CharacterStroke = Colors.White;
        gauge.StrokeWidth = 2;
    }
}
```

## Real-World Example: Status Monitor

Complete example showing practical event usage:

```csharp
public class StatusMonitorPage : ContentPage
{
    private SfDigitalGauge statusGauge;
    private Label timestampLabel;
    private Label historyLabel;
    private List<StatusChange> statusHistory = new List<StatusChange>();
    
    public StatusMonitorPage()
    {
        statusGauge = new SfDigitalGauge
        {
            Text = "IDLE",
            CharacterType = DigitalGaugeCharacterType.SixteenSegment,
            CharacterHeight = 100,
            CharacterWidth = 75,
            CharacterSpacing = 8,
            CharacterStroke = Colors.Gray,
            StrokeWidth = 3,
            BackgroundColor = Colors.Black
        };
        
        statusGauge.TextChanged += OnStatusChanged;
        
        timestampLabel = new Label
        {
            Text = "Last updated: Never",
            FontSize = 14,
            TextColor = Colors.Gray
        };
        
        historyLabel = new Label
        {
            Text = "Status History:\n(empty)",
            FontSize = 12,
            LineBreakMode = LineBreakMode.WordWrap
        };
        
        var startBtn = new Button { Text = "Start Process" };
        startBtn.Clicked += (s, e) => UpdateStatus("RUNNING");
        
        var completeBtn = new Button { Text = "Complete" };
        completeBtn.Clicked += (s, e) => UpdateStatus("COMPLETE");
        
        var errorBtn = new Button { Text = "Simulate Error" };
        errorBtn.Clicked += (s, e) => UpdateStatus("ERROR");
        
        var layout = new VerticalStackLayout
        {
            Padding = new Thickness(20),
            Spacing = 15,
            Children =
            {
                new Label { Text = "System Status Monitor", FontSize = 24 },
                statusGauge,
                timestampLabel,
                new HorizontalStackLayout
                {
                    Spacing = 10,
                    Children = { startBtn, completeBtn, errorBtn }
                },
                historyLabel
            }
        };
        
        this.Content = new ScrollView { Content = layout };
    }
    
    private void OnStatusChanged(object sender, DigitalGaugeTextChangedEventArgs e)
    {
        // Update timestamp
        DateTime now = DateTime.Now;
        timestampLabel.Text = $"Last updated: {now:HH:mm:ss}";
        
        // Log change
        var change = new StatusChange
        {
            Timestamp = now,
            OldStatus = e.OldText,
            NewStatus = e.NewText
        };
        statusHistory.Add(change);
        
        // Update history display (last 5 changes)
        var recentHistory = statusHistory.TakeLast(5).Reverse();
        historyLabel.Text = "Status History:\n" + 
            string.Join("\n", recentHistory.Select(h => 
                $"{h.Timestamp:HH:mm:ss} - {h.OldStatus} → {h.NewStatus}"));
        
        // Update color based on status
        statusGauge.CharacterStroke = e.NewText switch
        {
            "IDLE" => Colors.Gray,
            "RUNNING" => Colors.Blue,
            "COMPLETE" => Colors.Green,
            "ERROR" => Colors.Red,
            _ => Colors.White
        };
        
        // Trigger alert for errors
        if (e.NewText == "ERROR")
        {
            DisplayAlert("Status Alert", "System encountered an error!", "OK");
        }
    }
    
    private void UpdateStatus(string newStatus)
    {
        statusGauge.Text = newStatus;
    }
    
    private class StatusChange
    {
        public DateTime Timestamp { get; set; }
        public string OldStatus { get; set; }
        public string NewStatus { get; set; }
    }
}
```

## Event Handling Best Practices

### Unsubscribe When Done

Always unsubscribe from events to prevent memory leaks:

```csharp
protected override void OnDisappearing()
{
    base.OnDisappearing();
    digitalGauge.TextChanged -= OnTextChanged;
}
```

### Avoid Infinite Loops

Be careful when modifying the Text property inside the event handler:

```csharp
// ❌ BAD - Can cause infinite loop
private void OnTextChanged(object sender, DigitalGaugeTextChangedEventArgs e)
{
    var gauge = (SfDigitalGauge)sender;
    gauge.Text = e.NewText.ToUpper(); // This triggers another TextChanged event!
}

// ✅ GOOD - Unsubscribe before modifying
private void OnTextChanged(object sender, DigitalGaugeTextChangedEventArgs e)
{
    var gauge = (SfDigitalGauge)sender;
    
    if (e.NewText != e.NewText.ToUpper())
    {
        gauge.TextChanged -= OnTextChanged;
        gauge.Text = e.NewText.ToUpper();
        gauge.TextChanged += OnTextChanged;
    }
}
```

### Keep Handlers Lightweight

Event handlers should execute quickly:

```csharp
// ✅ GOOD - Quick operations
private void OnTextChanged(object sender, DigitalGaugeTextChangedEventArgs e)
{
    UpdateLabel(e.NewText);
    LogChange(e.NewText);
}

// ❌ BAD - Long-running operations block UI
private void OnTextChanged(object sender, DigitalGaugeTextChangedEventArgs e)
{
    Thread.Sleep(2000); // Blocks UI!
    SaveToDatabase(e.NewText);
}

// ✅ GOOD - Use async for long operations
private async void OnTextChanged(object sender, DigitalGaugeTextChangedEventArgs e)
{
    await Task.Run(() => SaveToDatabase(e.NewText));
}
```

### Check for Null

Always validate event arguments:

```csharp
private void OnTextChanged(object sender, DigitalGaugeTextChangedEventArgs e)
{
    if (e?.NewText == null)
        return;
    
    // Safe to use e.NewText
    ProcessText(e.NewText);
}
```

## Troubleshooting

### Event Not Firing

**Problem:** TextChanged event doesn't trigger.

**Solutions:**
1. Verify event is subscribed correctly
2. Check that Text property is actually changing
3. Ensure gauge is not null
4. Verify using correct event name (TextChanged, not TextChange)

### Event Fires Multiple Times

**Problem:** Event handler executes more than expected.

**Solutions:**
1. Check if you're subscribing multiple times
2. Verify no duplicate subscriptions in XAML and code
3. Use event handler unsubscription when needed

### Performance Issues

**Problem:** UI becomes sluggish with frequent text updates.

**Solutions:**
1. Debounce rapid text changes
2. Limit event handler operations
3. Use async operations for heavy tasks
4. Consider batching updates
