# Data Binding in .NET MAUI Kanban Board

## Table of Contents
- [Overview](#overview)
- [Default KanbanModel Binding](#default-kanbanmodel-binding)
- [Custom Data Model with Mapping](#custom-data-model-with-mapping)
- [ColumnMappingPath](#columnmappingpath)
- [ItemsSource Configuration](#itemssource-configuration)
- [MVVM Pattern](#mvvm-pattern)
- [ObservableCollection Usage](#observablecollection-usage)
- [Dynamic Data Updates](#dynamic-data-updates)
- [Common Patterns](#common-patterns)
- [Troubleshooting](#troubleshooting)

## Overview

The .NET MAUI Kanban Board (SfKanban) supports flexible data binding through two approaches:

1. **Default KanbanModel** - Use the built-in model with predefined properties
2. **Custom Data Model** - Map your existing models with custom properties

**When to use each approach:**

| Approach | Use When |
|----------|----------|
| **Default KanbanModel** | Starting from scratch, simple requirements, prototyping |
| **Custom Data Model** | Existing models, specific property names, integration with existing systems |

## Default KanbanModel Binding

The simplest approach is using the built-in `KanbanModel` class, which provides standard properties for kanban cards.

### KanbanModel Properties

```csharp
public class KanbanModel
{
    public object ID { get; set; }                    // Unique identifier
    public string Title { get; set; }                 // Card title
    public string Description { get; set; }           // Card description
    public object Category { get; set; }              // Column category (determines placement)
    public string ImageURL { get; set; }              // Image path for card
    public Color IndicatorFill { get; set; }          // Status indicator color
    public List<string> Tags { get; set; }            // Collection of tags/labels
}
```

### Step-by-Step Implementation

**Step 1: Create ViewModel**

```csharp
using Syncfusion.Maui.Kanban;
using System.Collections.ObjectModel;

namespace MyApp
{
    public class KanbanViewModel
    {
        public ObservableCollection<KanbanModel> Cards { get; set; }
        
        public KanbanViewModel()
        {
            Cards = new ObservableCollection<KanbanModel>();
            LoadCards();
        }
        
        private void LoadCards()
        {
            Cards.Add(new KanbanModel
            {
                ID = 1,
                Title = "iOS - 1002",
                Category = "Open",
                Description = "Analyze customer requirements",
                IndicatorFill = Colors.Red,
                Tags = new List<string> { "Incident", "Customer" }
            });

            Cards.Add(new KanbanModel
            {
                ID = 2,
                Title = "UWP - 13",
                Category = "In Progress",
                Description = "Add responsive support to application",
                IndicatorFill = Colors.Orange,
                Tags = new List<string> { "Story", "Customer" }
            });

            Cards.Add(new KanbanModel
            {
                ID = 3,
                Title = "iOS - 11",
                Category = "Code Review",
                Description = "Check login page validation",
                IndicatorFill = Colors.Blue,
                Tags = new List<string> { "Story" }
            });

            Cards.Add(new KanbanModel
            {
                ID = 4,
                Title = "UWP - 21",
                Category = "Done",
                Description = "Check login page validation",
                IndicatorFill = Colors.Green,
                Tags = new List<string> { "Story", "Customer" }
            });
        }
    }
}
```

**Step 2: Bind to SfKanban**

**XAML:**

```xml
<ContentPage xmlns:kanban="clr-namespace:Syncfusion.Maui.Kanban;assembly=Syncfusion.Maui.Kanban"
             xmlns:local="clr-namespace:MyApp">
    
    <kanban:SfKanban x:Name="kanban"
                     AutoGenerateColumns="False"
                     ItemsSource="{Binding Cards}">
        
        <kanban:SfKanban.Columns>
            <kanban:KanbanColumn Title="To Do" Categories="Open" />
            <kanban:KanbanColumn Title="In Progress" Categories="In Progress" />
            <kanban:KanbanColumn Title="Code Review" Categories="Code Review" />
            <kanban:KanbanColumn Title="Done" Categories="Done" />
        </kanban:SfKanban.Columns>
        
        <kanban:SfKanban.BindingContext>
            <local:KanbanViewModel />
        </kanban:SfKanban.BindingContext>
        
    </kanban:SfKanban>
    
</ContentPage>
```

**C#:**

```csharp
SfKanban kanban = new SfKanban();
KanbanViewModel viewModel = new KanbanViewModel();
kanban.AutoGenerateColumns = false;

kanban.Columns.Add(new KanbanColumn
{
    Title = "To Do",
    Categories = new List<object> { "Open" }
});

kanban.Columns.Add(new KanbanColumn
{
    Title = "In Progress",
    Categories = new List<object> { "In Progress" }
});

kanban.Columns.Add(new KanbanColumn
{
    Title = "Code Review",
    Categories = new List<object> { "Code Review" }
});

kanban.Columns.Add(new KanbanColumn
{
    Title = "Done",
    Categories = new List<object> { "Done" }
});

kanban.ItemsSource = viewModel.Cards;
this.Content = kanban;
```

**Key Points:**
- Use `ObservableCollection` for automatic UI updates when items are added/removed
- Each card's `Category` value determines which column it appears in
- The default card template automatically displays all KanbanModel properties

## Custom Data Model with Mapping

When you have existing data models or need custom property names, map your model to the kanban control.

### Step-by-Step Implementation

**Step 1: Create Custom Model**

```csharp
public class TaskDetails
{
    public int TaskID { get; set; }
    public string TaskTitle { get; set; }
    public string TaskDescription { get; set; }
    public string Status { get; set; }
    public string Assignee { get; set; }
    public DateTime DueDate { get; set; }
    public string Priority { get; set; }
}
```

**Step 2: Create ViewModel**

```csharp
using System.Collections.ObjectModel;

namespace MyApp
{
    public class TaskViewModel
    {
        public ObservableCollection<TaskDetails> TaskDetails { get; set; }
        
        public TaskViewModel()
        {
            TaskDetails = GetTaskDetails();
        }

        private ObservableCollection<TaskDetails> GetTaskDetails()
        {
            var tasks = new ObservableCollection<TaskDetails>();

            tasks.Add(new TaskDetails
            {
                TaskID = 1,
                TaskTitle = "UWP Issue",
                TaskDescription = "Sorting is not working properly in DateTimeAxis",
                Status = "Open",
                Assignee = "John Doe",
                DueDate = DateTime.Now.AddDays(5),
                Priority = "High"
            });

            tasks.Add(new TaskDetails
            {
                TaskID = 2,
                TaskTitle = "WPF Issue",
                TaskDescription = "Crosshair label template not visible",
                Status = "In Progress",
                Assignee = "Jane Smith",
                DueDate = DateTime.Now.AddDays(3),
                Priority = "Medium"
            });

            tasks.Add(new TaskDetails
            {
                TaskID = 3,
                TaskTitle = "Kanban Feature",
                TaskDescription = "Provide drag and drop support",
                Status = "Code Review",
                Assignee = "Bob Johnson",
                DueDate = DateTime.Now.AddDays(1),
                Priority = "High"
            });

            tasks.Add(new TaskDetails
            {
                TaskID = 4,
                TaskTitle = "New Feature",
                TaskDescription = "Dragging events support for Kanban",
                Status = "Done",
                Assignee = "Alice Williams",
                DueDate = DateTime.Now.AddDays(-2),
                Priority = "Low"
            });

            return tasks;
        }
    }
}
```

**Step 3: Map to SfKanban with CardTemplate**

**IMPORTANT:** When using a custom data model, you **must** define a `CardTemplate` because the default card UI only works with KanbanModel.

**XAML:**

```xml
<ContentPage xmlns:kanban="clr-namespace:Syncfusion.Maui.Kanban;assembly=Syncfusion.Maui.Kanban"
             xmlns:local="clr-namespace:MyApp">
    
    <kanban:SfKanban x:Name="kanban"
                     ItemsSource="{Binding TaskDetails}"
                     ColumnMappingPath="Status">
        
        <kanban:SfKanban.CardTemplate>
            <DataTemplate>
                <Border Stroke="LightGray"
                        StrokeThickness="1"
                        Background="White"
                        Margin="5"
                        Padding="10">
                    <VerticalStackLayout Spacing="5">
                        <Label Text="{Binding TaskTitle}"
                               FontAttributes="Bold"
                               FontSize="16" />
                        <Label Text="{Binding TaskDescription}"
                               FontSize="12"
                               LineBreakMode="WordWrap"
                               TextColor="Gray" />
                        <HorizontalStackLayout Spacing="10">
                            <Label Text="{Binding Assignee}"
                                   FontSize="12"
                                   TextColor="DarkBlue" />
                            <Label Text="{Binding Priority}"
                                   FontSize="12"
                                   FontAttributes="Bold"
                                   TextColor="Red" />
                        </HorizontalStackLayout>
                    </VerticalStackLayout>
                </Border>
            </DataTemplate>
        </kanban:SfKanban.CardTemplate>
        
        <kanban:SfKanban.BindingContext>
            <local:TaskViewModel />
        </kanban:SfKanban.BindingContext>
        
    </kanban:SfKanban>
    
</ContentPage>
```

**C#:**

```csharp
SfKanban kanban = new SfKanban();
TaskViewModel viewModel = new TaskViewModel();

kanban.ColumnMappingPath = "Status";
kanban.CardTemplate = new DataTemplate(() =>
{
    var titleLabel = new Label
    {
        FontAttributes = FontAttributes.Bold,
        FontSize = 16
    };
    titleLabel.SetBinding(Label.TextProperty, "TaskTitle");

    var descriptionLabel = new Label
    {
        FontSize = 12,
        LineBreakMode = LineBreakMode.WordWrap,
        TextColor = Colors.Gray
    };
    descriptionLabel.SetBinding(Label.TextProperty, "TaskDescription");

    var assigneeLabel = new Label
    {
        FontSize = 12,
        TextColor = Colors.DarkBlue
    };
    assigneeLabel.SetBinding(Label.TextProperty, "Assignee");

    var priorityLabel = new Label
    {
        FontSize = 12,
        FontAttributes = FontAttributes.Bold,
        TextColor = Colors.Red
    };
    priorityLabel.SetBinding(Label.TextProperty, "Priority");

    var horizontalStack = new HorizontalStackLayout
    {
        Spacing = 10,
        Children = { assigneeLabel, priorityLabel }
    };

    var verticalStack = new VerticalStackLayout
    {
        Spacing = 5,
        Children = { titleLabel, descriptionLabel, horizontalStack }
    };

    var border = new Border
    {
        Stroke = Colors.LightGray,
        StrokeThickness = 1,
        Background = Colors.White,
        Margin = new Thickness(5),
        Padding = new Thickness(10),
        Content = verticalStack
    };

    return border;
});

kanban.ItemsSource = viewModel.TaskDetails;
this.Content = kanban;
```

**Key Points:**
- Set `ColumnMappingPath` to the property name that determines column placement (e.g., "Status")
- **Must define CardTemplate** - default card UI doesn't work with custom models
- Bind template elements to your custom properties
- Columns are auto-generated based on unique values in the mapped property

## ColumnMappingPath

The `ColumnMappingPath` property specifies which property in your data model determines card placement in columns.

### How It Works

```csharp
// Custom model with "Status" property
public class TaskDetails
{
    public string Status { get; set; }  // This will be mapped
}

// Set ColumnMappingPath
kanban.ColumnMappingPath = "Status";
```

**With Default KanbanModel:**
- If not set, defaults to the `Category` property
- No need to explicitly set it unless using a different property

**With Custom Models:**
- **Required** - must specify the property name
- Property can be string, int, or any type
- Used for both auto-generation and manual column mapping

### Auto-Generation vs Manual Columns

**Auto-Generated Columns (ColumnMappingPath + AutoGenerateColumns=True):**

```xml
<kanban:SfKanban ItemsSource="{Binding TaskDetails}"
                 ColumnMappingPath="Status"
                 AutoGenerateColumns="True" />
```

**Result:** Columns are automatically created for each unique `Status` value.

**Manual Columns (ColumnMappingPath + Manual Definition):**

```xml
<kanban:SfKanban ItemsSource="{Binding TaskDetails}"
                 ColumnMappingPath="Status"
                 AutoGenerateColumns="False">
    <kanban:SfKanban.Columns>
        <kanban:KanbanColumn Title="Backlog" Categories="Open,Postponed" />
        <kanban:KanbanColumn Title="Active" Categories="In Progress" />
        <kanban:KanbanColumn Title="Completed" Categories="Done,Closed" />
    </kanban:SfKanban.Columns>
</kanban:SfKanban>
```

**Result:** Only the defined columns appear, grouped by multiple status values.

## ItemsSource Configuration

The `ItemsSource` property accepts any `IEnumerable` collection, but `ObservableCollection` is recommended for dynamic updates.

### Supported Collection Types

```csharp
// ObservableCollection (Recommended)
public ObservableCollection<KanbanModel> Cards { get; set; }

// List (No auto-updates)
public List<KanbanModel> Cards { get; set; }

// IEnumerable (Read-only)
public IEnumerable<KanbanModel> Cards { get; set; }
```

### Setting ItemsSource

**XAML Binding:**

```xml
<kanban:SfKanban ItemsSource="{Binding Cards}" />
```

**Code-Behind:**

```csharp
kanban.ItemsSource = viewModel.Cards;
```

**Property Changed:**

```csharp
// In ViewModel
private ObservableCollection<KanbanModel> _cards;
public ObservableCollection<KanbanModel> Cards
{
    get => _cards;
    set
    {
        _cards = value;
        OnPropertyChanged();
    }
}
```

## MVVM Pattern

Best practice is to use the MVVM (Model-View-ViewModel) pattern for data binding.

### Complete MVVM Example

**Model (TaskModel.cs):**

```csharp
public class TaskModel
{
    public int ID { get; set; }
    public string Title { get; set; }
    public string Description { get; set; }
    public string Status { get; set; }
    public string Assignee { get; set; }
}
```

**ViewModel (TaskViewModel.cs):**

```csharp
using System.Collections.ObjectModel;
using System.ComponentModel;
using System.Runtime.CompilerServices;

public class TaskViewModel : INotifyPropertyChanged
{
    private ObservableCollection<TaskModel> _tasks;
    
    public ObservableCollection<TaskModel> Tasks
    {
        get => _tasks;
        set
        {
            _tasks = value;
            OnPropertyChanged();
        }
    }

    public TaskViewModel()
    {
        LoadTasks();
    }

    private void LoadTasks()
    {
        Tasks = new ObservableCollection<TaskModel>
        {
            new TaskModel
            {
                ID = 1,
                Title = "Design UI",
                Description = "Create mockups",
                Status = "Open",
                Assignee = "John"
            },
            // More tasks...
        };
    }

    public event PropertyChangedEventHandler PropertyChanged;

    protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

**View (MainPage.xaml):**

```xml
<ContentPage xmlns:kanban="clr-namespace:Syncfusion.Maui.Kanban;assembly=Syncfusion.Maui.Kanban"
             xmlns:local="clr-namespace:MyApp">
    
    <ContentPage.BindingContext>
        <local:TaskViewModel />
    </ContentPage.BindingContext>
    
    <kanban:SfKanban ItemsSource="{Binding Tasks}"
                     ColumnMappingPath="Status">
        <kanban:SfKanban.CardTemplate>
            <!-- Card template here -->
        </kanban:SfKanban.CardTemplate>
    </kanban:SfKanban>
    
</ContentPage>
```

## ObservableCollection Usage

`ObservableCollection` automatically notifies the UI when items are added, removed, or replaced.

### Adding Items Dynamically

```csharp
public void AddNewTask()
{
    Cards.Add(new KanbanModel
    {
        ID = Cards.Count + 1,
        Title = "New Task",
        Category = "Open",
        Description = "Description",
        IndicatorFill = Colors.Blue
    });
    // UI updates automatically
}
```

### Removing Items

```csharp
public void RemoveTask(KanbanModel card)
{
    Cards.Remove(card);
    // UI updates automatically
}
```

### Updating Items

```csharp
// Changing a property
public void UpdateTask(KanbanModel card)
{
    card.Category = "Done";  // Won't trigger UI update
    
    // Need to implement INotifyPropertyChanged in KanbanModel
    // OR replace the item
    var index = Cards.IndexOf(card);
    Cards[index] = card;  // This triggers UI update
}
```

## Dynamic Data Updates

### Refreshing ItemsSource

To completely refresh the data:

```csharp
public void RefreshData()
{
    var newCards = LoadCardsFromDatabase();
    Cards.Clear();
    foreach (var card in newCards)
    {
        Cards.Add(card);
    }
}
```

### Filtering Data

```csharp
private ObservableCollection<KanbanModel> _allCards;
public ObservableCollection<KanbanModel> FilteredCards { get; set; }

public void FilterByAssignee(string assignee)
{
    FilteredCards.Clear();
    var filtered = _allCards.Where(c => c.Tags.Contains(assignee));
    foreach (var card in filtered)
    {
        FilteredCards.Add(card);
    }
}
```

## Common Patterns

### Pattern 1: Loading from API

```csharp
public async Task LoadTasksFromAPI()
{
    var tasks = await _apiService.GetTasksAsync();
    Tasks.Clear();
    foreach (var task in tasks)
    {
        Tasks.Add(task);
    }
}
```

### Pattern 2: Grouping Multiple Categories

```xml
<kanban:KanbanColumn Title="Backlog" 
                     Categories="Open,Postponed,New" />
```

This groups multiple status values into one column.

### Pattern 3: Lazy Loading

```csharp
public async Task LoadMore()
{
    var moreTasks = await _apiService.GetNextPageAsync();
    foreach (var task in moreTasks)
    {
        Tasks.Add(task);
    }
}
```

## Troubleshooting

### Issue: Cards not appearing in kanban

**Check:**
1. ItemsSource is bound correctly
2. BindingContext is set
3. Category/ColumnMappingPath property matches column Categories
4. Collection is not null or empty

**Debug:**
```csharp
System.Diagnostics.Debug.WriteLine($"Cards count: {Cards.Count}");
System.Diagnostics.Debug.WriteLine($"First card category: {Cards.FirstOrDefault()?.Category}");
```

### Issue: Custom model not displaying

**Solution:** Ensure you've defined a `CardTemplate`. The default template only works with `KanbanModel`.

### Issue: UI not updating when data changes

**Solution:** 
1. Use `ObservableCollection` instead of `List`
2. Implement `INotifyPropertyChanged` in your model if updating properties
3. For property changes, replace the item in the collection

### Issue: Column mismatch

**Problem:** Cards appear in wrong columns or not at all.

**Solution:**
1. Ensure `ColumnMappingPath` matches your property name (case-sensitive)
2. Verify `Categories` in column definition matches data values
3. Check for typos or extra whitespace in category values

**Example:**
```csharp
// Data has "In Progress" but column expects "InProgress"
// Solution: Match exactly
<kanban:KanbanColumn Categories="In Progress" />
```

## Next Steps

- **Customize cards:** See [cards.md](cards.md)
- **Configure columns:** See [columns.md](columns.md)
- **Handle events:** See [events.md](events.md)