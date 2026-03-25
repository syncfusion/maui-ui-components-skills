# Appointment Interactions in .NET MAUI Scheduler

## Table of Contents

- [Overview](#overview)
- [Drag and Drop](#drag-and-drop)
  - [Enable Drag and Drop](#enable-drag-and-drop)
  - [Drag Events](#drag-events)
  - [Drag Settings](#drag-settings)
- [Appointment Resizing](#appointment-resizing)
  - [Enable Resizing](#enable-resizing)
  - [Resize Settings](#resize-settings)
  - [Resize Events](#resize-events)
- [Appointment Editor](#appointment-editor)
  - [Enable Editor](#enable-editor)
  - [Editor Modes](#editor-modes)
  - [Adding Appointments](#adding-appointments)
  - [Editing Appointments](#editing-appointments)
  - [Editor Events](#editor-events)
- [Appointment Tooltip](#appointment-tooltip)
  - [Enable Tooltip](#enable-tooltip)
  - [Tooltip Settings](#tooltip-settings)
  - [Custom Tooltip Template](#custom-tooltip-template)
- [Cell Selection](#cell-selection)
  - [Selection Appearance](#selection-appearance)
  - [Custom Selection Template](#custom-selection-template)
- [Complete Examples](#complete-examples)
- [Troubleshooting](#troubleshooting)

## Overview

The .NET MAUI Scheduler provides rich interactive capabilities for managing appointments, including drag-and-drop rescheduling, appointment resizing, built-in appointment editors, contextual tooltips, and cell selection customization. These features enable users to interact with appointments naturally and efficiently.

## Drag and Drop

### Enable Drag and Drop

Appointments can be rescheduled using drag-and-drop operations. Enable this feature by setting the `AllowAppointmentDrag` property to `true`. By default, this property is set to `true`.

```xml
<scheduler:SfScheduler x:Name="scheduler"
                       View="Week" 
                       AllowAppointmentDrag="true">        
</scheduler:SfScheduler>
```

```csharp
this.scheduler.AllowAppointmentDrag = true;
```

**Disable drag and drop:**

```csharp
this.scheduler.AllowAppointmentDrag = false;
```

**Important:** Your business object class must inherit from `INotifyPropertyChanged` for dynamic changes to work correctly during drag-and-drop operations.

### Drag Events

#### AppointmentDragStarting Event

Triggered when an appointment starts being dragged. Use this event to validate whether the appointment can be dragged and optionally cancel the operation.

```csharp
scheduler.AppointmentDragStarting += OnSchedulerAppointmentDragStarting;

private void OnSchedulerAppointmentDragStarting(object? sender, AppointmentDragStartingEventArgs e)
{
    var appointment = e.Appointment;
    var resource = e.Resource; // Resource under which appointment is located
    
    // Cancel drag operation if needed
    e.Cancel = false;
}
```

**Example: Prevent all-day appointments from being dragged**

```csharp
private void OnSchedulerAppointmentDragStarting(object? sender, AppointmentDragStartingEventArgs e)
{
    var appointment = e.Appointment as SchedulerAppointment;
    if (appointment.IsAllDay)
    {
        e.Cancel = true; // Prevent all-day appointments from being dragged
    }
}
```

#### AppointmentDragOver Event

Triggered continuously while the appointment is being dragged. Use this to monitor the drag position and time.

```csharp
scheduler.AppointmentDragOver += OnSchedulerAppointmentDragOver;

private void OnSchedulerAppointmentDragOver(object? sender, AppointmentDragOverEventArgs e)
{
    var appointment = e.Appointment;
    var draggingPoint = e.DragPoint;      // Current drag position
    var draggingTime = e.DragTime;        // Time at drag position
    var dragResource = e.DragResource;     // Resource during drag
}
```

#### AppointmentDrop Event

Triggered when the appointment is dropped. Use this to validate the drop location and optionally cancel or modify the drop operation.

```csharp
scheduler.AppointmentDrop += OnSchedulerAppointmentDrop;

private void OnSchedulerAppointmentDrop(object? sender, AppointmentDropEventArgs e)
{
    var appointment = e.Appointment;
    var dropTime = e.DropTime;                    // Time where dropped (can be modified)
    var sourceResource = e.SourceResource;         // Original resource
    var targetResource = e.TargetResource;         // Target resource
    var isDroppingToAllDay = e.IsDroppingToAllDay; // Dropping to all-day panel?
    
    // Cancel drop if needed
    e.Cancel = false;
}
```

**Example: Prevent dropping into all-day panel**

```csharp
private void OnSchedulerAppointmentDrop(object? sender, AppointmentDropEventArgs e)
{
    if (e.IsDroppingToAllDay)
    {
        e.Cancel = true;
    }
}
```

### Drag Settings

Configure drag-and-drop behavior using the `DragDropSettings` property:

```xml
<scheduler:SfScheduler x:Name="scheduler" View="Week">
    <scheduler:SfScheduler.DragDropSettings>
        <scheduler:DragDropSettings AllowNavigation="true" 
                                   AllowScroll="true" 
                                   ShowTimeIndicator="true"
                                   AutoNavigationDelay="00:00:02"/>
    </scheduler:SfScheduler.DragDropSettings>
</scheduler:SfScheduler>
```

```csharp
scheduler.DragDropSettings = new DragDropSettings 
{
    AllowNavigation = true,      // Navigate views while dragging
    AllowScroll = true,          // Scroll timeslots while dragging
    ShowTimeIndicator = true,    // Show time indicator during drag
    AutoNavigationDelay = new TimeSpan(0, 0, 2) // Navigation delay
};
```

#### AllowNavigation

Controls whether views can be navigated while dragging an appointment to the edge of the current view. Default is `true`.

```csharp
scheduler.DragDropSettings.AllowNavigation = false;
```

#### AllowScroll

Controls whether timeslots scroll automatically when dragging to the edge of the view. Default is `true`. Not applicable for Month view.

```csharp
scheduler.DragDropSettings.AllowScroll = false;
```

#### ShowTimeIndicator

Controls whether the drag time indicator is displayed. Default is `true`. Requires `TimeRulerWidth` > 0 for Day/Week views or `TimeRulerHeight` > 0 for Timeline views. Not applicable for Month and Timeline Month views.

```csharp
scheduler.DragDropSettings.ShowTimeIndicator = false;
```

#### Customize Time Indicator

**Style:**

```xml
<scheduler:SfScheduler.DragDropSettings>
    <scheduler:DragDropSettings 
        TimeIndicatorStyle="{scheduler:SchedulerTextStyle TextColor=Green}"/>
</scheduler:SfScheduler.DragDropSettings>
```

```csharp
scheduler.DragDropSettings.TimeIndicatorStyle = new SchedulerTextStyle()
{
    TextColor = Colors.Green,
    FontSize = 14,
    FontAttributes = FontAttributes.Bold
};
```

**Format:**

```csharp
scheduler.DragDropSettings.TimeIndicatorTextFormat = "hh:mm";
```

## Appointment Resizing

### Enable Resizing

Appointments can be resized interactively to adjust their duration. By default, `AllowAppointmentResize` is `false`. Enable it to allow resizing:

```xml
<scheduler:SfScheduler x:Name="scheduler" 
                       View="Day" 
                       AllowAppointmentResize="true">
</scheduler:SfScheduler>
```

```csharp
scheduler.AllowAppointmentResize = true;
```

**Resizing behavior by view:**
- **Day/Week/WorkWeek**: Drag top or bottom edges
- **Month/Timeline**: Drag left or right edges

**Note:** Appointment resizing is supported only on desktop platforms using mouse interactions.

### Resize Settings

Configure resize behavior using `AppointmentResizeSettings`:

```xml
<scheduler:SfScheduler x:Name="scheduler" 
                       View="Day" 
                       AllowAppointmentResize="true">
    <scheduler:SfScheduler.AppointmentResizeSettings>
        <scheduler:AppointmentResizeSettings AllowResizeScroll="true"
                                            ShowTimeIndicator="true"
                                            TimeIndicatorTextFormat="HH:mm"
                                            ResizeBorderThickness="5"
                                            ResizeBorderStroke="Red"/>
    </scheduler:SfScheduler.AppointmentResizeSettings>
</scheduler:SfScheduler>
```

```csharp
scheduler.AppointmentResizeSettings = new AppointmentResizeSettings
{
    AllowResizeScroll = true,          // Auto-scroll during resize
    ShowTimeIndicator = true,          // Show time indicator
    TimeIndicatorTextFormat = "HH:mm", // Time format
    ResizeBorderThickness = 5,         // Border thickness
    ResizeBorderStroke = Colors.Red    // Border color
};
```

#### AllowResizeScroll

Enables automatic scrolling when resizing reaches view boundaries. Default is `true`.

```csharp
scheduler.AppointmentResizeSettings.AllowResizeScroll = false;
```

#### ShowTimeIndicator

Shows time indicator during resize. Default is `true`. Not displayed in Month view, All-Day layout, or Timeline Month view.

```csharp
scheduler.AppointmentResizeSettings.ShowTimeIndicator = false;
```

#### Customize Time Indicator Style

```xml
<scheduler:AppointmentResizeSettings>
    <scheduler:AppointmentResizeSettings.TimeIndicatorStyle>
        <scheduler:SchedulerTextStyle TextColor="Green" 
                                     FontSize="15" 
                                     FontAttributes="Bold" 
                                     FontFamily="OpenSansSemibold"/>
    </scheduler:AppointmentResizeSettings.TimeIndicatorStyle>
</scheduler:AppointmentResizeSettings>
```

```csharp
scheduler.AppointmentResizeSettings.TimeIndicatorStyle = new SchedulerTextStyle() 
{
    TextColor = Colors.Green,
    FontSize = 15,
    FontAttributes = FontAttributes.Bold,
    FontFamily = "OpenSansSemibold"
};
```

### Resize Events

#### AppointmentResizeStart Event

Triggered when resize operation begins. Use to validate or cancel the resize.

```csharp
scheduler.AppointmentResizeStart += Scheduler_AppointmentResizeStart;

private void Scheduler_AppointmentResizeStart(object sender, AppointmentResizeStartEventArgs e)
{
    var appointment = e.Appointment;
    var resource = e.Resource;
    var resizeEdge = e.ResizeEdge; // Top, Bottom, Left, Right
    
    // Cancel resize if needed
    e.Cancel = false;
}
```

#### AppointmentResizing Event

Triggered continuously during resize. Use to monitor or validate during the operation.

```csharp
scheduler.AppointmentResizing += Scheduler_AppointmentResizing;

private void Scheduler_AppointmentResizing(object sender, AppointmentResizingEventArgs e)
{
    var appointment = e.Appointment;
    var resource = e.Resource;
    var resizeEdge = e.ResizeEdge;
    var resizingTime = e.ResizingTime; // Current resize position time
    
    // Cancel during resize if needed
    e.Cancel = false;
}
```

#### AppointmentResizeEnd Event

Triggered when resize operation completes. Use to validate final size or cancel the update.

```csharp
scheduler.AppointmentResizeEnd += Scheduler_AppointmentResizeEnd;

private void Scheduler_AppointmentResizeEnd(object sender, AppointmentResizeEndEventArgs e)
{
    var appointment = e.Appointment;
    var resource = e.Resource;
    var resizeEdge = e.ResizeEdge;
    var resizedTime = e.ResizedTime; // Final time (can be modified)
    
    // Modify the final time if needed
    // e.ResizedTime = newDateTime;
    
    // Cancel the resize update
    e.Cancel = false;
}
```

## Appointment Editor

### Enable Editor

The built-in appointment editor allows users to create, edit, and delete appointments through a popup dialog. Control editor availability using `AppointmentEditorMode`:

```xml
<scheduler:SfScheduler x:Name="scheduler" 
                       View="Day" 
                       AppointmentEditorMode="Add,Edit">
</scheduler:SfScheduler>
```

```csharp
scheduler.AppointmentEditorMode = AppointmentEditorMode.Add | AppointmentEditorMode.Edit;
```

### Editor Modes

- **None**: Disables the editor entirely (default)
- **Add**: Allows creating new appointments
- **Edit**: Allows modifying existing appointments
- **Add,Edit**: Enables both modes

### Adding Appointments

Enable `Add` mode to create appointments by double-tapping time slots:

```csharp
scheduler.AppointmentEditorMode = AppointmentEditorMode.Add;
```

**User workflow:**
1. Double-tap an empty time slot
2. Editor opens with pre-filled date/time
3. Enter appointment details
4. Tap Save to create the appointment

### Editing Appointments

Enable `Edit` mode to modify or delete existing appointments:

```csharp
scheduler.AppointmentEditorMode = AppointmentEditorMode.Edit;
```

**User workflow:**
1. Double-tap an existing appointment
2. Dialog shows appointment details with Edit/Delete options
3. Select Edit to open editor with current details
4. Modify and tap Save, or tap Cancel to discard

#### Editing Recurring Appointments

When editing a recurring appointment, a dialog prompts:
- Edit entire series, or
- Edit only this occurrence

After selection, the editor opens with corresponding details.

### Editor Events

#### AppointmentEditorOpening Event

Triggered before the editor dialog appears. Use to validate or cancel editor opening.

```csharp
scheduler.AppointmentEditorOpening += Scheduler_AppointmentEditorOpening;

private void Scheduler_AppointmentEditorOpening(object? sender, AppointmentEditorOpeningEventArgs e)
{
    var appointment = e.Appointment;  // Null when creating new
    var dateTime = e.DateTime;         // Selected time slot
    var resource = e.Resource;         // Associated resource
    var editMode = e.RecurringAppointmentEditMode; // Edit mode for recurring
    
    // Cancel editor opening if needed
    e.Cancel = false;
}
```

#### AppointmentEditorClosing Event

Triggered when the editor is about to close. Use to handle or cancel the action.

```csharp
scheduler.AppointmentEditorClosing += Scheduler_AppointmentEditorClosing;

private void Scheduler_AppointmentEditorClosing(object? sender, AppointmentEditorClosingEventArgs e)
{
    var action = e.Action;         // Add, Edit, Delete, Cancel
    var appointment = e.Appointment;
    var resources = e.Resources;    // Assigned resources
    var handled = e.Handled;        // Set true to handle action manually
    
    // Cancel editor closing if needed
    e.Cancel = false;
}
```

**Actions:** `Add`, `Edit`, `Delete`, `Cancel`

#### RecurringAppointmentBeginningEdit Event

Triggered when editing or deleting a recurring appointment. Control how the recurring appointment is modified.

```csharp
scheduler.RecurringAppointmentBeginningEdit += Scheduler_RecurringAppointmentBeginningEdit;

private void Scheduler_RecurringAppointmentBeginningEdit(object? sender, RecurringAppointmentBeginningEditEventArgs e)
{
    // Set edit mode
    e.EditMode = RecurringAppointmentEditMode.Series;
}
```

**EditMode options:**
- **User**: Show dialog to prompt user
- **Occurrence**: Edit only selected occurrence
- **Series**: Edit entire series

## Appointment Tooltip

### Enable Tooltip

Display contextual appointment details on hover (desktop) or tap (mobile):

```xml
<scheduler:SfScheduler x:Name="scheduler" 
                       View="Day" 
                       IsAppointmentToolTipEnabled="true">
</scheduler:SfScheduler>
```

```csharp
scheduler.IsAppointmentToolTipEnabled = true;
```

**Platform behavior:**
- **Desktop**: Hover mouse over appointment
- **Mobile**: Tap or long-press appointment (long-press only works when dragging is disabled)

### Tooltip Settings

Customize tooltip appearance using `AppointmentToolTipSettings`:

```xml
<scheduler:SfScheduler IsAppointmentToolTipEnabled="true">
    <scheduler:SfScheduler.AppointmentToolTipSettings>
        <scheduler:AppointmentToolTipSettings Background="PaleGreen" 
                                             Padding="5" 
                                             ToolTipPosition="Right">
            <scheduler:AppointmentToolTipSettings.TextStyle>
                <scheduler:SchedulerTextStyle TextColor="Purple" 
                                             FontSize="15" 
                                             FontAttributes="Bold"/>
            </scheduler:AppointmentToolTipSettings.TextStyle>
        </scheduler:AppointmentToolTipSettings>
    </scheduler:SfScheduler.AppointmentToolTipSettings>
</scheduler:SfScheduler>
```

```csharp
scheduler.AppointmentToolTipSettings = new AppointmentToolTipSettings()
{
    Background = Colors.PaleGreen,
    Padding = new Thickness(5),
    ToolTipPosition = SchedulerToolTipPosition.Right,
    TextStyle = new SchedulerTextStyle
    {
        TextColor = Colors.Purple,
        FontSize = 15,
        FontAttributes = FontAttributes.Bold
    }
};
```

**ToolTipPosition options:** Auto (default), Left, Right, Top, Bottom

### Custom Tooltip Template

Create custom tooltip layouts using `AppointmentToolTipTemplate`:

```xml
<scheduler:SfScheduler IsAppointmentToolTipEnabled="true">
    <scheduler:SfScheduler.AppointmentToolTipSettings>
        <scheduler:AppointmentToolTipSettings ToolTipPosition="Left"/>
    </scheduler:SfScheduler.AppointmentToolTipSettings>

    <scheduler:SfScheduler.AppointmentToolTipTemplate>
        <DataTemplate x:DataType="scheduler:SchedulerAppointment">
            <Grid ColumnDefinitions="Auto,*">
                <BoxView Grid.Column="0"
                         Background="{Binding Background}"
                         WidthRequest="10"
                         VerticalOptions="Fill"/>

                <VerticalStackLayout Grid.Column="1" Spacing="5" Margin="5,0,0,0">
                    <Label Text="{Binding Subject}"
                           FontAttributes="Bold"
                           FontSize="14"/>
                    
                    <Label Text="{Binding StartTime, StringFormat='Start: {0:g}'}"
                           FontSize="12"/>
                    
                    <Label Text="{Binding EndTime, StringFormat='End: {0:g}'}"
                           FontSize="12"/>
                    
                    <Label Text="{Binding Location}"
                           FontSize="12"
                           IsVisible="{Binding Location, Converter={StaticResource NotNullConverter}}"/>
                </VerticalStackLayout>
            </Grid>
        </DataTemplate>
    </scheduler:SfScheduler.AppointmentToolTipTemplate>
</scheduler:SfScheduler>
```

## Cell Selection

### Selection Appearance

Customize the appearance of selected cells using `CellSelectionView`:

```xml
<scheduler:SfScheduler x:Name="scheduler">
    <scheduler:SfScheduler.CellSelectionView>
        <scheduler:SchedulerCellSelectionView Stroke="Red"
                                              Background="LightCoral"
                                              StrokeWidth="2"
                                              CornerRadius="5"/>
    </scheduler:SfScheduler.CellSelectionView>
</scheduler:SfScheduler>
```

```csharp
scheduler.CellSelectionView = new SchedulerCellSelectionView
{
    Stroke = Colors.Red,
    Background = Colors.LightCoral,
    StrokeWidth = 2,
    CornerRadius = 5
};
```

**Properties:**
- **Stroke**: Border color (default has a value)
- **Background**: Fill color (default is `Transparent`)
- **StrokeWidth**: Border thickness
- **CornerRadius**: Rounded corners

**Note:** To show only background without border, set `Stroke` to `Transparent`.

### Custom Selection Template

Use a custom view for cell selection using `Template`:

```xml
<scheduler:SfScheduler.CellSelectionView>
    <scheduler:SchedulerCellSelectionView>
        <scheduler:SchedulerCellSelectionView.Template>
            <DataTemplate>
                <Button BackgroundColor="#FF9800"
                        Text="+ Add event"
                        TextColor="White"
                        CornerRadius="0"/>
            </DataTemplate>
        </scheduler:SchedulerCellSelectionView.Template>
    </scheduler:SchedulerCellSelectionView>
</scheduler:SfScheduler.CellSelectionView>
```

## Complete Examples

### Example 1: Full Interaction Setup

```xml
<scheduler:SfScheduler x:Name="scheduler"
                       View="Week"
                       AllowAppointmentDrag="true"
                       AllowAppointmentResize="true"
                       AppointmentEditorMode="Add,Edit"
                       IsAppointmentToolTipEnabled="true">
    
    <!-- Drag settings -->
    <scheduler:SfScheduler.DragDropSettings>
        <scheduler:DragDropSettings AllowNavigation="true" 
                                   AllowScroll="true" 
                                   ShowTimeIndicator="true"/>
    </scheduler:SfScheduler.DragDropSettings>
    
    <!-- Resize settings -->
    <scheduler:SfScheduler.AppointmentResizeSettings>
        <scheduler:AppointmentResizeSettings AllowResizeScroll="true"
                                            ShowTimeIndicator="true"
                                            ResizeBorderStroke="DodgerBlue"
                                            ResizeBorderThickness="3"/>
    </scheduler:SfScheduler.AppointmentResizeSettings>
    
    <!-- Tooltip settings -->
    <scheduler:SfScheduler.AppointmentToolTipSettings>
        <scheduler:AppointmentToolTipSettings Background="DarkSlateGray"
                                             ToolTipPosition="Auto">
            <scheduler:AppointmentToolTipSettings.TextStyle>
                <scheduler:SchedulerTextStyle TextColor="White" FontSize="14"/>
            </scheduler:AppointmentToolTipSettings.TextStyle>
        </scheduler:AppointmentToolTipSettings>
    </scheduler:SfScheduler.AppointmentToolTipSettings>
    
    <!-- Cell selection -->
    <scheduler:SfScheduler.CellSelectionView>
        <scheduler:SchedulerCellSelectionView Stroke="DodgerBlue"
                                              StrokeWidth="2"
                                              Background="LightBlue"
                                              CornerRadius="4"/>
    </scheduler:SfScheduler.CellSelectionView>
</scheduler:SfScheduler>
```

### Example 2: Validation During Interactions

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        
        scheduler.AppointmentDragStarting += OnDragStarting;
        scheduler.AppointmentDrop += OnDrop;
        scheduler.AppointmentResizeStart += OnResizeStart;
        scheduler.AppointmentEditorOpening += OnEditorOpening;
    }
    
    private void OnDragStarting(object? sender, AppointmentDragStartingEventArgs e)
    {
        // Prevent dragging read-only appointments
        var appointment = e.Appointment as SchedulerAppointment;
        if (appointment.IsReadOnly)
        {
            e.Cancel = true;
        }
    }
    
    private void OnDrop(object? sender, AppointmentDropEventArgs e)
    {
        // Prevent dropping into all-day panel
        if (e.IsDroppingToAllDay)
        {
            e.Cancel = true;
        }
        
        // Prevent dropping on weekends
        if (e.DropTime.DayOfWeek == DayOfWeek.Saturday || 
            e.DropTime.DayOfWeek == DayOfWeek.Sunday)
        {
            e.Cancel = true;
        }
    }
    
    private void OnResizeStart(object sender, AppointmentResizeStartEventArgs e)
    {
        // Prevent resizing appointments shorter than 30 minutes
        var appointment = e.Appointment as SchedulerAppointment;
        if ((appointment.EndTime - appointment.StartTime).TotalMinutes < 30)
        {
            e.Cancel = true;
        }
    }
    
    private void OnEditorOpening(object? sender, AppointmentEditorOpeningEventArgs e)
    {
        // Prevent editing appointments in the past
        if (e.DateTime < DateTime.Now)
        {
            e.Cancel = true;
        }
    }
}
```

## Troubleshooting

### Drag and Drop Issues

**Problem:** Drag and drop not working  
**Solution:** Verify `AllowAppointmentDrag="true"` and ensure business objects implement `INotifyPropertyChanged`

**Problem:** Time indicator not showing during drag  
**Solution:** Check `TimeRulerWidth` > 0 for Day/Week views or `TimeRulerHeight` > 0 for Timeline views

**Problem:** Cannot drag to different views  
**Solution:** Enable `DragDropSettings.AllowNavigation="true"`

### Resizing Issues

**Problem:** Resize not working  
**Solution:** Appointment resizing is desktop-only. Ensure `AllowAppointmentResize="true"` and test on Windows/macOS

**Problem:** Resize cursor not appearing  
**Solution:** Resizing requires mouse interaction with native cursors (not available on touch devices)

### Editor Issues

**Problem:** Double-tap not opening editor  
**Solution:** Verify `AppointmentEditorMode` includes `Add` (for time slots) or `Edit` (for appointments)

**Problem:** Changes not reflecting in data source  
**Solution:** Ensure `AppointmentsSource` is bound to `ObservableCollection` for automatic updates

### Tooltip Issues

**Problem:** Tooltip not showing on mobile  
**Solution:** On mobile, use tap or long-press. Long-press requires `AllowAppointmentDrag="false"`

**Problem:** Custom template not working  
**Solution:** Ensure `x:DataType="scheduler:SchedulerAppointment"` is set on DataTemplate

### Selection Issues

**Problem:** Selection not visible  
**Solution:** Check that both `Stroke` and `Background` are not `Transparent`. Default `Stroke` has a value, so set explicitly if needed

**Problem:** Custom template not rendering  
**Solution:** Verify `Template` property is set correctly and DataTemplate is valid

---

**Related References:**
- [Getting Started](getting-started.md) - Initial setup and configuration
- [Appointments](appointments.md) - Creating and managing appointments
- [Day, Week, and WorkWeek Views](day-week-views.md) - View configuration
- [Advanced Features](advanced-features.md) - Events and additional capabilities
