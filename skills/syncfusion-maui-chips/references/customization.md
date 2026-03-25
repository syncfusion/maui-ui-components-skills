# Customization in .NET MAUI Chips

## Table of Contents
- [Overview](#overview)
- [SfChip Customization](#sfchip-customization)
- [SfChipGroup Customization](#sfchipgroup-customization)
- [Command for MVVM](#command-for-mvvm)
- [Complete Styling Examples](#complete-styling-examples)

## Overview

The Syncfusion .NET MAUI Chips control provides extensive customization options for both individual chips (`SfChip`) and chip groups (`SfChipGroup`). You can customize colors, borders, fonts, icons, images, and more to match your application's design.

## SfChip Customization

Individual `SfChip` instances can be customized with the following properties.

### Close Button

#### ShowCloseButton

Shows or hides the close button on the chip.

```xaml
<chip:SfChip Text="James" 
             ShowCloseButton="True" />
```

```csharp
SfChip chip = new SfChip
{
    Text = "James",
    ShowCloseButton = true
};
```

**Default:** `false`

#### CloseButtonColor

Customizes the color of the close button.

```xaml
<chip:SfChip Text="James" 
             ShowCloseButton="True"
             CloseButtonColor="Red" />
```

```csharp
SfChip chip = new SfChip
{
    Text = "James",
    ShowCloseButton = true,
    CloseButtonColor = Colors.Red
};
```

**Default:** `Color.FromArgb("#49454E")`

### Selection Indicator

#### ShowSelectionIndicator

Shows or hides the selection indicator (check mark) on the chip.

```xaml
<chip:SfChip Text="James"
             ShowSelectionIndicator="True" />
```

```csharp
SfChip chip = new SfChip
{
    Text = "James",
    ShowSelectionIndicator = true
};
```

**Default:** `false`

#### SelectionIndicatorColor

Customizes the color of the selection indicator.

```xaml
<chip:SfChip Text="James" 
             ShowSelectionIndicator="True"
             SelectionIndicatorColor="Yellow" />
```

```csharp
SfChip chip = new SfChip
{
    Text = "James",
    ShowSelectionIndicator = true,
    SelectionIndicatorColor = Colors.Yellow
};
```

**Default:** `Color.FromRgb(30, 25, 43)`

### Background and Border

#### Background

Customizes the background color of the chip.

```xaml
<chip:SfChip Text="James"
             Background="LightCoral" />
```

```csharp
SfChip chip = new SfChip
{
    Text = "James",
    Background = Colors.LightCoral
};
```

You can also use gradients:

```xaml
<chip:SfChip Text="James">
    <chip:SfChip.Background>
        <LinearGradientBrush StartPoint="0,0" EndPoint="1,1">
            <GradientStop Color="LightBlue" Offset="0.0" />
            <GradientStop Color="Blue" Offset="1.0" />
        </LinearGradientBrush>
    </chip:SfChip.Background>
</chip:SfChip>
```

#### Stroke

Customizes the border color of the chip.

```xaml
<chip:SfChip Text="James"
             Stroke="Black" />
```

```csharp
SfChip chip = new SfChip
{
    Text = "James",
    Stroke = Colors.Black
};
```

#### StrokeThickness

Customizes the border thickness on all four sides.

```xaml
<chip:SfChip Text="James"
             StrokeThickness="3"
             Stroke="Black" />
```

```csharp
SfChip chip = new SfChip
{
    Text = "James",
    StrokeThickness = 3,
    Stroke = Colors.Black
};
```

#### CornerRadius

Customizes the rounded corners of the chip.

```xaml
<chip:SfChip Text="James"
             CornerRadius="25"
             Stroke="Black" />
```

```csharp
SfChip chip = new SfChip
{
    Text = "James",
    CornerRadius = 25,
    Stroke = Colors.Black
};
```

**Default:** `Thickness(8)`

You can set different radius for each corner:

```xaml
<chip:SfChip Text="James"
             CornerRadius="0,15,0,15" />
```

### Text Styling

#### TextColor

Customizes the color of the text.

```xaml
<chip:SfChip Text="James"
             TextColor="Red" />
```

```csharp
SfChip chip = new SfChip
{
    Text = "James",
    TextColor = Colors.Red
};
```

**Default:** `Color.FromArgb("#1C1B1F")`

#### FontSize

Customizes the size of the text.

```xaml
<chip:SfChip Text="James"
             FontSize="18" />
```

```csharp
SfChip chip = new SfChip
{
    Text = "James",
    FontSize = 18
};
```

**Default:** `14d`

#### FontAttributes

Customizes the font style (Bold, Italic, or both).

```xaml
<chip:SfChip Text="James"
             FontAttributes="Bold,Italic" />
```

```csharp
SfChip chip = new SfChip
{
    Text = "James",
    FontAttributes = FontAttributes.Bold | FontAttributes.Italic
};
```

**Default:** `FontAttributes.None`

#### FontFamily

Customizes the font family.

```xaml
<chip:SfChip Text="James"
             FontFamily="OpenSans-Semibold" />
```

```csharp
SfChip chip = new SfChip
{
    Text = "James",
    FontFamily = "OpenSans-Semibold"
};
```

#### Text Alignment

Customizes horizontal and vertical text alignment.

```xaml
<chip:SfChip Text="James"
             ShowCloseButton="True"
             HorizontalTextAlignment="Start"
             VerticalTextAlignment="Center" />
```

```csharp
SfChip chip = new SfChip
{
    Text = "James",
    ShowCloseButton = true,
    HorizontalTextAlignment = TextAlignment.Start,
    VerticalTextAlignment = TextAlignment.Center
};
```

**Default:** `TextAlignment.Center` for both

### Icon Customization

#### ShowIcon

Enables or disables the icon image.

```xaml
<chip:SfChip Text="James"
             ImageSource="user_icon.png"
             ShowIcon="True" />
```

```csharp
SfChip chip = new SfChip
{
    Text = "James",
    ImageSource = "user_icon.png",
    ShowIcon = true
};
```

**Default:** `false`

**Important:** You must set `ShowIcon="True"` for the icon to appear.

#### ImageSource

Specifies the icon image.

```xaml
<chip:SfChip Text="James"
             ImageSource="user_icon.png"
             ShowIcon="True" />
```

```csharp
SfChip chip = new SfChip
{
    Text = "James",
    ImageSource = "user_icon.png",
    ShowIcon = true
};
```

Supports:
- File-based images (`"user.png"`)
- Embedded resources
- URI-based images
- Font icons (see advanced-features.md)

#### ImageSize

Customizes the width and height of the icon.

```xaml
<chip:SfChip Text="James"
             ImageSource="user_icon.png"
             ImageSize="30"
             ShowIcon="True" />
```

```csharp
SfChip chip = new SfChip
{
    Text = "James",
    ImageSource = "user_icon.png",
    ImageSize = 30,
    ShowIcon = true
};
```

**Default:** `18d`

#### ImageAlignment

Specifies the icon position (Start or End).

```xaml
<chip:SfChip Text="James"
             ImageSource="user_icon.png"
             ImageAlignment="End"
             ShowIcon="True" />
```

```csharp
SfChip chip = new SfChip
{
    Text = "James",
    ImageSource = "user_icon.png",
    ImageAlignment = Alignment.End,
    ShowIcon = true
};
```

**Default:** `Alignment.Start`

### Background Image

#### BackgroundImageSource

Sets a background image for the entire chip.

```xaml
<chip:SfChip BackgroundImageSource="background.png" />
```

```csharp
SfChip chip = new SfChip
{
    BackgroundImageSource = "background.png"
};
```

**Use Case:** Create visually rich chips with photo backgrounds.

## SfChipGroup Customization

Customize all chips in a group simultaneously.

### Chip Colors

#### ChipBackground

Sets the default background color for all chips.

```xaml
<chip:SfChipGroup ItemsSource="{Binding Items}"
                  DisplayMemberPath="Name"
                  ChipBackground="#512dcd"
                  ChipTextColor="White"
                  CloseButtonColor="White" />
```

```csharp
SfChipGroup chipGroup = new SfChipGroup
{
    DisplayMemberPath = "Name",
    ChipBackground = Color.FromHex("#512dcd"),
    ChipTextColor = Colors.White,
    CloseButtonColor = Colors.White
};
chipGroup.SetBinding(SfChipGroup.ItemsSourceProperty, "Items");
```

**Default:** `Colors.Transparent`

#### SelectedChipBackground

Sets the background color for selected chips (Choice/Filter types).

```xaml
<chip:SfChipGroup ItemsSource="{Binding Items}"
                  DisplayMemberPath="Name"
                  ChipType="Filter"
                  ChipBackground="LightGray"
                  SelectedChipBackground="#512dcd" />
```

```csharp
SfChipGroup chipGroup = new SfChipGroup
{
    DisplayMemberPath = "Name",
    ChipType = SfChipsType.Filter,
    ChipBackground = Colors.LightGray,
    SelectedChipBackground = Color.FromHex("#512dcd")
};
chipGroup.SetBinding(SfChipGroup.ItemsSourceProperty, "Items");
```

**Note:** For more control, use VisualStateManager (see chip-types.md).

#### ChipTextColor

Sets the text color for all chips.

```xaml
<chip:SfChipGroup ItemsSource="{Binding Items}"
                  DisplayMemberPath="Name"
                  ChipTextColor="Red" />
```

```csharp
SfChipGroup chipGroup = new SfChipGroup
{
    DisplayMemberPath = "Name",
    ChipTextColor = Colors.Red
};
chipGroup.SetBinding(SfChipGroup.ItemsSourceProperty, "Items");
```

**Default:** `Color.FromArgb("#1C1B1F")`

#### SelectedChipTextColor

Sets the text color for selected chips.

```xaml
<chip:SfChipGroup ItemsSource="{Binding Items}"
                  DisplayMemberPath="Name"
                  ChipType="Filter"
                  ChipTextColor="Black"
                  SelectedChipTextColor="White"
                  SelectedChipBackground="#512dcd" />
```

```csharp
SfChipGroup chipGroup = new SfChipGroup
{
    DisplayMemberPath = "Name",
    ChipType = SfChipsType.Filter,
    ChipTextColor = Colors.Black,
    SelectedChipTextColor = Colors.White,
    SelectedChipBackground = Color.FromHex("#512dcd")
};
chipGroup.SetBinding(SfChipGroup.ItemsSourceProperty, "Items");
```

### Border Styling

#### ChipStroke

Sets the border color for all chips.

```xaml
<chip:SfChipGroup ItemsSource="{Binding Items}"
                  DisplayMemberPath="Name"
                  ChipStroke="Red"
                  ChipBackground="LightYellow" />
```

```csharp
SfChipGroup chipGroup = new SfChipGroup
{
    DisplayMemberPath = "Name",
    ChipStroke = Colors.Red,
    ChipBackground = Colors.LightYellow
};
chipGroup.SetBinding(SfChipGroup.ItemsSourceProperty, "Items");
```

**Default:** `Color.FromArgb("#79747E")`

#### ChipStrokeThickness

Sets the border width for all chips.

```xaml
<chip:SfChipGroup ItemsSource="{Binding Items}"
                  DisplayMemberPath="Name"
                  ChipStrokeThickness="3"
                  ChipStroke="Red" />
```

```csharp
SfChipGroup chipGroup = new SfChipGroup
{
    DisplayMemberPath = "Name",
    ChipStrokeThickness = 3,
    ChipStroke = Colors.Red
};
chipGroup.SetBinding(SfChipGroup.ItemsSourceProperty, "Items");
```

**Default:** `2d`

### Text Styling

#### ChipTextSize

Sets the font size for all chips.

```xaml
<chip:SfChipGroup ItemsSource="{Binding Items}"
                  DisplayMemberPath="Name"
                  ChipTextSize="16" />
```

```csharp
SfChipGroup chipGroup = new SfChipGroup
{
    DisplayMemberPath = "Name",
    ChipTextSize = 16
};
chipGroup.SetBinding(SfChipGroup.ItemsSourceProperty, "Items");
```

**Default:** `14d`

#### ChipFontAttributes

Sets the font style for all chips.

```xaml
<chip:SfChipGroup ItemsSource="{Binding Items}"
                  DisplayMemberPath="Name"
                  ChipFontAttributes="Bold" />
```

```csharp
SfChipGroup chipGroup = new SfChipGroup
{
    DisplayMemberPath = "Name",
    ChipFontAttributes = FontAttributes.Bold
};
chipGroup.SetBinding(SfChipGroup.ItemsSourceProperty, "Items");
```

**Default:** `FontAttributes.None`

#### ChipFontFamily

Sets the font family for all chips.

```xaml
<chip:SfChipGroup ItemsSource="{Binding Items}"
                  DisplayMemberPath="Name"
                  ChipFontFamily="OpenSans-Semibold" />
```

```csharp
SfChipGroup chipGroup = new SfChipGroup
{
    DisplayMemberPath = "Name",
    ChipFontFamily = "OpenSans-Semibold"
};
chipGroup.SetBinding(SfChipGroup.ItemsSourceProperty, "Items");
```

**Default:** `string.Empty`

### Layout and Spacing

#### ChipPadding

Sets spacing between chips.

```xaml
<chip:SfChipGroup ItemsSource="{Binding Items}"
                  DisplayMemberPath="Name"
                  ChipPadding="10,5,0,0" />
```

```csharp
SfChipGroup chipGroup = new SfChipGroup
{
    DisplayMemberPath = "Name",
    ChipPadding = new Thickness(10, 5, 0, 0)
};
chipGroup.SetBinding(SfChipGroup.ItemsSourceProperty, "Items");
```

**Default:** `Thickness(5, 0, 0, 0)`

**Format:** `Left, Top, Right, Bottom`

#### ItemHeight

Sets the height of all chips.

```xaml
<chip:SfChipGroup ItemsSource="{Binding Items}"
                  DisplayMemberPath="Name"
                  ItemHeight="50" />
```

```csharp
SfChipGroup chipGroup = new SfChipGroup
{
    DisplayMemberPath = "Name",
    ItemHeight = 50
};
chipGroup.SetBinding(SfChipGroup.ItemsSourceProperty, "Items");
```

**Default:** `double.NaN` (auto-sized)

### Icons and Indicators

#### ShowIcon

Enables icons for all chips in the group.

```xaml
<chip:SfChipGroup ItemsSource="{Binding Employees}"
                  DisplayMemberPath="Name"
                  ImageMemberPath="Image"
                  ShowIcon="True"
                  ChipImageSize="30" />
```

```csharp
SfChipGroup chipGroup = new SfChipGroup
{
    DisplayMemberPath = "Name",
    ImageMemberPath = "Image",
    ShowIcon = true,
    ChipImageSize = 30
};
chipGroup.SetBinding(SfChipGroup.ItemsSourceProperty, "Employees");
```

**Default:** `false`

#### ChipImageSize

Sets the icon size for all chips.

```xaml
<chip:SfChipGroup ItemsSource="{Binding Employees}"
                  DisplayMemberPath="Name"
                  ImageMemberPath="Image"
                  ShowIcon="True"
                  ChipImageSize="40" />
```

```csharp
SfChipGroup chipGroup = new SfChipGroup
{
    DisplayMemberPath = "Name",
    ImageMemberPath = "Image",
    ShowIcon = true,
    ChipImageSize = 40
};
chipGroup.SetBinding(SfChipGroup.ItemsSourceProperty, "Employees");
```

**Default:** `18d`

#### CloseButtonColor

Sets the close button color for all chips.

```xaml
<chip:SfChipGroup ItemsSource="{Binding Items}"
                  DisplayMemberPath="Name"
                  ChipType="Input"
                  CloseButtonColor="Red" />
```

```csharp
SfChipGroup chipGroup = new SfChipGroup
{
    DisplayMemberPath = "Name",
    ChipType = SfChipsType.Input,
    CloseButtonColor = Colors.Red
};
chipGroup.SetBinding(SfChipGroup.ItemsSourceProperty, "Items");
```

**Default:** `Color.FromArgb("#49454E")`

#### SelectionIndicatorColor

Sets the selection indicator color (Filter type).

```xaml
<chip:SfChipGroup ItemsSource="{Binding Items}"
                  DisplayMemberPath="Name"
                  ChipType="Filter"
                  SelectionIndicatorColor="White"
                  SelectedChipBackground="#512dcd" />
```

```csharp
SfChipGroup chipGroup = new SfChipGroup
{
    DisplayMemberPath = "Name",
    ChipType = SfChipsType.Filter,
    SelectionIndicatorColor = Colors.White,
    SelectedChipBackground = Color.FromHex("#512dcd")
};
chipGroup.SetBinding(SfChipGroup.ItemsSourceProperty, "Items");
```

**Default:** `Color.FromRgb(30, 25, 43)`

### Input View

#### InputView

Provides a custom view for adding chips (Input type only).

```xaml
<chip:SfChipGroup ItemsSource="{Binding Tags}"
                  DisplayMemberPath="Name"
                  ChipType="Input">
    <chip:SfChipGroup.InputView>
        <Entry x:Name="entry"
               Placeholder="Add tag"
               WidthRequest="110"
               HeightRequest="40"
               Completed="Entry_Completed" />
    </chip:SfChipGroup.InputView>
</chip:SfChipGroup>
```

```csharp
var entry = new Entry
{
    Placeholder = "Add tag",
    WidthRequest = 110,
    HeightRequest = 40
};
entry.Completed += Entry_Completed;

SfChipGroup chipGroup = new SfChipGroup
{
    DisplayMemberPath = "Name",
    ChipType = SfChipsType.Input,
    InputView = entry
};
chipGroup.SetBinding(SfChipGroup.ItemsSourceProperty, "Tags");
```

**Default:** `null`

## Command for MVVM

Both `SfChip` and `SfChipGroup` support the `Command` property for MVVM pattern.

### SfChip Command

```xaml
<ContentPage.BindingContext>
    <local:CommandViewModel />
</ContentPage.BindingContext>

<chip:SfChip Text="Click Me"
             Background="{Binding ChipBackground}"
             Command="{Binding ChipCommand}" />
```

```csharp
// ViewModel
public class CommandViewModel : INotifyPropertyChanged
{
    private Color chipBackground = Colors.Violet;
    
    public Color ChipBackground
    {
        get { return chipBackground; }
        set 
        { 
            chipBackground = value; 
            OnPropertyChanged(nameof(ChipBackground)); 
        }
    }
    
    public ICommand ChipCommand => new Command(() =>
    {
        ChipBackground = ChipBackground == Colors.Violet 
            ? Colors.DeepSkyBlue 
            : Colors.Violet;
    });
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

### SfChipGroup Command (Action Type)

```xaml
<chip:SfChipGroup ItemsSource="{Binding Actions}"
                  DisplayMemberPath="Name"
                  ChipType="Action"
                  Command="{Binding ActionCommand}" />

<Label Text="{Binding SelectedAction}" />
```

```csharp
// ViewModel
public class ActionViewModel : INotifyPropertyChanged
{
    private string selectedAction;
    
    public ObservableCollection<ActionItem> Actions { get; set; }
    
    public string SelectedAction
    {
        get { return selectedAction; }
        set 
        { 
            selectedAction = value; 
            OnPropertyChanged(nameof(SelectedAction)); 
        }
    }
    
    public ICommand ActionCommand => new Command<object>((item) =>
    {
        var action = item as ActionItem;
        SelectedAction = $"Executed: {action.Name}";
    });
    
    public ActionViewModel()
    {
        Actions = new ObservableCollection<ActionItem>
        {
            new ActionItem { Name = "Save" },
            new ActionItem { Name = "Edit" },
            new ActionItem { Name = "Delete" }
        };
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}

public class ActionItem
{
    public string Name { get; set; }
}
```

## Complete Styling Examples

### Material Design Style

```xaml
<chip:SfChipGroup ItemsSource="{Binding Categories}"
                  DisplayMemberPath="Name"
                  ChipType="Choice"
                  ChipTextSize="14"
                  ChipFontAttributes="None"
                  ChipPadding="8,8,0,0">
    <chip:SfChipGroup.ChipLayout>
        <FlexLayout Wrap="Wrap" HorizontalOptions="Start" />
    </chip:SfChipGroup.ChipLayout>
    
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">
                <VisualState.Setters>
                    <Setter Property="ChipBackground" Value="Transparent" />
                    <Setter Property="ChipTextColor" Value="#1C1B1F" />
                    <Setter Property="ChipStroke" Value="#79747E" />
                    <Setter Property="ChipStrokeThickness" Value="1" />
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="Selected">
                <VisualState.Setters>
                    <Setter Property="ChipBackground" Value="#E8DEF8" />
                    <Setter Property="ChipTextColor" Value="#1D1B20" />
                    <Setter Property="ChipStroke" Value="#E8DEF8" />
                    <Setter Property="ChipStrokeThickness" Value="1" />
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</chip:SfChipGroup>
```

### Dark Theme Style

```xaml
<chip:SfChipGroup ItemsSource="{Binding Items}"
                  DisplayMemberPath="Name"
                  ChipType="Filter"
                  ChipTextSize="14"
                  SelectionIndicatorColor="White"
                  ChipPadding="8,8,0,0">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">
                <VisualState.Setters>
                    <Setter Property="ChipBackground" Value="#2C2C2C" />
                    <Setter Property="ChipTextColor" Value="#E0E0E0" />
                    <Setter Property="ChipStroke" Value="#424242" />
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="Selected">
                <VisualState.Setters>
                    <Setter Property="ChipBackground" Value="#BB86FC" />
                    <Setter Property="ChipTextColor" Value="Black" />
                    <Setter Property="ChipStroke" Value="#BB86FC" />
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</chip:SfChipGroup>
```

### Pill-Shaped Chips

```xaml
<chip:SfChipGroup ItemsSource="{Binding Tags}"
                  DisplayMemberPath="Name"
                  ChipBackground="#F5F5F5"
                  ChipTextColor="#333333"
                  ItemHeight="36"
                  ChipPadding="10,5,0,0">
    <chip:SfChipGroup.ChipLayout>
        <FlexLayout Wrap="Wrap" />
    </chip:SfChipGroup.ChipLayout>
    
    <!-- Add CornerRadius through style if needed -->
</chip:SfChipGroup>
```
