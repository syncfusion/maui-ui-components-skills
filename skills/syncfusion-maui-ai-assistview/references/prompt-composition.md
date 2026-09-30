# Structured Prompt Support in SfAIAssistView

`SfAIAssistView` can compose a request from multiple prompt sources. This lets applications combine system instructions, context, reusable prompt parts, selected agent context, and user input into one deterministic prompt.

## Table of Contents
- [Prompt Sources](#prompt-sources)
- [Prompt Composition Order](#prompt-composition-order)
- [Prompt Composing Event](#prompt-composing-event)
- [Scenarios](#scenarios)

---

## Prompt Sources

The control exposes these prompt inputs:

- `SystemPrompt` for global AI instructions
- `ContextPrompt` for app-specific context
- `PromptParts` for ordered prompt fragments

Each `AssistPromptPart` supports `Content`, `Order`, and `IsEnabled`.

```csharp
sfAIAssistView.SystemPrompt = "You are a helpful assistant.";
sfAIAssistView.ContextPrompt = "Project: AI AssistView documentation update";
```

```csharp
sfAIAssistView.PromptParts.Add(new AssistPromptPart
{
    Content = "Keep responses concise.",
    Order = 1,
    IsEnabled = true
});
```

---

## Prompt Composition Order

The final prompt is built in a deterministic order:

1. `SystemPrompt`
2. selected agent context, when present
3. `ContextPrompt`
4. enabled `PromptParts` sorted by `Order`
5. user input from the request editor

Null, empty, and disabled segments are skipped.

---

## Prompt Composing Event

Use `PromptComposing` to inspect the final composed prompt before the request is sent.

```csharp
sfAIAssistView.PromptComposing += (sender, e) =>
{
    Console.WriteLine(e.ComposedPrompt);
};
```

`PromptComposingEventArgs` provides the read-only `ComposedPrompt` and the enabled `Parts` in composition order.

---

## Scenarios

- When `SystemPrompt` is set, it appears at the start of the composed prompt.
- When `ContextPrompt` is set, it appears after the system prompt and before prompt parts.
- When prompt parts are added, enabled parts appear in ascending `Order` sequence.
- When a prompt part is disabled, it is excluded from the final prompt.
- When `PromptComposing` is raised, the application can inspect the final composed prompt before submission.
- When user input is entered, it appears last in the composed prompt.
- When an agent is selected, its context is included between the system prompt and application context.

---

## Technical Notes

- **Delimiter**: prompt segments are joined with `Environment.NewLine`
- **Prompt parts**: `AssistPromptPart` exposes `Content`, `Order`, and `IsEnabled`; internal role metadata is implementation detail

### Bindable Properties

- `SystemPrompt` (`string`) — Global AI instructions
- `ContextPrompt` (`string`) — Application context for the conversation
- `PromptParts` (`IList<AssistPromptPart>`) — Ordered collection of prompt fragments

### Events

- `PromptComposing` (`EventHandler<PromptComposingEventArgs>`) — Raised after the prompt is composed and before the request is sent
