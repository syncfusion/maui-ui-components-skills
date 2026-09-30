---
name: syncfusion-maui-treeview
description: Implement and customize Syncfusion® .NET MAUI TreeView (SfTreeView) for displaying hierarchical data structures. Use when working with TreeView, hierarchical data display, tree structures, organizational charts, nested data, expand/collapse nodes, file explorer UI, folder structures, parent-child relationships, or multi-level data visualization in .NET MAUI applications.
metadata:
  author: "Syncfusion Inc"
  version: "34.1.29"
---

# Implementing Syncfusion® .NET MAUI TreeView

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

### Data Binding and Population

📄 **Read:** [references/data-binding.md](references/data-binding.md)

**When to read:** Populating TreeView with data, choosing between bound/unbound modes, hierarchical data structures

**Covers:**
- Bound vs unbound modes comparison
- Creating nodes without data source (unbound mode with TreeViewNode)
- Data binding with ItemsSource (bound mode)
- Using HierarchyPropertyDescriptors for complex hierarchies
- ChildPropertyName configuration

### Selection

📄 **Read:** [references/selection.md](references/selection.md)

**When to read:** Implementing node selection, handling selection events, customizing selection appearance

**Covers:**
- Selection modes: Single, SingleDeselect, Multiple, Extended, None
- SelectedItem, CurrentItem, and SelectedItems properties
- SelectionChanging and SelectionChanged events
- SelectionBackground and SelectionForeground customization
- Keyboard navigation (WinUI, MacCatalyst)

### Node Expansion and Collapse

📄 **Read:** [references/expand-collapse.md](references/expand-collapse.md)

**When to read:** Configuring expand/collapse behavior, handling expansion events, initial node states

**Covers:**
- AutoExpandMode options (None, AllNodes, RootNodes, specific levels)
- ExpandActionTarget (Expander, Node)
- NodeExpanding and NodeCollapsing events
- IsExpanded property for individual nodes
- Expand/collapse animations

### Data Operation

📄 **Read:** [references/filtering.md](references/data-operation.md)

**When to read:** Filtering tree nodes, implementing search functionality, showing/hiding nodes based on criteria, Sorting tree nodes, custom sort logic, multi-level sorting

**Covers:**
- FilterMode (None, Contains, StartsWith, Equals, Custom)
- Filtering API (FilterText, FilterPath, FilterPaths, AutoExpandOnFilter, FilteredItems)
- SortComparer configuration
- Sorting hierarchical data at each level

### Styling and Appearance

📄 **Read:** [references/styling-appearance.md](references/styling-appearance.md)

**When to read:** Customizing visual appearance, adjusting spacing and indentation, applying themes, Customizing node appearance, creating custom expanders, designing tree item UI

**Covers:**
- Item height customization (ItemHeight property)
- Indentation settings for nested levels
- Liquid glass effect styling
- Background and foreground colors
- Border and padding customization
- Theme integration
- ItemTemplate, ExpanderTemplate customization and Template selectors for conditional templates
- ItemTemplateContextType (Node vs Data binding context)

### Checkbox

📄 **Read:** [references/styling-appearance.md](references/checkbox.md)

**When to read:** Adding checkbox functionality to tree nodes, tracking checked items, customizing checkbox behavior and appearance

**Covers:**
- Checkbox enablement and state management
- CheckBoxMode configuration:
  - None
  - Individual
  - Recursive
- TreeViewNode.IsChecked property
- CheckedItems collection binding
- GetCheckedNodes API
- CheckBoxPosition and CheckBoxWidth customization
- CheckActionTarget configuration
- Parent-child recursive checkbox synchronization
- Bound mode and unbound mode checkbox handling
- Custom checkbox templates with SfCheckBox
- Programmatic checkbox 

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

### Item Height Customization

📄 **Read:** [references/item-height-customization.md](references/item-height-customization.md)

**When to read:** Customizing node heights, dynamic sizing, content-based measurements

**Covers:**
- Static ItemHeight property
- QueryNodeSize event for per-item heights
- NodeSizeMode property (Dynamic vs None)
- GetActualNodeHeight method
- Dynamic height calculation

### Advanced Features

📄 **Read:** [references/advanced-features.md](references/advanced-features.md)

**When to read:** Implementing advanced TreeView operations, events, Right-to-left, Enabling node reordering, implementing drag-drop functionality, customizing drag behavior, mplementing lazy loading of child nodes, handling large datasets, API-based hierarchies, Adding checkboxes to nodes, handling checked/unchecked states, recursive checkbox modes, Displaying empty states, customizing no-data UI, template-based empty views

**Covers:**
- Working with TreeViewNode programmatically
- Right-to-Left (RTL)
- Events
- Drag and Drop
- Load on Demand
- Checkbox, CheckBoxMode (Recursive, Cascade, None), Working with checked items collection
- Display view when empty item state
- EmptyViewTemplate for templated empty views

### Troubleshooting

📄 **Read:** [references/troubleshooting.md](references/troubleshooting.md)

**When to read:** Debugging issues, resolving errors, optimization problems

**Covers:**
- Common TreeView issues and solutions
- Performance troubleshooting
- Data binding problems
- Selection not working
- Template rendering issues
