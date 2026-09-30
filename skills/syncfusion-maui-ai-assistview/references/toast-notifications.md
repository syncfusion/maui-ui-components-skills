# Toast Notifications in SfAIAssistView

Covers the non-blocking toast notification system used to display success, error, and warning feedback in `SfAIAssistView`.

## Table of Contents
- [Overview](#overview)
- [Event Handling](#event-handling)

---

## Overview

Toast notifications provide lightweight, contextual feedback without interrupting the user workflow. Only one toast is displayed at a time, and a new toast replaces any currently visible toast immediately.

## Event Handling

The `ToastOpening` event is raised when a toast becomes visible. Event handlers can set `Handled = true` on `ToastNotificationEventArgs` to suppress the toast.

### Example: Suppress a toast in the event handler

```csharp
sfAIAssistView.ToastOpening += (sender, args) =>
{
    if (args.Toast?.Type == ToastType.Warning)
    {
        args.Handled = true;
    }
};
```