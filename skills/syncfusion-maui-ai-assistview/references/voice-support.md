# Voice Support in SfAIAssistView

`SfAIAssistView` can accept speech input through a microphone button and can play assistant responses through built-in audio playback controls.

## Table of Contents
- [Voice Input](#voice-input)
- [Response Audio Playback](#response-audio-playback)
- [Platform Support](#platform-support)
- [Scenarios](#scenarios)

---

## Voice Input

Use `EnableVoiceInput` to show or hide the microphone button. The default value is `true`.

```xaml
<syncfusion:SfAIAssistView x:Name="sfAIAssistView"
                           EnableVoiceInput="True" />
```

```csharp
sfAIAssistView.EnableVoiceInput = true;
```

When the microphone button is tapped, the control requests microphone permission and starts speech recognition if permission is granted.

Recognized text is inserted into the request editor while voice input is active.

---

## Response Audio Playback

The control provides audio playback controls for assistant responses, including play, pause, resume, and stop behavior.

---

## Platform Support

Voice input is supported on Android, iOS/macOS, and Windows. Unsupported platforms degrade gracefully.

### Required permissions

- Android: microphone permission
- iOS/macOS: microphone and speech recognition usage descriptions
- Windows: microphone capability

---

## Scenarios

- When `EnableVoiceInput` is `true`, the microphone button is visible in the request editor.
- When `EnableVoiceInput` is `false`, the microphone button is hidden and voice input is disabled.
- When the microphone button is tapped and permission is granted, speech recognition starts.
- When voice input is active, partial and final recognition text is inserted into the request editor.
- When microphone permission is denied, the control shows a user-facing error and does not start recognition.
- When an assistant response is available, the speaker control plays the response through the built-in audio player.
- When the speaker control is tapped again, playback toggles between pause and resume.
- When the app runs on an unsupported platform, the control degrades gracefully without crashing.

---

## Technical Notes

- **Voice input pipeline**: microphone button tap → permission request → speech recognition service → partial/final result events → editor text injection
- **Response playback pipeline**: speaker action → audio player view → internal or injected audio/text-to-audio services
- **Fallback behavior**: unsupported platforms use a graceful no-op implementation for voice input

### Bindable Properties

- `EnableVoiceInput` (`bool`) — Enables or disables the microphone button

### Voice input services

- `IVoiceInputService` — Internal abstraction for speech-to-text handling
- `VoiceInputService` — Platform-specific implementation for microphone permission, listening, and recognition events
