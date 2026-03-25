# Data Binding and Population

This guide explains how to populate the TreeView with data using bound and unbound modes.

## Table of Contents
- [Overview](#overview)
- [Bound Mode vs Unbound Mode](#bound-mode-vs-unbound-mode)
- [Unbound Mode - Creating Nodes Manually](#unbound-mode---creating-nodes-manually)
- [Bound Mode - Data Binding](#bound-mode---data-binding)
- [Hierarchical Data with HierarchyPropertyDescriptors](#hierarchical-data-with-hierarchypropertydescriptors)
- [NodePopulationMode](#nodepopulationmode)
- [NotificationSubscriptionMode](#notificationsubscriptionmode)
- [Best Practices](#best-practices)

---

## Overview

TreeView supports two primary methods for populating data:

1. **Unbound Mode:** Manually create and manage `TreeViewNode` objects
2. **Bound Mode:** Bind to a hierarchical data source using `ItemsSource`

**Important:** You cannot set both `ItemsSource` and `Nodes` simultaneously. Choose one approach based on your requirements.

---

## Bound Mode vs Unbound Mode

| Feature | Unbound Mode | Bound Mode |
|---------|--------------|------------|
| **Data Source** | Manual `TreeViewNode` creation | `ItemsSource` property |
| **Flexibility** | Full control over each node | Data-driven, automatic |
| **MVVM Support** | Limited | Full MVVM support |
| **Performance** | Good for static data | Better for dynamic data |
| **Complexity** | More code | Less code |
| **Best For** | Fixed hierarchies, prototypes | Production apps, dynamic data |

---

## Unbound Mode - Creating Nodes Manually

In unbound mode, you create `TreeViewNode` objects and add them to the `Nodes` collection.

### Key Properties of TreeViewNode

| Property | Type | Description |
|----------|------|-------------|
| `Content` | object | Data object to display |
| `ChildNodes` | ObservableCollection | Child nodes collection |
| `IsExpanded` | bool | Expansion state |
| `IsChecked` | bool | Checkbox state (if enabled) |
| `ParentNode` | TreeViewNode | Reference to parent node |
| `Level` | int | Zero-based depth (root = 0) |

### XAML Example

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:syncfusion="clr-namespace:Syncfusion.Maui.TreeView;assembly=Syncfusion.Maui.TreeView"
             xmlns:treeviewengine="clr-namespace:Syncfusion.TreeView.Engine;assembly=Syncfusion.Maui.TreeView">
    
    <syncfusion:SfTreeView x:Name="treeView">
        <syncfusion:SfTreeView.Nodes>
            <treeviewengine:TreeViewNode Content="Australia">
                <treeviewengine:TreeViewNode.ChildNodes>
                    <treeviewengine:TreeViewNode Content="New South Wales">
                    </treeviewengine:TreeViewNode>
                </treeviewengine:TreeViewNode.ChildNodes>
            </treeviewengine:TreeViewNode>
            <treeviewengine:TreeViewNode Content="United States of America">
                <treeviewengine:TreeViewNode.ChildNodes>
                    <treeviewengine:TreeViewNode Content="California">
                    </treeviewengine:TreeViewNode>
                </treeviewengine:TreeViewNode.ChildNodes>
            </treeviewengine:TreeViewNode>
        </syncfusion:SfTreeView.Nodes>
    </syncfusion:SfTreeView>
</ContentPage>
```

### C# Example

```csharp
using Syncfusion.Maui.TreeView;
using Syncfusion.TreeView.Engine;

public class MainPage : ContentPage
{
    public MainPage()
    {
        SfTreeView treeView = new SfTreeView();

        var australia = new TreeViewNode() { Content = "Australia" };
        var nsw = new TreeViewNode() { Content = "New South Wales" };
        australia.ChildNodes.Add(nsw);
 
        var usa = new TreeViewNode() { Content = "United States of America" };
        var newYork = new TreeViewNode() { Content = "New York" };
        var california = new TreeViewNode() { Content = "California" };
        usa.ChildNodes.Add(newYork);
        usa.ChildNodes.Add(california);
        
        treeView.Nodes.Add(australia);
        treeView.Nodes.Add(usa);

        this.Content = treeView;
    }
}
```

---

## Bound Mode - Data Binding

In bound mode, bind a hierarchical data source to the `ItemsSource` property.

### Step 1: Create Data Model

```csharp
public class FileManager : INotifyPropertyChanged
{
    private string itemName;
    private ImageSource imageIcon;
    private ObservableCollection<FileManager> subFiles;

    public string ItemName
    {
        get { return itemName; }
        set
        {
            itemName = value;
            RaisedOnPropertyChanged("ItemName");
        }
    }

    public ImageSource ImageIcon
    {
        get { return imageIcon; }
        set
        {
            imageIcon = value;
            RaisedOnPropertyChanged("ImageIcon");
        }
    }

    public ObservableCollection<FileManager> SubFiles
    {
        get { return subFiles; }
        set
        {
            subFiles = value;
            RaisedOnPropertyChanged("SubFiles");
        }
    }

    public event PropertyChangedEventHandler PropertyChanged;

    public void RaisedOnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

**Note:** Implement `INotifyPropertyChanged` to enable UI updates when properties change.

### Step 2: Create ViewModel

```csharp
public class FileManagerViewModel
{
    public ObservableCollection<FileManager> ImageNodeInfo { get; set; }

    public FileManagerViewModel()
    {
        GenerateSource();
    }

    private void GenerateSource()
    {
        var nodeImageInfo = new ObservableCollection<FileManager>();
        
        var doc = new FileManager() { ItemName = "Documents", ImageIcon = "folder.png" };
        var download = new FileManager() { ItemName = "Downloads", ImageIcon = "folder.png" };
        var mp3 = new FileManager() { ItemName = "Music", ImageIcon = "folder.png" };
        var video = new FileManager() { ItemName = "Videos", ImageIcon = "folder.png" };

        var pollution = new FileManager() { ItemName = "Environmental Pollution.docx", ImageIcon = "word.png" };
        var globalWarming = new FileManager() { ItemName = "Global Warming.ppt", ImageIcon = "ppt.png" };
        var socialNetwork = new FileManager() { ItemName = "Social Network.pdf", ImageIcon = "pdfimage.png" };

        doc.SubFiles = new ObservableCollection<FileManager>
        {
            pollution,
            globalWarming,
            socialNetwork
        };

        var tutorials = new FileManager() { ItemName = "Tutorials.zip", ImageIcon = "zip.png" };
        var uiGuide = new FileManager() { ItemName = "UI-Guide.pdf", ImageIcon = "pdfimage.png" };

        download.SubFiles = new ObservableCollection<FileManager>
        {
            tutorials,
            uiGuide
        };

        nodeImageInfo.Add(doc);
        nodeImageInfo.Add(download);
        nodeImageInfo.Add(mp3);
        nodeImageInfo.Add(video);
        
        ImageNodeInfo = nodeImageInfo;
    }
}
```

### Step 3: Bind to TreeView

**XAML:**

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:syncfusion="clr-namespace:Syncfusion.Maui.TreeView;assembly=Syncfusion.Maui.TreeView"
             xmlns:local="clr-namespace:GettingStarted">
    
    <ContentPage.BindingContext>
        <local:FileManagerViewModel x:Name="viewModel"/>
    </ContentPage.BindingContext>
    
    <syncfusion:SfTreeView x:Name="treeView"
                           ItemsSource="{Binding ImageNodeInfo}"
                           ChildPropertyName="SubFiles"/>
</ContentPage>
```

**C#:**

```csharp
SfTreeView treeView = new SfTreeView();
FileManagerViewModel viewModel = new FileManagerViewModel();
treeView.ItemsSource = viewModel.ImageNodeInfo;
treeView.ChildPropertyName = "SubFiles";
this.Content = treeView;
```

**Key:** `ChildPropertyName` must match the property name in your data model that contains child items.

---

## Hierarchical Data with HierarchyPropertyDescriptors

For complex hierarchies with different types at each level, use `HierarchyPropertyDescriptors`.

### When to Use

Use `HierarchyPropertyDescriptors` when:
- Different levels have different types (e.g., Folder → File → SubFile)
- Each type has a unique child property name
- You need precise control over multi-level hierarchies

### Example: Three-Level Hierarchy

#### Data Models

```csharp
public class Folder : INotifyPropertyChanged
{
    public string FolderName { get; set; }
    public ImageSource ImageIcon { get; set; }
    public ObservableCollection<File> Files { get; set; }
    
    // INotifyPropertyChanged implementation...
}

public class File : INotifyPropertyChanged
{
    public string FileName { get; set; }
    public ImageSource ImageIcon { get; set; }
    public ObservableCollection<SubFile> SubFiles { get; set; }
    
    // INotifyPropertyChanged implementation...
}

public class SubFile : INotifyPropertyChanged
{
    public string SubFileName { get; set; }
    public ImageSource ImageIcon { get; set; }
    
    // INotifyPropertyChanged implementation...
}
```

#### XAML Configuration

```xml
<syncfusion:SfTreeView x:Name="treeView" ItemsSource="{Binding Folders}">
    <syncfusion:SfTreeView.HierarchyPropertyDescriptors>
        <treeviewengine:HierarchyPropertyDescriptor 
            TargetType="{x:Type local:Folder}" 
            ChildPropertyName="Files"/>
        <treeviewengine:HierarchyPropertyDescriptor 
            TargetType="{x:Type local:File}" 
            ChildPropertyName="SubFiles"/>
    </syncfusion:SfTreeView.HierarchyPropertyDescriptors>
</syncfusion:SfTreeView>
```

#### C# Configuration

```csharp
var treeView = new SfTreeView();
treeView.ItemsSource = viewModel.Folders;

var propertyDescriptors = new HierarchyPropertyDescriptors();
propertyDescriptors.Add(new HierarchyPropertyDescriptor() 
{
    TargetType = typeof(Folder), 
    ChildPropertyName = "Files" 
});
propertyDescriptors.Add(new HierarchyPropertyDescriptor() 
{ 
    TargetType = typeof(File), 
    ChildPropertyName = "SubFiles" 
});

treeView.HierarchyPropertyDescriptors = propertyDescriptors;
```

---

## NodePopulationMode

Controls when child nodes are populated.

### Options

| Mode | Description | Use Case |
|------|-------------|----------|
| `OnDemand` | Populate children only when parent expands (default) | Large datasets, lazy loading |
| `Instant` | Populate all children immediately | Small datasets, fast access |

### Example

```xml
<syncfusion:SfTreeView x:Name="treeView"
                       NodePopulationMode="Instant"
                       ItemsSource="{Binding Items}"
                       ChildPropertyName="SubItems"/>
```

```csharp
treeView.NodePopulationMode = TreeNodePopulationMode.Instant;
```

---

## NotificationSubscriptionMode

Controls how TreeView responds to collection changes.

### Options

| Mode | Description | When to Use |
|------|-------------|-------------|
| `None` | No automatic updates (default) | Static data |
| `CollectionChange` | Update when child collections change | Dynamic item addition/removal |
| `PropertyChange` | Update when properties change | Property modifications |

### Example

```csharp
treeView.NotificationSubscriptionMode = NotificationSubscriptionMode.CollectionChange;
```

**Important:** For UI updates, ensure your data model implements `INotifyPropertyChanged` and use `ObservableCollection`.

---

## Best Practices

### Choose the Right Mode

- **Use Unbound Mode:**
  - Static hierarchies (settings, menus)
  - Prototyping
  - Full control over node properties

- **Use Bound Mode:**
  - Dynamic data from databases/APIs
  - MVVM applications
  - Large datasets
  - Data-driven scenarios

### Performance Optimization

1. **Use `NodePopulationMode.OnDemand`** for large hierarchies
2. **Implement `INotifyPropertyChanged`** properly
3. **Use `ObservableCollection`** for dynamic collections
4. **Enable `NotificationSubscriptionMode`** only when needed
5. **Avoid deep nesting** (>5 levels) when possible

### Data Model Guidelines

1. **Always implement `INotifyPropertyChanged`** for bound mode
2. **Use `ObservableCollection<T>`** for child collections
3. **Keep property names consistent** with `ChildPropertyName`
4. **Include image paths** as `ImageSource` properties
5. **Add metadata properties** (HasChildren, NodeType, etc.) for complex scenarios

### Common Pitfalls

❌ **Don't:** Set both `ItemsSource` and `Nodes`
✅ **Do:** Choose one approach

❌ **Don't:** Forget `ChildPropertyName` in bound mode
✅ **Do:** Always specify the child property

❌ **Don't:** Use incompatible types in `HierarchyPropertyDescriptors`
✅ **Do:** Match `TargetType` with actual data model types

❌ **Don't:** Modify collections without `ObservableCollection`
✅ **Do:** Use `ObservableCollection` for dynamic updates

---

## Sample Projects

- [Bound Mode Sample](https://github.com/SyncfusionExamples/data-binding-in-.net-maui-treeview)
- [Unbound Mode Sample](https://github.com/SyncfusionExamples/populate-the-nodes-in-unbound-mode-in-.net-maui-treeview)

---

## Related Topics

- [Templating](templating.md) - Customize node appearance
- [Selection](selection.md) - Handle node selection
- [Load on Demand](advanced-features.md#load-on-demand) - Lazy load child items
