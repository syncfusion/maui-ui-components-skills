# Custom Content in Circular ProgressBar

Add any MAUI view to the center of the Syncfusion .NET MAUI Circular ProgressBar using the `Content` property. Display text, images, buttons, or complex layouts at the center for enhanced user interaction and information display.

## Overview

The `Content` property allows you to place custom views at the center of the circular progress bar, enabling:
- Progress percentage displays
- Status text and labels
- Interactive buttons (start, pause, stop)
- Images and icons
- Complex layouts with multiple elements
- Data binding to progress values

## Content Property

**Type**: View (any MAUI view)  
**Default**: null (no center content)

## Basic Text Content

### Simple Label

**XAML:**
```xml
<progressBar:SfCircularProgressBar Progress="75">
    <progressBar:SfCircularProgressBar.Content>
        <Label Text="75%" 
               FontSize="24"
               FontAttributes="Bold"
               HorizontalTextAlignment="Center"
               VerticalTextAlignment="Center" />
    </progressBar:SfCircularProgressBar.Content>
</progressBar:SfCircularProgressBar>
```

**C#:**
```csharp
SfCircularProgressBar circularProgressBar = new SfCircularProgressBar
{
    Progress = 75
};

Label label = new Label
{
    Text = "75%",
    FontSize = 24,
    FontAttributes = FontAttributes.Bold,
    HorizontalTextAlignment = TextAlignment.Center,
    VerticalTextAlignment = TextAlignment.Center
};

circularProgressBar.Content = label;
this.Content = circularProgressBar;
```

## Data Binding to Progress

Bind label text to the progress value for automatic updates.

**XAML:**
```xml
<progressBar:SfCircularProgressBar x:Name="customContentCircularProgressBar" 
                                   Progress="23">
    <progressBar:SfCircularProgressBar.Content>
        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition />
                <RowDefinition />
            </Grid.RowDefinitions>
            
            <Label TextColor="#007cee"  
                   Text="{Binding Source={x:Reference customContentCircularProgressBar}, Path=Progress, StringFormat='{0}%'}"
                   HorizontalTextAlignment="Center" 
                   VerticalTextAlignment="End"
                   FontSize="20"
                   FontAttributes="Bold" />
            
            <Label Grid.Row="1" 
                   TextColor="#007cee" 
                   Text="used" 
                   VerticalOptions="Start" 
                   HorizontalTextAlignment="Center" 
                   VerticalTextAlignment="Start"
                   FontSize="12" />
        </Grid>
    </progressBar:SfCircularProgressBar.Content>
</progressBar:SfCircularProgressBar>
```

**C# with Binding:**
```csharp
SfCircularProgressBar circularProgressBar = new SfCircularProgressBar
{
    Progress = 23
};

Grid grid = new Grid();
grid.RowDefinitions.Add(new RowDefinition());
grid.RowDefinitions.Add(new RowDefinition());

Label progressLabel = new Label
{
    BindingContext = circularProgressBar,
    HorizontalTextAlignment = TextAlignment.Center,
    VerticalOptions = LayoutOptions.End,
    TextColor = Color.FromArgb("#007cee"),
    FontSize = 20,
    FontAttributes = FontAttributes.Bold
};

Binding binding = new Binding
{
    Path = "Progress",
    StringFormat = "{0}%"
};
progressLabel.SetBinding(Label.TextProperty, binding);

Grid.SetRow(progressLabel, 0);
grid.Children.Add(progressLabel);

Label textLabel = new Label
{
    Text = "used",
    HorizontalTextAlignment = TextAlignment.Center,
    VerticalOptions = LayoutOptions.Start,
    TextColor = Color.FromArgb("#007cee"),
    FontSize = 12
};

Grid.SetRow(textLabel, 1);
grid.Children.Add(textLabel);

circularProgressBar.Content = grid;

Content = new StackLayout
{
    Padding = 40,
    Children = { circularProgressBar }
};
```

## Interactive Buttons

### Play/Pause Button

```xml
<progressBar:SfCircularProgressBar x:Name="controlProgressBar" 
                                   Progress="0">
    <progressBar:SfCircularProgressBar.Content>
        <Button x:Name="controlButton"
                Text="▶"
                FontSize="30"
                BackgroundColor="Transparent"
                TextColor="#007cee"
                Clicked="ControlButton_Clicked"
                WidthRequest="60"
                HeightRequest="60" />
    </progressBar:SfCircularProgressBar.Content>
</progressBar:SfCircularProgressBar>
```

```csharp
private bool isRunning = false;

private void ControlButton_Clicked(object sender, EventArgs e)
{
    if (!isRunning)
    {
        controlButton.Text = "⏸";
        StartProgress();
        isRunning = true;
    }
    else
    {
        controlButton.Text = "▶";
        isRunning = false;
    }
}

private void StartProgress()
{
    Device.StartTimer(TimeSpan.FromMilliseconds(100), () =>
    {
        if (isRunning && controlProgressBar.Progress < 100)
        {
            controlProgressBar.Progress += 1;
            return true;
        }
        return false;
    });
}
```

## Images and Icons

### Image at Center

```xml
<progressBar:SfCircularProgressBar Progress="65">
    <progressBar:SfCircularProgressBar.Content>
        <Image Source="download_icon.png"
               WidthRequest="50"
               HeightRequest="50" />
    </progressBar:SfCircularProgressBar.Content>
</progressBar:SfCircularProgressBar>
```

## Complex Layouts

### Grid Layout with Multiple Elements

```xml
<progressBar:SfCircularProgressBar x:Name="dashboardProgress" 
                                   Progress="75"
                                   HeightRequest="200"
                                   WidthRequest="200">
    <progressBar:SfCircularProgressBar.Content>
        <Grid RowSpacing="5" ColumnSpacing="10">
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto" />
                <RowDefinition Height="Auto" />
                <RowDefinition Height="Auto" />
            </Grid.RowDefinitions>
            
            <Label Grid.Row="0"
                   Text="Dashboard"
                   FontSize="12"
                   FontAttributes="Bold"
                   HorizontalTextAlignment="Center" />
            
            <Label Grid.Row="1"
                   Text="Pending"
                   FontSize="32"
                   FontAttributes="Bold"
                   TextColor="#FF9800"
                   HorizontalTextAlignment="Center" />
            
            <Label Grid.Row="2"
                   Text="Completed"
                   FontSize="10"
                   TextColor="Gray"
                   HorizontalTextAlignment="Center" />
        </Grid>
    </progressBar:SfCircularProgressBar.Content>
</progressBar:SfCircularProgressBar>
```

## Dynamic Content Updates

### Updating Content Programmatically

```csharp
public partial class DynamicContentPage : ContentPage
{
    private SfCircularProgressBar progressBar;
    private Label statusLabel;
    private Label progressLabel;
    
    public DynamicContentPage()
    {
        InitializeComponent();
        
        progressBar = new SfCircularProgressBar { Progress = 0 };
        
        var stackLayout = new StackLayout { Spacing = 10 };
        
        progressLabel = new Label
        {
            Text = "0%",
            FontSize = 24,
            FontAttributes = FontAttributes.Bold,
            HorizontalTextAlignment = TextAlignment.Center
        };
        
        statusLabel = new Label
        {
            Text = "Ready",
            FontSize = 12,
            HorizontalTextAlignment = TextAlignment.Center,
            TextColor = Colors.Gray
        };
        
        stackLayout.Children.Add(progressLabel);
        stackLayout.Children.Add(statusLabel);
        
        progressBar.Content = stackLayout;
        Content = progressBar;
    }
}
```

## Common Patterns

### Pattern 1: Timer Display

```xml
<progressBar:SfCircularProgressBar x:Name="timerProgress" 
                                   Minimum="0"
                                   Maximum="60"
                                   Progress="45">
    <progressBar:SfCircularProgressBar.Content>
        <StackLayout>
            <Label x:Name="timerLabel"
                   Text="00:45"
                   FontSize="28"
                   FontFamily="Courier"
                   FontAttributes="Bold"
                   HorizontalTextAlignment="Center" />
            <Label Text="seconds"
                   FontSize="12"
                   TextColor="Gray"
                   HorizontalTextAlignment="Center" />
        </StackLayout>
    </progressBar:SfCircularProgressBar.Content>
</progressBar:SfCircularProgressBar>
```

### Pattern 2: Score Display

```xml
<progressBar:SfCircularProgressBar Minimum="0" 
                                   Maximum="100" 
                                   Progress="85">
    <progressBar:SfCircularProgressBar.Content>
        <StackLayout Spacing="5">
            <Label Text="85"
                   FontSize="36"
                   FontAttributes="Bold"
                   TextColor="#00C851"
                   HorizontalTextAlignment="Center" />
            <HorizontalStackLayout HorizontalOptions="Center" Spacing="5">
                <Label Text="★" FontSize="16" TextColor="#FFD700" />
                <Label Text="★" FontSize="16" TextColor="#FFD700" />
                <Label Text="★" FontSize="16" TextColor="#FFD700" />
                <Label Text="★" FontSize="16" TextColor="#FFD700" />
                <Label Text="☆" FontSize="16" TextColor="#FFD700" />
            </HorizontalStackLayout>
            <Label Text="Score"
                   FontSize="12"
                   TextColor="Gray"
                   HorizontalTextAlignment="Center" />
        </StackLayout>
    </progressBar:SfCircularProgressBar.Content>
</progressBar:SfCircularProgressBar>
```

### Pattern 3: Multi-Value Display

```xml
<progressBar:SfCircularProgressBar x:Name="multiValueProgress" 
                                   Progress="73">
    <progressBar:SfCircularProgressBar.Content>
        <Grid RowSpacing="3">
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto" />
                <RowDefinition Height="Auto" />
                <RowDefinition Height="Auto" />
            </Grid.RowDefinitions>
            
            <Label Grid.Row="0"
                   Text="Storage"
                   FontSize="10"
                   TextColor="Gray"
                   HorizontalTextAlignment="Center" />
            
            <Label Grid.Row="1"
                   Text="365 GB"
                   FontSize="22"
                   FontAttributes="Bold"
                   HorizontalTextAlignment="Center" />
            
            <Label Grid.Row="2"
                   Text="of 500 GB"
                   FontSize="12"
                   TextColor="Gray"
                   HorizontalTextAlignment="Center" />
        </Grid>
    </progressBar:SfCircularProgressBar.Content>
</progressBar:SfCircularProgressBar>
```

### Pattern 4: Interactive Control Panel

```xml
<progressBar:SfCircularProgressBar x:Name="interactiveProgress" 
                                   Progress="0"
                                   HeightRequest="250"
                                   WidthRequest="250">
    <progressBar:SfCircularProgressBar.Content>
        <StackLayout Spacing="15">
            <Label x:Name="progressPercentLabel"
                   Text="0%"
                   FontSize="32"
                   FontAttributes="Bold"
                   HorizontalTextAlignment="Center" />
            
            <HorizontalStackLayout HorizontalOptions="Center" Spacing="10">
                <Button Text="◀"
                        FontSize="20"
                        BackgroundColor="Transparent"
                        TextColor="#2196F3"
                        Clicked="DecreaseProgress_Clicked"
                        WidthRequest="50" />
                
                <Button x:Name="playPauseButton"
                        Text="▶"
                        FontSize="20"
                        BackgroundColor="#2196F3"
                        TextColor="White"
                        CornerRadius="25"
                        Clicked="PlayPause_Clicked"
                        WidthRequest="50"
                        HeightRequest="50" />
                
                <Button Text="▶"
                        FontSize="20"
                        BackgroundColor="Transparent"
                        TextColor="#2196F3"
                        Clicked="IncreaseProgress_Clicked"
                        WidthRequest="50" />
            </HorizontalStackLayout>
        </StackLayout>
    </progressBar:SfCircularProgressBar.Content>
</progressBar:SfCircularProgressBar>
```

```csharp
private void IncreaseProgress_Clicked(object sender, EventArgs e)
{
    if (interactiveProgress.Progress < 100)
    {
        interactiveProgress.Progress += 10;
        UpdateProgressLabel();
    }
}

private void DecreaseProgress_Clicked(object sender, EventArgs e)
{
    if (interactiveProgress.Progress > 0)
    {
        interactiveProgress.Progress -= 10;
        UpdateProgressLabel();
    }
}

private void UpdateProgressLabel()
{
    progressPercentLabel.Text = $"{interactiveProgress.Progress:F0}%";
}
```

## Best Practices

### Layout Guidelines
1. **Keep it centered**: Use `HorizontalTextAlignment` and `VerticalTextAlignment` for proper centering
2. **Size appropriately**: Content should fit within the inner circle radius
3. **Use StackLayout or Grid**: These containers work best for center content
4. **Test different sizes**: Ensure content scales well on various screen sizes

### Content Sizing
1. **Small progress bars (<150px)**: Single label or small icon
2. **Medium progress bars (150-250px)**: Text + icon or 2-3 text lines
3. **Large progress bars (>250px)**: Complex layouts with multiple elements

### Data Binding
1. **Bind to Progress**: Use `{Binding Source={x:Reference...}, Path=Progress}` for auto-updates
2. **String formatting**: Apply `StringFormat='{0:F0}%'` for clean percentage display
3. **Update manually**: Use code-behind for complex formatting or calculations

### Interactivity
1. **Buttons**: Add interactive controls for user input
2. **Touch gestures**: Consider adding tap gestures to content
3. **Accessibility**: Ensure interactive elements are accessible

### Performance
1. **Avoid complex layouts**: Keep content simple for smooth animation
2. **Minimize updates**: Update content only when necessary
3. **Use binding**: Leverage data binding instead of frequent manual updates

## Summary

The `Content` property enables rich customization of the circular progress bar center:
- **Text displays**: Progress percentages, status messages, values
- **Images**: Icons, logos, task indicators
- **Buttons**: Interactive controls (play, pause, start)
- **Complex layouts**: Multi-element displays with Grid or StackLayout
- **Data binding**: Automatic updates tied to progress value

Use center content to enhance user understanding, provide context, and enable interaction with your progress indicators.