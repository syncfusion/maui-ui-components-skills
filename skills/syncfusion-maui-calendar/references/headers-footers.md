# Headers and Footers in .NET MAUI Calendar

The SfCalendar provides customizable header and footer views. This guide covers all header and footer customization options including appearance, text formatting, navigation arrows, action buttons, and custom templates.

## Table of Contents
- [Header View](#header-view)
- [Header Templates](#header-templates)
- [Footer View](#footer-view)
- [Footer Templates](#footer-templates)

## Header View

Customize the calendar header using the `HeaderView` property. The header displays the current month/year and optional navigation arrows.

### Header Height

**XAML:**
```xml
<calendar:SfCalendar x:Name="calendar" View="Month">
    <calendar:SfCalendar.HeaderView>
        <calendar:CalendarHeaderView Height="70" />
    </calendar:SfCalendar.HeaderView>
</calendar:SfCalendar>
```

**C#:**
```csharp
calendar.HeaderView = new CalendarHeaderView
{
    Height = 70
};
```

### Header Appearance

Customize background, text style, and text format.

**XAML:**
```xml
<calendar:SfCalendar x:Name="calendar" View="Month">
    <calendar:SfCalendar.HeaderView>
        <calendar:CalendarHeaderView Background="LightBlue" 
                                     TextFormat="MMM yyyy"
                                     ShowNavigationArrows="True">
            <calendar:CalendarHeaderView.TextStyle>
                <calendar:CalendarTextStyle TextColor="DarkBlue" 
                                           FontSize="16"
                                           FontAttributes="Bold" />
            </calendar:CalendarHeaderView.TextStyle>
        </calendar:CalendarHeaderView>
    </calendar:SfCalendar.HeaderView>
</calendar:SfCalendar>
```

**C#:**
```csharp
CalendarTextStyle headerTextStyle = new CalendarTextStyle
{
    TextColor = Colors.DarkBlue,
    FontSize = 16,
    FontAttributes = FontAttributes.Bold
};

calendar.HeaderView = new CalendarHeaderView
{
    Background = Colors.LightBlue,
    TextFormat = "MMM yyyy",
    ShowNavigationArrows = true,
    TextStyle = headerTextStyle
};
```

### Header Text Format

Customize the date format displayed in the header using standard .NET date format strings.

**Common Formats:**
```csharp
// "March 2026"
calendar.HeaderView.TextFormat = "MMMM yyyy";

// "Mar 2026"
calendar.HeaderView.TextFormat = "MMM yyyy";

// "03/2026"
calendar.HeaderView.TextFormat = "MM/yyyy";

// "March, 2026"
calendar.HeaderView.TextFormat = "MMMM, yyyy";

// Custom format
calendar.HeaderView.TextFormat = "MMMM dd, yyyy";
```

**Format Specifiers:**
- `MMMM`: Full month name (January, February, etc.)
- `MMM`: Abbreviated month name (Jan, Feb, etc.)
- `MM`: Month as number (01, 02, etc.)
- `yyyy`: Four-digit year (2026)
- `yy`: Two-digit year (26)

### Show Navigation Arrows

Display left/right navigation arrows in the header.

**XAML:**
```xml
<calendar:SfCalendar.HeaderView>
    <calendar:CalendarHeaderView ShowNavigationArrows="True" />
</calendar:SfCalendar.HeaderView>
```

**C#:**
```csharp
calendar.HeaderView.ShowNavigationArrows = true;
```

**Default:** `false`

When enabled, users can tap arrows to navigate to previous/next months without swiping.

### Complete Header Example

```csharp
CalendarTextStyle headerTextStyle = new CalendarTextStyle
{
    TextColor = Colors.White,
    FontSize = 18,
    FontFamily = "Arial",
    FontAttributes = FontAttributes.Bold
};

calendar.HeaderView = new CalendarHeaderView
{
    Height = 60,
    Background = Colors.DarkBlue,
    TextFormat = "MMMM yyyy",
    ShowNavigationArrows = true,
    TextStyle = headerTextStyle
};
```

## Header Templates

Create completely custom headers using `HeaderTemplate`. This provides full control over header layout and content.

### Basic Header Template

**XAML:**
```xml
<calendar:SfCalendar x:Name="calendar" View="Month">
    <calendar:SfCalendar.HeaderTemplate>
        <DataTemplate>
            <Grid Background="Purple" Padding="10">
                <Label x:Name="headerLabel" 
                       TextColor="White" 
                       FontSize="18"
                       FontAttributes="Bold"
                       HorizontalOptions="Center" 
                       VerticalOptions="Center">
                    <Label.Text>
                        <MultiBinding StringFormat="{}{0:MMMM yyyy}">
                            <Binding Path="DisplayDate" Source="{x:Reference calendar}" />
                        </MultiBinding>
                    </Label.Text>
                </Label>
            </Grid>
        </DataTemplate>
    </calendar:SfCalendar.HeaderTemplate>
</calendar:SfCalendar>
```

### Advanced Header Template with Custom Navigation

```xml
<calendar:SfCalendar x:Name="calendar" View="Month">
    <calendar:SfCalendar.HeaderTemplate>
        <DataTemplate>
            <Grid ColumnDefinitions="Auto,*,Auto" 
                  Background="DeepSkyBlue" 
                  Padding="10,5">
                
                <!-- Previous Button -->
                <Button Grid.Column="0" 
                        Text="◀" 
                        BackgroundColor="Transparent"
                        TextColor="White"
                        Clicked="OnPreviousMonthClicked" />
                
                <!-- Month/Year Label -->
                <Label Grid.Column="1" 
                       TextColor="White" 
                       FontSize="18"
                       FontAttributes="Bold"
                       HorizontalOptions="Center" 
                       VerticalOptions="Center">
                    <Label.Text>
                        <MultiBinding StringFormat="{}{0:MMMM yyyy}">
                            <Binding Path="DisplayDate" Source="{x:Reference calendar}" />
                        </MultiBinding>
                    </Label.Text>
                </Label>
                
                <!-- Next Button -->
                <Button Grid.Column="2" 
                        Text="▶" 
                        BackgroundColor="Transparent"
                        TextColor="White"
                        Clicked="OnNextMonthClicked" />
            </Grid>
        </DataTemplate>
    </calendar:SfCalendar.HeaderTemplate>
</calendar:SfCalendar>
```

```csharp
private void OnPreviousMonthClicked(object sender, EventArgs e)
{
    calendar.DisplayDate = calendar.DisplayDate.AddMonths(-1);
}

private void OnNextMonthClicked(object sender, EventArgs e)
{
    calendar.DisplayDate = calendar.DisplayDate.AddMonths(1);
}
```

### Header Template with Picker

```xml
<calendar:SfCalendar.HeaderTemplate>
    <DataTemplate>
        <Grid Background="Teal" Padding="10">
            <Picker x:Name="monthYearPicker"
                    TextColor="White"
                    TitleColor="White"
                    SelectedIndexChanged="OnMonthYearPickerChanged">
                <!-- Populated with month/year options -->
            </Picker>
        </Grid>
    </DataTemplate>
</calendar:SfCalendar.HeaderTemplate>
```

## Footer View

Customize the calendar footer using the `FooterView` property. The footer can display action buttons (OK/Cancel) and a Today button.

### Footer Height

**XAML:**
```xml
<calendar:SfCalendar x:Name="calendar" View="Month">
    <calendar:SfCalendar.FooterView>
        <calendar:CalendarFooterView Height="70" 
                                     ShowActionButtons="True"
                                     ShowTodayButton="True" />
    </calendar:SfCalendar.FooterView>
</calendar:SfCalendar>
```

**C#:**
```csharp
calendar.FooterView = new CalendarFooterView
{
    Height = 70,
    ShowActionButtons = true,
    ShowTodayButton = true
};
```

### Show Action Buttons

Display OK and Cancel buttons in the footer.

**XAML:**
```xml
<calendar:SfCalendar.FooterView>
    <calendar:CalendarFooterView ShowActionButtons="True" />
</calendar:SfCalendar.FooterView>
```

**C#:**
```csharp
calendar.FooterView.ShowActionButtons = true;
```

**Default:** `false`

**Behavior:**
- **OK Button:** Confirms the current selection and can trigger custom logic
- **Cancel Button:** Cancels the selection and reverts to previous state

### Show Today Button

Display a button to quickly navigate to today's date.

**XAML:**
```xml
<calendar:SfCalendar.FooterView>
    <calendar:CalendarFooterView ShowTodayButton="True" />
</calendar:SfCalendar.FooterView>
```

**C#:**
```csharp
calendar.FooterView.ShowTodayButton = true;
```

**Default:** `false`

When tapped, the calendar navigates to the month containing today's date.

### Footer Appearance

Customize footer background, text style, and divider color.

**XAML:**
```xml
<calendar:SfCalendar.FooterView>
    <calendar:CalendarFooterView Background="LightGray"
                                 DividerColor="DarkGray"
                                 ShowActionButtons="True">
        <calendar:CalendarFooterView.TextStyle>
            <calendar:CalendarTextStyle TextColor="DarkBlue" 
                                       FontSize="14"
                                       FontAttributes="Bold" />
        </calendar:CalendarFooterView.TextStyle>
    </calendar:CalendarFooterView>
</calendar:SfCalendar.FooterView>
```

**C#:**
```csharp
calendar.FooterView = new CalendarFooterView
{
    Background = Colors.LightGray,
    DividerColor = Colors.DarkGray,
    ShowActionButtons = true,
    TextStyle = new CalendarTextStyle
    {
        TextColor = Colors.DarkBlue,
        FontSize = 14,
        FontAttributes = FontAttributes.Bold
    }
};
```

### Complete Footer Example

```csharp
calendar.FooterView = new CalendarFooterView
{
    Height = 60,
    Background = Colors.WhiteSmoke,
    DividerColor = Colors.Gray,
    ShowActionButtons = true,
    ShowTodayButton = true,
    TextStyle = new CalendarTextStyle
    {
        TextColor = Colors.Black,
        FontSize = 14,
        FontAttributes = FontAttributes.Bold
    }
};
```

## Best Practices

1. **Keep Headers Concise:** Show only essential information (month/year)
2. **Use Clear Formatting:** Choose readable text formats for your audience
3. **Provide Navigation Options:** Enable arrows or custom buttons for easy navigation
4. **Test on All Platforms:** Header/footer appearance may vary across iOS, Android, Windows
5. **Consider Localization:** Use appropriate date formats for different cultures
6. **Action Buttons for Confirmations:** Use footer action buttons for date picker scenarios
7. **Today Button for Convenience:** Help users quickly return to current date
8. **Custom Templates for Branding:** Use templates to match your app's design language
