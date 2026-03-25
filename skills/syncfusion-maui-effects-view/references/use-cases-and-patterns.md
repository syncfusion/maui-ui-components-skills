# Use Cases and Patterns

Practical examples and best practices for implementing SfEffectsView in common scenarios.

## Button-Like Interactions

### Primary Action Button

Create a Material Design button with ripple effect:

```xaml
<effectsView:SfEffectsView TouchDownEffects="Ripple"
                            RippleBackground="#FFFFFF"
                            RippleAnimationDuration="600"
                            AnimationCompleted="OnSubmitClicked">
    <Border BackgroundColor="#6200EE"
            Padding="32,12"
            StrokeThickness="0">
        <Border.StrokeShape>
            <RoundRectangle CornerRadius="4" />
        </Border.StrokeShape>
        <Label Text="SUBMIT"
               TextColor="White"
               FontSize="14"
               FontAttributes="Bold"
               HorizontalOptions="Center" />
    </Border>
</effectsView:SfEffectsView>
```

### Press-and-Hold Button

Scale down on press, spring back on release:

```xaml
<effectsView:SfEffectsView TouchDownEffects="Scale"
                            TouchUpEffects="None"
                            ScaleFactor="0.92"
                            ScaleAnimationDuration="150"
                            TouchUp="OnButtonReleased">
    <Border BackgroundColor="#FF5722" Padding="24,10">
        <Border.StrokeShape>
            <RoundRectangle CornerRadius="8" />
        </Border.StrokeShape>
        <Label Text="PRESS ME" TextColor="White" FontAttributes="Bold" />
    </Border>
</effectsView:SfEffectsView>
```

### Icon Button

Add subtle feedback to icon buttons:

```xaml
<effectsView:SfEffectsView TouchDownEffects="Scale"
                            ScaleFactor="0.85"
                            ScaleAnimationDuration="100"
                            AnimationCompleted="OnFavoriteClicked">
    <Border BackgroundColor="Transparent"
            Padding="12"
            WidthRequest="48"
            HeightRequest="48">
        <Image Source="favorite_icon.png" 
               WidthRequest="24" 
               HeightRequest="24" />
    </Border>
</effectsView:SfEffectsView>
```

## List Item Selection with Visual Feedback

### Single-Tap Selection

Immediate selection on tap:

```xaml
<CollectionView ItemsSource="{Binding Items}">
    <CollectionView.ItemTemplate>
        <DataTemplate x:DataType="local:ItemViewModel">
            <effectsView:SfEffectsView TouchDownEffects="Ripple"
                                        AnimationCompleted="OnItemTapped"
                                        RippleBackground="#2196F3">
                <Grid Padding="15" BackgroundColor="White">
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition Width="50" />
                        <ColumnDefinition Width="*" />
                    </Grid.ColumnDefinitions>
                    
                    <Image Source="{Binding IconUrl}" 
                           WidthRequest="40" 
                           HeightRequest="40" />
                    
                    <StackLayout Grid.Column="1" VerticalOptions="Center">
                        <Label Text="{Binding Title}" FontSize="16" />
                        <Label Text="{Binding Subtitle}" 
                               FontSize="12" 
                               TextColor="Gray" />
                    </StackLayout>
                </Grid>
            </effectsView:SfEffectsView>
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

```csharp
private async void OnItemTapped(object sender, EventArgs e)
{
    var effectsView = sender as SfEffectsView;
    var item = effectsView.BindingContext as ItemViewModel;
    await Navigation.PushAsync(new ItemDetailsPage(item));
}
```

### Long-Press Multi-Selection

Select multiple items with long press:

```xaml
<CollectionView ItemsSource="{Binding Items}">
    <CollectionView.ItemTemplate>
        <DataTemplate x:DataType="local:ItemViewModel">
            <effectsView:SfEffectsView LongPressEffects="Selection"
                                        IsSelected="{Binding IsSelected, Mode=TwoWay}"
                                        SelectionBackground="#C8E6C9"
                                        SelectionChanged="OnItemSelectionChanged">
                <Grid Padding="15">
                    <Label Text="{Binding Name}" FontSize="16" />
                    <Label Text="✓" 
                           IsVisible="{Binding IsSelected}"
                           HorizontalOptions="End"
                           FontSize="20"
                           TextColor="Green" />
                </Grid>
            </effectsView:SfEffectsView>
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

```csharp
private void OnItemSelectionChanged(object sender, EventArgs e)
{
    var effectsView = sender as SfEffectsView;
    var item = effectsView.BindingContext as ItemViewModel;
    
    UpdateSelectionToolbar();  // Show/hide delete, share buttons
}
```

### Highlight-Only Feedback

Subtle highlight for browsing (no selection):

```xaml
<effectsView:SfEffectsView TouchDownEffects="Highlight"
                            HighlightBackground="#10000000"
                            AnimationCompleted="OnBrowseItem">
    <Grid Padding="15">
        <Image Source="{Binding Thumbnail}" 
               Aspect="AspectFill"
               HeightRequest="120" />
        <Label Text="{Binding Title}" 
               VerticalOptions="End"
               BackgroundColor="#80000000"
               TextColor="White"
               Padding="8" />
    </Grid>
</effectsView:SfEffectsView>
```

## Card Interactions in Dashboards

### Dashboard Card

Interactive card with scale feedback:

```xaml
<effectsView:SfEffectsView TouchDownEffects="Scale"
                            ScaleFactor="0.97"
                            ScaleAnimationDuration="150"
                            AnimationCompleted="OnCardTapped">
    <Border BackgroundColor="White"
            Padding="16"
            StrokeThickness="0">
        <Border.StrokeShape>
            <RoundRectangle CornerRadius="12" />
        </Border.StrokeShape>
        <Border.Shadow>
            <Shadow Brush="Black" Opacity="0.1" Radius="8" Offset="0,4" />
        </Border.Shadow>
        
        <Grid RowDefinitions="Auto,*,Auto" RowSpacing="12">
            <Label Text="Sales" 
                   FontSize="14" 
                   TextColor="Gray" />
            
            <Label Grid.Row="1" 
                   Text="$24,500" 
                   FontSize="32" 
                   FontAttributes="Bold"
                   TextColor="#4CAF50" />
            
            <Grid Grid.Row="2" ColumnDefinitions="*,Auto">
                <Label Text="↑ 12% from last month" 
                       FontSize="12" 
                       TextColor="#4CAF50" />
                <Label Grid.Column="1" 
                       Text="→" 
                       FontSize="16" />
            </Grid>
        </Grid>
    </Border>
</effectsView:SfEffectsView>
```

### Stat Card with Ripple

```xaml
<effectsView:SfEffectsView TouchDownEffects="Ripple"
                            RippleBackground="#2196F3"
                            AnimationCompleted="OnStatCardTapped">
    <Border BackgroundColor="#E3F2FD" Padding="20">
        <Border.StrokeShape>
            <RoundRectangle CornerRadius="8" />
        </Border.StrokeShape>
        
        <StackLayout Spacing="8">
            <Label Text="Active Users" 
                   FontSize="12" 
                   TextColor="#1976D2" />
            <Label Text="1,234" 
                   FontSize="28" 
                   FontAttributes="Bold"
                   TextColor="#1565C0" />
        </StackLayout>
    </Border>
</effectsView:SfEffectsView>
```

## Long-Press Actions and Context Menus

### Context Menu Trigger

```xaml
<effectsView:SfEffectsView TouchDownEffects="Ripple"
                            LongPressed="OnShowContextMenu">
    <Grid Padding="15" BackgroundColor="White">
        <Label Text="Tap to view, long-press for options" />
    </Grid>
</effectsView:SfEffectsView>
```

```csharp
private async void OnShowContextMenu(object sender, EventArgs e)
{
    var effectsView = sender as SfEffectsView;
    var item = effectsView.BindingContext;
    
    string action = await DisplayActionSheet(
        "Item Options",
        "Cancel",
        "Delete",
        "Edit",
        "Share",
        "Duplicate",
        "Move");
    
    await HandleContextAction(action, item);
}
```

### Reorder Mode Activation

```xaml
<CollectionView ItemsSource="{Binding Items}">
    <CollectionView.ItemTemplate>
        <DataTemplate>
            <effectsView:SfEffectsView TouchDownEffects="Highlight"
                                        LongPressed="OnEnterReorderMode">
                <Grid Padding="15">
                    <Image Source="drag_handle.png" 
                           WidthRequest="24"
                           HorizontalOptions="Start"
                           Opacity="0.5" />
                    <Label Text="{Binding Title}" Margin="40,0,0,0" />
                </Grid>
            </effectsView:SfEffectsView>
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

```csharp
private void OnEnterReorderMode(object sender, EventArgs e)
{
    // Enable drag-and-drop reordering
    collectionView.CanReorderItems = true;
    ShowReorderToolbar();
}
```

## Image Galleries with Touch Effects

### Photo Grid Item

```xaml
<CollectionView ItemsSource="{Binding Photos}"
                SelectionMode="None">
    <CollectionView.ItemsLayout>
        <GridItemsLayout Orientation="Vertical" 
                         Span="3" 
                         HorizontalItemSpacing="4" 
                         VerticalItemSpacing="4" />
    </CollectionView.ItemsLayout>
    
    <CollectionView.ItemTemplate>
        <DataTemplate>
            <effectsView:SfEffectsView TouchDownEffects="Scale"
                                        ScaleFactor="0.9"
                                        ScaleAnimationDuration="150"
                                        AnimationCompleted="OnPhotoTapped">
                <Image Source="{Binding ThumbnailUrl}"
                       Aspect="AspectFill"
                       HeightRequest="120"
                       WidthRequest="120" />
            </effectsView:SfEffectsView>
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

### Image with Overlay

```xaml
<effectsView:SfEffectsView TouchDownEffects="Highlight"
                            HighlightBackground="#40000000"
                            AnimationCompleted="OnImageTapped">
    <Grid>
        <Image Source="{Binding FullImageUrl}" 
               Aspect="AspectFill"
               HeightRequest="250" />
        
        <Border BackgroundColor="#80000000"
                VerticalOptions="End"
                Padding="12">
            <Grid ColumnDefinitions="*,Auto">
                <StackLayout>
                    <Label Text="{Binding Title}" 
                           TextColor="White" 
                           FontSize="16"
                           FontAttributes="Bold" />
                    <Label Text="{Binding Date}" 
                           TextColor="White" 
                           FontSize="12"
                           Opacity="0.8" />
                </StackLayout>
                
                <effectsView:SfEffectsView Grid.Column="1"
                                            TouchDownEffects="Scale"
                                            ScaleFactor="0.8"
                                            TouchUp="OnFavoriteClicked">
                    <Image Source="favorite_outline.png" 
                           WidthRequest="24" 
                           HeightRequest="24" />
                </effectsView:SfEffectsView>
            </Grid>
        </Border>
    </Grid>
</effectsView:SfEffectsView>
```

## Custom Control Wrappers

### Custom Button Control

```csharp
public class RippleButton : ContentView
{
    private SfEffectsView _effectsView;
    private Border _border;
    private Label _label;
    
    public static readonly BindableProperty TextProperty =
        BindableProperty.Create(nameof(Text), typeof(string), typeof(RippleButton), 
            propertyChanged: OnTextChanged);
    
    public string Text
    {
        get => (string)GetValue(TextProperty);
        set => SetValue(TextProperty, value);
    }
    
    public event EventHandler Clicked;
    
    public RippleButton()
    {
        _label = new Label
        {
            TextColor = Colors.White,
            FontAttributes = FontAttributes.Bold,
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.Center
        };
        
        _border = new Border
        {
            BackgroundColor = Color.FromArgb("#6200EE"),
            Padding = new Thickness(32, 12),
            StrokeThickness = 0,
            StrokeShape = new RoundRectangle { CornerRadius = 4 },
            Content = _label
        };
        
        _effectsView = new SfEffectsView
        {
            TouchDownEffects = SfEffects.Ripple,
            RippleBackground = new SolidColorBrush(Colors.White),
            Content = _border
        };
        
        _effectsView.AnimationCompleted += (s, e) => Clicked?.Invoke(this, e);
        
        Content = _effectsView;
    }
    
    private static void OnTextChanged(BindableObject bindable, object oldValue, object newValue)
    {
        var button = (RippleButton)bindable;
        button._label.Text = newValue as string;
    }
}
```

**Usage:**

```xaml
<local:RippleButton Text="CUSTOM BUTTON" 
                    Clicked="OnCustomButtonClicked" />
```

### Selectable Card Control

```csharp
public class SelectableCard : ContentView
{
    private SfEffectsView _effectsView;
    
    public static readonly BindableProperty IsSelectedProperty =
        BindableProperty.Create(nameof(IsSelected), typeof(bool), typeof(SelectableCard),
            defaultBindingMode: BindingMode.TwoWay);
    
    public bool IsSelected
    {
        get => (bool)GetValue(IsSelectedProperty);
        set => SetValue(IsSelectedProperty, value);
    }
    
    public event EventHandler SelectionChanged;
    
    public SelectableCard()
    {
        _effectsView = new SfEffectsView
        {
            LongPressEffects = SfEffects.Selection,
            TouchDownEffects = SfEffects.Highlight,
            SelectionBackground = new SolidColorBrush(Color.FromArgb("#E3F2FD")),
            HighlightBackground = new SolidColorBrush(Color.FromArgb("#20000000"))
        };
        
        _effectsView.SetBinding(SfEffectsView.IsSelectedProperty, 
            new Binding(nameof(IsSelected), source: this));
        
        _effectsView.SelectionChanged += (s, e) => 
            SelectionChanged?.Invoke(this, e);
        
        Content = _effectsView;
    }
    
    public void SetContent(View content)
    {
        _effectsView.Content = content;
    }
}
```

## Accessibility Considerations

### Screen Reader Support

Provide meaningful descriptions for accessibility:

```xaml
<effectsView:SfEffectsView TouchDownEffects="Ripple"
                            AnimationCompleted="OnSubmitClicked"
                            AutomationProperties.Name="Submit button"
                            AutomationProperties.HelpText="Tap to submit the form">
    <Border BackgroundColor="#6200EE" Padding="30,15">
        <Label Text="SUBMIT" TextColor="White" />
    </Border>
</effectsView:SfEffectsView>
```

### Semantic Properties

```xaml
<effectsView:SfEffectsView TouchDownEffects="Ripple"
                            SemanticProperties.Description="Profile card"
                            SemanticProperties.Hint="Tap to view full profile">
    <Grid Padding="15">
        <Image Source="avatar.png" />
        <Label Text="John Doe" />
    </Grid>
</effectsView:SfEffectsView>
```

### Reduced Motion Support

Adjust animations for accessibility preferences:

```csharp
public class AccessibleEffectsView : SfEffectsView
{
    public AccessibleEffectsView()
    {
        // Check for reduced motion preference (platform-specific implementation)
        bool prefersReducedMotion = CheckReducedMotion();
        
        if (prefersReducedMotion)
        {
            // Use instant feedback instead of animations
            TouchDownEffects = SfEffects.Highlight;
            RippleAnimationDuration = 0;
            ScaleAnimationDuration = 0;
        }
        else
        {
            // Normal animations
            TouchDownEffects = SfEffects.Ripple;
            RippleAnimationDuration = 600;
        }
    }
    
    private bool CheckReducedMotion()
    {
        // Platform-specific implementation
        // Return user's motion preference
        return false;
    }
}
```

## Performance Optimization Tips

### 1. Avoid Nested EffectsViews

**Don't do this:**

```xaml
<!-- BAD: Nested effects views -->
<effectsView:SfEffectsView TouchDownEffects="Ripple">
    <effectsView:SfEffectsView TouchDownEffects="Scale">
        <Label Text="Bad Pattern" />
    </effectsView:SfEffectsView>
</effectsView:SfEffectsView>
```

**Do this instead:**

```xaml
<!-- GOOD: Single effects view with combined effects -->
<effectsView:SfEffectsView TouchDownEffects="Ripple, Scale">
    <Label Text="Good Pattern" />
</effectsView:SfEffectsView>
```

### 2. Reuse EffectsView Instances

For CollectionViews, leverage data template recycling:

```xaml
<CollectionView ItemsSource="{Binding Items}"
                ItemTemplate="{StaticResource ItemTemplate}"
                RemainingItemsThreshold="10" />
```

### 3. Optimize Animation Durations

Shorter durations = better performance:

```xaml
<!-- Fast, responsive, performant -->
<effectsView:SfEffectsView TouchDownEffects="Ripple"
                            RippleAnimationDuration="400">
    <Label Text="Optimized" />
</effectsView:SfEffectsView>
```

### 4. Use Appropriate Effects

Choose the right effect for the job:

- **Simple buttons:** Ripple or Scale (lightweight)
- **List items:** Highlight (instant, no animation overhead)
- **Complex interactions:** Combine effects sparingly

### 5. Virtualization for Long Lists

Always use CollectionView (virtualized) instead of StackLayout for long lists:

```xaml
<!-- GOOD: Virtualized -->
<CollectionView ItemsSource="{Binding Items}">
    <CollectionView.ItemTemplate>
        <DataTemplate>
            <effectsView:SfEffectsView TouchDownEffects="Highlight">
                <!-- Item content -->
            </effectsView:SfEffectsView>
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

### 6. Lazy Load Heavy Content

Defer loading of images or complex layouts:

```csharp
private async void OnEffectsViewAppearing(object sender, EventArgs e)
{
    var effectsView = sender as SfEffectsView;
    
    // Load content after a delay
    await Task.Delay(100);
    LoadHeavyContent(effectsView);
}
```

## Best Practices Summary

1. **Use appropriate effects** for the interaction type
2. **Keep animations fast** (300-600ms) for responsiveness
3. **Provide visual feedback** but don't overdo it
4. **Consider accessibility** (screen readers, reduced motion)
5. **Test on real devices** to verify performance
6. **Follow platform guidelines** (Material Design on Android, etc.)
7. **Use data binding** for selection state in MVVM
8. **Handle events** for business logic, not just visual feedback
9. **Optimize for lists** using virtualization and appropriate effects
10. **Combine effects thoughtfully** - more isn't always better
