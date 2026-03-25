# Content Management

## Table of Contents
- [Overview](#overview)
- [ContentView (Main Content Area)](#contentview-main-content-area)
- [Drawer Header](#drawer-header)
- [Drawer Content](#drawer-content)
- [Drawer Footer](#drawer-footer)
- [Background Customization](#background-customization)
- [Using CollectionView as Drawer Content](#using-collectionview-as-drawer-content)
- [Dynamic Content Switching](#dynamic-content-switching)
- [Best Practices](#best-practices)

## Overview

The Navigation Drawer has two main areas:
1. **ContentView** - The always-visible main content area (mandatory)
2. **Drawer Panel** - The sliding panel with three optional sections:
   - Header (top)
   - Content (middle - takes remaining space)
   - Footer (bottom)

## ContentView (Main Content Area)

The ContentView is the **mandatory** main content area that remains visible at all times. All user interaction typically starts here.

### Basic ContentView

```xml
<navigationDrawer:SfNavigationDrawer x:Name="navigationDrawer">
    <navigationDrawer:SfNavigationDrawer.ContentView>
        <Grid BackgroundColor="White">
            <Label Text="Main Content" 
                   HorizontalOptions="Center"
                   VerticalOptions="Center"/>
        </Grid>
    </navigationDrawer:SfNavigationDrawer.ContentView>
</navigationDrawer:SfNavigationDrawer>
```

```csharp
navigationDrawer.ContentView = new Grid
{
    BackgroundColor = Colors.White,
    Children =
    {
        new Label
        {
            Text = "Main Content",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.Center
        }
    }
};
```

**⚠️ Critical:** ContentView must be set before displaying the drawer. Setting it to null will cause an exception.

### ContentView with Navigation Bar

```xml
<navigationDrawer:SfNavigationDrawer.ContentView>
    <Grid RowDefinitions="Auto,*" BackgroundColor="White">
        <!-- Header/Navigation Bar -->
        <HorizontalStackLayout BackgroundColor="#6750A4" 
                               Spacing="10" 
                               Padding="5,0,0,0">
            <ImageButton Source="hamburgericon.png"
                         HeightRequest="50"
                         WidthRequest="50"
                         BackgroundColor="#6750A4"
                         Clicked="OnToggleDrawer"/>
            <Label Text="Home"
                   HeightRequest="50"
                   VerticalTextAlignment="Center"
                   FontSize="16"
                   TextColor="White"/>
        </HorizontalStackLayout>
        
        <!-- Main Content Area -->
        <ScrollView Grid.Row="1">
            <VerticalStackLayout Padding="20">
                <Label Text="Welcome to the app!"
                       FontSize="18"/>
                <!-- More content -->
            </VerticalStackLayout>
        </ScrollView>
    </Grid>
</navigationDrawer:SfNavigationDrawer.ContentView>
```

## Drawer Header

The drawer header appears at the top of the drawer panel. Common uses include user profiles, app branding, or section titles.

### Basic Header

```xml
<navigationDrawer:DrawerSettings.DrawerHeaderView>
    <Grid BackgroundColor="#6750A4">
        <Label Text="Navigation Menu"
               TextColor="White"
               FontSize="20"
               HorizontalOptions="Center"
               VerticalOptions="Center"/>
    </Grid>
</navigationDrawer:DrawerSettings.DrawerHeaderView>
```

### Header with User Profile

```xml
<navigationDrawer:DrawerSettings.DrawerHeaderView>
    <Grid BackgroundColor="#6750A4" 
          RowDefinitions="120,40"
          Padding="10">
        <!-- Profile Image -->
        <Image Source="user.png"
               HeightRequest="110"
               WidthRequest="110"
               Aspect="AspectFill"
               VerticalOptions="Center"
               HorizontalOptions="Center">
            <Image.Clip>
                <EllipseGeometry Center="55,55" 
                                 RadiusX="55" 
                                 RadiusY="55"/>
            </Image.Clip>
        </Image>
        
        <!-- User Name -->
        <Label Grid.Row="1"
               Text="James Pollock"
               FontSize="20"
               TextColor="White"
               HorizontalTextAlignment="Center"/>
    </Grid>
</navigationDrawer:DrawerSettings.DrawerHeaderView>
```

### DrawerHeaderHeight Property

Control the header height with the `DrawerHeaderHeight` property:

```xml
<navigationDrawer:DrawerSettings DrawerHeaderHeight="160">
    <navigationDrawer:DrawerSettings.DrawerHeaderView>
        <!-- Header content -->
    </navigationDrawer:DrawerSettings.DrawerHeaderView>
</navigationDrawer:DrawerSettings>
```

```csharp
drawerSettings.DrawerHeaderHeight = 160;
```

**💡 Tip:** Set `DrawerHeaderHeight` to `0` to hide the header completely.

**Guidelines:**
- **Simple text header:** 50-80 pixels
- **User profile with image:** 140-180 pixels
- **Branding with logo:** 100-120 pixels

## Drawer Content

The main content area of the drawer, displayed between header and footer. This is where navigation items typically appear.

### Simple Text Content

```xml
<navigationDrawer:DrawerSettings.DrawerContentView>
    <Grid BackgroundColor="White">
        <Label Text="Drawer Content"
               VerticalOptions="Center"
               HorizontalOptions="Center"/>
    </Grid>
</navigationDrawer:DrawerSettings.DrawerContentView>
```

### Navigation Menu with VerticalStackLayout

```xml
<navigationDrawer:DrawerSettings.DrawerContentView>
    <ScrollView>
        <VerticalStackLayout Spacing="0">
            <Button Text="Home"
                    BackgroundColor="Transparent"
                    TextColor="Black"
                    HorizontalOptions="FillAndExpand"
                    Clicked="OnHomeClicked"/>
            <Button Text="Profile"
                    BackgroundColor="Transparent"
                    TextColor="Black"
                    HorizontalOptions="FillAndExpand"
                    Clicked="OnProfileClicked"/>
            <Button Text="Settings"
                    BackgroundColor="Transparent"
                    TextColor="Black"
                    HorizontalOptions="FillAndExpand"
                    Clicked="OnSettingsClicked"/>
            <Button Text="Help"
                    BackgroundColor="Transparent"
                    TextColor="Black"
                    HorizontalOptions="FillAndExpand"
                    Clicked="OnHelpClicked"/>
        </VerticalStackLayout>
    </ScrollView>
</navigationDrawer:DrawerSettings.DrawerContentView>
```

### CollectionView with Data Binding

```xml
<navigationDrawer:DrawerSettings.DrawerContentView>
    <CollectionView x:Name="menuCollectionView"
                    SelectionMode="Single"
                    SelectionChanged="OnMenuItemSelected">
        <CollectionView.ItemsSource>
            <x:Array Type="{x:Type x:String}">
                <x:String>Home</x:String>
                <x:String>Profile</x:String>
                <x:String>Inbox</x:String>
                <x:String>Outbox</x:String>
                <x:String>Sent</x:String>
                <x:String>Draft</x:String>
                <x:String>Trash</x:String>
            </x:Array>
        </CollectionView.ItemsSource>
        <CollectionView.ItemTemplate>
            <DataTemplate>
                <Grid Padding="15,10">
                    <Label Text="{Binding}"
                           FontSize="16"
                           TextColor="Black"
                           VerticalOptions="Center"/>
                </Grid>
            </DataTemplate>
        </CollectionView.ItemTemplate>
    </CollectionView>
</navigationDrawer:DrawerSettings.DrawerContentView>
```

**💡 Note:** The drawer content view automatically takes up the space remaining after header and footer heights are allocated.

## Drawer Footer

The footer appears at the bottom of the drawer panel. Common uses include app version info, logout buttons, or secondary actions.

### Basic Footer

```xml
<navigationDrawer:DrawerSettings.DrawerFooterView>
    <Grid BackgroundColor="#6750A4" Padding="10">
        <Label Text="Version 1.0.0"
               TextColor="White"
               FontSize="12"
               HorizontalOptions="Center"
               VerticalOptions="Center"/>
    </Grid>
</navigationDrawer:DrawerSettings.DrawerFooterView>
```

### Footer with Actions

```xml
<navigationDrawer:DrawerSettings.DrawerFooterView>
    <Grid BackgroundColor="#F5F5F5" 
          Padding="10"
          RowDefinitions="Auto,Auto">
        <Button Text="Settings"
                BackgroundColor="Transparent"
                TextColor="#6750A4"
                Clicked="OnSettingsClicked"/>
        <Button Grid.Row="1"
                Text="Logout"
                BackgroundColor="Transparent"
                TextColor="#D32F2F"
                Clicked="OnLogoutClicked"/>
    </Grid>
</navigationDrawer:DrawerSettings.DrawerFooterView>
```

### DrawerFooterHeight Property

Control the footer height with the `DrawerFooterHeight` property:

```xml
<navigationDrawer:DrawerSettings DrawerFooterHeight="100">
    <navigationDrawer:DrawerSettings.DrawerFooterView>
        <!-- Footer content -->
    </navigationDrawer:DrawerSettings.DrawerFooterView>
</navigationDrawer:DrawerSettings>
```

```csharp
drawerSettings.DrawerFooterHeight = 100;
```

**💡 Tip:** Set `DrawerFooterHeight` to `0` to hide the footer completely.

## Background Customization

### ContentBackground Property

Customize the drawer content's background color:

```xml
<navigationDrawer:DrawerSettings ContentBackground="LightGray">
    <navigationDrawer:DrawerSettings.DrawerContentView>
        <!-- Content -->
    </navigationDrawer:DrawerSettings.DrawerContentView>
</navigationDrawer:DrawerSettings>
```

```csharp
drawerSettings.ContentBackground = Colors.LightGray;
```

### Gradient Background

```csharp
var drawer = new SfNavigationDrawer();
drawer.DrawerSettings = new DrawerSettings
{
    ContentBackground = new LinearGradientBrush
    {
        StartPoint = new Point(0, 0),
        EndPoint = new Point(1, 1),
        GradientStops = new GradientStopCollection
        {
            new GradientStop { Color = Colors.LightBlue, Offset = 0.0f },
            new GradientStop { Color = Colors.Blue, Offset = 1.0f }
        }
    }
};
```

## Using CollectionView as Drawer Content

CollectionView is ideal for drawer menus with selectable items, data binding, and templates.

### Complete CollectionView Example

**XAML:**

```xml
<navigationDrawer:SfNavigationDrawer x:Name="navigationDrawer">
    <navigationDrawer:SfNavigationDrawer.DrawerSettings>
        <navigationDrawer:DrawerSettings DrawerWidth="250"
                                         DrawerHeaderHeight="160">
            <!-- Header -->
            <navigationDrawer:DrawerSettings.DrawerHeaderView>
                <Grid BackgroundColor="#6750A4">
                    <Image Source="user.png" 
                           HeightRequest="80"
                           WidthRequest="80"/>
                </Grid>
            </navigationDrawer:DrawerSettings.DrawerHeaderView>
            
            <!-- CollectionView Content -->
            <navigationDrawer:DrawerSettings.DrawerContentView>
                <CollectionView x:Name="menuItems"
                                SelectionMode="Single"
                                SelectionChanged="OnMenuItemSelected"
                                BackgroundColor="White">
                    <CollectionView.ItemTemplate>
                        <DataTemplate>
                            <Grid Padding="20,15" 
                                  ColumnDefinitions="Auto,*">
                                <Image Source="{Binding Icon}"
                                       HeightRequest="24"
                                       WidthRequest="24"/>
                                <Label Grid.Column="1"
                                       Text="{Binding Title}"
                                       FontSize="16"
                                       Margin="15,0,0,0"
                                       VerticalOptions="Center"/>
                            </Grid>
                        </DataTemplate>
                    </CollectionView.ItemTemplate>
                </CollectionView>
            </navigationDrawer:DrawerSettings.DrawerContentView>
        </navigationDrawer:DrawerSettings>
    </navigationDrawer:SfNavigationDrawer.DrawerSettings>
</navigationDrawer:SfNavigationDrawer>
```

**Code-Behind:**

```csharp
public class MenuItem
{
    public string Title { get; set; }
    public string Icon { get; set; }
}

public MainPage()
{
    InitializeComponent();
    
    menuItems.ItemsSource = new List<MenuItem>
    {
        new MenuItem { Title = "Home", Icon = "home.png" },
        new MenuItem { Title = "Profile", Icon = "profile.png" },
        new MenuItem { Title = "Settings", Icon = "settings.png" },
        new MenuItem { Title = "Help", Icon = "help.png" }
    };
}

private void OnMenuItemSelected(object sender, SelectionChangedEventArgs e)
{
    if (e.CurrentSelection.FirstOrDefault() is MenuItem selected)
    {
        // Handle navigation
        NavigateTo(selected.Title);
        
        // Close drawer
        navigationDrawer.ToggleDrawer();
    }
}
```

## Dynamic Content Switching

Update ContentView dynamically based on user selection in the drawer.

### Method 1: Replace Entire ContentView

```csharp
private void OnMenuItemSelected(object sender, SelectionChangedEventArgs e)
{
    if (e.CurrentSelection.FirstOrDefault() is string selectedItem)
    {
        View newContent = selectedItem switch
        {
            "Home" => new HomeView(),
            "Profile" => new ProfileView(),
            "Settings" => new SettingsView(),
            _ => new HomeView()
        };
        
        // Replace content
        navigationDrawer.ContentView = newContent;
        
        // Close drawer
        navigationDrawer.ToggleDrawer();
    }
}
```

### Method 2: Update Content Label/Area

```csharp
// XAML: Define content label
<navigationDrawer:SfNavigationDrawer.ContentView>
    <Grid RowDefinitions="Auto,*">
        <Label x:Name="headerLabel" 
               Text="Home"
               FontSize="20"
               Padding="20"/>
        <Label Grid.Row="1"
               x:Name="contentLabel"
               Text="Home content"
               Padding="20"/>
    </Grid>
</navigationDrawer:SfNavigationDrawer.ContentView>

// Code-behind
private void OnMenuItemSelected(object sender, SelectionChangedEventArgs e)
{
    if (e.CurrentSelection.FirstOrDefault() is string selected)
    {
        headerLabel.Text = selected;
        contentLabel.Text = $"{selected} content loaded";
        navigationDrawer.ToggleDrawer();
    }
}
```

### Method 3: ContentView with Frame for Page Navigation

```xml
<navigationDrawer:SfNavigationDrawer.ContentView>
    <Grid RowDefinitions="Auto,*">
        <HorizontalStackLayout BackgroundColor="#6750A4" Padding="10">
            <ImageButton Source="hamburger.png" Clicked="OnToggleDrawer"/>
            <Label x:Name="pageTitle" Text="Home" TextColor="White"/>
        </HorizontalStackLayout>
        
        <ContentView Grid.Row="1" x:Name="contentFrame"/>
    </Grid>
</navigationDrawer:SfNavigationDrawer.ContentView>
```

```csharp
private void OnMenuItemSelected(object sender, SelectionChangedEventArgs e)
{
    if (e.CurrentSelection.FirstOrDefault() is string selected)
    {
        pageTitle.Text = selected;
        
        contentFrame.Content = selected switch
        {
            "Home" => new HomeContentView(),
            "Profile" => new ProfileContentView(),
            "Settings" => new SettingsContentView(),
            _ => new HomeContentView()
        };
        
        navigationDrawer.ToggleDrawer();
    }
}
```

## Best Practices

### 1. Always Set ContentView

```csharp
// ✓ Correct - ContentView set before assignment
var drawer = new SfNavigationDrawer
{
    ContentView = new Grid()
};
this.Content = drawer;

// ✗ Wrong - ContentView null
var drawer = new SfNavigationDrawer();
this.Content = drawer; // Exception!
```

### 2. Use Appropriate Heights

```csharp
// Define standard heights
public static class DrawerHeights
{
    public const double SimpleHeader = 60;
    public const double ProfileHeader = 160;
    public const double SimpleFooter = 50;
    public const double ActionsFooter = 100;
}

drawerSettings.DrawerHeaderHeight = DrawerHeights.ProfileHeader;
drawerSettings.DrawerFooterHeight = DrawerHeights.SimpleFooter;
```

### 3. Wrap Long Content in ScrollView

```xml
<!-- Good: Scrollable content -->
<navigationDrawer:DrawerSettings.DrawerContentView>
    <ScrollView>
        <VerticalStackLayout>
            <!-- Many items -->
        </VerticalStackLayout>
    </ScrollView>
</navigationDrawer:DrawerSettings.DrawerContentView>
```

### 4. Close Drawer After Selection

```csharp
private void OnItemSelected(object sender, EventArgs e)
{
    // Handle selection
    ProcessSelection();
    
    // Always close drawer after selection
    navigationDrawer.ToggleDrawer();
}
```

### 5. Provide Visual Feedback

```xml
<!-- Highlight selected item -->
<CollectionView.ItemTemplate>
    <DataTemplate>
        <Grid BackgroundColor="{Binding IsSelected, Converter={StaticResource BoolToColorConverter}}">
            <Label Text="{Binding Title}"/>
        </Grid>
    </DataTemplate>
</CollectionView.ItemTemplate>
```

## Common Scenarios

### Scenario 1: Email App Drawer

```xml
<navigationDrawer:DrawerSettings DrawerHeaderHeight="0"
                                 DrawerFooterHeight="60">
    <!-- No header -->
    
    <navigationDrawer:DrawerSettings.DrawerContentView>
        <CollectionView ItemsSource="{Binding Folders}"/>
    </navigationDrawer:DrawerSettings.DrawerContentView>
    
    <navigationDrawer:DrawerSettings.DrawerFooterView>
        <Button Text="Compose" Clicked="OnCompose"/>
    </navigationDrawer:DrawerSettings.DrawerFooterView>
</navigationDrawer:DrawerSettings>
```

### Scenario 2: Settings Panel with Sections

```xml
<navigationDrawer:DrawerSettings.DrawerContentView>
    <ScrollView>
        <VerticalStackLayout Spacing="20" Padding="10">
            <Label Text="ACCOUNT" FontAttributes="Bold"/>
            <Button Text="Profile"/>
            <Button Text="Privacy"/>
            
            <Label Text="PREFERENCES" FontAttributes="Bold"/>
            <Button Text="Notifications"/>
            <Button Text="Appearance"/>
        </VerticalStackLayout>
    </ScrollView>
</navigationDrawer:DrawerSettings.DrawerContentView>
```

## Troubleshooting

### Issue: "ContentView cannot be null"
**Solution:** Set ContentView before assigning drawer to page.

### Issue: Drawer content cut off
**Solution:** Wrap content in ScrollView or reduce header/footer heights.

### Issue: Header/Footer not showing
**Solution:** Ensure DrawerHeaderHeight and DrawerFooterHeight are set to non-zero values.

### Issue: CollectionView not responding to selection
**Solution:** Set SelectionMode="Single" and handle SelectionChanged event.
