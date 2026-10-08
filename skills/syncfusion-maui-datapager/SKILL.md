---
name: syncfusion-maui-datapager
description: Implement and customize Syncfusion .NET MAUI DataPager (SfDataPager) for paginating large datasets into manageable pages. Use when working with MAUI paging, SfDataPager, normal or on-demand paging, numeric buttons, navigation buttons, page navigation, PageChanging or PageChanged events, or styling the pager.
metadata:
  author: "Syncfusion Inc"
  version: "34.1.29"
---

# Implementing .NET MAUI DataPager

A comprehensive skill for implementing and customizing the Syncfusion .NET MAUI DataPager (`SfDataPager`) control. The DataPager is a standalone pagination control that divides large datasets into manageable pages and integrates seamlessly with data display controls like the `SfDataGrid`.

## When to Use This Skill

Use this skill when you need to:
- Display large datasets in paginated views in a .NET MAUI application
- Add pagination to a `SfDataGrid` or any items control
- Choose between normal paging (load all upfront) and on-demand paging (load per page)
- Configure numeric and navigation buttons (First, Last, Previous, Next)
- Control button shape, size, font size, or orientation
- Style the pager with custom colors or button templates
- Handle page navigation events (PageChanging, PageChanged)
- Navigate pages programmatically (MoveToFirstPage, MoveToPage, etc.)
- Enable auto-ellipsis for large page ranges
- Optimize memory for very large datasets with ResetCache

## Component Overview

**SfDataPager** is a standalone pagination control for .NET MAUI that provides:

**Core Features:**
- Normal and On-Demand paging modes
- First, Last, Previous, Next, and numeric button navigation
- Programmatic page switching (MoveToFirstPage, MoveToLastPage, MoveToNextPage, MoveToPreviousPage, MoveToPage)
- Customizable display modes (choose which buttons to show)
- Button shape, size, and font size customization
- Horizontal or vertical orientation
- Auto-ellipsis support for large page ranges
- Comprehensive style and template customization via `DataPagerStyle`
- PageChanging and PageChanged events
- Memory optimization with ResetCache for on-demand paging

**Data Flow:**
- Bind data collection to `SfDataPager.Source`
- `SfDataPager.PagedSource` exposes the current page's data
- Bind `PagedSource` to a display control's `ItemsSource` (e.g., `SfDataGrid`)

**NuGet Package:** `Syncfusion.Maui.DataPager`

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Prerequisites (.NET 9 SDK, Visual Studio / VS Code / Rider)
- Install the Syncfusion.Maui.DataPager NuGet package
- Register Syncfusion core handler in MauiProgram.cs
- Import the DataPager namespace (XAML and C#)
- Create data model (OrderInfo) and repository (OrderInfoRepository)
- Add SfDataPager with SfDataGrid (XAML and C#)
- Source-to-PagedSource binding pattern

### Paging Modes & Navigation
📄 **Read:** [references/paging-modes.md](references/paging-modes.md)
- Normal paging (load entire collection upfront)
- On-demand paging (UseOnDemandPaging, OnDemandLoading event)
- LoadDynamicItems(StartIndex, PageSize)
- PageCount for on-demand paging (instead of Source)
- ResetCache() for memory optimization
- Programmatic navigation (MoveToFirstPage, MoveToLastPage, MoveToNextPage, MoveToPreviousPage, MoveToPage)
- Animated page navigation (MoveToPage with duration)
- Boundary behavior (no exceptions at first/last page)

### Customization
📄 **Read:** [references/customization.md](references/customization.md)
- ButtonShape (Rectangle, Circle)
- NumericButtonsGenerateMode (Auto, explicit via NumericButtonCount)
- ButtonSize and ButtonFontSize
- DisplayMode (None, First, Last, Previous, Next, Numeric, and combinations)
- AutoEllipsisMode (None, Before, After, Both)
- AutoEllipsisText customization
- Orientation (Horizontal, Vertical)

### Appearance & Styling
📄 **Read:** [references/appearance.md](references/appearance.md)
- DataPagerStyle overview and DefaultStyle assignment
- Background colors (DataPager, navigation buttons, numeric buttons)
- Selection colors (numeric button selection background and text)
- Disabled navigation button colors
- Navigation button templates (FirstPageButtonTemplate, LastPageButtonTemplate, NextPageButtonTemplate, PreviousPageButtonTemplate)

### Events
📄 **Read:** [references/events.md](references/events.md)
- PageChanging event (PageChangingEventArgs: OldPageIndex, NewPageIndex)
- PageChanged event (PageChangedEventArgs: OldPageIndex, NewPageIndex)
- When to use each event (before vs after navigation)
- Event handler signatures and wiring (XAML and C#)

## Quick Start

A minimal DataPager bound to a collection and paired with a `SfDataGrid`:

```csharp
using Syncfusion.Maui.DataPager;
using Syncfusion.Maui.DataGrid;
using System.Collections.ObjectModel;

public class OrderInfo
{
    public string? OrderID { get; set; }
    public string? CustomerID { get; set; }
    public string? ShipCountry { get; set; }

    public OrderInfo(string orderId, string customerId, string country)
    {
        OrderID = orderId;
        CustomerID = customerId;
        ShipCountry = country;
    }
}

var orders = new ObservableCollection<OrderInfo>
{
    new OrderInfo("1001", "Maria Anders", "Germany"),
    new OrderInfo("1002", "Ana Trujillo", "Mexico"),
    new OrderInfo("1003", "Ant Fuller", "Mexico"),
    // ... more items
};

SfDataPager dataPager = new SfDataPager();
dataPager.PageSize = 15;
dataPager.NumericButtonCount = 10;
dataPager.Source = orders;

SfDataGrid dataGrid = new SfDataGrid();
dataGrid.ItemsSource = dataPager.PagedSource;

this.Content = dataGrid;
```

**Namespace (XAML):**

```xaml
xmlns:pager="clr-namespace:Syncfusion.Maui.DataPager;assembly=Syncfusion.Maui.DataPager"
```

**Don't forget** to register the Syncfusion core handler in `MauiProgram.cs`:

```csharp
builder.ConfigureSyncfusionCore();
```

Forgetting `ConfigureSyncfusionCore()` causes runtime errors when rendering the DataPager.

## Common Patterns

### Bind a DataGrid to paged data

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="*" />
        <RowDefinition Height="Auto" />
    </Grid.RowDefinitions>
    <syncfusion:SfDataGrid x:Name="dataGrid"
                           Grid.Row="0"
                           ItemsSource="{Binding Source={x:Reference dataPager}, Path=PagedSource}" />
    <Border Grid.Row="1" Padding="5">
        <pager:SfDataPager x:Name="dataPager"
                           PageSize="15"
                           NumericButtonCount="10"
                           Source="{Binding Orders}" />
    </Border>
</Grid>
```

### Load pages on demand

```csharp
dataPager.UseOnDemandPaging = true;
dataPager.PageCount = 67; // total pages, e.g. 1000 items / 15 per page
dataPager.OnDemandLoading += (sender, e) =>
{
    dataPager.LoadDynamicItems(e.StartIndex, orders.Skip(e.StartIndex).Take(e.PageSize));
};
```

### Navigate programmatically

```csharp
dataPager.MoveToFirstPage();
dataPager.MoveToPreviousPage();
dataPager.MoveToNextPage();
dataPager.MoveToLastPage();
dataPager.MoveToPage(5);
// Animated navigation: MoveToPage(pageIndex, durationMs, animate)
dataPager.MoveToPage(5, 300, true);
```

### Style the pager

```csharp
DataPagerStyle style = new DataPagerStyle();
style.NumericButtonSelectionBackgroundColor = Color.FromArgb("#CDB4DB");
style.NumericButtonBackgroundColor = Color.FromArgb("#FFC8DD");
style.NavigationButtonBackgroundColor = Color.FromArgb("#90E0EF");
style.NavigationButtonIconColor = Color.FromArgb("#0077B6");
dataPager.DefaultStyle = style;
```

### Choosing the right reference

| User wants to... | Read |
|---|---|
| Set up the DataPager for the first time | `references/getting-started.md` |
| Choose normal vs on-demand paging or navigate pages | `references/paging-modes.md` |
| Change button shape, size, display mode, ellipsis, or orientation | `references/customization.md` |
| Apply colors, styles, or button templates | `references/appearance.md` |
| Handle page change events | `references/events.md` |
