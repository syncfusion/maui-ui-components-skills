# Cards in .NET MAUI Kanban Board

## Table of Contents
- [Overview](#overview)
- [KanbanModel Properties](#kanbanmodel-properties)
- [Card Template Customization](#card-template-customization)
- [DataTemplateSelector for Dynamic Templates](#datatemplateselector-for-dynamic-templates)
- [Image Handling](#image-handling)
- [Common Card Patterns](#common-card-patterns)
- [Troubleshooting](#troubleshooting)

## Overview

Cards are the primary visual elements in a kanban board, representing individual tasks or items. The **SfKanban** control provides flexibility in customizing card appearance through:

1. **Default KanbanModel properties** - Built-in properties for standard card elements
2. **CardTemplate** - Complete customization of card design
3. **DataTemplateSelector** - Different templates based on conditions

**When to customize:**
- **Use default properties:** Quick setup, standard kanban layout
- **Use CardTemplate:** Unique design requirements, custom data models
- **Use DataTemplateSelector:** Multiple card designs based on data (e.g., priority, type, status)

## KanbanModel Properties

The `KanbanModel` class provides built-in properties for standard card elements.

### Property Reference

| Property | Type | Description | Display Location |
|----------|------|-------------|------------------|
| `ID` | object | Unique identifier for the card | Not displayed (internal use) |
| `Title` | string | Card title/headline | Top of card |
| `Description` | string | Detailed text | Below title |
| `Category` | object | Column category (determines placement) | Not displayed (used for routing) |
| `ImageURL` | string | Path to image file | Right side of card |
| `IndicatorFill` | Color | Status indicator color | Left border/indicator |
| `Tags` | List&lt;string&gt; | Collection of labels | Bottom of card |

### Basic Card Example

```csharp
var card = new KanbanModel
{
    ID = 1,
    Title = "iOS - 1002",
    Description = "Analyze customer requirements",
    Category = "Open",
    ImageURL = "avatar1.png",
    IndicatorFill = Colors.Red,
    Tags = new List<string> { "High Priority", "Customer", "Incident" }
};
```

**Visual Result:**
```
┌───────────────────────────────────────┐
│ ▌iOS - 1002                    [IMG] │  ← Title + Image
│ ▌                                    │  ← Indicator (Red)
│ ▌Analyze customer requirements       │  ← Description
│ ▌                                    │
│ ▌[High Priority] [Customer]          │  ← Tags
└───────────────────────────────────────┘
```

### Property Details

#### Title
```csharp
Title = "iOS - 1002"
```
- **Purpose:** Main identifier for the card
- **Best practices:** Keep concise (≤50 characters), include ticket/issue number

#### Description
```csharp
Description = "Analyze customer requirements and create specification document"
```
- **Purpose:** Detailed information about the task
- **Best practices:** 1-2 sentences, avoid excessive length
- **Word wrapping:** Automatically handled by default template

#### Category
```csharp
Category = "In Progress"
```
- **Purpose:** Determines which column the card appears in
- **Must match:** Column's `Categories` property
- **Type:** Can be string, int, or any object
- **Case-sensitive:** "Open" ≠ "open"

#### ImageURL
```csharp
ImageURL = "avatar1.png"
```
- **Purpose:** Display user avatar, icon, or image
- **Supported formats:** PNG, JPG, SVG
- **Location options:**
  - **Assembly resource:** `"Image1.png"` (stored in `Resources/Images/`)
  - **Local file:** `"C:/Path/To/Image.png"`
  - **URL:** `"YOUR_ONLINE_IMAGE"` (requires internet)

**Example with assembly resource:**
```csharp
ImageURL = "avatar_john.png"  // Stored in Resources/Images/avatar_john.png
```

#### IndicatorFill
```csharp
IndicatorFill = Colors.Red
```
- **Purpose:** Visual status indicator (priority, severity, type)
- **Type:** `Microsoft.Maui.Graphics.Color`
- **Common uses:**
  - **Red:** High priority, urgent, blocked
  - **Orange/Yellow:** Medium priority, in progress
  - **Green:** Completed, approved, low priority
  - **Blue:** Information, feature
  - **Purple:** Epic, enhancement

**Color options:**
```csharp
// Predefined colors
IndicatorFill = Colors.Red;
IndicatorFill = Colors.Orange;
IndicatorFill = Colors.Green;

// Custom hex color
IndicatorFill = Color.FromArgb("#FF5733");

// RGB color
IndicatorFill = Color.FromRgb(255, 87, 51);
```

#### Tags
```csharp
Tags = new List<string> { "Bug", "Customer", "High Priority" }
```
- **Purpose:** Categorize, filter, or add metadata to cards
- **Display:** Shown as badges/chips at bottom of card
- **Best practices:** 
  - Limit to 2-4 tags per card for readability
  - Use consistent naming (e.g., always "High Priority" not "high-priority")
  - Common tags: Priority, Type (Bug/Feature), Team, Sprint

## Card Template Customization

For complete control over card appearance, use the `CardTemplate` property.

### When to Use CardTemplate

**Required when:**
- Using custom data models (not `KanbanModel`)
- Need specific layout not provided by default template
- Want to add custom UI elements (buttons, progress bars, etc.)

**Optional when:**
- Using `KanbanModel` but want different styling
- Need brand-specific design
- Want to display additional information

### Basic CardTemplate Example

**XAML:**

```xml
<kanban:SfKanban ItemsSource="{Binding Cards}">
    <kanban:SfKanban.CardTemplate>
        <DataTemplate>
            <Border Stroke="LightGray"
                    StrokeThickness="1"
                    Background="White"
                    Padding="10"
                    Margin="5">
                <VerticalStackLayout Spacing="5">
                    <Label Text="{Binding Title}"
                           FontAttributes="Bold"
                           FontSize="16"
                           TextColor="Black" />
                    <Label Text="{Binding Description}"
                           FontSize="12"
                           TextColor="Gray"
                           LineBreakMode="WordWrap" />
                    <HorizontalStackLayout Spacing="5">
                        <BoxView Color="{Binding IndicatorFill}"
                                 WidthRequest="10"
                                 HeightRequest="10"
                                 CornerRadius="5" />
                        <Label Text="{Binding Tags[0]}"
                               FontSize="10"
                               TextColor="DarkBlue"
                               VerticalOptions="Center" />
                    </HorizontalStackLayout>
                </VerticalStackLayout>
            </Border>
        </DataTemplate>
    </kanban:SfKanban.CardTemplate>
</kanban:SfKanban>
```

**C#:**

```csharp
kanban.CardTemplate = new DataTemplate(() =>
{
    var titleLabel = new Label
    {
        FontAttributes = FontAttributes.Bold,
        FontSize = 16,
        TextColor = Colors.Black
    };
    titleLabel.SetBinding(Label.TextProperty, "Title");

    var descriptionLabel = new Label
    {
        FontSize = 12,
        TextColor = Colors.Gray,
        LineBreakMode = LineBreakMode.WordWrap
    };
    descriptionLabel.SetBinding(Label.TextProperty, "Description");

    var indicator = new BoxView
    {
        WidthRequest = 10,
        HeightRequest = 10,
        CornerRadius = 5
    };
    indicator.SetBinding(BoxView.ColorProperty, "IndicatorFill");

    var tagLabel = new Label
    {
        FontSize = 10,
        TextColor = Colors.DarkBlue,
        VerticalOptions = LayoutOptions.Center
    };
    tagLabel.SetBinding(Label.TextProperty, "Tags[0]");

    var tagStack = new HorizontalStackLayout
    {
        Spacing = 5,
        Children = { indicator, tagLabel }
    };

    var contentStack = new VerticalStackLayout
    {
        Spacing = 5,
        Children = { titleLabel, descriptionLabel, tagStack }
    };

    var border = new Border
    {
        Stroke = Colors.LightGray,
        StrokeThickness = 1,
        Background = Colors.White,
        Padding = 10,
        Margin = 5,
        Content = contentStack
    };

    return border;
});
```

### Advanced CardTemplate with Image

```xml
<kanban:SfKanban.CardTemplate>
    <DataTemplate>
        <Border Stroke="#E0E0E0"
                StrokeThickness="1"
                Background="White"
                Padding="12"
                Margin="8,4">
            <Grid RowDefinitions="Auto,Auto,Auto,Auto" 
                  ColumnDefinitions="*,Auto">
                
                <!-- Title and Image -->
                <Label Grid.Row="0" Grid.Column="0"
                       Text="{Binding Title}"
                       FontAttributes="Bold"
                       FontSize="14" />
                <Image Grid.Row="0" Grid.Column="1"
                       Grid.RowSpan="2"
                       Source="{Binding ImageURL}"
                       WidthRequest="40"
                       HeightRequest="40"
                       Aspect="AspectFill"
                       Margin="10,0,0,0">
                    <Image.Clip>
                        <EllipseGeometry RadiusX="20" RadiusY="20" 
                                         Center="20,20" />
                    </Image.Clip>
                </Image>
                
                <!-- Description -->
                <Label Grid.Row="1" Grid.Column="0"
                       Text="{Binding Description}"
                       FontSize="12"
                       TextColor="#666"
                       LineBreakMode="WordWrap"
                       MaxLines="2"
                       Margin="0,4,0,0" />
                
                <!-- Indicator bar -->
                <BoxView Grid.Row="2" Grid.ColumnSpan="2"
                         Color="{Binding IndicatorFill}"
                         HeightRequest="3"
                         Margin="0,8,0,8" />
                
                <!-- Tags -->
                <HorizontalStackLayout Grid.Row="3" Grid.ColumnSpan="2"
                                       Spacing="4"
                                       BindableLayout.ItemsSource="{Binding Tags}">
                    <BindableLayout.ItemTemplate>
                        <DataTemplate>
                            <Border Background="#E3F2FD"
                                    Padding="6,2"
                                    StrokeThickness="0">
                                <Border.StrokeShape>
                                    <RoundRectangle CornerRadius="10" />
                                </Border.StrokeShape>
                                <Label Text="{Binding .}"
                                       FontSize="10"
                                       TextColor="#1976D2" />
                            </Border>
                        </DataTemplate>
                    </BindableLayout.ItemTemplate>
                </HorizontalStackLayout>
                
            </Grid>
        </Border>
    </DataTemplate>
</kanban:SfKanban.CardTemplate>
```

## DataTemplateSelector for Dynamic Templates

Use `DataTemplateSelector` to apply different card designs based on data properties.

### When to Use

- **Different card types:** Bug vs Feature vs Task
- **Priority-based styling:** High/Medium/Low priority cards
- **Status-specific designs:** Open vs In Progress vs Done
- **Role-based views:** Different templates for different user roles

### Step-by-Step Implementation

**Step 1: Create Template Classes**

```csharp
// High Priority Template
public class HighPriorityTemplate : Grid
{
    public HighPriorityTemplate()
    {
        Padding = 10;
        BackgroundColor = Color.FromArgb("#FFEBEE");  // Light red
        
        var titleLabel = new Label
        {
            FontAttributes = FontAttributes.Bold,
            FontSize = 14,
            TextColor = Color.FromArgb("#C62828")
        };
        titleLabel.SetBinding(Label.TextProperty, "Title");
        
        var descLabel = new Label
        {
            FontSize = 12,
            TextColor = Color.FromArgb("#B71C1C")
        };
        descLabel.SetBinding(Label.TextProperty, "Description");
        
        var stack = new VerticalStackLayout
        {
            Children = { titleLabel, descLabel }
        };
        
        Children.Add(stack);
    }
}

// Normal Priority Template
public class NormalPriorityTemplate : Grid
{
    public NormalPriorityTemplate()
    {
        Padding = 10;
        BackgroundColor = Colors.White;
        
        var titleLabel = new Label
        {
            FontAttributes = FontAttributes.Bold,
            FontSize = 14
        };
        titleLabel.SetBinding(Label.TextProperty, "Title");
        
        var descLabel = new Label
        {
            FontSize = 12,
            TextColor = Colors.Gray
        };
        descLabel.SetBinding(Label.TextProperty, "Description");
        
        var stack = new VerticalStackLayout
        {
            Children = { titleLabel, descLabel }
        };
        
        Children.Add(stack);
    }
}
```

**Step 2: Create DataTemplateSelector**

```csharp
public class KanbanCardTemplateSelector : DataTemplateSelector
{
    public DataTemplate HighPriorityTemplate { get; set; }
    public DataTemplate NormalPriorityTemplate { get; set; }
    public DataTemplate LowPriorityTemplate { get; set; }

    public KanbanCardTemplateSelector()
    {
        HighPriorityTemplate = new DataTemplate(typeof(HighPriorityTemplate));
        NormalPriorityTemplate = new DataTemplate(typeof(NormalPriorityTemplate));
        LowPriorityTemplate = new DataTemplate(typeof(LowPriorityTemplate));
    }

    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        var card = item as KanbanModel;
        if (card == null)
            return NormalPriorityTemplate;

        // Logic based on tags
        if (card.Tags != null && card.Tags.Contains("High Priority"))
            return HighPriorityTemplate;
        else if (card.Tags != null && card.Tags.Contains("Low Priority"))
            return LowPriorityTemplate;
        else
            return NormalPriorityTemplate;
    }
}
```

**Step 3: Apply to SfKanban**

**XAML:**

```xml
<ContentPage xmlns:local="clr-namespace:MyApp">
    <ContentPage.Resources>
        <ResourceDictionary>
            <local:KanbanCardTemplateSelector x:Key="cardTemplateSelector" />
        </ResourceDictionary>
    </ContentPage.Resources>

    <kanban:SfKanban ItemsSource="{Binding Cards}"
                     CardTemplate="{StaticResource cardTemplateSelector}" />
</ContentPage>
```

**C#:**

```csharp
kanban.CardTemplate = new KanbanCardTemplateSelector();
```

## Image Handling

### Assembly Resource Images

**Step 1:** Add image to `Resources/Images/` folder in your project.

**Step 2:** Reference by filename:

```csharp
ImageURL = "avatar_john.png"
```

### Local File Images

```csharp
ImageURL = "C:/Users/John/Pictures/avatar.png"
```

### URL Images (Requires Internet)

```csharp
ImageURL = "YOUR_ONLINE_Image"
```

### Circular Image Example

```xml
<Image Source="{Binding ImageURL}"
       WidthRequest="50"
       HeightRequest="50"
       Aspect="AspectFill">
    <Image.Clip>
        <EllipseGeometry RadiusX="25" RadiusY="25" Center="25,25" />
    </Image.Clip>
</Image>
```

## Common Card Patterns

### Pattern 1: Priority-Based Color Coding

```csharp
public Color GetIndicatorColor(string priority)
{
    return priority switch
    {
        "High" => Colors.Red,
        "Medium" => Colors.Orange,
        "Low" => Colors.Green,
        _ => Colors.Gray
    };
}

// Usage
card.IndicatorFill = GetIndicatorColor("High");
```

### Pattern 2: Dynamic Tag Display

```xml
<HorizontalStackLayout BindableLayout.ItemsSource="{Binding Tags}">
    <BindableLayout.ItemTemplate>
        <DataTemplate>
            <Border Background="#E0E0E0" 
                    Padding="5,2" 
                    Margin="2"
                    StrokeThickness="0">
                <Border.StrokeShape>
                    <RoundRectangle CornerRadius="8" />
                </Border.StrokeShape>
                <Label Text="{Binding .}" FontSize="10" />
            </Border>
        </DataTemplate>
    </BindableLayout.ItemTemplate>
</HorizontalStackLayout>
```

### Pattern 3: Card with Actions (Buttons)

```xml
<kanban:SfKanban.CardTemplate>
    <DataTemplate>
        <Border Padding="10">
            <Grid RowDefinitions="Auto,Auto,Auto">
                <Label Grid.Row="0" Text="{Binding Title}" FontAttributes="Bold" />
                <Label Grid.Row="1" Text="{Binding Description}" />
                <HorizontalStackLayout Grid.Row="2" Spacing="5">
                    <Button Text="Edit" 
                            FontSize="10" 
                            Padding="5,2"
                            Clicked="OnEditClicked" />
                    <Button Text="Delete" 
                            FontSize="10" 
                            Padding="5,2"
                            BackgroundColor="Red"
                            TextColor="White"
                            Clicked="OnDeleteClicked" />
                </HorizontalStackLayout>
            </Grid>
        </Border>
    </DataTemplate>
</kanban:SfKanban.CardTemplate>
```

## Troubleshooting

### Issue: Images not displaying

**Possible causes:**
1. Image path incorrect
2. Image not in `Resources/Images/` folder
3. Build action not set to `MauiImage`

**Solution:**
1. Verify image path matches filename
2. Ensure image is in correct folder
3. Right-click image → Properties → Build Action: MauiImage

### Issue: Custom template not working with KanbanModel

**Solution:** Ensure bindings use correct property names:
- `{Binding Title}` not `{Binding TaskTitle}`
- Property names are case-sensitive

### Issue: Tags not displaying

**Check:**
1. Tags collection is not null
2. Tags collection has items
3. Template has binding to `Tags`

**Debug:**
```csharp
System.Diagnostics.Debug.WriteLine($"Tags count: {card.Tags?.Count ?? 0}");
```

### Issue: Card appears blank

**Common causes:**
1. BindingContext not set correctly
2. Property bindings misspelled
3. Data is null

**Solution:** Add fallback values in XAML:
```xml
<Label Text="{Binding Title, FallbackValue='No Title'}" />
```

## Next Steps

- **Configure columns:** See [columns.md](columns.md)
- **Handle card events:** See [events.md](events.md)
- **Implement workflows:** See [workflows.md](workflows.md)