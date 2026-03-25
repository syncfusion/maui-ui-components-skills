# Header in SfAIAssistView

The header appears at the top of the `SfAIAssistView` content area (below the toolbar). It is hidden by default and is the anchor point for common suggestions.

---

## Show / Hide Header

Use `ShowHeader` to toggle the header visibility. The default value is `false`.

```xml
<syncfusion:SfAIAssistView x:Name="sfAIAssistView"
                           AssistItems="{Binding AssistItems}"
                           ShowHeader="True" />
```

```csharp
sfAIAssistView.ShowHeader = true;
```

> **Important:** The `Suggestions` property (common/header suggestions) only renders when `ShowHeader` is `true`. Always set `ShowHeader="True"` when using `Suggestions`.

---

## Header Text

Use `HeaderText` to display a simple string title inside the default header layout.

```xml
<syncfusion:SfAIAssistView x:Name="sfAIAssistView"
                           ShowHeader="True"
                           HeaderText="Ask AI Anything" />
```

```csharp
sfAIAssistView.ShowHeader = true;
sfAIAssistView.HeaderText = "Ask AI Anything";
```

---

## Header Template

Use `HeaderTemplate` to replace the default header with a fully custom layout. Any `DataTemplate` is accepted — you can include images, labels, suggestion chips, or any MAUI view.

### XAML

```xml
<ContentPage.Resources>
    <DataTemplate x:Key="customHeaderTemplate">
        <Grid RowDefinitions="45,30,Auto"
              RowSpacing="10"
              Padding="0,18,0,0">

            <!-- Hero image -->
            <Image Source="aiassistview.png"
                   HorizontalOptions="Center" />

            <!-- Title label -->
            <Label Grid.Row="1"
                   Text="Ask AI Anything!"
                   HorizontalOptions="Center"
                   FontSize="16"
                   FontAttributes="Bold" />

            <!-- Dynamic info chips bound to ViewModel collection -->
            <FlexLayout Grid.Row="2"
                        BindableLayout.ItemsSource="{Binding HeaderInfoCollection}"
                        Wrap="Wrap"
                        JustifyContent="Center">
                <BindableLayout.ItemTemplate>
                    <DataTemplate>
                        <Border Margin="4" Padding="8,4" StrokeShape="RoundRectangle 12">
                            <Label Text="{Binding Text}" FontSize="12" />
                        </Border>
                    </DataTemplate>
                </BindableLayout.ItemTemplate>
            </FlexLayout>
        </Grid>
    </DataTemplate>
</ContentPage.Resources>

<syncfusion:SfAIAssistView x:Name="sfAIAssistView"
                           AssistItems="{Binding AssistItems}"
                           ShowHeader="True"
                           HeaderTemplate="{StaticResource customHeaderTemplate}" />
```

### C#

```csharp
sfAIAssistView.ShowHeader = true;
sfAIAssistView.HeaderTemplate = CreateHeaderTemplate();

private DataTemplate CreateHeaderTemplate()
{
    return new DataTemplate(() =>
    {
        var grid = new Grid
        {
            RowDefinitions =
            {
                new RowDefinition { Height = 45 },
                new RowDefinition { Height = GridLength.Auto }
            },
            RowSpacing = 10,
            Padding = new Thickness(0, 18, 0, 0)
        };

        var image = new Image
        {
            Source = "aiassistview.png",
            HorizontalOptions = LayoutOptions.Center
        };

        var title = new Label
        {
            Text = "Ask AI Anything!",
            HorizontalOptions = LayoutOptions.Center,
            FontSize = 16,
            FontAttributes = FontAttributes.Bold
        };
        Grid.SetRow(title, 1);

        grid.Children.Add(image);
        grid.Children.Add(title);
        return grid;
    });
}
```

---

## Header with Suggestions

The most common use of the header is to pair it with `Suggestions` — quick-start chips shown below the header content.

```xml
<syncfusion:SfAIAssistView
    AssistItems="{Binding AssistItems}"
    Suggestions="{Binding Suggestions}"
    ShowHeader="True"
    HeaderText="What can I help you with?" />
```

When a user taps a suggestion chip, it is sent as a request automatically (unless `CancelRequest` is set in `SuggestionItemSelected`). See `references/suggestions.md` for full suggestions configuration.
