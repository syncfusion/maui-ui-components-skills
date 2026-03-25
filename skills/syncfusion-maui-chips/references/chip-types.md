# Chip Types in .NET MAUI

## Table of Contents
- [Overview](#overview)
- [Input Type](#input-type)
- [Choice Type](#choice-type)
- [Filter Type](#filter-type)
- [Action Type](#action-type)
- [Visual States for Selection](#visual-states-for-selection)
- [Choosing the Right Type](#choosing-the-right-type)

## Overview

The Syncfusion .NET MAUI Chips control supports four distinct types, each designed for specific interaction patterns:

| Type | Selection | Indicators | Use Case |
|------|-----------|-----------|----------|
| **Input** | None | Close button | Dynamic add/remove of items |
| **Choice** | Single | Visual state | Select one option from many |
| **Filter** | Multiple | Check marks | Select multiple filters |
| **Action** | None | None | Execute commands on tap |

Set the chip type using the `ChipType` property:

```xaml
<chip:SfChipGroup ChipType="Choice" ... />
```

```csharp
chipGroup.ChipType = SfChipsType.Choice;
```

## Input Type

The Input type allows users to dynamically add and remove chips at runtime. Each chip displays a close button for removal.

### Key Features

- Displays close button on all chips
- Supports `InputView` for adding new chips
- Fires `ItemRemoved` event when chips are removed
- Ideal for tags, categories, or recipient lists

### Basic Input Chip

```xaml
<chip:SfChipGroup ItemsSource="{Binding Tags}"
                  DisplayMemberPath="Name"
                  ChipType="Input"
                  ChipBackground="White"
                  ChipTextColor="Black"
                  CloseButtonColor="Red">
    <chip:SfChipGroup.ChipLayout>
        <FlexLayout Wrap="Wrap" HorizontalOptions="Start" />
    </chip:SfChipGroup.ChipLayout>
</chip:SfChipGroup>
```

```csharp
SfChipGroup chipGroup = new SfChipGroup
{
    DisplayMemberPath = "Name",
    ChipType = SfChipsType.Input,
    ChipBackground = Colors.White,
    ChipTextColor = Colors.Black,
    CloseButtonColor = Colors.Red
};
chipGroup.SetBinding(SfChipGroup.ItemsSourceProperty, "Tags");

FlexLayout layout = new FlexLayout
{
    Wrap = FlexWrap.Wrap,
    HorizontalOptions = LayoutOptions.Start
};
chipGroup.ChipLayout = layout;
```

### Input Chip with InputView

Add an `Entry` control to allow users to create new chips:

```xaml
<chip:SfChipGroup ItemsSource="{Binding Employees}"
                  DisplayMemberPath="Name"
                  ChipType="Input"
                  ChipPadding="8,8,0,0">
    <chip:SfChipGroup.InputView>
        <Entry x:Name="entry"
               Placeholder="Enter Name"
               VerticalOptions="Center"
               HeightRequest="40"
               FontSize="15"
               WidthRequest="110"
               Completed="Entry_Completed" />
    </chip:SfChipGroup.InputView>
</chip:SfChipGroup>
```

```csharp
// Code-behind
private void Entry_Completed(object sender, EventArgs e)
{
    var viewModel = this.BindingContext as EmployeeViewModel;
    var name = (sender as Entry).Text;
    
    if (!string.IsNullOrWhiteSpace(name))
    {
        viewModel.Employees.Add(new Employee { Name = name });
        entry.Text = string.Empty;
    }
}
```

**C# Setup:**
```csharp
var entry = new Entry
{
    Placeholder = "Enter Name",
    VerticalOptions = LayoutOptions.Center,
    FontSize = 15,
    WidthRequest = 110,
    HeightRequest = 40
};
entry.Completed += Entry_Completed;

SfChipGroup chipGroup = new SfChipGroup
{
    InputView = entry,
    DisplayMemberPath = "Name",
    ChipType = SfChipsType.Input,
    ChipPadding = new Thickness(8, 8, 0, 0)
};
chipGroup.SetBinding(SfChipGroup.ItemsSourceProperty, "Employees");
```

### Complete Input Example with ViewModel

```csharp
// Model
public class Employee
{
    public string Name { get; set; }
}

// ViewModel
public class EmployeeViewModel : INotifyPropertyChanged
{
    private ObservableCollection<Employee> employees;
    
    public ObservableCollection<Employee> Employees
    {
        get { return employees; }
        set 
        { 
            employees = value; 
            OnPropertyChanged(nameof(Employees)); 
        }
    }
    
    public EmployeeViewModel()
    {
        Employees = new ObservableCollection<Employee>
        {
            new Employee { Name = "Joseph" },
            new Employee { Name = "Anne Joseph" },
            new Employee { Name = "Andrew Fuller" },
            new Employee { Name = "Emilio Alvaro" },
            new Employee { Name = "Janet Leverling" }
        };
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

## Choice Type

Choice type allows users to select a single chip from a group. Selecting a chip automatically deselects any previously selected chip.

### ChoiceMode Options

**Single:** At least one item must remain selected. Users cannot deselect the current selection.

**SingleOrNone:** Users can deselect the selected item, leaving all items unselected.

```xaml
<chip:SfChipGroup ChipType="Choice" 
                  ChoiceMode="SingleOrNone" ... />
```

### Basic Choice Chip

```xaml
<chip:SfChipGroup ItemsSource="{Binding Sizes}"
                  DisplayMemberPath="Name"
                  ChipType="Choice"
                  ChoiceMode="Single">
    <chip:SfChipGroup.ChipLayout>
        <FlexLayout HorizontalOptions="Start" VerticalOptions="Center" />
    </chip:SfChipGroup.ChipLayout>
    
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">
                <VisualState.Setters>
                    <Setter Property="ChipTextColor" Value="Black" />
                    <Setter Property="ChipBackground" Value="White" />
                    <Setter Property="ChipStroke" Value="Gray" />
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="Selected">
                <VisualState.Setters>
                    <Setter Property="ChipTextColor" Value="White" />
                    <Setter Property="ChipBackground" Value="#512dcd" />
                    <Setter Property="ChipStroke" Value="#512dcd" />
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</chip:SfChipGroup>
```

### Choice Chip in C#

```csharp
SfChipGroup chipGroup = new SfChipGroup
{
    DisplayMemberPath = "Name",
    ChipType = SfChipsType.Choice,
    ChoiceMode = ChoiceMode.Single
};
chipGroup.SetBinding(SfChipGroup.ItemsSourceProperty, "Sizes");

// Setup visual states
VisualStateGroupList visualStateGroupList = new VisualStateGroupList();
VisualStateGroup commonStateGroup = new VisualStateGroup();

VisualState normalState = new VisualState { Name = "Normal" };
normalState.Setters.Add(new Setter 
{ 
    Property = SfChipGroup.ChipTextColorProperty, 
    Value = Colors.Black 
});
normalState.Setters.Add(new Setter 
{ 
    Property = SfChipGroup.ChipBackgroundProperty, 
    Value = Colors.White 
});
normalState.Setters.Add(new Setter 
{ 
    Property = SfChipGroup.ChipStrokeProperty, 
    Value = Colors.Gray 
});

VisualState selectedState = new VisualState { Name = "Selected" };
selectedState.Setters.Add(new Setter 
{ 
    Property = SfChipGroup.ChipTextColorProperty, 
    Value = Colors.White 
});
selectedState.Setters.Add(new Setter 
{ 
    Property = SfChipGroup.ChipBackgroundProperty, 
    Value = Color.FromHex("#512dcd") 
});
selectedState.Setters.Add(new Setter 
{ 
    Property = SfChipGroup.ChipStrokeProperty, 
    Value = Color.FromHex("#512dcd") 
});

commonStateGroup.States.Add(normalState);
commonStateGroup.States.Add(selectedState);
visualStateGroupList.Add(commonStateGroup);

VisualStateManager.SetVisualStateGroups(chipGroup, visualStateGroupList);
```

### Using Items Collection

```xaml
<chip:SfChipGroup ChipType="Choice" ChoiceMode="SingleOrNone">
    <chip:SfChipGroup.Items>
        <chip:SfChip Text="Extra Small" />
        <chip:SfChip Text="Small" />
        <chip:SfChip Text="Medium" />
        <chip:SfChip Text="Large" />
        <chip:SfChip Text="Extra Large" />
    </chip:SfChipGroup.Items>
    
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">
                <VisualState.Setters>
                    <Setter Property="ChipBackground" Value="LightGray" />
                    <Setter Property="ChipTextColor" Value="Black" />
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="Selected">
                <VisualState.Setters>
                    <Setter Property="ChipBackground" Value="Green" />
                    <Setter Property="ChipTextColor" Value="White" />
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</chip:SfChipGroup>
```

## Filter Type

Filter type allows users to select multiple chips simultaneously. Selected chips display a check mark indicator.

### Key Features

- Multiple selection support
- Selection indicators (check marks)
- Customizable indicator color
- Access selected items via `SelectedItem` property
- Selection events: `SelectionChanging`, `SelectionChanged`

### Basic Filter Chip

```xaml
<chip:SfChipGroup ItemsSource="{Binding Categories}"
                  DisplayMemberPath="Name"
                  ChipType="Filter"
                  SelectionIndicatorColor="White">
    <chip:SfChipGroup.ChipLayout>
        <FlexLayout Wrap="Wrap" HorizontalOptions="Start" />
    </chip:SfChipGroup.ChipLayout>
    
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">
                <VisualState.Setters>
                    <Setter Property="ChipTextColor" Value="Black" />
                    <Setter Property="ChipBackground" Value="White" />
                    <Setter Property="ChipStroke" Value="Gray" />
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="Selected">
                <VisualState.Setters>
                    <Setter Property="ChipTextColor" Value="White" />
                    <Setter Property="ChipBackground" Value="#512dcd" />
                    <Setter Property="ChipStroke" Value="#512dcd" />
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</chip:SfChipGroup>
```

### Filter Chip with Selection Handling

```csharp
SfChipGroup chipGroup = new SfChipGroup
{
    DisplayMemberPath = "Name",
    ChipType = SfChipsType.Filter,
    SelectionIndicatorColor = Colors.White
};
chipGroup.SetBinding(SfChipGroup.ItemsSourceProperty, "Categories");

// Handle selection changes
chipGroup.SelectionChanged += (s, e) =>
{
    if (e.AddedItem != null)
    {
        var selected = e.AddedItem as Category;
        // Apply filter logic
    }
    
    if (e.RemovedItem != null)
    {
        var deselected = e.RemovedItem as Category;
        // Remove filter logic
    }
};

// Setup visual states
VisualStateGroupList visualStateGroupList = new VisualStateGroupList();
VisualStateGroup commonStateGroup = new VisualStateGroup();

VisualState normalState = new VisualState { Name = "Normal" };
normalState.Setters.Add(new Setter 
{ 
    Property = SfChipGroup.ChipTextColorProperty, 
    Value = Colors.Black 
});
normalState.Setters.Add(new Setter 
{ 
    Property = SfChipGroup.ChipBackgroundProperty, 
    Value = Colors.White 
});

VisualState selectedState = new VisualState { Name = "Selected" };
selectedState.Setters.Add(new Setter 
{ 
    Property = SfChipGroup.ChipTextColorProperty, 
    Value = Colors.White 
});
selectedState.Setters.Add(new Setter 
{ 
    Property = SfChipGroup.ChipBackgroundProperty, 
    Value = Color.FromHex("#512dcd") 
});

commonStateGroup.States.Add(normalState);
commonStateGroup.States.Add(selectedState);
visualStateGroupList.Add(commonStateGroup);

VisualStateManager.SetVisualStateGroups(chipGroup, visualStateGroupList);
```

## Action Type

Action type executes a command when a chip is tapped. No selection state or indicators are shown.

### Basic Action Chip

```xaml
<ContentPage.BindingContext>
    <local:ViewModel />
</ContentPage.BindingContext>

<chip:SfChipGroup ItemsSource="{Binding Actions}"
                  DisplayMemberPath="Name"
                  ChipType="Action"
                  Command="{Binding ActionCommand}"
                  ChipBackground="LightBlue"
                  ChipTextColor="Black"
                  CloseButtonColor="Black">
    <chip:SfChipGroup.ChipLayout>
        <FlexLayout HorizontalOptions="Start" VerticalOptions="Center" />
    </chip:SfChipGroup.ChipLayout>
</chip:SfChipGroup>

<!-- Display result -->
<StackLayout Orientation="Horizontal" Margin="10,20,0,0">
    <Label Text="Selected Action:" FontAttributes="Bold" />
    <Label Text="{Binding Result}" FontAttributes="Bold" />
</StackLayout>
```

### Action Chip ViewModel

```csharp
public class ViewModel : INotifyPropertyChanged
{
    private ICommand actionCommand;
    private ObservableCollection<Action> actions;
    private string result;
    
    public ICommand ActionCommand
    {
        get { return actionCommand; }
        set { actionCommand = value; }
    }
    
    public ObservableCollection<Action> Actions
    {
        get { return actions; }
        set 
        { 
            actions = value; 
            OnPropertyChanged(nameof(Actions)); 
        }
    }
    
    public string Result
    {
        get { return result; }
        set 
        { 
            result = value; 
            OnPropertyChanged(nameof(Result)); 
        }
    }
    
    public ViewModel()
    {
        ActionCommand = new Command<object>(HandleAction);
        
        Actions = new ObservableCollection<Action>
        {
            new Action { Name = "Edit" },
            new Action { Name = "Delete" },
            new Action { Name = "Share" },
            new Action { Name = "Download" }
        };
    }
    
    private void HandleAction(object obj)
    {
        var action = obj as Action;
        Result = action.Name;
        
        // Execute action-specific logic
        switch (action.Name)
        {
            case "Edit":
                // Edit logic
                break;
            case "Delete":
                // Delete logic
                break;
            // ... more cases
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}

// Model
public class Action
{
    public string Name { get; set; }
}
```

## Visual States for Selection

Visual states are required for Choice and Filter types to provide visual feedback when chips are selected.

### Complete Visual State Example

```xaml
<chip:SfChipGroup x:Name="sfChipGroup"
                  ItemsSource="{Binding Items}"
                  DisplayMemberPath="Name"
                  ChipType="Choice">
    
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <!-- Normal State (not selected) -->
            <VisualState x:Name="Normal">
                <VisualState.Setters>
                    <Setter Property="ChipTextColor" Value="#1C1B1F" />
                    <Setter Property="ChipBackground" Value="Transparent" />
                    <Setter Property="ChipStroke" Value="#79747E" />
                    <Setter Property="ChipStrokeThickness" Value="1" />
                </VisualState.Setters>
            </VisualState>
            
            <!-- Selected State -->
            <VisualState x:Name="Selected">
                <VisualState.Setters>
                    <Setter Property="ChipTextColor" Value="White" />
                    <Setter Property="ChipBackground" Value="#6750A4" />
                    <Setter Property="ChipStroke" Value="#6750A4" />
                    <Setter Property="ChipStrokeThickness" Value="1" />
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</chip:SfChipGroup>
```

## Choosing the Right Type

| Scenario | Recommended Type | Key Feature |
|----------|-----------------|-------------|
| Email recipients, tags | Input | Dynamic add/remove with InputView |
| Product size selection | Choice | Single selection with visual feedback |
| Category filters | Filter | Multi-select with indicators |
| Action menu items | Action | Command execution on tap |
| Preference toggles | Choice (SingleOrNone) | Optional single selection |
| Search filters | Filter | Multiple active filters |
| Quick actions toolbar | Action | Execute without selection |

## Common Patterns

### Multi-Step Form with Choice

```xaml
<!-- Step 1: Select Size -->
<Label Text="Select Size" FontAttributes="Bold" />
<chip:SfChipGroup ChipType="Choice" 
                  ChoiceMode="Single"
                  SelectedItem="{Binding SelectedSize}">
    <chip:SfChipGroup.Items>
        <chip:SfChip Text="S" />
        <chip:SfChip Text="M" />
        <chip:SfChip Text="L" />
        <chip:SfChip Text="XL" />
    </chip:SfChipGroup.Items>
    <!-- Visual states omitted for brevity -->
</chip:SfChipGroup>
```

### Filter Panel with Clear All

```xaml
<StackLayout>
    <Button Text="Clear All Filters" 
            Command="{Binding ClearFiltersCommand}" />
    
    <chip:SfChipGroup ItemsSource="{Binding Filters}"
                      DisplayMemberPath="Name"
                      ChipType="Filter"
                      SelectionChanged="OnFiltersChanged">
        <!-- Visual states omitted for brevity -->
    </chip:SfChipGroup>
</StackLayout>
```
