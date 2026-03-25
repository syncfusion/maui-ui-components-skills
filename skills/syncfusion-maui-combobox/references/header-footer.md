# Header and Footer in .NET MAUI ComboBox

## Overview

The Syncfusion .NET MAUI ComboBox control allows you to add custom header and footer views at the top and bottom of the dropdown list. This is useful for displaying additional information, action buttons, or custom content above or below the suggestion items.

## Header View

### Enabling Header View

Use the `ShowDropdownHeaderView` property to show or hide the header view in the dropdown. The default value is `false`.

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    ShowDropdownHeaderView="True" />
```

**C#:**
```csharp
comboBox.ShowDropdownHeaderView = true;
```

### Setting Header Content

Use the `DropDownHeaderView` property to provide a custom view as the header.

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    ItemsSource="{Binding SocialMedias}"
                    DisplayMemberPath="Name"
                    TextMemberPath="Name"
                    Placeholder="Enter Media"
                    ShowDropdownHeaderView="True">
    <editors:SfComboBox.DropDownHeaderView>
        <Grid BackgroundColor="LightCyan" 
              HeightRequest="40">
            <Label Text="Social Medias"
                   FontSize="16"
                   FontAttributes="Bold"
                   TextColor="Black"
                   VerticalOptions="Center"
                   HorizontalOptions="Center"/>
        </Grid>
    </editors:SfComboBox.DropDownHeaderView>
</editors:SfComboBox>
```

**C#:**
```csharp
SfComboBox comboBox = new SfComboBox
{
    ItemsSource = socialMediaViewModel.SocialMedias,
    DisplayMemberPath = "Name",
    TextMemberPath = "Name",
    Placeholder = "Enter Media",
    ShowDropdownHeaderView = true,
    DropDownHeaderView = new Grid
    {
        BackgroundColor = Colors.LightCyan,
        HeightRequest = 40,
        Children =
        {
            new Label
            {
                Text = "Social Medias",
                FontSize = 16,
                FontAttributes = FontAttributes.Bold,
                TextColor = Colors.Black,
                VerticalOptions = LayoutOptions.Center,
                HorizontalOptions = LayoutOptions.Center
            }
        }
    }
};
```

### Header Height

You can customize the header height using the `HeightRequest` property on the header view.

**XAML:**
```xml
<editors:SfComboBox.DropDownHeaderView>
    <Grid BackgroundColor="LightCyan" 
          HeightRequest="60">
        <Label Text="Select from the list below"
               VerticalOptions="Center"
               HorizontalOptions="Center"/>
    </Grid>
</editors:SfComboBox.DropDownHeaderView>
```

## Footer View

### Enabling Footer View

Use the `ShowDropdownFooterView` property to show or hide the footer view in the dropdown. The default value is `false`.

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    ShowDropdownFooterView="True" />
```

**C#:**
```csharp
comboBox.ShowDropdownFooterView = true;
```

### Setting Footer Content

Use the `DropDownFooterView` property to provide a custom view as the footer.

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    ItemsSource="{Binding SocialMedias}"
                    DisplayMemberPath="Name"
                    TextMemberPath="Name"
                    Placeholder="Enter Media"
                    ShowDropdownFooterView="True">
    <editors:SfComboBox.DropDownFooterView>
        <Grid BackgroundColor="LightCyan" 
              HeightRequest="40">
            <Label Text="Add New Item"
                   FontSize="14"
                   TextColor="Blue"
                   VerticalOptions="Center"
                   HorizontalOptions="Center"/>
            <Grid.GestureRecognizers>
                <TapGestureRecognizer Tapped="OnFooterTapped"/>
            </Grid.GestureRecognizers>
        </Grid>
    </editors:SfComboBox.DropDownFooterView>
</editors:SfComboBox>
```

**C#:**
```csharp
var footerGrid = new Grid
{
    BackgroundColor = Colors.LightCyan,
    HeightRequest = 40
};

var footerLabel = new Label
{
    Text = "Add New Item",
    FontSize = 14,
    TextColor = Colors.Blue,
    VerticalOptions = LayoutOptions.Center,
    HorizontalOptions = LayoutOptions.Center
};

var tapGesture = new TapGestureRecognizer();
tapGesture.Tapped += OnFooterTapped;
footerGrid.GestureRecognizers.Add(tapGesture);
footerGrid.Children.Add(footerLabel);

SfComboBox comboBox = new SfComboBox
{
    ItemsSource = socialMediaViewModel.SocialMedias,
    DisplayMemberPath = "Name",
    TextMemberPath = "Name",
    Placeholder = "Enter Media",
    ShowDropdownFooterView = true,
    DropDownFooterView = footerGrid
};

void OnFooterTapped(object sender, EventArgs e)
{
    // Handle footer tap - e.g., add new item
    DisplayAlert("Footer", "Add New Item clicked", "OK");
}
```

### Footer Height

You can customize the footer height using the `HeightRequest` property on the footer view.

**XAML:**
```xml
<editors:SfComboBox.DropDownFooterView>
    <Grid BackgroundColor="LightGreen" 
          HeightRequest="50">
        <Button Text="Load More" 
                Clicked="OnLoadMoreClicked"
                HorizontalOptions="Center"
                VerticalOptions="Center"/>
    </Grid>
</editors:SfComboBox.DropDownFooterView>
```

## Combined Header and Footer

You can use both header and footer views simultaneously.

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    ItemsSource="{Binding SocialMedias}"
                    DisplayMemberPath="Name"
                    TextMemberPath="Name"
                    Placeholder="Select Social Media"
                    ShowDropdownHeaderView="True"
                    ShowDropdownFooterView="True">
    
    <!-- Header -->
    <editors:SfComboBox.DropDownHeaderView>
        <Grid BackgroundColor="#E3F2FD" 
              HeightRequest="50">
            <Label Text="Social Media Platforms"
                   FontSize="18"
                   FontAttributes="Bold"
                   TextColor="#1976D2"
                   VerticalOptions="Center"
                   HorizontalOptions="Center"/>
        </Grid>
    </editors:SfComboBox.DropDownHeaderView>
    
    <!-- Footer -->
    <editors:SfComboBox.DropDownFooterView>
        <Grid BackgroundColor="#FFF3E0" 
              HeightRequest="45">
            <HorizontalStackLayout HorizontalOptions="Center"
                                   VerticalOptions="Center"
                                   Spacing="10">
                <Image Source="info_icon.png" 
                       WidthRequest="20" 
                       HeightRequest="20"/>
                <Label Text="Can't find what you're looking for?"
                       FontSize="12"
                       TextColor="#E65100"
                       VerticalOptions="Center"/>
            </HorizontalStackLayout>
        </Grid>
    </editors:SfComboBox.DropDownFooterView>
    
</editors:SfComboBox>
```

## Common Use Cases

### 1. Category Header

**XAML:**
```xml
<editors:SfComboBox.DropDownHeaderView>
    <Grid BackgroundColor="LightBlue" HeightRequest="35">
        <Label Text="Available Options"
               Margin="10,0"
               VerticalOptions="Center"
               FontAttributes="Bold"/>
    </Grid>
</editors:SfComboBox.DropDownHeaderView>
```

### 2. Action Button Footer

**XAML:**
```xml
<editors:SfComboBox.DropDownFooterView>
    <Grid BackgroundColor="WhiteSmoke" HeightRequest="50">
        <Button Text="+ Add New Item"
                Clicked="OnAddNewItemClicked"
                BackgroundColor="Transparent"
                TextColor="Blue"
                HorizontalOptions="Start"
                Margin="10,0"/>
    </Grid>
</editors:SfComboBox.DropDownFooterView>
```

### 3. Information Footer

**XAML:**
```xml
<editors:SfComboBox.DropDownFooterView>
    <Grid BackgroundColor="#FFFACD" HeightRequest="40">
        <Label Text="💡 Tip: Type to search items"
               Margin="10,0"
               VerticalOptions="Center"
               FontSize="12"/>
    </Grid>
</editors:SfComboBox.DropDownFooterView>
```

### 4. Multi-line Header

**XAML:**
```xml
<editors:SfComboBox.DropDownHeaderView>
    <VerticalStackLayout BackgroundColor="AliceBlue" 
                         Padding="10"
                         Spacing="5">
        <Label Text="Select Employee"
               FontSize="16"
               FontAttributes="Bold"
               TextColor="DarkBlue"/>
        <Label Text="Choose from the list of active employees"
               FontSize="12"
               TextColor="Gray"/>
    </VerticalStackLayout>
</editors:SfComboBox.DropDownHeaderView>
```

## Best Practices

1. **Keep Headers Concise**: Headers should provide context without taking up too much space
2. **Make Footers Actionable**: If using buttons in footers, ensure they have clear actions
3. **Consistent Styling**: Match header/footer styling with your overall app theme
4. **Appropriate Heights**: Use `HeightRequest` to ensure headers/footers don't overwhelm the dropdown
5. **Test on Different Screen Sizes**: Ensure header/footer content is visible on small devices

## Notes

- Header and footer views are displayed inside the dropdown list container
- These views scroll with the dropdown content if `MaxDropDownHeight` is exceeded
- You can use any .NET MAUI view or layout as header/footer content
- Header appears above the first item in the list
- Footer appears below the last item in the list
- Both are independent of the load more feature (see Advanced Features documentation)

## Related Topics

- [Advanced Features](advanced-features.md) - Load more functionality with LoadMoreText/Template
- [UI Customization](ui-customization.md) - Dropdown styling and appearance
- [Getting Started](getting-started.md) - Basic ComboBox implementation
