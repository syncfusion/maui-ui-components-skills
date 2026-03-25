# Customization in StepProgressBar

## Table of Contents
- [Overview](#overview)
- [Step Shape Customization](#step-shape-customization)
- [Step Content Types](#step-content-types)
- [Animation Duration](#animation-duration)
- [Progress Bar Background](#progress-bar-background)
- [Custom Progress Track Size](#custom-progress-track-size)
- [Step Appearance with StepSettings](#step-appearance-with-stepsettings)
- [Custom Step Templates](#custom-step-templates)
- [Custom Label Templates](#custom-label-templates)
- [Complete Customization Examples](#complete-customization-examples)
- [Best Practices](#best-practices)

## Overview

The StepProgressBar offers extensive customization capabilities for visual appearance, animations, templates, and per-step styling. You can customize shapes, colors, content types, progress indicators, and create fully custom templates for advanced scenarios.

## Step Shape Customization

Control the shape of step indicators using the `ShapeType` property in `StepSettings`.

### Available Shapes

- **Circle**: Rounded step indicators (default)
- **Square**: Rectangular step indicators with sharp corners

### Setting Shape Type

**XAML:**
```xml
<stepProgressBar:SfStepProgressBar ItemsSource="{Binding StepProgressItem}"
                                   ActiveStepIndex="2" 
                                   ActiveStepProgressValue="50">
    
    <stepProgressBar:SfStepProgressBar.InProgressStepSettings>
        <stepProgressBar:StepSettings 
            Background="#ff67579c" 
            ContentType="Dot" 
            ShapeType="Square" 
            ContentFillColor="White"/>
    </stepProgressBar:SfStepProgressBar.InProgressStepSettings>
    
    <stepProgressBar:SfStepProgressBar.CompletedStepSettings>
        <stepProgressBar:StepSettings 
            Background="#ff67579c" 
            ContentType="Tick" 
            ShapeType="Square" 
            ContentFillColor="White"/>
    </stepProgressBar:SfStepProgressBar.CompletedStepSettings>
    
    <stepProgressBar:SfStepProgressBar.NotStartedStepSettings>
        <stepProgressBar:StepSettings 
            Background="#ff67579c" 
            ContentType="Cross" 
            ShapeType="Square" 
            ContentFillColor="White"/>
    </stepProgressBar:SfStepProgressBar.NotStartedStepSettings>
    
</stepProgressBar:SfStepProgressBar>
```

**C#:**
```csharp
SfStepProgressBar stepProgressBar = new SfStepProgressBar()
{
    ActiveStepIndex = 2,
    ActiveStepProgressValue = 50,
};

StepSettings inProgressStepSettings = new StepSettings()
{
    Background = Color.FromHex("#ff67579c"),
    ContentType = StepContentType.Dot,
    ShapeType = StepShapeType.Square,
    ContentFillColor = Colors.White
};

StepSettings completedStepSettings = new StepSettings()
{
    Background = Color.FromHex("#ff67579c"),
    ContentType = StepContentType.Tick,
    ShapeType = StepShapeType.Square,
    ContentFillColor = Colors.White
};

StepSettings notStartedStepSettings = new StepSettings()
{
    Background = Color.FromHex("#ff67579c"),
    ContentType = StepContentType.Cross,
    ShapeType = StepShapeType.Square,
    ContentFillColor = Colors.White
};

stepProgressBar.InProgressStepSettings = inProgressStepSettings;
stepProgressBar.CompletedStepSettings = completedStepSettings;
stepProgressBar.NotStartedStepSettings = notStartedStepSettings;

this.Content = stepProgressBar;
```

### When to Use Each Shape

**Circle:**
- Modern, friendly appearance
- Standard for most progress indicators
- Works well with all content types

**Square:**
- Professional, formal look
- Good for business/enterprise apps
- Creates a more structured visual hierarchy

## Step Content Types

Customize what appears inside each step indicator using the `ContentType` property.

### Available Content Types

- **Numbering**: Sequential numbers (1, 2, 3...)
- **Tick**: Checkmark symbol ✓
- **Cross**: X symbol ✗
- **Dot**: Small filled circle •
- **None**: Empty (use with images via ImageSource)

### Setting Content Types

**XAML:**
```xml
<stepProgressBar:SfStepProgressBar x:Name="stepProgressBar"
                                   StepSize="40"
                                   StepContentSize="25"
                                   ActiveStepIndex="1"
                                   ActiveStepProgressValue="60">
    
    <stepProgressBar:SfStepProgressBar.InProgressStepSettings>
        <stepProgressBar:StepSettings 
            Background="#ff67579c" 
            ContentType="Dot" 
            ContentFillColor="White"/>
    </stepProgressBar:SfStepProgressBar.InProgressStepSettings>
    
    <stepProgressBar:SfStepProgressBar.CompletedStepSettings>
        <stepProgressBar:StepSettings 
            Background="#ff67579c" 
            ContentType="Tick" 
            ContentFillColor="White"/>
    </stepProgressBar:SfStepProgressBar.CompletedStepSettings>
    
    <stepProgressBar:SfStepProgressBar.NotStartedStepSettings>
        <stepProgressBar:StepSettings 
            Background="#ff67579c" 
            ContentType="Cross" 
            ContentFillColor="White"/>
    </stepProgressBar:SfStepProgressBar.NotStartedStepSettings>
    
</stepProgressBar:SfStepProgressBar>
```

**C#:**
```csharp
SfStepProgressBar stepProgressBar = new SfStepProgressBar()
{
    StepContentSize = 25,
    StepSize = 40,
    ActiveStepIndex = 1,
    ActiveStepProgressValue = 60
};

StepSettings inProgressStepSettings = new StepSettings()
{
    Background = Color.FromHex("#ff67579c"),
    ContentType = StepContentType.Dot,
    ContentFillColor = Colors.White
};

StepSettings completedStepSettings = new StepSettings()
{
    Background = Color.FromHex("#ff67579c"),
    ContentType = StepContentType.Tick,
    ContentFillColor = Colors.White
};

StepSettings notStartedStepSettings = new StepSettings()
{
    Background = Color.FromHex("#ff67579c"),
    ContentType = StepContentType.Cross,
    ContentFillColor = Colors.White
};

stepProgressBar.InProgressStepSettings = inProgressStepSettings;
stepProgressBar.CompletedStepSettings = completedStepSettings;
stepProgressBar.NotStartedStepSettings = notStartedStepSettings;

this.Content = stepProgressBar;
```

### Recommended Patterns

**Standard completion pattern:**
- Completed: `Tick` (✓)
- In Progress: `Dot` (•)
- Not Started: `Numbering` or `None`

**Error indication pattern:**
- Completed: `Tick` (✓)
- In Progress: `Dot` (•)
- Failed/Canceled: `Cross` (✗)

**Sequential steps pattern:**
- All states: `Numbering` with different colors

## Animation Duration

Control the speed of progress animations using `ProgressAnimationDuration`.

**Default:** 1000 milliseconds (1 second)

**XAML:**
```xml
<stepProgressBar:SfStepProgressBar 
    x:Name="stepProgressBar"
    ProgressBarBackground="LightBlue" 
    ProgressAnimationDuration="2000"
    ActiveStepIndex="1" 
    ActiveStepProgressValue="40">
</stepProgressBar:SfStepProgressBar>
```

**C#:**
```csharp
SfStepProgressBar stepProgressBar = new SfStepProgressBar()
{
    ActiveStepIndex = 1,
    ActiveStepProgressValue = 50,
    ProgressBarBackground = Colors.LightBlue,
    ProgressAnimationDuration = 2000  // 2 seconds
};

this.Content = stepProgressBar;
```

### Animation Duration Guidelines

- **Fast (500-800ms)**: Quick transitions, data-driven updates
- **Standard (1000-1500ms)**: Default, balanced feel
- **Smooth (2000-3000ms)**: Emphasis on progress, onboarding flows
- **Slow (3000+ ms)**: Special effects, ceremonial completions

**Warning:** Durations > 4000ms may feel sluggish to users.

## Progress Bar Background

Customize the connector line color between steps.

**XAML:**
```xml
<stepProgressBar:SfStepProgressBar 
    x:Name="stepProgressBar"
    ProgressBarBackground="LightBlue">
</stepProgressBar:SfStepProgressBar>
```

**C#:**
```csharp
SfStepProgressBar stepProgressBar = new SfStepProgressBar()
{
    ProgressBarBackground = Colors.LightBlue
};

this.Content = stepProgressBar;
```

**Use Cases:**
- Match brand colors
- Create visual hierarchy
- Improve contrast in light/dark themes

## Custom Progress Track Size

Set custom height for the progress connector of individual steps using `ProgressTrackSize`.

**Default:** 50 pixels

### Per-Step Custom Heights

**ViewModel:**
```csharp
public class ViewModel
{
    public ObservableCollection<StepProgressBarItem> StepProgressItem { get; set; }
    
    public ViewModel()
    {
        StepProgressItem = new ObservableCollection<StepProgressBarItem>();
        
        StepProgressItem.Add(new StepProgressBarItem() 
        { 
            PrimaryText = "Requirement Gathering", 
            ProgressTrackSize = 150  // Tall connector
        });
        
        StepProgressItem.Add(new StepProgressBarItem() 
        { 
            PrimaryText = "Design", 
            ProgressTrackSize = 30   // Short connector
        });
        
        StepProgressItem.Add(new StepProgressBarItem() 
        { 
            PrimaryText = "Development", 
            ProgressTrackSize = 80   // Medium connector
        });
        
        StepProgressItem.Add(new StepProgressBarItem() 
        { 
            PrimaryText = "Testing", 
            ProgressTrackSize = 50   // Default height
        });
        
        StepProgressItem.Add(new StepProgressBarItem() 
        { 
            PrimaryText = "Deployment"
            // Last step: ProgressTrackSize has no effect
        });
    }
}
```

**XAML:**
```xml
<stepProgressBar:SfStepProgressBar
    x:Name="stepProgress"
    Orientation="Vertical"
    ActiveStepIndex="3"
    ActiveStepProgressValue="50"
    ItemsSource="{Binding StepProgressItem}">
</stepProgressBar:SfStepProgressBar>
```

**Important Notes:**
- Values < 1 are ignored (default used)
- Last step's `ProgressTrackSize` has no effect (no connector after it)
- Only applicable in vertical orientation for visual impact

## Step Appearance with StepSettings

Comprehensive styling for different step states using `InProgressStepSettings`, `CompletedStepSettings`, and `NotStartedStepSettings`.

### Available Properties

| Property | Type | Description |
|----------|------|-------------|
| `Background` | `Brush` | Step indicator background color |
| `ContentType` | `StepContentType` | Content inside step (Tick/Cross/Dot/Numbering) |
| `ShapeType` | `StepShapeType` | Step shape (Circle/Square) |
| `ContentFillColor` | `Color` | Color of content symbol |
| `ProgressColor` | `Color` | Color of progress connector line |
| `Stroke` | `Color` | Border color of step indicator |
| `TextStyle` | `StepTextStyle` | Text styling (color, font, size, attributes) |

### Complete Styling Example

**XAML:**
```xml
<stepProgressBar:SfStepProgressBar ItemsSource="{Binding StepProgressItem}"
                                   ActiveStepIndex="2" 
                                   ActiveStepProgressValue="50">
    
    <!-- In Progress Step Styling -->
    <stepProgressBar:SfStepProgressBar.InProgressStepSettings>
        <stepProgressBar:StepSettings 
            Background="#ff67579c" 
            ContentType="Dot" 
            ShapeType="Circle" 
            ProgressColor="Orange" 
            ContentFillColor="White" 
            Stroke="DarkViolet">
            <stepProgressBar:StepSettings.TextStyle>
                <stepProgressBar:StepTextStyle 
                    TextColor="CadetBlue"
                    FontAutoScalingEnabled="True"
                    FontFamily="Roboto"
                    FontSize="14"
                    FontAttributes="Bold"/>
            </stepProgressBar:StepSettings.TextStyle>
        </stepProgressBar:StepSettings>
    </stepProgressBar:SfStepProgressBar.InProgressStepSettings>
    
    <!-- Completed Step Styling -->
    <stepProgressBar:SfStepProgressBar.CompletedStepSettings>
        <stepProgressBar:StepSettings 
            Background="#ff67579c" 
            ContentType="Tick" 
            ShapeType="Circle" 
            ProgressColor="Orange" 
            ContentFillColor="White" 
            Stroke="DarkViolet">
            <stepProgressBar:StepSettings.TextStyle>
                <stepProgressBar:StepTextStyle 
                    TextColor="CadetBlue" 
                    FontAttributes="Bold"
                    FontAutoScalingEnabled="True"
                    FontFamily="Roboto"
                    FontSize="14"/>
            </stepProgressBar:StepSettings.TextStyle>
        </stepProgressBar:StepSettings>
    </stepProgressBar:SfStepProgressBar.CompletedStepSettings>
    
    <!-- Not Started Step Styling -->
    <stepProgressBar:SfStepProgressBar.NotStartedStepSettings>
        <stepProgressBar:StepSettings 
            Background="#ff67579c" 
            ContentType="Cross" 
            ShapeType="Circle" 
            ProgressColor="Orange" 
            ContentFillColor="White" 
            Stroke="DarkViolet">
            <stepProgressBar:StepSettings.TextStyle>
                <stepProgressBar:StepTextStyle 
                    TextColor="CadetBlue"
                    FontAutoScalingEnabled="True"
                    FontFamily="Roboto"
                    FontAttributes="Bold"/>
            </stepProgressBar:StepSettings.TextStyle>
        </stepProgressBar:StepSettings>
    </stepProgressBar:SfStepProgressBar.NotStartedStepSettings>
    
</stepProgressBar:SfStepProgressBar>
```

## Custom Step Templates

Use `StepTemplate` with `DataTemplate` for complete visual control over step appearance.

### Basic Step Template

**XAML:**
```xml
<stepProgressBar:SfStepProgressBar 
    x:Name="stepProgress"
    Orientation="Horizontal"
    ActiveStepIndex="3"
    ItemsSource="{Binding StepProgressItem}">
    
    <stepProgressBar:SfStepProgressBar.StepTemplate>
        <DataTemplate>
            <Grid>
                <Image Source="tick.png"/>
            </Grid>
        </DataTemplate>
    </stepProgressBar:SfStepProgressBar.StepTemplate>
    
</stepProgressBar:SfStepProgressBar>
```

**C#:**
```csharp
SfStepProgressBar stepProgressBar = new SfStepProgressBar();
ViewModel viewModel = new ViewModel();
stepProgressBar.ActiveStepIndex = 3;
stepProgressBar.Orientation = StepProgressBarOrientation.Horizontal;
stepProgressBar.ItemsSource = viewModel.StepProgressItem;

var stepTemplate = new DataTemplate(() =>
{
    var grid = new Grid();
    var image = new Image { Source = "tick.png" };
    grid.Children.Add(image);
    return grid;
});

stepProgressBar.StepTemplate = stepTemplate;
this.Content = stepProgressBar;
```

### DataTemplateSelector for Conditional Templates

Create different templates for different steps:

**Template Selector Class:**
```csharp
public class StepTemplateSelector : DataTemplateSelector
{
    public DataTemplate Template1 { get; set; }
    public DataTemplate Template2 { get; set; }
    public DataTemplate Template3 { get; set; }
    public DataTemplate Template4 { get; set; }
    
    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        var stepDetails = item as StepProgressBarItem;
        
        if (stepDetails.PrimaryText == "Ordered")
            return Template1;
        else if (stepDetails.PrimaryText == "Packed")
            return Template2;
        else if (stepDetails.PrimaryText == "Shipped")
            return Template3;
        else
            return Template4;
    }
}
```

**XAML with Selector:**
```xml
<Grid>
    <Grid.Resources>
        <DataTemplate x:Key="template1">
            <Grid>
                <Image Source="ordered.png" 
                       HorizontalOptions="Center" 
                       VerticalOptions="Center"/>
            </Grid>
        </DataTemplate>
        
        <DataTemplate x:Key="template2">
            <Grid>
                <Image Source="packed.png" 
                       HorizontalOptions="Center" 
                       VerticalOptions="Center"/>
            </Grid>
        </DataTemplate>
        
        <DataTemplate x:Key="template3">
            <Grid>
                <Image Source="shipped.png" 
                       HorizontalOptions="Center" 
                       VerticalOptions="Center"/>
            </Grid>
        </DataTemplate>
        
        <DataTemplate x:Key="template4">
            <Grid>
                <Image Source="delivered.png" 
                       HorizontalOptions="Center" 
                       VerticalOptions="Center"/>
            </Grid>
        </DataTemplate>
        
        <local:StepTemplateSelector 
            x:Key="stepTemplateSelector" 
            Template1="{StaticResource template1}" 
            Template2="{StaticResource template2}" 
            Template3="{StaticResource template3}"
            Template4="{StaticResource template4}"/>
    </Grid.Resources>
    
    <stepProgressBar:SfStepProgressBar 
        x:Name="stepProgress"
        Orientation="Horizontal"
        ActiveStepIndex="3"
        ItemsSource="{Binding StepProgressItem}"
        StepTemplate="{StaticResource stepTemplateSelector}"/>
</Grid>
```

**Important:** When using `StepTemplate`, these properties are ignored:
- `ShapeType`
- `Background`
- `ContentType`
- `ContentFillColor`
- `Stroke`

Template renders based on `StepSize` property.

## Custom Label Templates

Customize primary and secondary text appearance using templates.

### PrimaryTextTemplate Example

**XAML:**
```xml
<stepProgressBar:SfStepProgressBar 
    x:Name="stepProgress"
    Orientation="Horizontal"
    ActiveStepIndex="3"
    ItemsSource="{Binding StepProgressItem}">
    
    <stepProgressBar:SfStepProgressBar.PrimaryTextTemplate>
        <DataTemplate>
            <StackLayout Orientation="Vertical">
                <Image Source="tick.png" 
                       HorizontalOptions="Center" 
                       WidthRequest="20" 
                       HeightRequest="20"/>
                <Label Text="{Binding PrimaryText}" 
                       HorizontalOptions="Center"/>
            </StackLayout>
        </DataTemplate>
    </stepProgressBar:SfStepProgressBar.PrimaryTextTemplate>
    
</stepProgressBar:SfStepProgressBar>
```

**C#:**
```csharp
SfStepProgressBar stepProgressBar = new SfStepProgressBar();
ViewModel viewModel = new ViewModel();
stepProgressBar.ItemsSource = viewModel.StepProgressItem;
stepProgressBar.Orientation = StepProgressBarOrientation.Horizontal;
stepProgressBar.ActiveStepIndex = 3;

var primaryTextTemplate = new DataTemplate(() =>
{
    var stackLayout = new StackLayout
    {
        Orientation = StackOrientation.Vertical
    };
    
    var image = new Image
    {
        Source = "tick.png",
        HorizontalOptions = LayoutOptions.Center,
        WidthRequest = 20,
        HeightRequest = 20
    };
    
    var label = new Label
    {
        HorizontalOptions = LayoutOptions.Center
    };
    label.SetBinding(Label.TextProperty, "PrimaryText");
    
    stackLayout.Children.Add(image);
    stackLayout.Children.Add(label);
    
    return stackLayout;
});

stepProgressBar.PrimaryTextTemplate = primaryTextTemplate;
this.Content = stepProgressBar;
```

### DataTemplateSelector for Labels

**Selector Class:**
```csharp
public class PrimaryTemplateSelector : DataTemplateSelector
{
    public DataTemplate PrimaryTemplate1 { get; set; }
    public DataTemplate PrimaryTemplate2 { get; set; }
    public DataTemplate PrimaryTemplate3 { get; set; }
    public DataTemplate PrimaryTemplate4 { get; set; }
    
    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        var stepDetails = item as StepProgressBarItem;
        
        if (stepDetails.PrimaryText == "Ordered")
            return PrimaryTemplate1;
        else if (stepDetails.PrimaryText == "Packed")
            return PrimaryTemplate2;
        else if (stepDetails.PrimaryText == "Shipped")
            return PrimaryTemplate3;
        else
            return PrimaryTemplate4;
    }
}
```

**XAML:**
```xml
<Grid>
    <Grid.Resources>
        <DataTemplate x:Key="primaryTemplate1">
            <StackLayout Orientation="Vertical">
                <Image Source="ordered.png" 
                       HorizontalOptions="Center" 
                       WidthRequest="30" 
                       HeightRequest="30"/>
                <Label Text="{Binding PrimaryText}" 
                       HorizontalOptions="Center"/>
            </StackLayout>
        </DataTemplate>
        
        <!-- Define primaryTemplate2, primaryTemplate3, primaryTemplate4 similarly -->
        
        <local:PrimaryTemplateSelector 
            x:Key="primaryTemplateSelector" 
            PrimaryTemplate1="{StaticResource primaryTemplate1}" 
            PrimaryTemplate2="{StaticResource primaryTemplate2}" 
            PrimaryTemplate3="{StaticResource primaryTemplate3}"
            PrimaryTemplate4="{StaticResource primaryTemplate4}"/>
    </Grid.Resources>
    
    <stepProgressBar:SfStepProgressBar 
        x:Name="stepProgress"
        Orientation="Horizontal"
        ActiveStepIndex="3"
        ItemsSource="{Binding StepProgressItem}"
        PrimaryTextTemplate="{StaticResource primaryTemplateSelector}">
    </stepProgressBar:SfStepProgressBar>
</Grid>
```

**Important:** When using `PrimaryTextTemplate` or `SecondaryTextTemplate`, these properties are ignored:
- `TextColor` (from StepTextStyle)
- `FontSize`
- `FontFamily`
- `FontAttributes`

## Complete Customization Examples

### Example 1: Brand-Colored Progress Bar

```xml
<stepProgressBar:SfStepProgressBar 
    ItemsSource="{Binding StepProgressItem}"
    ActiveStepIndex="2"
    ProgressAnimationDuration="1500"
    ProgressBarBackground="#E0E0E0">
    
    <stepProgressBar:SfStepProgressBar.CompletedStepSettings>
        <stepProgressBar:StepSettings 
            Background="#4CAF50"
            ContentType="Tick"
            ContentFillColor="White"
            ProgressColor="#4CAF50"
            ShapeType="Circle">
            <stepProgressBar:StepSettings.TextStyle>
                <stepProgressBar:StepTextStyle 
                    TextColor="#4CAF50"
                    FontAttributes="Bold"
                    FontSize="14"/>
            </stepProgressBar:StepSettings.TextStyle>
        </stepProgressBar:StepSettings>
    </stepProgressBar:SfStepProgressBar.CompletedStepSettings>
    
    <stepProgressBar:SfStepProgressBar.InProgressStepSettings>
        <stepProgressBar:StepSettings 
            Background="#FF9800"
            ContentType="Dot"
            ContentFillColor="White"
            ProgressColor="#FF9800"
            ShapeType="Circle">
            <stepProgressBar:StepSettings.TextStyle>
                <stepProgressBar:StepTextStyle 
                    TextColor="#FF9800"
                    FontAttributes="Bold"
                    FontSize="14"/>
            </stepProgressBar:StepSettings.TextStyle>
        </stepProgressBar:StepSettings>
    </stepProgressBar:SfStepProgressBar.InProgressStepSettings>
    
    <stepProgressBar:SfStepProgressBar.NotStartedStepSettings>
        <stepProgressBar:StepSettings 
            Background="#BDBDBD"
            ContentType="Numbering"
            ContentFillColor="White"
            ProgressColor="#BDBDBD"
            ShapeType="Circle">
            <stepProgressBar:StepSettings.TextStyle>
                <stepProgressBar:StepTextStyle 
                    TextColor="#9E9E9E"
                    FontSize="14"/>
            </stepProgressBar:StepSettings.TextStyle>
        </stepProgressBar:StepSettings>
    </stepProgressBar:SfStepProgressBar.NotStartedStepSettings>
    
</stepProgressBar:SfStepProgressBar>
```

## Best Practices

### Practice 1: Consistent Visual Language

Use the same shape and content types across all step states for consistency. Only vary colors.

### Practice 2: Color Contrast

Ensure sufficient contrast between:
- Step background and content (checkmarks, dots)
- Progress color and page background
- Text color and page background

### Practice 3: Animation Duration

Match animation speed to user expectations:
- Quick updates (form validation): 500-1000ms
- Progress transitions (page navigation): 1000-2000ms
- Ceremonial completions: 2000-3000ms

### Practice 4: Template Performance

- Avoid complex nested layouts in templates
- Use simple images (< 50KB)
- Limit DataTemplateSelector conditions to < 10 cases

### Practice 5: Accessibility

When using custom templates:
- Include text labels for screen readers
- Ensure sufficient touch target size (44x44 minimum)
- Maintain clear visual hierarchy