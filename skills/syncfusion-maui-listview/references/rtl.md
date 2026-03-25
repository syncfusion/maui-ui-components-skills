# RTL Support in .NET MAUI ListView

## Overview

SfListView supports Right-to-Left (RTL) layout for languages like Arabic, Hebrew, and Persian.

## Right-to-Left (RTL) Support

### FlowDirection Property

Set RTL layout for the entire ListView:

```xml
<syncfusion:SfListView FlowDirection="RightToLeft" 
                       ItemsSource="{Binding Products}" />
```

```csharp
listView.FlowDirection = FlowDirection.RightToLeft;
```

### Dynamic RTL Based on Language

```csharp
public class LocalizationService
{
    private static readonly string[] RtlLanguages = { "ar", "he", "fa", "ur" };
    
    public void SetCulture(string cultureCode)
    {
        var culture = new CultureInfo(cultureCode);
        CultureInfo.CurrentCulture = culture;
        CultureInfo.CurrentUICulture = culture;
        
        // Set flow direction based on language
        var flowDirection = IsRtlLanguage(cultureCode) 
            ? FlowDirection.RightToLeft 
            : FlowDirection.LeftToRight;
        
        Application.Current.MainPage.FlowDirection = flowDirection;
    }
    
    private bool IsRtlLanguage(string cultureCode)
    {
        return RtlLanguages.Any(lang => cultureCode.StartsWith(lang, 
                                         StringComparison.OrdinalIgnoreCase));
    }
}
```

### RTL ItemTemplate

```xml
<syncfusion:SfListView FlowDirection="{Binding CurrentFlowDirection}">
    <syncfusion:SfListView.ItemTemplate>
        <DataTemplate>
            <Grid Padding="15" ColumnSpacing="10">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="80" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>
                
                <!-- Image automatically moves to right in RTL -->
                <Image Grid.Column="0"
                       Source="{Binding ImageUrl}" 
                       HeightRequest="80" 
                       WidthRequest="80" />
                
                <!-- Text aligns correctly in RTL -->
                <StackLayout Grid.Column="1" VerticalOptions="Center">
                    <Label Text="{Binding Name}" 
                           FontAttributes="Bold" 
                           FontSize="16" />
                    <Label Text="{Binding Description}" 
                           FontSize="12" 
                           TextColor="Gray" />
                </StackLayout>
            </Grid>
        </DataTemplate>
    </syncfusion:SfListView.ItemTemplate>
</syncfusion:SfListView>
```


### RTL Swipe Actions

Swipe templates automatically mirror in RTL:

```xml
<syncfusion:SfListView AllowSwiping="True" 
                       FlowDirection="{Binding CurrentFlowDirection}">
    
    <!-- In RTL, this appears on the right side -->
    <syncfusion:SfListView.StartSwipeTemplate>
        <DataTemplate>
            <Grid BackgroundColor="#4CAF50">
                <Label Text="{x:Static resources:Strings.Archive}"
                       TextColor="White"
                       HorizontalOptions="Center" 
                       VerticalOptions="Center" />
            </Grid>
        </DataTemplate>
    </syncfusion:SfListView.StartSwipeTemplate>
    
    <!-- In RTL, this appears on the left side -->
    <syncfusion:SfListView.EndSwipeTemplate>
        <DataTemplate>
            <Grid BackgroundColor="#F44336">
                <Label Text="{x:Static resources:Strings.Delete}"
                       TextColor="White"
                       HorizontalOptions="Center" 
                       VerticalOptions="Center" />
            </Grid>
        </DataTemplate>
    </syncfusion:SfListView.EndSwipeTemplate>
</syncfusion:SfListView>
```

## Best Practices

1. **Implement INotifyPropertyChanged** - Enable dynamic updates
2. **Test RTL Thoroughly** - Check all interactions (swipe, drag, selection)
3. **Flexible Layouts** - Avoid fixed widths that may break with long translations
4. **Bidirectional Text** - Handle mixed LTR/RTL content properly

## Troubleshooting

**Issue:** Text not updating when language changes
→ Ensure INotifyPropertyChanged is implemented and OnPropertyChanged is called

**Issue:** RTL layout not applying
→ Check FlowDirection is set on parent container, verify language code

**Issue:** Text alignment wrong in RTL
→ Set TextAlignment="Start" (not Left/Right) for automatic alignment

**Issue:** Mixed LTR/RTL content displaying incorrectly
→ Use Unicode directional markers or HorizontalTextAlignment property
