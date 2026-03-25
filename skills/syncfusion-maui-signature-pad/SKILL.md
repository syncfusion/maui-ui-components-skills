---
name: syncfusion-maui-signature-pad
description: Implements Syncfusion .NET MAUI SignaturePad (SfSignaturePad) for capturing digital signatures with realistic handwritten appearance. Use when working with signature capture, digital signatures, electronic signatures, or signing documents in .NET MAUI applications. Covers stroke customization, saving signatures as images, signature point data retrieval, and signature validation.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing .NET MAUI SignaturePad

The .NET MAUI SignaturePad (`SfSignaturePad`) is an interactive UI control that enables users to capture smooth, realistic digital signatures. It supports finger, pen, or mouse input on tablets, touchscreens, and other devices, providing a natural signing experience with customizable stroke appearance and advanced features.

## When to Use This Skill

Use this skill when users need to:
- **Capture digital signatures** in .NET MAUI applications for documents, forms, or authentication
- **Implement signature functionality** for contracts, agreements, invoices, or any document requiring signatures
- **Save signatures as images** to embed in PDFs, documents, or databases
- **Customize signature appearance** with specific colors, thickness ranges, or visual styles
- **Handle signature events** for validation, processing, or workflow control
- **Retrieve signature point data** for advanced analysis or custom rendering
- **Integrate with document signing workflows** where electronic signatures are required
- **Apply modern UI effects** like Liquid Glass Effect for premium signature experiences

The SignaturePad provides a unique stroke rendering algorithm that creates realistic, handwritten signatures based on drawing speed and gesture pressure, offering a more authentic signing experience than simple drawing controls.

## Component Overview

**Key Features:**
- **Realistic Handwriting:** Dynamic stroke thickness based on drawing speed and pressure
- **Stroke Customization:** Configure color, minimum/maximum thickness, and appearance
- **Image Export:** Save signatures as `ImageSource` for document integration
- **Point Data Retrieval:** Access raw signature points as `List<List<float>>` for custom processing
- **Event Handling:** Respond to drawing start/completion, with cancellation support
- **Clear Functionality:** Reset the signature pad programmatically
- **Modern Effects:** Support for Liquid Glass Effect on compatible platforms
- **Cross-Platform:** Consistent behavior across iOS, Android, macOS, and Windows

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)

Use this reference when you need to:
- Set up the SignaturePad in a new .NET MAUI project
- Install the `Syncfusion.Maui.SignaturePad` NuGet package
- Register Syncfusion handlers in `MauiProgram.cs`
- Create a basic SignaturePad in XAML or C#
- Learn the minimal implementation setup

### Stroke Customization
📄 **Read:** [references/stroke-customization.md](references/stroke-customization.md)

Use this reference when you need to:
- Change the signature stroke color (`StrokeColor`)
- Set minimum and maximum stroke thickness for realistic rendering
- Use simple stroke width for uniform appearance
- Configure background color or transparency
- Understand the realistic handwriting algorithm
- Learn best practices for stroke appearance
- Decide between `StrokeWidth` vs `MinimumStrokeThickness`/`MaximumStrokeThickness`

### Saving and Retrieving Signatures
📄 **Read:** [references/saving-signatures.md](references/saving-signatures.md)

Use this reference when you need to:
- Save signatures as images using `ToImageSource()`
- Export signatures to file system or databases
- Retrieve signature point data with `GetSignaturePoints()`
- Work with the `List<List<float>>` point data structure
- Integrate signatures with PDFs or documents
- Implement save/load workflows
- Handle synchronization across devices
- Manage image format considerations

### Events and Methods
📄 **Read:** [references/events-and-methods.md](references/events-and-methods.md)

Use this reference when you need to:
- Handle the `DrawStarted` event to validate or restrict drawing
- Use `CancelEventArgs` to prevent signature drawing
- Respond to `DrawCompleted` event for post-drawing processing
- Implement the `Clear()` method to reset signatures
- Create validation workflows with event handling
- Build event-driven signature capture processes
- Understand event sequencing and lifecycle

### Advanced: Liquid Glass Effect
📄 **Read:** [references/liquid-glass-effect.md](references/liquid-glass-effect.md)

Use this reference when you need to:
- Apply Liquid Glass Effect for modern, translucent UI design
- Wrap SignaturePad in `SfGlassEffectView`
- Configure transparency and glass effect properties
- Understand platform requirements (.NET 10, iOS 26, macOS 26)
- Implement premium visual experiences
- Customize glass effect appearance

