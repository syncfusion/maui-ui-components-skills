# Item Height Customization in .NET MAUI TreeView (SfTreeView)

This guide covers various methods to customize the height of TreeView items.

## Table of Contents

- [Overview](#overview)
- [Static Item Height](#static-item-height)
- [QueryNodeSize Event](#querynodesize-event)
- [NodeSizeMode Property](#nodesizemode-property)
- [Dynamic Height Calculation](#dynamic-height-calculation)
- [Best Practices](#best-practices)

---

## Overview

TreeView provides multiple approaches to customize item heights:

1. **ItemHeight Property** - Uniform height for all items
2. **QueryNodeSize Event** - Per-item custom heights
3. **NodeSizeMode** - Dynamic sizing based on content
4. **GetActualNodeHeight()** - Measure content height

**Default:** `ItemHeight = 48`

---

## Static Item Height

Set a uniform height for all items using the `ItemHeight` property.

### XAML

```xml
<syncfusion:SfTreeView x:Name="treeView"
                       ItemHeight="60"
                       ItemsSource="{Binding Items}"
                       ChildPropertyName="SubItems"/>
```

### C#

```csharp
var treeView = new SfTreeView();
treeView.ItemHeight = 60;
treeView.ItemsSource = viewModel.Items;
```

### Use Cases

- Uniform spacing in simple hierarchies
- Performance optimization for large datasets
- Consistent UI when all items are similar

---

## QueryNodeSize Event

Customize height on a per-item basis using the `QueryNodeSize` event.

### Event Handler

```csharp
treeView.QueryNodeSize += TreeView_QueryNodeSize;

private void TreeView_QueryNodeSize(object sender, QueryNodeSizeEventArgs e)
{
    if (e.Node.Level == 0)
    {
        // Root items are taller
        e.Height = 80;
        e.Handled = true;
    }
    else if (e.Node.Level == 1)
    {
        // Child items are shorter
        e.Height = 50;
        e.Handled = true;
    }
}
```

### QueryNodeSizeEventArgs Properties

| Property | Type | Description |
|----------|------|-------------|
| `Node` | `TreeViewNode` | The node being measured |
| `Height` | `double` | Returned height (default or custom) |
| `Handled` | `bool` | Set to `true` to apply custom height |
| `GetActualNodeHeight()` | `method` | Returns measured content height |

### XAML Setup

```xml
<syncfusion:SfTreeView x:Name="treeView"
                       QueryNodeSize="TreeView_QueryNodeSize"
                       ItemsSource="{Binding Items}"
                       ChildPropertyName="SubItems"/>
```

### Conditional Heights Based on Content

```csharp
private void TreeView_QueryNodeSize(object sender, QueryNodeSizeEventArgs e)
{
    var content = e.Node.Content as FileItem;
    
    if (content?.Type == "Folder")
    {
        e.Height = 60;  // Folders are taller
    }
    else if (content?.Type == "File")
    {
        e.Height = 40;  // Files are smaller
    }
    else
    {
        e.Height = 48;  // Default
    }
    
    e.Handled = true;
}
```

---

## NodeSizeMode Property

Automatically adjust item heights based on content size.

### Options

| Mode | Description |
|------|-------------|
| `None` | Use `ItemHeight` property (default) |
| `Dynamic` | Auto-adjust based on template content |

### Dynamic Sizing

```xml
<syncfusion:SfTreeView x:Name="treeView"
                       NodeSizeMode="Dynamic"
                       ItemsSource="{Binding Items}"
                       ChildPropertyName="SubItems">
    <syncfusion:SfTreeView.ItemTemplate>
        <DataTemplate>
            <VerticalStackLayout Padding="10" Spacing="5">
                <Label Text="{Binding Title}" 
                       FontSize="16" 
                       FontAttributes="Bold"/>
                <Label Text="{Binding Description}" 
                       FontSize="12" 
                       TextColor="Gray"
                       LineBreakMode="WordWrap"/>
            </VerticalStackLayout>
        </DataTemplate>
    </syncfusion:SfTreeView.ItemTemplate>
</syncfusion:SfTreeView>
```

### C#

```csharp
var treeView = new SfTreeView();
treeView.NodeSizeMode = TreeViewNodeSizeMode.Dynamic;
treeView.ItemsSource = viewModel.Items;
```

### When to Use

- Multi-line text content
- Variable content per item
- Image + text combinations
- Responsive layouts

---

## Dynamic Height Calculation

Calculate height based on actual content size.

### Using GetActualNodeHeight()

```csharp
private void TreeView_QueryNodeSize(object sender, QueryNodeSizeEventArgs e)
{
    // Get measured height from content
    var measuredHeight = e.GetActualNodeHeight();
    
    // Set minimum height
    if (measuredHeight < 50)
    {
        e.Height = 50;
    }
    else
    {
        e.Height = measuredHeight;
    }
    
    e.Handled = true;
}
```

### Important Note on Image Sizing

When using images in `ItemTemplate`, you must specify explicit dimensions:

```xml
<syncfusion:SfTreeView.ItemTemplate>
    <DataTemplate>
        <Grid ColumnDefinitions="50,*" ColumnSpacing="10" Padding="5">
            <!-- Image must have explicit size -->
            <Image Grid.Column="0"
                   Source="{Binding ImagePath}"
                   WidthRequest="40"
                   HeightRequest="40"
                   Aspect="AspectFill"/>
            
            <Label Grid.Column="1"
                   Text="{Binding ItemName}"
                   VerticalOptions="Center"/>
        </Grid>
    </DataTemplate>
</syncfusion:SfTreeView.ItemTemplate>
```

**Why?** The measured height might not include images correctly if they don't have explicit dimensions.

---

## Complete Example: Mixed Content Types

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:syncfusion="clr-namespace:Syncfusion.Maui.TreeView;assembly=Syncfusion.Maui.TreeView"
             xmlns:local="clr-namespace:YourNamespace"
             x:Class="YourNamespace.MainPage">

    <ContentPage.BindingContext>
        <local:FileManagerViewModel x:Name="viewModel"/>
    </ContentPage.BindingContext>

    <syncfusion:SfTreeView x:Name="treeView"
                           QueryNodeSize="TreeView_QueryNodeSize"
                           ItemsSource="{Binding FileItems}"
                           ChildPropertyName="SubFiles">
        
        <syncfusion:SfTreeView.ItemTemplate>
            <DataTemplate>
                <Grid ColumnDefinitions="50,*" ColumnSpacing="10" Padding="8">
                    <Image Grid.Column="0"
                           Source="{Binding Icon}"
                           WidthRequest="40"
                           HeightRequest="40"
                           Aspect="AspectFill"/>
                    
                    <VerticalStackLayout Grid.Column="1" Spacing="3">
                        <Label Text="{Binding FileName}"
                               FontSize="14"
                               FontAttributes="Bold"/>
                        <Label Text="{Binding Size}"
                               FontSize="12"
                               TextColor="Gray"/>
                    </VerticalStackLayout>
                </Grid>
            </DataTemplate>
        </syncfusion:SfTreeView.ItemTemplate>
    </syncfusion:SfTreeView>
</ContentPage>
```

### Code-Behind

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
    }

    private void TreeView_QueryNodeSize(object sender, QueryNodeSizeEventArgs e)
    {
        var item = e.Node.Content as FileItem;
        
        // Folders with descriptions are taller
        if (item?.IsFolder == true && !string.IsNullOrEmpty(item.Description))
        {
            e.Height = e.GetActualNodeHeight();
        }
        // Regular items
        else
        {
            e.Height = 50;
        }
        
        e.Handled = true;
    }
}
```

---

## Real-World Scenario: File Browser

```csharp
private void TreeView_QueryNodeSize(object sender, QueryNodeSizeEventArgs e)
{
    var fileItem = e.Node.Content as FileItem;
    
    switch (fileItem?.Type)
    {
        case "Folder":
            // Folders with many files: taller
            e.Height = fileItem.SubFiles?.Count > 10 ? 70 : 55;
            break;
            
        case "Image":
            // Image files: medium height
            e.Height = 60;
            break;
            
        case "Document":
            // Documents: standard height
            e.Height = 48;
            break;
            
        default:
            // Default
            e.Height = 48;
            break;
    }
    
    e.Handled = true;
}
```

---

## Best Practices

### ✅ Do's

1. **Set explicit dimensions for images**
   ```xml
   <Image WidthRequest="40" HeightRequest="40"/>
   ```

2. **Use `NodeSizeMode="Dynamic"`** for content-driven layouts
   ```xml
   <syncfusion:SfTreeView NodeSizeMode="Dynamic"/>
   ```

3. **Set `Handled = true`** in QueryNodeSize to apply custom height
   ```csharp
   e.Handled = true;
   ```

4. **Use different heights for different levels**
   ```csharp
   if (e.Node.Level == 0) e.Height = 80;
   else e.Height = 50;
   ```

5. **Optimize for performance** - cache measurements if possible
   ```csharp
   private Dictionary<int, double> heightCache = new();
   ```

### ❌ Don'ts

1. **Don't forget to set `Handled = true`**
   - Custom height won't apply otherwise

2. **Don't set very large heights** (>200)
   - Impacts scrolling performance

3. **Don't use `GetActualNodeHeight()` for every node**
   - It's expensive; use for complex layouts only

4. **Don't mix `NodeSizeMode="Dynamic"` with fixed `ItemHeight`**
   - Choose one approach

---

## Performance Tips

1. **Cache measurements** for repeated patterns
2. **Use `NodeSizeMode="None"`** with `ItemHeight` for large datasets
3. **Avoid complex calculations** in `QueryNodeSize`
4. **Profile with large datasets** to find bottlenecks

---

## Common Issues

### Issue: Items overlap
**Solution:** Set `Handled = true` in QueryNodeSize

### Issue: Height not updating
**Solution:** Ensure `QueryNodeSize` event is subscribed

### Issue: Performance degradation
**Solution:** Switch to `NodeSizeMode="None"` with uniform `ItemHeight`

