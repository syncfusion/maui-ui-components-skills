# Advanced Features

Comprehensive guide to advanced TreeView capabilities. Quick reference for all advanced features with links to dedicated documentation for in-depth coverage.

## Table of Contents
- [Right-to-Left (RTL)](#right-to-left-rtl)
- [Working with TreeViewNode](#working-with-treeviewnode)
- [Events](#events)
- [Drag and Drop](#drag-and-drop)
- [Load on Demand](#load-on-demand)
- [Checkbox](#checkbox)
- [EmptyView](#emptyview)
- [Best Practices](#best-practices)

---

## Right-to-Left (RTL)

Support for right-to-left languages and layouts.

> - RTL implementation patterns
> - Multi-language support (Arabic, Hebrew, Persian)
> - Dynamic RTL toggle
> - RTL templates and keyboard navigation
> - Platform-specific considerations

### Enable RTL

```xaml
<syncfusion:SfTreeView x:Name="treeView" FlowDirection="RightToLeft"/>
```

```csharp
treeView.FlowDirection = FlowDirection.RightToLeft;
```

---

## Working with TreeViewNode

### Accessing TreeViewNode

```csharp
// Get node at index
var node = treeView.Nodes[5];

// Get all nodes
var allNodes = treeView.Nodes;
```

### Node Properties

```csharp
var node = treeView.GetNodeAtRowIndex(0);

var level = node.Level;                 // Depth in tree
var hasChildNodes = node.HasChildNodes; // Has children
var isExpanded = node.IsExpanded;       // Expanded state
var content = node.Content;             // Data object
var parentNode = node.ParentNode;       // Parent reference
var childNodes = node.ChildNodes;       // Children
```

---

## Events
### Loaded Event

```csharp
treeView.Loaded += OnTreeViewLoaded;

private void OnTreeViewLoaded(object sender, TreeViewLoadedEventArgs e)
{
    // TreeView fully loaded
}
```

### ItemTapped Event

```csharp
treeView.ItemTapped += OnItemTapped;

private void OnItemTapped(object sender, ItemTappedEventArgs e)
{
    var node = e.Node;
    var data = node.Content as FileManager;
}
```

### ItemDoubleTapped Event

```csharp
treeView.ItemDoubleTapped += OnItemDoubleTapped;

private void OnItemDoubleTapped(object sender, ItemDoubleTappedEventArgs e)
{
    var node = e.Node;
    // Handle double tap
}
```

### ItemRightTapped Event

```csharp
treeView.ItemRightTapped  += OnItemRightTapped;

private void OnItemRightTapped(object sender, ItemRightTappedEventArgs e)
{
    var node = e.Node;
    // Handle right tap
}
```

### ItemLongPress Event

```csharp
treeView.ItemLongPress += OnItemLongPress;

private void OnItemLongPress(object sender, ItemLongPressEventArgs e)
{
    var node = e.Node;
    var position = e.Position;
    // Show context menu
}
```

### KeyDown Event

```csharp
treeView.KeyDown += OnKeyDown;

private void OnKeyDown(object sender, TreeViewKeyEventArgs e)
{
    if (e.Key == "F2")
    {
        // Enter edit mode
        e.Handled = true;
    }
}
```

## Drag and Drop

TreeView supports drag-and-drop reordering of nodes within the tree. Nodes can be dropped:
- **Above** another node
- **Below** another node
- **As a child** of another node

---

### Enable Drag and Drop

Set the `AllowDragging` property to `true`.

**XAML:**

```xaml
<syncfusion:SfTreeView x:Name="treeView"
                       ItemsSource="{Binding Folders}"
                       ChildPropertyName="SubFiles"
                       AllowDragging="True"/>
```

**C#:**

```csharp
treeView.AllowDragging = true;
```

**Note:** Drag-and-drop is NOT supported when Load on Demand is enabled.

---

### Dragging Multiple Items

Enable multiple selection to drag multiple items simultaneously.

```xaml
<syncfusion:SfTreeView x:Name="treeView"
                       SelectionMode="Multiple"
                       AllowDragging="True"
                       ItemsSource="{Binding Files}"
                       ChildPropertyName="SubFiles"/>
```

**Behavior:**
- Select multiple items using `Multiple` or `Extended` selection mode
- Drag any selected item to move all selected items together

---

### Customize Drag Item View

Use `DragItemTemplate` to customize the dragging visual.

```xaml
<syncfusion:SfTreeView AllowDragging="True">
    <syncfusion:SfTreeView.DragItemTemplate>
        <DataTemplate>
            <Border Padding="8" 
                    StrokeThickness="1"  
                    Stroke="#6750A4"
                    Background="White">
                <Border.StrokeShape>
                    <RoundRectangle CornerRadius="8"/>
                </Border.StrokeShape>
                <HorizontalStackLayout Spacing="8">
                    <Image Source="{Binding ImageIcon}"
                           WidthRequest="24" 
                           HeightRequest="24"/>
                    <Label Text="{Binding FolderName}"
                           VerticalOptions="Center"/>
                </HorizontalStackLayout>
            </Border>
        </DataTemplate>
    </syncfusion:SfTreeView.DragItemTemplate>
</syncfusion:SfTreeView>
```

---

### ItemDragging Event

Handle drag-and-drop lifecycle events.

#### Event Actions

| Action | Description | When Fired |
|--------|-------------|------------|
| `Start` | Drag operation begins | When user starts dragging |
| `Dragging` | Item is being dragged | Continuously while dragging |
| `Dropping` | Item is about to be dropped | Before drop completes |
| `Drop` | Item was dropped | After drop completes |

#### Basic Event Handling

```csharp
treeView.ItemDragging += TreeView_ItemDragging;

private void TreeView_ItemDragging(object sender, ItemDraggingEventArgs e)
{
    switch (e.Action)
    {
        case DragAction.Start:
            // Drag started
            break;
            
        case DragAction.Dragging:
            // Currently dragging
            break;
            
        case DragAction.Dropping:
            // About to drop
            break;
            
        case DragAction.Drop:
            // Dropped
            break;
    }
}
```

#### Disable Dragging for Specific Items

```csharp
private void TreeView_ItemDragging(object sender, ItemDraggingEventArgs e)
{
    if (e.Action == DragAction.Start)
    {
        var item = e.DraggingNodes[0].Content as FileManager;
        
        // Prevent dragging locked files
        if (item.IsLocked)
        {
            e.Cancel = true;
        }
    }
}
```

#### Cancel Dropping

```csharp
private void TreeView_ItemDragging(object sender, ItemDraggingEventArgs e)
{
    if (e.Action == DragAction.Dropping)
    {
        var targetItem = e.TargetNode.Content as Folder;
        
        // Prevent dropping into read-only folders
        if (targetItem.IsReadOnly)
        {
            e.Cancel = true;
        }
    }
}
```

---

### Auto Scroll Options

#### Scroll Margin

Adjust the margin that triggers auto-scrolling.

```csharp
// Auto-scroll when drag item is within 20px of edge
treeView.AutoScroller.ScrollMargin = 20;

// Disable auto-scroll
treeView.AutoScroller.ScrollMargin = 0;
```

**Default:** 15 pixels

#### Scroll Interval

Adjust the auto-scroll speed.

```csharp
// Scroll every 200 milliseconds
treeView.AutoScroller.Interval = new TimeSpan(0, 0, 0, 0, 200);
```

**Default:** 150 milliseconds

#### Disable Outside Scroll

Prevent scrolling when dragged item leaves the TreeView bounds.

```csharp
treeView.AutoScroller.AllowOutsideScroll = false;
```

**Default:** `true` (allows outside scroll)

---

### Auto Expand

#### Enable Auto Expand

Automatically expand nodes when hovering during drag.

**XAML:**

```xaml
<syncfusion:SfTreeView AllowDragging="True">
    <syncfusion:SfTreeView.DragAndDropController>
        <syncfusion:DragAndDropController CanAutoExpand="True"/>
    </syncfusion:SfTreeView.DragAndDropController>
</syncfusion:SfTreeView>
```

**C#:**

```csharp
treeView.DragAndDropController.CanAutoExpand = true;
```

#### Auto Expand Delay

Set the delay before auto-expansion.

**XAML:**

```xaml
<syncfusion:DragAndDropController CanAutoExpand="True" 
                                 AutoExpandDelay="0:0:1"/>
```

**C#:**

```csharp
treeView.DragAndDropController.AutoExpandDelay = new TimeSpan(0, 0, 1);
```

**Default:** 3 seconds

---

### Restrictions and Validation

#### Validate Drop Position

```csharp
private void TreeView_ItemDragging(object sender, ItemDraggingEventArgs e)
{
    if (e.Action == DragAction.Dropping)
    {
        var draggedItem = e.DraggingNodes[0].Content as FileItem;
        var targetItem = e.TargetNode.Content as FileItem;
        
        // Only allow files to be dropped into folders
        if (draggedItem.IsFile && !targetItem.IsFolder)
        {
            e.Cancel = true;
        }
        
        // Prevent dropping parent into its own child
        if (IsDescendant(e.TargetNode, e.DraggingNodes[0]))
        {
            e.Cancel = true;
        }
    }
}

private bool IsDescendant(TreeViewNode target, TreeViewNode source)
{
    var node = target;
    while (node != null)
    {
        if (node == source)
            return true;
        node = node.ParentNode;
    }
    return false;
}
```

#### Custom Drop Indicator

```csharp
private void TreeView_ItemDragging(object sender, ItemDraggingEventArgs e)
{
    if (e.Action == DragAction.Dragging)
    {
        // Access drag position
        var position = e.Position;
        
        // Access drop position indicator
        var dropPosition = e.DropPosition;
        
        // Handle custom visualization
        if (e.Handled)
        {
            // Custom drag handling
        }
    }
}
```

---

### Limitations

#### Invalid Drop Scenarios

Drag-and-drop will show an "Invalid drop" indicator in these cases:

1. **Drop as child into same node**
   - Cannot drop a node as its own child

2. **Incompatible child node type** (with HierarchyPropertyDescriptors)
   - Target node's child type doesn't match dragged item type

3. **Different parent types** (with HierarchyPropertyDescriptors)
   - Siblings must have compatible types

#### Not Supported

- **Load on Demand:** Drag-and-drop cannot be used with load-on-demand enabled
- **Cross-TreeView:** Cannot drag between different TreeView controls
- **External Drops:** Cannot drag items from outside the TreeView

---

#### Sample Projects

- [Drag and Drop Customization](https://github.com/SyncfusionExamples/how-to-customize-the-drag-item-view)

---

## Load On Demand

Load on Demand (lazy loading) allows loading child items only when:
- A user expands a parent node
- The parent is scrolled into view
- Items are explicitly requested

**Use Cases:**
- Large hierarchies (thousands of nodes)
- Data from remote APIs
- Performance optimization
- Gradual data discovery

**Important:** Only applicable in **bound mode**.

---

### Implementation

#### Step 1: Create Data Model

```csharp
public class MenuItem : INotifyPropertyChanged
{
    private string itemName;
    private int id;
    private bool hasChildNodes;
    private ObservableCollection<MenuItem> subMenuItems;

    public string ItemName
    {
        get { return itemName; }
        set
        {
            itemName = value;
            OnPropertyChanged("ItemName");
        }
    }

    public int ID
    {
        get { return id; }
        set
        {
            id = value;
            OnPropertyChanged("ID");
        }
    }

    public bool HasChildNodes
    {
        get { return hasChildNodes; }
        set
        {
            hasChildNodes = value;
            OnPropertyChanged("HasChildNodes");
        }
    }

    public ObservableCollection<MenuItem> SubMenuItems
    {
        get { return subMenuItems; }
        set
        {
            subMenuItems = value;
            OnPropertyChanged("SubMenuItems");
        }
    }

    public event PropertyChangedEventHandler PropertyChanged;

    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

#### Step 2: Create ViewModel with LoadOnDemandCommand

```csharp
public class MainViewModel : INotifyPropertyChanged
{
    public ObservableCollection<MenuItem> Menu { get; set; }
    public ICommand TreeViewOnDemandCommand { get; set; }

    public MainViewModel()
    {
        Menu = GetMenuItems();
        TreeViewOnDemandCommand = new Command(ExecuteOnDemandLoading, CanExecuteOnDemandLoading);
    }

    // Check if node has children before loading
    private bool CanExecuteOnDemandLoading(object sender)
    {
        if (sender is not TreeViewNode node)
            return false;

        var hasChildNodes = (node.Content as MenuItem)?.HasChildNodes ?? false;
        return hasChildNodes;
    }

    // Load children when node expands
    private void ExecuteOnDemandLoading(object obj)
    {
        if (obj is not TreeViewNode node)
            return;

        // Prevent duplicate loading
        if (node.ChildNodes.Count > 0)
        {
            node.IsExpanded = true;
            return;
        }

        node.ShowExpanderAnimation = true;
        var menuItem = node.Content as MenuItem;

        // Simulate async loading (API call, database query, etc.)
        MainThread.BeginInvokeOnMainThread(async () =>
        {
            await Task.Delay(1500); // Simulate network delay

            var childItems = GetSubMenu(menuItem.ID);
            
            // Populate child nodes
            node.PopulateChildNodes(childItems);

            // Expand after loading
            if (childItems.Any())
                node.IsExpanded = true;

            node.ShowExpanderAnimation = false;
        });
    }

    // Get root menu items
    private ObservableCollection<MenuItem> GetMenuItems()
    {
        return new ObservableCollection<MenuItem>
        {
            new MenuItem 
            { 
                ItemName = "My Drive", 
                HasChildNodes = true, 
                ID = 0 
            },
            new MenuItem 
            { 
                ItemName = "Recent", 
                HasChildNodes = true, 
                ID = 1 
            },
            new MenuItem 
            { 
                ItemName = "Starred", 
                HasChildNodes = false, 
                ID = 2 
            }
        };
    }

    // Get child items based on parent ID
    private IEnumerable<MenuItem> GetSubMenu(int parentId)
    {
        var childItems = new ObservableCollection<MenuItem>();

        if (parentId == 0)
        {
            childItems.Add(new MenuItem { ItemName = "Documents", HasChildNodes = true, ID = 10 });
            childItems.Add(new MenuItem { ItemName = "Downloads", HasChildNodes = true, ID = 11 });
            childItems.Add(new MenuItem { ItemName = "Desktop", HasChildNodes = false, ID = 12 });
        }
        else if (parentId == 1)
        {
            childItems.Add(new MenuItem { ItemName = "Presentation.pptx", HasChildNodes = false, ID = 20 });
            childItems.Add(new MenuItem { ItemName = "Report.docx", HasChildNodes = false, ID = 21 });
        }
        else if (parentId == 10)
        {
            childItems.Add(new MenuItem { ItemName = "Project Proposal.pdf", HasChildNodes = false, ID = 100 });
            childItems.Add(new MenuItem { ItemName = "Budget.xlsx", HasChildNodes = false, ID = 101 });
        }

        return childItems;
    }

    public event PropertyChangedEventHandler PropertyChanged;

    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

#### Step 3: XAML Implementation

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:syncfusion="clr-namespace:Syncfusion.Maui.TreeView;assembly=Syncfusion.Maui.TreeView"
             xmlns:local="clr-namespace:YourNamespace"
             x:Class="YourNamespace.MainPage">

    <ContentPage.BindingContext>
        <local:MainViewModel x:Name="viewModel"/>
    </ContentPage.BindingContext>

    <Grid RowDefinitions="Auto,*" Padding="10" RowSpacing="10">
        <Label Grid.Row="0"
               Text="Load on Demand Demo"
               FontSize="24"
               FontAttributes="Bold"/>

        <syncfusion:SfTreeView Grid.Row="1"
                               x:Name="treeView"
                               LoadOnDemandCommand="{Binding TreeViewOnDemandCommand}"
                               ItemsSource="{Binding Menu}"
                               ChildPropertyName="SubMenuItems">
            
            <syncfusion:SfTreeView.ItemTemplate>
                <DataTemplate>
                    <Grid ColumnDefinitions="40,*" ColumnSpacing="10" Padding="8">
                        <Label Grid.Column="0"
                               Text="📁"
                               FontSize="24"
                               VerticalOptions="Center"/>
                        <Label Grid.Column="1"
                               Text="{Binding ItemName}"
                               FontSize="14"
                               VerticalOptions="Center"/>
                    </Grid>
                </DataTemplate>
            </syncfusion:SfTreeView.ItemTemplate>
        </syncfusion:SfTreeView>
    </Grid>
</ContentPage>
```

---

### ShowExpanderAnimation

The `ShowExpanderAnimation` property displays a loading indicator while children are being loaded.

```csharp
// Start animation before loading
node.ShowExpanderAnimation = true;

// Simulate loading...
await Task.Delay(2000);

// Load children
node.PopulateChildNodes(childItems);

// Stop animation after loading
node.ShowExpanderAnimation = false;
```

**Visual Effect:** An animated spinner appears next to the expander icon while loading.

---

### PopulateChildNodes

The `PopulateChildNodes()` method adds loaded child nodes to the parent.

#### Basic Usage

```csharp
private void ExecuteOnDemandLoading(object obj)
{
    var node = obj as TreeViewNode;
    var menuItem = node.Content as MenuItem;

    // Load children from API/database
    var childItems = await FetchChildItemsAsync(menuItem.ID);

    // Add to TreeViewNode
    node.PopulateChildNodes(childItems);

    // Expand to show children
    if (childItems.Any())
        node.IsExpanded = true;
}
```

#### With Data Conversion

```csharp
// If API returns different type than expected
var menuItems = rawData.Select(x => new MenuItem 
{ 
    ItemName = x.Name, 
    ID = x.Id, 
    HasChildNodes = x.ChildCount > 0 
}).ToList();

node.PopulateChildNodes(menuItems);
```

---

### Avoiding Duplicate Loading

Check if children are already loaded before reloading:

```csharp
private void ExecuteOnDemandLoading(object obj)
{
    var node = obj as TreeViewNode;

    // Skip if already loaded
    if (node.ChildNodes.Count > 0)
    {
        node.IsExpanded = true;
        return;
    }

    // Load children...
    LoadChildrenAsync(node);
}
```

---

### Complete Example: API-Based Load on Demand

```csharp
public class ApiMenuViewModel : INotifyPropertyChanged
{
    private HttpClient httpClient;

    public ObservableCollection<MenuItem> Menu { get; set; }
    public ICommand TreeViewOnDemandCommand { get; set; }

    public ApiMenuViewModel()
    {
        httpClient = new HttpClient();
        Menu = new ObservableCollection<MenuItem>();
        TreeViewOnDemandCommand = new Command(
            ExecuteOnDemandLoading, 
            CanExecuteOnDemandLoading);
        
        LoadRootItems();
    }

    private bool CanExecuteOnDemandLoading(object sender)
    {
        return (sender as TreeViewNode)?.Content is MenuItem item && item.HasChildNodes;
    }

    private void ExecuteOnDemandLoading(object obj)
    {
        if (obj is not TreeViewNode node)
            return;

        if (node.ChildNodes.Count > 0)
        {
            node.IsExpanded = true;
            return;
        }

        node.ShowExpanderAnimation = true;
        var menuItem = node.Content as MenuItem;

        // Load from API
        MainThread.BeginInvokeOnMainThread(async () =>
        {
            try
            {
                var childItems = await FetchChildItemsAsync(menuItem.ID);
                node.PopulateChildNodes(childItems);
                if (childItems.Any())
                    node.IsExpanded = true;
            }
            catch (Exception ex)
            {
                Debug.WriteLine($"Error loading children: {ex.Message}");
            }
            finally
            {
                node.ShowExpanderAnimation = false;
            }
        });
    }

    private async Task<IEnumerable<MenuItem>> FetchChildItemsAsync(int parentId)
    {
        try
        {
            var response = await httpClient.GetAsync("url");
            response.EnsureSuccessStatusCode();
            
            var json = await response.Content.ReadAsStringAsync();
            var items = JsonSerializer.Deserialize<List<MenuItem>>(json);
            
            return items ?? new List<MenuItem>();
        }
        catch (Exception ex)
        {
            Debug.WriteLine($"API Error: {ex.Message}");
            return new List<MenuItem>();
        }
    }

    private async void LoadRootItems()
    {
        var rootItems = await FetchChildItemsAsync(0);
        foreach (var item in rootItems)
        {
            Menu.Add(item);
        }
    }

    public event PropertyChangedEventHandler PropertyChanged;

    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

---

## CheckBox

To use checkboxes in TreeView:

1. **Add CheckBox to ItemTemplate** - Include a `SfCheckBox` control in your `ItemTemplate`
2. **Bind IsChecked** - Bind the checkbox `IsChecked` property to `TreeViewNode.IsChecked`
3. **Set ItemTemplateContextType** - Must be set to `Node` for checkbox binding
4. **Configure CheckBoxMode** - Define how checkboxes behave (Recursive, Cascade, etc.)

**Key Point:** Always set `ItemTemplateContextType="Node"` when using checkboxes.

---

### Checkbox in Bound Mode

In bound mode, use the `CheckedItems` property to work with checked items through your ViewModel.

#### Step 1: Create Data Model

```csharp
public class Folder : INotifyPropertyChanged
{
    private string folderName;
    private ObservableCollection<Folder> filesInfo;

    public string FolderName
    {
        get { return folderName; }
        set
        {
            folderName = value;
            OnPropertyChanged("FolderName");
        }
    }

    public ObservableCollection<Folder> FilesInfo
    {
        get { return filesInfo; }
        set
        {
            filesInfo = value;
            OnPropertyChanged("FilesInfo");
        }
    }

    public event PropertyChangedEventHandler PropertyChanged;

    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

#### Step 2: Create ViewModel with CheckedItems

```csharp
public class FileManagerViewModel : INotifyPropertyChanged
{
    private ObservableCollection<object> checkedItems;
    public ObservableCollection<Folder> Folders { get; set; }

    public ObservableCollection<object> CheckedItems
    {
        get { return checkedItems; }
        set
        {
            checkedItems = value;
            OnPropertyChanged("CheckedItems");
        }
    }

    public FileManagerViewModel()
    {
        this.Folders = GetFolders();
        this.CheckedItems = new ObservableCollection<object>();
    }

    private ObservableCollection<Folder> GetFolders()
    {
        var folders = new ObservableCollection<Folder>();
        
        var documents = new Folder() { FolderName = "Documents" };
        documents.FilesInfo = new ObservableCollection<Folder>
        {
            new Folder() { FolderName = "Resume.pdf" },
            new Folder() { FolderName = "CoverLetter.docx" }
        };

        folders.Add(documents);
        return folders;
    }

    public event PropertyChangedEventHandler PropertyChanged;

    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

#### Step 3: XAML with Checkbox

```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:syncfusion="clr-namespace:Syncfusion.Maui.TreeView;assembly=Syncfusion.Maui.TreeView"
             xmlns:checkbox="clr-namespace:Syncfusion.Maui.Buttons;assembly=Syncfusion.Maui.Buttons"
             xmlns:local="clr-namespace:YourNamespace"
             x:Class="YourNamespace.MainPage">

    <ContentPage.BindingContext>
        <local:FileManagerViewModel x:Name="viewModel"/>
    </ContentPage.BindingContext>

    <syncfusion:SfTreeView x:Name="treeView"
                           ItemsSource="{Binding Folders}"
                           ChildPropertyName="FilesInfo"
                           ItemTemplateContextType="Node"
                           CheckedItems="{Binding CheckedItems}"
                           CheckBoxMode="Recursive"
                           AutoExpandMode="AllNodesExpanded">
        
        <syncfusion:SfTreeView.ItemTemplate>
            <DataTemplate>
                <Grid ColumnDefinitions="40,*" ColumnSpacing="10" Padding="5">
                    <!-- Checkbox -->
                    <checkbox:SfCheckBox Grid.Column="0"
                                        VerticalOptions="Center"
                                        HorizontalOptions="Center"
                                        IsChecked="{Binding IsChecked, Mode=TwoWay}"/>
                    
                    <!-- Label -->
                    <Label Grid.Column="1"
                           Text="{Binding Content.FolderName}"
                           VerticalOptions="Center"
                           FontSize="14"/>
                </Grid>
            </DataTemplate>
        </syncfusion:SfTreeView.ItemTemplate>
    </syncfusion:SfTreeView>
</ContentPage>
```

**Important Notes:**
- `ItemTemplateContextType="Node"` enables binding to `TreeViewNode.IsChecked`
- `CheckedItems` must be an `ObservableCollection<object>`
- Two-way binding on checkbox: `IsChecked="{Binding IsChecked, Mode=TwoWay}"`

---

### Checkbox in Unbound Mode

In unbound mode, directly access and modify the `IsChecked` property of `TreeViewNode` objects.

### XAML Example

```xaml
<syncfusion:SfTreeView x:Name="treeView"
                       ItemTemplateContextType="Node">
    
    <syncfusion:SfTreeView.Nodes>
        <treeviewengine:TreeViewNode Content="Documents" IsChecked="False">
            <treeviewengine:TreeViewNode.ChildNodes>
                <treeviewengine:TreeViewNode Content="Report.pdf" IsChecked="False"/>
                <treeviewengine:TreeViewNode Content="Budget.xlsx" IsChecked="False"/>
            </treeviewengine:TreeViewNode.ChildNodes>
        </treeviewengine:TreeViewNode>
    </syncfusion:SfTreeView.Nodes>

    <syncfusion:SfTreeView.ItemTemplate>
        <DataTemplate>
            <Grid ColumnDefinitions="40,*" ColumnSpacing="10" Padding="5">
                <checkbox:SfCheckBox Grid.Column="0"
                                    IsChecked="{Binding IsChecked, Mode=TwoWay}"/>
                <Label Grid.Column="1"
                       Text="{Binding Content}"
                       VerticalOptions="Center"/>
            </Grid>
        </DataTemplate>
    </syncfusion:SfTreeView.ItemTemplate>
</syncfusion:SfTreeView>
```

#### C# Code-Behind

```csharp
// Access checked nodes programmatically
private void GetCheckedNodes()
{
    var checkedNodes = new List<TreeViewNode>();
    foreach (var node in treeView.Nodes)
    {
        CollectCheckedNodes(node, checkedNodes);
    }
}

private void CollectCheckedNodes(TreeViewNode node, List<TreeViewNode> checkedList)
{
    if (node.IsChecked)
    {
        checkedList.Add(node);
    }

    foreach (var childNode in node.ChildNodes)
    {
        CollectCheckedNodes(childNode, checkedList);
    }
}
```

---

### CheckBoxMode Property

The `CheckBoxMode` property defines how checkbox states propagate in the hierarchy.

#### Options

| Mode | Description |
|------|-------------|
| **Recursive** | Checking a parent automatically checks all children; unchecking a child propagates to parent |
| **Cascade** | Similar to Recursive but with stricter cascade behavior |
| **None** | Checkboxes work independently (default) |

#### Example: Recursive Mode

```xaml
<syncfusion:SfTreeView x:Name="treeView"
                       ItemsSource="{Binding Items}"
                       CheckBoxMode="Recursive"
                       ItemTemplateContextType="Node">
    <!-- ItemTemplate with CheckBox -->
</syncfusion:SfTreeView>
```

When a parent is checked in Recursive mode:
- ✅ All child nodes automatically become checked
- ✅ Parent shows as checked if all children are checked
- ✅ Parent shows as partially checked if some children are checked

---

### Working with Checked Items

#### Access Checked Items from ViewModel (Bound Mode)

```csharp
public class FileManagerViewModel
{
    public ObservableCollection<object> CheckedItems { get; set; }

    public void ProcessCheckedItems()
    {
        foreach (var item in CheckedItems)
        {
            if (item is Folder folder)
            {
                // Process checked folder
                Debug.WriteLine($"Checked: {folder.FolderName}");
            }
        }
    }
}
```

#### GetCheckedNodes() Method (Unbound Mode)

In unbound mode, use the `GetCheckedNodes()` method to retrieve all checked nodes.

```csharp
// Get all checked nodes
var checkedNodes = treeView.GetCheckedNodes();

foreach (var node in checkedNodes)
{
    Debug.WriteLine($"Checked Node: {node.Content}");
}
```

**Return Type:**
```csharp
// Returns: IEnumerable<TreeViewNode>
public IEnumerable<TreeViewNode> GetCheckedNodes()
```

**Key Points:**
- Returns a collection of all nodes with `IsChecked = true`
- Works only in unbound mode or when using direct `TreeViewNode` manipulation
- Useful for batch processing checked items
- Called at any time during application lifecycle

**Example: Process Checked Items**

```csharp
private void OnProcessButtonClicked(object sender, EventArgs e)
{
    var checkedNodes = treeView.GetCheckedNodes();
    
    if (!checkedNodes.Any())
    {
        DisplayAlert("Info", "No items selected", "OK");
        return;
    }

    var selectedContent = new List<string>();
    foreach (var node in checkedNodes)
    {
        selectedContent.Add(node.Content?.ToString() ?? "Unknown");
    }

    var result = string.Join(", ", selectedContent);
    DisplayAlert("Selected Items", result, "OK");
}
```

#### Programmatically Set Checked State

```csharp
// Check all items
private void CheckAllItems()
{
    foreach (var node in treeView.Nodes)
    {
        SetCheckedRecursively(node, true);
    }
}

private void SetCheckedRecursively(TreeViewNode node, bool isChecked)
{
    node.IsChecked = isChecked;
    foreach (var childNode in node.ChildNodes)
    {
        SetCheckedRecursively(childNode, isChecked);
    }
}
```

---

### IsChecked Property

The `IsChecked` property of `TreeViewNode` represents the checkbox state.

#### Binding in XAML

```xaml
<checkbox:SfCheckBox IsChecked="{Binding IsChecked, Mode=TwoWay}"/>
```

#### Property States

```csharp
public bool? IsChecked { get; set; }

// Three-state checkbox (if supported):
// true  = Checked
// false = Unchecked
// null  = Indeterminate (partial selection)
```

#### Programmatically Setting Checked State

```csharp
// Set single node as checked
treeView.Nodes[0].IsChecked = true;

// Check all nodes
foreach (var node in treeView.Nodes)
{
    node.IsChecked = true;
}
```

---

### NodeChecked Event

The `NodeChecked` event is raised whenever a node's checkbox is checked or unchecked during user interaction or programmatic changes.

#### Event Registration

```csharp
treeView.NodeChecked += OnTreeViewNodeChecked;

private void OnTreeViewNodeChecked(object sender, NodeCheckedEventArgs e)
{
    var checkedNode = e.Node;
    Debug.WriteLine($"Node Checked: {checkedNode.Content}");
}
```

#### NodeCheckedEventArgs Properties

The `NodeCheckedEventArgs` provides the following information:

| Property | Type | Description |
|----------|------|-------------|
| `Node` | `TreeViewNode` | The node that was checked/unchecked |
| `Node.Content` | `object` | The data associated with the node |
| `Node.IsChecked` | `bool?` | Current checked state |

#### Complete Event Handler Example

```csharp
private void OnTreeViewNodeChecked(object sender, NodeCheckedEventArgs e)
{
    var node = e.Node;
    
    // Get the data associated with the node
    var nodeData = node.Content as Folder;
    
    if (node.IsChecked == true)
    {
        Debug.WriteLine($"Checked: {nodeData?.FolderName}");
    }
    else if (node.IsChecked == false)
    {
        Debug.WriteLine($"Unchecked: {nodeData?.FolderName}");
    }
}
```

#### Event Behavior

**Important Notes:**
- ✅ Event fires for both check and uncheck operations
- ✅ Event only fires on **UI interactions** (not programmatic changes)
- ✅ When `CheckBoxMode` is enabled, `ItemTapped` and `ItemDoubleTapped` events are **not triggered**
- ❌ Programmatic calls like `node.IsChecked = true` do NOT raise this event

#### Example: Validate Before Checking

```csharp
private void OnTreeViewNodeChecked(object sender, NodeCheckedEventArgs e)
{
    var node = e.Node;
    
    // Example: Don't allow checking more than 5 items
    var checkedCount = treeView.GetCheckedNodes().Count();
    
    if (checkedCount > 5 && node.IsChecked == true)
    {
        DisplayAlert("Limit", "Cannot select more than 5 items", "OK");
        node.IsChecked = false; // Uncheck it
    }
}
```

#### Example: Track Checkbox Changes

```csharp
private List<string> checkedItems = new List<string>();

private void OnTreeViewNodeChecked(object sender, NodeCheckedEventArgs e)
{
    var node = e.Node;
    var content = node.Content?.ToString() ?? "Unknown";
    
    if (node.IsChecked == true)
    {
        if (!checkedItems.Contains(content))
        {
            checkedItems.Add(content);
        }
    }
    else
    {
        checkedItems.Remove(content);
    }
    
    Debug.WriteLine($"Total Checked: {checkedItems.Count}");
}
```

---

### Comparison: Getting Checked Items

| Method | Mode | Use Case |
|--------|------|----------|
| `CheckedItems` collection | Bound Mode | Data binding with MVVM |
| `GetCheckedNodes()` | Unbound Mode | Manual node creation |
| `NodeChecked` event | Both | Real-time checkbox changes |
| Direct `IsChecked` property | Both | Individual node manipulation |

---

## EmptyView

The `EmptyView` property displays content when:

- `ItemsSource` is empty or null (bound mode)
- `Nodes` collection is empty (unbound mode)

You can display either:
1. **String message** - Simple text
2. **Custom view** - Complex UI elements
3. **Template** - Data-driven custom appearance with `EmptyViewTemplate`

---

### Display String Message

The simplest way to show empty state is with a string message.

#### XAML

```xaml
<syncfusion:SfTreeView x:Name="treeView"
                       ItemsSource="{Binding Items}"
                       EmptyView="No items available">
</syncfusion:SfTreeView>
```

#### C#

```csharp
var treeView = new SfTreeView();
treeView.ItemsSource = viewModel.Items;
treeView.EmptyView = "No items available";
```

#### Result
When `ItemsSource` is empty, the TreeView displays:
```
No items available
```

---

### Display Custom View

Display complex UI elements when the TreeView has no data.

#### Custom Border with Label

```xaml
<syncfusion:SfTreeView x:Name="treeView"
                       ItemsSource="{Binding Items}">
    <syncfusion:SfTreeView.EmptyView>
        <Border Padding="20" 
                Stroke="LightGray" 
                StrokeThickness="2" 
                HorizontalOptions="Center" 
                VerticalOptions="Center">
            <Border.StrokeShape>
                <RoundRectangle CornerRadius="10"/>
            </Border.StrokeShape>
            
            <VerticalStackLayout Spacing="10" 
                                HorizontalOptions="Center" 
                                VerticalOptions="Center">
                <Label Text="📭" 
                       FontSize="48" 
                       HorizontalOptions="Center"/>
                <Label Text="No Items Found" 
                       FontSize="16" 
                       FontAttributes="Bold" 
                       TextColor="DarkGray"/>
                <Label Text="Try adding new items" 
                       FontSize="12" 
                       TextColor="Gray"/>
            </VerticalStackLayout>
        </Border>
    </syncfusion:SfTreeView.EmptyView>
</syncfusion:SfTreeView>
```

#### Custom View with Button

```xaml
<syncfusion:SfTreeView x:Name="treeView"
                       ItemsSource="{Binding Items}">
    <syncfusion:SfTreeView.EmptyView>
        <Grid RowDefinitions="*,Auto" 
              Padding="20" 
              RowSpacing="20">
            
            <Label Grid.Row="0"
                   Text="No data to display"
                   FontSize="18"
                   FontAttributes="Bold"
                   HorizontalOptions="Center"
                   VerticalOptions="Center"/>
            
            <Button Grid.Row="1"
                    Text="Load Data"
                    Command="{Binding LoadDataCommand}"
                    BackgroundColor="#2196F3"
                    TextColor="White"
                    Padding="20,10"
                    CornerRadius="5"/>
        </Grid>
    </syncfusion:SfTreeView.EmptyView>
</syncfusion:SfTreeView>
```

#### C# Code-Behind

```csharp
var treeView = new SfTreeView();
treeView.ItemsSource = viewModel.Items;

var emptyViewContent = new VerticalStackLayout
{
    Spacing = 10,
    HorizontalOptions = LayoutOptions.Center,
    VerticalOptions = LayoutOptions.Center,
    Children =
    {
        new Label { Text = "📭", FontSize = 48, HorizontalOptions = LayoutOptions.Center },
        new Label { Text = "No Items Found", FontSize = 16, FontAttributes = FontAttributes.Bold },
        new Label { Text = "Try adding new items", FontSize = 12, TextColor = Colors.Gray }
    }
};

treeView.EmptyView = emptyViewContent;
```

---

### EmptyViewTemplate

Use `EmptyViewTemplate` to customize the appearance of `EmptyView` with data binding.

#### When to Use

- Display dynamic content based on ViewModel data
- Complex templating with bindings
- Conditional formatting in empty state

#### Setup with Template

```xaml
<syncfusion:SfTreeView x:Name="treeView"
                       ItemsSource="{Binding Items}"
                       NotificationSubscriptionMode="CollectionChange">
    
    <!-- EmptyView with data binding -->
    <syncfusion:SfTreeView.EmptyView>
        <local:EmptyStateModel Message="{Binding EmptyMessage}"/>
    </syncfusion:SfTreeView.EmptyView>
    
    <!-- Template for EmptyView appearance -->
    <syncfusion:SfTreeView.EmptyViewTemplate>
        <DataTemplate>
            <Border Padding="20"
                    Stroke="Purple"
                    StrokeThickness="2"
                    HorizontalOptions="Center"
                    VerticalOptions="Center">
                <Border.StrokeShape>
                    <RoundRectangle CornerRadius="6"/>
                </Border.StrokeShape>
                
                <VerticalStackLayout Spacing="15" HorizontalOptions="Center">
                    <Label Text="⚠️" 
                           FontSize="40" 
                           HorizontalOptions="Center"/>
                    <Label Text="Empty State" 
                           FontSize="16" 
                           FontAttributes="Bold" 
                           TextColor="Blue"/>
                    <Label Text="{Binding Message}" 
                           FontSize="12" 
                           TextColor="DarkGray"
                           HorizontalTextAlignment="Center"/>
                </VerticalStackLayout>
            </Border>
        </DataTemplate>
    </syncfusion:SfTreeView.EmptyViewTemplate>
</syncfusion:SfTreeView>
```

#### EmptyStateModel Class

```csharp
public class EmptyStateModel
{
    public string Message { get; set; }
}
```

#### ViewModel Implementation

```csharp
public class FileManagerViewModel : INotifyPropertyChanged
{
    private string emptyMessage;
    
    public string EmptyMessage
    {
        get { return emptyMessage; }
        set
        {
            emptyMessage = value;
            OnPropertyChanged("EmptyMessage");
        }
    }

    public ObservableCollection<Folder> Items { get; set; }

    public FileManagerViewModel()
    {
        Items = new ObservableCollection<Folder>();
        EmptyMessage = "No folders available. Create one to get started!";
    }

    public event PropertyChangedEventHandler PropertyChanged;

    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

---

### Trigger Conditions

#### Empty ItemsSource (Bound Mode)

```csharp
// When ItemsSource is empty or null
viewModel.Items.Clear();  // Triggers EmptyView

// When ItemsSource is null
treeView.ItemsSource = null;  // Triggers EmptyView
```

#### Empty Nodes Collection (Unbound Mode)

```csharp
// When Nodes collection is empty
treeView.Nodes.Clear();  // Triggers EmptyView
```

#### Filtering Results

```csharp
public void FilterItems(string searchTerm)
{
    var filtered = Items.Where(x => x.Name.Contains(searchTerm)).ToList();
    
    FilteredItems.Clear();
    foreach (var item in filtered)
    {
        FilteredItems.Add(item);
    }
    
    // If no results, EmptyView is shown
}
```

---

### Complete Example: Search with Empty State

```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:syncfusion="clr-namespace:Syncfusion.Maui.TreeView;assembly=Syncfusion.Maui.TreeView"
             xmlns:local="clr-namespace:YourNamespace"
             x:Class="YourNamespace.MainPage">
    
    <ContentPage.BindingContext>
        <local:FileManagerViewModel/>
    </ContentPage.BindingContext>

    <Grid RowDefinitions="Auto,*" Padding="10" RowSpacing="10">
        
        <!-- Search Bar -->
        <SearchBar x:Name="searchBar"
                   Placeholder="Search items..."
                   TextChanged="OnSearchTextChanged"
                   Grid.Row="0"/>
        
        <!-- TreeView with EmptyView -->
        <syncfusion:SfTreeView x:Name="treeView"
                               Grid.Row="1"
                               ItemsSource="{Binding FilteredItems}"
                               ChildPropertyName="SubItems"
                               NotificationSubscriptionMode="CollectionChange"
                               AutoExpandMode="AllNodesExpanded">
            
            <syncfusion:SfTreeView.EmptyView>
                <VerticalStackLayout Spacing="10" 
                                    HorizontalOptions="Center" 
                                    VerticalOptions="Center" 
                                    Padding="20">
                    <Label Text="🔍" FontSize="40" HorizontalOptions="Center"/>
                    <Label Text="No Results" 
                           FontSize="16" 
                           FontAttributes="Bold"
                           HorizontalOptions="Center"/>
                    <Label Text="Try a different search term" 
                           FontSize="12" 
                           TextColor="Gray"
                           HorizontalOptions="Center"/>
                </VerticalStackLayout>
            </syncfusion:SfTreeView.EmptyView>
            
            <syncfusion:SfTreeView.ItemTemplate>
                <DataTemplate>
                    <Label Text="{Binding Name}" 
                           Padding="10,5"
                           FontSize="14"/>
                </DataTemplate>
            </syncfusion:SfTreeView.ItemTemplate>
        </syncfusion:SfTreeView>
    </Grid>
</ContentPage>
```

#### Code-Behind

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
    }

    private void OnSearchTextChanged(object sender, TextChangedEventArgs e)
    {
        var viewModel = (FileManagerViewModel)BindingContext;
        viewModel.FilterItems(e.NewTextValue);
    }
}
```

---

## Best Practices

1. **Use LoadOnDemand** for large hierarchical datasets
2. **Leverage checkboxes** for multi-selection scenarios
3. **Implement empty views** for better UX
4. **Use events judiciously** - prefer commands in MVVM
5. **Call ResetTreeViewItems** after changing ItemsSource

### ✅ Do's

1. **Check `HasChildNodes` before loading**
   ```csharp
   if (!item.HasChildNodes) return;
   ```

2. **Use `ShowExpanderAnimation`** while loading
   ```csharp
   node.ShowExpanderAnimation = true;
   ```

3. **Check existing children** to avoid duplicate loading
   ```csharp
   if (node.ChildNodes.Count > 0) return;
   ```

4. **Handle errors gracefully**
   ```csharp
   try { ... } 
   catch (Exception ex) 
   { 
       Debug.WriteLine(ex.Message); 
   }
   ```

5. **Use async/await** for I/O operations
   ```csharp
   await FetchDataAsync();

6. **Always use `ItemTemplateContextType="Node"`** when working with checkboxes
   ```xaml
   <syncfusion:SfTreeView ItemTemplateContextType="Node">
   ```

7. **Use `CheckedItems` collection** in bound mode for MVVM
   ```csharp
   CheckedItems="{Binding CheckedItems}"
   ```

8. **Use `GetCheckedNodes()`** in unbound mode to retrieve checked items
   ```csharp
   var checkedNodes = treeView.GetCheckedNodes();
   ```

9. **Subscribe to `NodeChecked` event** for real-time checkbox changes
   ```csharp
   treeView.NodeChecked += OnTreeViewNodeChecked;
   ```

10. **Implement `INotifyPropertyChanged`** in your data models
   ```csharp
   public event PropertyChangedEventHandler PropertyChanged;
   ```

11. **Use `ObservableCollection`** for dynamic updates
   ```csharp
   public ObservableCollection<object> CheckedItems { get; set; }
   ```

12. **Bind with TwoWay mode** for checkbox updates
   ```xaml
   IsChecked="{Binding IsChecked, Mode=TwoWay}"
   ```

13. **Always provide an EmptyView** for better UX
   ```xaml
   <syncfusion:SfTreeView EmptyView="No data available"/>
   ```

14. **Use EmptyViewTemplate** for dynamic empty states
   ```xaml
   <syncfusion:SfTreeView.EmptyViewTemplate>
       <DataTemplate>...</DataTemplate>
   </syncfusion:SfTreeView.EmptyViewTemplate>
   ```

15. **Set `NotificationSubscriptionMode="CollectionChange"`** when content might change
   ```xaml
   <syncfusion:SfTreeView NotificationSubscriptionMode="CollectionChange"/>
   ```

16. **Make empty view visually distinct** with icons or styling
   ```xaml
   <Label Text="📭" FontSize="48"/>
   ```

17. **Include actionable content** like "Add New" button if applicable

#### ❌ Don'ts

1. **Don't load on every expand**
   - Cache results after first load

2. **Don't block UI during loading**
   - Use `MainThread.BeginInvokeOnMainThread()`

3. **Don't forget to stop animation**
   - Always set `ShowExpanderAnimation = false`

4. **Don't set very large delays** in simulation
   - Real APIs should respond reasonably

5. **Don't forget to set `ItemTemplateContextType="Node"`**
   - Without this, `IsChecked` binding will fail

6. **Don't use `CheckedItems` in unbound mode**
   - Use `GetCheckedNodes()` method instead

7. **Don't expect `NodeChecked` event for programmatic changes**
   - Event only fires on UI interactions
   - For programmatic checks, use `NodeChecked` event only for UI-initiated actions

8. **Don't mix checkbox with `SelectionMode`**
   - Choose either checkbox or selection, not both

9. **Don't perform heavy operations in `NodeChecked` event**
   - Use async/await for performance-intensive tasks

10. **Don't forget to unsubscribe from events** in cleanup
   ```csharp
   treeView.NodeChecked -= OnTreeViewNodeChecked;
   ```

11. **Don't set EmptyView to null**
   - Always provide meaningful empty state

12. **Don't ignore `NotificationSubscriptionMode`**
   - Updates might not reflect in EmptyView

13. **Don't make empty view too large**
   - Keep it centered and moderate size

14. **Don't use complex templates for empty view**
   - Keep it simple and fast to render

---

### Performance Tips

1. **Disable animations during heavy drag operations**
2. **Limit auto-expand delay** to improve responsiveness
3. **Use `Handled` property** in `ItemDragging` for custom drag logic
4. **Batch load multiple levels** when possible
5. **Implement caching** to avoid reloading
6. **Limit initial nodes** to prevent large memory usage
7. **Use `NodePopulationMode.OnDemand`** (default for load on demand)
8. **Monitor for memory leaks** with many loaded nodes

#### User Experience

1. **Provide visual feedback** using custom `DragItemTemplate`
2. **Show drop indicators** clearly
3. **Validate drops** in `Dropping` action, not `Drop`
4. **Use auto-expand** for deep hierarchies

#### Example: Complete Drag Implementation

```csharp
public class DragDropViewModel
{
    public void SetupDragDrop(SfTreeView treeView)
    {
        treeView.AllowDragging = true;
        treeView.DragAndDropController.CanAutoExpand = true;
        treeView.DragAndDropController.AutoExpandDelay = TimeSpan.FromSeconds(1);
        treeView.AutoScroller.ScrollMargin = 20;
        
        treeView.ItemDragging += OnItemDragging;
    }
    
    private void OnItemDragging(object sender, ItemDraggingEventArgs e)
    {
        switch (e.Action)
        {
            case DragAction.Start:
                ValidateDragStart(e);
                break;
                
            case DragAction.Dropping:
                ValidateDrop(e);
                break;
                
            case DragAction.Drop:
                HandleDrop(e);
                break;
        }
    }
    
    private void ValidateDragStart(ItemDraggingEventArgs e)
    {
        var item = e.DraggingNodes[0].Content as FileItem;
        if (item.IsLocked)
        {
            e.Cancel = true;
        }
    }
    
    private void ValidateDrop(ItemDraggingEventArgs e)
    {
        // Add validation logic
    }
    
    private void HandleDrop(ItemDraggingEventArgs e)
    {
        // Update data model
        // Notify UI
    }
}
```

---

## Common Issues and Troubleshooting

### Issue: Children not appearing after load
**Solution:** Call `node.PopulateChildNodes()` with correct data

### Issue: Animation never stops
**Solution:** Set `ShowExpanderAnimation = false` in finally block

### Issue: Infinite loading
**Solution:** Check condition in `CanExecuteOnDemandLoading`

### Issue: CheckBox not showing
**Solution:** Add `ItemTemplateContextType="Node"` to TreeView
```xaml
<syncfusion:SfTreeView ItemTemplateContextType="Node">
```

### Issue: Checked state not updating
**Solution:** Use two-way binding: `IsChecked="{Binding IsChecked, Mode=TwoWay}"`
```xaml
<checkbox:SfCheckBox IsChecked="{Binding IsChecked, Mode=TwoWay}"/>
```

### Issue: CheckedItems collection is empty
**Solution:** Ensure `CheckBoxMode` is set appropriately and TreeView is bound correctly

### Issue: GetCheckedNodes() returns empty collection
**Cause:** In bound mode, `GetCheckedNodes()` won't work  
**Solution:** Use `CheckedItems` collection instead
```csharp
// ✅ Correct for unbound mode
var checkedNodes = treeView.GetCheckedNodes();

// ✅ Correct for bound mode
var checkedItems = treeView.CheckedItems;
```

### Issue: NodeChecked event not firing
**Cause:** Programmatic checkbox changes don't trigger the event  
**Solution:** Handle UI-initiated changes only or manually call a method for programmatic changes
```csharp
// NodeChecked event fires for this:
treeView.NodeChecked += OnNodeChecked; // User clicks checkbox

// NodeChecked event does NOT fire for this:
treeView.Nodes[0].IsChecked = true; // Programmatic change
```

### Issue: CheckBoxMode="Recursive" not working properly
**Cause:** CheckBoxMode needs to be set on TreeView declaration  
**Solution:** Ensure `CheckBoxMode` is set before binding data
```xaml
<syncfusion:SfTreeView ItemsSource="{Binding Items}"
                       CheckBoxMode="Recursive">
</syncfusion:SfTreeView>
```

### Issue: Too many NodeChecked events firing
**Cause:** Cascading checks in Recursive mode trigger multiple events  
**Solution:** Use flag to prevent recursive handling
```csharp
private bool isUpdatingCheckState = false;

private void OnNodeChecked(object sender, NodeCheckedEventArgs e)
{
    if (isUpdatingCheckState)
        return;

    try
    {
        isUpdatingCheckState = true;
        // Handle the event
    }
    finally
    {
        isUpdatingCheckState = false;
    }
}
```

### Issue: EmptyView Not Showing
**Solution:** Ensure `NotificationSubscriptionMode="CollectionChange"` is set

### Issue: EmptyView Shows When Items Exist
**Solution:** Check if `ItemsSource` binding is correct

### Issue: Template Bindings Not Working
**Solution:** Verify the data model passed to `EmptyView` is correct

---

## Related Topics

- [Selection](selection.md) - Multi-select for drag
- [Events](advanced-features.md#events) - Additional event handling

---