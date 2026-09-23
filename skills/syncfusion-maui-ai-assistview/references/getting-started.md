# Getting Started with SfAIAssistView

## Table of Contents
- [Step 1: Install NuGet Package](#step-1-install-nuget-package)
- [Step 2: Register the Handler](#step-2-register-the-handler)
- [Step 3: Initialize SfAIAssistView](#step-3-initialize-sfaiassistview)
- [Step 4: Define the ViewModel](#step-4-define-the-viewmodel)
- [Step 5: Bind AssistItems](#step-5-bind-assistitems)
- [Request and Response Items](#request-and-response-items)
- [Troubleshooting](#troubleshooting)

---

## Step 1: Install NuGet Package

### Visual Studio
1. Right-click the project in **Solution Explorer** ? **Manage NuGet Packages**
2. Search for `Syncfusion.Maui.AIAssistView` and install the latest version
3. Ensure dependencies are restored correctly

### .NET CLI
```bash
dotnet add package Syncfusion.Maui.AIAssistView
```

> `Syncfusion.Maui.Core` is installed automatically as a dependency. It is required by all Syncfusion .NET MAUI controls.

---

## Step 2: Register the Handler

In `MauiProgram.cs`, call `ConfigureSyncfusionCore()` to register the Syncfusion handler. Without this step the control will not render.

```csharp
// MauiProgram.cs
using Syncfusion.Maui.Core.Hosting;

public class MauiProgram
{
    public static MauiApp CreateMauiApp()
    {
        var builder = MauiApp.CreateBuilder();
        builder
            .UseMauiApp<App>()
            .ConfigureFonts(fonts =>
            {
                fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
            });

        builder.ConfigureSyncfusionCore();  // Required
        return builder.Build();
    }
}
```

---

## Step 3: Initialize SfAIAssistView

### XAML
Import the namespace and add the control to your page:

```xml
<!-- MainPage.xaml -->
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:syncfusion="clr-namespace:Syncfusion.Maui.AIAssistView;assembly=Syncfusion.Maui.AIAssistView"
             x:Class="MyApp.MainPage">

    <ContentPage.Content>
        <syncfusion:SfAIAssistView x:Name="sfAIAssistView" />
    </ContentPage.Content>
</ContentPage>
```

### C# (Code-Behind)
```csharp
using Syncfusion.Maui.AIAssistView;

public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        SfAIAssistView sfAIAssistView = new SfAIAssistView();
        this.Content = sfAIAssistView;
    }
}
```

---

## Step 4: Define the ViewModel

Create a ViewModel that holds the conversation items and handles the request command. The `AssistItems` property is of type `IList<IAssistItem>` — use `ObservableCollection<IAssistItem>` to get automatic UI updates.

```csharp
// ViewModel.cs
using Syncfusion.Maui.AIAssistView;

public class ViewModel : INotifyPropertyChanged
{
    private ObservableCollection<IAssistItem> assistItems;

    public ObservableCollection<IAssistItem> AssistItems
    {
        get => assistItems;
        set
        {
            assistItems = value;
            RaisePropertyChanged(nameof(AssistItems));
        }
    }

    public ICommand AssistViewRequestCommand { get; set; }

    public ViewModel()
    {
        assistItems = new ObservableCollection<IAssistItem>();
        AssistViewRequestCommand = new Command<object>(ExecuteRequestCommand);
        GenerateInitialItems();
    }

    private void GenerateInitialItems()
    {
        // Add an initial request item
        AssistItems.Add(new AssistItem
        {
            Text = "What is .NET MAUI?",
            IsRequested = true
        });

        // Simulate an AI response
        _ = GetResultAsync();
    }

    private async Task GetResultAsync()
    {
        await Task.Delay(1000).ConfigureAwait(true);

        AssistItems.Add(new AssistItem
        {
            Text = "MAUI stands for .NET Multi-platform App UI. It lets you build " +
                   "cross-platform apps for iOS, Android, macOS, and Windows from " +
                   "a single C# codebase.",
            IsRequested = false
        });
    }

    private async void ExecuteRequestCommand(object obj)
    {
        var args = obj as RequestEventArgs;
        var requestItem = args?.RequestItem as AssistItem;
        if (requestItem == null) return;

        // Replace this with your actual AI service call
        await Task.Delay(1000).ConfigureAwait(true);

        AssistItems.Add(new AssistItem
        {
            Text = $"Response to: {requestItem.Text}",
            IsRequested = false,
            RequestItem = requestItem
        });
    }

    public event PropertyChangedEventHandler PropertyChanged;
    protected void RaisePropertyChanged(string name) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
}
```

---

## Step 5: Bind AssistItems

Set the ViewModel as `BindingContext` and bind `AssistItems` and `RequestCommand`.

### XAML
```xml
<!-- MainPage.xaml -->
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:syncfusion="clr-namespace:Syncfusion.Maui.AIAssistView;assembly=Syncfusion.Maui.AIAssistView"
             xmlns:local="clr-namespace:MyApp"
             x:Class="MyApp.MainPage">

    <ContentPage.BindingContext>
        <local:ViewModel />
    </ContentPage.BindingContext>

    <ContentPage.Content>
        <syncfusion:SfAIAssistView
            x:Name="sfAIAssistView"
            AssistItems="{Binding AssistItems}"
            RequestCommand="{Binding AssistViewRequestCommand}" />
    </ContentPage.Content>
</ContentPage>
```

### C# (Code-Behind)
```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        var viewModel = new ViewModel();
        var sfAIAssistView = new SfAIAssistView
        {
            AssistItems = viewModel.AssistItems
        };
        sfAIAssistView.SetBinding(
            SfAIAssistView.RequestCommandProperty,
            new Binding("AssistViewRequestCommand"));
        this.BindingContext = viewModel;
        this.Content = sfAIAssistView;
    }
}
```

---

## Request and Response Items

The `IsRequested` property on `AssistItem` determines how the item is displayed:

| `IsRequested` | Meaning | Alignment |
|---|---|---|
| `true` | Sent by the user | Right side |
| `false` | AI response | Left side |

```csharp
// User request
var requestItem = new AssistItem
{
    Text = "Tell me about .NET MAUI",
    IsRequested = true
};

// AI response — link it back to the request via RequestItem
var responseItem = new AssistItem
{
    Text = "MAUI is a cross-platform UI framework...",
    IsRequested = false,
    RequestItem = requestItem   // Associates response with its originating request
};

AssistItems.Add(requestItem);
AssistItems.Add(responseItem);
```

> **Note:** When adding items programmatically (not via user input), always set `IsRequested = true` explicitly for request items. The control does not infer this automatically.

> **Note:** Use `CurrentUser` on `SfAIAssistView` to specify the profile of the person sending requests.

---

## Troubleshooting

| Issue | Cause | Fix |
|---|---|---|
| Control not rendering / blank page | Handler not registered | Call `builder.ConfigureSyncfusionCore()` in `MauiProgram.cs` |
| NuGet restore fails | Network or version conflict | Run `dotnet restore`; check for conflicting `Syncfusion.Maui.Core` versions |
| Items not updating in UI | Wrong collection type | Use `ObservableCollection<IAssistItem>`, not `List<IAssistItem>` |
| Request command not firing | Binding missing | Ensure `RequestCommand="{Binding ...}"` is set on the control |
| Response appears on the wrong side | `IsRequested` not set | Set `IsRequested = true` for user items, `false` for AI responses |
