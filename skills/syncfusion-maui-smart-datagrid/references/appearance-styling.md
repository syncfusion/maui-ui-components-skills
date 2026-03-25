# Appearance and Styling

## Table of Contents
- [Toolbar Styling](#toolbar-styling)
- [Toolbar Template](#toolbar-template)
- [AssistButton Styling](#assistbutton-styling)
- [AssistButton Templates](#assistbutton-templates)
- [AssistView Styling](#assistview-styling)
- [AssistView Header Template](#assistview-header-template)
- [AssistView Banner and Editor Templates](#assistview-banner-and-editor-templates)

## Toolbar Styling

The toolbar appears at the top of the Smart DataGrid and contains the AI Assistant button. Style it using `SmartAssistStyle` properties.

### Available Properties

- **ToolbarBackground** - Background color
- **ToolbarStroke** - Border color
- **ToolbarStrokeThickness** - Border width

### XAML Configuration

```xaml
<syncfusion:SfSmartDataGrid ItemsSource="{Binding OrderInfoCollection}">
    <syncfusion:SfSmartDataGrid.AssistViewSettings>
        <syncfusion:DataGridAssistViewSettings>
            <syncfusion:DataGridAssistViewSettings.AssistStyle>
                <syncfusion:SmartAssistStyle
                    ToolbarBackground="#F7F2FB"
                    ToolbarStroke="#CAC4D0"
                    ToolbarStrokeThickness="1" />
            </syncfusion:DataGridAssistViewSettings.AssistStyle>
        </syncfusion:DataGridAssistViewSettings>
    </syncfusion:SfSmartDataGrid.AssistViewSettings>
</syncfusion:SfSmartDataGrid>
```

### Code-Behind Configuration

```csharp
var style = dataGrid.AssistViewSettings.AssistStyle;
style.ToolbarBackground = Color.FromArgb("#F7F2FB");
style.ToolbarStroke = Color.FromArgb("#CAC4D0");
style.ToolbarStrokeThickness = 1f;
```

### Design Examples

**Modern Light Theme:**
```csharp
style.ToolbarBackground = Colors.White;
style.ToolbarStroke = Color.FromArgb("#E0E0E0");
style.ToolbarStrokeThickness = 1f;
```

**Dark Theme:**
```csharp
style.ToolbarBackground = Color.FromArgb("#2C2C2C");
style.ToolbarStroke = Color.FromArgb("#444444");
style.ToolbarStrokeThickness = 1f;
```

**Accent Theme:**
```csharp
style.ToolbarBackground = Color.FromArgb("#F3E5F5");
style.ToolbarStroke = Color.FromArgb("#9C27B0");
style.ToolbarStrokeThickness = 2f;
```

## Toolbar Template

Replace the entire default toolbar with a custom layout using `ToolbarTemplate`.

### XAML Configuration

```xaml
<syncfusion:SfSmartDataGrid ItemsSource="{Binding OrderInfoCollection}">
    <syncfusion:SfSmartDataGrid.ToolbarTemplate>
        <DataTemplate>
            <Grid BackgroundColor="LightGray" Padding="8" ColumnDefinitions="*,Auto">
                <Label Text="Smart Analysis Tools"
                       VerticalTextAlignment="Center"
                       FontAttributes="Bold"
                       FontSize="14" />
                
                <Button Text="Ask AI"
                        Grid.Column="1"
                        BackgroundColor="#6750A4"
                        TextColor="White"
                        Clicked="OnAskAIClicked"
                        CornerRadius="5"
                        Padding="10,5" />
            </Grid>
        </DataTemplate>
    </syncfusion:SfSmartDataGrid.ToolbarTemplate>
</syncfusion:SfSmartDataGrid>
```

### Code-Behind Configuration

```csharp
dataGrid.ToolbarTemplate = new DataTemplate(() =>
{
    var grid = new Grid
    {
        BackgroundColor = Colors.LightGray,
        Padding = new Thickness(8),
        ColumnDefinitions =
        {
            new ColumnDefinition { Width = GridLength.Star },
            new ColumnDefinition { Width = GridLength.Auto }
        }
    };

    var label = new Label
    {
        Text = "Smart Analysis Tools",
        VerticalTextAlignment = TextAlignment.Center,
        FontAttributes = FontAttributes.Bold,
        FontSize = 14
    };

    var button = new Button
    {
        Text = "Ask AI",
        BackgroundColor = Color.FromArgb("#6750A4"),
        TextColor = Colors.White,
        CornerRadius = 5,
        Padding = new Thickness(10, 5)
    };

    button.Clicked += OnAskAIClicked;

    grid.Add(label, 0, 0);
    grid.Add(button, 1, 0);

    return grid;
});

private void OnAskAIClicked(object sender, EventArgs e)
{
    dataGrid.ShowAssistView();
}
```

### Advanced Template Example

```xaml
<syncfusion:SfSmartDataGrid.ToolbarTemplate>
    <DataTemplate>
        <Grid BackgroundColor="#F5F5F5" Padding="12" 
              ColumnDefinitions="*,Auto,Auto,Auto"
              RowDefinitions="Auto,Auto">
            
            <!-- Title -->
            <Label Text="Data Explorer"
                   FontSize="16"
                   FontAttributes="Bold"
                   Grid.ColumnSpan="4" />

            <!-- Quick action buttons -->
            <Button Text="📊 Sort" Grid.Column="0" Grid.Row="1" Clicked="OnSortClicked" />
            <Button Text="🔍 Filter" Grid.Column="1" Grid.Row="1" Clicked="OnFilterClicked" />
            <Button Text="📂 Group" Grid.Column="2" Grid.Row="1" Clicked="OnGroupClicked" />
            <Button Text="🤖 AI" Grid.Column="3" Grid.Row="1" Clicked="OnAIClicked" />
        </Grid>
    </DataTemplate>
</syncfusion:SfSmartDataGrid.ToolbarTemplate>
```

## AssistButton Styling

The AssistButton is the button users click to open the AI Assistant. Configure its appearance with `SmartAssistStyle` properties.

### Available Properties

- **AssistButtonBackground** - Button background color
- **AssistButtonIconColor** - Icon color
- **AssistButtonCornerRadius** - Border radius for rounded appearance

### XAML Configuration

```xaml
<syncfusion:SfSmartDataGrid ItemsSource="{Binding OrderInfoCollection}">
    <syncfusion:SfSmartDataGrid.AssistViewSettings>
        <syncfusion:DataGridAssistViewSettings>
            <syncfusion:DataGridAssistViewSettings.AssistStyle>
                <syncfusion:SmartAssistStyle
                    AssistButtonBackground="#6750A4"
                    AssistButtonIconColor="#FFFFFF"
                    AssistButtonCornerRadius="10" />
            </syncfusion:DataGridAssistViewSettings.AssistStyle>
        </syncfusion:DataGridAssistViewSettings>
    </syncfusion:SfSmartDataGrid.AssistViewSettings>
</syncfusion:SfSmartDataGrid>
```

### Code-Behind Configuration

```csharp
var style = dataGrid.AssistViewSettings.AssistStyle;
style.AssistButtonBackground = Color.FromArgb("#6750A4");
style.AssistButtonIconColor = Colors.White;
style.AssistButtonCornerRadius = 10;
```

### Design Examples

**Minimal Style:**
```csharp
style.AssistButtonBackground = Colors.White;
style.AssistButtonIconColor = Color.FromArgb("#666666");
style.AssistButtonCornerRadius = 5;
```

**Prominent Style:**
```csharp
style.AssistButtonBackground = Color.FromArgb("#FF6B35");
style.AssistButtonIconColor = Colors.White;
style.AssistButtonCornerRadius = 20;
```

### Hide Button Icon

Show only text, not the icon:

```xaml
<syncfusion:SfSmartDataGrid ShowAssistButtonIcon="False" />
```

```csharp
dataGrid.ShowAssistButtonIcon = false;
```

## AssistButton Templates

Replace the entire assist button or just its icon using templates.

### Custom AssistButton Template

Replace the default button completely:

```xaml
<syncfusion:SfSmartDataGrid ItemsSource="{Binding OrderInfoCollection}">
    <syncfusion:SfSmartDataGrid.AssistButtonTemplate>
        <DataTemplate>
            <Button Text="🤖 Ask AI"
                    BackgroundColor="#6750A4"
                    TextColor="White"
                    CornerRadius="20"
                    Padding="15,8"
                    FontSize="12"
                    FontAttributes="Bold" />
        </DataTemplate>
    </syncfusion:SfSmartDataGrid.AssistButtonTemplate>
</syncfusion:SfSmartDataGrid>
```

### Code-Behind Configuration

```csharp
dataGrid.AssistButtonTemplate = new DataTemplate(() =>
{
    return new Button
    {
        Text = "🤖 Ask AI",
        BackgroundColor = Color.FromArgb("#6750A4"),
        TextColor = Colors.White,
        CornerRadius = 20,
        Padding = new Thickness(15, 8),
        FontSize = 12,
        FontAttributes = FontAttributes.Bold
    };
});
```

### Custom AssistButton Icon Template

Replace just the icon:

```xaml
<syncfusion:SfSmartDataGrid ItemsSource="{Binding OrderInfoCollection}">
    <syncfusion:SfSmartDataGrid.AssistButtonIconTemplate>
        <DataTemplate>
            <Image Source="assistant_icon.png"
                   HeightRequest="20"
                   WidthRequest="20"
                   Aspect="AspectFit" />
        </DataTemplate>
    </syncfusion:SfSmartDataGrid.AssistButtonIconTemplate>
</syncfusion:SfSmartDataGrid>
```

```csharp
dataGrid.AssistButtonIconTemplate = new DataTemplate(() =>
{
    return new Image
    {
        Source = "assistant_icon.png",
        HeightRequest = 20,
        WidthRequest = 20,
        Aspect = Aspect.AspectFit
    };
});
```

### Advanced Button Example

```xaml
<syncfusion:SfSmartDataGrid.AssistButtonTemplate>
    <DataTemplate>
        <Frame BackgroundColor="#6750A4" 
               CornerRadius="25" 
               Padding="0" 
               BorderColor="White" 
               HasShadow="True">
            <Grid WidthRequest="50" HeightRequest="50" 
                  Padding="0" HorizontalOptions="Center" VerticalOptions="Center">
                <Label Text="🤖"
                       FontSize="28"
                       HorizontalOptions="Center"
                       VerticalOptions="Center" />
            </Grid>
        </Frame>
    </DataTemplate>
</syncfusion:SfSmartDataGrid.AssistButtonTemplate>
```

## AssistView Styling

Customize the AssistView popup appearance using `SmartAssistStyle` properties.

### Available Properties

- **AssistPopupStroke** - Popup border color
- **AssistPopupStrokeThickness** - Border width
- **AssistViewHeaderTextColor** - Header text color
- **AssistViewHeaderFontFamily** - Header font
- **AssistViewHeaderFontAttributes** - Bold, Italic, etc.
- **AssistViewHeaderFontSize** - Header font size
- **AssistViewHeaderBackground** - Header background color
- **HighlightColor** - Default highlight color for operations

### XAML Configuration

```xaml
<syncfusion:SfSmartDataGrid ItemsSource="{Binding OrderInfoCollection}">
    <syncfusion:SfSmartDataGrid.AssistViewSettings>
        <syncfusion:DataGridAssistViewSettings>
            <syncfusion:DataGridAssistViewSettings.AssistStyle>
                <syncfusion:SmartAssistStyle 
                    AssistPopupStroke="#CAC4D0"
                    AssistPopupStrokeThickness="1"
                    AssistViewHeaderTextColor="#6750A4"
                    AssistViewHeaderFontFamily="TimesNewRoman"
                    AssistViewHeaderFontAttributes="Bold"
                    AssistViewHeaderFontSize="16"
                    AssistViewHeaderBackground="#FFFBFE" 
                    HighlightColor="Red" />
            </syncfusion:DataGridAssistViewSettings.AssistStyle>
        </syncfusion:DataGridAssistViewSettings>
    </syncfusion:SfSmartDataGrid.AssistViewSettings>
</syncfusion:SfSmartDataGrid>
```

### Code-Behind Configuration

```csharp
var style = dataGrid.AssistViewSettings.AssistStyle;
style.AssistPopupStroke = Color.FromArgb("#CAC4D0");
style.AssistPopupStrokeThickness = 1;
style.AssistViewHeaderTextColor = Color.FromArgb("#6750A4");
style.AssistViewHeaderFontFamily = "TimesNewRoman";
style.AssistViewHeaderFontAttributes = FontAttributes.Bold;
style.AssistViewHeaderFontSize = 16d;
style.AssistViewHeaderBackground = Color.FromArgb("#FFFBFE");
style.HighlightColor = Colors.Red;
```

### Design Examples

**Professional Dark:**
```csharp
style.AssistViewHeaderBackground = Color.FromArgb("#2C2C2C");
style.AssistViewHeaderTextColor = Colors.White;
style.AssistPopupStroke = Color.FromArgb("#555555");
style.HighlightColor = Color.FromArgb("#FFD700");
```

**Colorful Accent:**
```csharp
style.AssistViewHeaderBackground = Color.FromArgb("#FF6B6B");
style.AssistViewHeaderTextColor = Colors.White;
style.AssistPopupStroke = Color.FromArgb("#FF6B6B");
style.HighlightColor = Color.FromArgb("#FFE66D");
```

### Custom Header Text

```xaml
<syncfusion:SfSmartDataGrid ItemsSource="{Binding OrderInfoCollection}">
    <syncfusion:SfSmartDataGrid.AssistViewSettings>
        <syncfusion:DataGridAssistViewSettings AssistViewHeaderText="Smart Assistant" />
    </syncfusion:SfSmartDataGrid.AssistViewSettings>
</syncfusion:SfSmartDataGrid>
```

```csharp
dataGrid.AssistViewSettings.AssistViewHeaderText = "Data Explorer AI";
```

### Control Header Elements

```xaml
<!-- Show close button -->
<syncfusion:DataGridAssistViewSettings ShowAssistViewCloseButton="True" />

<!-- Show banner area with suggestions -->
<syncfusion:DataGridAssistViewSettings ShowAssistViewBanner="True" />
```

```csharp
dataGrid.AssistViewSettings.ShowAssistViewCloseButton = false; // Hide close button
dataGrid.AssistViewSettings.ShowAssistViewBanner = true;       // Show banner
```

## AssistView Header Template

Completely customize the header section with a template.

### XAML Configuration

```xaml
<syncfusion:SfSmartDataGrid ItemsSource="{Binding OrderInfoCollection}">
    <syncfusion:SfSmartDataGrid.AssistViewSettings>
        <syncfusion:DataGridAssistViewSettings>
            <syncfusion:DataGridAssistViewSettings.AssistViewHeaderTemplate>
                <DataTemplate>
                    <Grid Padding="10" BackgroundColor="#F5F5F5" 
                          ColumnDefinitions="*,Auto">
                        
                        <!-- Title -->
                        <Label Text="My Smart Assistant"
                               FontAttributes="Bold"
                               FontSize="18"
                               VerticalOptions="Center"
                               HorizontalOptions="Start" />
                        
                        <!-- Close button -->
                        <Button Grid.Column="1"
                                Text="✕"
                                TranslationY="-4"
                                TextColor="Red"
                                FontSize="16"
                                HeightRequest="18"
                                WidthRequest="18"
                                BackgroundColor="Transparent"
                                Clicked="OnAssistViewCloseClicked" />
                    </Grid>
                </DataTemplate>
            </syncfusion:DataGridAssistViewSettings.AssistViewHeaderTemplate>
        </syncfusion:DataGridAssistViewSettings>
    </syncfusion:SfSmartDataGrid.AssistViewSettings>
</syncfusion:SfSmartDataGrid>
```

### Code-Behind Configuration

```csharp
dataGrid.AssistViewSettings.AssistViewHeaderTemplate = new DataTemplate(() =>
{
    var grid = new Grid
    {
        Padding = new Thickness(10),
        BackgroundColor = Color.FromArgb("#F5F5F5"),
        ColumnDefinitions =
        {
            new ColumnDefinition { Width = GridLength.Star },
            new ColumnDefinition { Width = GridLength.Auto }
        }
    };

    var title = new Label
    {
        Text = "Data Assistant",
        FontAttributes = FontAttributes.Bold,
        FontSize = 18,
        VerticalOptions = LayoutOptions.Center,
        HorizontalOptions = LayoutOptions.Start
    };

    var closeButton = new Button
    {
        Text = "✕",
        TextColor = Colors.Red,
        FontSize = 16,
        HeightRequest = 18,
        WidthRequest = 18,
        BackgroundColor = Colors.Transparent
    };

    closeButton.Clicked += (s, e) => dataGrid.CloseAssistView();

    grid.Add(title, 0, 0);
    grid.Add(closeButton, 1, 0);

    return grid;
});
```

## AssistView Banner and Editor Templates

### Banner Template

Customize the area that displays suggestions:

```xaml
<syncfusion:SfSmartDataGrid ItemsSource="{Binding OrderInfoCollection}">
    <syncfusion:SfSmartDataGrid.AssistViewSettings>
        <syncfusion:DataGridAssistViewSettings ShowAssistViewBanner="True">
            <syncfusion:DataGridAssistViewSettings.AssistViewBannerTemplate>
                <DataTemplate>
                    <StackLayout Padding="10" Spacing="5">
                        <Label Text="Suggested Actions:"
                               FontAttributes="Bold"
                               FontSize="12" />
                        <Label Text="💡 Try 'Sort by OrderID' or 'Filter by Germany'"
                               FontSize="11"
                               TextColor="#666666" />
                    </StackLayout>
                </DataTemplate>
            </syncfusion:DataGridAssistViewSettings.AssistViewBannerTemplate>
        </syncfusion:DataGridAssistViewSettings>
    </syncfusion:SfSmartDataGrid.AssistViewSettings>
</syncfusion:SfSmartDataGrid>
```

### Editor Template

Customize the input area:

```xaml
<syncfusion:SfSmartDataGrid ItemsSource="{Binding OrderInfoCollection}">
    <syncfusion:SfSmartDataGrid.AssistViewSettings>
        <syncfusion:DataGridAssistViewSettings>
            <syncfusion:DataGridAssistViewSettings.AssistViewEditorTemplate>
                <DataTemplate>
                    <Grid Padding="8" ColumnDefinitions="*,Auto">
                        
                        <Entry x:Name="entry"
                               Placeholder="Ask the grid anything..."
                               Completed="Entry_Completed"
                               FontSize="14" />
                        
                        <Button Text="🚀 Send"
                                Grid.Column="1"
                                BackgroundColor="#6750A4"
                                TextColor="White"
                                Clicked="Button_Clicked"
                                CornerRadius="5"
                                Padding="10,5" />
                    </Grid>
                </DataTemplate>
            </syncfusion:DataGridAssistViewSettings.AssistViewEditorTemplate>
        </syncfusion:DataGridAssistViewSettings>
    </syncfusion:SfSmartDataGrid.AssistViewSettings>
</syncfusion:SfSmartDataGrid>
```

### Code-Behind Handler

```csharp
private void Entry_Completed(object sender, EventArgs e)
{
    // Auto-send on Return key
    Button_Clicked(null, null);
}

private async void Button_Clicked(object sender, EventArgs e)
{
    var entry = sender as Entry ?? FindByName<Entry>("entry");
    if (!string.IsNullOrEmpty(entry?.Text))
    {
        await dataGrid.GetResponseAsync(entry.Text);
        entry.Text = string.Empty;
    }
}
```
