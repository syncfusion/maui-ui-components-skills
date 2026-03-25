# Orientation in StepProgressBar

## Overview

The StepProgressBar control supports two orientations: **Horizontal** and **Vertical**. The orientation determines how steps are arranged and how progress flows. Choose the orientation based on your UI layout, available screen space, and user experience requirements.

## Horizontal Orientation

Horizontal is the **default** orientation. Steps are arranged from left to right with progress flowing horizontally.

### When to Use Horizontal

- **Wide screens**: Desktop, tablets in landscape mode
- **Header/footer layouts**: Progress indicators at top or bottom of page
- **Checkout flows**: E-commerce cart → address → payment → confirmation
- **Short step sequences**: 3-6 steps fit comfortably

### Setting Horizontal Orientation

**XAML:**
```xml
<stepProgressBar:SfStepProgressBar 
    Orientation="Horizontal"
    ItemsSource="{Binding StepProgressItem}">
</stepProgressBar:SfStepProgressBar>
```

**C#:**
```csharp
SfStepProgressBar stepProgressBar = new SfStepProgressBar()
{
    Orientation = StepProgressBarOrientation.Horizontal,
    ItemsSource = viewModel.StepProgressItem
};

this.Content = stepProgressBar;
```

### Horizontal Layout Behavior

**Label Position Defaults:**
- **PrimaryText**: Displayed below the step (Bottom)
- **SecondaryText**: Displayed above the step (Top)

**Progress Flow:**
- Left step = Completed
- Middle step = In Progress
- Right step = Not Started

**Visual Example:**
```
[✓] ──── [✓] ──── [◉] ──── [ ] ──── [ ]
Cart    Address  Delivery  Pay   Confirm
```

### Horizontal with Custom Label Position

```xml
<stepProgressBar:SfStepProgressBar 
    Orientation="Horizontal"
    LabelPosition="Top"
    ItemsSource="{Binding StepProgressItem}">
</stepProgressBar:SfStepProgressBar>
```

Labels appear above steps instead of below.

## Vertical Orientation

Steps are arranged from top to bottom with progress flowing vertically.

### When to Use Vertical

- **Sidebar layouts**: Left or right panel navigation
- **Mobile screens**: Better use of vertical space on phones
- **Long step sequences**: 6+ steps that would be cramped horizontally
- **Detailed descriptions**: Each step needs longer text labels

### Setting Vertical Orientation

**XAML:**
```xml
<stepProgressBar:SfStepProgressBar 
    Orientation="Vertical"
    ItemsSource="{Binding StepProgressItem}">
</stepProgressBar:SfStepProgressBar>
```

**C#:**
```csharp
SfStepProgressBar stepProgressBar = new SfStepProgressBar()
{
    Orientation = StepProgressBarOrientation.Vertical,
    ItemsSource = viewModel.StepProgressItem
};

this.Content = stepProgressBar;
```

### Vertical Layout Behavior

**Label Position Defaults:**
- **PrimaryText**: Displayed to the right of the step (End)
- **SecondaryText**: Displayed to the right of the step (Start)

**Progress Flow:**
- Top step = Completed
- Middle step = In Progress
- Bottom step = Not Started

**Visual Example:**
```
[✓] ─ Step 1: Cart
│
[✓] ─ Step 2: Address
│
[◉] ─ Step 3: Delivery (In Progress)
│
[ ] ─ Step 4: Payment
│
[ ] ─ Step 5: Confirm
```

### Vertical with Custom Label Position

```xml
<stepProgressBar:SfStepProgressBar 
    Orientation="Vertical"
    LabelPosition="Start"
    ItemsSource="{Binding StepProgressItem}">
</stepProgressBar:SfStepProgressBar>
```

Labels appear to the left of steps instead of right.

## Complete Examples

### Example 1: Horizontal Checkout Flow

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:stepProgressBar="clr-namespace:Syncfusion.Maui.ProgressBar;assembly=Syncfusion.Maui.ProgressBar"
             xmlns:local="clr-namespace:YourNamespace"
             x:Class="YourNamespace.CheckoutPage">
    
    <ContentPage.BindingContext>
        <local:CheckoutViewModel />
    </ContentPage.BindingContext>
    
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="100" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>
        
        <!-- Horizontal progress at top -->
        <stepProgressBar:SfStepProgressBar 
            Grid.Row="0"
            Orientation="Horizontal"
            HorizontalOptions="Center"
            VerticalOptions="Center"
            ActiveStepIndex="1"
            ActiveStepProgressValue="70"
            ItemsSource="{Binding CheckoutSteps}" />
        
        <!-- Main content area -->
        <ContentView Grid.Row="1" Content="{Binding CurrentStepContent}" />
    </Grid>
    
</ContentPage>
```

### Example 2: Vertical Registration Sidebar

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:stepProgressBar="clr-namespace:Syncfusion.Maui.ProgressBar;assembly=Syncfusion.Maui.ProgressBar"
             xmlns:local="clr-namespace:YourNamespace"
             x:Class="YourNamespace.RegistrationPage">
    
    <ContentPage.BindingContext>
        <local:RegistrationViewModel />
    </ContentPage.BindingContext>
    
    <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="200" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>
        
        <!-- Vertical progress sidebar -->
        <stepProgressBar:SfStepProgressBar 
            Grid.Column="0"
            Orientation="Vertical"
            HorizontalOptions="Center"
            VerticalOptions="Center"
            LabelPosition="End"
            ActiveStepIndex="2"
            ItemsSource="{Binding RegistrationSteps}" />
        
        <!-- Form content area -->
        <ScrollView Grid.Column="1" Padding="20">
            <ContentView Content="{Binding CurrentForm}" />
        </ScrollView>
    </Grid>
    
</ContentPage>
```

### Example 3: Responsive Orientation (Adaptive)

Switch orientation based on screen size:

```csharp
public partial class AdaptivePage : ContentPage
{
    private SfStepProgressBar stepProgressBar;
    private ViewModel viewModel;
    
    public AdaptivePage()
    {
        InitializeComponent();
        
        viewModel = new ViewModel();
        
        stepProgressBar = new SfStepProgressBar()
        {
            ItemsSource = viewModel.StepProgressItem,
            ActiveStepIndex = 1
        };
        
        SetOrientation();
        
        this.SizeChanged += OnPageSizeChanged;
        this.Content = stepProgressBar;
    }
    
    private void OnPageSizeChanged(object sender, EventArgs e)
    {
        SetOrientation();
    }
    
    private void SetOrientation()
    {
        // Use horizontal for wide screens, vertical for narrow
        if (this.Width > 600)
        {
            stepProgressBar.Orientation = StepProgressBarOrientation.Horizontal;
            stepProgressBar.LabelPosition = LabelPosition.Bottom;
        }
        else
        {
            stepProgressBar.Orientation = StepProgressBarOrientation.Vertical;
            stepProgressBar.LabelPosition = LabelPosition.End;
        }
    }
}
```

## Comparison Table

| Aspect | Horizontal | Vertical |
|--------|------------|----------|
| **Default Label Position** | Bottom | End (Right) |
| **Best For** | Wide screens, headers | Sidebars, mobile |
| **Space Usage** | Uses width | Uses height |
| **Step Count** | 3-6 optimal | 6-10+ works well |
| **Progress Direction** | Left → Right | Top → Bottom |
| **Typical Use Cases** | Checkout, wizards | Registration, onboarding |

## Tips and Best Practices

### Tip 1: Choose Based on Content Area

**Horizontal:** When you have a large content area below/above the progress indicator.

**Vertical:** When you have a wide content area beside the progress indicator (sidebar pattern).

### Tip 2: Consider Step Count

**Few steps (3-5):** Either orientation works, horizontal is more common.

**Many steps (6+):** Vertical prevents horizontal scrolling and cramping.

### Tip 3: Mobile Considerations

For mobile apps, vertical orientation often works better:
- Uses vertical scrolling (natural on mobile)
- Doesn't require horizontal cramming
- Labels can be longer without truncation

### Tip 4: Match Your Design System

If your app uses:
- **Top/bottom navigation bars:** Use horizontal progress bars
- **Sidebar navigation:** Use vertical progress bars
- **Tabbed interface:** Horizontal usually fits better

### Edge Case: RTL (Right-to-Left) Languages

For RTL languages (Arabic, Hebrew), horizontal orientation automatically reverses:
- Progress flows right → left
- Set `FlowDirection="RightToLeft"` on the StepProgressBar

```xml
<stepProgressBar:SfStepProgressBar 
    Orientation="Horizontal"
    FlowDirection="RightToLeft"
    ItemsSource="{Binding StepProgressItem}">
</stepProgressBar:SfStepProgressBar>
```

Vertical orientation is unaffected by RTL.

### Performance Note

Both orientations have similar performance characteristics. Choice should be based on UX, not performance.