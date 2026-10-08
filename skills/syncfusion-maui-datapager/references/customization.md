# Customization

## Table of Contents
- [Numeric Button Shapes](#numeric-button-shapes)
- [Generating Numeric Buttons](#generating-numeric-buttons)
- [Customizing Button Size and Font Size](#customizing-button-size-and-font-size)
- [Display Mode](#display-mode)
- [Auto-Ellipsis Mode](#auto-ellipsis-mode)
- [Customize the Auto-Ellipsis Text](#customize-the-auto-ellipsis-text)
- [Orientation](#orientation)

## Numeric Button Shapes

The `SfDataPager.ButtonShape` property changes the shape of the numeric buttons. The default shape is a circle; set it to `Rectangle` for squared buttons.

**XAML:**

```xaml
<ContentPage.BindingContext>
    <local:OrderInfoViewModel x:Name="viewModel"/>
</ContentPage.BindingContext>

<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="*" />
        <RowDefinition Height="Auto" />
    </Grid.RowDefinitions>
    <Border Grid.Row="1" Padding="5">
        <pager:SfDataPager x:Name="dataPager"
                           PageSize="15"
                           ButtonShape="Rectangle"
                           Source="{Binding Orders}">
        </pager:SfDataPager>
    </Border>
</Grid>
```

**C#:**

```csharp
SfDataPager dataPager = new SfDataPager();
OrderInfoViewModel viewModel = new OrderInfoViewModel();
dataPager.PageSize = 15;
dataPager.ButtonShape = DataPagerButtonShape.Rectangle;
dataPager.Source = viewModel.Orders;

Border border = new Border();
border.Padding = new Thickness(5);
border.Content = dataPager;

Grid grid = new Grid();
grid.RowDefinitions.Add(new RowDefinition() { Height = GridLength.Star });
grid.RowDefinitions.Add(new RowDefinition() { Height = GridLength.Auto });
grid.Children.Add(border);
grid.SetRow(border, 1);
this.Content = grid;
```

**Available shapes:** `DataPagerButtonShape.Rectangle`, `DataPagerButtonShape.Circle`

## Generating Numeric Buttons

The `SfDataPager.NumericButtonsGenerateMode` property controls how numeric buttons are generated:

- **Auto:** The pager automatically determines how many numeric buttons fit in the available view size.
- **Explicit (via `NumericButtonCount`):** You specify the exact number of numeric buttons to display.

**XAML:**

```xaml
<ContentPage.BindingContext>
    <local:OrderInfoViewModel x:Name="viewModel"/>
</ContentPage.BindingContext>

<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="*" />
        <RowDefinition Height="Auto" />
    </Grid.RowDefinitions>
    <Border Grid.Row="1" Padding="5">
        <pager:SfDataPager x:Name="dataPager"
                           PageSize="15"
                           NumericButtonsGenerateMode="Auto"
                           Source="{Binding Orders}">
        </pager:SfDataPager>
    </Border>
</Grid>
```

**C#:**

```csharp
SfDataPager dataPager = new SfDataPager();
OrderInfoViewModel viewModel = new OrderInfoViewModel();
dataPager.PageSize = 15;
dataPager.NumericButtonsGenerateMode = DataPagerNumericButtonsGenerateMode.Auto;
dataPager.Source = viewModel.Orders;

Border border = new Border();
border.Padding = new Thickness(5);
border.Content = dataPager;

Grid grid = new Grid();
grid.RowDefinitions.Add(new RowDefinition() { Height = GridLength.Star });
grid.RowDefinitions.Add(new RowDefinition() { Height = GridLength.Auto });
grid.Children.Add(border);
grid.SetRow(border, 1);
this.Content = grid;
```

> **Note:** The size of the `SfDataPager` is automatically adjusted based on the available screen size if the view cannot accommodate the numeric buttons specified in the `NumericButtonCount` property.

## Customizing Button Size and Font Size

The `SfDataPager` button loads with a default width and height of 40. The default button font size is 14. Customize these with the `ButtonSize` and `ButtonFontSize` properties.

**XAML:**

```xaml
<ContentPage.BindingContext>
    <local:OrderInfoViewModel x:Name="viewModel"/>
</ContentPage.BindingContext>

<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="*" />
        <RowDefinition Height="Auto" />
    </Grid.RowDefinitions>
    <Border Grid.Row="1" Padding="5">
        <pager:SfDataPager x:Name="dataPager"
                           PageSize="15"
                           ButtonSize="60"
                           ButtonFontSize="21"
                           Source="{Binding Orders}">
        </pager:SfDataPager>
    </Border>
</Grid>
```

**C#:**

```csharp
SfDataPager dataPager = new SfDataPager();
OrderInfoViewModel viewModel = new OrderInfoViewModel();
dataPager.PageSize = 15;
dataPager.ButtonSize = 60;
dataPager.ButtonFontSize = 21;
dataPager.Source = viewModel.Orders;

Border border = new Border();
border.Padding = new Thickness(5);
border.Content = dataPager;

Grid grid = new Grid();
grid.RowDefinitions.Add(new RowDefinition() { Height = GridLength.Auto });
grid.Children.Add(border);
grid.SetRow(border, 1);
this.Content = grid;
```

**Defaults:**
- `ButtonSize`: 40 (width and height)
- `ButtonFontSize`: 14

## Display Mode

The `SfDataPager.DisplayMode` property controls which navigation and numeric buttons are visible. The default value is `FirstLastPreviousNextNumeric`, which displays all buttons.

**Available values:**

| Property Value | Description |
|---|---|
| `None` | Displays no page buttons |
| `First` | Displays only the first page button |
| `Last` | Displays only the last page button |
| `Previous` | Displays only the previous page button |
| `Next` | Displays only the next page button |
| `Numeric` | Displays only the numeric page buttons |
| `FirstLast` | Displays the first and last page buttons |
| `PreviousNext` | Displays the previous and next page buttons |
| `FirstLastNumeric` | Displays the first, last, and numeric page buttons |
| `PreviousNextNumeric` | Displays the previous, next, and numeric page buttons |
| `FirstLastPreviousNext` | Displays the first, last, previous, and next page buttons |
| `FirstLastPreviousNextNumeric` | Displays all buttons (default) |

**XAML:**

```xaml
<ContentPage.BindingContext>
    <local:OrderInfoViewModel x:Name="viewModel"/>
</ContentPage.BindingContext>

<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="*" />
        <RowDefinition Height="Auto" />
    </Grid.RowDefinitions>
    <Border Grid.Row="1" Padding="5">
        <pager:SfDataPager x:Name="dataPager"
                           PageSize="15"
                           DisplayMode="FirstLastNumeric"
                           Source="{Binding Orders}">
        </pager:SfDataPager>
    </Border>
</Grid>
```

**C#:**

```csharp
SfDataPager dataPager = new SfDataPager();
OrderInfoViewModel viewModel = new OrderInfoViewModel();
dataPager.PageSize = 15;
dataPager.DisplayMode = DataPagerDisplayMode.FirstLastNumeric;
dataPager.Source = viewModel.Orders;

Border border = new Border();
border.Padding = new Thickness(5);
border.Content = dataPager;

Grid grid = new Grid();
grid.RowDefinitions.Add(new RowDefinition() { Height = GridLength.Auto });
grid.Children.Add(border);
grid.SetRow(border, 1);
this.Content = grid;
```

## Auto-Ellipsis Mode

The `AutoEllipsisMode` property controls whether ellipsis buttons appear for navigating large ranges of page numbers. This is independent of `DisplayMode`, which controls which button *types* (First, Last, Previous, Next, Numeric) are visible.

The `SfDataPager` displays an ellipsis button at the beginning and/or end of the numeric buttons when the scroll view contains additional numeric buttons before or after the currently selected numeric button.

**XAML:**

```xaml
<ContentPage.BindingContext>
    <local:OrderInfoViewModel x:Name="viewModel"/>
</ContentPage.BindingContext>

<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="*" />
        <RowDefinition Height="Auto" />
    </Grid.RowDefinitions>
    <Border Grid.Row="1" Padding="5">
        <pager:SfDataPager x:Name="dataPager"
                           PageSize="15"
                           AutoEllipsisMode="After"
                           Source="{Binding Orders}">
        </pager:SfDataPager>
    </Border>
</Grid>
```

**C#:**

```csharp
SfDataPager dataPager = new SfDataPager();
OrderInfoViewModel viewModel = new OrderInfoViewModel();
dataPager.PageSize = 15;
dataPager.AutoEllipsisMode = DataPagerEllipsisMode.After;
dataPager.Source = viewModel.Orders;

Border border = new Border();
border.Padding = new Thickness(5);
border.Content = dataPager;

Grid grid = new Grid();
grid.RowDefinitions.Add(new RowDefinition() { Height = GridLength.Star });
grid.RowDefinitions.Add(new RowDefinition() { Height = GridLength.Auto });
grid.Children.Add(border);
grid.SetRow(border, 1);
this.Content = grid;
```

**Available values:** `DataPagerEllipsisMode.None`, `DataPagerEllipsisMode.Before`, `DataPagerEllipsisMode.After`, `DataPagerEllipsisMode.Both`

## Customize the Auto-Ellipsis Text

The auto-ellipsis text can be customized using the `SfDataPager.AutoEllipsisText` property. The default value is `…`.

**XAML:**

```xaml
<ContentPage.BindingContext>
    <local:OrderInfoViewModel x:Name="viewModel"/>
</ContentPage.BindingContext>

<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="*" />
        <RowDefinition Height="Auto" />
    </Grid.RowDefinitions>
    <Border Grid.Row="1" Padding="5">
        <pager:SfDataPager x:Name="dataPager"
                           PageSize="15"
                           AutoEllipsisMode="After"
                           AutoEllipsisText="***"
                           Source="{Binding Orders}">
        </pager:SfDataPager>
    </Border>
</Grid>
```

**C#:**

```csharp
SfDataPager dataPager = new SfDataPager();
OrderInfoViewModel viewModel = new OrderInfoViewModel();
dataPager.PageSize = 15;
dataPager.AutoEllipsisMode = DataPagerEllipsisMode.After;
dataPager.AutoEllipsisText = "***";
dataPager.Source = viewModel.Orders;

Border border = new Border();
border.Padding = new Thickness(5);
border.Content = dataPager;

Grid grid = new Grid();
grid.RowDefinitions.Add(new RowDefinition() { Height = GridLength.Star });
grid.RowDefinitions.Add(new RowDefinition() { Height = GridLength.Auto });
grid.Children.Add(border);
grid.SetRow(border, 1);
this.Content = grid;
```

## Orientation

By default, `SfDataPager` displays buttons horizontally. The `SfDataPager.Orientation` property lets you display buttons vertically — useful for side-pane layouts.

**XAML:**

```xaml
<ContentPage.BindingContext>
    <local:OrderInfoViewModel x:Name="viewModel"/>
</ContentPage.BindingContext>

<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="*" />
        <ColumnDefinition Width="Auto" />
    </Grid.ColumnDefinitions>
    <Border Grid.Column="1" Padding="5">
        <pager:SfDataPager x:Name="dataPager"
                           PageSize="15"
                           Orientation="Vertical"
                           Source="{Binding Orders}">
        </pager:SfDataPager>
    </Border>
    <syncfusion:SfDataGrid x:Name="dataGrid"
                           Grid.Column="0"
                           ItemsSource="{Binding Source={x:Reference dataPager}, Path=PagedSource}">
    </syncfusion:SfDataGrid>
</Grid>
```

**C#:**

```csharp
SfDataPager dataPager = new SfDataPager();
OrderInfoViewModel viewModel = new OrderInfoViewModel();
dataPager.PageSize = 15;
dataPager.Orientation = DataPagerScrollOrientation.Vertical;
dataPager.Source = viewModel.Orders;

SfDataGrid dataGrid = new SfDataGrid();
dataGrid.ItemsSource = dataPager.PagedSource;

Border border = new Border();
border.Padding = new Thickness(5);
border.Content = dataPager;

Grid grid = new Grid();
grid.ColumnDefinitions.Add(new ColumnDefinition() { Width = GridLength.Star });
grid.ColumnDefinitions.Add(new ColumnDefinition() { Width = GridLength.Auto });
grid.Children.Add(dataGrid);
grid.Children.Add(border);
grid.SetColumn(dataGrid, 0);
grid.SetColumn(border, 1);
this.Content = grid;
```

**Available values:** `DataPagerScrollOrientation.Horizontal` (default), `DataPagerScrollOrientation.Vertical`
