# Load on Demand in .NET MAUI TreeView (SfTreeView)

This guide covers implementing lazy loading of child nodes when users expand parent nodes.

## Table of Contents

- [Overview](#overview)
- [Implementation](#implementation)
- [ShowExpanderAnimation](#showexpanderanime)
- [PopulateChildNodes](#populatechildnodes)
- [Avoiding Duplicate Loading](#avoiding-duplicate-loading)
- [Best Practices](#best-practices)

---

## Overview

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

## Implementation

### Step 1: Create Data Model

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

### Step 2: Create ViewModel with LoadOnDemandCommand

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

### Step 3: XAML Implementation

```xml
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

## ShowExpanderAnimation

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

## PopulateChildNodes

The `PopulateChildNodes()` method adds loaded child nodes to the parent.

### Basic Usage

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

### With Data Conversion

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

## Avoiding Duplicate Loading

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

## Complete Example: API-Based Load on Demand

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

## Best Practices

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
   ```

### ❌ Don'ts

1. **Don't load on every expand**
   - Cache results after first load

2. **Don't block UI during loading**
   - Use `MainThread.BeginInvokeOnMainThread()`

3. **Don't forget to stop animation**
   - Always set `ShowExpanderAnimation = false`

4. **Don't set very large delays** in simulation
   - Real APIs should respond reasonably

---

## Performance Tips

1. **Batch load multiple levels** when possible
2. **Implement caching** to avoid reloading
3. **Limit initial nodes** to prevent large memory usage
4. **Use `NodePopulationMode.OnDemand`** (default for load on demand)
5. **Monitor for memory leaks** with many loaded nodes

---

## Common Issues

### Issue: Children not appearing after load
**Solution:** Call `node.PopulateChildNodes()` with correct data

### Issue: Animation never stops
**Solution:** Set `ShowExpanderAnimation = false` in finally block

### Issue: Infinite loading
**Solution:** Check condition in `CanExecuteOnDemandLoading`

