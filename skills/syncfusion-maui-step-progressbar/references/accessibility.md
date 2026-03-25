# Accessibility in StepProgressBar

## Overview

The Syncfusion .NET MAUI StepProgressBar is designed with accessibility in mind, ensuring that users with disabilities can effectively navigate and understand multi-step processes. The control provides built-in screen reader support and follows inclusive design principles.

## Accessibility Features

### Screen Reader Support

The StepProgressBar automatically generates descriptive text for screen readers, announcing:
- Step number
- Primary text label
- Secondary text label
- Current step status (Completed, In Progress, Not Started)

### Announcement Format

Screen readers announce each step using the following patterns:

#### With Text Labels

**Format:** `Step [Number] [PrimaryText]/[SecondaryText] [Step Status]`

**Examples:**

```
"Step 1 Login Login successfully completed"
"Step 2 Address Enter delivery address In Progress"
"Step 3 Payment Choose payment method Not Started"
```

#### With Formatted Text Labels

**Format:** `Step [Number] [PrimaryFormattedText]/[SecondaryFormattedText] [Step Status]`

**Example:**

```
"Step 1 Welcome to our platform Join us and explore! Completed"
```

### Status Announcements

The control announces three step states:

| Status | Announcement |
|--------|--------------|
| **Completed** | "Completed" or "completed" |
| **In Progress** | "In Progress" or "in progress" |
| **Not Started** | "Not Started" or "not started" |

## Accessibility Best Practices

### Practice 1: Always Provide Text Labels

Even when using images or custom templates, always include `PrimaryText` for screen reader support:

**Good:**
```csharp
new StepProgressBarItem() 
{ 
    ImageSource = "cart.png",
    PrimaryText = "Shopping Cart"  // Important for screen readers
}
```

**Bad:**
```csharp
new StepProgressBarItem() 
{ 
    ImageSource = "cart.png"
    // Missing text - screen reader can't describe this step
}
```

### Practice 2: Use Descriptive Labels

Make labels clear and descriptive:

**Good:**
```csharp
new StepProgressBarItem() { PrimaryText = "Shipping Address" }
new StepProgressBarItem() { PrimaryText = "Payment Method" }
new StepProgressBarItem() { PrimaryText = "Order Confirmation" }
```

**Poor:**
```csharp
new StepProgressBarItem() { PrimaryText = "Step 1" }
new StepProgressBarItem() { PrimaryText = "Step 2" }
new StepProgressBarItem() { PrimaryText = "Next" }
```

### Practice 3: Provide Context with SecondaryText

Use `SecondaryText` to add helpful context:

```csharp
new StepProgressBarItem() 
{ 
    PrimaryText = "Verification",
    SecondaryText = "Check your email"  // Provides guidance
}
```

### Practice 4: Meaningful Progress States

Ensure step states accurately reflect the user's actual progress:

```csharp
// Update ActiveStepIndex as user completes steps
stepProgressBar.ActiveStepIndex = currentCompletedSteps;

// Show partial progress within a step
stepProgressBar.ActiveStepProgressValue = percentComplete;
```

### Practice 5: Keyboard Navigation Support

While the StepProgressBar doesn't have built-in keyboard navigation, ensure the surrounding page supports keyboard access:

```csharp
// Provide keyboard shortcuts for navigation
// Add focus management for step content
// Ensure all interactive elements are keyboard accessible
```

### Practice 6: Color Contrast

Maintain sufficient color contrast for users with visual impairments:

**WCAG AA Compliance:**
- Text: Minimum 4.5:1 contrast ratio
- UI Components: Minimum 3:1 contrast ratio

```xml
<stepProgressBar:SfStepProgressBar.CompletedStepSettings>
    <stepProgressBar:StepSettings 
        Background="#4CAF50"       <!-- Green background -->
        ContentFillColor="White">  <!-- White checkmark: High contrast -->
</stepProgressBar:SfStepProgressBar.CompletedStepSettings>
```

**Testing Contrast:**
Use tools like:
- WebAIM Contrast Checker
- Color Contrast Analyzer
- Browser DevTools accessibility audits

### Practice 7: Focus Indicators

Ensure tappable steps have clear focus indicators when using keyboard navigation:

```xml
<stepProgressBar:SfStepProgressBar.CompletedStepSettings>
    <stepProgressBar:StepSettings 
        Stroke="Blue">          <!-- Border color for focus -->
</stepProgressBar:SfStepProgressBar.CompletedStepSettings>
```

## Testing Accessibility

### Screen Reader Testing

**iOS VoiceOver:**
1. Enable: Settings → Accessibility → VoiceOver
2. Navigate: Swipe right/left to move between steps
3. Activate: Double-tap to select a step

**Android TalkBack:**
1. Enable: Settings → Accessibility → TalkBack
2. Navigate: Swipe right/left to move between steps
3. Activate: Double-tap to select a step

**Windows Narrator:**
1. Enable: Win + Ctrl + Enter
2. Navigate: Tab or arrow keys
3. Verify step announcements are clear

### Checklist

- [ ] All steps have PrimaryText or PrimaryFormattedText
- [ ] Text labels are descriptive and meaningful
- [ ] Color contrast meets WCAG AA standards (4.5:1 for text, 3:1 for UI)
- [ ] Status changes are announced correctly
- [ ] Step order is logical and intuitive
- [ ] Tooltips (if used) provide additional context
- [ ] Focus indicators are visible
- [ ] Interactive steps are keyboard accessible

## Accessibility Properties

### Semantic Properties

MAUI provides semantic properties for enhanced accessibility:

```csharp
// Set semantic description for the entire control
SemanticProperties.SetDescription(stepProgressBar, 
    "Order checkout progress with 5 steps");

// Set hint for interaction
SemanticProperties.SetHint(stepProgressBar, 
    "Tap a completed step to return to that page");
```

### Specific Step Semantics

For custom scenarios, you can set semantics on individual steps:

```csharp
// In a custom StepTemplate
SemanticProperties.SetDescription(stepView, 
    $"Step {index + 1}: {stepItem.PrimaryText}");
```

## Inclusive Design Considerations

### Visual Indicators

Don't rely solely on color to convey information. Use multiple indicators:

**Completed Steps:**
- Green color ✓
- Checkmark icon ✓
- "Completed" text ✓

**In Progress Steps:**
- Orange color ✓
- Pulsing animation ✓
- Dot or progress indicator ✓

**Not Started Steps:**
- Gray color ✓
- Empty or X icon ✓
- "Not Started" text ✓

### Text Size and Readability

Use appropriate font sizes:

```xml
<stepProgressBar:StepProgressBar.CompletedStepSettings>
    <stepProgressBar:StepSettings>
        <stepProgressBar:StepSettings.TextStyle>
            <stepProgressBar:StepTextStyle 
                FontFamily="OpenSans"
                FontSize="14"/> <!-- Minimum 14 for readability -->
        </stepProgressBar:StepSettings.TextStyle>
    </stepProgressBar:StepSettings>
</stepProgressBar:StepProgressBar.CompletedStepSettings>
```

**Recommendations:**
- Minimum font size: 14px
- Use clear, readable fonts (avoid decorative fonts)
- Maintain consistent spacing

### Touch Target Size

Ensure steps are large enough to tap easily:

**Minimum recommended:** 44x44 pixels (iOS HIG) / 48x48 dp (Android Material)

```xml
<stepProgressBar:SfStepProgressBar 
    StepSize="48"  
    StepContentSize="32"><!-- Adequate touch target -->
</stepProgressBar:SfStepProgressBar>
```

## Real-World Example

Here's a fully accessible StepProgressBar implementation:

```csharp
public class AccessibleCheckoutViewModel
{
    public ObservableCollection<StepProgressBarItem> CheckoutSteps { get; set; }
    
    public AccessibleCheckoutViewModel()
    {
        CheckoutSteps = new ObservableCollection<StepProgressBarItem>
        {
            new StepProgressBarItem() 
            { 
                PrimaryText = "Shopping Cart",
                SecondaryText = "Review items",
                ToolTipText = "Review and update items in your cart"
            },
            new StepProgressBarItem() 
            { 
                PrimaryText = "Shipping Address",
                SecondaryText = "Where to send",
                ToolTipText = "Enter your delivery address"
            },
            new StepProgressBarItem() 
            { 
                PrimaryText = "Delivery Method",
                SecondaryText = "How to send",
                ToolTipText = "Choose standard or express shipping"
            },
            new StepProgressBarItem() 
            { 
                PrimaryText = "Payment",
                SecondaryText = "Secure checkout",
                ToolTipText = "Enter payment details securely"
            },
            new StepProgressBarItem() 
            { 
                PrimaryText = "Confirmation",
                SecondaryText = "Review order",
                ToolTipText = "Final review before placing order"
            }
        };
    }
}
```

```xml
<stepProgressBar:SfStepProgressBar 
    ItemsSource="{Binding CheckoutSteps}"
    ActiveStepIndex="2"
    StepSize="48"
    ShowToolTip="True"
    AutomationId="CheckoutProgressBar">
    
    <stepProgressBar:SfStepProgressBar.CompletedStepSettings>
        <stepProgressBar:StepSettings 
            Background="#2E7D32" 
            ContentFillColor="White"
            ContentType="Tick">
            <stepProgressBar:StepSettings.TextStyle>
                <stepProgressBar:StepTextStyle 
                    TextColor="#1B5E20"
                    FontSize="14"
                    FontAttributes="Bold"/>
            </stepProgressBar:StepSettings.TextStyle>
        </stepProgressBar:StepSettings>
    </stepProgressBar:SfStepProgressBar.CompletedStepSettings>
    
</stepProgressBar:SfStepProgressBar>
```

This implementation:
- ✓ Provides clear text labels for all steps
- ✓ Includes secondary text for context
- ✓ Adds tooltips for additional guidance
- ✓ Uses high-contrast colors (WCAG AA)
- ✓ Sets adequate touch target size (48px)
- ✓ Includes automation ID for testing
- ✓ Uses bold text for emphasis
- ✓ Shows clear visual state indicators

## Resources

- [WCAG 2.1 Guidelines](https://www.w3.org/WAI/WCAG21/quickref/)
- [iOS Accessibility Human Interface Guidelines](https://developer.apple.com/design/human-interface-guidelines/accessibility/overview/introduction/)
- [Android Accessibility Guidelines](https://developer.android.com/guide/topics/ui/accessibility)
- [Microsoft Inclusive Design](https://www.microsoft.com/design/inclusive/)