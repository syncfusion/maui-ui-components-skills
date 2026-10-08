# Appearance and Styling

## Table of Contents
- [Customizing Appearance with DataPagerStyle](#customizing-appearance-with-datapagerstyle)
- [Color Properties](#color-properties)
- [Button Templates](#button-templates)
- [Applying a Custom Style](#applying-a-custom-style)

## Customizing Appearance with DataPagerStyle

The DataPager allows you to change its appearance by modifying the properties of `DataPagerStyle` and then assigning it to the `SfDataPager.DefaultStyle` property.

This is the primary styling mechanism — there is no separate style key. Create a `DataPagerStyle` instance, set its properties, and assign it to `DefaultStyle`.

## Color Properties

The following `DataPagerStyle` properties control colors across the pager:

| Property | Description |
|---|---|
| `DataPagerBackgroundColor` | Background color of the `SfDataPager` |
| `NavigationButtonBackgroundColor` | Background color of the navigation buttons |
| `NavigationButtonDisableBackgroundColor` | Background color of navigation buttons when disabled |
| `NavigationButtonDisableIconColor` | Icon color of navigation buttons when disabled |
| `NavigationButtonIconColor` | Icon color of the navigation buttons |
| `NumericButtonBackgroundColor` | Background color of the numeric buttons |
| `NumericButtonSelectionBackgroundColor` | Background color of the currently selected numeric button |
| `NumericButtonSelectionTextColor` | Text color of the currently selected numeric button |
| `NumericButtonTextColor` | Text color of the numeric buttons |

## Button Templates

The following `DataPagerStyle` properties accept `DataTemplate` values to fully replace the appearance of each navigation button:

| Property | Description |
|---|---|
| `FirstPageButtonTemplate` | Template for the first page navigation button |
| `LastPageButtonTemplate` | Template for the last page navigation button |
| `NextPageButtonTemplate` | Template for the next page navigation button |
| `PreviousPageButtonTemplate` | Template for the previous page navigation button |

Use templates when you need custom icons, vector graphics, or complex button content beyond what the color properties provide.

## Applying a Custom Style

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
    <Border Grid.Row="1"
            Padding="5">
        <pager:SfDataPager x:Name="dataPager"
                           PageSize="15"
                           Source="{Binding Orders}">
            <pager:SfDataPager.DefaultStyle>
                <pager:DataPagerStyle NumericButtonSelectionBackgroundColor="#cdb4db"
                                      NumericButtonBackgroundColor="#ffc8dd"
                                      NavigationButtonBackgroundColor="#90e0ef"
                                      NavigationButtonIconColor="#0077b6"
                                      NavigationButtonDisableBackgroundColor="#caf0f8"
                                      NavigationButtonDisableIconColor="#9a8c98">
                </pager:DataPagerStyle>
            </pager:SfDataPager.DefaultStyle>
        </pager:SfDataPager>
    </Border>
</Grid>
```

**C#:**

```csharp
SfDataPager dataPager = new SfDataPager();
OrderInfoViewModel viewModel = new OrderInfoViewModel();
dataPager.PageSize = 15;
dataPager.Source = viewModel.Orders;

DataPagerStyle dataPagerStyle = new DataPagerStyle();
dataPagerStyle.NumericButtonSelectionBackgroundColor = Color.FromArgb("#CDB4DB");
dataPagerStyle.NumericButtonBackgroundColor = Color.FromArgb("#FFC8DD");
dataPagerStyle.NavigationButtonBackgroundColor = Color.FromArgb("#90E0EF");
dataPagerStyle.NavigationButtonIconColor = Color.FromArgb("#0077B6");
dataPagerStyle.NavigationButtonDisableBackgroundColor = Color.FromArgb("#CAF0F8");
dataPagerStyle.NavigationButtonDisableIconColor = Color.FromArgb("#9A8C98");
dataPager.DefaultStyle = dataPagerStyle;

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

**Notes:**
- In XAML, hex color values are case-insensitive (e.g., `#cdb4db` and `#CDB4DB` are equivalent).
- In C#, use `Color.FromArgb("#RRGGBB")` or `Color.FromArgb("#AARRGGBB")` for alpha-channel support.
- Assign the completed `DataPagerStyle` to `SfDataPager.DefaultStyle`. Setting individual color properties on the pager directly is not supported — they live on the style object.
