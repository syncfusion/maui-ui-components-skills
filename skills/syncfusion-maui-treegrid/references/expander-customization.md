# Expander Column Customization in TreeGrid

## Table of Contents
- [Overview](#overview)
- [Load Expander Icon Through Template](#load-expander-icon-through-template)
- [Load Expander Icon Through Template Selector](#load-expander-icon-through-template-selector)
- [Change the Expander Column](#change-the-expander-column)
- [Customize the Expander Column Width](#customize-the-expander-column-width)
- [Expand Nodes Using a Model Property](#expand-nodes-using-a-model-property)

## Overview

The .NET MAUI Tree Grid displays hierarchical data using an expander column that allows users to expand and collapse parent nodes. The `SfTreeGrid` provides customization options for the expander column, including customizing the expander icon, changing the expander column, modifying the expander column width, and controlling the initial expansion state through a data source property.

## Load Expander Icon Through Template

Customize the expand and collapse indicator using the `SfTreeGrid.ExpanderIcon` property. This property accepts a `DataTemplate` that lets you replace the default expander icon with custom content.

```xaml
<syncfusion:SfTreeGrid x:Name="treeGrid"
                       ItemsSource="{Binding PersonDetails}"
                       ChildPropertyName="Children">
    <syncfusion:SfTreeGrid.ExpanderIcon>
        <DataTemplate>
            <Image Source="expand_icon.png"
                   HeightRequest="15"
                   WidthRequest="15"/>
        </DataTemplate>
    </syncfusion:SfTreeGrid.ExpanderIcon>
</syncfusion:SfTreeGrid>
```

```csharp
SfTreeGrid treeGrid = new SfTreeGrid();
EmployeeViewModel employeeViewModel = new EmployeeViewModel();
treeGrid.ItemsSource = employeeViewModel.PersonDetails;
treeGrid.ChildPropertyName = "Children";
treeGrid.ExpanderIcon = new DataTemplate(() =>
{
    return new Image
    {
        Source = "expand_icon.png",
        HeightRequest = 15,
        WidthRequest = 15
    };
});
this.Content = treeGrid;
```

## Load Expander Icon Through Template Selector

Use a `DataTemplateSelector` with the `ExpanderIcon` property to display different icons for expanded and collapsed nodes. This lets you show distinct visuals depending on the node's expansion state.

### Define templates in resources

```xaml
<ContentPage.BindingContext>
    <local:EmployeeViewModel/>
</ContentPage.BindingContext>

<ContentPage.Resources>
    <ResourceDictionary>
        <DataTemplate x:Key="Collapsed">
            <Image HeightRequest="12"
                   WidthRequest="12">
                <Image.Source>
                    <FontImageSource Color="Black"
                                     Glyph="&#xe704;"
                                     FontFamily="{OnPlatform iOS=MauiMaterialAssets, MacCatalyst=MauiMaterialAssets, WinUI=MauiMaterialAssets.ttf#, Android=MauiMaterialAssets.ttf#}"/>
                </Image.Source>
            </Image>
        </DataTemplate>
        <DataTemplate x:Key="Expanded">
            <Image HeightRequest="12"
                   WidthRequest="12">
                <Image.Source>
                    <FontImageSource Color="Black"
                                     Glyph="&#xe701;"
                                     FontFamily="{OnPlatform iOS=MauiMaterialAssets, MacCatalyst=MauiMaterialAssets, WinUI=MauiMaterialAssets.ttf#, Android=MauiMaterialAssets.ttf#}"/>
                </Image.Source>
            </Image>
        </DataTemplate>
    </ResourceDictionary>
</ContentPage.Resources>

<syncfusion:SfTreeGrid x:Name="treeGrid"
                       ItemsSource="{Binding PersonDetails}"
                       ChildPropertyName="Children">

    <syncfusion:SfTreeGrid.ExpanderIcon>
        <local:ExpanderIconTemplateSelector ExpandedTemplate="{StaticResource Expanded}"
                                            CollapsedTemplate="{StaticResource Collapsed}"/>
    </syncfusion:SfTreeGrid.ExpanderIcon>

</syncfusion:SfTreeGrid>
```

### Implement the DataTemplateSelector

```csharp
public class ExpanderIconTemplateSelector : DataTemplateSelector
{
    public DataTemplate? ExpandedTemplate { get; set; }

    public DataTemplate? CollapsedTemplate { get; set; }

    protected override DataTemplate? OnSelectTemplate(object item, BindableObject container)
    {
        if (item is TreeNode treeNode)
        {
            return treeNode.IsExpanded
                ? ExpandedTemplate
                : CollapsedTemplate;
        }

        return CollapsedTemplate;
    }
}
```

> **Note:** When using a data template selector, performance issues may occur as converting template views takes time within the framework. Prefer a single `DataTemplate` when the expanded and collapsed visuals are the same.

## Change the Expander Column

By default, the expander icon is displayed in the first column. Display the expander icon in another column by specifying the corresponding column mapping name using the `SfTreeGrid.ExpanderColumn` property.

```xaml
<syncfusion:SfTreeGrid x:Name="treeGrid"
                       ItemsSource="{Binding PersonDetails}"
                       ChildPropertyName="Children"
                       ExpanderColumn="LastName">
</syncfusion:SfTreeGrid>
```

```csharp
SfTreeGrid treeGrid = new SfTreeGrid();
EmployeeViewModel employeeViewModel = new EmployeeViewModel();
treeGrid.ItemsSource = employeeViewModel.PersonDetails;
treeGrid.ChildPropertyName = "Children";
treeGrid.ExpanderColumn = "LastName";
this.Content = treeGrid;
```

## Customize the Expander Column Width

Customize the width of the expander column using the `SfTreeGrid.ExpanderWidth` property.

```xaml
<syncfusion:SfTreeGrid x:Name="treeGrid"
                       ItemsSource="{Binding PersonDetails}"
                       ChildPropertyName="Children"
                       ExpanderWidth="50">
</syncfusion:SfTreeGrid>
```

```csharp
SfTreeGrid treeGrid = new SfTreeGrid();
EmployeeViewModel employeeViewModel = new EmployeeViewModel();
treeGrid.ItemsSource = employeeViewModel.PersonDetails;
treeGrid.ChildPropertyName = "Children";
treeGrid.ExpanderWidth = 50;
this.Content = treeGrid;
```

## Expand Nodes Using a Model Property

Control the initial expansion state of nodes through a property in the underlying data object using the `SfTreeGrid.ExpandStateMappingName` property. This lets each data record declare whether it should start expanded or collapsed, rather than relying solely on `AutoExpandMode`.

```xaml
<syncfusion:SfTreeGrid x:Name="treeGrid"
                       ItemsSource="{Binding PersonDetails}"
                       ChildPropertyName="Children"
                       ExpandStateMappingName="Availability">
</syncfusion:SfTreeGrid>
```

```csharp
SfTreeGrid treeGrid = new SfTreeGrid();
EmployeeViewModel employeeViewModel = new EmployeeViewModel();
treeGrid.ItemsSource = employeeViewModel.PersonDetails;
treeGrid.ChildPropertyName = "Children";
treeGrid.ExpandStateMappingName = "Availability";
this.Content = treeGrid;
```

The referenced property (e.g., `Availability`) should be a `bool` where `true` indicates the node starts expanded and `false` indicates it starts collapsed.

### Choosing an expansion strategy

| Need | Use |
|------|-----|
| Same expansion state for all root/all nodes at load | `AutoExpandMode` (`RootNodesExpanded`, `AllNodesExpanded`, `None`) |
| Per-record initial expansion driven by data | `ExpandStateMappingName` bound to a boolean property |
| Expand/collapse at runtime by user interaction | Default expander column (tap the icon) |
| Programmatic control | `ExpandNode` / `CollapseNode` / `ExpandAllNodes` / `CollapseAllNodes` (see data-binding.md) |
