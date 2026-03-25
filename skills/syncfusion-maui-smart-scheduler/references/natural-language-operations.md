# Natural Language Operations

## Table of Contents
- [Overview](#overview)
- [Understanding Natural Language CRUD](#understanding-natural-language-crud)
- [Creating Appointments](#creating-appointments)
- [Updating Appointments](#updating-appointments)
- [Deleting Appointments](#deleting-appointments)
- [Querying Appointments](#querying-appointments)
- [Supported Patterns and Keywords](#supported-patterns-and-keywords)
- [Context-Aware Scheduling](#context-aware-scheduling)
- [Multi-Operation Commands](#multi-operation-commands)
- [Best Practices](#best-practices)

---

## Overview

The SfSmartScheduler's core strength is understanding natural language to perform CRUD operations (Create, Read, Update, Delete) without requiring structured forms. Users can type conversationally, and the AI interprets their intent, extracts scheduling details, and executes the appropriate action.

**Key capabilities:**
- Understand informal time expressions ("tomorrow", "next Friday", "in 2 hours")
- Parse dates in multiple formats ("March 25", "3/25/2026", "Friday")
- Extract subjects, locations, durations, and recurrence patterns
- Respect current scheduler context (view, resources, filters)
- Handle partial information with intelligent defaults

---

## Understanding Natural Language CRUD

### How It Works

1. **User Input:** User types natural language in assist view
2. **AI Processing:** AI service analyzes text and extracts:
   - Action (create, update, delete, query)
   - Subject/title
   - Date and time
   - Duration
   - Recurrence pattern
   - Resources/location
   - Additional context
3. **Validation:** SfSmartScheduler validates against current view and resources
4. **Execution:** Appointment is created, updated, or deleted
5. **Confirmation:** User sees result in scheduler and assist view

### Natural Language vs Traditional Forms

**Traditional Form:**
```
Title: [Team Meeting]
Date: [03/25/2026]
Start Time: [14:00]
End Time: [15:00]
Recurrence: [None] ▼
Location: [Conference Room A]
[Submit]
```

**Natural Language:**
```
"Schedule team meeting in conference room A on March 25th at 2pm for 1 hour"
```

The AI extracts all fields automatically, reducing clicks and making scheduling feel conversational.

---

## Creating Appointments

### Basic Appointment Creation

**Pattern:** "Schedule/Create/Add [subject] [when]"

**Examples:**

```
"Schedule team standup tomorrow at 9am"
```
Result: Creates appointment titled "Team Standup" tomorrow at 9:00 AM

```
"Add dentist appointment next Friday at 10am"
```
Result: Creates appointment titled "Dentist Appointment" next Friday at 10:00 AM

```
"Create project review on March 25th"
```
Result: Creates appointment titled "Project Review" on March 25th (uses default time or current time)

### With Duration

**Pattern:** "Schedule [subject] [when] for [duration]"

**Examples:**

```
"Schedule client call tomorrow at 2pm for 1 hour"
```
Result: Creates 1-hour appointment from 2:00 PM to 3:00 PM

```
"Add lunch break at noon for 30 minutes"
```
Result: Creates 30-minute appointment from 12:00 PM to 12:30 PM

```
"Create workshop on Friday from 10am to 4pm"
```
Result: Creates 6-hour appointment from 10:00 AM to 4:00 PM

### With Location/Resources

**Pattern:** "Schedule [subject] in/at [location] [when]"

**Examples:**

```
"Schedule team sync in conference room B tomorrow at 3pm"
```
Result: Creates appointment with resource "Conference Room B"

```
"Book meeting at downtown office next Monday"
```
Result: Creates appointment with location "Downtown Office"

```
"Schedule video call using Zoom on Wednesday at 11am"
```
Result: Creates appointment with resource reference to "Zoom"

### Recurring Appointments

**Pattern:** "Create [frequency] [subject] [when]"

**Examples:**

```
"Create weekly team meeting every Monday at 10am"
```
Result: Creates recurring appointment every Monday at 10:00 AM

```
"Schedule daily standup at 9am"
```
Result: Creates daily recurring appointment at 9:00 AM

```
"Add monthly review on the 15th at 2pm"
```
Result: Creates monthly recurring appointment on the 15th

```
"Create biweekly sprint planning every other Wednesday at 1pm"
```
Result: Creates appointment every two weeks on Wednesday at 1:00 PM

### With Multiple Attributes

**Complex Examples:**

```
"Schedule weekly team retrospective every Friday at 3pm in main conference room for 1 hour"
```
Result: Recurring appointment, weekly frequency, specific time, location, and duration

```
"Add quarterly business review on the last Friday of March at 2pm for 2 hours"
```
Result: Appointment with specific date calculation, time, and duration

```
"Create doctor appointment next Tuesday at 10:30am for 45 minutes at clinic"
```
Result: Specific time (including minutes), custom duration, location

---

## Updating Appointments

### Modifying Existing Appointments

**Pattern:** "Move/Reschedule/Change [subject] to [new when]"

**Examples:**

```
"Move team meeting to tomorrow at 3pm"
```
Result: Finds "Team Meeting" appointment and reschedules to tomorrow at 3:00 PM

```
"Reschedule dentist appointment to next week"
```
Result: Moves dentist appointment to same day/time next week

```
"Change project review to Friday afternoon"
```
Result: Moves appointment to Friday (uses default afternoon time or interprets from context)

### Extending or Shortening

**Pattern:** "Extend/Shorten [subject] by [duration]"

**Examples:**

```
"Extend client call by 30 minutes"
```
Result: Increases appointment duration by 30 minutes

```
"Make marketing meeting 1 hour longer"
```
Result: Adds 1 hour to existing duration

```
"Shorten workshop by 2 hours"
```
Result: Reduces appointment duration by 2 hours

### Changing Other Attributes

**Examples:**

```
"Move standup to conference room A"
```
Result: Updates location/resource while keeping time unchanged

```
"Change team sync to include video call"
```
Result: Updates appointment details/notes

---

## Deleting Appointments

### Basic Deletion

**Pattern:** "Delete/Cancel/Remove [subject] [when]"

**Examples:**

```
"Cancel team meeting tomorrow"
```
Result: Deletes "Team Meeting" appointment scheduled for tomorrow

```
"Delete dentist appointment"
```
Result: Prompts for clarification if multiple exist, otherwise deletes the appointment

```
"Remove all meetings on Friday"
```
Result: Deletes all appointments on Friday (may prompt for confirmation)

### Conditional Deletion

**Examples:**

```
"Cancel meetings with John this week"
```
Result: Deletes appointments involving "John" in current week

```
"Remove standup for next Monday only"
```
Result: Deletes single instance of recurring appointment

```
"Delete all recurring team syncs"
```
Result: Removes entire recurring series (may prompt for confirmation)

---

## Querying Appointments

### Finding Information

**Pattern:** "Show/List/Find [subject] [when]"

**Examples:**

```
"Show my meetings today"
```
Result: Lists all appointments for today

```
"What meetings do I have tomorrow?"
```
Result: Displays tomorrow's appointments

```
"Find all appointments with client next week"
```
Result: Searches for appointments containing "client" in next 7 days

```
"List meetings in conference room A this week"
```
Result: Shows appointments using Conference Room A resource

### Free Time Queries

**Examples:**

```
"When am I free tomorrow?"
```
Result: Displays available time slots tomorrow

```
"Find free time this week for 1-hour meeting"
```
Result: Suggests 1-hour slots with no conflicts

```
"Show available conference rooms Friday afternoon"
```
Result: Lists unbooked conference room resources for Friday PM

---

## Supported Patterns and Keywords

### Action Keywords

**Create/Add:**
- "Schedule", "Create", "Add", "Book", "Plan", "Arrange", "Set up"

**Update/Modify:**
- "Move", "Reschedule", "Change", "Update", "Modify", "Shift", "Extend", "Shorten"

**Delete/Remove:**
- "Cancel", "Delete", "Remove", "Clear", "Drop"

**Query/Search:**
- "Show", "List", "Find", "Display", "What", "When", "Where"

### Time Expressions

**Relative:**
- "today", "tomorrow", "yesterday"
- "next week", "last week", "this week"
- "next Monday", "this Friday"
- "in 2 hours", "in 30 minutes"
- "later today", "tonight"

**Specific:**
- "March 25", "3/25/2026", "25th of March"
- "at 2pm", "at 14:00", "at 2:30pm"
- "from 9am to 5pm"
- "noon", "midnight", "morning", "afternoon", "evening"

**Duration:**
- "for 1 hour", "for 30 minutes", "for 2 hours"
- "1-hour", "30-minute", "all day"

**Recurrence:**
- "daily", "weekly", "monthly", "yearly"
- "every day", "every Monday", "every other week"
- "biweekly", "quarterly"

### Resource/Location Keywords

- "in [room name]", "at [location]"
- "using [resource]", "with [resource]"
- "conference room", "meeting room"
- "Zoom", "Teams", "video call"

---

## Context-Aware Scheduling

The SfSmartScheduler uses current scheduler state to inform natural language interpretation.

### Current View Context

**Day View:**
```
"Schedule meeting at 2pm"
```
Interprets as today at 2:00 PM (current day view date)

**Week View:**
```
"Add appointment on Wednesday"
```
Uses Wednesday of the currently viewed week

**Month View:**
```
"Create event on the 15th"
```
Uses 15th of currently viewed month

### Resource Context

If user has filtered by specific resource:
```
"Schedule meeting tomorrow at 10am"
```
Automatically assigns the filtered resource to the appointment

### Time Zone Context

Respects user's current time zone settings for all time interpretations.

### Working Hours Context

When querying free time:
```
"Find free time tomorrow"
```
Returns slots within configured working hours (e.g., 9 AM - 5 PM)

---

## Multi-Operation Commands

The AI can handle multiple operations in a single command.

### Multiple Appointments

```
"Schedule standup at 9am and retrospective at 3pm tomorrow"
```
Result: Creates two separate appointments

### Batch Operations

```
"Cancel all meetings on Friday and reschedule team sync to Monday"
```
Result: Deletes Friday appointments, moves one to Monday

### Complex Scenarios

```
"Move morning meeting to 11am and extend it by 30 minutes"
```
Result: Reschedules and adjusts duration in one operation

---

## Best Practices

### Be Specific When Needed

**Vague:**
```
"Schedule meeting"
```
AI may need to prompt for missing details (when? with whom?)

**Better:**
```
"Schedule team meeting tomorrow at 2pm"
```
Clear intent with all necessary information

### Use Natural Phrasing

**Robotic:**
```
"Create appointment subject 'Dentist' date 2026-03-25 time 10:00"
```

**Natural:**
```
"Schedule dentist appointment on March 25th at 10am"
```

### Provide Context for Ambiguity

**Ambiguous:**
```
"Move the meeting"
```
Which meeting? When?

**Clear:**
```
"Move the team meeting from tomorrow to Friday"
```

### Leverage Defaults

**Minimal:**
```
"Schedule standup tomorrow"
```
Uses default duration (e.g., 30 minutes) and start time

**With overrides:**
```
"Schedule standup tomorrow at 9:15am for 15 minutes"
```

### Test Common User Phrases

Before deploying, test the natural language patterns your users are likely to use:
- "Book a room for meeting"
- "Set up team call"
- "Plan review session"
- Adjust AI prompts if certain phrases aren't understood

### Provide Feedback to Users

Configure `SuggestedPrompts` to show users example phrases:
```csharp
smartScheduler.AssistViewSettings.SuggestedPrompts = new ObservableCollection<string>
{
    "Schedule team meeting tomorrow at 2pm",
    "Find free time this week",
    "Cancel Friday's standup",
    "Summarize my day"
};
```

### Handle Errors Gracefully

If AI doesn't understand:
- Prompt user to rephrase
- Suggest specific patterns
- Show example commands
- Log unclear patterns for AI prompt improvement

### Customize AI Prompts

Adjust the AI service's system prompt to match your domain:

```csharp
// Example: Customize for healthcare
smartScheduler.AssistViewSettings.Prompt = @"
You are a medical appointment scheduler. 
Understand appointment types: consultation, follow-up, procedure, check-up.
Respect doctor availability and clinic hours.
Prioritize urgent appointments.
";
```

### Performance Considerations

- Natural language processing requires API calls (may have latency)
- Cache common interpretations if possible
- Provide instant feedback while processing
- Consider offline mode with Ollama for critical applications

---

## Examples by Domain

### Corporate/Office

```
"Schedule board meeting next Wednesday at 10am in executive room"
"Book conference room B for client presentation Friday 2-4pm"
"Create weekly all-hands every Monday at 9am"
"Find free time for 1-hour code review this week"
```

### Healthcare

```
"Schedule patient consultation tomorrow at 9:30am in room 3"
"Book Dr. Smith for follow-up next Monday at 2pm"
"Add physical therapy session every Tuesday and Thursday at 10am"
"Cancel checkup on Friday"
```

### Education

```
"Schedule math class every Monday at 10am in room 205"
"Create parent-teacher conference next week"
"Book computer lab for Friday afternoon"
"Add study group session tomorrow at 3pm"
```

### Service/Retail

```
"Schedule haircut appointment for Sarah on Saturday at 11am"
"Book car service next Tuesday at 9am"
"Add massage session for client tomorrow at 2pm"
"Create recurring maintenance every first Monday at 10am"
```

---

## Troubleshooting

### AI Not Understanding Commands

**Issue:** Commands aren't being interpreted correctly

**Solutions:**
- Simplify language: "Schedule meeting tomorrow at 2pm" instead of complex sentences
- Verify AI service is properly configured and responding
- Check AI service logs for interpretation errors
- Customize AI prompt to better understand your domain terminology
- Test with example phrases from Syncfusion documentation

### Wrong Date/Time Interpretation

**Issue:** Appointments created at incorrect times

**Solutions:**
- Be explicit: Use "2:00 PM" instead of "afternoon"
- Specify date fully: "March 25, 2026" instead of "the 25th"
- Check time zone settings in scheduler configuration
- Verify current view context matches expectation

### Duplicate Appointments Created

**Issue:** Multiple appointments for same event

**Solutions:**
- Check if command is ambiguous (AI creates multiple interpretations)
- Use specific identifiers: "the team meeting" instead of "meeting"
- Handle `AssistAppointmentResponseCompleted` event to prevent duplicates
- Review AI response text for clarification requests

### Resources Not Assigned

**Issue:** Location/resource not attached to appointment

**Solutions:**
- Ensure resource names match exactly (case-sensitive in some configs)
- Use "in" or "at" keywords: "in conference room A"
- Verify resources are configured in scheduler
- Check resource filtering in current view

---

## Next Steps

- **Learn resource-aware features:** [resource-aware-features.md](resource-aware-features.md)
- **Customize assist view:** [assist-view-customization.md](assist-view-customization.md)
- **Handle events programmatically:** [events-and-methods.md](events-and-methods.md)
