# Basic Features in .NET MAUI Numeric Entry

This guide covers essential configuration and feature properties for the Numeric Entry control.

## Table of Contents
- [Setting Placeholder Text](#setting-placeholder-text)
- [Clear Button Visibility](#clear-button-visibility)
- [Clear Button Color](#clear-button-color)
- [Clear Button Customization](#clear-button-customization)
- [Value Changed Notification](#value-changed-notification)
- [Value Change Mode](#value-change-mode)
- [Completed Event](#completed-event)
- [ClearButtonClicked Event](#clearbuttonclicked-event)
- [Stroke (Border Color)](#stroke-border-color)
- [Border Visibility](#border-visibility)
- [Text Alignment](#text-alignment)
- [Select Text on Focus](#select-text-on-focus)
- [Return Type](#return-type)
- [Return Command](#return-command)
- [Automation ID](#automation-id)

## Setting Placeholder Text

The `Placeholder` property displays hint text when the control is empty or null. Placeholder text only appears when `AllowNull` is `true` and the value is `null`.

### Basic Placeholder

```xml
<editors:SfNumericEntry WidthRequest="200"
                        Placeholder="Enter input here..." />
```

```csharp
var numericEntry = new SfNumericEntry
{
    WidthRequest = 200,
    Placeholder = "Enter input here..."
};
```

### Placeholder with Format Hint

```xml
<editors:SfNumericEntry Placeholder="$0.00" CustomFormat="C2" />
<editors:SfNumericEntry Placeholder="Enter percentage" CustomFormat="P0" />
<editors:SfNumericEntry Placeholder="0.00" CustomFormat="N2" />
```

**Default value:** `string.Empty` (no placeholder shown)

**When it appears:**
- Value is `null`
- `AllowNull` is `true`
- Control does not have focus or is empty

**When it disappears:**
- User starts typing
- Value is set programmatically
- `AllowNull` is `false`

## Clear Button Visibility

The `ShowClearButton` property controls whether the clear button (X icon) is displayed.

### Enable Clear Button (Default)

```xml
<editors:SfNumericEntry WidthRequest="200"
                        ShowClearButton="True"
                        IsEditable="True"
                        Value="10" />
```

```csharp
var numericEntry = new SfNumericEntry
{
    WidthRequest = 200,
    ShowClearButton = true,
    IsEditable = true,
    Value = 10
};
```

### Disable Clear Button

```xml
<editors:SfNumericEntry WidthRequest="200"
                        ShowClearButton="False"
                        Value="10" />
```

**Default value:** `true` (clear button enabled)

**Important conditions:**
- Clear button appears only when `IsEditable="True"`
- Clear button appears only when control has focus
- Clicking clear button sets value to `null` (if `AllowNull=true`) or `0`/`Minimum` (if `AllowNull=false`)

## Clear Button Color

The `ClearButtonColor` property sets the color of the clear button icon.

```xml
<editors:SfNumericEntry WidthRequest="200"
                        ShowClearButton="True"
                        ClearButtonColor="Red" />
```

```csharp
var numericEntry = new SfNumericEntry
{
    WidthRequest = 200,
    ShowClearButton = true,
    ClearButtonColor = Colors.Red
};
```

**Use case:** Match your app's theme or make the clear button more prominent

## Clear Button Customization

The `ClearButtonPath` property allows you to customize the clear button's appearance with a custom path geometry.

```xml
<editors:SfNumericEntry x:Name="numericEntry"
                        WidthRequest="200"
                        ShowClearButton="True"
                        IsEditable="True"
                        Value="10">
    <editors:SfNumericEntry.ClearButtonPath>
        <Path Data="M1.70711 0.292893C1.31658 -0.097631 0.683417 -0.097631 0.292893 0.292893C-0.097631 0.683417 -0.097631 1.31658 0.292893 1.70711L5.58579 7L0.292893 12.2929C-0.097631 12.6834 -0.097631 13.3166 0.292893 13.7071C0.683417 14.0976 1.31658 14.0976 1.70711 13.7071L7 8.41421L12.2929 13.7071C12.6834 14.0976 13.3166 14.0976 13.7071 13.7071C14.0976 13.3166 14.0976 12.6834 13.7071 12.2929L8.41421 7L13.7071 1.70711C14.0976 1.31658 14.0976 0.683417 13.7071 0.292893C13.3166 -0.097631 12.6834 -0.097631 12.2929 0.292893L7 5.58579L1.70711 0.292893Z"
              Fill="Red"
              Stroke="Red" />
    </editors:SfNumericEntry.ClearButtonPath>
</editors:SfNumericEntry>
```

```csharp
var customPath = "M1.70711 0.292893C1.31658 -0.097631 0.683417 -0.097631 0.292893 0.292893C-0.097631 0.683417 -0.097631 1.31658 0.292893 1.70711L5.58579 7L0.292893 12.2929C-0.097631 12.6834 -0.097631 13.3166 0.292893 13.7071C0.683417 14.0976 1.31658 14.0976 1.70711 13.7071L7 8.41421L12.2929 13.7071C12.6834 14.0976 13.3166 14.0976 13.7071 13.7071C14.0976 13.3166 14.0976 12.6834 13.7071 12.2929L8.41421 7L13.7071 1.70711C14.0976 1.31658 14.0976 0.683417 13.7071 0.292893C13.3166 -0.097631 12.6834 -0.097631 12.2929 0.292893L7 5.58579L1.70711 0.292893Z";

var converter = new PathGeometryConverter();
var path = new Path
{
    Data = (PathGeometry)converter.ConvertFromInvariantString(customPath),
    Fill = Colors.Red,
    Stroke = Colors.Red
};

var numericEntry = new SfNumericEntry
{
    WidthRequest = 200,
    Value = 10,
    ShowClearButton = true,
    IsEditable = true,
    ClearButtonPath = path
};
```

**Use case:** Custom icon designs, branding consistency, unique UI styling

## Value Changed Notification

The `ValueChanged` event is triggered when the `Value` property changes. The value changes after validation on:
- **Enter key press**
- **Focus loss** (user clicks outside or tabs away)

**Note:** The event is NOT fired while the user is typing. It fires only after validation.

### ValueChanged Event Handler

```xml
<editors:SfNumericEntry WidthRequest="200"
                        ValueChanged="OnValueChanged" />
```

```csharp
private void OnValueChanged(object sender, NumericEntryValueChangedEventArgs e)
{
    var oldValue = e.OldValue;
    var newValue = e.NewValue;
    
    Console.WriteLine($"Value changed from {oldValue} to {newValue}");
}
```

### NumericEntryValueChangedEventArgs Properties

| Property | Type | Description |
|----------|------|-------------|
| `OldValue` | double? | Previous value before change |
| `NewValue` | double? | New value after change |

### Example: Calculate Total

```xml
<VerticalStackLayout Spacing="10">
    <editors:SfNumericEntry x:Name="quantityEntry"
                            Placeholder="Quantity"
                            Value="1"
                            ValueChanged="CalculateTotal" />
    
    <editors:SfNumericEntry x:Name="priceEntry"
                            Placeholder="Price"
                            CustomFormat="C2"
                            Value="10"
                            ValueChanged="CalculateTotal" />
    
    <Label x:Name="totalLabel" Text="Total: $0.00" FontSize="18" FontAttributes="Bold" />
</VerticalStackLayout>
```

```csharp
private void CalculateTotal(object sender, NumericEntryValueChangedEventArgs e)
{
    var quantity = quantityEntry.Value ?? 0;
    var price = priceEntry.Value ?? 0;
    var total = quantity * price;
    
    totalLabel.Text = $"Total: {total:C2}";
}
```

## Value Change Mode

The `ValueChangeMode` property determines **when** the value is updated and the `ValueChanged` event is fired.

### OnLostFocus (Default)

Value updates when focus is lost or Enter key is pressed.

```xml
<editors:SfNumericEntry WidthRequest="200"
                        ValueChangeMode="OnLostFocus"
                        ValueChanged="OnValueChanged" />
```

**Behavior:**
- User types "123"
- Value remains unchanged while typing
- User presses Enter or clicks outside
- Value updates to 123
- ValueChanged event fires

**Use case:** Form validation, batch updates, reduce event noise

### OnKeyFocus

Value updates with **each keystroke**.

```xml
<editors:SfNumericEntry WidthRequest="200"
                        ValueChangeMode="OnKeyFocus"
                        ValueChanged="OnValueChanged" />
```

**Behavior:**
- User types "1" → Value=1, event fires
- User types "2" → Value=12, event fires
- User types "3" → Value=123, event fires

**Use case:** Real-time calculations, live totals, instant feedback

### Example: Real-Time Dollar Display

```xml
<VerticalStackLayout Spacing="10" VerticalOptions="Center">
    <editors:SfNumericEntry x:Name="numericEntry"
                            WidthRequest="200"
                            HeightRequest="40"
                            VerticalOptions="Center"
                            ValueChangeMode="OnKeyFocus"
                            Value="50"
                            ValueChanged="OnRealtimeValueChanged" />
    
    <HorizontalStackLayout Spacing="5" HeightRequest="40" WidthRequest="200">
        <Label Text="Dollar:" />
        <Label x:Name="valueDisplay"
               TextColor="Green"
               Text="$50.00"
               HeightRequest="40" />
    </HorizontalStackLayout>
</VerticalStackLayout>
```

```csharp
private void OnRealtimeValueChanged(object sender, NumericEntryValueChangedEventArgs e)
{
    valueDisplay.Text = $"${e.NewValue:F2}";
}
```

## Completed Event

The `Completed` event is raised when the user presses the **return key** on the keyboard while the Numeric Entry has focus.

```xml
<editors:SfNumericEntry x:Name="numericEntry"
                        WidthRequest="200"
                        Value="153"
                        Completed="OnCompleted" />
```

```csharp
private async void OnCompleted(object sender, EventArgs e)
{
    await DisplayAlert("Message", "Input completed", "OK");
    
    // Example: Move focus to next control
    nextEntry.Focus();
}
```

**Use cases:**
- Submit form on Enter
- Move to next input field
- Trigger calculations
- Show confirmation message

## ClearButtonClicked Event

The `ClearButtonClicked` event is raised when the user clicks the clear button.

```xml
<editors:SfNumericEntry x:Name="numericEntry"
                        WidthRequest="200"
                        Value="153"
                        ShowClearButton="True"
                        ClearButtonClicked="OnClearButtonClicked" />
```

```csharp
private async void OnClearButtonClicked(object sender, EventArgs e)
{
    await DisplayAlert("Message", "Clear button clicked", "OK");
    
    // Example: Reset related fields
    quantityEntry.Value = 1;
    totalLabel.Text = "Total: $0.00";
}
```

**Use cases:**
- Reset form fields
- Clear calculations
- Log user actions
- Show confirmation before clearing

## Stroke (Border Color)

The `Stroke` property sets the border color of the Numeric Entry.

### Set Border Color

```xml
<editors:SfNumericEntry WidthRequest="200"
                        Stroke="Red" />
```

```csharp
var numericEntry = new SfNumericEntry
{
    WidthRequest = 200,
    Stroke = Colors.Red
};
```

**Default value:** `Black`

### Dynamic Border Color (Focus Indicator)

```xml
<editors:SfNumericEntry x:Name="numericEntry"
                        WidthRequest="200"
                        Stroke="Gray"
                        Focused="OnFocused"
                        Unfocused="OnUnfocused" />
```

```csharp
private void OnFocused(object sender, FocusEventArgs e)
{
    numericEntry.Stroke = Colors.Blue;
}

private void OnUnfocused(object sender, FocusEventArgs e)
{
    numericEntry.Stroke = Colors.Gray;
}
```

## Border Visibility

The `ShowBorder` property controls whether the border is displayed.

### Show Border (Default)

```xml
<editors:SfNumericEntry WidthRequest="200"
                        ShowBorder="True" />
```

### Hide Border

```xml
<editors:SfNumericEntry WidthRequest="200"
                        HeightRequest="40"
                        ShowBorder="False" />
```

```csharp
var numericEntry = new SfNumericEntry
{
    WidthRequest = 200,
    HeightRequest = 40,
    ShowBorder = false
};
```

**Default value:** `true` (border shown)

**Use case:** Borderless input for modern minimalist UI designs

## Text Alignment

Control text alignment with `HorizontalTextAlignment` and `VerticalTextAlignment` properties.

### Horizontal Alignment

```xml
<!-- Left aligned (default) -->
<editors:SfNumericEntry WidthRequest="200"
                        HorizontalTextAlignment="Start" />

<!-- Center aligned -->
<editors:SfNumericEntry WidthRequest="200"
                        HorizontalTextAlignment="Center" />

<!-- Right aligned -->
<editors:SfNumericEntry WidthRequest="200"
                        HorizontalTextAlignment="End" />
```

```csharp
numericEntry.HorizontalTextAlignment = TextAlignment.Center;
```

**Options:** `Start`, `Center`, `End`

**Note:** Dynamic changes to `HorizontalTextAlignment` may not work on Android platform.

### Vertical Alignment

```xml
<editors:SfNumericEntry WidthRequest="200"
                        HeightRequest="60"
                        VerticalTextAlignment="Start" />
```

```csharp
numericEntry.VerticalTextAlignment = TextAlignment.Start;
```

**Options:** `Start`, `Center`, `End`

### Combined Alignment

```xml
<editors:SfNumericEntry WidthRequest="200"
                        HeightRequest="50"
                        HorizontalTextAlignment="Center"
                        VerticalTextAlignment="Center" />
```

## Select Text on Focus

The `SelectAllOnFocus` property automatically selects all text when the control gains focus.

### Enable Auto-Select (Default)

```xml
<editors:SfNumericEntry WidthRequest="200"
                        Value="123456"
                        SelectAllOnFocus="True" />
```

**Behavior:** When user clicks or tabs into the field, all text is selected (ready to replace)

### Disable Auto-Select

```xml
<editors:SfNumericEntry WidthRequest="200"
                        Value="123456"
                        SelectAllOnFocus="False" />
```

```csharp
var numericEntry = new SfNumericEntry
{
    WidthRequest = 200,
    Value = 123456,
    SelectAllOnFocus = false
};
```

**Behavior:** Cursor appears at click position, text not selected

**Default value:** `true` (auto-select enabled)

**Use case:** 
- Enable: Quick replacement of entire value
- Disable: Editing specific digits within value

## Return Type

The `ReturnType` property specifies the return key button displayed on the keyboard.

### Available Return Types

```xml
<!-- Default return key -->
<editors:SfNumericEntry ReturnType="Default" />

<!-- Done button -->
<editors:SfNumericEntry ReturnType="Done" />

<!-- Go button -->
<editors:SfNumericEntry ReturnType="Go" />

<!-- Next button -->
<editors:SfNumericEntry ReturnType="Next" />

<!-- Search button -->
<editors:SfNumericEntry ReturnType="Search" />

<!-- Send button -->
<editors:SfNumericEntry ReturnType="Send" />
```

```csharp
numericEntry.ReturnType = ReturnType.Next;
```

**Default value:** `Default`

**Options:** `Default`, `Done`, `Go`, `Next`, `Search`, `Send`

### Example: Multi-Field Form

```xml
<VerticalStackLayout Spacing="10">
    <editors:SfNumericEntry x:Name="quantity"
                            Placeholder="Quantity"
                            ReturnType="Next"
                            Completed="FocusNext" />
    
    <editors:SfNumericEntry x:Name="price"
                            Placeholder="Price"
                            CustomFormat="C2"
                            ReturnType="Next"
                            Completed="FocusNext" />
    
    <editors:SfNumericEntry x:Name="discount"
                            Placeholder="Discount %"
                            ReturnType="Done"
                            Completed="SubmitForm" />
</VerticalStackLayout>
```

```csharp
private void FocusNext(object sender, EventArgs e)
{
    if (sender == quantity)
        price.Focus();
    else if (sender == price)
        discount.Focus();
}

private void SubmitForm(object sender, EventArgs e)
{
    // Submit form logic
}
```

## Return Command

The `ReturnCommand` and `ReturnCommandParameter` properties allow you to bind a command that executes when the return key is pressed.

### Command Binding

```xml
<ContentPage.BindingContext>
    <local:CommandDemoViewModel />
</ContentPage.BindingContext>

<editors:SfNumericEntry x:Name="numericEntry"
                        WidthRequest="200"
                        ReturnCommand="{Binding AlertCommand}"
                        ReturnCommandParameter="Return key pressed" />
```

```csharp
// ViewModel
public class CommandDemoViewModel
{
    public ICommand AlertCommand { get; }
    
    public CommandDemoViewModel()
    {
        AlertCommand = new Command<string>(OnAlertCommandExecuted);
    }
    
    private async void OnAlertCommandExecuted(string parameter)
    {
        await Application.Current.MainPage.DisplayAlert("Alert", parameter, "OK");
    }
}
```

### Programmatic Setup

```csharp
var viewModel = new CommandDemoViewModel();

var numericEntry = new SfNumericEntry
{
    WidthRequest = 200,
    ReturnCommand = viewModel.AlertCommand,
    ReturnCommandParameter = "Return key pressed"
};
```

**Use cases:**
- MVVM pattern form submission
- Navigation commands
- Validation commands
- Save/update operations

## Automation ID

The `AutomationId` property provides unique identifiers for UI automation testing. The Numeric Entry supports AutomationId for:
- The editable entry element
- The clear button element

### Set Automation ID

```xml
<editors:SfNumericEntry x:Name="employeeNumericEntry"
                        AutomationId="EmployeeNumericEntry"
                        WidthRequest="200" />
```

```csharp
numericEntry.AutomationId = "EmployeeNumericEntry";
```

### Generated AutomationIds

When you set `AutomationId="EmployeeNumericEntry"`, the control generates:

| Element | AutomationId |
|---------|--------------|
| Editable Entry | `"EmployeeNumericEntry Entry"` |
| Clear Button | `"EmployeeNumericEntry Clear Button"` |

**Example UI Test Code:**

```csharp
// Find the entry element
app.WaitForElement("EmployeeNumericEntry Entry");
app.EnterText("EmployeeNumericEntry Entry", "12345");

// Find and click clear button
app.Tap("EmployeeNumericEntry Clear Button");
```

**Use cases:**
- Automated UI testing (Xamarin.UITest, Appium)
- Accessibility features
- Screen readers
- QA automation

## Complete Example

Here's a comprehensive example using multiple basic features:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:editors="clr-namespace:Syncfusion.Maui.Inputs;assembly=Syncfusion.Maui.Inputs"
             x:Class="NumericEntryApp.BasicFeaturesPage">
    
    <ScrollView>
        <VerticalStackLayout Padding="20" Spacing="20">
            
            <!-- Price Entry with Validation -->
            <StackLayout Spacing="5">
                <Label Text="Price:" FontAttributes="Bold" />
                <editors:SfNumericEntry x:Name="priceEntry"
                                        WidthRequest="250"
                                        CustomFormat="C2"
                                        Placeholder="$0.00"
                                        PlaceholderColor="LightGray"
                                        ShowClearButton="True"
                                        ClearButtonColor="Red"
                                        Stroke="Gray"
                                        AllowNull="False"
                                        SelectAllOnFocus="True"
                                        HorizontalTextAlignment="End"
                                        ReturnType="Next"
                                        AutomationId="PriceEntry"
                                        ValueChanged="OnPriceChanged"
                                        Focused="OnEntryFocused"
                                        Unfocused="OnEntryUnfocused"
                                        ClearButtonClicked="OnClearClicked" />
            </StackLayout>
            
            <!-- Quantity with Real-time Update -->
            <StackLayout Spacing="5">
                <Label Text="Quantity:" FontAttributes="Bold" />
                <HorizontalStackLayout Spacing="10">
                    <editors:SfNumericEntry x:Name="quantityEntry"
                                            WidthRequest="150"
                                            Value="1"
                                            CustomFormat="N0"
                                            ValueChangeMode="OnKeyFocus"
                                            ShowBorder="True"
                                            AllowNull="False"
                                            ValueChanged="OnQuantityChanged" />
                    <Label x:Name="quantityLabel"
                           Text="Qty: 1"
                           VerticalOptions="Center"
                           FontSize="16" />
                </HorizontalStackLayout>
            </StackLayout>
            
            <!-- Total Display -->
            <StackLayout Spacing="5">
                <Label Text="Total:" FontAttributes="Bold" />
                <Label x:Name="totalLabel"
                       Text="$0.00"
                       FontSize="24"
                       TextColor="Green"
                       FontAttributes="Bold" />
            </StackLayout>
            
            <Button Text="Focus Price"
                    Clicked="OnFocusPriceClicked" />
            
        </VerticalStackLayout>
    </ScrollView>
</ContentPage>
```

```csharp
using Syncfusion.Maui.Inputs;

namespace NumericEntryApp;

public partial class BasicFeaturesPage : ContentPage
{
    public BasicFeaturesPage()
    {
        InitializeComponent();
    }
    
    private void OnPriceChanged(object sender, NumericEntryValueChangedEventArgs e)
    {
        UpdateTotal();
    }
    
    private void OnQuantityChanged(object sender, NumericEntryValueChangedEventArgs e)
    {
        quantityLabel.Text = $"Qty: {e.NewValue:N0}";
        UpdateTotal();
    }
    
    private void UpdateTotal()
    {
        var price = priceEntry.Value ?? 0;
        var quantity = quantityEntry.Value ?? 0;
        var total = price * quantity;
        
        totalLabel.Text = $"{total:C2}";
    }
    
    private void OnEntryFocused(object sender, FocusEventArgs e)
    {
        var entry = sender as SfNumericEntry;
        if (entry != null)
        {
            entry.Stroke = Colors.Blue;
        }
    }
    
    private void OnEntryUnfocused(object sender, FocusEventArgs e)
    {
        var entry = sender as SfNumericEntry;
        if (entry != null)
        {
            entry.Stroke = Colors.Gray;
        }
    }
    
    private async void OnClearClicked(object sender, EventArgs e)
    {
        await DisplayAlert("Info", "Clear button clicked", "OK");
    }
    
    private void OnFocusPriceClicked(object sender, EventArgs e)
    {
        priceEntry.Focus();
    }
}
```

## Summary

This reference covered:
- ✅ Placeholder text configuration
- ✅ Clear button visibility, color, and customization
- ✅ Value changed notifications and modes
- ✅ Completed and ClearButtonClicked events
- ✅ Border stroke and visibility
- ✅ Text alignment options
- ✅ Select all on focus behavior
- ✅ Return type configuration
- ✅ Return command binding
- ✅ AutomationId for UI testing

**Next:** Read [formatting.md](formatting.md) for detailed number formatting options.
