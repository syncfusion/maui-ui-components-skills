---
name: syncfusion-maui-treeview
description: Implements and customize Syncfusion .NET MAUI TreeView (SfTreeView) for displaying hierarchical data structures. Use when working with TreeView, hierarchical data display, tree structures, organizational charts, nested data, expand/collapse nodes, file explorer UI, folder structures, parent-child relationships, or multi-level data visualization in .NET MAUI applications.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion .NET MAUI TreeView

The Syncfusion .NET MAUI TreeView (SfTreeView) is a powerful data-oriented control for displaying hierarchical data structures such as organizational charts, file systems, nested connections, and multi-level data. It provides intuitive expand/collapse functionality, multiple selection modes, drag-and-drop support, and extensive customization options.

## When to Use This Skill

Use this skill when implementing:

- **Hierarchical Data Display**: Organizational structures, file explorers, category trees, nested menus
- **Interactive Tree Navigation**: Expandable/collapsible nodes, multi-level browsing
- **Data Binding Scenarios**: Bound mode with ItemsSource or unbound mode with manual nodes
- **Selection Requirements**: Single, multiple, or extended selection in tree structures
- **Custom Tree UI**: Templated nodes, custom expanders, styled tree items
- **Advanced Tree Features**: Drag-drop reordering, filtering, sorting, load on demand
- **MVVM Applications**: TreeView with commands, data binding, and ViewModel patterns

## Component Overview

**Key Capabilities:**
- Enhanced performance with optimized view-reusing strategy
- Bound and unbound data modes
- Multiple selection modes with keyboard navigation
- Item and expander templating
- Expand/collapse with animations
- Drag-and-drop node reordering
- Filtering and sorting support
- Load on demand for large datasets
- MVVM pattern support
- RTL (right-to-left) support
- Customizable appearance and styling

## Documentation and Navigation Guide

### Getting Started

📄 **Read:** [references/getting-started.md](references/getting-started.md)

**When to read:** Setting up TreeView for the first time, initial configuration, basic implementation

**Covers:**
- Installing Syncfusion.Maui.TreeView NuGet package
- Registering Syncfusion handler in MauiProgram.cs
- Creating a basic TreeView control
- First working example
- Running and testing the application

---

### Data Binding and Population

📄 **Read:** [references/data-binding.md](references/data-binding.md)

**When to read:** Populating TreeView with data, choosing between bound/unbound modes, hierarchical data structures

**Covers:**
- Bound vs unbound modes comparison
- Creating nodes without data source (unbound mode with TreeViewNode)
- Data binding with ItemsSource (bound mode)
- Defining hierarchical data models
- Using HierarchyPropertyDescriptors for complex hierarchies
- ChildPropertyName configuration
- Self-relational data structures
- ObservableCollection integration

---

### Selection

📄 **Read:** [references/selection.md](references/selection.md)

**When to read:** Implementing node selection, handling selection events, customizing selection appearance

**Covers:**
- Selection modes: Single, SingleDeselect, Multiple, Extended, None
- SelectedItem, CurrentItem, and SelectedItems properties
- SelectionChanging and SelectionChanged events
- SelectionBackground and SelectionForeground customization
- Keyboard navigation (WinUI, MacCatalyst)
- Programmatic selection
- Selection validation and cancellation

---

### Node Expansion and Collapse

📄 **Read:** [references/expand-collapse.md](references/expand-collapse.md)

**When to read:** Configuring expand/collapse behavior, handling expansion events, initial node states

**Covers:**
- AutoExpandMode options (None, AllNodes, RootNodes, specific levels)
- ExpandActionTarget (Expander, Node)
- Programmatic expand/collapse methods
- NodeExpanding and NodeCollapsing events
- IsExpanded property for individual nodes
- Expand/collapse animations
- Event cancellation and validation

---

### Templating and Customization

📄 **Read:** [references/templating.md](references/templating.md)

**When to read:** Customizing node appearance, creating custom expanders, designing tree item UI

**Covers:**
- ItemTemplate for node content customization
- ExpanderTemplate for custom expand/collapse icons
- ItemTemplateContextType (Node vs Data binding context)
- DataTemplate creation and binding
- Mixing images, text, and custom controls
- Template selectors for conditional templates
- Advanced templating patterns

---

### Drag and Drop

📄 **Read:** [references/drag-and-drop.md](references/drag-and-drop.md)

**When to read:** Enabling node reordering, implementing drag-drop functionality, customizing drag behavior

**Covers:**
- Enabling AllowDragging property
- DragStarting, DragOver, and Drop events
- Reordering nodes within the tree
- Drag restrictions and validation
- Visual feedback during drag operations
- Custom drag templates
- Handling drag between different levels

---

### Filtering

📄 **Read:** [references/filtering.md](references/filtering.md)

**When to read:** Filtering tree nodes, implementing search functionality, showing/hiding nodes based on criteria

**Covers:**
- FilterLevel property (Root, All, Extended)
- Filter predicates and expressions
- Filtering hierarchical data
- Clearing and updating filters
- Dynamic filtering with ObservableCollection
- Performance considerations

---

### Sorting

📄 **Read:** [references/sorting.md](references/sorting.md)

**When to read:** Sorting tree nodes, custom sort logic, multi-level sorting

**Covers:**
- SortComparer configuration
- Sorting hierarchical data at each level
- Custom IComparer implementations
- Ascending/descending order
- Sorting with data binding
- Performance optimization for large trees

---

### Styling and Appearance

📄 **Read:** [references/styling-appearance.md](references/styling-appearance.md)

**When to read:** Customizing visual appearance, adjusting spacing and indentation, applying themes

**Covers:**
- Item height customization (ItemHeight property)
- Indentation settings for nested levels
- Liquid glass effect styling
- RTL (right-to-left) support
- Background and foreground colors
- Border and padding customization
- Theme integration

---

### MVVM Support

📄 **Read:** [references/mvvm-support.md](references/mvvm-support.md)

**When to read:** Implementing TreeView with MVVM pattern, using commands and data binding

**Covers:**
- ViewModel setup for TreeView
- INotifyPropertyChanged implementation
- Command binding for node actions
- Data-bound ItemsSource
- Selection binding in MVVM
- Event-to-command patterns
- Best practices for MVVM architecture

---

### Checkbox Support

📄 **Read:** [references/checkbox-support.md](references/checkbox-support.md)

**When to read:** Adding checkboxes to nodes, handling checked/unchecked states, recursive checkbox modes

**Covers:**
- Checkbox in bound mode with CheckedItems
- Checkbox in unbound mode with IsChecked property
- CheckBoxMode property (Recursive, Cascade, None)
- Working with checked items collection
- Programmatic checkbox state management
- Best practices for checkbox implementation

---

### Load on Demand

📄 **Read:** [references/load-on-demand.md](references/load-on-demand.md)

**When to read:** Implementing lazy loading of child nodes, handling large datasets, API-based hierarchies

**Covers:**
- Load on Demand implementation with LoadOnDemandCommand
- ShowExpanderAnimation for loading feedback
- PopulateChildNodes method
- Avoiding duplicate loading
- API-based lazy loading patterns
- Performance optimization with lazy loading

---

### Scrolling and Navigation

📄 **Read:** [references/scrolling-navigation.md](references/scrolling-navigation.md)

**When to read:** Programmatic scrolling, bringing items into view, keyboard navigation

**Covers:**
- BringIntoView method with all overloads
- Scroll position options (Start, Center, End, MakeVisible)
- Scroll animation control
- Horizontal scrolling configuration
- Scrollbar visibility control
- Keyboard navigation (arrow keys, Tab)
- Events (Loaded, ItemTapped, ItemDoubleTapped, ItemRightTapped)

---

### Item Height Customization

📄 **Read:** [references/item-height-customization.md](references/item-height-customization.md)

**When to read:** Customizing node heights, dynamic sizing, content-based measurements

**Covers:**
- Static ItemHeight property
- QueryNodeSize event for per-item heights
- NodeSizeMode property (Dynamic vs None)
- GetActualNodeHeight method
- Dynamic height calculation
- Performance considerations

---

### Empty View

📄 **Read:** [references/empty-view.md](references/empty-view.md)

**When to read:** Displaying empty states, customizing no-data UI, template-based empty views

**Covers:**
- Display string message when empty
- Display custom views in empty state
- EmptyViewTemplate for templated empty views
- Trigger conditions for empty view
- Binding in empty view templates
- Search/filter empty state patterns

---

### Advanced Features

📄 **Read:** [references/advanced-features.md](references/advanced-features.md)

**When to read:** Implementing load on demand, performance optimization, advanced TreeView operations

**Covers:**
- Load on demand for large datasets
- Empty view customization (EmptyView property)
- Scrolling configuration and optimization
- Working with TreeViewNode programmatically
- Checkbox support
- Events

---

### Troubleshooting

📄 **Read:** [references/troubleshooting.md](references/troubleshooting.md)

**When to read:** Debugging issues, resolving errors, optimization problems

**Covers:**
- Common TreeView issues and solutions
- Performance troubleshooting
- Data binding problems
- Selection not working
- Template rendering issues
- Expand/collapse problems
- Memory leaks and optimization
- Best practices checklist

---

