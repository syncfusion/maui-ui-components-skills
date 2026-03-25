# Styling and Appearance

Guide to customizing the visual appearance of TreeView.

## Table of Contents
- [Item Height Customization](#item-height-customization)
- [Indentation](#indentation)
- [Expander Width and Position](#expander-width-and-position)
- [Level-Based Styling](#level-based-styling)
- [Animation](#animation)
- [RTL Support](#rtl-support)
- [Theme Customization](#theme-customization)

---

## Item Height Customization

Set uniform or variable item heights:

**XAML:**

```xml
<syncfusion:SfTreeView ItemHeight="50"/>
```

**C#:**

```csharp
treeView.ItemHeight = 50;
```

**Variable Height:** Use `QueryNodeSize` event for dynamic heights.

---

## Indentation

Customize indent spacing for child levels:

**XAML:**

```xml
<syncfusion:SfTreeView Indentation="40"/>
```

**C#:**

```csharp
treeView.Indentation = 40;
```

**Default:** 30 pixels

---

## Expander Width and Position

### Expander Width

```xml
<syncfusion:SfTreeView ExpanderWidth="40"/>
```

```csharp
treeView.ExpanderWidth = 40;
```

**Default:** 32 pixels

### Expander Position

```xml
<syncfusion:SfTreeView ExpanderPosition="End"/>
```

| Value | Description |
|-------|-------------|
| `Start` | Left side (default) |
| `End` | Right side |

---

## Level-Based Styling

Use converters to style based on node level:

**Converter:**

```csharp
public class LevelToFontAttributeConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        var level = (int)value;
        return level == 0 ? FontAttributes.Bold : FontAttributes.None;
    }

    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        throw new NotImplementedException();
    }
}
```

**XAML:**

```xml
<ContentPage.Resources>
    <local:LevelToFontAttributeConverter x:Key="LevelConverter"/>
</ContentPage.Resources>

<syncfusion:SfTreeView ItemTemplateContextType="Node">
    <syncfusion:SfTreeView.ItemTemplate>
        <DataTemplate>
            <Label Text="{Binding Content.Name}"
                   FontAttributes="{Binding Level, Converter={StaticResource LevelConverter}}"/>
        </DataTemplate>
    </syncfusion:SfTreeView.ItemTemplate>
</syncfusion:SfTreeView>
```

---

## Animation

Enable expand/collapse animations:

**XAML:**

```xml
<syncfusion:SfTreeView IsAnimationEnabled="True"/>
```

**C#:**

```csharp
treeView.IsAnimationEnabled = true;
```

**Default:** `false`

---

## RTL Support

Right-to-left layout support:

```xml
<syncfusion:SfTreeView FlowDirection="RightToLeft"/>
```

---

## Theme Customization

### Disable Ripple Effect

```xml
<ContentPage.Resources>
    <syncTheme:SyncfusionThemeDictionary>
        <syncTheme:SyncfusionThemeDictionary.MergedDictionaries>
            <ResourceDictionary>
                <x:String x:Key="SfTreeViewTheme">CustomTheme</x:String>
                <Color x:Key="SfTreeViewRippleBackground">Transparent</Color>
            </ResourceDictionary>
        </syncTheme:SyncfusionThemeDictionary.MergedDictionaries>
    </syncTheme:SyncfusionThemeDictionary>
</ContentPage.Resources>
```

### Selection Colors

```xml
<syncfusion:SfTreeView SelectionBackground="#EADDFF"
                       SelectionForeground="#1C1B1F"/>
```

---

## Best Practices

1. **Keep ItemHeight reasonable** - Too small or large affects usability
2. **Use animations sparingly** - Can impact performance on large trees
3. **Test RTL layouts** - Ensure proper rendering in RTL languages
4. **Consistent indentation** - Match platform conventions

---

## Sample Projects

- [Level-Based Styling](https://github.com/SyncfusionExamples/node-level-based-styling-in-.net.maui-treeview)

---

## Related Topics

- [Templating](templating.md) - Custom item appearance
- [Selection](selection.md) - Selection styling
