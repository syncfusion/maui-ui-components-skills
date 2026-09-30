# TreeView Checkbox Support

Comprehensive guide for implementing and customizing TreeView checkboxes. The TreeView provides built-in checkbox support for each node, enabling users to check or uncheck corresponding nodes with various modes and customization options.

## Table of Contents

- [Overview](#overview)
- [Checkbox State and Modes](#checkbox-state-and-modes)
- [Checkbox Configuration](#checkbox-configuration)
- [Checked Items in Bound Mode](#checked-items-in-bound-mode)
- [Checked Items in Unbound Mode](#checked-items-in-unbound-mode)
- [Custom Checkbox Templates](#custom-checkbox-templates)
- [Events and Commands](#events-and-commands)
- [Best Practices](#best-practices)

---

## Overview

Enable built-in checkbox support for tree view nodes.

**Key Namespaces:**
- `Syncfusion.Maui.TreeView` — TreeView control
- `Syncfusion.TreeView.Engine` — TreeViewNode class
- `Syncfusion.Maui.Buttons` — SfCheckBox control (for custom templates)

---

## Checkbox State and Modes

### CheckBoxMode Property

Configure parent and child checkbox behavior.

- **None** (Default): Checkboxes are not displayed. Checking and unchecking are not tracked in `CheckedItems`.
- **Individual**: Checkbox state of a node affects only that node; parent and child nodes are not affected.
- **Recursive**: Checking/unchecking a node affects parent and child nodes accordingly. Parent shows indeterminate state if only some children are checked.

#### CheckBoxMode: Individual

```xaml
<syncfusion:SfTreeView x:Name="treeView"
                       CheckBoxMode="Individual"
                       ItemsSource="{Binding Folders}"/>
```

#### CheckBoxMode: Recursive

```xaml
<syncfusion:SfTreeView x:Name="treeView"
                       CheckBoxMode="Recursive"
                       ItemsSource="{Binding Folders}"
                       AutoExpandMode="AllNodesExpanded"
                       NodePopulationMode="Instant"/>
```

> **Note:** Set `NodePopulationMode` to `Instant` and `CheckBoxMode` to `Recursive` to support recursive checking programmatically through `CheckedItems`.

> **Important:** When `CheckBoxMode` is enabled, the `ItemTapped` and `ItemDoubleTapped` events will NOT be triggered. Only the `NodeChecked` event is triggered.

---

## Checkbox Configuration

### CheckBoxWidth Property

Reserve space for checkboxes in tree view items.

- **Default value:** 42 pixels
- **Set to 0:** Hides the built-in checkbox (useful for custom checkbox templates)

```xaml
<syncfusion:SfTreeView x:Name="treeView"
                       CheckBoxMode="Recursive"
                       CheckBoxWidth="50"/>
```

### CheckBoxPosition Property

Set the position of checkboxes.

- **Start** (Default): Checkbox appears at the beginning of the item
- **End**: Checkbox appears at the end of the item

```xaml
<syncfusion:SfTreeView x:Name="treeView"
                       CheckBoxMode="Recursive"
                       CheckBoxPosition="End"/>
```

### CheckActionTarget Property

Choose how nodes are checked.

- **CheckBox** (Default): Only tapping the checkbox checks/unchecks the node
- **Node**: Tapping either the checkbox or the node content checks/unchecks the node

```xaml
<syncfusion:SfTreeView x:Name="treeView"
                       CheckBoxMode="Recursive"
                       CheckActionTarget="Node"/>
---

## Checked Items in Bound Mode

Track checked items in bound mode.

### Basic Checkbox Setup in Bound Mode

#### Step 1: Create Data Model

```csharp
public class Folder : INotifyPropertyChanged
{
    private string folderName;
    private ImageSource imageIcon;
    private ObservableCollection<Folder> subFolders;

    public string FolderName
    {
        get { return folderName; }
        set
        {
            if (folderName != value)
            {
                folderName = value;
                OnPropertyChanged(nameof(FolderName));
            }
        }
    }

    public ImageSource ImageIcon
    {
        get { return imageIcon; }
        set
        {
            if (imageIcon != value)
            {
                imageIcon = value;
                OnPropertyChanged(nameof(ImageIcon));
            }
        }
    }

    public ObservableCollection<Folder> SubFolders
    {
        get { return subFolders; }
        set
        {
            if (subFolders != value)
            {
                subFolders = value;
                OnPropertyChanged(nameof(SubFolders));
            }
        }
    }

    public event PropertyChangedEventHandler PropertyChanged;

    protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

#### Step 2: Create ViewModel with CheckedItems

```csharp
public class FileManagerViewModel : INotifyPropertyChanged
{
    private ObservableCollection<Folder> folders;
    private ObservableCollection<object> checkedItems;

    public ObservableCollection<Folder> Folders
    {
        get { return folders; }
        set
        {
            if (folders != value)
            {
                folders = value;
                OnPropertyChanged(nameof(Folders));
            }
        }
    }

    public ObservableCollection<object> CheckedItems
    {
        get { return checkedItems; }
        set
        {
            if (checkedItems != value)
            {
                checkedItems = value;
                OnPropertyChanged(nameof(CheckedItems));
            }
        }
    }

    public FileManagerViewModel()
    {
        CheckedItems = new ObservableCollection<object>();
        Folders = GetFolders();
        
        // Pre-check specific items
        CheckedItems.Add(Folders[0]);
        CheckedItems.Add(Folders[1].SubFolders[0]);
    }

    private ObservableCollection<Folder> GetFolders()
    {
        var folders = new ObservableCollection<Folder>
        {
            new Folder
            {
                FolderName = "Documents",
                ImageIcon = "documents.png",
                SubFolders = new ObservableCollection<Folder>
                {
                    new Folder { FolderName = "Project Files", ImageIcon = "folder.png" },
                    new Folder { FolderName = "Reports", ImageIcon = "folder.png" }
                }
            },
            new Folder
            {
                FolderName = "Photos",
                ImageIcon = "photos.png",
                SubFolders = new ObservableCollection<Folder>
                {
                    new Folder { FolderName = "Vacation", ImageIcon = "folder.png" },
                    new Folder { FolderName = "Family", ImageIcon = "folder.png" }
                }
            }
        };
        return folders;
    }

    public event PropertyChangedEventHandler PropertyChanged;

    protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

#### Step 3: XAML with CheckedItems Binding

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:syncfusion="clr-namespace:Syncfusion.Maui.TreeView;assembly=Syncfusion.Maui.TreeView"
             x:Class="TreeViewCheckboxSample.MainPage">

    <ContentPage.BindingContext>
        <local:FileManagerViewModel/>
    </ContentPage.BindingContext>

    <VerticalStackLayout Padding="10" Spacing="10">
        
        <Label Text="Select Files and Folders"
               FontSize="18"
               FontAttributes="Bold"/>

        <syncfusion:SfTreeView x:Name="treeView"
                               ItemsSource="{Binding Folders}"
                               CheckBoxMode="Recursive"
                               CheckedItems="{Binding CheckedItems}"
                               NodePopulationMode="Instant"
                               AutoExpandMode="AllNodesExpanded">
            
            <syncfusion:SfTreeView.ItemTemplate>
                <DataTemplate>
                    <!-- Your content -->
                </DataTemplate>
            </syncfusion:SfTreeView.ItemTemplate>

            <syncfusion:SfTreeView.ChildPropertyName>
                <x:String>SubFolders</x:String>
            </syncfusion:SfTreeView.ChildPropertyName>
        </syncfusion:SfTreeView>
    </VerticalStackLayout>
</ContentPage>
```

### Programmatic Checkbox Control in Bound Mode

Check or uncheck nodes programmatically.

```csharp
// Add items to CheckedItems (checks them)
treeView.CheckedItems.Add(viewModel.Folders[0]);
treeView.CheckedItems.Add(viewModel.Folders[1].SubFolders[0]);

// Remove items from CheckedItems (unchecks them)
treeView.CheckedItems.Remove(viewModel.Folders[0]);

// Clear all checked items
treeView.CheckedItems.Clear();

// Access checked items
foreach (var item in treeView.CheckedItems)
{
    var folder = item as Folder;
    // Process checked folder
}
```

> **Note:** When `NodePopulationMode` is `Instant` and `CheckBoxMode` is `Recursive`, adding items to `CheckedItems` propagates the checkbox state to parent and child nodes. Otherwise, programmatic changes do not affect parent/child checkbox states.

---

## Checked Items in Unbound Mode

Set the checked state of nodes in unbound mode.

### Basic Unbound Mode Setup

```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:syncfusion="clr-namespace:Syncfusion.Maui.TreeView;assembly=Syncfusion.Maui.TreeView"
             xmlns:treeviewengine="clr-namespace:Syncfusion.TreeView.Engine;assembly=Syncfusion.TreeView.Engine"
             x:Class="TreeViewCheckboxSample.MainPage">

    <syncfusion:SfTreeView x:Name="treeView" 
                           CheckBoxMode="Recursive"
                           SelectionMode="None">

        <syncfusion:SfTreeView.Nodes>
            <treeviewengine:TreeViewNode Content="Desktop" IsExpanded="True">
                <treeviewengine:TreeViewNode.ChildNodes>
                    <treeviewengine:TreeViewNode Content="Projects" />
                    <treeviewengine:TreeViewNode Content="Downloads" />
                    <treeviewengine:TreeViewNode Content="Screenshots" IsChecked="True" />
                </treeviewengine:TreeViewNode.ChildNodes>
            </treeviewengine:TreeViewNode>
            
            <treeviewengine:TreeViewNode Content="Documents" IsExpanded="True">
                <treeviewengine:TreeViewNode.ChildNodes>
                    <treeviewengine:TreeViewNode Content="Work" IsChecked="True" />
                    <treeviewengine:TreeViewNode Content="Personal" />
                    <treeviewengine:TreeViewNode Content="Archive" />
                </treeviewengine:TreeViewNode.ChildNodes>
            </treeviewengine:TreeViewNode>
        </syncfusion:SfTreeView.Nodes>

    </syncfusion:SfTreeView>
</ContentPage>
```

### Get Checked Nodes in Unbound Mode

```csharp
// Get all checked nodes
var checkedNodes = treeView.GetCheckedNodes();

// Process checked nodes
foreach (var node in checkedNodes)
{
    var content = node.Content as string;
    // Use the checked node content
}

// Set checkbox state programmatically
var node = treeView.Nodes[0].ChildNodes[2];
node.IsChecked = true;

// Collect checked nodes recursively
private List<TreeViewNode> CollectCheckedNodes()
{
    var checkedList = new List<TreeViewNode>();
    foreach (var node in treeView.Nodes)
    {
        CollectCheckedNodesRecursive(node, checkedList);
    }
    return checkedList;
}

private void CollectCheckedNodesRecursive(TreeViewNode node, List<TreeViewNode> checkedList)
{
    if (node.IsChecked)
    {
        checkedList.Add(node);
    }

    foreach (var childNode in node.ChildNodes)
    {
        CollectCheckedNodesRecursive(childNode, checkedList);
    }
}
```

> **Note:** The `TreeViewNode` class is in the `Syncfusion.TreeView.Engine` namespace. Add `using Syncfusion.TreeView.Engine;` to access it.

---

## Custom Checkbox Templates

Display a custom checkbox instead of the built-in checkbox.

### Prerequisites

Add the Syncfusion.Maui.Buttons namespace to use `SfCheckBox`:

```xaml
xmlns:checkbox="clr-namespace:Syncfusion.Maui.Buttons;assembly=Syncfusion.Maui.Buttons"
```

### Custom Checkbox in Bound Mode

```xaml
<syncfusion:SfTreeView x:Name="treeView"
                       ItemsSource="{Binding Folders}"
                       AutoExpandMode="AllNodesExpanded"
                       CheckBoxMode="Recursive"
                       CheckBoxWidth="0"
                       ItemTemplateContextType="Node"
                       CheckedItems="{Binding CheckedItems}"
                       NodePopulationMode="Instant"
                       SelectionMode="None">

    <syncfusion:SfTreeView.ItemTemplate>
        <DataTemplate>
            <Grid Padding="5">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="40" />
                    <ColumnDefinition Width="40" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>

                <!-- Custom Checkbox -->
                <Grid Grid.Column="0" VerticalOptions="Center" HorizontalOptions="Center">
                    <checkbox:SfCheckBox IsChecked="{Binding IsChecked, Mode=TwoWay}"/>
                </Grid>

                <!-- Folder Icon -->
                <Grid Grid.Column="1" VerticalOptions="Center" HorizontalOptions="Center">
                    <Image Source="{Binding Content.ImageIcon}"
                           HeightRequest="24"
                           WidthRequest="24"/>
                </Grid>

                <!-- Folder Name -->
                <Grid Grid.Column="2" VerticalOptions="Center" Padding="5,0,0,0">
                    <Label Text="{Binding Content.FolderName}"
                           FontSize="14"
                           VerticalTextAlignment="Center"/>
                </Grid>
            </Grid>
        </DataTemplate>
    </syncfusion:SfTreeView.ItemTemplate>

    <syncfusion:SfTreeView.ChildPropertyName>
        <x:String>SubFolders</x:String>
    </syncfusion:SfTreeView.ChildPropertyName>
</syncfusion:SfTreeView>
```

> **Important:** Set `ItemTemplateContextType="Node"` to bind to `TreeViewNode.IsChecked`. When context is `Node`, the underlying data is accessible through the `Content` property (e.g., `Content.FolderName`).

### Custom Checkbox in Unbound Mode

```xaml
<syncfusion:SfTreeView x:Name="treeView"
                       CheckBoxMode="Recursive"
                       CheckBoxWidth="0"
                       SelectionMode="None">

    <syncfusion:SfTreeView.Nodes>
        <treeviewengine:TreeViewNode Content="Documents" IsExpanded="True" IsChecked="True">
            <treeviewengine:TreeViewNode.ChildNodes>
                <treeviewengine:TreeViewNode Content="Work" IsChecked="True" />
                <treeviewengine:TreeViewNode Content="Personal" />
            </treeviewengine:TreeViewNode.ChildNodes>
        </treeviewengine:TreeViewNode>
    </syncfusion:SfTreeView.Nodes>

    <syncfusion:SfTreeView.ItemTemplate>
        <DataTemplate>
            <Grid Padding="5">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="40" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>

                <!-- Custom Checkbox bound to TreeViewNode.IsChecked -->
                <Grid Grid.Column="0" VerticalOptions="Center" HorizontalOptions="Center">
                    <checkbox:SfCheckBox IsChecked="{Binding IsChecked, Mode=TwoWay}"/>
                </Grid>

                <!-- Content -->
                <Grid Grid.Column="1" VerticalOptions="Center" Padding="5,0,0,0">
                    <Label Text="{Binding Content}" FontSize="14"/>
                </Grid>
            </Grid>
        </DataTemplate>
    </syncfusion:SfTreeView.ItemTemplate>
</syncfusion:SfTreeView>
```

> **Note:** In unbound mode, the binding context of `ItemTemplate` is `TreeViewNode` by default, so `ItemTemplateContextType` is not required.

### Advanced Custom Checkbox with Visual States

```xaml
<syncfusion:SfTreeView x:Name="treeView"
                       ItemsSource="{Binding Folders}"
                       CheckBoxMode="Recursive"
                       CheckBoxWidth="0"
                       ItemTemplateContextType="Node"
                       CheckedItems="{Binding CheckedItems}"
                       NodePopulationMode="Instant"
                       SelectionMode="None">

    <syncfusion:SfTreeView.ItemTemplate>
        <DataTemplate>
            <Border Padding="8" StrokeThickness="1" Stroke="Transparent">
                <Border.StrokeShape>
                    <RoundRectangle CornerRadius="4"/>
                </Border.StrokeShape>
                
                <Grid ColumnSpacing="10">
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition Width="40" />
                        <ColumnDefinition Width="40" />
                        <ColumnDefinition Width="*" />
                        <ColumnDefinition Width="Auto" />
                    </Grid.ColumnDefinitions>

                    <!-- Checkbox -->
                    <checkbox:SfCheckBox Grid.Column="0"
                                        VerticalOptions="Center"
                                        HorizontalOptions="Center"
                                        IsChecked="{Binding IsChecked, Mode=TwoWay}"/>

                    <!-- Icon -->
                    <Image Grid.Column="1"
                           Source="{Binding Content.ImageIcon}"
                           WidthRequest="24"
                           HeightRequest="24"
                           VerticalOptions="Center"/>

                    <!-- Name and Description -->
                    <VerticalStackLayout Grid.Column="2" Spacing="2">
                        <Label Text="{Binding Content.FolderName}"
                               FontSize="14"
                               FontAttributes="Bold"/>
                        <Label Text="Folder"
                               FontSize="12"
                               TextColor="Gray"
                               Opacity="0.7"/>
                    </VerticalStackLayout>

                    <!-- Badge showing child count -->
                    <Label Grid.Column="3"
                           Text="{Binding ChildNodes.Count}"
                           VerticalOptions="Center"
                           FontSize="12"
                           Padding="6,2"
                           Background="#2196F3"
                           TextColor="White"
                           CornerRadius="10"/>
                </Grid>
            </Border>
        </DataTemplate>
    </syncfusion:SfTreeView.ItemTemplate>

    <syncfusion:SfTreeView.ChildPropertyName>
        <x:String>SubFolders</x:String>
    </syncfusion:SfTreeView.ChildPropertyName>
</syncfusion:SfTreeView>
```

---

## Events and Commands

### NodeChecked Event

Detect when a node is checked or unchecked.

```csharp
treeView.NodeChecked += OnTreeViewNodeChecked;

private void OnTreeViewNodeChecked(object sender, NodeCheckedEventArgs e)
{
    var node = e.Node;
    var data = node.Content as Folder;
    
    if (node.IsChecked)
    {
        // Node was checked
        Debug.WriteLine($"Checked: {data?.FolderName}");
    }
    else
    {
        // Node was unchecked
        Debug.WriteLine($"Unchecked: {data?.FolderName}");
    }
}
```

**Properties:**
- **Node**: The `TreeViewNode` that was checked or unchecked

> **Important:** `NodeChecked` event occurs ONLY on UI interactions. To respond to programmatic checkbox changes, monitor the `CheckedItems` collection or directly bind to `IsChecked` property.

### NodeCheckedCommand

Use `NodeCheckedCommand` for MVVM-based checkbox handling:

```csharp
public class FileManagerViewModel : INotifyPropertyChanged
{
    public ICommand NodeCheckedCommand { get; }

    public FileManagerViewModel()
    {
        NodeCheckedCommand = new Command<NodeCheckedEventArgs>(OnNodeChecked);
    }

    private void OnNodeChecked(NodeCheckedEventArgs args)
    {
        var node = args.Node;
        var folder = node.Content as Folder;
        
        if (node.IsChecked)
        {
            // Handle checked state
        }
        else
        {
            // Handle unchecked state
        }
    }
}
```

```xaml
<syncfusion:SfTreeView x:Name="treeView"
                       ItemsSource="{Binding Folders}"
                       CheckBoxMode="Recursive"
                       NodeCheckedCommand="{Binding NodeCheckedCommand}"/>
```

---

## Best Practices

### 1. Choose Appropriate CheckBoxMode

- **Individual**: Use when each node's state is independent (file selection, permissions)
- **Recursive**: Use for hierarchical selection (select all files in a folder, select all dependencies)
- **None**: Use when you need other selection mechanisms without checkboxes

### 2. Handle CheckedItems Collection

```csharp
// In ViewModel
public ObservableCollection<object> CheckedItems { get; set; }

// Initialize
CheckedItems = new ObservableCollection<object>();

// Listen for changes
CheckedItems.CollectionChanged += (s, e) => 
{
    // Update UI or process checked items
};
```

### 3. Performance Optimization

- Use `NodePopulationMode="Instant"` with `CheckBoxMode="Recursive"` for large trees
- Avoid unnecessary UI updates when programmatically modifying `CheckedItems`
- Use `AutoExpandMode="AllNodesExpanded"` only for small trees

### 4. Accessibility

```xaml
<checkbox:SfCheckBox IsChecked="{Binding IsChecked, Mode=TwoWay}"
                     SemanticProperties.Description="Select folder"
                     SemanticProperties.Hint="Double tap to toggle"/>
```

### 5. Custom Checkbox Styling

```xaml
<checkbox:SfCheckBox IsChecked="{Binding IsChecked, Mode=TwoWay}"
                     CornerRadius="4"
                     CheckedBackground="#2196F3"
                     UncheckedBackground="Transparent"
                     CheckedStroke="#2196F3"
                     StrokeThickness="2"/>
```

### 6. Filtering Checked Items

```csharp
// Get checked folders
var checkedFolders = treeView.CheckedItems.OfType<Folder>();

// Filter by specific criteria
var selectedDocuments = checkedFolders.Where(f => f.FolderName.Contains("Doc"));

// Export checked items
var selectedNames = string.Join(", ", checkedFolders.Select(f => f.FolderName));
```

### 7. Avoid Common Pitfalls

```csharp
// ❌ WRONG: Direct modification of tree without updating CheckedItems
treeView.Nodes[0].IsChecked = true; // Won't update CheckedItems in bound mode

// ✅ CORRECT: Update CheckedItems collection
treeView.CheckedItems.Add(viewModel.Folders[0]); // Properly updates both tree and collection

// ❌ WRONG: Using ItemTapped with CheckBoxMode enabled
// ItemTapped won't fire when CheckBoxMode is enabled

// ✅ CORRECT: Use NodeChecked event for checkbox interactions
treeView.NodeChecked += OnNodeChecked;
```

---

## Sample Projects

- [TreeView Checkbox - Bound Mode](https://github.com/SyncfusionExamples/load-checkbox-in-each-nodes-in-.net-maui-treeview)
- [TreeView Checkbox - Unbound Mode](https://github.com/SyncfusionExamples/load-checkbox-in-each-nodes-with-unbound-mode-in-.net-maui-treeview)
- [Custom Checkbox Templates](https://github.com/SyncfusionExamples/maui-treeview-checkbox-customization)

---
