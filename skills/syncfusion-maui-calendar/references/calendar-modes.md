# Calendar Modes and Display Options

This guide covers the different display modes for the Syncfusion .NET MAUI Calendar, including inline display, dialog popup, and relative positioning options.

## Table of Contents
- [Overview](#overview)
- [Default Mode](#default-mode)
- [Dialog Mode](#dialog-mode)
- [Relative Dialog Mode](#relative-dialog-mode)
- [Custom Popup Size](#custom-popup-size)
- [Common Scenarios](#common-scenarios)

## Overview

The `SfCalendar` supports three display modes through the `Mode` property:

- **Default**: Calendar displays inline within the layout
- **Dialog**: Calendar displays as a centered popup dialog
- **RelativeDialog**: Calendar displays as a popup positioned relative to a reference element

**CalendarMode Enum Values:**
- `CalendarMode.Default` (default)
- `CalendarMode.Dialog`
- `CalendarMode.RelativeDialog`

## Default Mode

The default mode displays the calendar inline as part of your page layout. This is the standard behavior when no mode is specified.

### Implementation

{% tabs %}
{% highlight xaml tabtitle="XAML" %}

<calendar:SfCalendar x:Name="calendar"
                     Mode="Default"/>

{% endhighlight %}
{% highlight c# tabtitle="C#" %}

SfCalendar calendar = new SfCalendar()
{
    Mode = CalendarMode.Default
};

this.Content = calendar;

{% endhighlight %}
{% endtabs %}

**When to Use:**
- Calendar is always visible on the page
- Part of a dashboard or main screen layout
- No need for show/hide functionality

## Dialog Mode

Dialog mode displays the calendar as a centered popup overlay. The calendar appears over the current page content with a dimmed background.

### Basic Dialog Setup

{% tabs %}
{% highlight xaml tabtitle="XAML" %}

<calendar:SfCalendar x:Name="calendar"
                     Mode="Dialog"/>

{% endhighlight %}
{% highlight c# tabtitle="C#" %}

SfCalendar calendar = new SfCalendar()
{
    Mode = CalendarMode.Dialog
};

this.Content = calendar;

{% endhighlight %}
{% endtabs %}

### Opening and Closing the Dialog

Use the `IsOpen` property to programmatically control the dialog visibility.

{% tabs %}
{% highlight xaml tabtitle="MainPage.xaml" %}

<Grid>
    <calendar:SfCalendar x:Name="calendar"
                         Mode="Dialog"/>
    
    <Button Text="Open Calendar" 
            x:Name="calendarButton"
            Clicked="Button_Clicked"
            HorizontalOptions="Center"
            VerticalOptions="Center"
            HeightRequest="50" 
            WidthRequest="150">
    </Button>
</Grid>

{% endhighlight %}
{% highlight c# tabtitle="MainPage.xaml.cs" %}

private void Button_Clicked(object sender, System.EventArgs e)
{
    this.calendar.IsOpen = true;
}

{% endhighlight %}
{% endtabs %}

**IsOpen Property:**
- Type: `bool`
- Default: `false`
- Automatically set to `false` when user clicks outside the dialog

### Dialog with Selection Confirmation

{% tabs %}
{% highlight c# tabtitle="C#" %}

// Open dialog
private void OpenCalendar_Clicked(object sender, EventArgs e)
{
    calendar.IsOpen = true;
}

// Handle selection and close
private void calendar_SelectionChanged(object sender, CalendarSelectionChangedEventArgs e)
{
    // Process the selected date
    DateTime selectedDate = e.NewValue as DateTime? ?? DateTime.Today;
    
    // Close the dialog after selection
    calendar.IsOpen = false;
    
    // Display the selected date
    DisplayAlert("Date Selected", selectedDate.ToShortDateString(), "OK");
}

{% endhighlight %}
{% endtabs %}

**When to Use:**
- Date picker functionality
- Calendar should not always be visible
- User needs to select a date then return to main content
- Space-constrained layouts

## Relative Dialog Mode

Relative dialog mode displays the calendar as a popup positioned relative to a specific location. This is useful for dropdown-style calendars or context menus.

### Relative Position Options

The `RelativePosition` property controls where the calendar appears:

**CalendarRelativePosition Enum:**
- `AlignTop` (default): Above the reference point
- `AlignBottom`: Below the reference point
- `AlignToLeftOf`: To the left of the reference point
- `AlignToRightOf`: To the right of the reference point
- `AlignTopLeft`: Top-left corner alignment
- `AlignTopRight`: Top-right corner alignment
- `AlignBottomLeft`: Bottom-left corner alignment
- `AlignBottomRight`: Bottom-right corner alignment

### Basic Relative Dialog

{% tabs %}
{% highlight xaml tabtitle="MainPage.xaml" %}

<Grid>
    <calendar:SfCalendar x:Name="calendar" 
                         Mode="RelativeDialog"
                         RelativePosition="AlignTopLeft">
    </calendar:SfCalendar>
    
    <Button Text="Open Calendar" 
            x:Name="calendarButton"
            Clicked="Button_Clicked"
            HorizontalOptions="Center"
            VerticalOptions="Center"
            HeightRequest="50" 
            WidthRequest="150">
    </Button>
</Grid>

{% endhighlight %}
{% highlight c# tabtitle="MainPage.xaml.cs" %}

private void Button_Clicked(object sender, System.EventArgs e)
{
    this.calendar.IsOpen = true;
}

{% endhighlight %}
{% endtabs %}

### Dropdown Calendar Example

Create a calendar that appears below a button, like a date picker dropdown:

{% tabs %}
{% highlight xaml tabtitle="XAML" %}

<Grid>
    <calendar:SfCalendar x:Name="calendar" 
                         Mode="RelativeDialog"
                         RelativePosition="AlignBottom">
    </calendar:SfCalendar>
    
    <Button Text="Select Date" 
            x:Name="datePickerButton"
            Clicked="ShowCalendar_Clicked"
            HorizontalOptions="Start"
            VerticalOptions="Start"
            Margin="20">
    </Button>
</Grid>

{% endhighlight %}
{% highlight c# tabtitle="C#" %}

private void ShowCalendar_Clicked(object sender, EventArgs e)
{
    calendar.IsOpen = true;
}

{% endhighlight %}
{% endtabs %}

### All Position Examples

{% tabs %}
{% highlight c# tabtitle="C#" %}

// Align above button
calendar.RelativePosition = CalendarRelativePosition.AlignTop;

// Align below button
calendar.RelativePosition = CalendarRelativePosition.AlignBottom;

// Align to left side
calendar.RelativePosition = CalendarRelativePosition.AlignToLeftOf;

// Align to right side
calendar.RelativePosition = CalendarRelativePosition.AlignToRightOf;

// Align to top-left corner
calendar.RelativePosition = CalendarRelativePosition.AlignTopLeft;

// Align to top-right corner
calendar.RelativePosition = CalendarRelativePosition.AlignTopRight;

// Align to bottom-left corner
calendar.RelativePosition = CalendarRelativePosition.AlignBottomLeft;

// Align to bottom-right corner
calendar.RelativePosition = CalendarRelativePosition.AlignBottomRight;

{% endhighlight %}
{% endtabs %}

**When to Use:**
- Dropdown date picker controls
- Context-sensitive date selection
- Alignment near form fields
- Custom UI layouts requiring specific positioning

## Custom Popup Size

For both Dialog and RelativeDialog modes, you can customize the popup dimensions using `PopupWidth` and `PopupHeight` properties.

### Setting Custom Dimensions

{% tabs %}
{% highlight xaml tabtitle="XAML" %}

<calendar:SfCalendar x:Name="calendar" 
                     Mode="Dialog"
                     PopupWidth="300"
                     PopupHeight="400"/>

{% endhighlight %}
{% highlight c# tabtitle="C#" %}

calendar.Mode = CalendarMode.Dialog;
calendar.PopupWidth = 300;
calendar.PopupHeight = 400;

{% endhighlight %}
{% endtabs %}

### Responsive Popup Size

Adjust popup size based on device:

{% tabs %}
{% highlight c# tabtitle="C#" %}

if (DeviceInfo.Current.Platform == DevicePlatform.Android || 
    DeviceInfo.Current.Platform == DevicePlatform.iOS)
{
    // Smaller popup for mobile
    calendar.PopupWidth = 350;
    calendar.PopupHeight = 400;
}
else
{
    // Larger popup for desktop
    calendar.PopupWidth = 450;
    calendar.PopupHeight = 500;
}

{% endhighlight %}
{% endtabs %}

**Properties:**
- `PopupWidth`: double (default: auto-sized)
- `PopupHeight`: double (default: auto-sized)

## Common Scenarios

### Date Picker with Button Trigger

{% tabs %}
{% highlight xaml tabtitle="XAML" %}

<Grid>
    <calendar:SfCalendar x:Name="calendar"
                         Mode="Dialog"
                         SelectionMode="Single"
                         SelectionChanged="Calendar_SelectionChanged"/>
    
    <StackLayout VerticalOptions="Center" HorizontalOptions="Center">
        <Label Text="Selected Date:" FontSize="16"/>
        <Label x:Name="selectedDateLabel" 
               Text="No date selected" 
               FontSize="14" 
               FontAttributes="Bold"/>
        <Button Text="Pick Date" 
                Clicked="OpenCalendar_Clicked"
                Margin="0,10,0,0"/>
    </StackLayout>
</Grid>

{% endhighlight %}
{% highlight c# tabtitle="C#" %}

private void OpenCalendar_Clicked(object sender, EventArgs e)
{
    calendar.IsOpen = true;
}

private void Calendar_SelectionChanged(object sender, CalendarSelectionChangedEventArgs e)
{
    if (e.NewValue is DateTime selectedDate)
    {
        selectedDateLabel.Text = selectedDate.ToString("MMMM dd, yyyy");
        calendar.IsOpen = false;
    }
}

{% endhighlight %}
{% endtabs %}

### Dropdown Calendar Next to Input

{% tabs %}
{% highlight xaml tabtitle="XAML" %}

<StackLayout Orientation="Horizontal" Margin="20">
    <calendar:SfCalendar x:Name="calendar"
                         Mode="RelativeDialog"
                         RelativePosition="AlignBottom"/>
    
    <Entry x:Name="dateEntry" 
           Placeholder="Select date"
           IsReadOnly="True"
           WidthRequest="200"/>
    
    <Button Text="📅" 
            Clicked="ShowCalendar_Clicked"
            WidthRequest="50"/>
</StackLayout>

{% endhighlight %}
{% highlight c# tabtitle="C#" %}

private void ShowCalendar_Clicked(object sender, EventArgs e)
{
    calendar.IsOpen = true;
}

{% endhighlight %}
{% endtabs %}

### Always-Visible Calendar Dashboard

{% tabs %}
{% highlight xaml tabtitle="XAML" %}

<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    
    <Label Text="Schedule Dashboard" 
           FontSize="24" 
           Margin="20"
           Grid.Row="0"/>
    
    <calendar:SfCalendar Mode="Default"
                         SelectionMode="Multiple"
                         Grid.Row="1"
                         Margin="20"/>
</Grid>

{% endhighlight %}
{% endtabs %}

### Programmatic Dialog Control

{% tabs %}
{% highlight c# tabtitle="C#" %}

// Open dialog
public void OpenCalendar()
{
    calendar.IsOpen = true;
}

// Close dialog
public void CloseCalendar()
{
    calendar.IsOpen = false;
}

// Toggle dialog
public void ToggleCalendar()
{
    calendar.IsOpen = !calendar.IsOpen;
}

// Check if dialog is open
public bool IsCalendarOpen()
{
    return calendar.IsOpen;
}

{% endhighlight %}
{% endtabs %}

## Best Practices

**Mode Selection:**
- Use **Default** for always-visible calendars (dashboards, schedulers)
- Use **Dialog** for modal date selection (date pickers, forms)
- Use **RelativeDialog** for dropdown-style date pickers

**IsOpen Management:**
- Close dialog after user selects a date (unless multi-selection)
- Don't rely on `IsOpen = true` in XAML; control it programmatically
- Listen for automatic closure when user clicks outside

**Popup Sizing:**
- Test popup sizes on different devices and orientations
- Use responsive sizing for mobile vs desktop
- Ensure popup doesn't exceed screen bounds

**Relative Positioning:**
- Choose position based on available screen space
- Consider RTL layouts when using left/right positions
- Test positioning on different screen sizes
