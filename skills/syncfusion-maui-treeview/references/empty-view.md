# Empty View in .NET MAUI TreeView (SfTreeView)

The `EmptyView` feature allows you to display custom content when the TreeView has no data. This guide covers implementation and customization.

## Table of Contents

- [Overview](#overview)
- [Display String Message](#display-string-message)
- [Display Custom View](#display-custom-view)
- [EmptyViewTemplate](#emptyviewtemplate)
- [Trigger Conditions](#trigger-conditions)
- [Best Practices](#best-practices)

---

## Overview

The `EmptyView` property displays content when:

- `ItemsSource` is empty or null (bound mode)
- `Nodes` collection is empty (unbound mode)

You can display either:
1. **String message** - Simple text
2. **Custom view** - Complex UI elements
3. **Template** - Data-driven custom appearance with `EmptyViewTemplate`

---

## Display String Message

The simplest way to show empty state is with a string message.

### XAML

```xml
<syncfusion:SfTreeView x:Name="treeView"
                       ItemsSource="{Binding Items}"
                       EmptyView="No items available">
</syncfusion:SfTreeView>
```

### C#

```csharp
var treeView = new SfTreeView();
treeView.ItemsSource = viewModel.Items;
treeView.EmptyView = "No items available";
```

### Result
When `ItemsSource` is empty, the TreeView displays:
```
No items available
```

---

## Display Custom View

Display complex UI elements when the TreeView has no data.

### Custom Border with Label

```xml
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

### Custom View with Button

```xml
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

### C# Code-Behind

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

## EmptyViewTemplate

Use `EmptyViewTemplate` to customize the appearance of `EmptyView` with data binding.

### When to Use

- Display dynamic content based on ViewModel data
- Complex templating with bindings
- Conditional formatting in empty state

### Setup with Template

```xml
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

### EmptyStateModel Class

```csharp
public class EmptyStateModel
{
    public string Message { get; set; }
}
```

### ViewModel Implementation

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

## Trigger Conditions

### Empty ItemsSource (Bound Mode)

```csharp
// When ItemsSource is empty or null
viewModel.Items.Clear();  // Triggers EmptyView

// When ItemsSource is null
treeView.ItemsSource = null;  // Triggers EmptyView
```

### Empty Nodes Collection (Unbound Mode)

```csharp
// When Nodes collection is empty
treeView.Nodes.Clear();  // Triggers EmptyView
```

### Filtering Results

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

## Complete Example: Search with Empty State

```xml
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

### Code-Behind

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

### ✅ Do's

1. **Always provide an EmptyView** for better UX
   ```xml
   <syncfusion:SfTreeView EmptyView="No data available"/>
   ```

2. **Use EmptyViewTemplate** for dynamic empty states
   ```xml
   <syncfusion:SfTreeView.EmptyViewTemplate>
       <DataTemplate>...</DataTemplate>
   </syncfusion:SfTreeView.EmptyViewTemplate>
   ```

3. **Set `NotificationSubscriptionMode="CollectionChange"`** when content might change
   ```xml
   <syncfusion:SfTreeView NotificationSubscriptionMode="CollectionChange"/>
   ```

4. **Make empty view visually distinct** with icons or styling
   ```xml
   <Label Text="📭" FontSize="48"/>
   ```

5. **Include actionable content** like "Add New" button if applicable

### ❌ Don'ts

1. **Don't set EmptyView to null**
   - Always provide meaningful empty state

2. **Don't ignore `NotificationSubscriptionMode`**
   - Updates might not reflect in EmptyView

3. **Don't make empty view too large**
   - Keep it centered and moderate size

4. **Don't use complex templates for empty view**
   - Keep it simple and fast to render

---

## Common Issues

### Issue: EmptyView Not Showing
**Solution:** Ensure `NotificationSubscriptionMode="CollectionChange"` is set

### Issue: EmptyView Shows When Items Exist
**Solution:** Check if `ItemsSource` binding is correct

### Issue: Template Bindings Not Working
**Solution:** Verify the data model passed to `EmptyView` is correct

