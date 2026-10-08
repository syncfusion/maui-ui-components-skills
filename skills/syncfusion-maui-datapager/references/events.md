# Events

## Table of Contents
- [PageChanging](#pagechanging)
- [PageChanged](#pagechanged)
- [Choosing Between the Two Events](#choosing-between-the-two-events)

## PageChanging

The `PageChanging` event is triggered when the user navigation from one page to another page **begins** — before the page actually changes. Use this event to perform operations or validation before the navigation completes.

**`PageChangingEventArgs` members:**
- `OldPageIndex` — Gets the current page index from which the page is navigating.
- `NewPageIndex` — Gets the new page index to which the page is navigating.

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
                           PageChanging="DataPager_PageChanging"
                           Source="{Binding Orders}">
        </pager:SfDataPager>
    </Border>
</Grid>
```

**C# (wiring):**

```csharp
SfDataPager dataPager = new SfDataPager();
OrderInfoViewModel viewModel = new OrderInfoViewModel();
dataPager.PageSize = 15;
dataPager.PageChanging += DataPager_PageChanging;
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

**C# (handler):**

```csharp
private void DataPager_PageChanging(object sender, Syncfusion.Maui.DataPager.PageChangingEventArgs e)
{
    int oldPageIndex = e.OldPageIndex;
    int newPageIndex = e.NewPageIndex;
    // Perform any operations before the page changes
}
```

## PageChanged

The `PageChanged` event is triggered when the user **has navigated** from one page to another — after the page change is complete. Use this event to react to the new page (e.g., update a header, log analytics, refresh dependent UI).

**`PageChangedEventArgs` members:**
- `OldPageIndex` — Gets the current page index from which the page is navigated.
- `NewPageIndex` — Gets the new page index to which the page is navigated.

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
                           PageChanged="DataPager_PageChanged"
                           Source="{Binding Orders}">
        </pager:SfDataPager>
    </Border>
</Grid>
```

**C# (wiring):**

```csharp
SfDataPager dataPager = new SfDataPager();
OrderInfoViewModel viewModel = new OrderInfoViewModel();
dataPager.PageSize = 15;
dataPager.PageChanged += DataPager_PageChanged;
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

**C# (handler):**

```csharp
private void DataPager_PageChanged(object sender, Syncfusion.Maui.DataPager.PageChangedEventArgs e)
{
    int oldPageIndex = e.OldPageIndex;
    int newPageIndex = e.NewPageIndex;
    // Perform any operations after the page has changed
}
```

## Choosing Between the Two Events

| Event | Fires When | Typical Use |
|---|---|---|
| `PageChanging` | Navigation begins (before the page changes) | Validation, cancel/prevent navigation, pre-load preparation, logging intent |
| `PageChanged` | Navigation completes (after the page changes) | Update dependent UI, analytics, refresh summaries, sync external state |

Both events expose `OldPageIndex` and `NewPageIndex`, so you can compute the direction of navigation (forward/backward) by comparing the two values.
