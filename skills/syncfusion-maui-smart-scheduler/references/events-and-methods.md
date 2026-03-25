# Events and Methods

## Table of Contents
- [Overview](#overview)
- [AssistAppointmentResponseCompleted Event](#assistappointmentresponsecompleted-event)
- [Event Arguments](#event-arguments)
- [Manual vs Automatic Handling](#manual-vs-automatic-handling)
- [Methods](#methods)
- [Common Scenarios](#common-scenarios)
- [Best Practices](#best-practices)

---

## Overview

The SfSmartScheduler provides events and methods for programmatic control over the assist view and appointment handling. This allows you to:
- Intercept AI-generated appointments before they're added
- Implement custom validation logic
- Control assist view visibility programmatically
- Reset assist view state
- Handle appointment actions manually

**Key event:**
- `AssistAppointmentResponseCompleted` - Fires when AI creates/modifies appointments

**Key methods:**
- `ResetAssistView()` - Reset assist view to initial state
- `CloseAssistView()` - Close assist view
- `OpenAssistView()` - Open assist view

---

## AssistAppointmentResponseCompleted Event

### Event Overview

The `AssistAppointmentResponseCompleted` event fires whenever the AI completes processing a user's natural language request that results in an appointment action (add, edit, or delete).

**When it fires:**
- After AI interprets user's natural language input
- Before the appointment is automatically applied (if not handled)
- For Create, Update, and Delete operations

**Use cases:**
- Custom validation before adding appointments
- Logging appointment changes
- Triggering external integrations
- Implementing approval workflows
- Custom business logic

### Subscribing to the Event

**XAML:**
```xml
<smartScheduler:SfSmartScheduler x:Name="smartScheduler" 
                                 AssistAppointmentResponseCompleted="OnAssistAppointmentResponseCompleted"/>
```

**C#:**
```csharp
smartScheduler.AssistAppointmentResponseCompleted += OnAssistAppointmentResponseCompleted;
```

### Event Handler Signature

```csharp
private void OnAssistAppointmentResponseCompleted(
    object sender, 
    AssistAppointmentResponseCompletedEventArgs e)
{
    // Access event data through 'e'
    var appointment = e.Appointment;
    var action = e.Action;
    var response = e.AssistantResponse;
    
    // Set e.Handled = true to prevent automatic handling
    e.Handled = false; // Default: let scheduler handle automatically
}
```

---

## Event Arguments

The `AssistAppointmentResponseCompletedEventArgs` provides detailed information about the AI-generated appointment action.

### Properties

#### Appointment
**Type:** `SchedulerAppointment`  
**Description:** The appointment object created or modified by the AI

```csharp
private void OnAssistAppointmentResponseCompleted(object sender, AssistAppointmentResponseCompletedEventArgs e)
{
    var appointment = e.Appointment;
    
    if (appointment != null)
    {
        string subject = appointment.Subject;
        DateTime startTime = appointment.StartTime;
        DateTime endTime = appointment.EndTime;
        string notes = appointment.Notes;
        // ... access other appointment properties
    }
}
```

#### AssistantResponse
**Type:** `string`  
**Description:** The AI's text response describing what it did

```csharp
private void OnAssistAppointmentResponseCompleted(object sender, AssistAppointmentResponseCompletedEventArgs e)
{
    string aiResponse = e.AssistantResponse;
    // Example: "I've scheduled 'Team Meeting' for tomorrow at 2 PM"
    
    // Display to user or log
    Console.WriteLine($"AI Response: {aiResponse}");
}
```

#### Action
**Type:** `AppointmentAction` (enum)  
**Description:** The type of action performed  
**Values:** `Add`, `Edit`, `Delete`

```csharp
private void OnAssistAppointmentResponseCompleted(object sender, AssistAppointmentResponseCompletedEventArgs e)
{
    switch (e.Action)
    {
        case AppointmentAction.Add:
            // Appointment was created
            Console.WriteLine("New appointment added");
            break;
            
        case AppointmentAction.Edit:
            // Appointment was modified
            Console.WriteLine("Appointment updated");
            break;
            
        case AppointmentAction.Delete:
            // Appointment was removed
            Console.WriteLine("Appointment deleted");
            break;
    }
}
```

#### Handled
**Type:** `bool`  
**Description:** Controls whether the scheduler automatically applies the action

```csharp
private void OnAssistAppointmentResponseCompleted(object sender, AssistAppointmentResponseCompletedEventArgs e)
{
    // Set to true to handle the appointment manually
    e.Handled = true;
    
    // Now you must manually add/update/delete the appointment
    if (e.Action == AppointmentAction.Add)
    {
        // Custom logic here
        // Then manually add to scheduler if validated
    }
}
```

---

## Manual vs Automatic Handling

### Automatic Handling (Default)

By default, `e.Handled = false`, so the scheduler automatically applies the AI's action:

```csharp
private void OnAssistAppointmentResponseCompleted(object sender, AssistAppointmentResponseCompletedEventArgs e)
{
    // Log the action but let scheduler handle it automatically
    Console.WriteLine($"AI Action: {e.Action}, Subject: {e.Appointment?.Subject}");
    
    // e.Handled = false (default) - scheduler applies changes automatically
}
```

**Use automatic handling when:**
- You trust the AI's decisions
- No validation is needed
- You only want to log or observe actions

### Manual Handling

Set `e.Handled = true` to take control:

```csharp
private void OnAssistAppointmentResponseCompleted(object sender, AssistAppointmentResponseCompletedEventArgs e)
{
    e.Handled = true; // Prevent automatic handling
    
    if (e.Action == AppointmentAction.Add)
    {
        // Custom validation
        if (IsAppointmentValid(e.Appointment))
        {
            // Manually add to scheduler
            AddAppointmentToScheduler(e.Appointment);
        }
        else
        {
            // Reject and notify user
            DisplayAlert("Error", "Appointment validation failed", "OK");
        }
    }
}
```

**Use manual handling when:**
- Custom validation is required
- Approval workflows are needed
- Integration with external systems is necessary
- Business rules must be enforced

---

## Methods

### ResetAssistView()

Resets the assist view to its initial state, clearing conversation history and input.

**Signature:**
```csharp
public void ResetAssistView()
```

**XAML:**
```xml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition/>
        <RowDefinition Height="50"/>
    </Grid.RowDefinitions>
    
    <smartScheduler:SfSmartScheduler x:Name="smartScheduler"/>
    
    <Button Grid.Row="1" 
            Text="Reset Assistant" 
            Clicked="ResetButton_Clicked"/>
</Grid>
```

**C#:**
```csharp
private void ResetButton_Clicked(object sender, EventArgs e)
{
    smartScheduler.ResetAssistView();
}
```

**Use cases:**
- Clear conversation history
- Start fresh after errors
- Reset after completing a workflow
- Provide "New Conversation" functionality

### CloseAssistView()

Closes the assist view programmatically.

**Signature:**
```csharp
public void CloseAssistView()
```

**XAML:**
```xml
<Grid>
    <smartScheduler:SfSmartScheduler x:Name="smartScheduler"/>
    
    <Button Text="Close Assistant" 
            Clicked="CloseButton_Clicked"
            HorizontalOptions="End"
            VerticalOptions="Start"
            Margin="10"/>
</Grid>
```

**C#:**
```csharp
private void CloseButton_Clicked(object sender, EventArgs e)
{
    smartScheduler.CloseAssistView();
}
```

**Use cases:**
- Close after successful appointment creation
- Dismiss after user completes task
- Close programmatically after timeout
- Implement custom close logic

### OpenAssistView()

Opens the assist view programmatically.

**Signature:**
```csharp
public void OpenAssistView()
```

**Example:**
```csharp
private void QuickScheduleButton_Clicked(object sender, EventArgs e)
{
    smartScheduler.OpenAssistView();
    
    // Optionally pre-fill with context
    // (depends on API support for programmatic input)
}
```

**Use cases:**
- Provide alternative entry points to assist view
- Open from menu or toolbar
- Auto-open on specific conditions
- Guided workflows that start with AI assistant

---

## Common Scenarios

### Scenario 1: Custom Validation

Validate appointments before adding them:

```csharp
private void OnAssistAppointmentResponseCompleted(object sender, AssistAppointmentResponseCompletedEventArgs e)
{
    if (e.Action == AppointmentAction.Add)
    {
        e.Handled = true; // Take control
        
        var appointment = e.Appointment;
        
        // Validation 1: Check if subject contains required keyword
        if (!appointment.Subject.Contains("Important") && IsHighPriorityTime(appointment.StartTime))
        {
            DisplayAlert("Validation Error", 
                "High-priority time slots require 'Important' keyword in subject", 
                "OK");
            return;
        }
        
        // Validation 2: Check duration
        var duration = appointment.EndTime - appointment.StartTime;
        if (duration.TotalHours > 4)
        {
            DisplayAlert("Validation Error", 
                "Meetings cannot exceed 4 hours. Please split into multiple sessions.", 
                "OK");
            return;
        }
        
        // Validation 3: Check resource availability (custom logic)
        if (!IsResourceAvailable(appointment))
        {
            DisplayAlert("Resource Unavailable", 
                "The requested resource is not available. Please choose a different time.", 
                "OK");
            return;
        }
        
        // All validations passed - manually add
        AddAppointmentToScheduler(appointment);
        DisplayAlert("Success", $"'{appointment.Subject}' scheduled successfully", "OK");
    }
}

private bool IsHighPriorityTime(DateTime time)
{
    // Example: 9am-11am are high-priority slots
    return time.Hour >= 9 && time.Hour < 11;
}

private bool IsResourceAvailable(SchedulerAppointment appointment)
{
    // Custom resource checking logic
    return true; // Simplified
}

private void AddAppointmentToScheduler(SchedulerAppointment appointment)
{
    // Add to scheduler's appointment collection
    // Implementation depends on your data source setup
}
```

### Scenario 2: Approval Workflow

Require manager approval for certain appointments:

```csharp
private void OnAssistAppointmentResponseCompleted(object sender, AssistAppointmentResponseCompletedEventArgs e)
{
    if (e.Action == AppointmentAction.Add)
    {
        var appointment = e.Appointment;
        
        // Check if approval is needed
        if (RequiresApproval(appointment))
        {
            e.Handled = true; // Prevent automatic addition
            
            // Store for pending approval
            AddToPendingApprovals(appointment);
            
            // Notify user
            DisplayAlert("Approval Required", 
                $"'{appointment.Subject}' requires manager approval. You'll be notified once approved.", 
                "OK");
            
            // Trigger approval workflow (email, notification, etc.)
            SendApprovalRequest(appointment);
        }
        // else: Let scheduler handle automatically (e.Handled remains false)
    }
}

private bool RequiresApproval(SchedulerAppointment appointment)
{
    // Example: Meetings with external clients or over 2 hours need approval
    return appointment.Subject.Contains("Client") || 
           (appointment.EndTime - appointment.StartTime).TotalHours > 2;
}

private void AddToPendingApprovals(SchedulerAppointment appointment)
{
    // Store in database or collection for approval queue
}

private void SendApprovalRequest(SchedulerAppointment appointment)
{
    // Send email, push notification, or other alert to manager
}
```

### Scenario 3: Logging and Analytics

Log all AI-generated appointments for analytics:

```csharp
private void OnAssistAppointmentResponseCompleted(object sender, AssistAppointmentResponseCompletedEventArgs e)
{
    // Log to analytics service
    LogAppointmentAction(e);
    
    // Log to file or database
    SaveAppointmentLog(e);
    
    // Update metrics
    UpdateSchedulingMetrics(e);
    
    // Let scheduler handle automatically (e.Handled = false by default)
}

private void LogAppointmentAction(AssistAppointmentResponseCompletedEventArgs e)
{
    var logEntry = new
    {
        Timestamp = DateTime.Now,
        Action = e.Action.ToString(),
        Subject = e.Appointment?.Subject,
        StartTime = e.Appointment?.StartTime,
        AIResponse = e.AssistantResponse,
        UserId = GetCurrentUserId()
    };
    
    // Send to analytics service
    AnalyticsService.Track("AppointmentAction", logEntry);
}

private void SaveAppointmentLog(AssistAppointmentResponseCompletedEventArgs e)
{
    // Save to database for audit trail
    var logEntry = $"{DateTime.Now}: {e.Action} - {e.Appointment?.Subject}";
    File.AppendAllText("appointment_log.txt", logEntry + Environment.NewLine);
}

private void UpdateSchedulingMetrics(AssistAppointmentResponseCompletedEventArgs e)
{
    // Update dashboard metrics
    if (e.Action == AppointmentAction.Add)
    {
        IncrementAppointmentCount();
    }
}
```

### Scenario 4: Integration with External Systems

Sync appointments with external calendar or CRM:

```csharp
private async void OnAssistAppointmentResponseCompleted(object sender, AssistAppointmentResponseCompletedEventArgs e)
{
    if (e.Action == AppointmentAction.Add)
    {
        e.Handled = true; // Handle manually to ensure sync completes
        
        try
        {
            // Add to Syncfusion scheduler first
            AddAppointmentToScheduler(e.Appointment);
            
            // Sync to external systems
            await SyncToGoogleCalendar(e.Appointment);
            await SyncToCRM(e.Appointment);
            await SendCalendarInvites(e.Appointment);
            
            // Notify user of success
            DisplayAlert("Success", 
                $"'{e.Appointment.Subject}' scheduled and synced to all calendars", 
                "OK");
        }
        catch (Exception ex)
        {
            DisplayAlert("Sync Error", 
                $"Appointment created but sync failed: {ex.Message}", 
                "OK");
            
            // Log error
            LogError(ex);
        }
    }
}

private async Task SyncToGoogleCalendar(SchedulerAppointment appointment)
{
    // Implementation for Google Calendar API sync
    await Task.Delay(100); // Simulated API call
}

private async Task SyncToCRM(SchedulerAppointment appointment)
{
    // Implementation for CRM integration
    await Task.Delay(100); // Simulated API call
}

private async Task SendCalendarInvites(SchedulerAppointment appointment)
{
    // Send invites to attendees
    await Task.Delay(100); // Simulated email send
}
```

### Scenario 5: Conditional Auto-Close

Close assist view automatically after successful appointment creation:

```csharp
private void OnAssistAppointmentResponseCompleted(object sender, AssistAppointmentResponseCompletedEventArgs e)
{
    if (e.Action == AppointmentAction.Add && e.Appointment != null)
    {
        // Show confirmation
        DisplayAlert("Success", $"'{e.Appointment.Subject}' scheduled successfully", "OK");
        
        // Auto-close assist view after short delay
        Device.StartTimer(TimeSpan.FromSeconds(1.5), () =>
        {
            smartScheduler.CloseAssistView();
            return false; // Stop timer
        });
    }
}
```

### Scenario 6: Reset After Error

Reset assist view if AI returns invalid response:

```csharp
private void OnAssistAppointmentResponseCompleted(object sender, AssistAppointmentResponseCompletedEventArgs e)
{
    if (e.Appointment == null || string.IsNullOrEmpty(e.Appointment.Subject))
    {
        // Invalid response
        DisplayAlert("Error", 
            "Could not understand your request. Please try rephrasing.", 
            "OK");
        
        // Reset to clear bad state
        smartScheduler.ResetAssistView();
    }
}
```

---

## Best Practices

### 1. Always Check for Null

AI-generated appointments may be null in error scenarios:

```csharp
private void OnAssistAppointmentResponseCompleted(object sender, AssistAppointmentResponseCompletedEventArgs e)
{
    if (e.Appointment == null)
    {
        Console.WriteLine("AI returned null appointment");
        return;
    }
    
    // Safe to access appointment properties
    var subject = e.Appointment.Subject;
}
```

### 2. Provide User Feedback

Always inform users about the outcome:

```csharp
private void OnAssistAppointmentResponseCompleted(object sender, AssistAppointmentResponseCompletedEventArgs e)
{
    switch (e.Action)
    {
        case AppointmentAction.Add:
            DisplayAlert("Success", $"Scheduled: {e.Appointment.Subject}", "OK");
            break;
        case AppointmentAction.Edit:
            DisplayAlert("Updated", $"Modified: {e.Appointment.Subject}", "OK");
            break;
        case AppointmentAction.Delete:
            DisplayAlert("Deleted", $"Removed: {e.Appointment.Subject}", "OK");
            break;
    }
}
```

### 3. Handle Exceptions

Wrap custom logic in try-catch:

```csharp
private void OnAssistAppointmentResponseCompleted(object sender, AssistAppointmentResponseCompletedEventArgs e)
{
    try
    {
        // Custom validation or processing
        if (e.Action == AppointmentAction.Add)
        {
            e.Handled = true;
            ValidateAndAddAppointment(e.Appointment);
        }
    }
    catch (Exception ex)
    {
        DisplayAlert("Error", $"Failed to process appointment: {ex.Message}", "OK");
        LogError(ex);
    }
}
```

### 4. Keep Event Handlers Lightweight

Avoid heavy processing in event handlers:

```csharp
private async void OnAssistAppointmentResponseCompleted(object sender, AssistAppointmentResponseCompletedEventArgs e)
{
    // Quick validation
    if (!QuickValidate(e.Appointment))
    {
        e.Handled = true;
        return;
    }
    
    // Offload heavy processing to background
    if (NeedsExternalSync(e.Appointment))
    {
        await Task.Run(() => SyncToExternalSystems(e.Appointment));
    }
}
```

### 5. Document Custom Logic

Add comments explaining why manual handling is needed:

```csharp
private void OnAssistAppointmentResponseCompleted(object sender, AssistAppointmentResponseCompletedEventArgs e)
{
    // Manual handling required for compliance with corporate policy:
    // - All client meetings must be approved by manager
    // - High-value meetings (>$10k) trigger CRM workflow
    // - External attendees require security clearance check
    
    if (e.Action == AppointmentAction.Add)
    {
        e.Handled = true;
        ApplyCorporatePolicy(e.Appointment);
    }
}
```

---

## Troubleshooting

**Issue:** Event not firing  
**Solution:** Ensure event handler is subscribed before using assist view

**Issue:** `e.Appointment` is null  
**Solution:** Check AI service configuration and test with simple prompts

**Issue:** Changes not appearing in scheduler  
**Solution:** If `e.Handled = true`, you must manually add/update appointments

**Issue:** Methods (Reset/Close/Open) not working  
**Solution:** Verify you're calling methods on the correct `SfSmartScheduler` instance

---

## Next Steps

- **Style your scheduler:** [styling.md](styling.md)
- **Customize assist view:** [assist-view-customization.md](assist-view-customization.md)
