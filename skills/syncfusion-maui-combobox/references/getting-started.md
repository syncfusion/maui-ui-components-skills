# Getting Started with .NET MAUI ComboBox

This guide walks through setting up and configuring a ComboBox in your .NET MAUI application, from installation to basic implementation.

## Installation Steps

### Step 1: Create a New MAUI Project

**Visual Studio:**
1. Go to **File > New > Project** and choose the **.NET MAUI App** template
2. Name the project and choose a location, then click **Next**
3. Select the .NET framework version and click **Create**

**Visual Studio Code:**
1. Open the Command Palette by pressing **Ctrl+Shift+P**
2. Type **.NET:New Project** and press Enter
3. Choose the **.NET MAUI App** template
4. Select the project location, type the project name and press Enter
5. Choose **Create project**

### Step 2: Install the Syncfusion MAUI Inputs NuGet Package

**Visual Studio:**
1. In **Solution Explorer**, right-click the project and choose **Manage NuGet Packages**
2. Search for [Syncfusion.Maui.Inputs](https://www.nuget.org/packages/Syncfusion.Maui.Inputs)
3. Install the latest version
4. Ensure the necessary dependencies are installed correctly, and the project is restored

**Visual Studio Code:**
1. Press <kbd>Ctrl</kbd> + <kbd>`</kbd> (backtick) to open the integrated terminal
2. Ensure you're in the project root directory where your .csproj file is located
3. Run the command: `dotnet add package Syncfusion.Maui.Inputs`
4. To ensure all dependencies are installed, run: `dotnet restore`

### Step 3: Register the Syncfusion Core Handler

The [Syncfusion.Maui.Core](https://www.nuget.org/packages/Syncfusion.Maui.Core) NuGet package is a dependent package for all Syncfusion controls. In the `MauiProgram.cs` file, register the handler for Syncfusion core.

```csharp
using Microsoft.Maui;
using Microsoft.Maui.Hosting;
using Microsoft.Maui.Controls.Compatibility;
using Microsoft.Maui.Controls.Hosting;
using Syncfusion.Maui.Core.Hosting;

namespace ComboBoxSample
{
    public static class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            var builder = MauiApp.CreateBuilder();
            builder
                .UseMauiApp<App>()
                .ConfigureSyncfusionCore()  // Register Syncfusion handler
                .ConfigureFonts(fonts =>
                {
                    fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
                });

            return builder.Build();
        }
    }
}
```

## Basic ComboBox Implementation

### Step 4: Add the Namespace

Add the namespace for Syncfusion controls in your XAML or C# file.

**XAML:**
```xml
xmlns:editors="clr-namespace:Syncfusion.Maui.Inputs;assembly=Syncfusion.Maui.Inputs"
```

**C#:**
```csharp
using Syncfusion.Maui.Inputs;
```

### Step 5: Add a Basic ComboBox Control

**XAML:**
```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:editors="clr-namespace:Syncfusion.Maui.Inputs;assembly=Syncfusion.Maui.Inputs"
             x:Class="ComboBoxSample.MainPage">

    <ContentPage.Content>
        <editors:SfComboBox x:Name="comboBox" 
                            WidthRequest="250"
                            HeightRequest="50" />
    </ContentPage.Content>
</ContentPage>
```

**C#:**
```csharp
using Syncfusion.Maui.Inputs;

SfComboBox comboBox = new SfComboBox
{
    WidthRequest = 250,
    HeightRequest = 50
};
Content = comboBox;
```

## Data Binding

### Step 6: Create Model and ViewModel

Define a simple model class and populate data in a ViewModel.

```csharp
// Model.cs
public class SocialMedia
{
    public string Name { get; set; }
    public int ID { get; set; }
}

// ViewModel.cs
public class SocialMediaViewModel
{
    public ObservableCollection<SocialMedia> SocialMedias { get; set; }
    
    public SocialMediaViewModel()
    {
        this.SocialMedias = new ObservableCollection<SocialMedia>();
        this.SocialMedias.Add(new SocialMedia { Name = "Facebook", ID = 0 });
        this.SocialMedias.Add(new SocialMedia { Name = "Google Plus", ID = 1 });
        this.SocialMedias.Add(new SocialMedia { Name = "Instagram", ID = 2 });
        this.SocialMedias.Add(new SocialMedia { Name = "LinkedIn", ID = 3 });
        this.SocialMedias.Add(new SocialMedia { Name = "Skype", ID = 4 });
        this.SocialMedias.Add(new SocialMedia { Name = "Telegram", ID = 5 });
        this.SocialMedias.Add(new SocialMedia { Name = "Tik Tok", ID = 6 });
        this.SocialMedias.Add(new SocialMedia { Name = "Twitter", ID = 7 });
        this.SocialMedias.Add(new SocialMedia { Name = "WhatsApp", ID = 8 });
        this.SocialMedias.Add(new SocialMedia { Name = "YouTube", ID = 9 });
    }
}
```

### Step 7: Bind Data to ComboBox

Bind the ViewModel data to the ComboBox control using the `ItemsSource` property.

**XAML:**
```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:editors="clr-namespace:Syncfusion.Maui.Inputs;assembly=Syncfusion.Maui.Inputs"
             xmlns:local="clr-namespace:ComboBoxSample"
             x:Class="ComboBoxSample.MainPage">

    <ContentPage.BindingContext>
        <local:SocialMediaViewModel />
    </ContentPage.BindingContext>

    <ContentPage.Content>
        <editors:SfComboBox x:Name="comboBox"
                            WidthRequest="250"
                            HeightRequest="50"
                            ItemsSource="{Binding SocialMedias}" />
    </ContentPage.Content>
</ContentPage>
```

**C#:**
```csharp
SocialMediaViewModel socialMediaViewModel = new SocialMediaViewModel();
this.BindingContext = socialMediaViewModel;

SfComboBox comboBox = new SfComboBox
{
    WidthRequest = 250,
    HeightRequest = 50,
    ItemsSource = socialMediaViewModel.SocialMedias
};
Content = comboBox;
```

**Note:** Set the BindingContext of your page to an instance of the ViewModel to enable data binding.

### Step 8: Set DisplayMemberPath and TextMemberPath

The ComboBox model contains two properties (ID and Name), so you need to specify which property to display in the selection box.

- **TextMemberPath:** Property path used to get the value for displaying in the selection box when an item is selected. Default: `string.Empty`
- **DisplayMemberPath:** Property path used to display each data item in the dropdown list. Default: `string.Empty`

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    WidthRequest="250"
                    HeightRequest="50"
                    DisplayMemberPath="Name"
                    TextMemberPath="Name"
                    ItemsSource="{Binding SocialMedias}" />
```

**C#:**
```csharp
SfComboBox comboBox = new SfComboBox
{
    WidthRequest = 250,
    HeightRequest = 50,
    DisplayMemberPath = "Name",
    TextMemberPath = "Name",
    ItemsSource = socialMediaViewModel.SocialMedias
};
```

**Result:** Non-editable ComboBox with data binding.

## Enabling Editable Mode

The ComboBox supports both editable and non-editable modes. To enable editing functionality, set the `IsEditable` property to `true`. The default value is `false`.

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    WidthRequest="250"
                    HeightRequest="50"
                    IsEditable="true"
                    ItemsSource="{Binding SocialMedias}"
                    DisplayMemberPath="Name"
                    TextMemberPath="Name" />
```

**C#:**
```csharp
SfComboBox comboBox = new SfComboBox
{
    WidthRequest = 250,
    HeightRequest = 50,
    IsEditable = true,
    ItemsSource = socialMediaViewModel.SocialMedias,
    DisplayMemberPath = "Name",
    TextMemberPath = "Name"
};
```

In editable mode, the ComboBox allows users to type text and automatically appends the remaining letters when the input is valid.

## Getting the Selected Text

The `Text` property is used to get the user-submitted text in the editable mode. The default value is `string.Empty`.

```csharp
string selectedText = comboBox.Text;
```

## AOT Publishing Note

When publishing in AOT (Ahead-of-Time) mode on iOS, ensure `[Preserve(AllMembers = true)]` is added to the model class to maintain `DisplayMemberPath` binding:

```csharp
using Foundation;

[Preserve(AllMembers = true)]
public class SocialMedia
{
    public string Name { get; set; }
    public int ID { get; set; }
}
```

## Complete Example

You can find the complete getting started sample from this [GitHub link](https://github.com/SyncfusionExamples/maui-combobox-samples).

## Next Steps

- Explore [Editing Modes](editing-modes.md) for more control over editable behavior
- Learn about [Selection](selection.md) for single and multiple selection modes
- Configure [Filtering](filtering.md) to enable searchable dropdowns
- Customize the [UI](ui-customization.md) with styling and templates
