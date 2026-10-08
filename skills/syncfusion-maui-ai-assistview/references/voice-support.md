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

### Required permissions

The following platform permissions must be declared before voice input can start.

#### Android

Add `RECORD_AUDIO` to `Platforms/Android/AndroidManifest.xml`:

```xml
<uses-permission android:name="android.permission.RECORD_AUDIO" />
```

On Android 6.0 (API 23) and later, also request the microphone permission at runtime before the user taps the microphone button, using `Permissions.RequestAsync<Permissions.Microphone>()`.

#### iOS and macOS

Add both `NSMicrophoneUsageDescription` and `NSSpeechRecognitionUsageDescription` to `Platforms/iOS/Info.plist` and `Platforms/MacCatalyst/Info.plist`:

```xml
<key>NSMicrophoneUsageDescription</key>
<string>This app uses the microphone for voice input in the chat.</string>
<key>NSSpeechRecognitionUsageDescription</key>
<string>This app uses speech recognition for voice input in the chat.</string>
```

#### Windows

Add the microphone device capability to `Platforms/Windows/Package.appxmanifest`:

```xml
<Capabilities>
  <rescap:Capability Name="runFullTrust" />
  <DeviceCapability Name="microphone" />
</Capabilities>
```

Also ensure these Windows privacy settings are enabled:
- **Microphone access**: Settings > Privacy & Security > Microphone — allow the app to access the microphone.
- **Online speech recognition**: Settings > Privacy & Security > Speech — turn on **Online speech recognition**.

If the online speech recognition setting or microphone permission is disabled, the microphone button may be visible but no speech is recognized.

---

## Response Audio Playback

The control provides audio playback controls for assistant responses, including play, pause, resume, and stop behavior.

---

## Platform Support

Voice input is supported on Android, iOS/macOS, and Windows. Unsupported platforms degrade gracefully.

### Required permissions

- Android: `RECORD_AUDIO` permission (plus runtime permission on Android 6+).
- iOS/macOS: `NSMicrophoneUsageDescription` and `NSSpeechRecognitionUsageDescription` usage descriptions.
- Windows: `microphone` device capability in `Package.appxmanifest`, app microphone permission enabled, and **Online speech recognition** enabled in Settings.

---

## Scenarios

- When `EnableVoiceInput` is `true`, the microphone button is visible in the request editor.
- When `EnableVoiceInput` is `false`, the microphone button is hidden and voice input is disabled.
- When the microphone button is tapped and permission is granted, speech recognition starts.
- When voice input is active, partial and final recognition text is inserted into the request editor.
- On Windows, if the **Online speech recognition** setting is turned off, the microphone button will appear but speech will not be recognized.
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

- `EnableVoiceInput` (`bool`) — Enables or disables the microphone button. Defaults to `true`.

### Voice input services

- `IVoiceInputService` — Internal abstraction for speech-to-text handling
- `VoiceInputService` — Platform-specific implementation for microphone permission, listening, and recognition events

### Troubleshooting

| Problem | Cause | Fix |
|---|---|---|
| Microphone button is not visible | `EnableVoiceInput` is `false` or the style hides it | Set `EnableVoiceInput="True"`. |
| Microphone button is visible but nothing happens | Microphone permission denied | Grant microphone permission in platform settings. |
| Microphone button is visible but no text is recognized | Runtime permission not requested (Android) | Request `Permissions.Microphone` at runtime on Android 6+. |
| Microphone button is visible but no text is recognized | iOS/macOS usage descriptions missing | Add both `NSMicrophoneUsageDescription` and `NSSpeechRecognitionUsageDescription`. |
| Microphone button is visible but no text is recognized | Windows online speech recognition disabled | Enable **Settings > Privacy & Security > Speech > Online speech recognition**. |
| Microphone button is visible but no text is recognized | Windows app microphone permission denied | Enable **Settings > Privacy & Security > Microphone** for the app. |
