# Value Restrictions in .NET MAUI Numeric Entry

This guide covers how to restrict and validate values in the Numeric Entry control using `AllowNull`, `Minimum`, `Maximum`, and `IsEditable` properties.

## Restrict Null Values

By default, the Numeric Entry allows **null** values. When the input is cleared, the `Value` property becomes `null`. You can control this behavior with the `AllowNull` property.

### Allow Null (Default Behavior)

```xml
<editors:SfNumericEntry WidthRequest="200"
                        AllowNull="True"
                        Placeholder="Enter value" />
```

```csharp
var numericEntry = new SfNumericEntry
{
    WidthRequest = 200,
    AllowNull = true  // Default
};
```

**Default value:** `true`

**Behavior when cleared:**
- Value becomes `null`
- Placeholder text appears (if set)
- Clear button sets value to `null`

### Disallow Null

```xml
<editors:SfNumericEntry WidthRequest="200"
                        Value="10"
                        AllowNull="False" />
```

```csharp
var numericEntry = new SfNumericEntry
{
    WidthRequest = 200,
    Value = 10,
    AllowNull = false
};
```

**Behavior when cleared:**
- Value returns to **0** (if no Minimum is set)
- Value returns to **Minimum** (if Minimum is set)
- Placeholder never appears
- Value property is never null

### Interaction with Minimum Property

#### AllowNull=True, Minimum=15

```xml
<editors:SfNumericEntry WidthRequest="200"
                        Minimum="15"
                        AllowNull="True" />
```

**Behavior:** When cleared, value becomes `null` (Minimum is ignored for null values)

#### AllowNull=False, Minimum=15

```xml
<editors:SfNumericEntry WidthRequest="200"
                        Minimum="15"
                        AllowNull="False" />
```

**Behavior:** When cleared, value returns to `15` (the Minimum value)

### Examples

```xml
<VerticalStackLayout Padding="20" Spacing="15">
    
    <!-- Allow Null -->
    <StackLayout Spacing="5">
        <Label Text="Allow Null (Optional Field):" />
        <editors:SfNumericEntry WidthRequest="250"
                                AllowNull="True"
                                Placeholder="Optional value"
                                Value="100" />
    </StackLayout>
    
    <!-- Disallow Null with Default -->
    <StackLayout Spacing="5">
        <Label Text="Required (Returns to 0):" />
        <editors:SfNumericEntry WidthRequest="250"
                                AllowNull="False"
                                Value="100" />
    </StackLayout>
    
    <!-- Disallow Null with Minimum -->
    <StackLayout Spacing="5">
        <Label Text="Required (Returns to Minimum):" />
        <editors:SfNumericEntry WidthRequest="250"
                                Minimum="10"
                                AllowNull="False"
                                Value="100" />
    </StackLayout>
    
</VerticalStackLayout>
```

### Use Cases

**Allow Null = True:**
- Optional form fields
- Discount/coupon inputs (no discount = null)
- Optional measurements
- Secondary contact numbers

**Allow Null = False:**
- Required form fields
- Quantity selectors (must have at least 1)
- Price inputs (must have value)
- Age or rating inputs

## Restrict Value Within Range

Use `Minimum` and `Maximum` properties to enforce value constraints. Values outside this range are automatically adjusted to the nearest valid value.

### Set Minimum Value

```xml
<editors:SfNumericEntry WidthRequest="200"
                        Minimum="10"
                        Value="5" />
<!-- Value automatically adjusted to 10 -->
```

```csharp
var numericEntry = new SfNumericEntry
{
    WidthRequest = 200,
    Minimum = 10,
    Value = 5  // Automatically adjusted to 10
};
```

**Default value:** `double.MinValue` (essentially no minimum)

**Behavior:**
- User enters value less than Minimum → adjusted to Minimum on validation
- Initial Value less than Minimum → automatically set to Minimum
- When cleared and AllowNull=False → returns to Minimum

### Set Maximum Value

```xml
<editors:SfNumericEntry WidthRequest="200"
                        Maximum="100"
                        Value="150" />
<!-- Value automatically adjusted to 100 -->
```

```csharp
var numericEntry = new SfNumericEntry
{
    WidthRequest = 200,
    Maximum = 100,
    Value = 150  // Automatically adjusted to 100
};
```

**Default value:** `double.MaxValue` (essentially no maximum)

**Behavior:**
- User enters value greater than Maximum → adjusted to Maximum on validation
- Initial Value greater than Maximum → automatically set to Maximum

### Set Both Minimum and Maximum

```xml
<editors:SfNumericEntry WidthRequest="200"
                        Value="50"
                        Minimum="10"
                        Maximum="30" />
<!-- Value automatically adjusted to 30 (Maximum) -->
```

```csharp
var numericEntry = new SfNumericEntry
{
    WidthRequest = 200,
    Minimum = 10,
    Maximum = 30,
    Value = 50  // Automatically adjusted to 30
};
```

**Behavior:**
- Value constrained to range [Minimum, Maximum]
- Out-of-range values adjusted on validation (Enter key or focus loss)

### Range Validation Examples

```xml
<VerticalStackLayout Padding="20" Spacing="15">
    
    <!-- Age (18-120) -->
    <StackLayout Spacing="5">
        <Label Text="Age (18-120):" />
        <editors:SfNumericEntry WidthRequest="250"
                                Minimum="18"
                                Maximum="120"
                                AllowNull="False"
                                CustomFormat="N0"
                                Placeholder="Enter age" />
    </StackLayout>
    
    <!-- Percentage (0-100) -->
    <StackLayout Spacing="5">
        <Label Text="Discount % (0-100):" />
        <editors:SfNumericEntry WidthRequest="250"
                                Minimum="0"
                                Maximum="100"
                                CustomFormat="N0"
                                Placeholder="Enter discount" />
    </StackLayout>
    
    <!-- Price (0.01-999999.99) -->
    <StackLayout Spacing="5">
        <Label Text="Price ($0.01-$999,999.99):" />
        <editors:SfNumericEntry WidthRequest="250"
                                Minimum="0.01"
                                Maximum="999999.99"
                                AllowNull="False"
                                CustomFormat="C2"
                                Placeholder="$0.00" />
    </StackLayout>
    
    <!-- Quantity (1-999) -->
    <StackLayout Spacing="5">
        <Label Text="Quantity (1-999):" />
        <editors:SfNumericEntry WidthRequest="250"
                                Minimum="1"
                                Maximum="999"
                                AllowNull="False"
                                CustomFormat="N0" />
    </StackLayout>
    
</VerticalStackLayout>
```

### Use Cases

**Common Ranges:**

| Field Type | Minimum | Maximum | Example |
|------------|---------|---------|---------|
| Age | 0 or 18 | 120 | Adult age verification |
| Percentage | 0 | 100 | Discount, tax rate |
| Rating | 1 | 5 or 10 | Star ratings |
| Quantity | 1 | 9999 | Shopping cart |
| Price | 0.01 | 999999.99 | Product pricing |
| Temperature (°C) | -273.15 | 1000 | Scientific measurements |
| Hour (0-24) | 0 | 23 | Time picker |
| Minute/Second | 0 | 59 | Time picker |

## Restrict Text Editing

The `IsEditable` property controls whether users can type directly into the Numeric Entry. When set to `false`, users can still change the value using:
- Up/Down buttons
- Mouse scrolling
- Keyboard arrow keys (↑↓)
- Keyboard Page Up/Down keys

### Enable Editing (Default)

```xml
<editors:SfNumericEntry WidthRequest="200"
                        IsEditable="True" />
```

**Default value:** `true`

**Behavior:**
- User can type numbers directly
- Clear button appears when focused
- Keyboard opens on mobile devices

### Disable Editing

```xml
<editors:SfNumericEntry WidthRequest="200"
                        IsEditable="False"
                        Value="50"
                        UpDownPlacementMode="Inline" />
```

```csharp
var numericEntry = new SfNumericEntry
{
    WidthRequest = 200,
    IsEditable = false,
    Value = 50,
    UpDownPlacementMode = NumericEntryUpDownPlacementMode.Inline
};
```

**Behavior:**
- User cannot type or edit text
- Clear button does NOT appear
- Keyboard does NOT open
- Value can still be changed via:
  - Up/Down buttons (if visible)
  - Mouse scrolling (desktop)
  - Keyboard arrow keys
  - Keyboard Page Up/Down keys

### Example: Read-Only with Button Controls

```xml
<VerticalStackLayout Padding="20" Spacing="15">
    
    <!-- Editable (Default) -->
    <StackLayout Spacing="5">
        <Label Text="Editable (Type allowed):" />
        <editors:SfNumericEntry WidthRequest="250"
                                Value="50"
                                IsEditable="True"
                                UpDownPlacementMode="Inline" />
    </StackLayout>
    
    <!-- Read-Only with Buttons -->
    <StackLayout Spacing="5">
        <Label Text="Read-Only (Buttons only):" />
        <editors:SfNumericEntry WidthRequest="250"
                                Value="50"
                                IsEditable="False"
                                UpDownPlacementMode="Inline"
                                SmallChange="5" />
    </StackLayout>
    
    <!-- Read-Only without Buttons -->
    <StackLayout Spacing="5">
        <Label Text="Display Only:" />
        <editors:SfNumericEntry WidthRequest="250"
                                Value="50"
                                IsEditable="False"
                                CustomFormat="C2"
                                ShowClearButton="False" />
    </StackLayout>
    
</VerticalStackLayout>
```

### Use Cases

**IsEditable = False:**
- Increment/decrement only inputs (volume controls, counters)
- Display formatted values with controlled changes
- Prevent manual entry errors in critical fields
- Step-based inputs (increment by specific amounts only)
- Touch-friendly mobile interfaces (buttons instead of keyboard)

**IsEditable = True:**
- Standard form inputs
- Free-form numeric entry
- Fast data entry scenarios
- Desktop applications where keyboard is preferred

## Edge Cases and Validation

### Edge Case 1: Value Less Than Minimum

```csharp
var entry = new SfNumericEntry
{
    Minimum = 10,
    Value = 5
};
// Value automatically adjusted to 10
```

### Edge Case 2: Value Greater Than Maximum

```csharp
var entry = new SfNumericEntry
{
    Maximum = 100,
    Value = 150
};
// Value automatically adjusted to 100
```

### Edge Case 3: Minimum Greater Than Maximum

```csharp
var entry = new SfNumericEntry
{
    Minimum = 100,
    Maximum = 10
};
// Behavior undefined - avoid this configuration
```

**Best practice:** Always ensure Minimum ≤ Maximum

### Edge Case 4: Null Value with Range

```csharp
var entry = new SfNumericEntry
{
    Minimum = 10,
    Maximum = 100,
    AllowNull = true,
    Value = null  // Allowed, not constrained by range
};
```

**Behavior:** Null values are NOT constrained by Minimum/Maximum. Range validation only applies to non-null values.

### Edge Case 5: Clear Button with Minimum

```csharp
var entry = new SfNumericEntry
{
    Minimum = 10,
    AllowNull = false,
    ShowClearButton = true
};
// Clicking clear button sets value to 10 (Minimum)
```

## Validation Best Practices

1. **Set AllowNull=False for required fields**
   - Ensures value is never null
   - Provides default value (0 or Minimum)

2. **Use Minimum for positive-only inputs**
   - Prices: Minimum="0.01"
   - Quantities: Minimum="1"
   - Ages: Minimum="0" or Minimum="18"

3. **Set both Minimum and Maximum for bounded inputs**
   - Percentages: 0-100
   - Ratings: 1-5
   - Hours: 0-23

4. **Combine IsEditable=False with UpDown buttons**
   - Prevents input errors
   - Forces discrete value changes
   - Better mobile experience

5. **Validate in ValueChanged event**
   - Show user-friendly error messages
   - Provide real-time feedback
   - Handle edge cases gracefully

6. **Use appropriate CustomFormat**
   - Match format to data type (C for currency, N0 for integers)
   - Helps users understand expected input

## Summary

This reference covered:
- ✅ AllowNull property (true/false behavior)
- ✅ Minimum property (enforce lower bound)
- ✅ Maximum property (enforce upper bound)
- ✅ IsEditable property (restrict typing, allow button/key changes)
- ✅ Interaction between AllowNull and Minimum
- ✅ Range validation and edge cases
- ✅ Validation best practices

**Next:** Read [updown-buttons.md](updown-buttons.md) for increment/decrement button features.
