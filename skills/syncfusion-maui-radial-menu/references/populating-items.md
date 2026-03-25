# Populating Items in MAUI Radial Menu

## Table of Contents
- [Through RadialMenuItem Collection](#through-radialmenuitem-collection)
  - [Text-Only Items](#text-only-items)
  - [Image with Text](#image-with-text)
  - [Custom Font Icons](#custom-font-icons)
  - [Nested Items](#nested-items-hierarchical-menu)
- [Through ItemsSource and ItemTemplate](#through-itemssource-and-itemtemplate)
- [AnimationDuration](#animationduration)
- [IsOpen Property](#isopen-property)
- [Separator Styling](#separator-styling)
- [Rim Customization](#rim-customization)
- [DisplayMemberPath](#displaymemberpath)
- [SelectionColor](#selectioncolor)

This section explains how to populate items in the Radial Menu through direct item collection and data binding approaches.

## Through RadialMenuItem Collection

By passing a collection of `SfRadialMenuItem`, you can view and interact with the `SfRadialMenu` control. The RadialMenuItem class provides various options to customize items with custom views, font icons, and images. You can add items hierarchically for nested menus.

### Text-Only Items

Display simple text items without icons or images.

**XAML:**
```xaml
<syncfusion:SfRadialMenu>
    <syncfusion:SfRadialMenu.Items>
        <syncfusion:SfRadialMenuItem Text="Bold" FontSize="12"/>
        <syncfusion:SfRadialMenuItem Text="Copy" FontSize="12"/>
        <syncfusion:SfRadialMenuItem Text="Undo" FontSize="12"/>
        <syncfusion:SfRadialMenuItem Text="Paste" FontSize="12"/>
        <syncfusion:SfRadialMenuItem Text="Color" FontSize="12"/>
    </syncfusion:SfRadialMenu.Items>
</syncfusion:SfRadialMenu>
```

**C#:**
```csharp
SfRadialMenu radialMenu = new SfRadialMenu();

RadialMenuItemsCollection itemCollection = new RadialMenuItemsCollection
{
    new SfRadialMenuItem { Text = "Bold", FontSize = 12 },
    new SfRadialMenuItem { Text = "Copy", FontSize = 12 },
    new SfRadialMenuItem { Text = "Paste", FontSize = 12 },
    new SfRadialMenuItem { Text = "Undo", FontSize = 12 },
    new SfRadialMenuItem { Text = "Color", FontSize = 12 }
};

radialMenu.Items = itemCollection;
this.Content = radialMenu;
```

**Important:** Always use `RadialMenuItemsCollection` instead of `ObservableCollection` for the items list, and `SubMenuItemsCollection` for nested items within each RadialMenuItem.

### Image with Text

Display images alongside text for visual menu items.

**XAML:**
```xaml
<syncfusion:SfRadialMenu>
    <syncfusion:SfRadialMenu.Items>
        <syncfusion:SfRadialMenuItem Text="John" Image="johnson.png"/>
        <syncfusion:SfRadialMenuItem Text="Krish" Image="krish.png"/>
        <syncfusion:SfRadialMenuItem Text="Ram" Image="ram.png"/>
        <syncfusion:SfRadialMenuItem Text="Kather" Image="kather.png"/>
        <syncfusion:SfRadialMenuItem Text="Joe" Image="joe.png"/>
    </syncfusion:SfRadialMenu.Items>
</syncfusion:SfRadialMenu>
```

**C#:**
```csharp
SfRadialMenu radialMenu = new SfRadialMenu();

RadialMenuItemsCollection itemCollection = new RadialMenuItemsCollection
{
    new SfRadialMenuItem { Text = "John", Image = "johnson.png" },
    new SfRadialMenuItem { Text = "Krish", Image = "krish.png" },
    new SfRadialMenuItem { Text = "Ram", Image = "ram.png" },
    new SfRadialMenuItem { Text = "Kather", Image = "kather.png" },
    new SfRadialMenuItem { Text = "Joe", Image = "joe.png" }
};

radialMenu.Items = itemCollection;
this.Content = radialMenu;
```

**Image Guidelines:**
- Place images in the `Resources/Images` folder
- Supported formats: PNG, JPG, SVG
- Recommended size: 32x32 to 64x64 pixels
- Use transparent backgrounds for better visual integration

### Custom Font Icons

Use font icons for scalable, resolution-independent menu items.

**XAML:**
```xaml
<syncfusion:SfRadialMenu x:Name="radialMenu"
                         CenterButtonFontFamily="Maui Material Assets"
                         CenterButtonFontSize="28"
                         CenterButtonText="&#xe710;"
                         CenterButtonBackFontFamily="Maui Material Assets"
                         CenterButtonBackFontSize="28"
                         CenterButtonBackText="&#xe72d;">
    <syncfusion:SfRadialMenu.Items>
        <syncfusion:SfRadialMenuItem FontFamily="Maui Material Assets"
                                     FontSize="20"
                                     Text="&#xe72e;"/>
        <syncfusion:SfRadialMenuItem FontFamily="Maui Material Assets"
                                     FontSize="20"
                                     Text="&#xe14d;"/>
        <syncfusion:SfRadialMenuItem FontFamily="Maui Material Assets"
                                     FontSize="20"
                                     Text="&#xe3c9;"/>
    </syncfusion:SfRadialMenu.Items>
</syncfusion:SfRadialMenu>
```

**C#:**
```csharp
SfRadialMenu radialMenu = new SfRadialMenu
{
    CenterButtonFontFamily = "Maui Material Assets",
    CenterButtonFontSize = 28,
    CenterButtonText = "\ue710",
    CenterButtonBackFontFamily = "Maui Material Assets",
    CenterButtonBackFontSize = 28,
    CenterButtonBackText = "\ue72d"
};

RadialMenuItemsCollection itemCollection = new RadialMenuItemsCollection
{
    new SfRadialMenuItem 
    { 
        FontFamily = "Maui Material Assets", 
        FontSize = 20, 
        Text = "\ue72e" 
    },
    new SfRadialMenuItem 
    { 
        FontFamily = "Maui Material Assets", 
        FontSize = 20, 
        Text = "\ue14d" 
    },
    new SfRadialMenuItem 
    { 
        FontFamily = "Maui Material Assets", 
        FontSize = 20, 
        Text = "\ue3c9" 
    }
};

radialMenu.Items = itemCollection;
this.Content = radialMenu;
```

**Font Icon Advantages:**
- Scale without quality loss
- No image file management
- Smaller app size
- Easy color customization
- Consistent across platforms

### Nested Items (Hierarchical Menu)

Create multi-level menus by adding items within items.

**XAML:**
```xaml
<syncfusion:SfRadialMenu CenterButtonText="Edit" 
                         CenterButtonFontSize="12">
    <syncfusion:SfRadialMenu.Items>
        <syncfusion:SfRadialMenuItem Text="Bold" FontSize="12"/>
        <syncfusion:SfRadialMenuItem Text="Copy" FontSize="12"/>
        <syncfusion:SfRadialMenuItem Text="Color" FontSize="12">
            <!-- Nested items appear when "Color" is tapped -->
            <syncfusion:SfRadialMenuItem.Items>
                <syncfusion:SfRadialMenuItem Text="Font" 
                                             FontSize="12" 
                                             ItemWidth="50"/>
                <syncfusion:SfRadialMenuItem Text="Gradient" 
                                             FontSize="12" 
                                             ItemWidth="50"/>
                <syncfusion:SfRadialMenuItem Text="Highlight" 
                                             FontSize="12" 
                                             ItemWidth="50"/>
            </syncfusion:SfRadialMenuItem.Items>
        </syncfusion:SfRadialMenuItem>
    </syncfusion:SfRadialMenu.Items>
</syncfusion:SfRadialMenu>
```

**C#:**
```csharp
string[] mainItems = { "Bold", "Copy", "Paste", "Undo", "Color" };
string[] colorItems = { "Font", "Gradient", "Highlight" };

SfRadialMenu radialMenu = new SfRadialMenu
{
    CenterButtonText = "Edit",
    CenterButtonFontSize = 12
};

// Add outer rim items
for (int i = 0; i < mainItems.Length; i++)
{
    SfRadialMenuItem mainMenuItem = new SfRadialMenuItem
    {
        Text = mainItems[i],
        FontSize = 12
    };
    radialMenu.Items.Add(mainMenuItem);
}

// Add nested items to the "Color" item (index 4)
for (int i = 0; i < colorItems.Length; i++)
{
    SfRadialMenuItem colorSubMenuItem = new SfRadialMenuItem
    {
        Text = colorItems[i],
        FontSize = 12,
        ItemWidth = 50
    };
    radialMenu.Items[4].Items.Add(colorSubMenuItem);
}

this.Content = radialMenu;
```

**Nested Menu Best Practices:**
- Limit nesting to 2-3 levels for usability
- Use smaller ItemWidth/ItemHeight for inner levels
- Group logically related actions
- Provide clear back navigation (automatic with back button)

## Through ItemsSource and ItemTemplate

For dynamic data binding, use `ItemsSource` with an `ItemTemplate` to define how data objects are displayed.

**Model Class:**
```csharp
public class EmployeeModel
{
    public string EmployeeName { get; set; }
}
```

**ViewModel:**
```csharp
public class EmployeeViewModel
{
    public ObservableCollection<EmployeeModel> EmployeeCollection { get; set; }

    public EmployeeViewModel()
    {
        EmployeeCollection = new ObservableCollection<EmployeeModel>
        {
            new EmployeeModel { EmployeeName = "Alex" },
            new EmployeeModel { EmployeeName = "Lee" },
            new EmployeeModel { EmployeeName = "Ben" },
            new EmployeeModel { EmployeeName = "Carl" },
            new EmployeeModel { EmployeeName = "Yang" }
        };
    }
}
```

**XAML:**
```xaml
<ContentPage.BindingContext>
    <local:EmployeeViewModel/>
</ContentPage.BindingContext>

<syncfusion:SfRadialMenu x:Name="radialMenu"
                         CenterButtonFontSize="28"
                         CenterButtonFontFamily="Maui Material Assets"
                         CenterButtonText="&#xe71b;"
                         ItemsSource="{Binding EmployeeCollection}">
    <syncfusion:SfRadialMenu.ItemTemplate>
        <DataTemplate>
            <StackLayout>
                <Image Source="user.png" 
                       HorizontalOptions="Center" 
                       WidthRequest="15"/>
                <Label Text="{Binding EmployeeName}" 
                       HorizontalTextAlignment="Center" 
                       VerticalTextAlignment="Center"/>
            </StackLayout>
        </DataTemplate>
    </syncfusion:SfRadialMenu.ItemTemplate>
</syncfusion:SfRadialMenu>
```

**ItemTemplate Use Cases:**
- Dynamic user lists
- Product catalogs
- Tool palettes
- Category browsing
- User-specific menus

## AnimationDuration

Control the speed of the menu's open and close animations.

**XAML:**
```xaml
<syncfusion:SfRadialMenu AnimationDuration="800">
    <syncfusion:SfRadialMenu.Items>
        <syncfusion:SfRadialMenuItem Text="Bold" FontSize="12"/>
        <syncfusion:SfRadialMenuItem Text="Copy" FontSize="12"/>
        <syncfusion:SfRadialMenuItem Text="Undo" FontSize="12"/>
    </syncfusion:SfRadialMenu.Items>
</syncfusion:SfRadialMenu>
```

**C#:**
```csharp
SfRadialMenu radialMenu = new SfRadialMenu
{
    AnimationDuration = 800 // milliseconds
};
```

**Guidelines:**
- Default: 300ms
- Fast: 150-250ms (responsive feel)
- Medium: 300-500ms (balanced)
- Slow: 600-1000ms (dramatic effect)
- 0 = Instant (no animation)

## IsOpen Property

Programmatically control or check the menu's open/closed state.

**XAML:**
```xaml
<syncfusion:SfRadialMenu IsOpen="True">
    <syncfusion:SfRadialMenu.Items>
        <syncfusion:SfRadialMenuItem Text="Bold" FontSize="12"/>
    </syncfusion:SfRadialMenu.Items>
</syncfusion:SfRadialMenu>
```

**C#:**
```csharp
SfRadialMenu radialMenu = new SfRadialMenu
{
    IsOpen = true // Keep menu open initially
};

// Toggle programmatically
radialMenu.IsOpen = !radialMenu.IsOpen;

// Check state
if (radialMenu.IsOpen)
{
    // Menu is currently open
}
```

**Common Uses:**
- Testing layout during development
- Opening menu in response to gestures
- Implementing custom open/close triggers
- Synchronizing with other UI elements

## Separator Styling

Customize the lines between menu items.

**XAML:**
```xaml
<syncfusion:SfRadialMenu SeparatorThickness="5" 
                         SeparatorColor="#FF1493">
    <syncfusion:SfRadialMenu.Items>
        <syncfusion:SfRadialMenuItem Text="Cut" FontSize="12"/>
        <syncfusion:SfRadialMenuItem Text="Copy" FontSize="12"/>
        <syncfusion:SfRadialMenuItem Text="Paste" FontSize="12"/>
    </syncfusion:SfRadialMenu.Items>
</syncfusion:SfRadialMenu>
```

**C#:**
```csharp
SfRadialMenu radialMenu = new SfRadialMenu
{
    SeparatorThickness = 5,
    SeparatorColor = Color.FromArgb("#FF1493")
};
```

**Design Tips:**
- Set `SeparatorThickness="0"` for seamless appearance
- Use subtle colors for professional look
- Match separator color to rim or theme
- Thicker separators for clear item boundaries

## Rim Customization

Adjust the outer ring's appearance and size.

**XAML:**
```xaml
<syncfusion:SfRadialMenu RimRadius="200" 
                         RimColor="#FF1493">
    <syncfusion:SfRadialMenu.Items>
        <syncfusion:SfRadialMenuItem Text="Bold" FontSize="12"/>
        <syncfusion:SfRadialMenuItem Text="Copy" FontSize="12"/>
        <syncfusion:SfRadialMenuItem Text="Undo" FontSize="12"/>
    </syncfusion:SfRadialMenu.Items>
</syncfusion:SfRadialMenu>
```

**C#:**
```csharp
SfRadialMenu radialMenu = new SfRadialMenu
{
    RimRadius = 150,
    RimColor = Color.FromArgb("#FF1493")
};
```

**RimRadius Guidelines:**
- Default: 150
- Small (touch): 120-150
- Medium: 150-200
- Large: 200-300
- Consider screen size and item count

**RimColor Tips:**
- Use `Colors.Transparent` for invisible rim
- Match app theme colors
- Contrast with background
- Semi-transparent for modern look

## DisplayMemberPath

Specify which property to display when using simple object collections.

**Model:**
```csharp
public class Employee
{
    public int ID { get; set; }
    public string EmployeeName { get; set; }
}
```

**ViewModel:**
```csharp
public class EmployeeViewModel
{
    public ObservableCollection<Employee> EmployeeCollection { get; set; }

    public EmployeeViewModel()
    {
        EmployeeCollection = new ObservableCollection<Employee>
        {
            new Employee { ID = 1, EmployeeName = "Eric" },
            new Employee { ID = 2, EmployeeName = "James" },
            new Employee { ID = 3, EmployeeName = "Jacob" },
            new Employee { ID = 4, EmployeeName = "Lucas" }
        };
    }
}
```

**XAML:**
```xaml
<ContentPage.BindingContext>
    <local:EmployeeViewModel/>
</ContentPage.BindingContext>

<syncfusion:SfRadialMenu ItemsSource="{Binding EmployeeCollection}"
                         DisplayMemberPath="EmployeeName"/>
```

**Use DisplayMemberPath when:**
- You have simple objects with a single display property
- You don't need custom item templates
- You want quick setup without DataTemplate

**Use ItemTemplate when:**
- You need complex layouts (images + text)
- Multiple properties displayed together
- Custom styling per item

## SelectionColor

Highlight the selected menu item with a custom color.

**XAML:**
```xaml
<syncfusion:SfRadialMenu SelectionColor="#FF1493">
    <syncfusion:SfRadialMenu.Items>
        <syncfusion:SfRadialMenuItem Text="Cut" FontSize="12"/>
        <syncfusion:SfRadialMenuItem Text="Copy" FontSize="12"/>
        <syncfusion:SfRadialMenuItem Text="Paste" FontSize="12"/>
    </syncfusion:SfRadialMenu.Items>
</syncfusion:SfRadialMenu>
```

**C#:**
```csharp
SfRadialMenu radialMenu = new SfRadialMenu
{
    SelectionColor = Color.FromArgb("#FF1493")
};
```

**Selection Color Best Practices:**
- Use theme accent colors
- Ensure sufficient contrast
- Test touch feedback visibility
- Coordinate with overall color scheme
