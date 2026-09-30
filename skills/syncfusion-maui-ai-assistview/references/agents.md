# Agent support

## Overview

`SfAIAssistView` supports multiple AI agents in a single chat experience. This lets host apps present specialized assistants, switch the current agent, and customize how the selected agent appears in the editor.

## Key capabilities

- `Agents` accepts a collection of `AssistAgent` items.
- `AssistAgent` supports `Name`, `Description`, `Instructions`, and `Icon`.
- `SelectedAgent` tracks the current agent.
- `ShowSelectedAgent` is enabled by default and shows the selected agent in the editor.
- `SelectedAgentTemplate` supports custom rendering for the selected agent UI.
- Users can type `@` in the editor to choose an agent from the available collection.

## Scenarios

### Populate agents

Bind an `ObservableCollection<AssistAgent>` to `SfAIAssistView.Agents` to display the available agents.

```csharp
using System.Collections.ObjectModel;
using Syncfusion.Maui.AIAssistView;

var agents = new ObservableCollection<AssistAgent>
{
	new AssistAgent
	{
		Name = "Writing Assistant",
		Description = "Helps with writing and editing",
		Instructions = "You are a writing assistant.",
		Icon = "richtexteditor.png"
	},
	new AssistAgent
	{
		Name = "Art Assistant",
		Description = "Creates images from prompts",
		Instructions = "You are an image generation assistant.",
		Icon = "imageeditor.png"
	}
};

sfAIAssistView.Agents = agents;
```

### Select a current agent

Assign `SelectedAgent` to show a specific agent in the editor.

```csharp
sfAIAssistView.SelectedAgent = agents[0];
```

### Hide the selected agent

Set `ShowSelectedAgent` to `false` when the selected agent should not be shown in the editor.

```csharp
sfAIAssistView.ShowSelectedAgent = false;
```

### Customize the selected agent view

Provide a `SelectedAgentTemplate` to replace the default selected-agent layout.

```xaml
<syncfusion:SfAIAssistView SelectedAgentTemplate="{StaticResource agentTemplate}" />
```

```csharp
sfAIAssistView.SelectedAgentTemplate = new DataTemplate(() =>
{
	return new VerticalStackLayout
	{
		Children =
		{
			new Label { Text = "Selected Agent", FontAttributes = FontAttributes.Bold },
			new Label { Text = "Custom selected-agent content" }
		}
	};
});
```

### Choose an agent from the editor

Typing `@` in the editor exposes the available agents for user selection.

```xaml
<syncfusion:SfAIAssistView x:Name="sfAIAssistView" />
```

When the user types `@` in the editor, the available agents are shown automatically.

## Technical notes

- **Dependencies**: `AssistAgent`, editor input handling, and template rendering
- **Platform behavior**: agent selection and rendering follow the host platform UI conventions

### Bindable properties

- `Agents` (`IList<AssistAgent>`) — collection of available agents
- `SelectedAgent` (`AssistAgent`) — currently selected agent
- `ShowSelectedAgent` (`bool`, default: `true`) — show or hide the selected agent in the editor
- `SelectedAgentTemplate` (`DataTemplate`) — custom template for selected-agent rendering