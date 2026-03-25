---
name: syncfusion-maui-kanban-board
description: Implements Syncfusion .NET MAUI Kanban Board (SfKanban) control. Use when implementing kanban boards, task management visualizations, workflow tracking, or agile boards in .NET MAUI applications. Covers installation, data binding with default and custom models, card customization, column configuration, workflow transitions, and drag-and-drop events.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Kanban Boards in .NET MAUI

## When to Use This Skill

Use this skill when the user needs to:

- **Implement kanban boards** in .NET MAUI applications for task management
- **Visualize workflows** at various stages of completion (To Do, In Progress, Done, etc.)
- **Create agile boards** for sprint planning and task tracking
- **Build card-based interfaces** for organizing and managing items across columns
- **Add drag-and-drop functionality** for moving tasks between workflow stages
- **Implement WIP limits** (Work In Progress) for columns
- **Configure workflow transitions** with restrictions on card movement
- **Customize card templates** with custom data models and visual designs
- **Add task categorization** and status indicators
- **Support RTL/LTR layouts** with localization for international audiences

This skill covers the **Syncfusion .NET MAUI Kanban Board (SfKanban)** control, which provides an efficient way to visualize and manage workflows with rich customization options.

## Component Overview

The **SfKanban** control is a highly interactive and customizable kanban board for .NET MAUI applications. It enables:

- **Workflow visualization** across multiple stages (columns)
- **Drag-and-drop** card movement between columns
- **Work-in-progress (WIP) limits** to control task flow
- **Flexible data binding** with default or custom models
- **Rich card customization** including templates, images, tags, and indicators
- **Column management** with auto-generation or manual configuration
- **Workflow rules** to restrict card transitions
- **Events** for card interactions and state changes
- **Sorting and filtering** capabilities
- **Localization** for multi-language support
- **Visual effects** including liquid glass styling

**Key Features:**
- Visualize the workflow of any process
- Limit work in progress (WIP)
- Manage workflow transitions with allowed/blocked states
- Customize at a high level (cards, columns, themes)
- Smooth transitions within processes
- Support for both default and custom data models

## Documentation and Navigation Guide

### Getting Started

📄 **Read:** [references/getting-started.md](references/getting-started.md)

**When to read:** User needs to set up SfKanban for the first time, install packages, or create their first kanban board.

**Topics covered:**
- Installing Syncfusion.Maui.Kanban NuGet package
- Registering the handler with ConfigureSyncfusionCore
- Basic SfKanban initialization in XAML and C#
- Creating your first kanban board
- Running and testing the application

### Data Binding

📄 **Read:** [references/data-binding.md](references/data-binding.md)

**When to read:** User needs to populate kanban with data, use default KanbanModel, create custom data models, or configure ItemsSource.

**Topics covered:**
- Using default KanbanModel with built-in properties
- Creating custom data models with data mapping
- Setting up ItemsSource with ObservableCollection
- Understanding ColumnMappingPath for column categorization
- AutoGenerateColumns vs manual column definition
- ViewModel pattern for MVVM architecture
- Binding context configuration

### Cards

📄 **Read:** [references/cards.md](references/cards.md)

**When to read:** User needs to customize card appearance, add images, tags, indicators, or create custom card templates.

**Topics covered:**
- Card properties (Title, ImageURL, Category, Description, ID)
- IndicatorFill for status colors
- Tags collection for labels
- CardTemplate for complete customization
- Image handling (assembly reference vs local paths)
- Card layout and design patterns
- Default vs custom card UI
- BindingContext for custom models

### Columns

📄 **Read:** [references/columns.md](references/columns.md)

**When to read:** User needs to configure columns, set width constraints, define categories, enable WIP limits, or customize column appearance.

**Topics covered:**
- Column sizing (MinimumColumnWidth, MaximumColumnWidth, ColumnWidth)
- Categorizing columns with Categories property
- AutoGenerateColumns configuration
- Manual column definition with KanbanColumn
- Multiple categories per column
- Work-in-progress (WIP) limits (MinimumLimit, MaximumLimit)
- Column templates and customization
- Column headers and titles

### Workflows

📄 **Read:** [references/workflows.md](references/workflows.md)

**When to read:** User needs to restrict card movement, define allowed transitions, or prevent drag-and-drop on specific columns.

**Topics covered:**
- KanbanWorkflow class definition
- Category property for source state
- AllowedTransitions for target states
- Restricting card movement between columns
- Preventing specific transitions
- Workflow validation
- State management examples

### Events

📄 **Read:** [references/events.md](references/events.md)

**When to read:** User needs to handle drag-and-drop events, card taps, or respond to card interactions.

**Topics covered:**
- Drag and drop event handling
- DragStart, DragOver, DragEnd events
- Card tapped/clicked events
- Event arguments and data access
- Custom event handlers
- Preventing or allowing drag operations
- Event-based validation

### Sorting

📄 **Read:** [references/sorting.md](references/sorting.md)

**When to read:** User needs to sort cards within columns or implement custom sorting logic.

**Topics covered:**
- Sorting configuration
- Custom sorting logic
- Sort order options (ascending, descending)
- Multiple sort keys
- IComparer implementation

### Advanced Features

📄 **Read:** [references/advanced-features.md](references/advanced-features.md)

**When to read:** User needs RTL support, localization, or multi-language configuration.

**Topics covered:**
- Flow direction (LeftToRight, RightToLeft)
- Localization support for international apps
- Resource file configuration
- Multi-language setup
- Culture-specific formatting

### Customization and Styling

📄 **Read:** [references/customization.md](references/customization.md)

**When to read:** User needs visual customization, liquid glass effect, theme integration, or migration guidance.

**Topics covered:**
- Liquid glass effect for modern UI styling
- Visual customization techniques
- Theme integration with application themes
- Custom templates for cards and columns
- Styling best practices
- Migration guidance from older versions

