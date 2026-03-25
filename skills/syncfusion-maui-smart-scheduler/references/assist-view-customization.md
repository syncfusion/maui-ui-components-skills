# Assist View Customization

## Table of Contents
- [Overview](#overview)
- [Assist Button Customization](#assist-button-customization)
- [Assist View Dimensions](#assist-view-dimensions)
- [Header Customization](#header-customization)
- [Placeholder and Prompts](#placeholder-and-prompts)
- [Suggested Prompts](#suggested-prompts)
- [Banner Customization](#banner-customization)
- [Template Customization](#template-customization)
- [Adaptive Layouts](#adaptive-layouts)
- [Complete Examples](#complete-examples)

---

## Overview

The assist view is the conversational interface where users interact with the AI-powered scheduler. It includes:
- **Assist Button:** Floating button to open the assist view
- **Assist Panel:** The main conversational interface
- **Header:** Title area of the assist view
- **Input Field:** Where users type natural language commands
- **Suggested Prompts:** Quick-action buttons
- **Banner:** Optional welcome/info message

All elements are customizable to match your app's design and branding.

---

## Assist Button Customization

### Enable or Disable Assist Button

Control whether the assist button appears:

**XAML:**
```xml
<smartScheduler:SfSmartScheduler x:Name="smartScheduler" 
                                 EnableAssistButton="True" />
```

**C#:**
```csharp
smartScheduler.EnableAssistButton = true; // Show button
smartScheduler.EnableAssistButton = false; // Hide button
```

**Default:** `true` (button is visible)

### Custom Assist Button Template

Replace the default assist button with custom design:

**XAML:**
```xml
<smartScheduler:SfSmartScheduler x:Name="smartScheduler">
    <smartScheduler:SfSmartScheduler.AssistButtonTemplate>
        <DataTemplate>
            <Grid BackgroundColor="#6750A4" 
                  WidthRequest="60" 
                  HeightRequest="60"
                  CornerRadius="30">
                <Label Text="AI"
                       FontAttributes="Bold"
                       FontSize="20"
                       TextColor="#FFFFFF"
                       VerticalOptions="Center"
                       HorizontalOptions="Center" />
            </Grid>
        </DataTemplate>
    </smartScheduler:SfSmartScheduler.AssistButtonTemplate>
</smartScheduler:SfSmartScheduler>
```

**C#:**
```csharp
smartScheduler.AssistButtonTemplate = new DataTemplate(() =>
{
    var grid = new Grid
    {
        BackgroundColor = Color.FromArgb("#6750A4"),
        WidthRequest = 60,
        HeightRequest = 60
    };
    
    // Make it circular
    grid.CornerRadius = 30;

    var label = new Label
    {
        Text = "AI",
        FontAttributes = FontAttributes.Bold,
        FontSize = 20,
        TextColor = Colors.White,
        HorizontalOptions = LayoutOptions.Center,
        VerticalOptions = LayoutOptions.Center
    };

    grid.Children.Add(label);
    return grid;
});
```

### Custom Icon Button

**XAML:**
```xml
<smartScheduler:SfSmartScheduler.AssistButtonTemplate>
    <DataTemplate>
        <Border BackgroundColor="#6750A4" 
                StrokeShape="RoundRectangle 30"
                Padding="12">
            <Image Source="ai_icon.png" 
                   WidthRequest="36" 
                   HeightRequest="36" />
        </Border>
    </DataTemplate>
</smartScheduler:SfSmartScheduler.AssistButtonTemplate>
```

### Animated Button

**C#:**
```csharp
smartScheduler.AssistButtonTemplate = new DataTemplate(() =>
{
    var button = new Border
    {
        BackgroundColor = Color.FromArgb("#6750A4"),
        StrokeShape = new RoundRectangle { CornerRadius = 30 },
        Padding = 16
    };
    
    var label = new Label
    {
        Text = "🤖",
        FontSize = 24,
        HorizontalOptions = LayoutOptions.Center,
        VerticalOptions = LayoutOptions.Center
    };
    
    button.Content = label;
    
    // Add pulsing animation
    var animation = new Animation(v => button.Scale = v, 1, 1.1);
    animation.Commit(button, "Pulse", length: 1000, repeat: () => true);
    
    return button;
});
```

---

## Assist View Dimensions

### Assist View Height

Customize the height of the assist panel:

**XAML:**
```xml
<smartScheduler:SfSmartScheduler x:Name="smartScheduler">
    <smartScheduler:SfSmartScheduler.AssistViewSettings>
        <smartScheduler:SchedulerAssistViewSettings AssistViewHeight="420"/>
    </smartScheduler:SfSmartScheduler.AssistViewSettings>
</smartScheduler:SfSmartScheduler>
```

**C#:**
```csharp
smartScheduler.AssistViewSettings.AssistViewHeight = 420;
```

**Default:** Adaptive based on platform and screen size

**Recommendations:**
- **Mobile (Phone):** 300-400 pixels
- **Tablet:** 400-500 pixels
- **Desktop:** 500-600 pixels

### Assist View Width

Customize the width of the assist panel:

**XAML:**
```xml
<smartScheduler:SfSmartScheduler x:Name="smartScheduler">
    <smartScheduler:SfSmartScheduler.AssistViewSettings>
        <smartScheduler:SchedulerAssistViewSettings AssistViewWidth="500"/>
    </smartScheduler:SfSmartScheduler.AssistViewSettings>
</smartScheduler:SfSmartScheduler>
```

**C#:**
```csharp
smartScheduler.AssistViewSettings.AssistViewWidth = 500;
```

**Default:** Adaptive based on platform

**Recommendations:**
- **Mobile (Phone):** Full width (or 90% of screen)
- **Tablet:** 400-600 pixels
- **Desktop:** 500-700 pixels

### Responsive Dimensions

Set dimensions based on device:

```csharp
double screenWidth = DeviceDisplay.MainDisplayInfo.Width / DeviceDisplay.MainDisplayInfo.Density;

if (DeviceInfo.Idiom == DeviceIdiom.Phone)
{
    smartScheduler.AssistViewSettings.AssistViewWidth = screenWidth * 0.9;
    smartScheduler.AssistViewSettings.AssistViewHeight = 350;
}
else if (DeviceInfo.Idiom == DeviceIdiom.Tablet)
{
    smartScheduler.AssistViewSettings.AssistViewWidth = 500;
    smartScheduler.AssistViewSettings.AssistViewHeight = 450;
}
else // Desktop
{
    smartScheduler.AssistViewSettings.AssistViewWidth = 600;
    smartScheduler.AssistViewSettings.AssistViewHeight = 550;
}
```

---

## Header Customization

### Header Text

Change the assist view header text:

**XAML:**
```xml
<smartScheduler:SchedulerAssistViewSettings AssistViewHeaderText="Smart Scheduler" />
```

**C#:**
```csharp
smartScheduler.AssistViewSettings.AssistViewHeaderText = "AI Assistant";
```

**Examples:**
- "AI Scheduling Assistant"
- "Smart Scheduler"
- "Meeting Planner"
- "Schedule Helper"

### Header Template

Fully customize header appearance:

**XAML:**
```xml
<smartScheduler:SfSmartScheduler.AssistViewSettings>
    <smartScheduler:SchedulerAssistViewSettings>
        <smartScheduler:SchedulerAssistViewSettings.AssistViewHeaderTemplate>
            <DataTemplate>
                <Grid BackgroundColor="#6750A4" Padding="16">
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition Width="Auto"/>
                        <ColumnDefinition Width="*"/>
                        <ColumnDefinition Width="Auto"/>
                    </Grid.ColumnDefinitions>
                    
                    <Image Source="ai_icon.png" 
                           WidthRequest="24" 
                           HeightRequest="24"
                           Grid.Column="0"/>
                    
                    <Label Text="AI Scheduler" 
                           FontSize="18"
                           FontAttributes="Bold"
                           TextColor="White"
                           VerticalOptions="Center"
                           Grid.Column="1"
                           Margin="12,0,0,0"/>
                    
                    <Button Text="✕" 
                            BackgroundColor="Transparent"
                            TextColor="White"
                            Grid.Column="2"
                            Clicked="CloseAssistView"/>
                </Grid>
            </DataTemplate>
        </smartScheduler:SchedulerAssistViewSettings.AssistViewHeaderTemplate>
    </smartScheduler:SchedulerAssistViewSettings>
</smartScheduler:SfSmartScheduler.AssistViewSettings>
```

**C#:**
```csharp
smartScheduler.AssistViewSettings.AssistViewHeaderTemplate = new DataTemplate(() =>
{
    var grid = new Grid 
    { 
        BackgroundColor = Color.FromArgb("#6750A4"), 
        Padding = 16,
        ColumnDefinitions =
        {
            new ColumnDefinition { Width = GridLength.Auto },
            new ColumnDefinition { Width = GridLength.Star },
            new ColumnDefinition { Width = GridLength.Auto }
        }
    };
    
    var icon = new Image 
    { 
        Source = "ai_icon.png", 
        WidthRequest = 24, 
        HeightRequest = 24 
    };
    Grid.SetColumn(icon, 0);
    
    var title = new Label 
    { 
        Text = "AI Scheduler",
        FontSize = 18,
        FontAttributes = FontAttributes.Bold,
        TextColor = Colors.White,
        VerticalOptions = LayoutOptions.Center,
        Margin = new Thickness(12, 0, 0, 0)
    };
    Grid.SetColumn(title, 1);
    
    var closeButton = new Button 
    { 
        Text = "✕",
        BackgroundColor = Colors.Transparent,
        TextColor = Colors.White
    };
    closeButton.Clicked += (s, e) => smartScheduler.CloseAssistView();
    Grid.SetColumn(closeButton, 2);
    
    grid.Children.Add(icon);
    grid.Children.Add(title);
    grid.Children.Add(closeButton);
    
    return grid;
});
```

---

## Placeholder and Prompts

### Input Placeholder Text

Customize the placeholder text in the input field:

**XAML:**
```xml
<smartScheduler:SchedulerAssistViewSettings Placeholder="Enter your message..." />
```

**C#:**
```csharp
smartScheduler.AssistViewSettings.Placeholder = "Ask me to schedule meetings...";
```

**Examples:**
- "Ask me anything about your schedule..."
- "Type to schedule, find, or modify appointments"
- "How can I help you today?"
- "Schedule meetings naturally..."

### AI System Prompt

Configure the AI's behavior and instructions:

**XAML:**
```xml
<smartScheduler:SchedulerAssistViewSettings 
    Prompt="You are a helpful scheduling assistant. Understand natural language and create appointments efficiently. Prioritize user preferences and suggest alternatives when conflicts arise." />
```

**C#:**
```csharp
smartScheduler.AssistViewSettings.Prompt = @"
You are a professional scheduling assistant for a corporate environment.
- Understand natural language appointment requests
- Check resource availability before booking
- Detect and resolve scheduling conflicts
- Suggest optimal meeting times
- Respect working hours (9am-6pm)
- Prioritize conference room capacity matching meeting size
";
```

**Domain-Specific Prompts:**

**Healthcare:**
```csharp
smartScheduler.AssistViewSettings.Prompt = @"
You are a medical appointment scheduler.
- Prioritize urgent cases
- Respect doctor specializations and availability
- Consider room equipment requirements
- Suggest earliest available slots for patients
- Handle appointment modifications sensitively
";
```

**Education:**
```csharp
smartScheduler.AssistViewSettings.Prompt = @"
You are an academic scheduling assistant.
- Understand class schedules and academic calendar
- Respect room capacities and AV equipment needs
- Avoid scheduling conflicts with exams and breaks
- Suggest appropriate classroom assignments
";
```

---

## Suggested Prompts

### Basic Suggested Prompts

Provide quick-action buttons for common tasks:

**XAML with ViewModel:**
```xml
<ContentPage.BindingContext>
    <local:ViewModel/>
</ContentPage.BindingContext>

<smartScheduler:SfSmartScheduler x:Name="smartScheduler">
    <smartScheduler:SfSmartScheduler.AssistViewSettings>
        <smartScheduler:SchedulerAssistViewSettings 
            SuggestedPrompts="{Binding SuggestedPrompts}"/>
    </smartScheduler:SfSmartScheduler.AssistViewSettings>
</smartScheduler:SfSmartScheduler>
```

**ViewModel.cs:**
```csharp
public class ViewModel : INotifyPropertyChanged
{
    private List<string> suggestedPrompts;
    
    public List<string> SuggestedPrompts
    {
        get => suggestedPrompts;
        set
        {
            suggestedPrompts = value;
            OnPropertyChanged(nameof(SuggestedPrompts));
        }
    }

    public ViewModel()
    {
        SuggestedPrompts = new List<string>
        {
            "Summarize today's appointments",
            "Find free time this week",
            "Check for conflicts",
            "Schedule team meeting tomorrow at 2pm"
        };
    }

    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

**C# Direct Assignment:**
```csharp
smartScheduler.AssistViewSettings.SuggestedPrompts = new ObservableCollection<string>
{
    "Find free time today",
    "Summarize my schedule",
    "Check for conflicts",
    "Schedule meeting tomorrow"
};
```

### Domain-Specific Suggested Prompts

**Corporate:**
```csharp
smartScheduler.AssistViewSettings.SuggestedPrompts = new ObservableCollection<string>
{
    "Book conference room for 10 people",
    "Find time for project sync this week",
    "Summarize client meetings",
    "Check room availability tomorrow"
};
```

**Healthcare:**
```csharp
smartScheduler.AssistViewSettings.SuggestedPrompts = new ObservableCollection<string>
{
    "Schedule patient consultation",
    "Find doctor's next available slot",
    "Check room equipment availability",
    "List today's appointments"
};
```

### Dynamic Suggested Prompts

Update prompts based on context:

```csharp
public void UpdateSuggestedPromptsBasedOnTime()
{
    var currentHour = DateTime.Now.Hour;
    
    if (currentHour < 12) // Morning
    {
        smartScheduler.AssistViewSettings.SuggestedPrompts = new ObservableCollection<string>
        {
            "Summarize today's schedule",
            "Find free time this afternoon",
            "Check for morning conflicts"
        };
    }
    else // Afternoon/Evening
    {
        smartScheduler.AssistViewSettings.SuggestedPrompts = new ObservableCollection<string>
        {
            "Plan tomorrow's meetings",
            "Review remaining appointments",
            "Schedule for next week"
        };
    }
}
```

---

## Banner Customization

### Show/Hide Banner

Control banner visibility:

**XAML:**
```xml
<smartScheduler:SchedulerAssistViewSettings ShowAssistViewBanner="True" />
```

**C#:**
```csharp
smartScheduler.AssistViewSettings.ShowAssistViewBanner = true;
```

**Default:** `false` (banner hidden)

### Custom Banner Template

Create a custom welcome banner:

**XAML:**
```xml
<smartScheduler:SfSmartScheduler.AssistViewSettings>
    <smartScheduler:SchedulerAssistViewSettings ShowAssistViewBanner="True">
        <smartScheduler:SchedulerAssistViewSettings.AssistViewBannerTemplate>
            <DataTemplate>
                <Grid BackgroundColor="#E9EEFF" 
                      Padding="16" 
                      Margin="0,40,0,0">
                    <Label Text="Hi! I'm your AI scheduling assistant.&#x0a;How can I help you today?"
                           HorizontalTextAlignment="Center"
                           VerticalTextAlignment="Center"
                           TextColor="#1C1B1F"
                           FontSize="16" />
                </Grid>
            </DataTemplate>
        </smartScheduler:SchedulerAssistViewSettings.AssistViewBannerTemplate>
    </smartScheduler:SchedulerAssistViewSettings>
</smartScheduler:SfSmartScheduler.AssistViewSettings>
```

**C#:**
```csharp
smartScheduler.AssistViewSettings.ShowAssistViewBanner = true;
smartScheduler.AssistViewSettings.AssistViewBannerTemplate = new DataTemplate(() =>
{
    var grid = new Grid
    {
        BackgroundColor = Color.FromArgb("#E9EEFF"),
        Padding = 16,
        Margin = new Thickness(0, 40, 0, 0)
    };

    var label = new Label
    {
        Text = "Hi! I'm your AI scheduling assistant.\nHow can I help you today?",
        HorizontalTextAlignment = TextAlignment.Center,
        VerticalTextAlignment = TextAlignment.Center,
        TextColor = Color.FromArgb("#1C1B1F"),
        FontSize = 16
    };

    grid.Children.Add(label);
    return grid;
});
```

### Rich Banner with Icon

```csharp
smartScheduler.AssistViewSettings.AssistViewBannerTemplate = new DataTemplate(() =>
{
    var grid = new Grid
    {
        BackgroundColor = Color.FromArgb("#E9EEFF"),
        Padding = 20,
        Margin = new Thickness(0, 40, 0, 0),
        RowDefinitions =
        {
            new RowDefinition { Height = GridLength.Auto },
            new RowDefinition { Height = GridLength.Auto }
        }
    };

    var icon = new Label
    {
        Text = "🤖",
        FontSize = 48,
        HorizontalOptions = LayoutOptions.Center
    };
    Grid.SetRow(icon, 0);

    var message = new Label
    {
        Text = "Welcome to Smart Scheduler!\nI can help you manage appointments using natural language.",
        HorizontalTextAlignment = TextAlignment.Center,
        TextColor = Color.FromArgb("#1C1B1F"),
        FontSize = 14,
        Margin = new Thickness(0, 12, 0, 0)
    };
    Grid.SetRow(message, 1);

    grid.Children.Add(icon);
    grid.Children.Add(message);
    
    return grid;
});
```

---

## Template Customization

### Complete Custom Assist View

You can fully customize the entire assist view layout (advanced):

```csharp
// Note: Full assist view template customization depends on 
// Syncfusion's API support for complete template override
// Check latest documentation for template extension points
```

---

## Adaptive Layouts

### Platform-Specific Layouts

Adjust layout based on device type:

```csharp
public void ConfigureAdaptiveLayout()
{
    if (DeviceInfo.Platform == DevicePlatform.Android || 
        DeviceInfo.Platform == DevicePlatform.iOS)
    {
        // Mobile layout
        if (DeviceInfo.Idiom == DeviceIdiom.Phone)
        {
            smartScheduler.AssistViewSettings.AssistViewHeight = 350;
            smartScheduler.AssistViewSettings.AssistViewWidth = 
                DeviceDisplay.MainDisplayInfo.Width / DeviceDisplay.MainDisplayInfo.Density * 0.95;
        }
        else // Tablet
        {
            smartScheduler.AssistViewSettings.AssistViewHeight = 450;
            smartScheduler.AssistViewSettings.AssistViewWidth = 500;
        }
    }
    else // Desktop (Windows, macOS)
    {
        smartScheduler.AssistViewSettings.AssistViewHeight = 550;
        smartScheduler.AssistViewSettings.AssistViewWidth = 600;
    }
}
```

### Orientation-Aware Layout

```csharp
protected override void OnSizeAllocated(double width, double height)
{
    base.OnSizeAllocated(width, height);
    
    if (width > height) // Landscape
    {
        smartScheduler.AssistViewSettings.AssistViewWidth = width * 0.5;
        smartScheduler.AssistViewSettings.AssistViewHeight = height * 0.7;
    }
    else // Portrait
    {
        smartScheduler.AssistViewSettings.AssistViewWidth = width * 0.9;
        smartScheduler.AssistViewSettings.AssistViewHeight = height * 0.5;
    }
}
```

---

## Complete Examples

### Corporate Branding Example

```csharp
public void ApplyCorporateBranding()
{
    // Custom assist button
    smartScheduler.EnableAssistButton = true;
    smartScheduler.AssistButtonTemplate = new DataTemplate(() =>
    {
        return new Border
        {
            BackgroundColor = Color.FromArgb("#0078D4"), // Corporate blue
            StrokeShape = new RoundRectangle { CornerRadius = 30 },
            Padding = 14,
            Content = new Image 
            { 
                Source = "company_ai_icon.png",
                WidthRequest = 32,
                HeightRequest = 32
            }
        };
    });

    // Assist view settings
    smartScheduler.AssistViewSettings.AssistViewHeight = 500;
    smartScheduler.AssistViewSettings.AssistViewWidth = 550;
    smartScheduler.AssistViewSettings.AssistViewHeaderText = "Corporate Scheduler AI";
    smartScheduler.AssistViewSettings.Placeholder = "Schedule meetings, find rooms, check availability...";
    
    // Suggested prompts
    smartScheduler.AssistViewSettings.SuggestedPrompts = new ObservableCollection<string>
    {
        "Book executive boardroom",
        "Find free time with CEO",
        "Check quarterly meeting schedule",
        "List today's client meetings"
    };
    
    // Corporate-specific AI prompt
    smartScheduler.AssistViewSettings.Prompt = @"
    You are a corporate scheduling assistant for [Company Name].
    - Prioritize executive availability
    - Respect meeting room hierarchies
    - Suggest video calls for remote participants
    - Maintain professional tone
    ";
}
```

### Healthcare Example

```csharp
public void ApplyHealthcareBranding()
{
    smartScheduler.AssistViewSettings.AssistViewHeaderText = "Patient Scheduler";
    smartScheduler.AssistViewSettings.Placeholder = "Schedule appointments, check doctor availability...";
    
    smartScheduler.AssistViewSettings.SuggestedPrompts = new ObservableCollection<string>
    {
        "Next available with Dr. Smith",
        "Schedule consultation",
        "Check exam room availability",
        "List today's patients"
    };
    
    smartScheduler.AssistViewSettings.Prompt = @"
    You are a medical appointment scheduler.
    - Prioritize urgent and emergency cases
    - Respect doctor specializations
    - Consider room equipment (X-ray, ultrasound, etc.)
    - Handle patient information sensitively
    ";
    
    // Show helpful banner
    smartScheduler.AssistViewSettings.ShowAssistViewBanner = true;
    smartScheduler.AssistViewSettings.AssistViewBannerTemplate = new DataTemplate(() =>
    {
        return new Grid
        {
            BackgroundColor = Color.FromArgb("#E8F5E9"), // Soft medical green
            Padding = 16,
            Margin = new Thickness(0, 40, 0, 0),
            Children =
            {
                new Label
                {
                    Text = "🏥 Schedule patient appointments naturally.\nI'll help find the best times and rooms.",
                    HorizontalTextAlignment = TextAlignment.Center,
                    FontSize = 14
                }
            }
        };
    });
}
```

---

## Best Practices

1. **Keep it simple:** Don't overcomplicate the UI—users should focus on natural language
2. **Brand consistently:** Match your app's colors, fonts, and design language
3. **Provide guidance:** Use suggested prompts to show users what's possible
4. **Test on multiple devices:** Ensure layouts work on phone, tablet, and desktop
5. **Update dynamically:** Change prompts and banners based on context
6. **Accessibility:** Ensure good contrast ratios and readable font sizes

---

## Next Steps

- **Learn about events and methods:** [events-and-methods.md](events-and-methods.md)
- **Style assist view elements:** [styling.md](styling.md)
