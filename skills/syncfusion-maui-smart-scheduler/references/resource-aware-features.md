# Resource-Aware Features

## Table of Contents
- [Overview](#overview)
- [Resource-Aware Booking](#resource-aware-booking)
- [Availability Checking](#availability-checking)
- [Conflict Detection](#conflict-detection)
- [Alternative Resource Suggestions](#alternative-resource-suggestions)
- [Free Time Finder](#free-time-finder)
- [Smart Summarization](#smart-summarization)
- [Current View Context](#current-view-context)
- [Resource Filtering](#resource-filtering)
- [Best Practices](#best-practices)

---

## Overview

The SfSmartScheduler goes beyond simple appointment creation by understanding resource availability, detecting conflicts, and providing intelligent suggestions. These features make it ideal for scenarios requiring coordination of shared resources like meeting rooms, equipment, or people.

**Key resource-aware capabilities:**
- Book resources while respecting their availability
- Detect overlapping appointments (conflicts)
- Suggest alternative resources when primary choice is unavailable
- Recommend adjacent time slots when conflicts exist
- Find free time across multiple resources
- Summarize appointments for quick overview
- Respect current view context and resource filters

---

## Resource-Aware Booking

### What is Resource-Aware Booking?

Resource-aware booking means the AI understands that appointments may need specific resources (rooms, equipment, people) and validates availability before creating appointments.

**Without resource awareness:**
```
"Book conference room A for 2pm"
```
Creates appointment regardless of whether room is available

**With resource awareness:**
```
"Book conference room A for 2pm"
```
- Checks if Conference Room A is available at 2pm
- If available: Books it
- If not: Suggests alternatives or different times

### Configuring Resources

Before using resource-aware features, configure your scheduler with resources.

**Example: Meeting Rooms**

```csharp
using Syncfusion.Maui.Scheduler;

// Define resources
var resources = new ObservableCollection<SchedulerResource>
{
    new SchedulerResource 
    { 
        Id = "room-a", 
        Name = "Conference Room A",
        Foreground = Colors.Blue,
        Background = Colors.LightBlue
    },
    new SchedulerResource 
    { 
        Id = "room-b", 
        Name = "Conference Room B",
        Foreground = Colors.Green,
        Background = Colors.LightGreen
    },
    new SchedulerResource 
    { 
        Id = "zoom", 
        Name = "Zoom Link",
        Foreground = Colors.Purple,
        Background = Colors.Lavender
    }
};

// Add resources to scheduler (via scheduler property, not smartScheduler directly)
// Access underlying scheduler if needed
```

### Booking with Natural Language

Once resources are configured, users can reference them naturally:

```
"Book conference room A for team sync tomorrow at 10am"
```

```
"Schedule meeting using Zoom on Friday at 2pm"
```

```
"Reserve conference room B for client presentation next Monday 2-4pm"
```

The AI:
1. Identifies resource name ("conference room A", "Zoom")
2. Checks availability at requested time
3. Books if available, suggests alternatives if not

---

## Availability Checking

### How Availability Checking Works

When a user requests a resource, the SfSmartScheduler:
1. Parses the resource reference from natural language
2. Queries existing appointments for that resource
3. Checks if the requested time slot is free
4. Returns availability status

### Example: Available Resource

**User request:**
```
"Book meeting room for tomorrow at 3pm"
```

**AI process:**
1. Checks "meeting room" availability tomorrow at 3pm
2. No conflicting appointments found
3. Creates appointment with meeting room resource

**Confirmation:**
```
"Meeting room booked for tomorrow at 3pm"
```

### Example: Unavailable Resource

**User request:**
```
"Book conference room A tomorrow at 2pm"
```

**AI process:**
1. Checks Conference Room A tomorrow at 2pm
2. Finds existing appointment occupying that slot
3. Detects conflict

**Response:**
```
"Conference Room A is unavailable at 2pm tomorrow. 
Suggestions:
- Conference Room B is available at 2pm
- Conference Room A is available at 3pm or 4pm
Would you like to book one of these instead?"
```

### Working Hours Validation

The scheduler can validate requests against configured working hours:

**User request:**
```
"Schedule meeting at 9pm tonight"
```

**AI response (if outside working hours):**
```
"9pm is outside standard working hours (9am-6pm).
Would you like to schedule during business hours?
Next available: Tomorrow at 9am"
```

---

## Conflict Detection

### What is Conflict Detection?

Conflict detection identifies overlapping appointments—when multiple appointments are scheduled for the same resource or person at the same time.

### Automatic Conflict Detection

When creating appointments, the AI automatically checks for conflicts:

**Scenario: User has existing appointment**

Existing: "Team Meeting" on Friday at 3pm

**User request:**
```
"Schedule client call Friday at 3pm"
```

**AI detection:**
```
"⚠️ Conflict detected: You already have 'Team Meeting' on Friday at 3pm.
Suggestions:
1. Reschedule client call to 2pm or 4pm
2. Reschedule team meeting to different time
3. Extend buffer time if meetings can overlap partially
What would you like to do?"
```

### Conflict Detection for Resources

**Scenario: Room is booked**

Existing: "Marketing Review" in Conference Room A Friday 2-3pm

**User request:**
```
"Book conference room A for Friday at 2:30pm"
```

**AI detection:**
```
"⚠️ Conference Room A is booked from 2-3pm for 'Marketing Review'.
Suggestions:
- Conference Room B is available at 2:30pm
- Conference Room A is available at 3pm, 4pm
Would you like to choose an alternative?"
```

### Manual Conflict Detection

Users can explicitly request conflict checking:

```
"Check for conflicts tomorrow"
```

**AI response:**
```
"Conflicts found for tomorrow:
• 10:00am: Team Standup and Project Review overlap
• 2:00pm: Conference Room A double-booked (Design Meeting, Client Call)
Would you like me to suggest resolutions?"
```

### Conflict Detection Across Date Ranges

```
"Detect conflicts for this week"
```

```
"Find overlapping appointments next Monday"
```

```
"Show conflicts for Conference Room A this month"
```

---

## Alternative Resource Suggestions

### When Suggestions are Provided

The AI suggests alternatives when:
- Requested resource is unavailable
- Requested time slot has conflicts
- User explicitly asks for options

### Types of Suggestions

**1. Alternative Resources (Same Time)**

**Request:**
```
"Book main conference room tomorrow at 2pm"
```

**Response (if unavailable):**
```
"Main conference room is unavailable at 2pm.
Alternative rooms available at 2pm:
- Conference Room B (capacity: 10)
- Small Meeting Room (capacity: 6)
- Executive Boardroom (capacity: 20)
Which would you prefer?"
```

**2. Alternative Time Slots (Same Resource)**

**Request:**
```
"Reserve projector for Friday morning"
```

**Response (if unavailable):**
```
"Projector is booked Friday 9-11am.
Available time slots Friday:
- 8:00am - 9:00am
- 11:00am - 12:00pm
- 2:00pm onwards
When would you like to book it?"
```

**3. Combination Suggestions**

**Request:**
```
"Schedule team meeting tomorrow afternoon"
```

**Response:**
```
"Multiple options available:
1. Conference Room A at 2pm
2. Conference Room B at 3pm
3. Virtual meeting (Zoom) at 2pm or 4pm
Which works best for your team?"
```

### Adjacent Time Slots

When exact time isn't available, AI suggests nearby slots:

**Request:**
```
"Book room for 2pm"
```

**Response:**
```
"2pm slot is taken. Adjacent available times:
- 1:30pm - 2:00pm (before)
- 2:30pm - 3:30pm (after)
- 3:00pm - 4:00pm (slightly later)
Would any of these work?"
```

---

## Free Time Finder

### Finding Free Time

Users can ask the AI to find available time slots without specifying exact times.

**Basic free time query:**
```
"When am I free tomorrow?"
```

**AI response:**
```
"Available time slots tomorrow:
- 9:00am - 10:30am (1.5 hours)
- 11:00am - 12:00pm (1 hour)
- 2:00pm - 4:00pm (2 hours)
- 4:30pm - 5:00pm (30 minutes)"
```

### With Duration Requirement

```
"Find free time for 1-hour meeting this week"
```

**AI response:**
```
"1-hour slots available this week:
Monday: 10am, 2pm, 3pm
Tuesday: 9am, 11am, 3pm
Wednesday: Full day available
Thursday: 10am, 4pm
Friday: 9am, 1pm
Which day/time works for you?"
```

### Resource-Specific Free Time

```
"When is conference room A available this week?"
```

**AI response:**
```
"Conference Room A availability:
Monday: 9-11am, 3-5pm
Tuesday: 1-5pm
Wednesday: All day
Thursday: 9am-12pm
Friday: 2-5pm"
```

### Multi-Resource Free Time

```
"Find time when both conference room A and projector are available tomorrow"
```

**AI response:**
```
"Times when both Conference Room A and Projector are available tomorrow:
- 9:00am - 10:00am
- 2:00pm - 3:30pm
- 4:00pm - 5:00pm"
```

---

## Smart Summarization

### Appointment Summarization

The AI can generate concise summaries of appointments, helping users quickly understand their schedule.

**Daily summary:**
```
"Summarize my meetings today"
```

**AI response:**
```
"Today's Schedule Summary (5 appointments):

Morning:
• 9:00am - Team Standup (30 min) - Main Office
• 10:00am - Project Review (1 hour) - Conference Room A

Afternoon:
• 1:00pm - Client Presentation (2 hours) - Zoom
• 3:30pm - Budget Discussion (45 min) - Executive Room
• 4:30pm - Code Review (30 min) - Virtual

Total: 5 hours in meetings
Free slots: 11am-1pm, after 5pm"
```

### Date Range Summaries

```
"Summarize my meetings tomorrow"
```

```
"What's my schedule for next week?"
```

```
"Give me an overview of this month's appointments"
```

### Resource-Specific Summaries

```
"Summarize conference room A bookings for this week"
```

**AI response:**
```
"Conference Room A - This Week:
Monday: 3 bookings (6 hours)
Tuesday: 2 bookings (3 hours)
Wednesday: Available all day
Thursday: 4 bookings (5 hours)
Friday: 1 booking (2 hours)

Most booked: Monday & Thursday
Recommendation: Schedule meetings on Wednesday"
```

### Meeting Type Summaries

```
"Summarize all client meetings this month"
```

```
"Show all recurring meetings"
```

```
"List all full-day events next week"
```

---

## Current View Context

### How View Context Works

The SfSmartScheduler uses the currently displayed view to provide context for natural language commands.

**Day View (March 25, 2026):**

User sees March 25th in day view.

```
"Schedule meeting at 2pm"
```

Interprets as: March 25, 2026 at 2pm (the displayed day)

**Week View (Week of March 23-29):**

User sees week view.

```
"Add appointment on Wednesday"
```

Interprets as: Wednesday, March 27, 2026 (Wednesday of displayed week)

**Month View (March 2026):**

User sees March 2026 month view.

```
"Create event on the 15th"
```

Interprets as: March 15, 2026 (the 15th of displayed month)

### Resource View Context

If a resource is filtered/selected:

**User filters to "Conference Room A"**

```
"Schedule meeting tomorrow at 10am"
```

Automatically assigns Conference Room A to the appointment.

---

## Resource Filtering

### Filtering Appointments by Resource

Users can ask to view appointments for specific resources:

```
"Show all meetings in conference room B this week"
```

```
"List appointments using the projector"
```

```
"Display Zoom meetings for tomorrow"
```

### Multi-Resource Filtering

```
"Show appointments for conference rooms A and B today"
```

```
"List meetings using either Zoom or Teams this week"
```

---

## Best Practices

### Define Clear Resource Names

**Good:**
- "Conference Room A", "Conference Room B"
- "Projector 1", "Projector 2"
- "Dr. Smith", "Dr. Johnson"

**Avoid:**
- "Room1", "Rm A" (ambiguous abbreviations)
- "The big room" (too vague)
- Similar-sounding names without clear distinction

### Configure Resource Hierarchies

Organize resources logically:

**Equipment:**
- Projectors
- Whiteboards
- Video conference systems

**Spaces:**
- Conference rooms
- Meeting rooms
- Phone booths

**People:**
- Executives
- Team members
- External consultants

### Set Resource Capacities

When configuring resources, include capacity information:

```csharp
new SchedulerResource 
{ 
    Id = "room-a",
    Name = "Conference Room A",
    // Store capacity in metadata/properties
    // Capacity: 20 people
}
```

This allows AI to suggest appropriate rooms based on meeting size:

```
"Book room for 15-person meeting"
```

AI can prioritize rooms with adequate capacity.

### Test Conflict Scenarios

Before deploying, test common conflict situations:
- Double-booking same resource
- Overlapping person schedules
- Back-to-back meetings with no buffer
- Resource unavailability during booking attempt

### Customize AI Prompts for Domain

Tailor AI behavior to your specific resource management needs:

**Healthcare:**
```csharp
smartScheduler.AssistViewSettings.Prompt = @"
Prioritize doctor availability.
Respect room equipment requirements (X-ray, ultrasound).
Suggest earliest available slots for urgent cases.
";
```

**Corporate:**
```csharp
smartScheduler.AssistViewSettings.Prompt = @"
Consider room capacity for meeting size.
Prefer video conferencing for remote participants.
Suggest conference rooms with necessary AV equipment.
";
```

### Provide Suggested Prompts

Help users discover resource-aware features:

```csharp
smartScheduler.AssistViewSettings.SuggestedPrompts = new ObservableCollection<string>
{
    "Find free conference rooms tomorrow",
    "Check for conflicts this week",
    "Book room for 10-person meeting",
    "When is projector available?",
    "Summarize today's appointments"
};
```

### Handle Edge Cases

**No resources available:**
```
User: "Book meeting room tomorrow"
AI: "All meeting rooms are fully booked tomorrow. 
     Would you like to:
     1. Schedule for different day
     2. Use virtual meeting (Zoom)
     3. See waiting list/cancellation options"
```

**Partial availability:**
```
User: "Book room from 2-5pm"
AI: "Room is available 2-4pm but booked at 4pm.
     Options:
     1. Book 2-4pm slot
     2. Find different room for full 3 hours
     3. Split meeting into two sessions"
```

### Monitor Resource Utilization

Use summarization to track resource usage:

```
"Show conference room utilization this month"
```

Helps identify:
- Over-utilized resources (need more)
- Under-utilized resources (can be repurposed)
- Peak booking times (capacity planning)

---

## Troubleshooting

### Resource Not Recognized

**Issue:** AI doesn't understand resource name

**Solutions:**
- Use exact resource name as configured
- Check spelling and capitalization
- Add common aliases in AI prompt configuration
- Use "in" or "at" keywords: "in conference room A"

### Conflicts Not Detected

**Issue:** Overlapping appointments created without warning

**Solutions:**
- Verify appointments have resources assigned
- Check that appointments are in the scheduler's data source
- Ensure conflict detection is not disabled in configuration
- Test with simple scenarios first

### Wrong Availability Reported

**Issue:** AI suggests times that are actually booked

**Solutions:**
- Verify data source is current (not cached)
- Check time zone settings match user's location
- Ensure recurring appointments are properly calculated
- Refresh scheduler view before querying availability

### Suggestions Not Helpful

**Issue:** AI suggests inappropriate alternatives

**Solutions:**
- Customize AI prompt to understand resource priorities
- Provide more context in the request: "Find large conference room"
- Configure resource metadata (capacity, features, location)
- Train AI with domain-specific terminology

---

## Next Steps

- **Customize assist view:** [assist-view-customization.md](assist-view-customization.md)
- **Handle events programmatically:** [events-and-methods.md](events-and-methods.md)
- **Style your scheduler:** [styling.md](styling.md)
