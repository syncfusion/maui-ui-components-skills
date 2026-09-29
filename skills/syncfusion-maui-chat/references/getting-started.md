# Getting Started with .NET MAUI Chat (SfChat)

## Table of Contents
- [Step 1: Install the NuGet Package](#step-1-install-the-nuget-package)
- [Step 2: Register the Handler](#step-2-register-the-handler)
- [Step 3: Add SfChat to a Page](#step-3-add-sfchat-to-a-page)
- [Step 4: Define the ViewModel](#step-4-define-the-viewmodel)
- [Step 5: Understanding CurrentUser](#step-5-understanding-currentuser)
- [Step 6: Avatar Images](#step-6-avatar-images)
- [Common Setup Mistakes](#common-setup-mistakes)

---

## Step 1: Install the NuGet Package

Install the `Syncfusion.Maui.Chat` NuGet package:

**Via .NET CLI:**
```bash
dotnet add package Syncfusion.Maui.Chat
```

**Via NuGet Package Manager:**
1. Right-click the project in **Solution Explorer**
2. Choose **Manage NuGet Packages**
3. Search for [Syncfusion.Maui.Chat](https://www.nuget.org/packages/Syncfusion.Maui.Chat) and install the latest version
4. Ensure all dependencies are restored

> [Syncfusion.Maui.Core](https://www.nuget.org/packages/Syncfusion.Maui.Core) is a required transitive dependency — it is installed automatically.

---

## Step 2: Register the Handler

Register the Syncfusion core handler in `MauiProgram.cs`. This is required for all Syncfusion .NET MAUI controls.

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

        builder.ConfigureSyncfusionCore(); // Required
        return builder.Build();
    }
}
```

> If `ConfigureSyncfusionCore()` is missing, controls will not render and a handler registration error will occur at runtime.

---

## Step 3: Add SfChat to a Page

Import the namespace and declare `SfChat` in XAML or C#.

**XAML:**
```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sfChat="clr-namespace:Syncfusion.Maui.Chat;assembly=Syncfusion.Maui.Chat"
             xmlns:local="clr-namespace:GettingStarted"
             x:Class="GettingStarted.MainPage">

    <ContentPage.BindingContext>
        <local:ChatViewModel/>
    </ContentPage.BindingContext>

    <ContentPage.Content>
        <sfChat:SfChat x:Name="sfChat"
                       Messages="{Binding Messages}"
                       CurrentUser="{Binding CurrentUser}" />
    </ContentPage.Content>
</ContentPage>
```

**C# code-behind:**
```csharp
using Syncfusion.Maui.Chat;

public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        SfChat sfChat = new SfChat();
        ChatViewModel viewModel = new ChatViewModel();
        sfChat.Messages = viewModel.Messages;
        sfChat.CurrentUser = viewModel.CurrentUser;
        this.Content = sfChat;
    }
}
```

---

## Step 4: Define the ViewModel

`SfChat` is data-bound. You need to provide:
- `Messages` — an `ObservableCollection<object>` containing message objects
- `CurrentUser` — an `Author` representing the local user (determines outgoing vs. incoming)

```csharp
// ChatViewModel.cs
using System.Collections.ObjectModel;
using System.ComponentModel;
using Syncfusion.Maui.Chat;

public class ChatViewModel : INotifyPropertyChanged
{
    private ObservableCollection<object> messages;
    private Author currentUser;

    public ChatViewModel()
    {
        this.currentUser = new Author { Name = "Nancy" };
        this.messages = new ObservableCollection<object>();
        GenerateMessages();
    }

    public ObservableCollection<object> Messages
    {
        get => messages;
        set { messages = value; OnPropertyChanged(nameof(Messages)); }
    }

    public Author CurrentUser
    {
        get => currentUser;
        set { currentUser = value; OnPropertyChanged(nameof(CurrentUser)); }
    }

    private void GenerateMessages()
    {
        messages.Add(new TextMessage
        {
            Author = currentUser,
            Text = "Hi! I'm very excited to share that our team is launching a new mobile application.",
        });

        messages.Add(new TextMessage
        {
            Author = new Author { Name = "Andrea", Avatar = "andrea.png" },
            Text = "Oh! That's great news.",
        });

        messages.Add(new TextMessage
        {
            Author = new Author { Name = "Harrison", Avatar = "harrison.png" },
            Text = "Looking forward to it!",
        });
    }

    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string name) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
}
```

---

## Step 5: Understanding CurrentUser

`CurrentUser` is the key to how SfChat renders messages:

- Messages whose `Author` matches `CurrentUser` ? displayed as **outgoing** (right side)
- All other messages ? displayed as **incoming** (left side)

```csharp
// Outgoing — Author is CurrentUser
new TextMessage { Author = currentUser, Text = "Hello!" }

// Incoming — Author is someone else
new TextMessage { Author = new Author { Name = "Bot" }, Text = "Hi there!" }
```

> Always set `CurrentUser` before binding `Messages` to avoid layout inconsistencies.

---

## Step 6: Avatar Images

Place avatar image files in the **Resources/Images** folder of your MAUI project. Reference them by filename:

```csharp
new Author { Name = "Andrea", Avatar = "andrea.png" }
```

> If no avatar is set, SfChat automatically shows the author's initials as a placeholder.

---

## Common Setup Mistakes

| Problem | Cause | Fix |
|---|---|---|
| **Compilation error: "namespace not found"** | **NuGet package not installed** | **Run `dotnet add package Syncfusion.Maui.Chat` and `dotnet restore`** |
| Control not rendering | Missing handler registration | Add `builder.ConfigureSyncfusionCore()` in `MauiProgram.cs` |
| Messages show as all outgoing | `CurrentUser` not set | Set `CurrentUser` before binding `Messages` |
| Avatar images not showing | Image not in Resources/Images | Add images to `Resources/Images` with Build Action = `MauiImage` |
| NuGet restore error | Missing dependencies | Run `dotnet restore` after install |
