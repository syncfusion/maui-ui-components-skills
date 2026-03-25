# UI Customization in .NET MAUI ComboBox

## Table of Contents
- [Overview](#overview)
- [Placeholder Customization](#placeholder-customization)
- [Button Customization](#button-customization)
  - [Clear Button](#clear-button)
  - [Dropdown Button](#dropdown-button)
- [Border and Stroke](#border-and-stroke)
- [Selection Highlight](#selection-highlight)
- [Custom View](#custom-view)
- [Dropdown Height](#dropdown-height)
- [Item Template](#item-template)
- [Dropdown Styling](#dropdown-styling)
- [Dropdown Item Styling](#dropdown-item-styling)
- [Dropdown Placement](#dropdown-placement)
- [Token Customization](#token-customization)
- [Text Alignment](#text-alignment)
- [Additional Settings](#additional-settings)
- [Events](#events)

## Overview

This section explains the various UI customization options available in the Syncfusion .NET MAUI ComboBox control.

## Placeholder Customization

### Placeholder Text

You can prompt the user with any information using the `Placeholder` property. This text is displayed only when no items are selected or the edit text is empty. The default value is `string.Empty`.

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    ItemsSource="{Binding SocialMedias}"
                    DisplayMemberPath="Name"
                    TextMemberPath="Name"
                    Placeholder="Select a social media" />
```

**C#:**
```csharp
SfComboBox comboBox = new SfComboBox
{
    ItemsSource = socialMediaViewModel.SocialMedias,
    TextMemberPath = "Name",
    DisplayMemberPath = "Name",
    Placeholder = "Select a social media"
};
```

### Placeholder Color

The placeholder text color can be changed using the `PlaceholderColor` property. The default value is `Colors.Gray`.

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    ItemsSource="{Binding SocialMedias}"
                    DisplayMemberPath="Name"
                    TextMemberPath="Name"
                    PlaceholderColor="Red"
                    Placeholder="Select a social media" />
```

**C#:**
```csharp
SfComboBox comboBox = new SfComboBox
{
    ItemsSource = socialMediaViewModel.SocialMedias,
    TextMemberPath = "Name",
    DisplayMemberPath = "Name",
    PlaceholderColor = Colors.Red,
    Placeholder = "Select a social media"
};
```

## Button Customization

### Clear Button

#### Clear Button Icon Color

The clear button icon color can be changed using the `ClearButtonIconColor` property. The default value is `Colors.Black`.

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    ItemsSource="{Binding SocialMedias}"
                    DisplayMemberPath="Name"
                    TextMemberPath="Name"
                    ClearButtonIconColor="Red" />
```

**C#:**
```csharp
SfComboBox comboBox = new SfComboBox
{
    ItemsSource = socialMediaViewModel.SocialMedias,
    TextMemberPath = "Name",
    DisplayMemberPath = "Name",
    ClearButtonIconColor = Colors.Red
};
```

#### Custom Clear Button Path

You can customize the appearance of the clear button using the `ClearButtonPath` property.

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    ItemsSource="{Binding SocialMedias}"
                    TextMemberPath="Name"
                    DisplayMemberPath="Name">
    <editors:SfComboBox.ClearButtonPath>
        <Path Data="M1.70711 0.292893C1.31658 -0.097631 0.683417 -0.097631 0.292893 0.292893C-0.097631 0.683417 -0.097631 1.31658 0.292893 1.70711L5.58579 7L0.292893 12.2929C-0.097631 12.6834 -0.097631 13.3166 0.292893 13.7071C0.683417 14.0976 1.31658 14.0976 1.70711 13.7071L7 8.41421L12.2929 13.7071C12.6834 14.0976 13.3166 14.0976 13.7071 13.7071C14.0976 13.3166 14.0976 12.6834 13.7071 12.2929L8.41421 7L13.7071 1.70711C14.0976 1.31658 14.0976 0.683417 13.7071 0.292893C13.3166 -0.097631 12.6834 -0.097631 12.2929 0.292893L7 5.58579L1.70711 0.292893Z" 
              Fill="Red" 
              Stroke="Red"/>
    </editors:SfComboBox.ClearButtonPath>
</editors:SfComboBox>
```

**C#:**
```csharp
var pathData = "M1.70711 0.292893C1.31658 -0.097631..."; // Full path data
var converter = new PathGeometryConverter();
var path = new Path 
{ 
    Data = (PathGeometry)converter.ConvertFromInvariantString(pathData),
    Fill = Colors.Red,
    Stroke = Colors.Red
};

comboBox.ClearButtonPath = path;
```

### Dropdown Button

#### Dropdown Icon Color

The dropdown icon color can be changed using the `DropDownIconColor` property. The default value is `Colors.Black`.

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    ItemsSource="{Binding SocialMedias}"
                    DisplayMemberPath="Name"
                    TextMemberPath="Name"
                    DropDownIconColor="Red" />
```

**C#:**
```csharp
SfComboBox comboBox = new SfComboBox
{
    ItemsSource = socialMediaViewModel.SocialMedias,
    TextMemberPath = "Name",
    DisplayMemberPath = "Name",
    DropDownIconColor = Colors.Red
};
```

#### Dropdown Button Settings

Customize the dropdown button size using `DropDownButtonSettings`.

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    Placeholder="Enter Social Media"
                    ItemsSource="{Binding SocialMedias}"
                    TextMemberPath="Name"
                    DisplayMemberPath="Name">
    <editors:SfComboBox.DropDownButtonSettings>
        <editors:DropDownButtonSettings Width="50" Height="50" />
    </editors:SfComboBox.DropDownButtonSettings>
</editors:SfComboBox>
```

**C#:**
```csharp
SfComboBox comboBox = new SfComboBox
{
    Placeholder = "Enter Social Media",
    ItemsSource = socialMediaViewModel.SocialMedias,
    TextMemberPath = "Name",
    DisplayMemberPath = "Name",
    DropDownButtonSettings = new DropDownButtonSettings
    {
        Width = 50,
        Height = 40
    }
};
```

#### Custom Dropdown Button View

Set a custom view for the dropdown button.

**XAML:**
```xml
<editors:SfComboBox Placeholder="Enter Social Media"
                    ItemsSource="{Binding SocialMedias}"
                    TextMemberPath="Name"
                    DisplayMemberPath="Name">
    <editors:SfComboBox.DropDownButtonSettings>
        <editors:DropDownButtonSettings Width="80" Height="40">
            <editors:DropDownButtonSettings.View>
                <Grid BackgroundColor="GreenYellow">
                    <Label Text="Click" 
                           FontSize="12" 
                           TextColor="Blue"
                           HorizontalTextAlignment="Center"
                           VerticalOptions="Center" />
                </Grid>
            </editors:DropDownButtonSettings.View>
        </editors:DropDownButtonSettings>
    </editors:SfComboBox.DropDownButtonSettings>
</editors:SfComboBox>
```

## Border and Stroke

### Stroke Color

The ComboBox border color can be changed using the `Stroke` property.

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    ItemsSource="{Binding SocialMedias}"
                    DisplayMemberPath="Name"
                    TextMemberPath="Name"
                    DropDownIconColor="Red"
                    Stroke="Red" />
```

**C#:**
```csharp
SfComboBox comboBox = new SfComboBox
{
    ItemsSource = socialMediaViewModel.SocialMedias,
    TextMemberPath = "Name",
    DisplayMemberPath = "Name",
    Stroke = Colors.Red,
    DropDownIconColor = Colors.Red
};
```

### Border Visibility

The `ShowBorder` property controls the visibility of the border. The default value is `true`.

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    ShowBorder="False"
                    ItemsSource="{Binding SocialMedias}" />
```

**C#:**
```csharp
SfComboBox comboBox = new SfComboBox
{
    ShowBorder = false,
    ItemsSource = socialMediaViewModel.SocialMedias
};
```

## Selection Highlight

The background color of the selected item text can be modified using the `SelectionTextHighlightColor` property.

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    ItemsSource="{Binding SocialMedias}"
                    DisplayMemberPath="Name"
                    TextMemberPath="Name"
                    Placeholder="Enter Media"
                    SelectionTextHighlightColor="Green" />
```

**C#:**
```csharp
SfComboBox comboBox = new SfComboBox
{
    ItemsSource = socialMediaViewModel.SocialMedias,
    DisplayMemberPath = "Name",
    TextMemberPath = "Name",
    Placeholder = "Enter Media",
    SelectionTextHighlightColor = Colors.Green
};
```

## Custom View

The `CustomView` property allows providing a custom view instead of the default entry.

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox">
    <editors:SfComboBox.CustomView>
        <Label x:Name="customLabel"  
               Text="Custom View"  
               TextColor="Red"
               HorizontalOptions="Center"
               VerticalTextAlignment="Center"/>
    </editors:SfComboBox.CustomView>
</editors:SfComboBox>
```

**C#:**
```csharp
SfComboBox comboBox = new SfComboBox
{
    CustomView = new Label
    {
        Text = "Custom View",
        TextColor = Colors.Red,
        HorizontalOptions = LayoutOptions.Center,
        VerticalTextAlignment = TextAlignment.Center
    }
};
```

**Note:** The clear button is not supported when using CustomView.

## Dropdown Height

The maximum height of the dropdown can be changed using the `MaxDropDownHeight` property. The default value is `400d`.

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    IsEditable="true"
                    MaxDropDownHeight="150"
                    ItemsSource="{Binding SocialMedias}"
                    DisplayMemberPath="Name"
                    TextMemberPath="Name" />
```

**C#:**
```csharp
SfComboBox comboBox = new SfComboBox
{
    IsEditable = true,
    MaxDropDownHeight = 150,
    ItemsSource = socialMediaViewModel.SocialMedias,
    TextMemberPath = "Name",
    DisplayMemberPath = "Name"
};
```

**Note:** If `MaxDropDownHeight` is too small, a scroll viewer will automatically appear.

## Item Template

The `ItemTemplate` property allows you to decorate dropdown items using custom templates.

**XAML:**
```xml
<editors:SfComboBox Placeholder="Select an employee"
                    TextMemberPath="Name"
                    DisplayMemberPath="Name"
                    ItemsSource="{Binding Employees}">
    <editors:SfComboBox.ItemTemplate>
        <DataTemplate>
            <ViewCell>
                <Grid Margin="0,5"
                      ColumnDefinitions="48,220"
                      RowDefinitions="50">
                    <Image Grid.Column="0"
                           HorizontalOptions="Center"
                           VerticalOptions="Center"
                           Source="{Binding ProfilePicture}"
                           Aspect="AspectFit"/>
                    <StackLayout Grid.Column="1"
                                 Margin="15,0,0,0">
                        <Label Text="{Binding Name}"
                               FontSize="14"
                               FontAttributes="Bold" />
                        <Label Text="{Binding Designation}"
                               FontSize="12"
                               TextColor="Gray" />
                    </StackLayout>
                </Grid>
            </ViewCell>
        </DataTemplate>
    </editors:SfComboBox.ItemTemplate>
</editors:SfComboBox>
```

### Data Template Selector

For conditional item templates, use DataTemplateSelector.

**C#:**
```csharp
public class EmployeeTemplateSelector : DataTemplateSelector
{
    public DataTemplate EmployeeTemplate1 { get; set; }
    public DataTemplate EmployeeTemplate2 { get; set; }

    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        var employeeName = ((Employee)item).Name;
        if (employeeName == "Anne Dodsworth" || employeeName == "Emilio Alvaro")
            return EmployeeTemplate1;
        else
            return EmployeeTemplate2;
    }
}
```

## Dropdown Styling

### Dropdown Background

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    ItemsSource="{Binding SocialMedias}"
                    DisplayMemberPath="Name"
                    TextMemberPath="Name"
                    DropDownBackground="YellowGreen" />
```

**C#:**
```csharp
comboBox.DropDownBackground = Colors.YellowGreen;
```

### Selected Item Background

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    SelectedDropDownItemBackground="LightSeaGreen" />
```

**C#:**
```csharp
comboBox.SelectedDropDownItemBackground = Colors.LightSeaGreen;
```

### Selected Item Text Style

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox">
    <editors:SfComboBox.SelectedDropDownItemTextStyle>
        <editors:DropDownTextStyle TextColor="Orange" 
                                   FontSize="16" 
                                   FontAttributes="Bold"/>
    </editors:SfComboBox.SelectedDropDownItemTextStyle>
</editors:SfComboBox>
```

**C#:**
```csharp
comboBox.SelectedDropDownItemTextStyle = new DropDownTextStyle
{
    TextColor = Colors.Orange,
    FontSize = 16,
    FontAttributes = FontAttributes.Bold
};
```

### Dropdown Border

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    DropDownStroke="DarkOrange"
                    DropDownStrokeThickness="5" />
```

**C#:**
```csharp
comboBox.DropDownStroke = Colors.DarkOrange;
comboBox.DropDownStrokeThickness = 5;
```

### Dropdown Corner Radius

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    DropDownCornerRadius="25" />
```

**C#:**
```csharp
comboBox.DropDownCornerRadius = 25;
```

### Dropdown Shadow Visibility

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    IsDropDownShadowVisible="False" />
```

**C#:**
```csharp
comboBox.IsDropDownShadowVisible = false;
```

### Dropdown Width

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    DropdownWidth="500" />
```

**C#:**
```csharp
comboBox.DropdownWidth = 500;
```

## Dropdown Item Styling

### Item Text Customization

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    DropDownItemFontAttributes="Italic"
                    DropDownItemFontFamily="OpenSansSemibold"
                    DropDownItemFontSize="16"
                    DropDownItemTextColor="DarkViolet" />
```

**C#:**
```csharp
SfComboBox comboBox = new SfComboBox
{
    DropDownItemFontAttributes = FontAttributes.Italic,
    DropDownItemFontFamily = "OpenSansSemibold",
    DropDownItemTextColor = Colors.DarkViolet,
    DropDownItemFontSize = 16
};
```

### Item Height

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    DropDownItemHeight="25" />
```

**C#:**
```csharp
comboBox.DropDownItemHeight = 25;
```

### Item Padding

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    ItemPadding="10,20,0,0" />
```

**C#:**
```csharp
comboBox.ItemPadding = new Thickness(10, 20, 0, 0);
```

## Dropdown Placement

The dropdown placement can be customized using the `DropDownPlacement` property.

**Available Options:**
- `Top` - Above the text box
- `Bottom` - Below the text box
- `Auto` - Based on available space
- `None` - Dropdown not shown

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    DropDownPlacement="Top" />
```

**C#:**
```csharp
comboBox.DropDownPlacement = DropDownPlacement.Top;
```

### Show Suggestions on Focus

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    ShowSuggestionsOnFocus="True" />
```

**C#:**
```csharp
comboBox.ShowSuggestionsOnFocus = true;
```

## Token Customization

Style token items in multiple selection mode using the `TokenItemStyle` property.

**XAML:**
```xml
<editors:SfComboBox SelectionMode="Multiple" 
                    ItemsSource="{Binding SocialMedias}">
    <editors:SfComboBox.TokenItemStyle>
        <Style TargetType="core:SfChipGroup">
            <Setter Property="ChipTextColor" Value="White"/>
            <Setter Property="ChipFontAttributes" Value="Bold"/>
            <Setter Property="CloseButtonColor" Value="White"/>
            <Setter Property="ChipBackground" Value="#d3a7ff"/>
            <Setter Property="ChipStroke" Value="#5118e3"/>
            <Setter Property="ChipStrokeThickness" Value="6"/>
            <Setter Property="ChipCornerRadius" Value="18"/>
        </Style>
    </editors:SfComboBox.TokenItemStyle>
</editors:SfComboBox>
```

## Text Alignment

Customize text alignment using `HorizontalTextAlignment` and `VerticalTextAlignment` properties.

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    HorizontalTextAlignment="Center" 
                    VerticalTextAlignment="Start" />
```

**C#:**
```csharp
SfComboBox comboBox = new SfComboBox
{
    HorizontalTextAlignment = TextAlignment.Center,
    VerticalTextAlignment = TextAlignment.Start
};
```

**Note:** Dynamic changes to `HorizontalTextAlignment` may not function as expected on Android.

## Additional Settings

### Return Type

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    ReturnType="Next" />
```

**C#:**
```csharp
comboBox.ReturnType = ReturnType.Next;
```

### Cursor Position

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    IsEditable="True"
                    CursorPosition="4" />
```

**C#:**
```csharp
comboBox.CursorPosition = 4;
```

### Automation ID

The ComboBox provides AutomationId support for UI testing frameworks.

If the ComboBox's AutomationId is set to "Employee ComboBox":
- Editable entry: "Employee ComboBox Entry"
- Clear button: "Employee ComboBox Clear Button"
- Dropdown button: "Employee ComboBox Dropdown Button"

## Events

### Completed Event

Raised when the user finalizes text by pressing return key (editable mode only).

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    IsEditable="True"
                    Completed="comboBox_Completed" />
```

**C#:**
```csharp
comboBox.Completed += comboBox_Completed;

private async void comboBox_Completed(object sender, EventArgs e)
{
    await DisplayAlert("Message", "Text entering Completed", "close");
}
```

**Note:** Not supported on Android platform.

### ClearButtonClicked Event

Raised when the clear button is tapped.

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    ClearButtonClicked="comboBox_ClearButtonClicked" />
```

**C#:**
```csharp
comboBox.ClearButtonClicked += comboBox_ClearButtonClicked;

private async void comboBox_ClearButtonClicked(object sender, EventArgs e)
{
    await DisplayAlert("Message", "Clear Button Clicked", "ok");
}
```

## Related Topics

- [Selection](selection.md) - Configure selection and token display
- [Editing Modes](editing-modes.md) - Configure editable behavior
- [Header and Footer](header-footer.md) - Add custom header/footer views
- [Advanced Features](advanced-features.md) - Additional customization options
