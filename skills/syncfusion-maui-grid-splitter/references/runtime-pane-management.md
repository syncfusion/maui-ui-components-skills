# Runtime Pane Management: Add, Remove, and Control

## Table of Contents
- [Adding Panes Dynamically](#adding-panes-dynamically)
- [Removing Panes](#removing-panes)
- [Pane Indexing and Access](#pane-indexing-and-access)
- [Controlling Pane Visibility](#controlling-pane-visibility)
- [Practical Scenarios](#practical-scenarios)

## Adding Panes Dynamically

The `AddPane` method adds new panes to the Grid Splitter at runtime, enabling dynamic layout modifications.

### AddPane Method

Add a pane to the end of the collection:

```csharp
SfGridSplitter gridSplitter = new SfGridSplitter();

// Create a new pane
SplitterPane newPane = new SplitterPane
{
    Size = "1*",
    Content = new Label
    {
        Text = "New Pane",
        HorizontalTextAlignment = TextAlignment.Center,
        VerticalTextAlignment = TextAlignment.Center
    }
};

// Add to splitter
gridSplitter.AddPane(newPane);
Content = gridSplitter;
```

### Adding Panes with Initial Spacing

```csharp
SfGridSplitter gridSplitter = new SfGridSplitter
{
    Orientation = GridSplitterOrientation.Horizontal
};

// Add initial pane
SplitterPane pane1 = new SplitterPane
{
    Size = "1*",
    Content = new Label { Text = "Initial Pane" }
};
gridSplitter.AddPane(pane1);

// Add second pane
SplitterPane pane2 = new SplitterPane
{
    Size = "1*",
    Content = new Label { Text = "New Pane" }
};
gridSplitter.AddPane(pane2);

Content = gridSplitter;
```

### Adding Multiple Panes in a Loop

Create and add multiple panes programmatically:

```csharp
SfGridSplitter gridSplitter = new SfGridSplitter();

// Create 5 panes
for (int i = 0; i < 5; i++)
{
    SplitterPane pane = new SplitterPane
    {
        Size = "1*",
        Content = new Label
        {
            Text = $"Pane {i + 1}",
            HorizontalTextAlignment = TextAlignment.Center,
            VerticalTextAlignment = TextAlignment.Center
        }
    };
    gridSplitter.AddPane(pane);
}

Content = gridSplitter;
```

### Adding Panes with Rich Content

Create panes with complex layouts:

```csharp
private void AddTabPane(SfGridSplitter splitter, string tabName)
{
    SplitterPane pane = new SplitterPane
    {
        Size = "1*",
        Content = new VerticalStackLayout
        {
            Padding = 10,
            Spacing = 10,
            Children =
            {
                new Label
                {
                    Text = tabName,
                    FontSize = 16,
                    FontAttributes = FontAttributes.Bold
                },
                new Frame
                {
                    BorderColor = Colors.Gray,
                    CornerRadius = 5,
                    Padding = 10,
                    Content = new Label { Text = "Tab content goes here" }
                }
            }
        }
    };
    
    splitter.AddPane(pane);
}
```

## Removing Panes

The `RemovePane` method removes a pane at a specified index from the Grid Splitter.

### RemovePane Method

Remove a pane by index:

```csharp
SfGridSplitter gridSplitter = new SfGridSplitter();

// Add panes
for (int i = 0; i < 3; i++)
{
    SplitterPane pane = new SplitterPane
    {
        Content = new Label { Text = $"Pane {i + 1}" }
    };
    gridSplitter.AddPane(pane);
}

// Remove the middle pane (index 1)
gridSplitter.RemovePane(1);

Content = gridSplitter;
```

### Remove Last Pane

```csharp
if (gridSplitter.SplitterPanes.Count > 0)
{
    int lastIndex = gridSplitter.SplitterPanes.Count - 1;
    gridSplitter.RemovePane(lastIndex);
}
```

### Remove All Panes

```csharp
while (gridSplitter.SplitterPanes.Count > 0)
{
    gridSplitter.RemovePane(0);
}
```

### Important Constraints

- **Minimum panes required**: Grid Splitter requires at least 2 panes to display separators
- **Invalid indexes ignored**: If index is out of range, operation is ignored
- **Automatic layout rebuild**: Layout updates automatically after removal
- **Separators recreated**: Associated separators are rebuilt as needed

## Pane Indexing and Access

Access and manipulate panes using their index in the collection.

### Access Pane by Index

```csharp
// Get first pane
SplitterPane firstPane = gridSplitter.SplitterPanes[0];

// Get last pane
SplitterPane lastPane = gridSplitter.SplitterPanes[gridSplitter.SplitterPanes.Count - 1];

// Get pane at specific index
SplitterPane targetPane = gridSplitter.SplitterPanes[2];
```

### Modify Pane Properties at Runtime

```csharp
// Change pane content
SplitterPane pane = gridSplitter.SplitterPanes[0];
pane.Content = new Label { Text = "Updated Content" };

// Change pane size
pane.Size = "2*";

// Change minimum/maximum size
pane.MinimumSize = 100;
pane.MaximumSize = 500;

// Enable collapse
pane.IsCollapsible = true;
```

### Iterate Through Panes

```csharp
// Process all panes
foreach (SplitterPane pane in gridSplitter.SplitterPanes)
{
    // Modify properties
    pane.MinimumSize = 100;
    pane.BackgroundColor = Colors.White;
}

// Process with index
for (int i = 0; i < gridSplitter.SplitterPanes.Count; i++)
{
    SplitterPane pane = gridSplitter.SplitterPanes[i];
    Debug.WriteLine($"Pane {i}: Size = {pane.Size}");
}
```

### Find Pane by Content

```csharp
// Find pane containing specific content
SplitterPane targetPane = null;
foreach (SplitterPane pane in gridSplitter.SplitterPanes)
{
    if (pane.Content is Label label && label.Text == "Target")
    {
        targetPane = pane;
        break;
    }
}

if (targetPane != null)
{
    // Process pane
}
```

## Controlling Pane Visibility

Manage pane visibility and state at runtime.

### Dynamic Pane Visibility

Show/hide panes based on conditions:

```csharp
public void UpdateLayoutBasedOnRole(UserRole role)
{
    // Admin: show all panes
    if (role == UserRole.Admin)
    {
        // Ensure all panes are visible
        foreach (SplitterPane pane in gridSplitter.SplitterPanes)
        {
            pane.IsVisible = true;
        }
    }
    // User: hide admin panel
    else if (role == UserRole.User)
    {
        if (gridSplitter.SplitterPanes.Count > 2)
        {
            gridSplitter.SplitterPanes[2].IsVisible = false;
        }
    }
}
```

### Toggle Pane Content Visibility

```csharp
// Show detailed view in pane
public void ShowDetailedView(int paneIndex)
{
    SplitterPane pane = gridSplitter.SplitterPanes[paneIndex];
    pane.Content = new DetailedViewLayout();
}

// Show summary view in pane
public void ShowSummaryView(int paneIndex)
{
    SplitterPane pane = gridSplitter.SplitterPanes[paneIndex];
    pane.Content = new SummaryViewLayout();
}
```

### Conditional Pane Addition

```csharp
public void InitializeLayout(LayoutMode mode)
{
    gridSplitter.SplitterPanes.Clear();
    
    // Add common panes
    gridSplitter.AddPane(CreateNavigationPane());
    gridSplitter.AddPane(CreateContentPane());
    
    // Add mode-specific panes
    switch (mode)
    {
        case LayoutMode.Analytics:
            gridSplitter.AddPane(CreateAnalyticsPane());
            break;
        case LayoutMode.Editing:
            gridSplitter.AddPane(CreatePropertiesPane());
            break;
        case LayoutMode.Preview:
            gridSplitter.AddPane(CreatePreviewPane());
            break;
    }
}
```

## Practical Scenarios

### Scenario 1: Tab-Based Content Manager

Dynamically add/remove tabs with panes:

```csharp
public class TabManager
{
    private SfGridSplitter gridSplitter;
    private Dictionary<string, SplitterPane> tabs = new();
    
    public TabManager(SfGridSplitter splitter)
    {
        gridSplitter = splitter;
    }
    
    public void AddTab(string tabName)
    {
        // Create pane for tab
        SplitterPane pane = new SplitterPane
        {
            Size = "1*",
            Content = CreateTabContent(tabName)
        };
        
        gridSplitter.AddPane(pane);
        tabs[tabName] = pane;
    }
    
    public void RemoveTab(string tabName)
    {
        if (tabs.TryGetValue(tabName, out SplitterPane pane))
        {
            int index = gridSplitter.SplitterPanes.IndexOf(pane);
            if (index >= 0)
            {
                gridSplitter.RemovePane(index);
                tabs.Remove(tabName);
            }
        }
    }
    
    public void ActivateTab(string tabName)
    {
        if (tabs.TryGetValue(tabName, out SplitterPane pane))
        {
            pane.BackgroundColor = Colors.LightBlue;
        }
    }
    
    private VerticalStackLayout CreateTabContent(string tabName)
    {
        return new VerticalStackLayout
        {
            Padding = 10,
            Children =
            {
                new Label { Text = $"Tab: {tabName}", FontAttributes = FontAttributes.Bold },
                new Label { Text = "Tab content goes here" }
            }
        };
    }
}

// Usage
TabManager tabManager = new TabManager(gridSplitter);
tabManager.AddTab("Home");
tabManager.AddTab("Settings");
tabManager.RemoveTab("Home");
```

### Scenario 2: Dynamic Document Editor

Add/remove document panels:

```csharp
public class DocumentManager
{
    private SfGridSplitter gridSplitter;
    private List<Document> documents = new();
    
    public void OpenDocument(Document doc)
    {
        SplitterPane pane = new SplitterPane
        {
            Size = "1*",
            Content = CreateEditorPane(doc)
        };
        
        gridSplitter.AddPane(pane);
        documents.Add(doc);
    }
    
    public void CloseDocument(Document doc)
    {
        var pane = gridSplitter.SplitterPanes.FirstOrDefault(p =>
            p.Content is VerticalStackLayout layout &&
            layout.Children[0] is Label label &&
            label.Text == doc.Name);

        if (pane != null)
        {
            int index = gridSplitter.SplitterPanes.IndexOf(pane);
            gridSplitter.RemovePane(index);
            documents.Remove(doc);
        }
    }
    
    public void CloseAllDocuments()
    {
        while (gridSplitter.SplitterPanes.Count > 1)
        {
            gridSplitter.RemovePane(gridSplitter.SplitterPanes.Count - 1);
        }
        documents.Clear();
    }
    
    private VerticalStackLayout CreateEditorPane(Document doc)
    {
        return new VerticalStackLayout
        {
            Padding = 10,
            Children =
            {
                new Label { Text = doc.Name, FontAttributes = FontAttributes.Bold },
                new Editor { Text = doc.Content }
            }
        };
    }
}
```

### Scenario 3: Responsive Multi-View Layout

Switch layouts based on screen size:

```csharp
public class ResponsiveLayout
{
    private SfGridSplitter gridSplitter;
    
    public void AdjustForScreenSize(double screenWidth)
    {
        MainThread.BeginInvokeOnMainThread(() =>
        {
            // Clear existing panes
            while (gridSplitter.SplitterPanes.Count > 0)
            {
                gridSplitter.RemovePane(0);
            }
            
            if (screenWidth < 600)  // Mobile
            {
                gridSplitter.AddPane(CreateNavigationPane());
                gridSplitter.AddPane(CreateContentPane());
            }
            else if (screenWidth < 1200)  // Tablet
            {
                gridSplitter.AddPane(CreateNavigationPane());
                gridSplitter.AddPane(CreateContentPane());
                gridSplitter.AddPane(CreatePreviewPane());
            }
            else  // Desktop
            {
                gridSplitter.AddPane(CreateNavigationPane());
                gridSplitter.AddPane(CreateContentPane());
                gridSplitter.AddPane(CreateDetailsPane());
            }
        });
    }
    
    private SplitterPane CreateNavigationPane() { /* ... */ }
    private SplitterPane CreateContentPane() { /* ... */ }
    private SplitterPane CreatePreviewPane() { /* ... */ }
    private SplitterPane CreateDetailsPane() { /* ... */ }
}
```

### Scenario 4: Context-Based Pane Restructuring

Change layout based on user context or application state:

```csharp
public enum ApplicationContext
{
    Viewing,
    Editing,
    Presenting
}

public void SwitchContext(ApplicationContext context)
{
    MainThread.BeginInvokeOnMainThread(async () =>
    {
        // Clear current panes
        while (gridSplitter.SplitterPanes.Count > 0)
        {
            gridSplitter.RemovePane(0);
        }
        
        switch (context)
        {
            case ApplicationContext.Viewing:
                // Viewing: sidebar + content + details
                gridSplitter.AddPane(CreateFilePaneView());
                gridSplitter.AddPane(CreateMainView());
                gridSplitter.AddPane(CreateDetailsPaneView());
                break;
                
            case ApplicationContext.Editing:
                // Editing: properties + canvas + preview
                gridSplitter.AddPane(CreatePropertiesPane());
                gridSplitter.AddPane(CreateCanvasPane());
                gridSplitter.AddPane(CreatePreviewPane());
                break;
                
            case ApplicationContext.Presenting:
                // Presenting: full content only
                gridSplitter.AddPane(CreateFullContentPane());
                break;
        }
        
        // Trigger layout refresh
        gridSplitter.InvalidateMeasure();
    });
}
```
---

**Next:** Learn about [responsive layouts and accessibility](responsive-layouts.md) for multi-platform support.
