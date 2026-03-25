# Templating and Customization

This guide explains how to customize the appearance of TreeView items using templates.

## Table of Contents
- [Overview](#overview)
- [ItemTemplate](#itemtemplate)
- [ExpanderTemplate](#expandertemplate)
- [ItemTemplateContextType](#itemtemplatecontexttype)
- [DataTemplateSelector](#datatemplateselector)
- [Best Practices](#best-practices)

---

## Overview

TreeView allows customizing node appearance through:
- **ItemTemplate:** Customize content view
- **ExpanderTemplate:** Customize expander icon
- **DataTemplateSelector:** Different templates based on conditions

---

## ItemTemplate

Customize the content area of each tree node.

### Basic ItemTemplate

```xml
<syncfusion:SfTreeView x:Name="treeView"
                       ItemsSource="{Binding Files}"
                       ChildPropertyName="SubFiles">
    <syncfusion:SfTreeView.ItemTemplate>
        <DataTemplate>
            <Grid Padding="5">
                <Label Text="{Binding ItemName}" 
                       FontSize="14"
                       VerticalOptions="Center"/>
            </Grid>
        </DataTemplate>
    </syncfusion:SfTreeView.ItemTemplate>
</syncfusion:SfTreeView>
```

### ItemTemplate with Icon and Text

```xml
<syncfusion:SfTreeView.ItemTemplate>
    <DataTemplate>
        <HorizontalStackLayout Spacing="8" Padding="5">
            <Image Source="{Binding ImageIcon}" 
                   WidthRequest="24" 
                   HeightRequest="24"
                   VerticalOptions="Center"/>
            <Label Text="{Binding ItemName}" 
                   VerticalOptions="Center"
                   FontSize="14"/>
        </HorizontalStackLayout>
    </DataTemplate>
</syncfusion:SfTreeView.ItemTemplate>
```

### C# ItemTemplate

```csharp
treeView.ItemTemplate = new DataTemplate(() =>
{
    var stack = new HorizontalStackLayout { Spacing = 8, Padding = 5 };
    
    var image = new Image 
    { 
        WidthRequest = 24, 
        HeightRequest = 24 
    };
    image.SetBinding(Image.SourceProperty, "ImageIcon");
    
    var label = new Label 
    { 
        FontSize = 14, 
        VerticalOptions = LayoutOptions.Center 
    };
    label.SetBinding(Label.TextProperty, "ItemName");
    
    stack.Children.Add(image);
    stack.Children.Add(label);
    
    return stack;
});
```

---

## ExpanderTemplate

Customize the expand/collapse icon.

### Custom Expander Icons

```xml
<syncfusion:SfTreeView.ExpanderTemplate>
    <DataTemplate>
        <Image Source="{Binding IsExpanded, Converter={StaticResource ExpanderIconConverter}}"
               IsVisible="{Binding HasChildNodes}"
               WidthRequest="20"
               HeightRequest="20"
               VerticalOptions="Center"
               HorizontalOptions="Center"/>
    </DataTemplate>
</syncfusion:SfTreeView.ExpanderTemplate>
```

### Expander Icon Converter

```csharp
public class ExpanderIconConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        bool isExpanded = (bool)value;
        return isExpanded ? "collapse_icon.png" : "expand_icon.png";
    }

    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        throw new NotImplementedException();
    }
}
```

### FontIcon Expander

```xml
<syncfusion:SfTreeView.ExpanderTemplate>
    <DataTemplate>
        <Label Text="{Binding IsExpanded, Converter={StaticResource ExpanderGlyphConverter}}"
               FontFamily="MaterialIcons"
               FontSize="20"
               IsVisible="{Binding HasChildNodes}"
               VerticalOptions="Center"
               HorizontalOptions="Center"/>
    </DataTemplate>
</syncfusion:SfTreeView.ExpanderTemplate>
```

---

## ItemTemplateContextType

**CRITICAL PROPERTY:** Controls the binding context for item templates. This is essential when you need to access TreeView node metadata or when binding complex nested structures.

### Options

| Value | Binding Context | Use Case |
|-------|----------------|----------|
| `Item` | Data model object (default) | Bound mode, direct property binding |
| `Node` | TreeViewNode object | Access node properties (Level, IsExpanded, etc.) |

### When to Use Each Option

#### Use `Item` (Default)

Direct binding to your data model properties:

```xml
<syncfusion:SfTreeView ItemsSource="{Binding Files}"
                       ChildPropertyName="SubFiles">
    <syncfusion:SfTreeView.ItemTemplate>
        <DataTemplate>
            <!-- Direct property binding - binding context is your data model -->
            <Label Text="{Binding ItemName}"/>
            <Label Text="{Binding Description}"/>
        </DataTemplate>
    </syncfusion:SfTreeView.ItemTemplate>
</syncfusion:SfTreeView>
```

#### Use `Node` (Recommended for Complex Scenarios)

**HIGHLY RECOMMENDED:** Use `ItemTemplateContextType="Node"` for:
- Accessing node-specific properties like `Level`, `IsExpanded`, `HasChildNodes`
- Complex templates with nested data structures
- When you need to distinguish between node metadata and data content
- Performance-critical applications (better memory efficiency)

```xml
<syncfusion:SfTreeView x:Name="treeView"
                       ItemTemplateContextType="Node"
                       ItemsSource="{Binding Files}"
                       ChildPropertyName="SubFiles">
    <syncfusion:SfTreeView.ItemTemplate>
        <DataTemplate>
            <HorizontalStackLayout Spacing="8" Padding="5">
                <!-- Access data via Content property -->
                <Image Source="{Binding Content.ImageIcon}" 
                       WidthRequest="24" 
                       HeightRequest="24"/>
                <Label Text="{Binding Content.ItemName}"/>
                
                <!-- Access node metadata directly -->
                <Label Text="{Binding Level, StringFormat='Level: {0}'}"
                       FontSize="10"
                       TextColor="Gray"/>
                
                <!-- Other available node properties -->
                <Label IsVisible="{Binding HasChildNodes}"
                       Text="Has children"/>
            </HorizontalStackLayout>
        </DataTemplate>
    </syncfusion:SfTreeView.ItemTemplate>
</syncfusion:SfTreeView>
```

### TreeViewNode Properties (Available with `ItemTemplateContextType="Node"`)

| Property | Type | Description |
|----------|------|-------------|
| `Content` | object | Your actual data model object |
| `Level` | int | Node depth level (0 = root) |
| `IsExpanded` | bool | Whether node is currently expanded |
| `HasChildNodes` | bool | Whether node has child items |
| `Parent` | TreeViewNode | Parent node reference |
| `ChildNodes` | IList<TreeViewNode> | Collection of child nodes |

### Practical Examples with Node Context

**Example 1: Display level-based styling**
```xml
<syncfusion:SfTreeView ItemTemplateContextType="Node">
    <syncfusion:SfTreeView.ItemTemplate>
        <DataTemplate>
            <Grid Padding="{Binding Level, Converter={StaticResource LevelToIndentConverter}}">
                <Label Text="{Binding Content.Name}"
                       FontAttributes="{Binding Level, Converter={StaticResource LevelToFontAttributesConverter}}"/>
            </Grid>
        </DataTemplate>
    </syncfusion:SfTreeView.ItemTemplate>
</syncfusion:SfTreeView>
```

**Example 2: Show expand indicator only for nodes with children**
```xml
<syncfusion:SfTreeView ItemTemplateContextType="Node">
    <syncfusion:SfTreeView.ItemTemplate>
        <DataTemplate>
            <HorizontalStackLayout Spacing="8">
                <Label Text="📁" IsVisible="{Binding HasChildNodes}"/>
                <Label Text="{Binding Content.Name}"/>
                <Label Text="{Binding Content.Size}" FontSize="10" TextColor="Gray"/>
            </HorizontalStackLayout>
        </DataTemplate>
    </syncfusion:SfTreeView.ItemTemplate>
</syncfusion:SfTreeView>
```

**Example 3: Icon and metadata display**
```xml
<syncfusion:SfTreeView ItemTemplateContextType="Node">
    <syncfusion:SfTreeView.ItemTemplate>
        <DataTemplate>
            <Grid ColumnDefinitions="Auto,*" ColumnSpacing="12" Padding="8,5">
                <Label Grid.Column="0"
                       Text="{Binding Content.Icon}" 
                       FontSize="24"
                       VerticalOptions="Center"/>
                
                <VerticalStackLayout Grid.Column="1" Spacing="2">
                    <Label Text="{Binding Content.Name}" 
                           FontSize="16"
                           FontAttributes="Bold"/>
                    <Label Text="{Binding Content.Description}" 
                           FontSize="12"
                           TextColor="#888"/>
                </VerticalStackLayout>
            </Grid>
        </DataTemplate>
    </syncfusion:SfTreeView.ItemTemplate>
</syncfusion:SfTreeView>
```

### Binding Context Comparison

```csharp
// Your data model
public class FileItem
{
    public string Name { get; set; }
    public string Icon { get; set; }
    public ObservableCollection<FileItem> Children { get; set; }
}

// With ItemTemplateContextType="Item" (default)
// Binding context is FileItem instance
{Binding Name}          // ✅ Works - direct property
{Binding Icon}          // ✅ Works - direct property
{Binding Level}         // ❌ Fails - FileItem has no Level property

// With ItemTemplateContextType="Node"
// Binding context is TreeViewNode instance wrapping FileItem
{Binding Content.Name}  // ✅ Works - access data via Content
{Binding Content.Icon}  // ✅ Works - access data via Content
{Binding Level}         // ✅ Works - node property
{Binding IsExpanded}    // ✅ Works - node property
```

---

## DataTemplateSelector

Apply different templates based on conditions.

### Create Template Selector

```csharp
public class FileTemplateSelector : DataTemplateSelector
{
    public DataTemplate FolderTemplate { get; set; }
    public DataTemplate FileTemplate { get; set; }
    
    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        var node = item as TreeViewNode;
        if (node == null)
            return null;
            
        var file = node.Content as FileManager;
        
        // Different template based on type
        if (file.HasChildren)
            return FolderTemplate;
        else
            return FileTemplate;
    }
}
```

### Define Templates in Resources

```xml
<ContentPage.Resources>
    <ResourceDictionary>
        <DataTemplate x:Key="FolderTemplate">
            <HorizontalStackLayout Spacing="8" Padding="5">
                <Image Source="folder.png" WidthRequest="24" HeightRequest="24"/>
                <Label Text="{Binding Content.ItemName}" FontAttributes="Bold"/>
            </HorizontalStackLayout>
        </DataTemplate>
        
        <DataTemplate x:Key="FileTemplate">
            <HorizontalStackLayout Spacing="8" Padding="5">
                <Image Source="file.png" WidthRequest="24" HeightRequest="24"/>
                <Label Text="{Binding Content.ItemName}"/>
            </HorizontalStackLayout>
        </DataTemplate>
        
        <local:FileTemplateSelector x:Key="FileTemplateSelector"
                                    FolderTemplate="{StaticResource FolderTemplate}"
                                    FileTemplate="{StaticResource FileTemplate}"/>
    </ResourceDictionary>
</ContentPage.Resources>

<syncfusion:SfTreeView ItemTemplate="{StaticResource FileTemplateSelector}"
                       ItemTemplateContextType="Node"/>
```

### Level-Based Styling

```csharp
public class LevelBasedTemplateSelector : DataTemplateSelector
{
    public DataTemplate Level0Template { get; set; }
    public DataTemplate Level1Template { get; set; }
    public DataTemplate DefaultTemplate { get; set; }
    
    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        var node = item as TreeViewNode;
        
        return node.Level switch
        {
            0 => Level0Template,
            1 => Level1Template,
            _ => DefaultTemplate
        };
    }
}
```

---

## Best Practices

### Performance

1. **Keep templates simple** - Complex layouts slow rendering
2. **Avoid heavy images** - Use vector icons when possible
3. **Reuse templates** - Use DataTemplateSelector instead of creating many templates
4. **Don't set background in ItemTemplate** - Let SelectionBackground show through

### Styling Tips

1. **Don't set Grid background in ItemTemplate:**
   ```xml
   <!-- ❌ Wrong - hides selection -->
   <DataTemplate>
       <Grid BackgroundColor="White">...</Grid>
   </DataTemplate>
   
   <!-- ✅ Correct -->
   <DataTemplate>
       <Grid>...</Grid>
   </DataTemplate>
   ```

2. **Use converters for dynamic styling:**
   ```xml
   <Label TextColor="{Binding IsActive, Converter={StaticResource BoolToColorConverter}}"/>
   ```

3. **Access node level for indentation:**
   ```xml
   <Grid Padding="{Binding Level, Converter={StaticResource LevelToIndentConverter}}">
   ```

### Common Template Patterns

#### File Explorer Style

```xml
<DataTemplate>
    <HorizontalStackLayout Spacing="8" Padding="5">
        <Image Source="{Binding Content.Icon}" WidthRequest="20" HeightRequest="20"/>
        <Label Text="{Binding Content.Name}"/>
        <Label Text="{Binding Content.Size}" TextColor="Gray" FontSize="12"/>
        <Label Text="{Binding Content.Modified}" TextColor="Gray" FontSize="12"/>
    </HorizontalStackLayout>
</DataTemplate>
```

#### Checkbox with Item

```xml
<DataTemplate>
    <HorizontalStackLayout Spacing="8" Padding="5">
        <CheckBox IsChecked="{Binding IsChecked, Mode=TwoWay}"/>
        <Label Text="{Binding Content.Name}" VerticalOptions="Center"/>
    </HorizontalStackLayout>
</DataTemplate>
```

---

## Sample Projects

- [DataTemplateSelector Demo](https://github.com/SyncfusionExamples/data-template-selector-demo-in-.net-maui-treeview)
- [Level-Based Styling](https://github.com/SyncfusionExamples/node-level-based-styling-in-.net.maui-treeview)

---

## Related Topics

- [Styling and Appearance](styling-appearance.md) - Colors, fonts, spacing
- [Data Binding](data-binding.md) - Bind data to templates
- [Checkbox Support](advanced-features.md#checkbox) - Add checkboxes to nodes
