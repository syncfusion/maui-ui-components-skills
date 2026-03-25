# Headers and Footers in .NET MAUI ListView

## Overview

SfListView supports headers, footers, and group headers to organize content and provide context. All can be made "sticky" to remain visible while scrolling.

## Adding Headers

### Basic Header

```xml
<syncfusion:SfListView ItemsSource="{Binding Items}">
    <syncfusion:SfListView.HeaderTemplate>
        <DataTemplate>
            <Grid BackgroundColor="#2196F3" Padding="15" HeightRequest="50">
                <Label Text="Product List" 
                       TextColor="White" 
                       FontSize="20"
                       FontAttributes="Bold"
                       VerticalOptions="Center" />
            </Grid>
        </DataTemplate>
    </syncfusion:SfListView.HeaderTemplate>
</syncfusion:SfListView>
```

### Sticky Header

The header remains visible at the top while scrolling.
```xml
<syncfusion:SfListView IsStickyHeader="True">
    <syncfusion:SfListView.HeaderTemplate>
        <DataTemplate>
            <Grid BackgroundColor="White" Padding="10">
                <SearchBar Placeholder="Search products..." />
            </Grid>
        </DataTemplate>
    </syncfusion:SfListView.HeaderTemplate>
</syncfusion:SfListView>
```
```csharp
listView.IsStickyHeader = true;
```


### Complex Header Example

```xml
<syncfusion:SfListView.HeaderTemplate>
    <DataTemplate>
        <StackLayout Padding="15" Spacing="10" BackgroundColor="White">
            <Label Text="My Inbox" 
                   FontSize="24" 
                   FontAttributes="Bold" />
            
            <Grid ColumnDefinitions="*, *, *" ColumnSpacing="10">
                <Button Grid.Column="0" Text="All" />
                <Button Grid.Column="1" Text="Unread" />
                <Button Grid.Column="2" Text="Starred" />
            </Grid>
            
            <SearchBar Placeholder="Search messages..." />
        </StackLayout>
    </DataTemplate>
</syncfusion:SfListView.HeaderTemplate>
```

## Adding Footers

### Basic Footer

```xml
<syncfusion:SfListView ItemsSource="{Binding Items}">
    <syncfusion:SfListView.FooterTemplate>
        <DataTemplate>
            <Grid BackgroundColor="LightGray" Padding="15" HeightRequest="40">
                <Label Text="{Binding Items.Count, StringFormat='{0} items total'}"
                       HorizontalOptions="Center"
                       VerticalOptions="Center" />
            </Grid>
        </DataTemplate>
    </syncfusion:SfListView.FooterTemplate>
</syncfusion:SfListView>
```

### Sticky Footer

```xml
<syncfusion:SfListView IsStickyFooter="True">
    <syncfusion:SfListView.FooterTemplate>
        <DataTemplate>
            <Button Text="Add New Item"
                    BackgroundColor="#4CAF50"
                    TextColor="White"
                    Margin="15" />
        </DataTemplate>
    </syncfusion:SfListView.FooterTemplate>
</syncfusion:SfListView>
```

### Sticky Footer Position

Use the `StickyFooterPosition` property to control where the sticky footer is anchored. The correct enum type is **`ListViewFooterPosition`** (NOT `StickyFooterPosition`).

| Enum Value | Description |
|------------|-------------|
| `ListViewFooterPosition.Default` | Footer sticks at the bottom of the visible list area (default) |
| `ListViewFooterPosition.Body` | Footer sticks at the bottom of the list body (below all items), scrolling with content until it reaches the bottom |

```xml
<!-- XAML -->
<syncfusion:SfListView IsStickyFooter="True"
                       StickyFooterPosition="Body">
    <syncfusion:SfListView.FooterTemplate>
        <DataTemplate>
            <Button Text="Add New Item"
                    BackgroundColor="#4CAF50"
                    TextColor="White"
                    Margin="15" />
        </DataTemplate>
    </syncfusion:SfListView.FooterTemplate>
</syncfusion:SfListView>
```

```csharp
// Code-behind — ALWAYS use ListViewFooterPosition enum (NOT StickyFooterPosition)
listView.StickyFooterPosition = ListViewFooterPosition.Default;  // ✅ Correct
listView.StickyFooterPosition = ListViewFooterPosition.Body;     // ✅ Correct

// ❌ WRONG — StickyFooterPosition is NOT a valid enum type:
// listView.StickyFooterPosition = StickyFooterPosition.Body;    // Compile error!
```

> ⚠️ **Common Mistake:** The *property* name is `StickyFooterPosition`, but the *enum type* is `ListViewFooterPosition`. Do **not** use `StickyFooterPosition` as the enum type — it does not exist and will cause a compile-time error: *"The name 'StickyFooterPosition' does not exist in the current context"*.

## Group Headers

### Basic Group Header

```xml
<syncfusion:SfListView IsStickyGroupHeader="True">
    <syncfusion:SfListView.DataSource>
        <data:DataSource>
            <data:DataSource.GroupDescriptors>
                <data:GroupDescriptor PropertyName="Category" />
            </data:DataSource.GroupDescriptors>
        </data:DataSource>
    </syncfusion:SfListView.DataSource>
    
    <syncfusion:SfListView.GroupHeaderTemplate>
        <DataTemplate>
            <Grid BackgroundColor="#E0E0E0" Padding="10" HeightRequest="40">
                <Label Text="{Binding Key}" 
                       FontAttributes="Bold" 
                       FontSize="16"
                       VerticalOptions="Center" />
            </Grid>
        </DataTemplate>
    </syncfusion:SfListView.GroupHeaderTemplate>
</syncfusion:SfListView>
```

### Group Header with Item Count

```xml
<syncfusion:SfListView.GroupHeaderTemplate>
    <DataTemplate>
        <Grid BackgroundColor="#F5F5F5" Padding="15,10">
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="*" />
                <ColumnDefinition Width="Auto" />
            </Grid.ColumnDefinitions>
            
            <Label Text="{Binding Key}" 
                   FontAttributes="Bold" 
                   FontSize="16"
                   VerticalOptions="Center" />
            
            <Label Grid.Column="1" 
                   Text="{Binding Count, StringFormat='({0})'}" 
                   TextColor="Gray"
                   FontSize="14"
                   VerticalOptions="Center" />
        </Grid>
    </DataTemplate>
</syncfusion:SfListView.GroupHeaderTemplate>
```

### Expandable Group Header

```xml
<syncfusion:SfListView.GroupHeaderTemplate>
    <DataTemplate>
        <Grid Padding="10" BackgroundColor="LightBlue">
            <Grid.GestureRecognizers>
                <TapGestureRecognizer Tapped="OnGroupHeaderTapped" />
            </Grid.GestureRecognizers>
            
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="Auto" />
                <ColumnDefinition Width="*" />
            </Grid.ColumnDefinitions>
            
            <Label Grid.Column="0" 
                   Text="{Binding IsExpanded, Converter={StaticResource BoolToExpanderConverter}}"
                   FontSize="20"
                   VerticalOptions="Center" />
            
            <Label Grid.Column="1" 
                   Text="{Binding Key}" 
                   FontAttributes="Bold"
                   VerticalOptions="Center"
                   Margin="10,0,0,0" />
        </Grid>
    </DataTemplate>
</syncfusion:SfListView.GroupHeaderTemplate>
```

```csharp
private void OnGroupHeaderTapped(object sender, EventArgs e)
{
    var grid = sender as Grid;
    var groupResult = grid?.BindingContext as GroupResult;
    
    if (groupResult != null)
    {
        if (groupResult.IsExpanded)
            listView.CollapseGroup(groupResult);
        else
            listView.ExpandGroup(groupResult);
    }
}
```

## Dynamic Header/Footer Content

### Update Header Based on Selection

```csharp
listView.SelectionChanged += (sender, e) =>
{
    var selectedCount = listView.SelectedItems.Count;
    
    if (selectedCount > 0)
    {
        // Show action header
        listView.HeaderTemplate = GetActionHeaderTemplate(selectedCount);
    }
    else
    {
        // Show default header
        listView.HeaderTemplate = GetDefaultHeaderTemplate();
    }
};
```

### Dynamic Footer with Statistics

```xml
<syncfusion:SfListView.FooterTemplate>
    <DataTemplate>
        <StackLayout Padding="15" BackgroundColor="White">
            <Grid ColumnDefinitions="*, *" RowDefinitions="Auto, Auto">
                <Label Grid.Row="0" Grid.Column="0" 
                       Text="Total Items:" 
                       FontAttributes="Bold" />
                <Label Grid.Row="0" Grid.Column="1" 
                       Text="{Binding Items.Count}" 
                       HorizontalOptions="End" />
                
                <Label Grid.Row="1" Grid.Column="0" 
                       Text="Total Value:" 
                       FontAttributes="Bold" />
                <Label Grid.Row="1" Grid.Column="1" 
                       Text="{Binding TotalValue, StringFormat='${0:F2}'}" 
                       HorizontalOptions="End" />
            </Grid>
        </StackLayout>
    </DataTemplate>
</syncfusion:SfListView.FooterTemplate>
```

## Common Header/Footer Patterns

### Pattern 1: Header with Filters

```xml
<syncfusion:SfListView.HeaderTemplate>
    <DataTemplate>
        <StackLayout Padding="10" Spacing="10">
            <SearchBar Placeholder="Search..." 
                       TextChanged="OnSearchChanged" />
            
            <ScrollView Orientation="Horizontal">
                <HorizontalStackLayout Spacing="10">
                    <Button Text="All" Command="{Binding FilterAllCommand}" />
                    <Button Text="Active" Command="{Binding FilterActiveCommand}" />
                    <Button Text="Completed" Command="{Binding FilterCompletedCommand}" />
                </HorizontalStackLayout>
            </ScrollView>
        </StackLayout>
    </DataTemplate>
</syncfusion:SfListView.HeaderTemplate>
```

### Pattern 2: Sticky Action Bar

```xml
<syncfusion:SfListView IsStickyHeader="True">
    <syncfusion:SfListView.HeaderTemplate>
        <DataTemplate>
            <Grid Padding="10" 
                  BackgroundColor="White"
                  IsVisible="{Binding HasSelection}">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="Auto" />
                </Grid.ColumnDefinitions>
                
                <Label Text="{Binding SelectedCount, StringFormat='{0} selected'}"
                       VerticalOptions="Center" />
                
                <Button Grid.Column="1" 
                        Text="Delete" 
                        Command="{Binding DeleteCommand}" />
                <Button Grid.Column="2" 
                        Text="Share" 
                        Command="{Binding ShareCommand}" />
            </Grid>
        </DataTemplate>
    </syncfusion:SfListView.HeaderTemplate>
</syncfusion:SfListView>
```

## Troubleshooting

**Issue:** Header/Footer not displaying
→ Ensure HeightRequest is set in template or content has measurable height

**Issue:** Sticky header not working
→ Verify IsStickyHeader="True" is set on SfListView

**Issue:** Header content cut off
→ Check that parent Grid/StackLayout has proper row/height definitions

**Issue:** Group header not sticky
→ Set IsStickyGroupHeader="True" on SfListView (not in template)

**Issue:** `"The name 'StickyFooterPosition' does not exist in the current context"` compile error
→ The *property* on SfListView is named `StickyFooterPosition`, but its *enum type* is `ListViewFooterPosition`. Always write:
```csharp
listView.StickyFooterPosition = ListViewFooterPosition.Body;     // ✅ Correct
listView.StickyFooterPosition = ListViewFooterPosition.Default;  // ✅ Correct
// NOT: StickyFooterPosition.Body  ← This enum type does not exist
```
