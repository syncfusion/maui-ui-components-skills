# SfRadialMenuItem Customization in MAUI Radial Menu

## Table of Contents
- [Items Property (Nested Items)](#items-property-nested-items)
- [Text Property](#text-property)
- [Image Property](#image-property)
- [Font Customization](#font-customization)
- [Colors](#colors)
- [Size Properties](#size-properties)
- [Custom Views](#custom-views)
- [Font Auto Scaling](#font-auto-scaling)
- [IsEnabled Property](#isenabled-property)
- [Command Binding](#command-binding)

The `SfRadialMenuItem` class provides comprehensive customization options for individual menu items, including custom views, font icons, images, and hierarchical nesting.

## Items Property (Nested Items)

The `Items` property allows you to create hierarchical menus by adding child items to a parent menu item. When a parent item is tapped, its children appear on an inner rim.

**XAML:**
```xaml
<syncfusion:SfRadialMenu CenterButtonText="Edit" 
                         CenterButtonFontSize="12">
    <syncfusion:SfRadialMenu.Items>
        <syncfusion:SfRadialMenuItem Text="Bold" FontSize="12"/>
        
        <!-- Parent item with nested children -->
        <syncfusion:SfRadialMenuItem Text="Color" FontSize="12">
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

SfRadialMenu radialMenu = new SfRadialMenu();

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

// Add nested items to "Color" (index 4)
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

**Nesting Best Practices:**
- **Collection Type:** Use `SubMenuItemsCollection` for nested items
- **Depth Limit:** Keep hierarchy to 2-3 levels maximum
- **Item Size:** Reduce `ItemWidth` and `ItemHeight` for inner levels
- **Visual Hierarchy:** Use different colors or sizes per level
- **Logical Grouping:** Group related actions together

## Text Property

Display text on menu items.

**XAML:**
```xaml
<syncfusion:SfRadialMenu>
    <syncfusion:SfRadialMenu.Items>
        <syncfusion:SfRadialMenuItem Text="Bold" FontSize="12"/>
        <syncfusion:SfRadialMenuItem Text="Italic" FontSize="12"/>
        <syncfusion:SfRadialMenuItem Text="Underline" FontSize="12"/>
    </syncfusion:SfRadialMenu.Items>
</syncfusion:SfRadialMenu>
```

**C#:**
```csharp
SfRadialMenu radialMenu = new SfRadialMenu();

RadialMenuItemsCollection itemCollection = new RadialMenuItemsCollection
{
    new SfRadialMenuItem { Text = "Bold", FontSize = 12 },
    new SfRadialMenuItem { Text = "Italic", FontSize = 12 },
    new SfRadialMenuItem { Text = "Underline", FontSize = 12 }
};

radialMenu.Items = itemCollection;
```

**Text Guidelines:**
- Keep text concise (1-10 characters)
- Use clear, descriptive labels
- Consider localization
- Test on actual device sizes
- Use icons for better recognition

## Image Property

Add images to menu items alongside or instead of text.

**XAML:**
```xaml
<syncfusion:SfRadialMenu>
    <syncfusion:SfRadialMenu.Items>
        <syncfusion:SfRadialMenuItem Text="Profile" Image="user.png" FontSize="12"/>
        <syncfusion:SfRadialMenuItem Text="Settings" Image="settings.png" FontSize="12"/>
        <syncfusion:SfRadialMenuItem Text="Exit" Image="exit.png" FontSize="12"/>
    </syncfusion:SfRadialMenu.Items>
</syncfusion:SfRadialMenu>
```

**C#:**
```csharp
RadialMenuItemsCollection itemCollection = new RadialMenuItemsCollection
{
    new SfRadialMenuItem { Text = "Profile", Image = "user.png", FontSize = 12 },
    new SfRadialMenuItem { Text = "Settings", Image = "settings.png", FontSize = 12 },
    new SfRadialMenuItem { Text = "Exit", Image = "exit.png", FontSize = 12 }
};

radialMenu.Items = itemCollection;
```

**Image Requirements:**
- **Location:** Place in `Resources/Images/` folder
- **Formats:** PNG, JPG, SVG
- **Size:** 24x24 to 64x64 pixels
- **Background:** Transparent for best results
- **Naming:** Use lowercase, no spaces

**Image vs Font Icons:**
- **Images:** Better for complex graphics, photos, branding
- **Font Icons:** Better for simple icons, scalability, app size

## Font Customization

### FontFamily and FontSize

Use custom fonts or icon fonts for menu items.

**XAML with Font Icons:**
```xaml
<syncfusion:SfRadialMenu>
    <syncfusion:SfRadialMenu.Items>
        <syncfusion:SfRadialMenuItem FontFamily="Maui Material Assets"
                                     Text="&#xe145;"
                                     FontSize="20"/>
        <syncfusion:SfRadialMenuItem FontFamily="Maui Material Assets"
                                     Text="&#xe3c9;"
                                     FontSize="20"/>
        <syncfusion:SfRadialMenuItem FontFamily="Maui Material Assets"
                                     Text="&#xe14d;"
                                     FontSize="20"/>
    </syncfusion:SfRadialMenu.Items>
</syncfusion:SfRadialMenu>
```

**C#:**
```csharp
RadialMenuItemsCollection itemCollection = new RadialMenuItemsCollection
{
    new SfRadialMenuItem 
    { 
        FontFamily = "Maui Material Assets",
        Text = "\ue145",
        FontSize = 20
    },
    new SfRadialMenuItem 
    { 
        FontFamily = "Maui Material Assets",
        Text = "\ue3c9",
        FontSize = 20
    },
    new SfRadialMenuItem 
    { 
        FontFamily = "Maui Material Assets",
        Text = "\ue14d",
        FontSize = 20
    }
};

radialMenu.Items = itemCollection;
```

**FontSize Guidelines:**
- Icon fonts: 18-24
- Regular text: 10-14
- Nested items: 10-12 (smaller)
- Scale with item size

### FontAttributes

Apply bold, italic, or other styling to text.

**XAML:**
```xaml
<syncfusion:SfRadialMenu>
    <syncfusion:SfRadialMenu.Items>
        <syncfusion:SfRadialMenuItem Text="Bold" 
                                     FontSize="12" 
                                     FontAttributes="Bold"/>
        <syncfusion:SfRadialMenuItem Text="Italic" 
                                     FontSize="12" 
                                     FontAttributes="Italic"/>
    </syncfusion:SfRadialMenu.Items>
</syncfusion:SfRadialMenu>
```

**C#:**
```csharp
RadialMenuItemsCollection itemCollection = new RadialMenuItemsCollection
{
    new SfRadialMenuItem 
    { 
        Text = "Bold", 
        FontSize = 12, 
        FontAttributes = FontAttributes.Bold 
    },
    new SfRadialMenuItem 
    { 
        Text = "Italic", 
        FontSize = 12, 
        FontAttributes = FontAttributes.Italic 
    }
};
```

## Colors

### BackgroundColor

Set the background color of individual menu items.

**XAML:**
```xaml
<syncfusion:SfRadialMenu>
    <syncfusion:SfRadialMenu.Items>
        <syncfusion:SfRadialMenuItem Text="Cut" 
                                     FontSize="12"
                                     BackgroundColor="#FF5722"/>
        <syncfusion:SfRadialMenuItem Text="Copy" 
                                     FontSize="12"
                                     BackgroundColor="#4CAF50"/>
        <syncfusion:SfRadialMenuItem Text="Paste" 
                                     FontSize="12"
                                     BackgroundColor="#2196F3"/>
    </syncfusion:SfRadialMenu.Items>
</syncfusion:SfRadialMenu>
```

**C#:**
```csharp
RadialMenuItemsCollection itemCollection = new RadialMenuItemsCollection
{
    new SfRadialMenuItem 
    { 
        Text = "Cut", 
        FontSize = 12,
        BackgroundColor = Color.FromArgb("#FF5722")
    },
    new SfRadialMenuItem 
    { 
        Text = "Copy", 
        FontSize = 12,
        BackgroundColor = Color.FromArgb("#4CAF50")
    },
    new SfRadialMenuItem 
    { 
        Text = "Paste", 
        FontSize = 12,
        BackgroundColor = Color.FromArgb("#2196F3")
    }
};
```

### TextColor

Customize the text color of menu items.

**XAML:**
```xaml
<syncfusion:SfRadialMenu>
    <syncfusion:SfRadialMenu.Items>
        <syncfusion:SfRadialMenuItem Text="Delete" 
                                     FontSize="12"
                                     TextColor="Red"
                                     BackgroundColor="White"/>
        <syncfusion:SfRadialMenuItem Text="Save" 
                                     FontSize="12"
                                     TextColor="Green"
                                     BackgroundColor="White"/>
    </syncfusion:SfRadialMenu.Items>
</syncfusion:SfRadialMenu>
```

**C#:**
```csharp
RadialMenuItemsCollection itemCollection = new RadialMenuItemsCollection
{
    new SfRadialMenuItem 
    { 
        Text = "Delete", 
        FontSize = 12,
        TextColor = Colors.Red,
        BackgroundColor = Colors.White
    },
    new SfRadialMenuItem 
    { 
        Text = "Save", 
        FontSize = 12,
        TextColor = Colors.Green,
        BackgroundColor = Colors.White
    }
};
```

**Color Best Practices:**
- Ensure sufficient contrast (WCAG AA: 4.5:1 minimum)
- Use semantic colors (red for delete, green for save)
- Maintain consistency across the app
- Test in light and dark modes
- Consider color blindness

## Size Properties

### ItemWidth and ItemHeight

Control the dimensions of menu items.

**XAML:**
```xaml
<syncfusion:SfRadialMenu>
    <syncfusion:SfRadialMenu.Items>
        <!-- Standard size -->
        <syncfusion:SfRadialMenuItem Text="Cut" FontSize="12"/>
        
        <!-- Custom size for nested items -->
        <syncfusion:SfRadialMenuItem Text="Format" FontSize="12">
            <syncfusion:SfRadialMenuItem.Items>
                <syncfusion:SfRadialMenuItem Text="Bold" 
                                             FontSize="10"
                                             ItemWidth="40"
                                             ItemHeight="40"/>
                <syncfusion:SfRadialMenuItem Text="Italic" 
                                             FontSize="10"
                                             ItemWidth="40"
                                             ItemHeight="40"/>
            </syncfusion:SfRadialMenuItem.Items>
        </syncfusion:SfRadialMenuItem>
    </syncfusion:SfRadialMenu.Items>
</syncfusion:SfRadialMenu>
```

**C#:**
```csharp
SfRadialMenuItem formatItem = new SfRadialMenuItem
{
    Text = "Format",
    FontSize = 12
};

// Add smaller nested items
formatItem.Items = new SubMenuItemsCollection
{
    new SfRadialMenuItem 
    { 
        Text = "Bold", 
        FontSize = 10,
        ItemWidth = 40,
        ItemHeight = 40
    },
    new SfRadialMenuItem 
    { 
        Text = "Italic", 
        FontSize = 10,
        ItemWidth = 40,
        ItemHeight = 40
    }
};

radialMenu.Items.Add(formatItem);
```

**Size Guidelines:**
- Default: Based on RimRadius
- Nested items: 30-50 (smaller than parent)
- Main items: 50-80
- Touch targets: Minimum 44x44 points

## Custom Views

Replace standard text/image with custom MAUI views.

**XAML:**
```xaml
<syncfusion:SfRadialMenu>
    <syncfusion:SfRadialMenu.Items>
        <syncfusion:SfRadialMenuItem>
            <syncfusion:SfRadialMenuItem.View>
                <Grid>
                    <StackLayout>
                        <Image Source="star.png" 
                               WidthRequest="24" 
                               HeightRequest="24"/>
                        <Label Text="Favorite" 
                               FontSize="10"
                               HorizontalTextAlignment="Center"/>
                    </StackLayout>
                </Grid>
            </syncfusion:SfRadialMenuItem.View>
        </syncfusion:SfRadialMenuItem>
    </syncfusion:SfRadialMenu.Items>
</syncfusion:SfRadialMenu>
```

**C#:**
```csharp
// Create custom view
Grid customView = new Grid();
StackLayout layout = new StackLayout();

Image icon = new Image
{
    Source = "star.png",
    WidthRequest = 24,
    HeightRequest = 24
};

Label label = new Label
{
    Text = "Favorite",
    FontSize = 10,
    HorizontalTextAlignment = TextAlignment.Center
};

layout.Children.Add(icon);
layout.Children.Add(label);
customView.Children.Add(layout);

// Create menu item with custom view
SfRadialMenuItem customItem = new SfRadialMenuItem
{
    View = customView
};

radialMenu.Items.Add(customItem);
```

**Custom View Use Cases:**
- Complex layouts (icon + badge)
- Progress indicators
- Custom animations
- Multi-line text
- Composite content

## Font Auto Scaling

Enable automatic font scaling based on OS accessibility settings.

**XAML:**
```xaml
<syncfusion:SfRadialMenu>
    <syncfusion:SfRadialMenu.Items>
        <syncfusion:SfRadialMenuItem Text="Settings" 
                                     FontSize="12"
                                     FontAutoScalingEnabled="True"/>
    </syncfusion:SfRadialMenu.Items>
</syncfusion:SfRadialMenu>
```

**C#:**
```csharp
SfRadialMenuItem item = new SfRadialMenuItem
{
    Text = "Settings",
    FontSize = 12,
    FontAutoScalingEnabled = true
};
```

**Benefits:**
- Improves accessibility
- Respects user preferences
- Required for accessibility compliance
- Recommended: Always enable for text

## IsEnabled Property

Enable or disable individual menu items.

**XAML:**
```xaml
<syncfusion:SfRadialMenu>
    <syncfusion:SfRadialMenu.Items>
        <syncfusion:SfRadialMenuItem Text="Cut" 
                                     FontSize="12"
                                     IsEnabled="True"/>
        <syncfusion:SfRadialMenuItem Text="Copy" 
                                     FontSize="12"
                                     IsEnabled="False"/>
    </syncfusion:SfRadialMenu.Items>
</syncfusion:SfRadialMenu>
```

**C#:**
```csharp
SfRadialMenuItem cutItem = new SfRadialMenuItem
{
    Text = "Cut",
    FontSize = 12,
    IsEnabled = true
};

SfRadialMenuItem copyItem = new SfRadialMenuItem
{
    Text = "Copy",
    FontSize = 12,
    IsEnabled = false // Grayed out, not tappable
};
```

**Use Cases:**
- Context-sensitive actions
- Conditional features
- Permission-based access
- State-dependent operations

## Command Binding

Bind menu items to ICommand for MVVM pattern.

**ViewModel:**
```csharp
public class MenuViewModel : INotifyPropertyChanged
{
    public ICommand CutCommand { get; }
    public ICommand CopyCommand { get; }
    public ICommand PasteCommand { get; }

    public MenuViewModel()
    {
        CutCommand = new Command(OnCut);
        CopyCommand = new Command(OnCopy);
        PasteCommand = new Command(OnPaste, CanPaste);
    }

    private void OnCut()
    {
        // Cut logic
    }

    private void OnCopy()
    {
        // Copy logic
    }

    private void OnPaste()
    {
        // Paste logic
    }

    private bool CanPaste()
    {
        // Return true if paste is available
        return true;
    }
}
```

**XAML:**
```xaml
<ContentPage.BindingContext>
    <local:MenuViewModel/>
</ContentPage.BindingContext>

<syncfusion:SfRadialMenu>
    <syncfusion:SfRadialMenu.Items>
        <syncfusion:SfRadialMenuItem Text="Cut" 
                                     FontSize="12"
                                     Command="{Binding CutCommand}"/>
        <syncfusion:SfRadialMenuItem Text="Copy" 
                                     FontSize="12"
                                     Command="{Binding CopyCommand}"/>
        <syncfusion:SfRadialMenuItem Text="Paste" 
                                     FontSize="12"
                                     Command="{Binding PasteCommand}"/>
    </syncfusion:SfRadialMenu.Items>
</syncfusion:SfRadialMenu>
```

**With CommandParameter:**
```xaml
<syncfusion:SfRadialMenuItem Text="Format" 
                             FontSize="12"
                             Command="{Binding FormatCommand}"
                             CommandParameter="Bold"/>
```

**Command Benefits:**
- Cleaner separation of concerns
- Testable business logic
- Reusable across UI
- Better MVVM compliance
