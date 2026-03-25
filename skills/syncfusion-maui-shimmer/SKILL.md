---
name: syncfusion-maui-shimmer
description: Implements Syncfusion .NET MAUI Shimmer (SfShimmer) loading placeholder effects. Use when implementing shimmer or skeleton loading animations, loading placeholders, or content loading indicators in .NET MAUI apps. Covers shimmer types (CirclePersona, Article, Feed, Shopping), custom shimmer views, wave animation, and customization.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing .NET MAUI Shimmer (SfShimmer)

`SfShimmer` displays an animated shimmer placeholder while content loads in the background, improving perceived app responsiveness. It supports 7 built-in layout types, fully custom views using `ShimmerView` shapes, and extensive wave animation customization.

## When to Use This Skill

Use this skill when users need to:
- **Add a loading placeholder** before data or content is fetched
- **Show a skeleton screen** while an API call completes
- **Replace a spinner** with a content-shaped loading animation
- **Customize shimmer colors**, wave direction, or animation speed
- **Build a custom shimmer layout** that mirrors your actual UI

## Component Overview
 
The **SfShimmer** control provides:
- **Built-in Layout Types**: 7 pre-designed shimmer patterns (CirclePersona, SquarePersona, Profile, Article, Video, Feed, Shopping)
- **Custom View Support**: Build custom shimmer layouts using ShimmerView with Circle, Rectangle, and RoundedRectangle shapes
- **Wave Animation Control**: Configurable wave direction (5 options), width, and animation duration
- **Color Customization**: Independent Fill (background) and WaveColor (highlight) properties
- **Repeat Patterns**: RepeatCount property for vertical stacking of shimmer layouts
- **Content Toggle**: IsActive property for seamless switching between shimmer and loaded content
- **Flexible Integration**: Works with any .NET MAUI View as child content
- **Performance Optimized**: Lightweight animation that improves perceived app responsiveness

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installing `Syncfusion.Maui.Core` NuGet package
- Registering the handler in `MauiProgram.cs`
- Adding `SfShimmer` in XAML and C#
- Toggling between shimmer and content with `IsActive`

### Built-in View Types
📄 **Read:** [references/built-in-views.md](references/built-in-views.md)
- 7 built-in `ShimmerType` values: CirclePersona, SquarePersona, Profile, Article, Video, Feed, Shopping
- Setting the `Type` property in XAML and C#
- Default type and when to choose each layout

### Custom Views
📄 **Read:** [references/custom-views.md](references/custom-views.md)
- Building a custom shimmer layout with `CustomView` and `BoxView`
- Using `ShimmerView` with `ShapeType` (Circle, Rectangle, RoundedRectangle)
- Full XAML and C# examples for complex custom layouts

### Customization & Animation
📄 **Read:** [references/customization.md](references/customization.md)
- `Fill` — shimmer background color
- `WaveColor` — shimmer highlight color
- `WaveWidth` — width of the wave highlight
- `WaveDirection` — 5 directions (Default, LeftToRight, RightToLeft, TopToBottom, BottomToTop)
- `RepeatCount` — repeat the shimmer pattern vertically
- `AnimationDuration` — control animation speed in milliseconds

