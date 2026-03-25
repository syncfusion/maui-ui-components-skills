# Grouping in .NET MAUI Radio Button

## Table of Contents
- [Overview](#overview)
- [SfRadioGroup Container](#sfradiogroup-container)
- [GroupKey Approach](#groupkey-approach)
- [CheckedItem Property](#checkeditem-property)
- [CheckedChanged Event](#checkedchanged-event)
- [Orientation in Radio Groups](#orientation-in-radio-groups)
- [SelectedValue Property](#selectedvalue-property)
- [Choosing Between Approaches](#choosing-between-approaches)
- [Advanced Scenarios](#advanced-scenarios)

## Overview

Radio buttons are designed for mutually exclusive selection—only one option can be selected at a time within a group. Syncfusion .NET MAUI Radio Button provides two approaches for grouping:

1. **SfRadioGroup:** A container control that manages radio buttons automatically
2. **GroupKey:** A property-based approach for grouping radio buttons across different layouts

## SfRadioGroup Container

`SfRadioGroup` is a container control that automatically manages mutual exclusion for all radio buttons added as its children. It's the recommended approach for most scenarios.

### Basic Usage

#### XAML

```xml
<buttons:SfRadioGroup x:Name="paymentGroup">
    <buttons:SfRadioButton Text="Credit Card"/>
    <buttons:SfRadioButton Text="Debit Card" IsChecked="True"/>
    <buttons:SfRadioButton Text="Net Banking"/>
    <buttons:SfRadioButton Text="Cash on Delivery"/>
</buttons:SfRadioGroup>
```

#### C#

```csharp
SfRadioGroup paymentGroup = new SfRadioGroup();

SfRadioButton creditCard = new SfRadioButton { Text = "Credit Card" };
SfRadioButton debitCard = new SfRadioButton { Text = "Debit Card", IsChecked = true };
SfRadioButton netBanking = new SfRadioButton { Text = "Net Banking" };
SfRadioButton cod = new SfRadioButton { Text = "Cash on Delivery" };

paymentGroup.Children.Add(creditCard);
paymentGroup.Children.Add(debitCard);
paymentGroup.Children.Add(netBanking);
paymentGroup.Children.Add(cod);

this.Content = paymentGroup;
```

### Benefits of SfRadioGroup

- **Automatic mutual exclusion:** No manual management required
- **CheckedItem property:** Easy access to the selected radio button
- **CheckedChanged event:** Single event for all selection changes
- **Built-in orientation support:** Vertical or horizontal layouts
- **SelectedValue binding:** Direct access to the selected value

## GroupKey Approach

The `GroupKey` property allows you to group radio buttons across different layouts or parent containers. This is useful when radio buttons are not direct siblings in the visual tree.

### Creating a GroupKey

Define `SfRadioGroupKey` instances as resources and assign them to radio buttons:

#### XAML

```xml
<ContentPage.Resources>
    <buttons:SfRadioGroupKey x:Key="carBrandKey"/>
    <buttons:SfRadioGroupKey x:Key="bikeBrandKey"/>
</ContentPage.Resources>

<StackLayout>
    <Label Text="Car Brands:" FontAttributes="Bold"/>
    <buttons:SfRadioButton Text="Honda" GroupKey="{StaticResource carBrandKey}"/>
    <buttons:SfRadioButton Text="Hyundai" GroupKey="{StaticResource carBrandKey}"/>
    <buttons:SfRadioButton Text="Volkswagen" GroupKey="{StaticResource carBrandKey}" IsChecked="True"/>
    
    <Label Text="Bike Brands:" FontAttributes="Bold" Margin="0,20,0,0"/>
    <buttons:SfRadioButton Text="Yamaha" GroupKey="{StaticResource bikeBrandKey}"/>
    <buttons:SfRadioButton Text="Bajaj" GroupKey="{StaticResource bikeBrandKey}" IsChecked="True"/>
    <buttons:SfRadioButton Text="Royal Enfield" GroupKey="{StaticResource bikeBrandKey}"/>
</StackLayout>
```

#### C#

```csharp
// Create GroupKeys
SfRadioGroupKey carBrandKey = new SfRadioGroupKey();
SfRadioGroupKey bikeBrandKey = new SfRadioGroupKey();

// Create car brand radio buttons
SfRadioButton honda = new SfRadioButton { Text = "Honda", GroupKey = carBrandKey };
SfRadioButton hyundai = new SfRadioButton { Text = "Hyundai", GroupKey = carBrandKey };
SfRadioButton volkswagen = new SfRadioButton { Text = "Volkswagen", GroupKey = carBrandKey, IsChecked = true };

// Create bike brand radio buttons
SfRadioButton yamaha = new SfRadioButton { Text = "Yamaha", GroupKey = bikeBrandKey };
SfRadioButton bajaj = new SfRadioButton { Text = "Bajaj", GroupKey = bikeBrandKey, IsChecked = true };
SfRadioButton royalEnfield = new SfRadioButton { Text = "Royal Enfield", GroupKey = bikeBrandKey };

// Create layout
StackLayout layout = new StackLayout();
layout.Children.Add(new Label { Text = "Car Brands:", FontAttributes = FontAttributes.Bold });
layout.Children.Add(honda);
layout.Children.Add(hyundai);
layout.Children.Add(volkswagen);
layout.Children.Add(new Label { Text = "Bike Brands:", FontAttributes = FontAttributes.Bold, Margin = new Thickness(0, 20, 0, 0) });
layout.Children.Add(yamaha);
layout.Children.Add(bajaj);
layout.Children.Add(royalEnfield);

this.Content = layout;
```

### GroupKey Across Different Layouts

GroupKey works even when radio buttons are in completely different containers:

```xml

    <ContentPage.Resources>
        <buttons:SfRadioGroupKey x:Key="sharedGroup"/>
    </ContentPage.Resources>
    
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
    </Grid.RowDefinitions>
    
    
    <!-- First container -->
    <StackLayout Grid.Row="0" BackgroundColor="LightBlue" Padding="10">
        <buttons:SfRadioButton Text="Option A" GroupKey="{StaticResource sharedGroup}"/>
        <buttons:SfRadioButton Text="Option B" GroupKey="{StaticResource sharedGroup}"/>
    </StackLayout>
    
    <!-- Second container -->
    <StackLayout Grid.Row="1" BackgroundColor="LightGreen" Padding="10">
        <buttons:SfRadioButton Text="Option C" GroupKey="{StaticResource sharedGroup}"/>
        <buttons:SfRadioButton Text="Option D" GroupKey="{StaticResource sharedGroup}"/>
    </StackLayout>
</Grid>
```

## CheckedItem Property

The `CheckedItem` property of `SfRadioGroup` provides direct access to the currently selected radio button.

### Accessing the Checked Item

```csharp
// Get the currently checked radio button
SfRadioButton checkedButton = paymentGroup.CheckedItem as SfRadioButton;

if (checkedButton != null)
{
    string selectedText = checkedButton.Text;
    Console.WriteLine($"Selected: {selectedText}");
}
```

### Programmatically Setting the Checked Item

```csharp
// Find and set a specific radio button as checked
var targetButton = paymentGroup.Children
    .OfType<SfRadioButton>()
    .FirstOrDefault(rb => rb.Text == "Credit Card");

if (targetButton != null)
{
    paymentGroup.CheckedItem = targetButton;
}
```

### Using CheckedItem in Data Binding

```xml
<buttons:SfRadioGroup x:Name="sizeGroup">
    <buttons:SfRadioButton Text="Small"/>
    <buttons:SfRadioButton Text="Medium" IsChecked="True"/>
    <buttons:SfRadioButton Text="Large"/>
</buttons:SfRadioGroup>

<Label Text="{Binding Source={x:Reference sizeGroup}, 
                      Path=CheckedItem.Text, 
                      StringFormat='Selected Size: {0}'}"/>
```

## CheckedChanged Event

The `CheckedChanged` event fires whenever the selected radio button changes within an `SfRadioGroup`. This provides a centralized way to handle selection changes.

### Event Arguments

The `CheckedChangedEventArgs` provides:
- **PreviousItem:** The previously checked radio button
- **CurrentItem:** The newly checked radio button

### XAML Event Handling

```xml
<buttons:SfRadioGroup x:Name="subscriptionGroup" 
                      CheckedChanged="OnSubscriptionChanged">
    <buttons:SfRadioButton Text="Basic" Value="basic"/>
    <buttons:SfRadioButton Text="Premium" Value="premium"/>
    <buttons:SfRadioButton Text="Enterprise" Value="enterprise"/>
</buttons:SfRadioGroup>
```

```csharp
private void OnSubscriptionChanged(object sender, CheckedChangedEventArgs e)
{
    if (e.PreviousItem is SfRadioButton previous)
    {
        Console.WriteLine($"Previous: {previous.Text}");
    }
    
    if (e.CurrentItem is SfRadioButton current)
    {
        Console.WriteLine($"Current: {current.Text}");
        
        // Perform actions based on selection
        UpdatePricing(current.Value?.ToString());
    }
}

private void UpdatePricing(string plan)
{
    switch (plan)
    {
        case "basic":
            priceLabel.Text = "$9.99/month";
            break;
        case "premium":
            priceLabel.Text = "$19.99/month";
            break;
        case "enterprise":
            priceLabel.Text = "$49.99/month";
            break;
    }
}
```

### C# Event Handling

```csharp
SfRadioGroup subscriptionGroup = new SfRadioGroup();
subscriptionGroup.CheckedChanged += (sender, e) =>
{
    if (e.CurrentItem is SfRadioButton current)
    {
        DisplayAlert("Selection Changed", $"You selected: {current.Text}", "OK");
    }
};
```

## Orientation in Radio Groups

`SfRadioGroup` supports both vertical (default) and horizontal orientations.

### Vertical Orientation (Default)

```xml
<buttons:SfRadioGroup Orientation="Vertical">
    <buttons:SfRadioButton Text="Option 1"/>
    <buttons:SfRadioButton Text="Option 2"/>
    <buttons:SfRadioButton Text="Option 3"/>
</buttons:SfRadioGroup>
```

### Horizontal Orientation

```xml
<buttons:SfRadioGroup Orientation="Horizontal">
    <buttons:SfRadioButton Text="Yes"/>
    <buttons:SfRadioButton Text="No"/>
    <buttons:SfRadioButton Text="Maybe"/>
</buttons:SfRadioGroup>
```

```csharp
SfRadioGroup orientationGroup = new SfRadioGroup
{
    Orientation = StackOrientation.Horizontal
};

orientationGroup.Children.Add(new SfRadioButton { Text = "Yes" });
orientationGroup.Children.Add(new SfRadioButton { Text = "No" });
orientationGroup.Children.Add(new SfRadioButton { Text = "Maybe" });
```

### Responsive Layout Example

```xml
<buttons:SfRadioGroup Orientation="Horizontal" 
                      HorizontalOptions="Center"
                      Spacing="20">
    <buttons:SfRadioButton Text="1 Star"/>
    <buttons:SfRadioButton Text="2 Stars"/>
    <buttons:SfRadioButton Text="3 Stars"/>
    <buttons:SfRadioButton Text="4 Stars"/>
    <buttons:SfRadioButton Text="5 Stars"/>
</buttons:SfRadioGroup>
```

## SelectedValue Property

The `SelectedValue` property provides a way to bind directly to the value of the selected radio button, rather than the radio button object itself.

### Setting and Retrieving SelectedValue

```xml
<buttons:SfRadioGroup x:Name="paymentGroup" SelectedValue="DebitCard">
    <buttons:SfRadioButton Text="Net Banking" Value="NetBanking"/>
    <buttons:SfRadioButton Text="Debit Card" Value="DebitCard"/>
    <buttons:SfRadioButton Text="Credit Card" Value="CreditCard"/>
</buttons:SfRadioGroup>
```

```csharp
// Get the selected value
string selectedPayment = paymentGroup.SelectedValue?.ToString();

// Set the selected value programmatically
paymentGroup.SelectedValue = "CreditCard";
```

### Data Binding with SelectedValue

This is particularly useful for MVVM patterns:

```xml
<buttons:SfRadioGroup SelectedValue="{Binding SelectedPaymentMethod}">
    <buttons:SfRadioButton Text="Net Banking" Value="NetBanking"/>
    <buttons:SfRadioButton Text="Debit Card" Value="DebitCard"/>
    <buttons:SfRadioButton Text="Credit Card" Value="CreditCard"/>
</buttons:SfRadioGroup>
```

```csharp
// ViewModel
public class PaymentViewModel : INotifyPropertyChanged
{
    private string _selectedPaymentMethod = "DebitCard";
    
    public string SelectedPaymentMethod
    {
        get => _selectedPaymentMethod;
        set
        {
            if (_selectedPaymentMethod != value)
            {
                _selectedPaymentMethod = value;
                OnPropertyChanged(nameof(SelectedPaymentMethod));
                ProcessPaymentMethod(value);
            }
        }
    }
    
    private void ProcessPaymentMethod(string method)
    {
        // Handle payment method change
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected virtual void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

## Choosing Between Approaches

### Use SfRadioGroup When:
- Radio buttons are visually grouped together
- You need a single container for all options
- You want centralized event handling
- You need easy access to the selected item
- You want built-in orientation support
- Working with MVVM and data binding

### Use GroupKey When:
- Radio buttons are in different layouts or containers
- You need flexible positioning across the UI
- Radio buttons are dynamically added to various locations
- You have multiple independent groups in complex layouts
- You need to group buttons across different visual hierarchies

## Advanced Scenarios

### Multiple Groups in the Same View

```xml
<VerticalStackLayout Spacing="20" Padding="20">
    
    <!-- Size Selection Group -->
    <Label Text="Size:" FontAttributes="Bold"/>
    <buttons:SfRadioGroup x:Name="sizeGroup">
        <buttons:SfRadioButton Text="Small"/>
        <buttons:SfRadioButton Text="Medium" IsChecked="True"/>
        <buttons:SfRadioButton Text="Large"/>
    </buttons:SfRadioGroup>
    
    <!-- Color Selection Group -->
    <Label Text="Color:" FontAttributes="Bold"/>
    <buttons:SfRadioGroup x:Name="colorGroup" Orientation="Horizontal">
        <buttons:SfRadioButton Text="Red"/>
        <buttons:SfRadioButton Text="Blue" IsChecked="True"/>
        <buttons:SfRadioButton Text="Green"/>
    </buttons:SfRadioGroup>
    
    <!-- Delivery Selection Group -->
    <Label Text="Delivery:" FontAttributes="Bold"/>
    <buttons:SfRadioGroup x:Name="deliveryGroup">
        <buttons:SfRadioButton Text="Standard (5-7 days)"/>
        <buttons:SfRadioButton Text="Express (2-3 days)" IsChecked="True"/>
        <buttons:SfRadioButton Text="Next Day"/>
    </buttons:SfRadioGroup>
    
    <Button Text="Place Order" Clicked="OnPlaceOrder"/>
    
</VerticalStackLayout>
```

```csharp
private void OnPlaceOrder(object sender, EventArgs e)
{
    var size = (sizeGroup.CheckedItem as SfRadioButton)?.Text ?? "None";
    var color = (colorGroup.CheckedItem as SfRadioButton)?.Text ?? "None";
    var delivery = (deliveryGroup.CheckedItem as SfRadioButton)?.Text ?? "None";
    
    DisplayAlert("Order Summary", 
        $"Size: {size}\nColor: {color}\nDelivery: {delivery}", 
        "OK");
}
```

### Dynamic Radio Button Groups

```csharp
public void CreateDynamicGroup(List<string> options, string groupName)
{
    SfRadioGroup dynamicGroup = new SfRadioGroup();
    
    foreach (var option in options)
    {
        SfRadioButton radioButton = new SfRadioButton
        {
            Text = option,
            Value = option.ToLower().Replace(" ", "_")
        };
        
        dynamicGroup.Children.Add(radioButton);
    }
    
    // Set first option as checked
    if (dynamicGroup.Children.Count > 0)
    {
        (dynamicGroup.Children[0] as SfRadioButton).IsChecked = true;
    }
    
    dynamicGroup.CheckedChanged += (s, e) =>
    {
        if (e.CurrentItem is SfRadioButton selected)
        {
            Console.WriteLine($"{groupName}: {selected.Text}");
        }
    };
    
    // Add to your layout
    mainLayout.Children.Add(new Label { Text = groupName, FontAttributes = FontAttributes.Bold });
    mainLayout.Children.Add(dynamicGroup);
}

// Usage
CreateDynamicGroup(new List<string> { "Option A", "Option B", "Option C" }, "Category 1");
```

### Conditional Grouping

```csharp
// Switch between different grouping strategies based on layout
if (isCompactLayout)
{
    // Use horizontal group for compact layout
    var compactGroup = new SfRadioGroup { Orientation = StackOrientation.Horizontal };
    // Add buttons...
}
else
{
    // Use GroupKey for expanded layout with sections
    var sectionKey = new SfRadioGroupKey();
    // Assign to buttons in different sections...
}
```

## Best Practices

1. **Prefer SfRadioGroup** for most scenarios—it's simpler and more maintainable
2. **Use GroupKey** only when radio buttons need to be in different containers
3. **Always initialize one option as checked** to avoid undefined states
4. **Use Value property** for data binding scenarios instead of relying on Text
5. **Handle CheckedChanged** at the group level rather than individual StateChanged events
6. **Provide clear labels** for each group to indicate what the user is selecting
7. **Limit options per group** to 5-7 for better usability (use dropdowns for more options)
8. **Consider horizontal orientation** for short labels and limited options (2-4 items)
