# Styling and Customization

## Table of Contents
- [Implicit Styling](#implicit-styling)
- [Set datagrid style from page resources](#set-datagrid-style-from-page-resources)
- [Cell Styling](#cell-styling)
- [Row Styling](#row-styling)
  - [AlternationRowCount](#alternationrowcount)
  - [AllowRowHoverHighlighting](#allowrowhoverhighlighting)
  - [RowHoveredBackground](#rowhoveredbackground)
- [Header Styling](#header-styling)
- [Changing the font style](#changing-the-font-style)
- [Border customization](#border-customization)
- [Changing the border color and width](#changing-the-border-color-and-width)
- [Theme Customization](#theme-customization)
- [Custom Templates](#custom-templates)

## Implicit Styling

### Styling Record Cell

```xml
<ContentPage xmlns:syncfusion="clr-namespace:Syncfusion.Maui.DataGrid;assembly=Syncfusion.Maui.DataGrid">
    <ContentPage.Resources>
        <Style TargetType="syncfusion:DataGridCell">
            <Setter Property="Background" Value="#AFD5FB"/>
            <Setter Property="TextColor" Value="#212121"/>
            <Setter Property="FontAttributes" Value="Italic"/>
            <Setter Property="FontSize" Value="14"/>
            <Setter Property="FontFamily" Value="TimesNewRoman"/>
        </Style>
    </ContentPage.Resources>
</ContentPage>
```

### Styling Record Row

```xml
<ContentPage xmlns:syncfusion="clr-namespace:Syncfusion.Maui.DataGrid;assembly=Syncfusion.Maui.DataGrid">
    <ContentPage.Resources>
        <Style TargetType="syncfusion:DataGridRow">
            <Setter Property="Background" Value="#AFD5FB"/>
        </Style>
    </ContentPage.Resources>
</ContentPage>
```

### Styling Header Cell

```xml
<ContentPage xmlns:syncfusion="clr-namespace:Syncfusion.Maui.DataGrid;assembly=Syncfusion.Maui.DataGrid">
    <ContentPage.Resources>
        <Style TargetType="syncfusion:DataGridHeaderCell">
            <Setter Property="Background" Value="#0074E3"/>
            <Setter Property="TextColor" Value="White"/>
            <Setter Property="FontAttributes" Value="Bold"/>
            <Setter Property="FontSize" Value="14"/>
            <Setter Property="FontFamily" Value="TimesNewRoman"/>
        </Style>
    </ContentPage.Resources>
</ContentPage>
```

## Set datagrid style from page resources

```xml
<ContentPage xmlns:syncfusion="clr-namespace:Syncfusion.Maui.DataGrid;assembly=Syncfusion.Maui.DataGrid">
    <ContentPage.Resources>
        <ResourceDictionary>
            <syncfusion:DataGridStyle x:Key="customStyle" 
                                  RowBackground="LightBlue"
                                  HeaderRowBackground="Blue"
                                  RowTextColor="Black"
                                  HeaderRowTextColor="White"/>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <syncfusion:SfDataGrid DefaultStyle="{StaticResource customStyle}" ItemsSource="{Binding OrderInfoCollection}" />
    </ContentPage.Content>
</ContentPage>
```

## Cell Styling

### Default Style

```xml
<syncfusion:SfDataGrid.DefaultStyle>
       <syncfusion:DataGridStyle x:Key="customStyle" 
                                  RowBackground="LightBlue"
                                  HeaderRowBackground="Blue"
                                  RowTextColor="Black"
                                  HeaderRowTextColor="White"/>
</syncfusion:SfDataGrid.DefaultStyle>
```

### Conditional Cell Styling

```xml
<ContentPage xmlns:syncfusion="clr-namespace:Syncfusion.Maui.DataGrid;assembly=Syncfusion.Maui.DataGrid">
    <ContentPage.Resources>
        <local:ColorConverter x:Key="converter"/>
        <Style TargetType="syncfusion:DataGridCell">
            <Setter Property="Background" Value="{Binding OrderID, Converter={StaticResource converter}}"/>
        </Style>
    </ContentPage.Resources>
</ContentPage>
```

```csharp
public class ColorConverter : IValueConverter
{
    object IValueConverter.Convert(object value, Type targetType, object parameter, CultureInfo info)
    {
        if ((int)value < 1006)
            return Colors.LightBlue;
        else
            return Colors.White;
    }
    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        throw new NotImplementedException();
    }
}
```

## Row Styling

### Alternating Row Colors

```xml
<syncfusion:SfDataGrid.DefaultStyle>
    <syncfusion:DataGridStyle AlternateRowBackground="LightGray"
                              RowBackground="White" />
</syncfusion:SfDataGrid.DefaultStyle>
```

### AlternationRowCount

```xml
<ContentPage xmlns:syncfusion="clr-namespace:Syncfusion.Maui.DataGrid;assembly=Syncfusion.Maui.DataGrid">
    <ContentPage.Content>
        <syncfusion:SfDataGrid x:Name="dataGrid" AlternationRowCount="3" 
                            ItemsSource="{Binding OrderInfoCollection}">
            <syncfusion:SfDataGrid.DefaultStyle>
                <syncfusion:DataGridStyle AlternateRowBackground="#AFD5FB"/>
            </syncfusion:SfDataGrid.DefaultStyle>
        </syncfusion:SfDataGrid>
    </ContentPage.Content>
</ContentPage>
```

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        this.dataGrid.DefaultStyle.AlternateRowBackground = Color.FromArgb("#AFD5FB");
        this.dataGrid.AlternationRowCount = 3;
    }
}
```

### AllowRowHoverHighlighting

```xml
<ContentPage xmlns:syncfusion="clr-namespace:Syncfusion.Maui.DataGrid;assembly=Syncfusion.Maui.DataGrid">
    <ContentPage.Content>
        <syncfusion:SfDataGrid x:Name="dataGrid"
                               ItemsSource="{Binding OrderInfoCollection}"
                               AllowRowHoverHighlighting="True">
        </syncfusion:SfDataGrid>
    </ContentPage.Content>
</ContentPage>
```

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        this.dataGrid.AllowRowHoverHighlighting = true;
    }
}
```

### RowHoveredBackground

```xml
<ContentPage xmlns:syncfusion="clr-namespace:Syncfusion.Maui.DataGrid;assembly=Syncfusion.Maui.DataGrid">
    <ContentPage.Content>
        <syncfusion:SfDataGrid x:Name="dataGrid"
                               ItemsSource="{Binding OrderInfoCollection}"
                               AllowRowHoverHighlighting="True">
            <syncfusion:SfDataGrid.DefaultStyle>
                <syncfusion:DataGridStyle RowHoveredBackground="#AFD5FB"/>
            </syncfusion:SfDataGrid.DefaultStyle>
        </syncfusion:SfDataGrid>
    </ContentPage.Content>
</ContentPage>
```

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        this.dataGrid.AllowRowHoverHighlighting = true;
        this.dataGrid.DefaultStyle.RowHoveredBackground = Color.FromArgb("#AFD5FB");
    }
}
```

### Conditional Row Styling

```xml
<ContentPage xmlns:syncfusion="clr-namespace:Syncfusion.Maui.DataGrid;assembly=Syncfusion.Maui.DataGrid">
    <ContentPage.Resources>
        <local:ColorConverter x:Key="converter"/>
        <Style TargetType="syncfusion:DataGridRow">
            <Setter Property="Background" Value="{Binding Converter={StaticResource converter}}" />
        </Style>
    </ContentPage.Resources>
</ContentPage>
```

```csharp
public class ColorConverter : IValueConverter
{
    object IValueConverter.Convert(object value, Type targetType, object parameter, CultureInfo info)
    {
        var input = (value as OrderInfo).OrderID;
        if (input < 1003)
            return Colors.Bisque;
        else if (input < 1007)
            return Colors.LightBlue;
        else
            return Colors.White;
    }
    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        throw new NotImplementedException();
    }
}

```

## Header Styling

```xml
<syncfusion:SfDataGrid.DefaultStyle>
    <syncfusion:DataGridStyle HeaderRowBackground="#0074E3"
                              HeaderRowTextColor="White"
                              HeaderRowFontSize="16"
                              HeaderRowFontAttributes="Bold" />
</syncfusion:SfDataGrid.DefaultStyle>
```

## Changing the font style

```xml
<ContentPage xmlns:syncfusion="clr-namespace:Syncfusion.Maui.DataGrid;assembly=Syncfusion.Maui.DataGrid">
    <ContentPage.Content>
        <syncfusion:SfDataGrid x:Name="dataGrid" ItemsSource="{Binding OrderInfoCollection}">
            <syncfusion:SfDataGrid.DefaultStyle>
                <syncfusion:DataGridStyle HeaderRowFontAttributes="Bold"
                                        HeaderRowFontFamily="TimesNewRoman"
                                        HeaderRowFontSize="16"
                                        RowFontAttributes="Italic"
                                        RowFontFamily="Adabi"
                                        RowFontSize="14"/>
            </syncfusion:SfDataGrid.DefaultStyle>
        </syncfusion:SfDataGrid>
    </ContentPage.Content>
</ContentPage>
```

## Border customization

Following are the list of options available to customize the grid borders:
- Both
- Horizontal
- Vertical
- None

```xml
<ContentPage xmlns:syncfusion="clr-namespace:Syncfusion.Maui.DataGrid;assembly=Syncfusion.Maui.DataGrid">
    <ContentPage.Content>
        <syncfusion:SfDataGrid x:Name="dataGrid" ItemsSource="{Binding OrderInfoCollection}"
                            GridLinesVisibility="Both"
                            HeaderGridLinesVisibility="Both"/>
    </ContentPage.Content>
</ContentPage>
```

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        this.dataGrid.GridLinesVisibility = Syncfusion.Maui.DataGrid.GridLinesVisibility.Both;
        this.dataGrid.HeaderGridLinesVisibility = Syncfusion.Maui.DataGrid.GridLinesVisibility.Both;
    }
}
```

## Changing the border color and width

```xml
<ContentPage xmlns:syncfusion="clr-namespace:Syncfusion.Maui.DataGrid;assembly=Syncfusion.Maui.DataGrid">
    <ContentPage.Content>
        <syncfusion:SfDataGrid x:Name="dataGrid" ItemsSource="{Binding OrderInfoCollection}">
            <syncfusion:SfDataGrid.DefaultStyle>
                <syncfusion:DataGridStyle HeaderGridLineColor="#219ebc" 
                                        GridLineColor="#219ebc"
                                        HeaderGridLineStrokeThickness="3"
                                        GridLineStrokeThickness="3" />
            </syncfusion:SfDataGrid.DefaultStyle>
        </syncfusion:SfDataGrid>
    </ContentPage.Content>
</ContentPage>
```

```csharp
var defaultsyle = new DataGridStyle()
{
   HeaderGridLineColor = Color.FromArgb("#219ebc"),
   GridLineColor = Color.FromArgb("#219ebc"),
   HeaderGridLineStrokeThickness = 3,
   GridLineStrokeThickness = 3
};
dataGrid.DefaultStyle = defaultsyle;
```

## Theme Customization

```xml
<syncfusion:SfDataGrid.DefaultStyle>
    <syncfusion:DataGridStyle GridLineColor="Gray"
                              GridLineStrokeThickness="1"
                              SelectionBackground="#FFE0B2" />
</syncfusion:SfDataGrid.DefaultStyle>
```

## Custom Templates

Use templates for complex cell content - see [columns.md](columns.md).

## Next Steps

- Read [columns.md](columns.md) for column customization
- Read [advanced-features.md](advanced-features.md) for more features
