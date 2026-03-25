# Inline Rendering

Inline rendering embeds the color picker directly into your UI layout instead of showing it as a popup/dropdown. This is ideal for scenarios where you want the color picker to be always visible or integrated into a larger form or settings panel.

## Overview

By default, the SfColorPicker displays as a compact button that opens a popup when clicked. Setting `IsInline="True"` changes this behavior to render the full color picker interface directly in your layout.

**Default (Popup Mode):** Compact button → Click → Popup opens  
**Inline Mode:** Full color picker always visible in layout

## When to Use Inline Mode

Use inline mode when:
- **Always-visible color selection** is needed
- Building a **settings panel** or **configuration UI**
- Creating a **color customization dashboard**
- Space is available for the full picker interface
- You want to avoid popup/modal interactions
- Building **tablet or desktop-first** layouts with ample space

Avoid inline mode when:
- Screen space is limited (mobile-first)
- Color selection is infrequent
- You need a compact UI
- Building forms with many inputs

## Basic Implementation

### XAML

```xaml
<ContentPage 
    xmlns:inputs="clr-namespace:Syncfusion.Maui.Inputs;assembly=Syncfusion.Maui.Inputs">
    
    <inputs:SfColorPicker IsInline="True"/>
    
</ContentPage>
```

### C#

```csharp
using Syncfusion.Maui.Inputs;

SfColorPicker colorPicker = new SfColorPicker()
{
    IsInline = true
};

Content = colorPicker;
```

## Complete Examples

### Example 1: Inline Spectrum Mode

```xaml
<VerticalStackLayout Padding="20">
    <Label Text="Choose a color:"
           FontSize="18"
           FontAttributes="Bold"
           Margin="0,0,0,10"/>
    
    <inputs:SfColorPicker IsInline="True"
                          ColorMode="Spectrum"
                          SelectedColor="DodgerBlue"/>
</VerticalStackLayout>
```

### Example 2: Inline Palette Mode

```xaml
<VerticalStackLayout Padding="20">
    <Label Text="Select from palette:"
           FontSize="18"
           FontAttributes="Bold"
           Margin="0,0,0,10"/>
    
    <inputs:SfColorPicker IsInline="True"
                          ColorMode="Palette"
                          ShowRecentColors="True"
                          SelectedColor="MediumSeaGreen"/>
</VerticalStackLayout>
```

### Example 3: Inline Without Action Buttons

When inline, action buttons are often unnecessary since changes can apply immediately:

```xaml
<inputs:SfColorPicker IsInline="True"
                      ColorMode="Spectrum"
                      IsActionButtonsVisible="False"
                      ColorChanged="OnColorChanged"/>
```

```csharp
private void OnColorChanged(object sender, ColorChangedEventArgs e)
{
    // Apply color immediately as user makes selections
    targetElement.BackgroundColor = e.NewColor;
}
```

### Example 4: Settings Panel with Inline Picker

```xaml
<ScrollView>
    <VerticalStackLayout Padding="20" Spacing="20">
        
        <!-- Background Color Setting -->
        <Frame BorderColor="LightGray" Padding="15">
            <VerticalStackLayout Spacing="15">
                <Label Text="Background Color"
                       FontSize="16"
                       FontAttributes="Bold"/>
                
                <inputs:SfColorPicker x:Name="backgroundColorPicker"
                                      IsInline="True"
                                      ColorMode="Palette"
                                      IsActionButtonsVisible="False"
                                      ShowRecentColors="True"
                                      SelectedColor="White"
                                      ColorChanged="OnBackgroundColorChanged"/>
            </VerticalStackLayout>
        </Frame>
        
        <!-- Text Color Setting -->
        <Frame BorderColor="LightGray" Padding="15">
            <VerticalStackLayout Spacing="15">
                <Label Text="Text Color"
                       FontSize="16"
                       FontAttributes="Bold"/>
                
                <inputs:SfColorPicker x:Name="textColorPicker"
                                      IsInline="True"
                                      ColorMode="Palette"
                                      IsActionButtonsVisible="False"
                                      ShowRecentColors="True"
                                      SelectedColor="Black"
                                      ColorChanged="OnTextColorChanged"/>
            </VerticalStackLayout>
        </Frame>
        
        <!-- Preview -->
        <Frame Padding="20" 
               BackgroundColor="{Binding Source={x:Reference backgroundColorPicker}, Path=SelectedColor}">
            <Label x:Name="previewLabel"
                   Text="Preview Text"
                   FontSize="24"
                   HorizontalOptions="Center"
                   TextColor="{Binding Source={x:Reference textColorPicker}, Path=SelectedColor}"/>
        </Frame>
        
    </VerticalStackLayout>
</ScrollView>
```

### Example 5: Split View with Live Preview

```xaml
<Grid ColumnDefinitions="*,2*" Padding="20" ColumnSpacing="20">
    
    <!-- Color Picker Column -->
    <VerticalStackLayout Grid.Column="0">
        <Label Text="Color Selection"
               FontSize="18"
               FontAttributes="Bold"
               Margin="0,0,0,15"/>
        
        <inputs:SfColorPicker x:Name="colorPicker"
                              IsInline="True"
                              ColorMode="Spectrum"
                              IsActionButtonsVisible="False"
                              ColorChanged="OnColorChanged"/>
    </VerticalStackLayout>
    
    <!-- Preview Column -->
    <VerticalStackLayout Grid.Column="1" Spacing="15">
        <Label Text="Live Preview"
               FontSize="18"
               FontAttributes="Bold"/>
        
        <BoxView x:Name="previewBox"
                 HeightRequest="200"
                 CornerRadius="10"
                 BackgroundColor="DodgerBlue"/>
        
        <Label x:Name="hexLabel"
               Text="#1E90FF"
               FontSize="20"
               FontAttributes="Bold"
               HorizontalOptions="Center"/>
        
        <Grid ColumnDefinitions="*,*" RowDefinitions="Auto,Auto" ColumnSpacing="10" RowSpacing="5">
            <Label Grid.Row="0" Grid.Column="0" Text="Red:" FontSize="14"/>
            <Label Grid.Row="0" Grid.Column="1" x:Name="redLabel" Text="30" FontSize="14"/>
            
            <Label Grid.Row="1" Grid.Column="0" Text="Green:" FontSize="14"/>
            <Label Grid.Row="1" Grid.Column="1" x:Name="greenLabel" Text="144" FontSize="14"/>
            
            <Label Grid.Row="2" Grid.Column="0" Text="Blue:" FontSize="14"/>
            <Label Grid.Row="2" Grid.Column="1" x:Name="blueLabel" Text="255" FontSize="14"/>
        </Grid>
    </VerticalStackLayout>
    
</Grid>
```

```csharp
private void OnColorChanged(object sender, ColorChangedEventArgs e)
{
    Color color = e.NewColor;
    
    // Update preview
    previewBox.BackgroundColor = color;
    hexLabel.Text = color.ToHex();
    
    // Update RGB values
    redLabel.Text = ((int)(color.Red * 255)).ToString();
    greenLabel.Text = ((int)(color.Green * 255)).ToString();
    blueLabel.Text = ((int)(color.Blue * 255)).ToString();
}
```

## Inline Mode Configuration

### Disable Mode Switcher in Inline

When inline, you might want to lock to a specific mode:

```xaml
<inputs:SfColorPicker IsInline="True"
                      ColorMode="Palette"
                      IsColorModeSwitcherVisible="False"/>
```

### Compact Inline Configuration

Minimize space usage while keeping inline:

```xaml
<inputs:SfColorPicker IsInline="True"
                      ColorMode="Palette"
                      IsColorModeSwitcherVisible="False"
                      ShowRecentColors="False"
                      IsActionButtonsVisible="False"
                      ShowInputArea="False"
                      PaletteRowCount="5"
                      PaletteColumnCount="6"/>
```

## Responsive Design with Inline

### Adaptive Layout: Inline on Tablet, Popup on Phone

```xaml
<ContentPage>
    <inputs:SfColorPicker x:Name="colorPicker">
        <inputs:SfColorPicker.IsInline>
            <OnIdiom x:TypeArguments="x:Boolean">
                <OnIdiom.Phone>False</OnIdiom.Phone>
                <OnIdiom.Tablet>True</OnIdiom.Tablet>
                <OnIdiom.Desktop>True</OnIdiom.Desktop>
            </OnIdiom>
        </inputs:SfColorPicker.IsInline>
    </inputs:SfColorPicker>
</ContentPage>
```

### Width-Based Adaptive

```csharp
public class AdaptiveColorPickerPage : ContentPage
{
    private SfColorPicker colorPicker;
    
    public AdaptiveColorPickerPage()
    {
        colorPicker = new SfColorPicker();
        Content = colorPicker;
        
        SizeChanged += OnPageSizeChanged;
    }
    
    private void OnPageSizeChanged(object sender, EventArgs e)
    {
        // Switch to inline mode if width > 600
        colorPicker.IsInline = Width > 600;
    }
}
```

## Scrollable Inline Picker

For smaller screens or compact layouts, wrap inline picker in ScrollView:

```xaml
<ScrollView>
    <VerticalStackLayout Padding="10">
        <Label Text="Color Customization"
               FontSize="20"
               Margin="0,0,0,15"/>
        
        <inputs:SfColorPicker IsInline="True"
                              ColorMode="Spectrum"
                              IsActionButtonsVisible="False"/>
    </VerticalStackLayout>
</ScrollView>
```

## Combining Inline with Popup Mode

Toggle between modes programmatically:

```xaml
<VerticalStackLayout>
    <Button Text="Toggle Mode" Clicked="OnToggleModeClicked"/>
    <inputs:SfColorPicker x:Name="colorPicker" IsInline="False"/>
</VerticalStackLayout>
```

```csharp
private void OnToggleModeClicked(object sender, EventArgs e)
{
    colorPicker.IsInline = !colorPicker.IsInline;
    
    var button = sender as Button;
    button.Text = colorPicker.IsInline ? "Switch to Popup" : "Switch to Inline";
}
```

## Best Practices

1. **Action Buttons:** Usually unnecessary in inline mode; hide them for better UX

2. **Space Management:** Reserve sufficient space (300-400px width minimum for spectrum mode)

3. **Responsive Design:** Use popup mode on mobile, inline on tablets/desktop

4. **Layout:** Wrap in ScrollView if inline picker might exceed available space

5. **Mode Selection:** Palette mode typically more compact than Spectrum for inline use

6. **Instant Feedback:** Without action buttons, provide immediate visual feedback on color changes

7. **Context:** Use inline mode in settings panels, customization screens, or design tools

## Performance Considerations

Inline mode keeps the full color picker interface in memory and rendered at all times. For pages with multiple inline pickers:

- Use lazy loading or conditional rendering
- Consider popup mode if color selection is infrequent
- Test performance on lower-end devices

## Comparison: Inline vs Popup

| Aspect | Inline Mode | Popup Mode |
|--------|-------------|------------|
| **Space** | Requires full picker space | Compact button |
| **Visibility** | Always visible | On-demand |
| **Best For** | Settings, design tools | Forms, compact UI |
| **Mobile** | Less suitable | Highly suitable |
| **Desktop** | Excellent | Good |
| **Workflow** | Direct manipulation | Click to open |
