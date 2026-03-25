# Getting Started with .NET MAUI Accordion

This guide walks you through setting up and configuring the Syncfusion .NET MAUI Accordion (SfAccordion) in your application.

## Installation

### Step 1: Install the NuGet Package

The Accordion control is part of the `Syncfusion.Maui.Expander` package.

**Option A: Using .NET CLI**
```bash
dotnet add package Syncfusion.Maui.Expander
```

**Option B: Using Package Manager Console (Visual Studio)**
```powershell
Install-Package Syncfusion.Maui.Expander
```

**Option C: Using NuGet Package Manager UI**
1. Right-click the project in Solution Explorer
2. Select **Manage NuGet Packages**
3. Search for `Syncfusion.Maui.Expander`
4. Click **Install**

### Step 2: Restore Dependencies

After installation, restore the project:
```bash
dotnet restore
```

## Handler Registration

The `Syncfusion.Maui.Core` NuGet is a required dependency for all Syncfusion .NET MAUI controls. You must register the Syncfusion handler in your `MauiProgram.cs` file.

**MauiProgram.cs:**
```csharp
using Microsoft.Maui;
using Microsoft.Maui.Hosting;
using Microsoft.Maui.Controls.Hosting;
using Syncfusion.Maui.Core.Hosting;

namespace YourAppNamespace
{
    public static class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            var builder = MauiApp.CreateBuilder();
            builder
                .UseMauiApp<App>()
                .ConfigureFonts(fonts =>
                {
                    fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
                    fonts.AddFont("OpenSans-Semibold.ttf", "OpenSansSemibold");
                })
                builder.ConfigureSyncfusionCore(); // ← Register Syncfusion handler
            
            return builder.Build();
        }
    }
}
```

**⚠️ Important:** Call `ConfigureSyncfusionCore()` before `Build()`. Failure to register the handler will result in runtime errors.

## Basic Accordion Setup

### Import the Namespace

Add the Syncfusion.Maui.Accordion namespace to your XAML or C# file.

**XAML:**
```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:syncfusion="clr-namespace:Syncfusion.Maui.Accordion;assembly=Syncfusion.Maui.Expander"
             x:Class="YourApp.MainPage">
    <!-- Content -->
</ContentPage>
```

**C#:**
```csharp
using Syncfusion.Maui.Accordion;
```

### Initialize SfAccordion

**XAML:**
```xml
<ContentPage xmlns:syncfusion="clr-namespace:Syncfusion.Maui.Accordion;assembly=Syncfusion.Maui.Expander">
    <syncfusion:SfAccordion />
</ContentPage>
```

**C#:**
```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        SfAccordion accordion = new SfAccordion();
        this.Content = accordion;
    }
}
```

## Defining Accordion Items

Each accordion item consists of a **Header** and **Content**. Both accept any .NET MAUI `View`.

**⚠️ Important:** Do not use `Label` directly as a child of `Header` or `Content`. Always wrap it inside a container like `Grid` to avoid crashes.

### Example: Simple Accordion with Items

```xml
<syncfusion:SfAccordion>
    <syncfusion:SfAccordion.Items>
        <!-- First Item -->
        <syncfusion:AccordionItem>
            <syncfusion:AccordionItem.Header>
                <Grid HeightRequest="48">
                    <Label Text="Robin Rane" 
                           Margin="16,14,0,14" 
                           CharacterSpacing="0.25" 
                           FontFamily="Roboto-Regular" 
                           FontSize="14" />
                </Grid>
            </syncfusion:AccordionItem.Header>
            <syncfusion:AccordionItem.Content>
                <Grid BackgroundColor="#f4f4f4" Padding="16">
                    <StackLayout>
                        <Label Text="Position: Chairman" FontSize="14" />
                        <Label Text="Organization: ABC Inc." FontSize="14" />
                        <Label Text="Location: Boston" FontSize="14" />
                        <Label Text="Phone: (617) 555-1234" FontSize="14" />
                    </StackLayout>
                </Grid>
            </syncfusion:AccordionItem.Content>
        </syncfusion:AccordionItem>

        <!-- Second Item -->
        <syncfusion:AccordionItem>
            <syncfusion:AccordionItem.Header>
                <Grid HeightRequest="48">
                    <Label Text="Paul Vent" 
                           Margin="16,14,0,14" 
                           FontSize="14" />
                </Grid>
            </syncfusion:AccordionItem.Header>
            <syncfusion:AccordionItem.Content>
                <Grid BackgroundColor="#f4f4f4" Padding="16">
                    <StackLayout>
                        <Label Text="Position: General Manager" FontSize="14" />
                        <Label Text="Organization: XYZ Corp." FontSize="14" />
                        <Label Text="Location: New York" FontSize="14" />
                        <Label Text="Phone: (212) 555-1234" FontSize="14" />
                    </StackLayout>
                </Grid>
            </syncfusion:AccordionItem.Content>
        </syncfusion:AccordionItem>
    </syncfusion:SfAccordion.Items>
</syncfusion:SfAccordion>
```

## Expand Modes

The `ExpandMode` property controls how many items can be expanded simultaneously.

### Single Mode (Default)

Only one item can be expanded at a time. Expanding a new item collapses the previously expanded item.

```xml
<syncfusion:SfAccordion ExpandMode="Single">
    <!-- Items -->
</syncfusion:SfAccordion>
```

```csharp
accordion.ExpandMode = AccordionExpandMode.Single;
```

### Multiple Mode

Multiple items can be expanded simultaneously. Expanding a new item does not collapse others.

```xml
<syncfusion:SfAccordion ExpandMode="Multiple">
    <!-- Items -->
</syncfusion:SfAccordion>
```

```csharp
accordion.ExpandMode = AccordionExpandMode.Multiple;
```

### MultipleOrNone Mode

Multiple items can be expanded, and tapping an expanded item's header collapses it without expanding another.

```xml
<syncfusion:SfAccordion ExpandMode="MultipleOrNone">
    <!-- Items -->
</syncfusion:SfAccordion>
```

```csharp
accordion.ExpandMode = AccordionExpandMode.MultipleOrNone;
```

## Animation Customization

### Animation Duration

Customize the expand/collapse animation duration (in milliseconds). Default is 200ms.

```xml
<syncfusion:SfAccordion AnimationDuration="300">
    <!-- Items -->
</syncfusion:SfAccordion>
```

```csharp
accordion.AnimationDuration = 300;
```

### Animation Easing

Control the animation easing style. Default is `Linear`.

**Available Easing Options:**
- Linear
- SinOut
- SinIn
- SinInOut
- CubicIn
- CubicOut
- CubicInOut

```xml
<syncfusion:SfAccordion AnimationDuration="250" 
                        AnimationEasing="SinOut">
    <!-- Items -->
</syncfusion:SfAccordion>
```

```csharp
accordion.AnimationDuration = 250;
accordion.AnimationEasing = ExpanderAnimationEasing.SinOut;
```

## Item Spacing

Add vertical spacing between accordion items using the `ItemSpacing` property.

```xml
<syncfusion:SfAccordion ItemSpacing="8">
    <!-- Items -->
</syncfusion:SfAccordion>
```

```csharp
accordion.ItemSpacing = 8.0;
```

## Auto Scroll Position

Control where the expanded item scrolls to after expansion using `AutoScrollPosition`. Default is `MakeVisible`.

**Options:**
- **MakeVisible**: Scrolls just enough to make the item visible
- **Top**: Scrolls the expanded item to the top of the view

```xml
<syncfusion:SfAccordion AutoScrollPosition="Top">
    <!-- Items -->
</syncfusion:SfAccordion>
```

```csharp
accordion.AutoScrollPosition = AccordionAutoScrollPosition.Top;
```

## Programmatic Scrolling with BringIntoView

Use the `BringIntoView` method to programmatically scroll a specific item into view.

```csharp
// Bring the item at index 5 into view
accordion.BringIntoView(accordion.Items[5]);
```

**Example with Button:**
```xml
<StackLayout>
    <Button Text="Scroll to Item 5" Clicked="ScrollButton_Clicked" />
    <syncfusion:SfAccordion x:Name="accordion">
        <!-- 10+ items -->
    </syncfusion:SfAccordion>
</StackLayout>
```

```csharp
private void ScrollButton_Clicked(object sender, EventArgs e)
{
    if (accordion.Items.Count > 5)
    {
        accordion.BringIntoView(accordion.Items[5]);
    }
}
```

## Running the Application

Press **F5** or run:
```bash
dotnet build
dotnet run
```

The accordion will display with your configured items and settings.

## Complete Example

Here's a complete working example:

**MainPage.xaml:**
```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:syncfusion="clr-namespace:Syncfusion.Maui.Accordion;assembly=Syncfusion.Maui.Expander"
             x:Class="AccordionApp.MainPage">
    
    <syncfusion:SfAccordion ExpandMode="Single" 
                            AnimationDuration="250"
                            AnimationEasing="SinOut"
                            ItemSpacing="6"
                            AutoScrollPosition="MakeVisible">
        <syncfusion:SfAccordion.Items>
            <syncfusion:AccordionItem>
                <syncfusion:AccordionItem.Header>
                    <Grid HeightRequest="48" BackgroundColor="White">
                        <Label Text="Getting Started" 
                               Margin="16,14,0,14" 
                               FontSize="14" 
                               VerticalTextAlignment="Center" />
                    </Grid>
                </syncfusion:AccordionItem.Header>
                <syncfusion:AccordionItem.Content>
                    <Grid BackgroundColor="#f4f4f4" Padding="16">
                        <Label Text="Learn the basics of .NET MAUI Accordion, including installation and setup."
                               LineBreakMode="WordWrap" />
                    </Grid>
                </syncfusion:AccordionItem.Content>
            </syncfusion:AccordionItem>
        </syncfusion:SfAccordion.Items>
    </syncfusion:SfAccordion>
</ContentPage>
```

## Next Steps

- **Data Binding:** See [data-binding.md](data-binding.md) to bind accordion items to ObservableCollection
- **Appearance:** See [appearance-customization.md](appearance-customization.md) to customize colors, icons, and Visual States
- **Events:** See [events.md](events.md) to handle expansion/collapse events
- **Advanced:** See [advanced-features.md](advanced-features.md) for Liquid Glass Effect and more

## Troubleshooting

**Problem:** "Handler not registered" error
**Solution:** Ensure `ConfigureSyncfusionCore()` is called in `MauiProgram.cs`

**Problem:** Label crashes when used as Header/Content
**Solution:** Wrap Label inside a Grid or other container

**Problem:** NuGet restore fails
**Solution:** Run `dotnet restore` manually and check your internet connection

**Problem:** Accordion doesn't display
**Solution:** Verify the Syncfusion.Maui.Expander package is installed and the namespace is imported correctly
