# maui-ui-components-skills

Skills for Syncfusion .NET MAUI components, designed for use with AI coding assistants.

This repository contains 76 AI-ready skill guides for working with Syncfusion .NET MAUI controls. Each skill includes a `SKILL.md` file that AI coding assistants can read automatically, plus a `references/` subfolder with detailed documentation covering setup, usage patterns, customization, and troubleshooting.

## Quick Start

### Option 1: Using npx (Recommended)

```bash
npx skills add https://github.com/syncfusion/maui-ui-components-skills
```

This will automatically add the skills to your workspace.

### Option 2: Manual Installation

**1. Clone this repository**
```bash
git clone https://github.com/syncfusion/maui-ui-components-skills.git
```

**2. Add it to your VS Code workspace**

Open your `.code-workspace` file (or create one) and add this repo as a second root folder:
```json
{
  "folders": [
    { "path": "/path/to/your-maui-app" },
    { "path": "/path/to/maui-ui-components-skills" }
  ]
}
```

**3. Start asking questions**

Your AI assistant will automatically detect and apply the relevant skill based on your prompt:
```text
How do I add grouping to the Syncfusion DataGrid?
How do I configure the Scheduler for week view?
How do I apply a dark theme to Syncfusion controls?
```

No configuration required. Skills are loaded automatically from the workspace.

---

## Prerequisites

- An AI coding assistant that supports skills/context files (e.g., GitHub Copilot, Cursor, or similar tools)
- [.NET 9 SDK](https://dotnet.microsoft.com/download/dotnet/9.0)
- A Syncfusion license key ([free community license available](https://www.syncfusion.com/products/communitylicense))

## How These Skills Work

Each `SKILL.md` file contains a `description` field in its YAML frontmatter. AI coding assistants read this description to decide when to automatically apply a skill during a conversation. When you ask about a specific Syncfusion control — for example, "How do I add sorting to my DataGrid?" — the AI assistant detects the match and loads the corresponding skill to guide its response.

You can also reference a skill explicitly by mentioning the component or control by name in your prompt.

### Example Prompts

```text
How do I bind data to the Syncfusion DataGrid in .NET MAUI?
```
→ The AI assistant loads the DataGrid skill and uses its get-started and data-binding reference docs.

```text
I need to add a date range picker to my app.
```
→ The AI assistant loads the Date Time Range Selector skill.

```text
Help me migrate my Xamarin.Forms app to .NET MAUI.
```
→ The AI assistant loads the Migration skill.

### Using Reference Files

Each `references/` subfolder contains deeper implementation guides. When the AI assistant loads a skill, it can also pull in these files when you ask follow-up questions:

```text
Show me how to export the DataGrid to Excel.
```
→ The AI assistant uses `references/advanced-features.md` from the DataGrid skill for the detailed answer.

## Skill File Structure

Every skill folder follows this layout:

```text
skills/
└── syncfusion-maui-<control>/
    ├── SKILL.md                  ← Loaded by AI assistant; contains When to Use, Component Overview, and navigation links
    └── references/
        ├── getting-started.md    ← Installation, setup, NuGet packages, MauiProgram.cs
        ├── advanced-features.md  ← In-depth feature guides and code samples
        └── ...                   ← Additional reference files per control
```

`SKILL.md` sections:
- **When to Use This Skill** — trigger phrases and scenarios that activate this skill
- **Component Overview** — NuGet package, namespace, key capabilities at a glance
- **Documentation and Navigation Guide** — links to all reference files in the skill

## Repository Structure

```text
README.md
skills/
    syncfusion-maui-getting-started/
    syncfusion-maui-migration/
    syncfusion-maui-theming/
    syncfusion-maui-accordion/
    syncfusion-maui-ai-assistview/
    syncfusion-maui-autocomplete/
    ... (one folder per control, 76 total)
```

## Skill Index

> **Tip:** Start with [Getting Started](skills/syncfusion-maui-getting-started/SKILL.md) if you are setting up a new project, and [Migration](skills/syncfusion-maui-migration/SKILL.md) if upgrading from Xamarin.Forms. For all other tasks, find the skill that matches the specific control below.

### Foundation

- [Getting Started](skills/syncfusion-maui-getting-started/SKILL.md) — installation, licensing, themes, AI service integration
- [Migration](skills/syncfusion-maui-migration/SKILL.md) — Xamarin.Forms to .NET MAUI migration guide
- [Theming](skills/syncfusion-maui-theming/SKILL.md) — Material/Cupertino themes, dark mode, custom styling

### Data Visualization

- [Cartesian Charts](skills/syncfusion-maui-cartesian-charts/SKILL.md)
- [Circular Charts](skills/syncfusion-maui-circular-charts/SKILL.md)
- [Funnel Charts](skills/syncfusion-maui-funnel-charts/SKILL.md)
- [Polar Charts](skills/syncfusion-maui-polar-charts/SKILL.md)
- [Pyramid Charts](skills/syncfusion-maui-pyramid-charts/SKILL.md)
- [Sunburst Charts](skills/syncfusion-maui-sunburst-charts/SKILL.md)
- [Circular ProgressBar](skills/syncfusion-maui-circular-progressbar/SKILL.md)
- [Linear ProgressBar](skills/syncfusion-maui-linear-progressbar/SKILL.md)
- [Step ProgressBar](skills/syncfusion-maui-step-progressbar/SKILL.md)
- [Digital Gauge](skills/syncfusion-maui-digital-gauge/SKILL.md)
- [Linear Gauge](skills/syncfusion-maui-linear-gauge/SKILL.md)
- [Radial Gauge](skills/syncfusion-maui-radial-gauge/SKILL.md)
- [Barcode Generator](skills/syncfusion-maui-barcode-generator/SKILL.md)
- [Maps](skills/syncfusion-maui-maps/SKILL.md)
- [TreeMap](skills/syncfusion-maui-treemap/SKILL.md)

### Grids and Data

- [DataGrid](skills/syncfusion-maui-datagrid/SKILL.md)
- [Smart DataGrid](skills/syncfusion-maui-smart-datagrid/SKILL.md)
- [ListView](skills/syncfusion-maui-listview/SKILL.md)
- [TreeView](skills/syncfusion-maui-treeview/SKILL.md)
- [Kanban Board](skills/syncfusion-maui-kanban-board/SKILL.md)
- [DataForm](skills/syncfusion-maui-dataform/SKILL.md)

### Scheduling and Calendars

- [Scheduler](skills/syncfusion-maui-scheduler/SKILL.md)
- [Smart Scheduler](skills/syncfusion-maui-smart-scheduler/SKILL.md)
- [Calendar](skills/syncfusion-maui-calendar/SKILL.md)

### Editors and Inputs

- [Autocomplete](skills/syncfusion-maui-autocomplete/SKILL.md)
- [ComboBox](skills/syncfusion-maui-combobox/SKILL.md)
- [Picker](skills/syncfusion-maui-picker/SKILL.md)
- [Date Picker](skills/syncfusion-maui-date-picker/SKILL.md)
- [Time Picker](skills/syncfusion-maui-time-picker/SKILL.md)
- [Date Time Picker](skills/syncfusion-maui-date-time-picker/SKILL.md)
- [Masked Entry](skills/syncfusion-maui-masked-entry/SKILL.md)
- [Numeric Entry](skills/syncfusion-maui-numeric-entry/SKILL.md)
- [Text Input Layout](skills/syncfusion-maui-text-input-layout/SKILL.md)
- [Rich Text Editor](skills/syncfusion-maui-rich-text-editor/SKILL.md)
- [Smart Text Editor](skills/syncfusion-maui-smart-text-editor/SKILL.md)
- [Image Editor](skills/syncfusion-maui-image-editor/SKILL.md)
- [Color Picker](skills/syncfusion-maui-color-picker/SKILL.md)
- [Signature Pad](skills/syncfusion-maui-signature-pad/SKILL.md)
- [Rating](skills/syncfusion-maui-rating/SKILL.md)

### Sliders and Range Controls

- [Slider](skills/syncfusion-maui-slider/SKILL.md)
- [Range Slider](skills/syncfusion-maui-range-slider/SKILL.md)
- [Range Selector](skills/syncfusion-maui-range-selector/SKILL.md)
- [Date Time Slider](skills/syncfusion-maui-date-time-slider/SKILL.md)
- [Date Time Range Slider](skills/syncfusion-maui-date-time-range-slider/SKILL.md)
- [Date Time Range Selector](skills/syncfusion-maui-date-time-range-selector/SKILL.md)

### Navigation

- [Tab View](skills/syncfusion-maui-tab-view/SKILL.md)
- [Navigation Drawer](skills/syncfusion-maui-navigation-drawer/SKILL.md)
- [Toolbar](skills/syncfusion-maui-toolbar/SKILL.md)
- [Radial Menu](skills/syncfusion-maui-radial-menu/SKILL.md)
- [DockLayout](skills/syncfusion-maui-docklayout/SKILL.md)

### Layout and Containers

- [Accordion](skills/syncfusion-maui-accordion/SKILL.md)
- [Expander](skills/syncfusion-maui-expander/SKILL.md)
- [Cards](skills/syncfusion-maui-cards/SKILL.md)
- [Carousel](skills/syncfusion-maui-carousel/SKILL.md)
- [Rotator](skills/syncfusion-maui-rotator/SKILL.md)
- [Backdrop](skills/syncfusion-maui-backdrop/SKILL.md)
- [Parallax View](skills/syncfusion-maui-parallax-view/SKILL.md)
- [Pull to Refresh](skills/syncfusion-maui-pull-to-refresh/SKILL.md)
- [Segmented Control](skills/syncfusion-maui-segmented-control/SKILL.md)

### Buttons and Indicators

- [Button](skills/syncfusion-maui-button/SKILL.md)
- [Checkbox](skills/syncfusion-maui-checkbox/SKILL.md)
- [Radio Button](skills/syncfusion-maui-radio-button/SKILL.md)
- [Switch](skills/syncfusion-maui-switch/SKILL.md)
- [Chips](skills/syncfusion-maui-chips/SKILL.md)
- [Badge View](skills/syncfusion-maui-badge-view/SKILL.md)
- [Busy Indicator](skills/syncfusion-maui-busy-indicator/SKILL.md)
- [Shimmer](skills/syncfusion-maui-shimmer/SKILL.md)
- [Effects View](skills/syncfusion-maui-effects-view/SKILL.md)

### Miscellaneous

- [Avatar View](skills/syncfusion-maui-avatar-view/SKILL.md)
- [AI AssistView](skills/syncfusion-maui-ai-assistview/SKILL.md)
- [Chat](skills/syncfusion-maui-chat/SKILL.md)
- [Markdown Viewer](skills/syncfusion-maui-markdown-viewer/SKILL.md)
- [TreeMap](skills/syncfusion-maui-treemap/SKILL.md)