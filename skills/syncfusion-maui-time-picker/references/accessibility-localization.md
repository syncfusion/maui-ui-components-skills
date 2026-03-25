# Accessibility and Localization

## Table of Contents
- [Accessibility Features](#accessibility-features)
- [WCAG Compliance](#wcag-compliance)
- [Screen Reader Support](#screen-reader-support)
- [Keyboard Navigation](#keyboard-navigation)
- [Localization](#localization)
- [Culture-Specific Formatting](#culture-specific-formatting)
- [RTL Language Support](#rtl-language-support)

## Accessibility Features

The TimePicker control is designed with accessibility in mind, ensuring all users can effectively interact with the component regardless of their abilities.

### Semantic Properties

```xml
<picker:SfTimePicker 
    SemanticProperties.Description="Select appointment time"
    SemanticProperties.Hint="Choose a time between 9 AM and 5 PM"
    SelectedTime="14:30:00" />
```

```csharp
var timePicker = new SfTimePicker
{
    SemanticProperties = 
    {
        Description = "Select appointment time",
        Hint = "Choose a time between 9 AM and 5 PM"
    },
    SelectedTime = new TimeSpan(14, 30, 0)
};
```

### Accessible Labels

```xml
<StackLayout>
    <Label 
        Text="Meeting Time"
        FontAttributes="Bold"
        FontSize="16"
        SemanticProperties.HeadingLevel="Level2" />
    
    <picker:SfTimePicker 
        x:Name="meetingTimePicker"
        SemanticProperties.Description="Select meeting time"
        Format="hh_mm_tt" />
</StackLayout>
```

## WCAG Compliance

The TimePicker meets WCAG 2.1 Level AA standards for accessibility:

### Color Contrast

Ensure text and background colors meet minimum contrast ratios:

```xml
<!-- High contrast theme for accessibility -->
<picker:SfTimePicker>
    <picker:SfTimePicker.HeaderView>
        <picker:PickerHeaderView 
            Text="Select Time"
            Background="#000000"
            TextStyle="{picker:PickerTextStyle 
                TextColor=#FFFFFF,
                FontSize=18,
                FontAttributes=Bold}" />
    </picker:SfTimePicker.HeaderView>
    
    <picker:SfTimePicker.SelectionView>
        <picker:PickerSelectionView 
            Background="#0066CC"
            Stroke="#000000"
            StrokeThickness="2" />
    </picker:SfTimePicker.SelectionView>
</picker:SfTimePicker>
```

### Focus Indicators

The control provides clear visual feedback when focused:

```csharp
var timePicker = new SfTimePicker
{
    SelectionView = new PickerSelectionView
    {
        Stroke = Colors.Blue,
        StrokeThickness = 3,
        CornerRadius = 8
    }
};
```

## Screen Reader Support

The TimePicker works seamlessly with screen readers like TalkBack (Android) and VoiceOver (iOS).

### Configuring for Screen Readers

```xml
<picker:SfTimePicker 
    AutomationId="AppointmentTimePicker"
    SemanticProperties.Description="Appointment time selector"
    SemanticProperties.Hint="Swipe to change time, double tap to confirm"
    SelectedTime="10:00:00"
    Format="hh_mm_tt" />
```

```csharp
var timePicker = new SfTimePicker
{
    AutomationId = "AppointmentTimePicker",
    SemanticProperties = 
    {
        Description = "Appointment time selector",
        Hint = "Swipe to change time, double tap to confirm"
    }
};

// Screen reader will announce: "Appointment time selector, 10:00 AM"
```

### Accessible Header and Footer

```xml
<picker:SfTimePicker>
    <picker:SfTimePicker.HeaderView>
        <picker:PickerHeaderView 
            Text="Appointment Time"
            SemanticProperties.HeadingLevel="Level1" />
    </picker:SfTimePicker.HeaderView>
    
    <picker:SfTimePicker.FooterView>
        <picker:PickerFooterView 
            OkButtonText="Confirm"
            CancelButtonText="Cancel"
            SemanticProperties.Hint="Confirm or cancel your selection" />
    </picker:SfTimePicker.FooterView>
</picker:SfTimePicker>
```

## Keyboard Navigation

The TimePicker supports full keyboard navigation for desktop platforms.

### Navigation Keys

- **Tab**: Move focus between controls
- **Arrow Keys**: Navigate through time values
- **Enter**: Confirm selection
- **Escape**: Cancel and close picker

```csharp
var timePicker = new SfTimePicker
{
    // Keyboard navigation is enabled by default
    Mode = PickerMode.Dialog
};

timePicker.Opened += (s, e) =>
{
    // Focus is automatically set to the selected time
    Debug.WriteLine("Picker opened - keyboard navigation active");
};
```

## Localization

The TimePicker supports localization for multiple languages and cultures.

### Setting Up Localization

**Step 1:** Install the localization NuGet package:

```bash
dotnet add package Syncfusion.Maui.Picker
```

**Step 2:** Configure the current culture in your app:

```csharp
// App.xaml.cs or MauiProgram.cs
using System.Globalization;
using Syncfusion.Maui.Picker;

public partial class App : Application
{
    public App()
    {
        InitializeComponent();
        
        // Set the desired culture
        CultureInfo culture = new CultureInfo("es-ES"); // Spanish
        CultureInfo.CurrentUICulture = culture;
        CultureInfo.CurrentCulture = culture;
        
        MainPage = new AppShell();
    }
}
```

### Localizing Text with Resource Files

**Step 1:** Create a resource file for each language (e.g., `SfPickerResources.resx`, `SfPickerResources.es.resx`):

**SfPickerResources.resx** (English - default):
```xml
<data name="OK" xml:space="preserve">
    <value>OK</value>
</data>
<data name="Cancel" xml:space="preserve">
    <value>Cancel</value>
</data>
```

**SfPickerResources.es.resx** (Spanish):
```xml
<data name="OK" xml:space="preserve">
    <value>Aceptar</value>
</data>
<data name="Cancel" xml:space="preserve">
    <value>Cancelar</value>
</data>
```

**Step 2:** Apply localized resources:

```csharp
using System.Resources;
using Syncfusion.Maui.Picker;

// In your MauiProgram.cs or App.xaml.cs
SfPickerResources.ResourceManager = new ResourceManager(
    "YourNamespace.Resources.SfPickerResources",
    Application.Current.GetType().Assembly
);
```

### Localized TimePicker Example

```xml
<picker:SfTimePicker 
    Format="HH_mm"
    SelectedTime="15:30:00">
    
    <picker:SfTimePicker.HeaderView>
        <picker:PickerHeaderView Text="{x:Static resx:SfPickerResources.SelectTime}" />
    </picker:SfTimePicker.HeaderView>
    
    <picker:SfTimePicker.FooterView>
        <picker:PickerFooterView 
            OkButtonText="{x:Static resx:SfPickerResources.OK}"
            CancelButtonText="{x:Static resx:SfPickerResources.Cancel}" />
    </picker:SfTimePicker.FooterView>
</picker:SfTimePicker>
```

### Multi-Language Support Example

```csharp
public class LocalizedTimePickerPage : ContentPage
{
    private SfTimePicker timePicker;
    
    public LocalizedTimePickerPage()
    {
        // Language selector
        var languagePicker = new Picker
        {
            Title = "Select Language",
            ItemsSource = new List<string> { "English", "Spanish", "French", "German" }
        };
        
        languagePicker.SelectedIndexChanged += OnLanguageChanged;
        
        timePicker = new SfTimePicker
        {
            HeaderView = new PickerHeaderView { Text = "Select Time" },
            FooterView = new PickerFooterView 
            { 
                OkButtonText = "OK",
                CancelButtonText = "Cancel"
            }
        };
        
        Content = new StackLayout
        {
            Children = { languagePicker, timePicker },
            Padding = 20
        };
    }
    
    private void OnLanguageChanged(object sender, EventArgs e)
    {
        var picker = (Picker)sender;
        var culture = picker.SelectedIndex switch
        {
            0 => new CultureInfo("en-US"),
            1 => new CultureInfo("es-ES"),
            2 => new CultureInfo("fr-FR"),
            3 => new CultureInfo("de-DE"),
            _ => CultureInfo.CurrentCulture
        };
        
        CultureInfo.CurrentUICulture = culture;
        CultureInfo.CurrentCulture = culture;
        
        // Update UI with localized text
        UpdateLocalizedText(culture);
    }
    
    private void UpdateLocalizedText(CultureInfo culture)
    {
        timePicker.HeaderView.Text = culture.Name switch
        {
            "es-ES" => "Seleccionar hora",
            "fr-FR" => "Sélectionner l'heure",
            "de-DE" => "Zeit auswählen",
            _ => "Select Time"
        };
        
        timePicker.FooterView.OkButtonText = culture.Name switch
        {
            "es-ES" => "Aceptar",
            "fr-FR" => "D'accord",
            "de-DE" => "OK",
            _ => "OK"
        };
        
        timePicker.FooterView.CancelButtonText = culture.Name switch
        {
            "es-ES" => "Cancelar",
            "fr-FR" => "Annuler",
            "de-DE" => "Abbrechen",
            _ => "Cancel"
        };
    }
}
```

## Culture-Specific Formatting

Different cultures display time in different formats. The TimePicker automatically adapts to the current culture.

### Automatic Culture Formatting

```csharp
// US Culture (12-hour with AM/PM)
CultureInfo.CurrentCulture = new CultureInfo("en-US");
var usTimePicker = new SfTimePicker
{
    Format = PickerFormat.hh_mm_tt, // 02:30 PM
    SelectedTime = new TimeSpan(14, 30, 0)
};

// German Culture (24-hour)
CultureInfo.CurrentCulture = new CultureInfo("de-DE");
var deTimePicker = new SfTimePicker
{
    Format = PickerFormat.HH_mm, // 14:30
    SelectedTime = new TimeSpan(14, 30, 0)
};

// French Culture (24-hour)
CultureInfo.CurrentCulture = new CultureInfo("fr-FR");
var frTimePicker = new SfTimePicker
{
    Format = PickerFormat.HH_mm, // 14h30
    SelectedTime = new TimeSpan(14, 30, 0)
};
```

### Localizing Column Headers

```csharp
var timePicker = new SfTimePicker
{
    ColumnHeaderView = new TimePickerColumnHeaderView()
};

// Set culture-specific column headers
var culture = CultureInfo.CurrentCulture.Name;
switch (culture)
{
    case "es-ES":
        timePicker.ColumnHeaderView.HourHeaderText = "Hora";
        timePicker.ColumnHeaderView.MinuteHeaderText = "Minuto";
        timePicker.ColumnHeaderView.SecondHeaderText = "Segundo";
        timePicker.ColumnHeaderView.MeridiemHeaderText = "AM/PM";
        break;
    case "fr-FR":
        timePicker.ColumnHeaderView.HourHeaderText = "Heure";
        timePicker.ColumnHeaderView.MinuteHeaderText = "Minute";
        timePicker.ColumnHeaderView.SecondHeaderText = "Seconde";
        timePicker.ColumnHeaderView.MeridiemHeaderText = "AM/PM";
        break;
    case "de-DE":
        timePicker.ColumnHeaderView.HourHeaderText = "Stunde";
        timePicker.ColumnHeaderView.MinuteHeaderText = "Minute";
        timePicker.ColumnHeaderView.SecondHeaderText = "Sekunde";
        timePicker.ColumnHeaderView.MeridiemHeaderText = "AM/PM";
        break;
    default:
        timePicker.ColumnHeaderView.HourHeaderText = "Hour";
        timePicker.ColumnHeaderView.MinuteHeaderText = "Minute";
        timePicker.ColumnHeaderView.SecondHeaderText = "Second";
        timePicker.ColumnHeaderView.MeridiemHeaderText = "AM/PM";
        break;
}
```

## RTL Language Support

The TimePicker supports Right-to-Left (RTL) languages like Arabic and Hebrew.

### Enabling RTL

```xml
<ContentPage xmlns:picker="clr-namespace:Syncfusion.Maui.Picker;assembly=Syncfusion.Maui.Picker"
             FlowDirection="RightToLeft">
    
    <picker:SfTimePicker 
        Format="hh_mm_tt"
        SelectedTime="15:30:00">
        
        <picker:SfTimePicker.HeaderView>
            <picker:PickerHeaderView Text="اختر الوقت" />
        </picker:SfTimePicker.HeaderView>
        
        <picker:SfTimePicker.FooterView>
            <picker:PickerFooterView 
                OkButtonText="موافق"
                CancelButtonText="إلغاء" />
        </picker:SfTimePicker.FooterView>
    </picker:SfTimePicker>
</ContentPage>
```

### RTL with Culture

```csharp
public class RTLTimePickerPage : ContentPage
{
    public RTLTimePickerPage()
    {
        // Set Arabic culture
        var arabicCulture = new CultureInfo("ar-SA");
        CultureInfo.CurrentUICulture = arabicCulture;
        CultureInfo.CurrentCulture = arabicCulture;
        
        // Enable RTL flow direction
        FlowDirection = FlowDirection.RightToLeft;
        
        var timePicker = new SfTimePicker
        {
            Format = PickerFormat.hh_mm_tt,
            SelectedTime = new TimeSpan(15, 30, 0),
            HeaderView = new PickerHeaderView 
            { 
                Text = "اختر الوقت" // "Select Time" in Arabic
            },
            FooterView = new PickerFooterView
            {
                OkButtonText = "موافق", // "OK" in Arabic
                CancelButtonText = "إلغاء" // "Cancel" in Arabic
            },
            ColumnHeaderView = new TimePickerColumnHeaderView
            {
                HourHeaderText = "ساعة",
                MinuteHeaderText = "دقيقة",
                SecondHeaderText = "ثانية",
                MeridiemHeaderText = "ص/م"
            }
        };
        
        Content = new StackLayout
        {
            Children = { timePicker },
            Padding = 20
        };
    }
}
```

### Hebrew RTL Example

```csharp
public class HebrewTimePickerPage : ContentPage
{
    public HebrewTimePickerPage()
    {
        var hebrewCulture = new CultureInfo("he-IL");
        CultureInfo.CurrentUICulture = hebrewCulture;
        CultureInfo.CurrentCulture = hebrewCulture;
        
        FlowDirection = FlowDirection.RightToLeft;
        
        var timePicker = new SfTimePicker
        {
            Format = PickerFormat.HH_mm,
            SelectedTime = new TimeSpan(15, 30, 0),
            HeaderView = new PickerHeaderView 
            { 
                Text = "בחר שעה" // "Select Time" in Hebrew
            },
            FooterView = new PickerFooterView
            {
                OkButtonText = "אישור", // "OK" in Hebrew
                CancelButtonText = "ביטול" // "Cancel" in Hebrew
            },
            ColumnHeaderView = new TimePickerColumnHeaderView
            {
                HourHeaderText = "שעה",
                MinuteHeaderText = "דקה",
                SecondHeaderText = "שנייה"
            }
        };
        
        Content = new StackLayout
        {
            Children = { timePicker },
            Padding = 20
        };
    }
}
```

## Best Practices

1. **Always provide semantic descriptions** for screen reader users
2. **Use high contrast colors** to meet WCAG standards
3. **Test with screen readers** on target platforms (TalkBack, VoiceOver)
4. **Localize all user-facing text** including headers, buttons, and hints
5. **Support RTL languages** when targeting international markets
6. **Use culture-appropriate time formats** automatically based on CurrentCulture
7. **Provide keyboard navigation** for desktop platforms
8. **Test with accessibility tools** to ensure compliance

## Common Scenarios

### Global Medical Appointment System

```csharp
public class GlobalMedicalAppointmentPage : ContentPage
{
    private SfTimePicker timePicker;
    
    public GlobalMedicalAppointmentPage()
    {
        // Detect user's preferred culture
        var userCulture = CultureInfo.CurrentUICulture;
        
        timePicker = new SfTimePicker
        {
            Format = userCulture.Name.StartsWith("en-") 
                ? PickerFormat.hh_mm_tt 
                : PickerFormat.HH_mm,
            SelectedTime = new TimeSpan(9, 0, 0),
            MinimumTime = new TimeSpan(9, 0, 0),
            MaximumTime = new TimeSpan(17, 0, 0),
            SemanticProperties =
            {
                Description = "Medical appointment time",
                Hint = "Business hours: 9 AM to 5 PM"
            }
        };
        
        // Apply RTL if needed
        if (userCulture.TextInfo.IsRightToLeft)
        {
            FlowDirection = FlowDirection.RightToLeft;
        }
        
        Content = new StackLayout
        {
            Children = { timePicker },
            Padding = 20
        };
    }
}
```

This comprehensive accessibility and localization support ensures the TimePicker control works seamlessly for users worldwide, regardless of language, culture, or accessibility needs.
