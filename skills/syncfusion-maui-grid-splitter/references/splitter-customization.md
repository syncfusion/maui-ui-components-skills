# Splitter Customization and Appearance

## Table of Contents
- [Orientation](#orientation)
- [Separator Styling](#separator-styling)
- [Resize Icons](#resize-icons)
- [Colors and Themes](#colors-and-themes)
- [Practical Styling Examples](#practical-styling-examples)

## Orientation

The `Orientation` property determines the layout direction of panes within the Grid Splitter.

### Horizontal Orientation (Default)

Panes are arranged side-by-side from left to right. Dragging the separator changes pane **widths**.

```xaml
<gridSplitter:SfGridSplitter Orientation="Horizontal">
    <gridSplitter:SplitterPane Size="250">
        <Label Text="Left Pane" HorizontalTextAlignment="Center" VerticalTextAlignment="Center"/>
    </gridSplitter:SplitterPane>
    <gridSplitter:SplitterPane Size="1*">
        <Label Text="Right Pane" HorizontalTextAlignment="Center" VerticalTextAlignment="Center"/>
    </gridSplitter:SplitterPane>
</gridSplitter:SfGridSplitter>
```

**Use cases:**
- Sidebar + main content
- Navigation + dashboard
- Explorer + editor

### Vertical Orientation

Panes are arranged top-to-bottom. Dragging the separator changes pane **heights**.

```xaml
<gridSplitter:SfGridSplitter Orientation="Vertical" Height="500">
    <gridSplitter:SplitterPane Size="100">
        <Label Text="Top Pane" HorizontalTextAlignment="Center" VerticalTextAlignment="Center"/>
    </gridSplitter:SplitterPane>
    <gridSplitter:SplitterPane Size="1*">
        <Label Text="Bottom Pane" HorizontalTextAlignment="Center" VerticalTextAlignment="Center"/>
    </gridSplitter:SplitterPane>
</gridSplitter:SfGridSplitter>
```

**Use cases:**
- Toolbar + content area
- Editor + output panel
- Preview + properties panel

### Programmatic Orientation

```csharp
SfGridSplitter gridSplitter = new SfGridSplitter
{
    Orientation = GridSplitterOrientation.Vertical,
    HeightRequest = 500
};

SplitterPane topPane = new SplitterPane
{
    Size = "100",
    Content = new Label { Text = "Top" }
};

SplitterPane bottomPane = new SplitterPane
{
    Size = "1*",
    Content = new Label { Text = "Bottom" }
};

gridSplitter.AddPane(topPane);
gridSplitter.AddPane(bottomPane);
Content = gridSplitter;
```

## Separator Styling

Customize the appearance of the splitter separator (the bar between panes).

### SeparatorSize

Set the width (horizontal) or height (vertical) of the separator:

```xaml
<gridSplitter:SfGridSplitter SeparatorSize="6">
    <gridSplitter:SplitterPane Size="1*">
        <Label Text="Pane 1"/>
    </gridSplitter:SplitterPane>
    <gridSplitter:SplitterPane Size="1*">
        <Label Text="Pane 2"/>
    </gridSplitter:SplitterPane>
</gridSplitter:SfGridSplitter>
```

**Common values:**
- `2` - Thin, minimal separator (default)
- `4` - Standard separator
- `6` - Thick, visible separator
- `10` - Extra thick for high-touch targets

### ResizeIconColor

Change the color of resize icons/handles on the separator:

```xaml
<gridSplitter:SfGridSplitter ResizeIconColor="Blue" SeparatorSize="4">
    <gridSplitter:SplitterPane Size="1*">
        <Label Text="Pane 1"/>
    </gridSplitter:SplitterPane>
    <gridSplitter:SplitterPane Size="1*">
        <Label Text="Pane 2"/>
    </gridSplitter:SplitterPane>
</gridSplitter:SfGridSplitter>
```

### ExpandCollapseIconColor

Specifies the color of the expand and collapse icon displayed in the splitter separator.

This property allows you to customize the appearance of the expand and collapse icon to match the application's theme and improve visibility against different separator backgrounds.

### XAML

```xaml
<gridSplitter:SfGridSplitter
    Orientation="Horizontal"
    ExpandCollapseIconColor="White">

    <gridSplitter:SplitterPane Size="250">
        <Label Text="Navigation Pane"/>
    </gridSplitter:SplitterPane>

    <gridSplitter:SplitterPane Size="1*">
       <Label Text="Content Pane"/>
    </gridSplitter:SplitterPane>

</gridSplitter:SfGridSplitter>
```

### C#

```csharp
SfGridSplitter gridSplitter = new SfGridSplitter
{
    Orientation = GridSplitterOrientation.Horizontal,
    ExpandCollapseIconColor = Colors.White
};

gridSplitter.AddPane(new SplitterPane
{
    Size = "250",
    Content = new Label
    {
        Text = "Navigation Pane"
    }
});

gridSplitter.AddPane(new SplitterPane
{
    Size = "1*",
    Content = new Label
    {
        Text = "Content Pane"
    }
});

Content = gridSplitter;
```

**Color options:**
- `Gray` - Neutral, blends with backgrounds
- `Blue`, `Green`, `Red` - Accent colors for visibility
- `White` - High contrast on dark backgrounds
- Custom hex: `#FF5A9FD3`

### Background Separator Color

Set the separator bar background color using `SeparatorBackground`:

```xaml
<gridSplitter:SfGridSplitter x:Name="SplitterWithBackground" SeparatorBackground="LightGray">
    <gridSplitter:SplitterPane>
        <Label Text="Pane"/>
    </gridSplitter:SplitterPane>
</gridSplitter:SfGridSplitter>
```

**Practical combinations:**
- Light separator + dark icon: `SeparatorBackground="LightGray"`, `ResizeIconColor="DarkGray"`
- Dark separator + light icon: `SeparatorBackground="DarkGray"`, `ResizeIconColor="White"`
- Themed separator: `SeparatorBackground="PrimaryColor"`, `ResizeIconColor="White"`

## Resize Icons

Customize the visual indicators for dragging and resizing.

### Icon Templates

Define custom icon templates for resize handles:

```xaml
<gridSplitter:SfGridSplitter>
    <gridSplitter:SfGridSplitter.ResizeIconTemplate>
        <DataTemplate>
            <Label Text="⋮⋮" FontSize="12" HorizontalTextAlignment="Center"/>
        </DataTemplate>
    </gridSplitter:SfGridSplitter.ResizeIconTemplate>
    
    <gridSplitter:SplitterPane Size="1*">
        <Label Text="Pane 1"/>
    </gridSplitter:SplitterPane>
    <gridSplitter:SplitterPane Size="1*">
        <Label Text="Pane 2"/>
    </gridSplitter:SplitterPane>
</gridSplitter:SfGridSplitter>
```

### Icon Symbol Examples

Various resize indicator symbols:

- `⋮⋮` - Vertical dots (good for horizontal splitter)
- `⋯⋯` - Horizontal dots (good for vertical splitter)
- `<>` - Arrows (indicates drag direction)
- `≡` - Hamburger lines
- `.` - Simple dots

### Hidden Icons

Remove icons for minimal visual impact:

```xaml
<gridSplitter:SfGridSplitter>
    <gridSplitter:SfGridSplitter.ResizeIconTemplate>
        <DataTemplate>
            <!-- Empty template hides icons -->
        </DataTemplate>
    </gridSplitter:SfGridSplitter.ResizeIconTemplate>
    
    <gridSplitter:SplitterPane Size="1*">
        <Label Text="Pane 1"/>
    </gridSplitter:SplitterPane>
</gridSplitter:SfGridSplitter>
```

## Colors and Themes

Apply consistent theming to the Grid Splitter.

### Light Theme

```xaml
<gridSplitter:SfGridSplitter SeparatorSize="3" 
                             SeparatorBackground="White"
                             ResizeIconColor="DarkGray">
    <gridSplitter:SplitterPane BackgroundColor="WhiteSmoke">
        <Label Text="Light Theme Pane 1"/>
    </gridSplitter:SplitterPane>
    <gridSplitter:SplitterPane BackgroundColor="White">
        <Label Text="Light Theme Pane 2"/>
    </gridSplitter:SplitterPane>
</gridSplitter:SfGridSplitter>
```

### Dark Theme

```xaml
<gridSplitter:SfGridSplitter SeparatorSize="3"
                             SeparatorBackground="#3E3E42"
                             ResizeIconColor="LightGray">
    <gridSplitter:SplitterPane BackgroundColor="#1E1E1E">
        <Label Text="Dark Theme" TextColor="White"/>
    </gridSplitter:SplitterPane>
    <gridSplitter:SplitterPane BackgroundColor="#252526">
        <Label Text="Dark Theme" TextColor="White"/>
    </gridSplitter:SplitterPane>
</gridSplitter:SfGridSplitter>
```

### Material Design Theme

```xaml
<gridSplitter:SfGridSplitter SeparatorSize="2"
                             SeparatorBackground="#F5F5F5"
                             ResizeIconColor="#424242">
    <gridSplitter:SplitterPane BackgroundColor="White">
        <Label Text="Material Design" Margin="16"/>
    </gridSplitter:SplitterPane>
    <gridSplitter:SplitterPane BackgroundColor="#FAFAFA">
        <Label Text="Material Design" Margin="16"/>
    </gridSplitter:SplitterPane>
</gridSplitter:SfGridSplitter>
```

### Fluent Design Theme

```xaml
<gridSplitter:SfGridSplitter SeparatorSize="1"
                             SeparatorBackground="#E1E1E1"
                             ResizeIconColor="Gray">
    <gridSplitter:SplitterPane BackgroundColor="White">
        <Label Text="Fluent Design"/>
    </gridSplitter:SplitterPane>
    <gridSplitter:SplitterPane BackgroundColor="#F3F3F3">
        <Label Text="Fluent Design"/>
    </gridSplitter:SplitterPane>
</gridSplitter:SfGridSplitter>
```

## Practical Styling Examples

### Example 1: IDE-Style Editor

Dark, professional appearance inspired by code editors:

```xaml
<gridSplitter:SfGridSplitter Orientation="Horizontal"
                             SeparatorSize="1"
                             SeparatorBackground="#3E3E42"
                             ResizeIconColor="LightGray">
    <!-- Project Explorer -->
    <gridSplitter:SplitterPane Size="250" BackgroundColor="#1E1E1E" MinimumSize="150">
        <VerticalStackLayout Padding="10" Spacing="5">
            <Label Text="Explorer" TextColor="White" FontAttributes="Bold"/>
            <Label Text="📁 Project" TextColor="White" FontSize="12"/>
            <Label Text="  📄 File.cs" TextColor="Gray" FontSize="11"/>
        </VerticalStackLayout>
    </gridSplitter:SplitterPane>

    <!-- Main Editor -->
    <gridSplitter:SplitterPane Size="1*" BackgroundColor="#252526">
        <Editor TextColor="White" BackgroundColor="#252526" 
                Placeholder="Code editor..." FontSize="12"/>
    </gridSplitter:SplitterPane>

    <!-- Output Panel -->
    <gridSplitter:SplitterPane Size="150" BackgroundColor="#1E1E1E" MinimumSize="80">
        <Label Text="Output" TextColor="White" Padding="10"/>
    </gridSplitter:SplitterPane>
</gridSplitter:SfGridSplitter>
```

### Example 2: Business Dashboard

Clean, professional dashboard with clear visual hierarchy:

```xaml
<gridSplitter:SfGridSplitter Orientation="Horizontal"
                             SeparatorSize="2"
                             SeparatorBackground="#E8E8E8"
                             ResizeIconColor="#0078D4">
    <!-- Navigation Sidebar -->
    <gridSplitter:SplitterPane Size="240" BackgroundColor="White" MinimumSize="180">
        <VerticalStackLayout Padding="10" Spacing="8">
            <Label Text="Dashboard" FontAttributes="Bold" FontSize="14"/>
            <Button Text="Overview" BackgroundColor="#0078D4" TextColor="White"/>
            <Button Text="Analytics"/>
            <Button Text="Reports"/>
        </VerticalStackLayout>
    </gridSplitter:SplitterPane>

    <!-- Main Content -->
    <gridSplitter:SplitterPane Size="1*" BackgroundColor="#F5F5F5">
        <VerticalStackLayout Padding="20">
            <Label Text="Overview" FontSize="18" FontAttributes="Bold"/>
        </VerticalStackLayout>
    </gridSplitter:SplitterPane>
</gridSplitter:SfGridSplitter>
```

### Example 3: Minimal, Clean Layout

Subtle separator for minimalist design:

```xaml
<gridSplitter:SfGridSplitter Orientation="Horizontal"
                             SeparatorSize="1"
                             SeparatorBackground="#D0D0D0"
                             ResizeIconColor="Transparent">
    <!-- Left Content -->
    <gridSplitter:SplitterPane Size="1*" BackgroundColor="White">
        <Label Text="Left Section" Padding="20"/>
    </gridSplitter:SplitterPane>

    <!-- Right Content -->
    <gridSplitter:SplitterPane Size="1*" BackgroundColor="#FAFAFA">
        <Label Text="Right Section" Padding="20"/>
    </gridSplitter:SplitterPane>
</gridSplitter:SfGridSplitter>
```

### Example 4: Responsive Multi-Pane Layout

Three-pane layout with color differentiation:

```xaml
<gridSplitter:SfGridSplitter Orientation="Horizontal"
                             SeparatorSize="2"
                             SeparatorBackground="#BDBDBD"
                             ResizeIconColor="#424242">
    <!-- Panel 1: Navigation -->
    <gridSplitter:SplitterPane Size="200" BackgroundColor="#E3F2FD" MinimumSize="150">
        <VerticalStackLayout Padding="10" Spacing="5">
            <Label Text="Navigation" FontAttributes="Bold"/>
            <Button Text="Menu 1"/>
            <Button Text="Menu 2"/>
        </VerticalStackLayout>
    </gridSplitter:SplitterPane>

    <!-- Panel 2: Content -->
    <gridSplitter:SplitterPane Size="2*" BackgroundColor="White" MinimumSize="300">
        <ScrollView>
            <VerticalStackLayout Padding="20">
                <Label Text="Main Content" FontSize="18" FontAttributes="Bold"/>
            </VerticalStackLayout>
        </ScrollView>
    </gridSplitter:SplitterPane>

    <!-- Panel 3: Details -->
    <gridSplitter:SplitterPane Size="250" BackgroundColor="#F3E5F5" MinimumSize="200">
        <VerticalStackLayout Padding="10" Spacing="5">
            <Label Text="Details" FontAttributes="Bold"/>
            <Label Text="Details content" FontSize="12"/>
        </VerticalStackLayout>
    </gridSplitter:SplitterPane>
</gridSplitter:SfGridSplitter>
```

### Programmatic Customization

```csharp
SfGridSplitter gridSplitter = new SfGridSplitter
{
    Orientation = GridSplitterOrientation.Horizontal,
    SeparatorSize = 4,
    SeparatorBackground = Color.FromArgb("#3E3E42"),
    ResizeIconColor = Colors.LightGray
};

SplitterPane pane1 = new SplitterPane
{
    Size = "250",
    BackgroundColor = Color.FromArgb("#1E1E1E"),
    Content = new Label { Text = "Pane 1", TextColor = Colors.White }
};

SplitterPane pane2 = new SplitterPane
{
    Size = "1*",
    BackgroundColor = Color.FromArgb("#252526"),
    Content = new Label { Text = "Pane 2", TextColor = Colors.White }
};

gridSplitter.AddPane(pane1);
gridSplitter.AddPane(pane2);
Content = gridSplitter;
```

---

**Next:** Learn about [collapsible panes](collapse-and-expand.md) to add expand/collapse functionality.
