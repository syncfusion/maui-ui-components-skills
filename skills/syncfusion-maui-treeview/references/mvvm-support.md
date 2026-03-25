# MVVM Support in TreeView

Guide to using TreeView with the MVVM pattern.

## Table of Contents
- [Overview](#overview)
- [Binding Properties](#binding-properties)
- [Commands](#commands)
- [Event to Command](#event-to-command)
- [Best Practices](#best-practices)
- [Sample Projects](#sample-projects)
- [Related Topics](#related-topics)

---

## Overview

TreeView provides comprehensive MVVM support through bindable properties, commands, and event-to-command patterns.

---

## Binding Properties

### Binding SelectedItem

```xml
<syncfusion:SfTreeView SelectedItem="{Binding SelectedPlace, Mode=TwoWay}"
                       ItemsSource="{Binding CountriesInfo}"
                       ChildPropertyName="States"/>
```

**ViewModel:**

```csharp
public class CountriesViewModel
{
    public CountriesViewModel()
    {
        GenerateCountriesInfo();
    }
    public ObservableCollection<Countries> CountriesInfo { get; set; }

    public object SelectedPlace { get; set; }

    private void GenerateCountriesInfo()
    {
        var australia = new Countries() { Name = "Australia" };
        var nsw = new Countries() { Name = "New South Wales" };
        var sydney = new Countries() { Name = "Sydney" };
        australia.States = new ObservableCollection<Countries>();
        australia.States.Add(nsw);
        nsw.States = new ObservableCollection<Countries>();
        nsw.States.Add(sydney);
        var usa = new Countries() { Name = "United States of America" };
        var california = new Countries() { Name = "California" };
        var losAngeles = new Countries() { Name = "Los Angeles" };
        usa.States = new ObservableCollection<Countries>();
        usa.States.Add(california);
        california.States = new ObservableCollection<Countries>();
        california.States.Add(losAngeles);
         
        this.CountriesInfo = new ObservableCollection<Countries>();
        CountriesInfo.Add(australia);
        CountriesInfo.Add(usa);

        SelectedPlace = nsw;
    }
}
```

### Binding SelectedItems

```xml
<syncfusion:SfTreeView x:Name="treeView"
                       SelectionMode="Multiple"
                       SelectedItems="{Binding SelectedCountries}"
                       ChildPropertyName="States"
                       ItemsSource="{Binding CountriesInfo}"/>
```

**ViewModel:**

```csharp
public class CountriesViewModel
{
    public CountriesViewModel()
    {
        GenerateCountriesInfo();
    }

    public ObservableCollection<Countries> CountriesInfo { get; set; }

    public ObservableCollection<object> SelectedCountries { get; set; }

    private void GenerateCountriesInfo()
    {
        var australia = new Countries() { Name = "Australia" };
        var nsw = new Countries() { Name = "New South Wales" };
        var sydney = new Countries() { Name = "Sydney" };
        var victoria = new Countries() { Name = "Victoria" };
        australia.States = new ObservableCollection<Countries>();
        australia.States.Add(nsw);
        australia.States.Add(victoria);
        nsw.States = new ObservableCollection<Countries>();
        nsw.States.Add(sydney);
        var usa = new Countries() { Name = "United States of America" };
        var california = new Countries() { Name = "California" };
        usa.States = new ObservableCollection<Countries>();
        usa.States.Add(california);
      
        this.CountriesInfo = new ObservableCollection<Countries>();
        CountriesInfo.Add(australia);
        CountriesInfo.Add(usa);

        SelectedCountries = new ObservableCollection<object>();
        SelectedCountries.Add(nsw);
        SelectedCountries.Add(victoria);
    }
}
```

---

## Commands

### TapCommand

```csharp
treeView.TapCommand = viewModel.TappedCommand;

public class CommandViewModel
{
    public Command<object> TappedCommand { get; }
    
    public CommandViewModel()
    {
        TappedCommand = new Command<object>(OnItemTapped);
    }
    
    private void OnItemTapped(object obj)
    {
        var node = obj as TreeViewNode;
        var data = node.Content as FileManager;
        // Handle tap
    }
}
```

### ExpandCommand

```xml
<syncfusion:SfTreeView ExpandCommand="{Binding ExpandingCommand}"/>
```

```csharp
public ICommand ExpandingCommand { get; }

public CommandViewModel()
{
    ExpandingCommand = new Command<TreeViewNode>(
        execute: OnNodeExpanded,
        canExecute: CanExpand);
}

private bool CanExpand(TreeViewNode node)
{
    var data = node.Content as FileManager;
    return !data.IsLocked;
}

private void OnNodeExpanded(TreeViewNode node)
{
    // Handle expansion
}
```

### CollapseCommand

```xml
<syncfusion:SfTreeView CollapseCommand="{Binding CollapsingCommand}"/>
```

```csharp
public ICommand CollapsingCommand { get; }

public CommandViewModel()
{
    CollapsingCommand = new Command<TreeViewNode>(
        execute: OnNodeCollapsed,
        canExecute: CanCollapse);
}

private bool CanCollapse(TreeViewNode node)
{
    return node.Level > 0; // Don't collapse root
}

private void OnNodeCollapsed(TreeViewNode node)
{
    // Handle collapse
}
```

---

## Event to Command

Use behaviors to convert events to commands:

```xml
<syncfusion:SfTreeView SelectionMode="Multiple"
                       SelectedItems="{Binding SelectedCountries}">
    <syncfusion:SfTreeView.Behaviors>
        <toolkit:EventToCommandBehavior 
            EventName="SelectionChanged" 
            Command="{Binding SelectionChangedCommand}"/>
    </syncfusion:SfTreeView.Behaviors>
</syncfusion:SfTreeView>
```

**ViewModel:**

```csharp
public ICommand SelectionChangedCommand { get; }

public CommandViewModel()
{
    SelectionChangedCommand = new Command<ItemSelectionChangedEventArgs>(OnSelectionChanged);
}

private void OnSelectionChanged(ItemSelectionChangedEventArgs args)
{
    if (args.AddedItems.Count > 0)
    {
        var selected = args.AddedItems[0] as Country;
        // Handle selection change
    }
}
```

---

## Best Practices

1. **Use TwoWay binding** for selection properties
2. **Implement INotifyPropertyChanged** in ViewModels
3. **Use ObservableCollection** for dynamic collections
4. **Leverage commands** instead of event handlers when possible
5. **Use event-to-command** for events not directly supported as commands

---

## Sample Projects

- [Binding Selected Items](https://github.com/SyncfusionExamples/binding-selected-items-in-.net-maui-treeview)
- [Event to Command](https://github.com/SyncfusionExamples/event-to-command-binding-in-.net-maui-treeview)

---

## Related Topics

- [Selection](selection.md) - Selection binding
- [Data Binding](data-binding.md) - Data source binding
- [Expand and Collapse](expand-collapse.md) - Expansion commands
