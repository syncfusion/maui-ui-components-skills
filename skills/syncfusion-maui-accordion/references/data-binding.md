# Data Binding with .NET MAUI Accordion

This guide demonstrates how to bind data to the Syncfusion .NET MAUI Accordion using BindableLayout, enabling dynamic accordion item generation from collections.

## Table of Contents
- [Overview](#overview)
- [Creating the Data Model](#creating-the-data-model)
- [Creating the ViewModel](#creating-the-viewmodel)
- [Setting the BindingContext](#setting-the-bindingcontext)
- [Binding Data to SfAccordion](#binding-data-to-sfaccordion)
- [Defining the AccordionItem Template](#defining-the-accordionitem-template)
- [Complete Example](#complete-example)
- [Two-Way Binding with IsExpanded](#two-way-binding-with-isexpanded)
- [Best Practices](#best-practices)

## Overview

The SfAccordion control utilizes [.NET MAUI BindableLayout](https://learn.microsoft.com/en-us/dotnet/maui/user-interface/layouts/bindablelayout) to bind data collections. This enables you to:

- Dynamically generate accordion items from ObservableCollection
- Bind item properties to data model properties
- Support two-way binding for expansion state
- Update UI automatically when collection changes

**Key Properties:**
- `BindableLayout.ItemsSource` - The data collection
- `BindableLayout.ItemTemplate` - Defines how each item is rendered
- `AccordionItem.IsExpanded` - Controls expansion state (bindable)

## Creating the Data Model

Create a data model class that implements `INotifyPropertyChanged` for two-way binding support.

**EmployeeInfo.cs:**
```csharp
using System.ComponentModel;
using System.Runtime.CompilerServices;

namespace AccordionApp.Models
{
    public class EmployeeInfo : INotifyPropertyChanged
    {
        private string name;
        private string image;
        private string position;
        private string organizationUnit;
        private string dateOfBirth;
        private string location;
        private string phone;
        private bool isExpanded;
        private string description;

        public string Name
        {
            get => name;
            set
            {
                name = value;
                OnPropertyChanged();
            }
        }

        public string Image
        {
            get => image;
            set
            {
                image = value;
                OnPropertyChanged();
            }
        }

        public string Position
        {
            get => position;
            set
            {
                position = value;
                OnPropertyChanged();
            }
        }

        public string OrganizationUnit
        {
            get => organizationUnit;
            set
            {
                organizationUnit = value;
                OnPropertyChanged();
            }
        }

        public string DateOfBirth
        {
            get => dateOfBirth;
            set
            {
                dateOfBirth = value;
                OnPropertyChanged();
            }
        }

        public string Location
        {
            get => location;
            set
            {
                location = value;
                OnPropertyChanged();
            }
        }

        public string Phone
        {
            get => phone;
            set
            {
                phone = value;
                OnPropertyChanged();
            }
        }

        public bool IsExpanded
        {
            get => isExpanded;
            set
            {
                isExpanded = value;
                OnPropertyChanged();
            }
        }

        public string Description
        {
            get => description;
            set
            {
                description = value;
                OnPropertyChanged();
            }
        }

        public event PropertyChangedEventHandler PropertyChanged;

        protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
        {
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
        }
    }
}
```

**Why INotifyPropertyChanged?**
- Enables two-way binding
- UI updates automatically when properties change
- Supports dynamic data updates

## Creating the ViewModel

Create a ViewModel with an `ObservableCollection` to hold the data.

**EmployeeViewModel.cs:**
```csharp
using System.Collections.ObjectModel;
using AccordionApp.Models;

namespace AccordionApp.ViewModels
{
    public class EmployeeViewModel
    {
        public ObservableCollection<EmployeeInfo> Employees { get; set; }

        public EmployeeViewModel()
        {
            Employees = new ObservableCollection<EmployeeInfo>
            {
                new EmployeeInfo
                {
                    Name = "Robin Rane",
                    Image = "emp_01.png",
                    Position = "Chairman",
                    OrganizationUnit = "ABC Inc.",
                    DateOfBirth = "09/17/1973",
                    Location = "Boston",
                    Phone = "(617) 555-1234",
                    IsExpanded = false,
                    Description = "Robin Rane, Chairman of ABC Inc., leads with dedication and vision. Under his guidance, the company thrives and continues to make a significant impact in the industry."
                },
                new EmployeeInfo
                {
                    Name = "Paul Vent",
                    Image = "emp_02.png",
                    Position = "General Manager",
                    OrganizationUnit = "XYZ Corp.",
                    DateOfBirth = "05/27/1985",
                    Location = "New York",
                    Phone = "(212) 555-1234",
                    IsExpanded = true, // Pre-expanded
                    Description = "Paul Vent, General Manager of XYZ Corp., oversees daily operations, ensuring the company's success and growth through strategic planning and effective management practices."
                },
                new EmployeeInfo
                {
                    Name = "Clara Venus",
                    Image = "emp_03.png",
                    Position = "Assistant Manager",
                    OrganizationUnit = "ABC Inc.",
                    DateOfBirth = "07/22/1988",
                    Location = "California",
                    Phone = "(415) 123-4567",
                    IsExpanded = false,
                    Description = "Clara Venus, Asst. Manager at ABC Inc., efficiently handles multiple tasks. With her strong skill set and dedication, she contributes significantly to the company's growth and success."
                },
                new EmployeeInfo
                {
                    Name = "Maria Even",
                    Image = "emp_04.png",
                    Position = "Executive Manager",
                    OrganizationUnit = "XYZ Corp.",
                    DateOfBirth = "04/16/1970",
                    Location = "New York",
                    Phone = "(516) 345-6789",
                    IsExpanded = false,
                    Description = "Maria Even, a highly experienced professional, holds the position of Executive Manager at XYZ Corp. She oversees crucial operations, enforcing company policies and practices."
                }
            };
        }
    }
}
```

**Why ObservableCollection?**
- Automatically notifies UI when items are added/removed
- Supports dynamic collection changes
- Required for live data binding

## Setting the BindingContext

Set the ViewModel as the page's `BindingContext`.

**XAML:**
```xml
<ContentPage xmlns:local="clr-namespace:AccordionApp.ViewModels">
    <ContentPage.BindingContext>
        <local:EmployeeViewModel />
    </ContentPage.BindingContext>
    
    <!-- Content -->
</ContentPage>
```

**C#:**
```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        this.BindingContext = new EmployeeViewModel();
    }
}
```

## Binding Data to SfAccordion

Use `BindableLayout.ItemsSource` to bind the collection to the accordion.

```xml
<syncfusion:SfAccordion BindableLayout.ItemsSource="{Binding Employees}">
    <!-- ItemTemplate defined next -->
</syncfusion:SfAccordion>
```

**C#:**
```csharp
SfAccordion accordion = new SfAccordion();
BindableLayout.SetItemsSource(accordion, viewModel.Employees);
```

## Defining the AccordionItem Template

Use `BindableLayout.ItemTemplate` to define how each data item is rendered.

```xml
<syncfusion:SfAccordion BindableLayout.ItemsSource="{Binding Employees}">
    <BindableLayout.ItemTemplate>
        <DataTemplate>
            <syncfusion:AccordionItem IsExpanded="{Binding IsExpanded}">
                <syncfusion:AccordionItem.Header>
                    <Grid HeightRequest="48">
                        <Label Text="{Binding Name}" 
                               Margin="16,14,0,14" 
                               CharacterSpacing="0.25" 
                               FontFamily="Roboto-Regular" 
                               FontSize="14" />
                    </Grid>
                </syncfusion:AccordionItem.Header>
                <syncfusion:AccordionItem.Content>
                    <Grid BackgroundColor="#f4f4f4" Padding="16">
                        <Grid.RowDefinitions>
                            <RowDefinition Height="Auto"/>
                            <RowDefinition Height="Auto"/>
                            <RowDefinition Height="Auto"/>
                            <RowDefinition Height="Auto"/>
                            <RowDefinition Height="Auto"/>
                        </Grid.RowDefinitions>
                        <Grid.ColumnDefinitions>
                            <ColumnDefinition Width="Auto"/>
                            <ColumnDefinition Width="*"/>
                        </Grid.ColumnDefinitions>
                        
                        <Label Text="Position: " Grid.Row="0" Grid.Column="0" FontSize="14" />
                        <Label Text="{Binding Position}" Grid.Row="0" Grid.Column="1" FontSize="14" />
                        
                        <Label Text="Organization: " Grid.Row="1" Grid.Column="0" FontSize="14" />
                        <Label Text="{Binding OrganizationUnit}" Grid.Row="1" Grid.Column="1" FontSize="14" />
                        
                        <Label Text="Date of Birth: " Grid.Row="2" Grid.Column="0" FontSize="14" />
                        <Label Text="{Binding DateOfBirth}" Grid.Row="2" Grid.Column="1" FontSize="14" />
                        
                        <Label Text="Location: " Grid.Row="3" Grid.Column="0" FontSize="14" />
                        <Label Text="{Binding Location}" Grid.Row="3" Grid.Column="1" FontSize="14" />
                        
                        <Label Text="Phone: " Grid.Row="4" Grid.Column="0" FontSize="14" />
                        <Label Text="{Binding Phone}" Grid.Row="4" Grid.Column="1" FontSize="14" />
                    </Grid>
                </syncfusion:AccordionItem.Content>
            </syncfusion:AccordionItem>
        </DataTemplate>
    </BindableLayout.ItemTemplate>
</syncfusion:SfAccordion>
```

**Key Bindings:**
- `IsExpanded="{Binding IsExpanded}"` - Two-way binding for expansion state
- `Text="{Binding Name}"` - One-way binding for header text
- All content fields bound to respective properties

## Complete Example

**MainPage.xaml:**
```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:syncfusion="clr-namespace:Syncfusion.Maui.Accordion;assembly=Syncfusion.Maui.Expander"
             xmlns:local="clr-namespace:AccordionApp.ViewModels"
             x:Class="AccordionApp.MainPage">
    
    <ContentPage.BindingContext>
        <local:EmployeeViewModel />
    </ContentPage.BindingContext>
    
    <syncfusion:SfAccordion ExpandMode="Single"
                            BindableLayout.ItemsSource="{Binding Employees}">
        <BindableLayout.ItemTemplate>
            <DataTemplate>
                <syncfusion:AccordionItem IsExpanded="{Binding IsExpanded}">
                    <syncfusion:AccordionItem.Header>
                        <Grid HeightRequest="48" BackgroundColor="White">
                            <Label Text="{Binding Name}" 
                                   Margin="16,14,0,14" 
                                   FontSize="14" 
                                   VerticalTextAlignment="Center" />
                        </Grid>
                    </syncfusion:AccordionItem.Header>
                    <syncfusion:AccordionItem.Content>
                        <Grid BackgroundColor="#f4f4f4" Padding="16">
                            <StackLayout Spacing="8">
                                <Label Text="{Binding Position, StringFormat='Position: {0}'}" FontSize="14" />
                                <Label Text="{Binding OrganizationUnit, StringFormat='Organization: {0}'}" FontSize="14" />
                                <Label Text="{Binding DateOfBirth, StringFormat='Date of Birth: {0}'}" FontSize="14" />
                                <Label Text="{Binding Location, StringFormat='Location: {0}'}" FontSize="14" />
                                <Label Text="{Binding Phone, StringFormat='Phone: {0}'}" FontSize="14" />
                                <Label Text="{Binding Description}" 
                                       LineBreakMode="WordWrap" 
                                       FontSize="13" 
                                       Margin="0,8,0,0" />
                            </StackLayout>
                        </Grid>
                    </syncfusion:AccordionItem.Content>
                </syncfusion:AccordionItem>
            </DataTemplate>
        </BindableLayout.ItemTemplate>
    </syncfusion:SfAccordion>
</ContentPage>
```

## Two-Way Binding with IsExpanded

The `IsExpanded` property supports two-way binding, allowing you to:
- Pre-expand specific items by setting `IsExpanded = true` in the model
- Track expansion state changes in your ViewModel
- Programmatically expand/collapse items

**Example: Programmatic Expansion**
```csharp
// In ViewModel
public void ExpandFirstEmployee()
{
    if (Employees.Count > 0)
    {
        Employees[0].IsExpanded = true;
    }
}

public void CollapseAll()
{
    foreach (var employee in Employees)
    {
        employee.IsExpanded = false;
    }
}
```

**Example: Tracking Expansion State**
```csharp
// In Model
private bool isExpanded;
public bool IsExpanded
{
    get => isExpanded;
    set
    {
        isExpanded = value;
        OnPropertyChanged();
        
        // Log or handle expansion state change
        Debug.WriteLine($"{Name} is now {(value ? "expanded" : "collapsed")}");
    }
}
```

## Best Practices

### 1. Always Implement INotifyPropertyChanged
Ensures UI updates when data changes.

### 2. Use ObservableCollection for Dynamic Data
Enables automatic UI updates when items are added/removed.

### 3. Wrap Labels in Containers
Never use `Label` directly as Header/Content - always wrap in `Grid` or other container.

### 4. Optimize Large Lists
For large datasets (100+ items), consider:
- Virtualizing content (load on expand)
- Lazy loading images
- Pagination or infinite scroll

### 5. Handle Null Values
Use null-conditional operators and fallback values:
```xml
<Label Text="{Binding Phone, TargetNullValue='N/A'}" />
```

### 6. Use StringFormat for Simple Formatting
```xml
<Label Text="{Binding Salary, StringFormat='${0:N2}'}" />
```

## Troubleshooting

**Problem:** Items don't update when data changes
**Solution:** Ensure model implements `INotifyPropertyChanged` and `OnPropertyChanged()` is called in property setters

**Problem:** Adding/removing items doesn't update UI
**Solution:** Use `ObservableCollection<T>` instead of `List<T>`

**Problem:** Binding errors in output
**Solution:** Verify property names match exactly (case-sensitive) between model and bindings

**Problem:** IsExpanded binding doesn't work
**Solution:** Ensure `IsExpanded` property in model implements `INotifyPropertyChanged` and is a `bool` type

## Additional Resources

- [.NET MAUI Data Binding Documentation](https://learn.microsoft.com/en-us/dotnet/maui/fundamentals/data-binding/)
- [BindableLayout Documentation](https://learn.microsoft.com/en-us/dotnet/maui/user-interface/layouts/bindablelayout)
- [Sample GitHub Repository](https://github.com/SyncfusionExamples/binding-items-using-bindable-layout-in-.net-maui-accordion)
