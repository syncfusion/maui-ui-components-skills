# Collapsible Panes: Collapse and Expand

## Table of Contents
- [Making Panes Collapsible](#making-panes-collapsible)
- [Programmatic Collapse and Expand](#programmatic-collapse-and-expand)
- [Managing Collapsed State](#managing-collapsed-state)
- [Events and State Tracking](#events-and-state-tracking)
- [Practical Examples](#practical-examples)

## Making Panes Collapsible

The `IsCollapsible` property enables collapse/expand buttons on panes, allowing users to minimize content areas with a single click.

### Enable Collapse Button

```xaml
<gridSplitter:SplitterPane IsCollapsible="True">
    <Label Text="Collapsible Pane"/>
</gridSplitter:SplitterPane>
```

When `IsCollapsible="True"`:
- A collapse/expand button (arrow icon) appears on the pane
- Users can click the button to toggle pane visibility
- The pane minimizes to a header-only state when collapsed
- Clicking again expands the pane to its previous size

### Complete Example: Sidebar with Collapse Button

```xaml
<gridSplitter:SfGridSplitter Orientation="Horizontal">
    <!-- Collapsible Navigation Sidebar -->
    <gridSplitter:SplitterPane Size="250" IsCollapsible="True" MinimumSize="30">
        <VerticalStackLayout Padding="10" Spacing="5">
            <Label Text="Navigation" FontAttributes="Bold"/>
            <Button Text="Dashboard"/>
            <Button Text="Reports"/>
            <Button Text="Settings"/>
        </VerticalStackLayout>
    </gridSplitter:SplitterPane>

    <!-- Main Content Area -->
    <gridSplitter:SplitterPane Size="1*">
        <Label Text="Main Content" HorizontalTextAlignment="Center" VerticalTextAlignment="Center"/>
    </gridSplitter:SplitterPane>
</gridSplitter:SfGridSplitter>
```

### Multiple Collapsible Panes

```xaml
<gridSplitter:SfGridSplitter Orientation="Vertical" Height="600">
    <!-- Top toolbar (collapsible) -->
    <gridSplitter:SplitterPane Size="60" IsCollapsible="True" MinimumSize="30">
        <HorizontalStackLayout Padding="10" Spacing="5">
            <Button Text="Save" WidthRequest="60"/>
            <Button Text="Undo" WidthRequest="60"/>
        </HorizontalStackLayout>
    </gridSplitter:SplitterPane>

    <!-- Editor area (non-collapsible) -->
    <gridSplitter:SplitterPane Size="1*" MinimumSize="200">
        <Editor Placeholder="Code editor..."/>
    </gridSplitter:SplitterPane>

    <!-- Output panel (collapsible) -->
    <gridSplitter:SplitterPane Size="150" IsCollapsible="True" MinimumSize="30">
        <VerticalStackLayout Padding="10">
            <Label Text="Output:" FontAttributes="Bold"/>
            <Label Text="Ready"/>
        </VerticalStackLayout>
    </gridSplitter:SplitterPane>
</gridSplitter:SfGridSplitter>
```

### Important: MinimumSize for Collapsible Panes

When a pane is collapsible, always set `MinimumSize` to a small value (typically 20-40) to allow proper collapse:

```xaml
<!-- ✓ Correct -->
<gridSplitter:SplitterPane IsCollapsible="True" MinimumSize="30">
    <!-- Content -->
</gridSplitter:SplitterPane>

<!-- ✗ Incorrect (pane cannot collapse if MinimumSize is too large) -->
<gridSplitter:SplitterPane IsCollapsible="True" MinimumSize="300">
    <!-- Content -->
</gridSplitter:SplitterPane>
```

## Programmatic Collapse and Expand

Control pane state through C# code using the `CollapsePane` and `ExpandPane` methods.

### CollapsePane Method

Collapse a pane at a specific index:

```csharp
SfGridSplitter gridSplitter = new SfGridSplitter();

SplitterPane pane1 = new SplitterPane { Content = new Label { Text = "Pane 1" } };
SplitterPane pane2 = new SplitterPane { Content = new Label { Text = "Pane 2" } };

gridSplitter.AddPane(pane1);
gridSplitter.AddPane(pane2);

// Collapse the first pane (index 0)
gridSplitter.CollapsePane(0);

Content = gridSplitter;
```

### ExpandPane Method

Expand a collapsed pane:

```csharp
// Expand the first pane
gridSplitter.ExpandPane(0);
```

### Toggle Collapse/Expand

Check current state and toggle:

```csharp
// Helper method to toggle pane state
void TogglePaneCollapse(SfGridSplitter splitter, int paneIndex)
{
    SplitterPane pane = splitter.SplitterPanes[paneIndex];
    
    if (pane.IsCollapsed)
    {
        splitter.ExpandPane(paneIndex);
    }
    else
    {
        splitter.CollapsePane(paneIndex);
    }
}
```

### Collapse Multiple Panes

```csharp
// Collapse all panes except the main content area
void MinimizeAllPanels(SfGridSplitter splitter)
{
    // Assume panes are indexed as: 0=sidebar, 1=main, 2=details
    splitter.CollapsePane(0);  // Collapse sidebar
    // Leave main content expanded
    splitter.CollapsePane(2);  // Collapse details
}
```

## Managing Collapsed State

Track and manage pane collapse/expand states in your application.

### IsCollapsed Property

Check if a pane is collapsed:

```csharp
SplitterPane pane = gridSplitter.SplitterPanes[0];

if (pane.IsCollapsed)
{
    // Pane is collapsed
}
else
{
    // Pane is expanded
}
```

### Initialize with Collapsed State

Start an application with a pane already collapsed:

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        
        // After building the UI, collapse the sidebar
        MainThread.BeginInvokeOnMainThread(() =>
        {
            if (gridSplitter.SplitterPanes.Count > 0)
            {
                gridSplitter.CollapsePane(0);
            }
        });
    }
}
```

### Persist Collapse State

Save and restore pane states using application preferences:

```csharp
public class GridSplitterStateManager
{
    private const string COLLAPSE_STATE_KEY = "GridSplitter_CollapseState";
    
    // Save current collapse states
    public void SaveState(SfGridSplitter splitter)
    {
        var states = splitter.SplitterPanes
            .Select((pane, index) => new { index, isCollapsed = pane.IsCollapsed })
            .ToList();
        
        string json = JsonConvert.SerializeObject(states);
        Preferences.Set(COLLAPSE_STATE_KEY, json);
    }
    
    // Restore saved collapse states
    public void RestoreState(SfGridSplitter splitter)
    {
        if (Preferences.ContainsKey(COLLAPSE_STATE_KEY))
        {
            string json = Preferences.Get(COLLAPSE_STATE_KEY, "");
            var states = JsonConvert.DeserializeObject<List<dynamic>>(json);
            
            foreach (var state in states)
            {
                if (state.isCollapsed)
                {
                    splitter.CollapsePane((int)state.index);
                }
            }
        }
    }
}

// Usage
public partial class MainPage : ContentPage
{
    private GridSplitterStateManager stateManager = new GridSplitterStateManager();
    
    protected override void OnAppearing()
    {
        base.OnAppearing();
        stateManager.RestoreState(gridSplitter);
    }
    
    protected override void OnDisappearing()
    {
        base.OnDisappearing();
        stateManager.SaveState(gridSplitter);
    }
}
```

## Events and State Tracking

Listen for collapse/expand events and respond to state changes.

### Collapsing Event

Occurs before one or more panes are collapsed.

Use this event to perform validation, save state information, or update the UI before the pane is collapsed.

### XAML

```xaml
<gridSplitter:SfGridSplitter
    Collapsing="OnCollapsing">

    <!-- Panes -->

</gridSplitter:SfGridSplitter>
```

### C#

```csharp
private void OnCollapsing(object sender, GridSplitterPaneCollapsingEventArgs e)
{
    Debug.WriteLine($"Collapsing pane: {string.Join(",", e.Indexes)}");
}
```

---

### Collapsed Event

Fires when a pane is collapsed (either by user click or programmatic call):

```xaml
<gridSplitter:SfGridSplitter x:Name="gridSplitter" Collapsed="OnPaneCollapsed">
    <!-- Panes -->
</gridSplitter:SfGridSplitter>
```

```csharp
private void OnPaneCollapsed(object sender, GridSplitterPaneCollapsedEventArgs e)
{
    // e.Index: Index of collapsed pane
    // e.Pane: Reference to the SplitterPane
    
    Debug.WriteLine($"Pane {e.Indexes} collapsed");
    
    // Update UI or application state
    UpdateStatusBar($"Pane collapsed: {e.Indexes}");
}
```

### Expanding Event

Occurs before one or more panes are expanded.

Use this event to prepare content, restore state information, or update the UI before the pane is expanded.

### XAML

```xaml
<gridSplitter:SfGridSplitter
    Expanding="OnExpanding">

    <!-- Panes -->

</gridSplitter:SfGridSplitter>
```

### C#

```csharp
private void OnExpanding(object sender, GridSplitterPaneExpandingEventArgs e)
{
    Debug.WriteLine($"Expanding pane: {string.Join(",", e.Indexes)}");
}
```

---

### Expanded Event

Fires when a pane is expanded:

```xaml
<gridSplitter:SfGridSplitter x:Name="gridSplitter" Expanded="OnPaneExpanded">
    <!-- Panes -->
</gridSplitter:SfGridSplitter>
```

```csharp
private void OnPaneExpanded(object sender, GridSplitterPaneExpandedEventArgs e)
{
    // e.Index: Index of expanded pane
    // e.Pane: Reference to the SplitterPane
    
    Debug.WriteLine($"Pane {e.Indexes} expanded");
    
    // Refresh content or reset state
    RefreshPaneContent(e.Indexes);
}
```

### ResizeStarted Event

Occurs when the user starts dragging a splitter separator.

Use this event to track resize operations, pause expensive processing, or display status information while resizing begins.

### XAML

```xaml
<gridSplitter:SfGridSplitter
    ResizeStarted="OnResizeStarted">

    <!-- Panes -->

</gridSplitter:SfGridSplitter>
```

### C#

```csharp
private void OnResizeStarted(object sender, GridSplitterResizeStartedEventArgs e)
{
    Debug.WriteLine("Pane resize started");
}
```

---

### Resizing Event

Occurs continuously while the user is dragging the splitter separator and pane sizes are being updated.

Use this event to monitor size changes in real time or update related UI elements during resizing.

### XAML

```xaml
<gridSplitter:SfGridSplitter
    Resizing="OnResizing">

    <!-- Panes -->

</gridSplitter:SfGridSplitter>
```

### C#

```csharp
private void OnResizing(object sender, GridSplitterResizingEventArgs e)
{
    Debug.WriteLine("Pane is resizing");
}
```

---

### ResizeEnded Event

Occurs when the user finishes dragging the splitter separator.

Use this event to save pane sizes, refresh layouts, or perform actions that should occur after resizing is completed.

### XAML

```xaml
<gridSplitter:SfGridSplitter
    ResizeEnded="OnResizeEnded">

    <!-- Panes -->

</gridSplitter:SfGridSplitter>
```

### C#

```csharp
private void OnResizeEnded(object sender, GridSplitterResizeEndedEventArgs e)
{
    Debug.WriteLine("Pane resize completed");
}
```

---

### Complete Event Handling Example

### XAML

```xaml
<gridSplitter:SfGridSplitter
    x:Name="gridSplitter"
    Orientation="Horizontal"
    Collapsing="OnCollapsing"
    Expanding="OnExpanding"
    ResizeStarted="OnResizeStarted"
    Resizing="OnResizing"
    ResizeEnded="OnResizeEnded">

    <gridSplitter:SplitterPane
        Size="250"
        IsCollapsible="True">

        <Label Text="Navigation Pane"/>
    </gridSplitter:SplitterPane>

    <gridSplitter:SplitterPane
        Size="1*">

        <Label Text="Content Pane"/>
    </gridSplitter:SplitterPane>

</gridSplitter:SfGridSplitter>
```

### C#

```csharp
private void OnCollapsing(object sender, GridSplitterPaneCollapsingEventArgs e)
{
    Debug.WriteLine($"Collapsing Pane: {string.Join(",", e.Indexes)}");
}

private void OnExpanding(object sender, GridSplitterPaneExpandingEventArgs e)
{
    Debug.WriteLine($"Expanding pane: {string.Join(",", e.Indexes)}");
}

private void OnResizeStarted(object sender, GridSplitterResizeStartedEventArgs e)
{
   Debug.WriteLine("Resize operation started");
}

private void OnResizing(object sender, GridSplitterResizingEventArgs e)
{
    Debug.WriteLine("Pane sizes are changing");
}
```

## Practical Examples

### Example 1: IDE with Collapsible Panels

A typical IDE layout with collapsible explorer, output, and properties panels:

```xaml
<gridSplitter:SfGridSplitter Orientation="Horizontal" SeparatorSize="2">
    <!-- Project Explorer (collapsible) -->
    <gridSplitter:SplitterPane Size="200" IsCollapsible="True" MinimumSize="25">
        <VerticalStackLayout Padding="10" Spacing="5">
            <Label Text="Explorer" FontAttributes="Bold"/>
            <CollectionView ItemsSource="{Binding Files}">
                <CollectionView.ItemTemplate>
                    <DataTemplate>
                        <Label Text="{Binding Name}" Padding="5"/>
                    </DataTemplate>
                </CollectionView.ItemTemplate>
            </CollectionView>
        </VerticalStackLayout>
    </gridSplitter:SplitterPane>

    <!-- Code Editor (main, non-collapsible) -->
    <gridSplitter:SplitterPane Size="1*" MinimumSize="300">
        <Editor Text="{Binding Code}" FontSize="12"/>
    </gridSplitter:SplitterPane>

    <!-- Properties (collapsible) -->
    <gridSplitter:SplitterPane Size="200" IsCollapsible="True" MinimumSize="25">
        <VerticalStackLayout Padding="10">
            <Label Text="Properties" FontAttributes="Bold"/>
            <Picker Title="Property" ItemsSource="{Binding Properties}"/>
        </VerticalStackLayout>
    </gridSplitter:SplitterPane>
</gridSplitter:SfGridSplitter>
```

### Example 2: Dashboard with Toggle Button

Dashboard that allows users to collapse sidebar via button:

```xaml
<VerticalStackLayout>
    <!-- Toggle Button -->
    <Button x:Name="ToggleSidebarButton" Text="Toggle Sidebar" Clicked="OnToggleSidebarClicked"/>
    
    <!-- Grid Splitter -->
    <gridSplitter:SfGridSplitter Orientation="Horizontal">
        <!-- Sidebar (collapsible) -->
        <gridSplitter:SplitterPane x:Name="SidebarPane" Size="250" IsCollapsible="True" MinimumSize="30">
            <VerticalStackLayout Padding="10" Spacing="10">
                <Label Text="Sidebar" FontAttributes="Bold"/>
                <Button Text="Menu 1" Command="{Binding Menu1Command}"/>
                <Button Text="Menu 2" Command="{Binding Menu2Command}"/>
            </VerticalStackLayout>
        </gridSplitter:SplitterPane>

        <!-- Main Content -->
        <gridSplitter:SplitterPane Size="1*">
            <VerticalStackLayout Padding="20">
                <Label Text="Dashboard Content" FontSize="18" FontAttributes="Bold"/>
            </VerticalStackLayout>
        </gridSplitter:SplitterPane>
    </gridSplitter:SfGridSplitter>
</VerticalStackLayout>
```

```csharp
private void OnToggleSidebarClicked(object sender, EventArgs e)
{
    if (SidebarPane.IsCollapsed)
    {
        gridSplitter.ExpandPane(0);
        ToggleSidebarButton.Text = "Hide Sidebar";
    }
    else
    {
        gridSplitter.CollapsePane(0);
        ToggleSidebarButton.Text = "Show Sidebar";
    }
}
```

### Example 3: Responsive Mobile Layout

Collapse navigation on mobile to maximize content space:

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        
        // Check screen size
        if (DeviceDisplay.MainDisplayInfo.Width < 600)  // Mobile device
        {
            MainThread.BeginInvokeOnMainThread(() =>
            {
                // Collapse sidebar on small screens
                gridSplitter.CollapsePane(0);
            });
        }
    }
}
```

### Example 4: State Preservation with Collapse/Expand

Maintain collapse state across app sessions:

```csharp
public class AppViewModel
{
    private SfGridSplitter gridSplitter;
    
    // Load app settings
    public async Task LoadSettings()
    {
        bool sidebarCollapsed = Preferences.Get("sidebar_collapsed", false);
        
        if (sidebarCollapsed && gridSplitter?.SplitterPanes.Count > 0)
        {
            gridSplitter.CollapsePane(0);
        }
    }
    
    // Save app settings
    public void SaveSettings()
    {
        if (gridSplitter?.SplitterPanes.Count > 0)
        {
            bool sidebarCollapsed = gridSplitter.SplitterPanes[0].IsCollapsed;
            Preferences.Set("sidebar_collapsed", sidebarCollapsed);
        }
    }
}
```

---

**Next:** Learn about [runtime pane management](runtime-pane-management.md) for dynamic pane creation and removal.
