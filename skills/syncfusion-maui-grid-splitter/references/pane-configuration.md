# Pane Configuration and Content Management

## Table of Contents
- [Pane Content](#pane-content)
- [Sizing Configuration](#sizing-configuration)
- [Size Modes](#size-modes)
- [Minimum and Maximum Constraints](#minimum-and-maximum-constraints)
- [Pane Styling](#pane-styling)
- [Practical Examples](#practical-examples)

## Pane Content

Each `SplitterPane` contains a `Content` property that accepts any MAUI view or layout. This enables flexible, rich pane designs.

### Setting Content via XAML

Direct content assignment:

```xaml
<gridSplitter:SplitterPane>
    <VerticalStackLayout Padding="16" VerticalOptions="Center" HorizontalOptions="Center">
        <Label Text="Customer Details" FontSize="20" FontAttributes="Bold"/>
        <Label Text="Manage customer information"/>
    </VerticalStackLayout>
</gridSplitter:SplitterPane>
```

Using explicit Content property:

```xaml
<gridSplitter:SplitterPane>
    <gridSplitter:SplitterPane.Content>
        <VerticalStackLayout Padding="16">
            <Label Text="Orders" FontAttributes="Bold"/>
            <CollectionView ItemsSource="{Binding Orders}"/>
        </VerticalStackLayout>
    </gridSplitter:SplitterPane.Content>
</gridSplitter:SplitterPane>
```

### Setting Content Programmatically

```csharp
SplitterPane pane = new SplitterPane
{
    Content = new VerticalStackLayout
    {
        Padding = 16,
        VerticalOptions = LayoutOptions.Center,
        HorizontalOptions = LayoutOptions.Center,
        Children =
        {
            new Label
            {
                Text = "Customer Details",
                FontSize = 20,
                FontAttributes = FontAttributes.Bold
            },
            new Label { Text = "Manage customer information" }
        }
    }
};
```

### Content Examples

**List View in a Pane:**
```xaml
<gridSplitter:SplitterPane>
    <CollectionView x:Name="ItemsList" ItemsSource="{Binding Items}">
        <CollectionView.ItemTemplate>
            <DataTemplate>
                <Label Text="{Binding Name}" Padding="10"/>
            </DataTemplate>
        </CollectionView.ItemTemplate>
    </CollectionView>
</gridSplitter:SplitterPane>
```

**Form Content:**
```xaml
<gridSplitter:SplitterPane>
    <VerticalStackLayout Padding="20" Spacing="10">
        <Label Text="Customer Information" FontAttributes="Bold" FontSize="16"/>
        <Entry Placeholder="First Name" Text="{Binding FirstName}"/>
        <Entry Placeholder="Last Name" Text="{Binding LastName}"/>
        <Entry Placeholder="Email" Text="{Binding Email}" Keyboard="Email"/>
        <Button Text="Save" Command="{Binding SaveCommand}"/>
    </VerticalStackLayout>
</gridSplitter:SplitterPane>
```

**Grid or Table Data:**
```xaml
<gridSplitter:SplitterPane>
    <Grid x:Name="ProductsGrid" BindingContext="{Binding Products}"/>
</gridSplitter:SplitterPane>
```

## Sizing Configuration

The `Size` property controls how available space is distributed among panes. It uses XAML grid sizing syntax.

### Size Property Format

Size values use the format: `[number][unit]`

| Syntax | Meaning | Example |
|--------|---------|---------|
| `1*` | Proportional - takes equal share of available space | `Size="1*"` |
| `2*` | Proportional - takes 2× the space of `1*` panes | `Size="2*"` |
| `200` | Absolute - fixed width/height in device units | `Size="200"` |
| `200px` | Explicit pixels (same as `200`) | `Size="200px"` |

### Equal-Sized Panes

Three panes with equal widths:

```xaml
<gridSplitter:SfGridSplitter>
    <gridSplitter:SplitterPane Size="1*">
        <Label Text="Pane 1" HorizontalTextAlignment="Center" VerticalTextAlignment="Center"/>
    </gridSplitter:SplitterPane>
    <gridSplitter:SplitterPane Size="1*">
        <Label Text="Pane 2" HorizontalTextAlignment="Center" VerticalTextAlignment="Center"/>
    </gridSplitter:SplitterPane>
    <gridSplitter:SplitterPane Size="1*">
        <Label Text="Pane 3" HorizontalTextAlignment="Center" VerticalTextAlignment="Center"/>
    </gridSplitter:SplitterPane>
</gridSplitter:SfGridSplitter>
```

### Proportional Sizing

Panes with different proportions (2:1:1 ratio):

```xaml
<gridSplitter:SfGridSplitter>
    <gridSplitter:SplitterPane Size="2*">
        <Label Text="Main (2x)" HorizontalTextAlignment="Center" VerticalTextAlignment="Center"/>
    </gridSplitter:SplitterPane>
    <gridSplitter:SplitterPane Size="1*">
        <Label Text="Side 1" HorizontalTextAlignment="Center" VerticalTextAlignment="Center"/>
    </gridSplitter:SplitterPane>
    <gridSplitter:SplitterPane Size="1*">
        <Label Text="Side 2" HorizontalTextAlignment="Center" VerticalTextAlignment="Center"/>
    </gridSplitter:SplitterPane>
</gridSplitter:SfGridSplitter>
```

### Fixed and Flexible Sizes

Combine absolute and proportional sizing:

```xaml
<gridSplitter:SfGridSplitter>
    <!-- Fixed sidebar: 250px -->
    <gridSplitter:SplitterPane Size="250">
        <Label Text="Navigation" HorizontalTextAlignment="Center" VerticalTextAlignment="Center"/>
    </gridSplitter:SplitterPane>
    <!-- Flexible main area: takes remaining space -->
    <gridSplitter:SplitterPane Size="1*">
        <Label Text="Content" HorizontalTextAlignment="Center" VerticalTextAlignment="Center"/>
    </gridSplitter:SplitterPane>
    <!-- Fixed right panel: 300px -->
    <gridSplitter:SplitterPane Size="300">
        <Label Text="Details" HorizontalTextAlignment="Center" VerticalTextAlignment="Center"/>
    </gridSplitter:SplitterPane>
</gridSplitter:SfGridSplitter>
```

**Result:** If total width is 1000px:
- Sidebar: 250px (fixed)
- Main: 450px (remaining flexible space)
- Details: 300px (fixed)

### C# Programmatic Sizing

```csharp
SfGridSplitter gridSplitter = new SfGridSplitter();

SplitterPane pane1 = new SplitterPane { Size = "1*" };
SplitterPane pane2 = new SplitterPane { Size = "2*" };
SplitterPane pane3 = new SplitterPane { Size = "300" };

gridSplitter.AddPane(pane1);
gridSplitter.AddPane(pane2);
gridSplitter.AddPane(pane3);
```

## Minimum and Maximum Constraints

Constrain pane sizes during resize operations:

### MinimumSize Property

Set minimum size (in device units) before a pane becomes too small:

```xaml
<gridSplitter:SplitterPane Size="1*" MinimumSize="100">
    <Label Text="Minimum 100px" HorizontalTextAlignment="Center" VerticalTextAlignment="Center"/>
</gridSplitter:SplitterPane>
```

When user drags separator, pane cannot resize below 100px.

### MaximumSize Property

Set maximum size (in device units) to prevent a pane from becoming too large:

```xaml
<gridSplitter:SplitterPane Size="200" MaximumSize="500">
    <Label Text="Maximum 500px" HorizontalTextAlignment="Center" VerticalTextAlignment="Center"/>
</gridSplitter:SplitterPane>
```

Pane cannot expand beyond 500px.

### Combining Constraints

```xaml
<gridSplitter:SfGridSplitter>
    <gridSplitter:SplitterPane Size="150" MinimumSize="100" MaximumSize="400">
        <Label Text="Sidebar" HorizontalTextAlignment="Center" VerticalTextAlignment="Center"/>
    </gridSplitter:SplitterPane>
    <gridSplitter:SplitterPane Size="1*" MinimumSize="300">
        <Label Text="Content (min 300px)" HorizontalTextAlignment="Center" VerticalTextAlignment="Center"/>
    </gridSplitter:SplitterPane>
</gridSplitter:SfGridSplitter>
```

**Behavior:**
- Sidebar: Fixed at ~150px, can resize between 100-400px
- Content: Flexible, but never smaller than 300px

### Programmatic Constraints

```csharp
SplitterPane pane = new SplitterPane
{
    Size = "200",
    MinimumSize = 100,
    MaximumSize = 500,
    Content = new Label { Text = "Constrained Pane" }
};
```
### IsResizable

Specifies whether the pane can be resized by dragging the splitter separator.

When set to true, users can resize the pane at runtime by dragging the splitter separator. When set to false, the pane size remains fixed and cannot be modified through user interaction.

### XAML

```xaml
<gridSplitter:SfGridSplitter Orientation="Horizontal">

    <gridSplitter:SplitterPane
        Size="250"
        IsResizable="False">

        <Label Text="Fixed Pane"
               HorizontalTextAlignment="Center"
               VerticalTextAlignment="Center"/>
    </gridSplitter:SplitterPane>

    <gridSplitter:SplitterPane Size="1*">

        <Label Text="Content Pane"
               HorizontalTextAlignment="Center"
               VerticalTextAlignment="Center"/>
    </gridSplitter:SplitterPane>

</gridSplitter:SfGridSplitter>
```

### C#

```csharp
SfGridSplitter gridSplitter = new SfGridSplitter
{
    Orientation = GridSplitterOrientation.Horizontal
};

SplitterPane fixedPane = new SplitterPane
{
    Size = "200",
    IsResizable = false,
    Content = new Label
    {
        Text = "Fixed Pane"
    }
};

SplitterPane contentPane = new SplitterPane
{
    Size = "1*",
    Content = new Label
    {
        Text = "Content Pane"
    }
};

gridSplitter.AddPane(fixedPane);
gridSplitter.AddPane(contentPane);

Content = gridSplitter;
```

---

## Pane Styling

Configure pane appearance and layout behavior.

### Background Color

```xaml
<gridSplitter:SplitterPane BackgroundColor="LightBlue">
    <Label Text="Pane with Background"/>
</gridSplitter:SplitterPane>
```

### Padding and Spacing

```xaml
<gridSplitter:SplitterPane Padding="20">
    <VerticalStackLayout Spacing="10">
        <Label Text="Content Item 1"/>
        <Label Text="Content Item 2"/>
    </VerticalStackLayout>
</gridSplitter:SplitterPane>
```

### Vertical Content Alignment

```xaml
<gridSplitter:SplitterPane VerticalOptions="Center">
    <Label Text="Vertically Centered"/>
</gridSplitter:SplitterPane>
```

## Practical Examples

### Example 1: Dashboard with Fixed Sidebar

A common dashboard pattern with fixed navigation and flexible main area:

```xaml
<gridSplitter:SfGridSplitter Orientation="Horizontal">
    <!-- Fixed sidebar -->
    <gridSplitter:SplitterPane Size="240" MinimumSize="150" MaximumSize="400">
        <VerticalStackLayout Padding="10" Spacing="5">
            <Label Text="Navigation" FontAttributes="Bold" FontSize="14"/>
            <Button Text="Dashboard"/>
            <Button Text="Analytics"/>
            <Button Text="Reports"/>
            <Button Text="Settings"/>
        </VerticalStackLayout>
    </gridSplitter:SplitterPane>

    <!-- Flexible main content -->
    <gridSplitter:SplitterPane Size="1*" MinimumSize="300">
        <ScrollView>
            <VerticalStackLayout Padding="20" Spacing="10">
                <Label Text="Dashboard Content" FontSize="18" FontAttributes="Bold"/>
                <Label Text="Main content area with flexible sizing"/>
            </VerticalStackLayout>
        </ScrollView>
    </gridSplitter:SplitterPane>
</gridSplitter:SfGridSplitter>
```

### Example 2: Three-Column Data Layout

Perfect for data analysis with filters, data grid, and details:

```xaml
<gridSplitter:SfGridSplitter Orientation="Horizontal">
    <!-- Filters (fixed) -->
    <gridSplitter:SplitterPane Size="200" MinimumSize="150">
        <VerticalStackLayout Padding="10" Spacing="5">
            <Label Text="Filters" FontAttributes="Bold"/>
            <HorizontalStackLayout Spacing="15">
                <CheckBox />
                <Label Text="Active"/>
                <CheckBox/>
                <Label Text="Completed"/>
            </HorizontalStackLayout>
        </VerticalStackLayout>
    </gridSplitter:SplitterPane>

    <!-- Grid (flexible) -->
    <gridSplitter:SplitterPane Size="2*" MinimumSize="300">
        <Grid x:Name="Grid"/>
    </gridSplitter:SplitterPane>

    <!-- Details Panel (fixed) -->
    <gridSplitter:SplitterPane Size="250" MinimumSize="200">
        <ScrollView>
            <VerticalStackLayout Padding="10">
                <Label Text="Details" FontAttributes="Bold"/>
            </VerticalStackLayout>
        </ScrollView>
    </gridSplitter:SplitterPane>
</gridSplitter:SfGridSplitter>
```

### Example 3: Responsive Master-Detail

Mobile-friendly layout:

```xaml
<gridSplitter:SfGridSplitter Orientation="Horizontal" x:Name="SplitterResponsive">
    <!-- Master list -->
    <gridSplitter:SplitterPane Size="1*" MinimumSize="250">
        <CollectionView ItemsSource="{Binding Items}">
            <CollectionView.ItemTemplate>
                <DataTemplate>
                    <Label Text="{Binding Name}" Padding="10"/>
                </DataTemplate>
            </CollectionView.ItemTemplate>
        </CollectionView>
    </gridSplitter:SplitterPane>

    <!-- Detail view -->
    <gridSplitter:SplitterPane Size="1*" MinimumSize="250">
        <ScrollView>
            <VerticalStackLayout Padding="20">
                <Label Text="{Binding SelectedItem.Name}" FontSize="18" FontAttributes="Bold"/>
                <Label Text="{Binding SelectedItem.Description}"/>
            </VerticalStackLayout>
        </ScrollView>
    </gridSplitter:SplitterPane>
</gridSplitter:SfGridSplitter>
```

### Example 4: Vertical Editor Layout

IDE-style with toolbar, editor, and output:

```xaml
<gridSplitter:SfGridSplitter Orientation="Vertical" Height="600">
    <!-- Toolbar (fixed) -->
    <gridSplitter:SplitterPane Size="50">
        <HorizontalStackLayout Padding="10" Spacing="5">
            <Button Text="Run" WidthRequest="60"/>
            <Button Text="Save" WidthRequest="60"/>
        </HorizontalStackLayout>
    </gridSplitter:SplitterPane>

    <!-- Editor (flexible) -->
    <gridSplitter:SplitterPane Size="2*" MinimumSize="200">
        <Editor Placeholder="Enter code here..."/>
    </gridSplitter:SplitterPane>

    <!-- Output (flexible) -->
    <gridSplitter:SplitterPane Size="1*" MinimumSize="100">
        <Label Text="Output area"/>
    </gridSplitter:SplitterPane>
</gridSplitter:SfGridSplitter>
```

---

**Next:** Learn about [splitter customization](splitter-customization.md) for separator styling and icons.
