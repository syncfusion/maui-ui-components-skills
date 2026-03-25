# Item Customization in .NET MAUI ListView

## Table of Contents
- [Overview](#overview)
- [Item Templates](#item-templates)
- [Item Sizing](#item-sizing)
- [Item Appearance](#item-appearance)
- [Item Spacing and Borders](#item-spacing-and-borders)
- [Visual Effects](#visual-effects)
- [Advanced Customization Techniques](#advanced-customization-techniques)

## Overview

SfListView provides extensive customization options for item appearance, including:
- Custom templates with DataTemplate and DataTemplateSelector
- Fixed, auto-fit, and dynamic sizing
- Borders, spacing, and visual effects
- Per-item visual states

## Item Templates

### Basic ItemTemplate

Define how each item appears using DataTemplate:

```xml
<syncfusion:SfListView.ItemTemplate>
    <DataTemplate>
        <Grid Padding="15" ColumnSpacing="10">
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="60" />
                <ColumnDefinition Width="*" />
            </Grid.ColumnDefinitions>
            
            <Image Source="{Binding ImageUrl}" 
                   HeightRequest="60" 
                   WidthRequest="60"
                   Aspect="AspectFill" />
            
            <StackLayout Grid.Column="1" VerticalOptions="Center">
                <Label Text="{Binding Title}" 
                       FontSize="16" 
                       FontAttributes="Bold" />
                <Label Text="{Binding Subtitle}" 
                       FontSize="12" 
                       TextColor="Gray" />
            </StackLayout>
        </Grid>
    </DataTemplate>
</syncfusion:SfListView.ItemTemplate>
```

### DataTemplateSelector

Choose different templates based on item data:

```csharp
public class MessageTemplateSelector : DataTemplateSelector
{
    public DataTemplate IncomingMessageTemplate { get; set; }
    public DataTemplate OutgoingMessageTemplate { get; set; }
    
    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        var message = item as MessageInfo;
        if (message == null)
            return IncomingMessageTemplate;
        
        return message.IsOutgoing 
            ? OutgoingMessageTemplate 
            : IncomingMessageTemplate;
    }
}
```

```xml
<ContentPage.Resources>
    <DataTemplate x:Key="IncomingTemplate">
        <Grid Padding="10,5" HorizontalOptions="Start">
            <Frame BackgroundColor="White" Padding="10" CornerRadius="10">
                <Label Text="{Binding Text}" TextColor="Black" />
            </Frame>
        </Grid>
    </DataTemplate>
    
    <DataTemplate x:Key="OutgoingTemplate">
        <Grid Padding="10,5" HorizontalOptions="End">
            <Frame BackgroundColor="#DCF8C6" Padding="10" CornerRadius="10">
                <Label Text="{Binding Text}" TextColor="Black" />
            </Frame>
        </Grid>
    </DataTemplate>
    
    <local:MessageTemplateSelector x:Key="MessageSelector"
        IncomingMessageTemplate="{StaticResource IncomingTemplate}"
        OutgoingMessageTemplate="{StaticResource OutgoingTemplate}" />
</ContentPage.Resources>

<syncfusion:SfListView ItemTemplate="{StaticResource MessageSelector}" />
```

### Complex Template Example

```xml
<syncfusion:SfListView.ItemTemplate>
    <DataTemplate>
        <Frame Padding="0" Margin="10,5" CornerRadius="12" HasShadow="True">
            <Grid>
                <Grid.RowDefinitions>
                    <RowDefinition Height="150" />
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="Auto" />
                </Grid.RowDefinitions>
                
                <!-- Product Image -->
                <Image Grid.Row="0" 
                       Source="{Binding ImageUrl}" 
                       Aspect="AspectFill" />
                
                <!-- Gradient overlay on image -->
                <BoxView Grid.Row="0" 
                         VerticalOptions="End" 
                         HeightRequest="60">
                    <BoxView.Background>
                        <LinearGradientBrush StartPoint="0,0" EndPoint="0,1">
                            <GradientStop Color="Transparent" Offset="0.0" />
                            <GradientStop Color="#80000000" Offset="1.0" />
                        </LinearGradientBrush>
                    </BoxView.Background>
                </BoxView>
                
                <!-- Product Name on Image -->
                <Label Grid.Row="0" 
                       Text="{Binding Name}"
                       TextColor="White"
                       FontSize="18"
                       FontAttributes="Bold"
                       Padding="15"
                       VerticalOptions="End" />
                
                <!-- Product Details -->
                <StackLayout Grid.Row="1" Padding="15,10">
                    <Label Text="{Binding Description}" 
                           FontSize="12" 
                           TextColor="Gray"
                           LineBreakMode="WordWrap" />
                </StackLayout>
                
                <!-- Price and Rating -->
                <Grid Grid.Row="2" Padding="15,0,15,10" ColumnSpacing="10">
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition Width="*" />
                        <ColumnDefinition Width="Auto" />
                    </Grid.ColumnDefinitions>
                    
                    <Label Text="{Binding Price, StringFormat='${0:F2}'}"
                           FontSize="20"
                           FontAttributes="Bold"
                           TextColor="#4CAF50"
                           VerticalOptions="Center" />
                    
                    <HorizontalStackLayout Grid.Column="1" Spacing="5">
                        <Label Text="★" TextColor="Orange" FontSize="16" />
                        <Label Text="{Binding Rating, StringFormat='{0:F1}'}"
                               FontSize="14"
                               VerticalOptions="Center" />
                    </HorizontalStackLayout>
                </Grid>
            </Grid>
        </Frame>
    </DataTemplate>
</syncfusion:SfListView.ItemTemplate>
```

## Item Sizing

### Fixed ItemSize

Set a fixed height (vertical) or width (horizontal) for all items:

```xml
<syncfusion:SfListView ItemSize="100" />
```

```csharp
listView.ItemSize = 80;
```

**Best for performance** - enables optimal view recycling.

### AutoFitMode

Automatically calculate item size based on content:

```xml
<syncfusion:SfListView AutoFitMode="Height" />
```

```csharp
listView.AutoFitMode = AutoFitMode.Height;
```

**AutoFitMode Options:**
- `None` - Use ItemSize (default)
- `Height` - Auto-calculate height based on template content
- `DynamicHeight` - Height adjusts as content changes at runtime

**Trade-off:** More flexible but slower than fixed ItemSize.

### QueryItemSize Event

Calculate size per item:

```csharp
listView.QueryItemSize += (sender, e) =>
{
    var item = e.DataItem as ProductInfo;
    
    if (item != null)
    {
        // Different heights based on item properties
        if (item.HasLongDescription)
            e.ItemSize = 150;
        else
            e.ItemSize = 80;
        
        e.Handled = true;
    }
};
```

## Item Appearance

### Item Borders

```xml
<syncfusion:SfListView x:Name="listView"
                       ShowItemBorder="True"
                       ItemsSource="{Binding BookInfo}"/>
```

### Alternating Row Colors

```csharp
public class AlternatingColorTemplateSelector : DataTemplateSelector
{
    public DataTemplate EvenTemplate { get; set; }
    public DataTemplate OddTemplate { get; set; }
    private int index = 0;
    
    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        return (index++ % 2 == 0) ? EvenTemplate : OddTemplate;
    }
}
```

Or use converter with index:

```xml
<syncfusion:SfListView.ItemTemplate>
    <DataTemplate>
        <Grid Padding="10" 
              BackgroundColor="{Binding Converter={StaticResource IndexToColorConverter}}">
            <Label Text="{Binding Name}" />
        </Grid>
    </DataTemplate>
</syncfusion:SfListView.ItemTemplate>
```

### Visual States

Apply different styles based on item state:

```xml
<syncfusion:SfListView.ItemTemplate>
    <DataTemplate>
        <Grid Padding="15">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup x:Name="CommonStates">
                    <VisualState x:Name="Normal">
                        <VisualState.Setters>
                            <Setter Property="BackgroundColor" Value="White" />
                        </VisualState.Setters>
                    </VisualState>
                    <VisualState x:Name="Selected">
                        <VisualState.Setters>
                            <Setter Property="BackgroundColor" Value="#E3F2FD" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
            
            <Label Text="{Binding Name}" />
        </Grid>
    </DataTemplate>
</syncfusion:SfListView.ItemTemplate>
```

## Item Spacing and Borders

### ItemSpacing Property

Add space between items:

```xml
<syncfusion:SfListView ItemSpacing="10,5,10,5" />
```

Values: Left, Top, Right, Bottom spacing.

### Item Borders with ItemBorder Property

```xml
<syncfusion:SfListView ItemBorderColor="LightGray"
                       ItemBorderThickness="1,0,1,1" />
```

### Custom Spacing with Margin

```xml
<syncfusion:SfListView.ItemTemplate>
    <DataTemplate>
        <Grid Margin="15,10">
            <!-- Your content -->
        </Grid>
    </DataTemplate>
</syncfusion:SfListView.ItemTemplate>
```

## Visual Effects

### Liquid Glass Effect

Create a modern glassmorphism effect (only on above iOS 26 versions):

```xml
<Grid>
      <core:SfGlassEffectView x:Name="glassview" EffectType="Regular">
          <ListView:SfListView x:Name="listView"
                           EnableLiquidGlassEffect="True"
                           SelectionMode="None"
                           DragStartMode="OnHold">

              <ListView:SfListView.ItemTemplate>
                  <DataTemplate>
                      <Grid Padding="8">
                          <Label Text="{Binding .}" />
                      </Grid>
                  </DataTemplate>
              </ListView:SfListView.ItemTemplate>

          </ListView:SfListView>
      </core:SfGlassEffectView>
  </Grid>
```

### Shadow Effects

```xml
<syncfusion:SfListView.ItemTemplate>
    <DataTemplate>
        <Frame Margin="10,5" 
               Padding="15" 
               CornerRadius="12"
               HasShadow="True"
               BackgroundColor="White">
            <Shadow Brush="Black" 
                    Offset="0,4" 
                    Radius="8" 
                    Opacity="0.3" />
            <Label Text="{Binding Name}" />
        </Frame>
    </DataTemplate>
</syncfusion:SfListView.ItemTemplate>
```

### Gradient Backgrounds

```xml
<syncfusion:SfListView.ItemTemplate>
    <DataTemplate>
        <Grid Padding="15" Margin="10,5">
            <Grid.Background>
                <LinearGradientBrush StartPoint="0,0" EndPoint="1,1">
                    <GradientStop Color="#667eea" Offset="0.0" />
                    <GradientStop Color="#764ba2" Offset="1.0" />
                </LinearGradientBrush>
            </Grid.Background>
            
            <Label Text="{Binding Name}" TextColor="White" />
        </Grid>
    </DataTemplate>
</syncfusion:SfListView.ItemTemplate>
```

## Advanced Customization Techniques

### Embedded Controls

Add interactive controls within items:

```xml
<syncfusion:SfListView.ItemTemplate>
    <DataTemplate>
        <Grid Padding="15" ColumnSpacing="10">
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="*" />
                <ColumnDefinition Width="Auto" />
                <ColumnDefinition Width="Auto" />
            </Grid.ColumnDefinitions>
            
            <Label Text="{Binding Name}" VerticalOptions="Center" />
            
            <Stepper Grid.Column="1"
                     Value="{Binding Quantity}"
                     Minimum="0"
                     Maximum="99"
                     Increment="1" />
            
            <Button Grid.Column="2"
                    Text="Remove"
                    Command="{Binding Source={x:Reference listView}, 
                              Path=BindingContext.RemoveCommand}"
                    CommandParameter="{Binding .}" />
        </Grid>
    </DataTemplate>
</syncfusion:SfListView.ItemTemplate>
```

### Expandable Items

Create accordion-style expandable items:

```xml
<syncfusion:SfListView.ItemTemplate>
    <DataTemplate>
        <StackLayout>
            <!-- Header (always visible) -->
            <Grid Padding="15" BackgroundColor="LightGray">
                <Grid.GestureRecognizers>
                    <TapGestureRecognizer Tapped="OnItemHeaderTapped" />
                </Grid.GestureRecognizers>
                <Label Text="{Binding Title}" FontAttributes="Bold" />
            </Grid>
            
            <!-- Expandable content -->
            <StackLayout Padding="15" 
                         IsVisible="{Binding IsExpanded}">
                <Label Text="{Binding Details}" />
            </StackLayout>
        </StackLayout>
    </DataTemplate>
</syncfusion:SfListView.ItemTemplate>
```

```csharp
private void OnItemHeaderTapped(object sender, EventArgs e)
{
    var grid = sender as Grid;
    var item = grid?.BindingContext as ExpandableItem;
    
    if (item != null)
    {
        item.IsExpanded = !item.IsExpanded;
    }
}
```

### Dynamic Content Loading

Load item details on demand:

```csharp
listView.ItemAppearing += async (sender, e) =>
{
    var product = e.DataItem as ProductInfo;
    
    if (product != null && !product.IsDetailsLoaded)
    {
        product.IsDetailsLoaded = true;
        
        // Load additional data
        var details = await LoadProductDetailsAsync(product.Id);
        product.DetailedDescription = details.Description;
        product.Reviews = details.Reviews;
    }
};
```

### Custom Animations

```csharp
listView.ItemAppearing += (sender, e) =>
{
    if (e.DataItem is ProductInfo)
    {
        // Get the visual element
        var element = sender as VisualElement;
        if (element != null)
        {
            element.Opacity = 0;
            element.FadeTo(1, 500, Easing.CubicInOut);
        }
    }
};
```

## Performance Tips

1. **Use Fixed ItemSize** when possible for best performance
2. **Simplify Templates** - avoid deeply nested layouts
3. **Optimize Images** - use appropriate resolution and caching
4. **Avoid Heavy Controls** - minimize use of WebView, complex charts in items
5. **Use Data Virtualization** - load item details on demand
6. **Cache Template Objects** - reuse converters and resources

## Complete Example: Card-Style Product List

```xml
<syncfusion:SfListView x:Name="listView"
                       ItemsSource="{Binding Products}"
                       ItemSize="280"
                       BackgroundColor="#F5F5F5">
    <syncfusion:SfListView.ItemTemplate>
        <DataTemplate>
            <Frame Padding="0" 
                   Margin="15,10" 
                   CornerRadius="16" 
                   HasShadow="True"
                   BackgroundColor="White">
                <Grid>
                    <Grid.RowDefinitions>
                        <RowDefinition Height="180" />
                        <RowDefinition Height="*" />
                    </Grid.RowDefinitions>
                    
                    <!-- Product Image with Favorite Button -->
                    <Grid Grid.Row="0">
                        <Image Source="{Binding ImageUrl}" 
                               Aspect="AspectFill" />
                        
                        <ImageButton Grid.Row="0"
                                     Source="heart.png"
                                     WidthRequest="30"
                                     HeightRequest="30"
                                     HorizontalOptions="End"
                                     VerticalOptions="Start"
                                     Margin="10"
                                     BackgroundColor="#80FFFFFF"
                                     CornerRadius="15"
                                     Command="{Binding ToggleFavoriteCommand}" />
                    </Grid>
                    
                    <!-- Product Info -->
                    <StackLayout Grid.Row="1" Padding="15">
                        <Label Text="{Binding Name}" 
                               FontSize="16" 
                               FontAttributes="Bold" />
                        <Label Text="{Binding Category}" 
                               FontSize="12" 
                               TextColor="Gray"
                               Margin="0,5,0,10" />
                        
                        <Grid ColumnDefinitions="*, Auto">
                            <Label Text="{Binding Price, StringFormat='${0:F2}'}"
                                   FontSize="20"
                                   FontAttributes="Bold"
                                   TextColor="#FF6B6B"
                                   VerticalOptions="Center" />
                            
                            <Button Grid.Column="1"
                                    Text="Add to Cart"
                                    BackgroundColor="#4CAF50"
                                    TextColor="White"
                                    CornerRadius="20"
                                    Padding="20,8"
                                    Command="{Binding AddToCartCommand}" />
                        </Grid>
                    </StackLayout>
                </Grid>
            </Frame>
        </DataTemplate>
    </syncfusion:SfListView.ItemTemplate>
</syncfusion:SfListView>
```
