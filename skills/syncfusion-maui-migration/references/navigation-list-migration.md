# Navigation and List Controls Migration: Xamarin.Forms to .NET MAUI

Migration guide for navigation and list controls from Xamarin.Forms to .NET MAUI.

## Table of Contents
- [SfListView Migration](#sflistview-migration)
- [SfSegmentedControl Migration](#sfsegmentedcontrol-migration)
- [SfAccordion Migration](#sfaccordion-migration)
- [SfExpander Migration](#sfexpander-migration)
- [SfTabView Migration](#sftabview-migration)

## SfListView Migration

### Namespace Changes

```csharp
// Xamarin
using Syncfusion.ListView.XForms;

// MAUI
using Syncfusion.Maui.ListView;
```

### Key Changes

Most APIs maintained with consistency updates.

### Migration Example

**Xamarin:**
```xml
<listView:SfListView ItemsSource="{Binding Contacts}"
                     ItemHeight="60"
                     AllowGrouping="True"
                     AllowSwiping="True"/>
```

**.NET MAUI:**
```xml
<listView:SfListView ItemsSource="{Binding Contacts}"
                     ItemSize="60"
                     AllowGroupExpandCollapse="True"
                     AllowSwiping="True"/>
```

**Property Changes:**

| Xamarin | MAUI |
|---------|------|
| `ItemHeight` | `ItemSize` |
| `AllowGrouping` | `AllowGroupExpandCollapse` |

## SfSegmentedControl Migration

### Namespace Changes

```csharp
// Xamarin
using Syncfusion.XForms.Buttons;

// MAUI
using Syncfusion.Maui.Buttons;
```

### Migration Example

**Xamarin:**
```xml
<buttons:SfSegmentedControl ItemsSource="{Binding Segments}"
                            DisplayMemberPath="Name"
                            SelectedIndex="0"/>
```

**.NET MAUI:**
```xml
<buttons:SfSegmentedControl ItemsSource="{Binding Segments}"
                            DisplayMemberPath="Name"
                            SelectedIndex="0"/>
```

APIs largely unchanged.

## SfAccordion Migration

### Namespace Changes

```csharp
// Xamarin
using Syncfusion.XForms.Accordion;

// MAUI
using Syncfusion.Maui.Accordion;
```

### Key Property Changes

| Xamarin | MAUI | Description |
|---------|------|-------------|
| `HeaderBackgroundColor` | `HeaderBackground` | Header background brush |
| `IconColor` | `HeaderIconColor` | Header icon color |

**Note:** `DynamicSizeMode` property removed in MAUI.

### Migration Example

**Xamarin:**
```xml
<accordion:SfAccordion ExpandMode="SingleOrNone">
    <accordion:AccordionItem HeaderBackgroundColor="LightBlue">
        <accordion:AccordionItem.Header>
            <Label Text="Section 1"/>
        </accordion:AccordionItem.Header>
        <accordion:AccordionItem.Content>
            <Label Text="Content here"/>
        </accordion:AccordionItem.Content>
    </accordion:AccordionItem>
</accordion:SfAccordion>
```

**.NET MAUI:**
```xml
<accordion:SfAccordion ExpandMode="SingleOrNone">
    <accordion:AccordionItem HeaderBackground="LightBlue">
        <accordion:AccordionItem.Header>
            <Label Text="Section 1"/>
        </accordion:AccordionItem.Header>
        <accordion:AccordionItem.Content>
            <Label Text="Content here"/>
        </accordion:AccordionItem.Content>
    </accordion:AccordionItem>
</accordion:SfAccordion>
```

## SfExpander Migration

### Namespace Changes

```csharp
// Xamarin
using Syncfusion.XForms.Expander;

// MAUI
using Syncfusion.Maui.Expander;
```

### Migration Example

**Xamarin:**
```xml
<expander:SfExpander IsExpanded="True">
    <expander:SfExpander.Header>
        <Label Text="Tap to expand"/>
    </expander:SfExpander.Header>
    <expander:SfExpander.Content>
        <Label Text="Expanded content"/>
    </expander:SfExpander.Content>
</expander:SfExpander>
```

**.NET MAUI:**
```xml
<expander:SfExpander IsExpanded="True">
    <expander:SfExpander.Header>
        <Label Text="Tap to expand"/>
    </expander:SfExpander.Header>
    <expander:SfExpander.Content>
        <Label Text="Expanded content"/>
    </expander:SfExpander.Content>
</expander:SfExpander>
```

APIs largely unchanged.

## SfTabView Migration

### Namespace Changes

```csharp
// Xamarin
using Syncfusion.XForms.TabView;

// MAUI
using Syncfusion.Maui.TabView;
```

### Migration Example

**Xamarin:**
```xml
<tabView:SfTabView>
    <tabView:SfTabItem Title="Home">
        <tabView:SfTabItem.Content>
            <Label Text="Home content"/>
        </tabView:SfTabItem.Content>
    </tabView:SfTabItem>
</tabView:SfTabView>
```

**.NET MAUI:**
```xml
<tabView:SfTabView>
    <tabView:SfTabItem Header="Home">
        <tabView:SfTabItem.Content>
            <Label Text="Home content"/>
        </tabView:SfTabItem.Content>
    </tabView:SfTabItem>
</tabView:SfTabView>
```

## Common Migration Patterns

### Selection Handling

**Xamarin:**
```csharp
listView.SelectionChanged += (s, e) =>
{
    var selectedItem = listView.SelectedItem;
};
```

**.NET MAUI:**
```csharp
listView.SelectionChanged += (s, e) =>
{
    var selectedItem = listView.SelectedItem;
};
```

### Item Templating

Templates remain largely compatible.

## Troubleshooting

### Issue: HeaderBackgroundColor not found

**Solution:** Use `HeaderBackground`:
```xml
<!-- Change -->
<accordion:AccordionItem HeaderBackgroundColor="Blue"/>

<!-- To -->
<accordion:AccordionItem HeaderBackground="Blue"/>
```

## Next Steps

1. Update NuGet packages
2. Update namespaces
3. Update property names (HeaderBackgroundColor → HeaderBackground)
4. Test selection and navigation
