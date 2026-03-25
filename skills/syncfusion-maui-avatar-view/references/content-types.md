# Content Types in .NET MAUI Avatar View

The SfAvatarView control supports five different content types for displaying user representations. This guide covers all content types with complete implementation examples.

## Table of Contents

- [Overview](#overview)
- [Default Type](#default-type)
- [Initials Type](#initials-type)
  - [Single Character Initials](#single-character-initials)
  - [Double Character Initials](#double-character-initials)
  - [Customizing Initials Color](#customizing-initials-color)
- [Custom Image Type](#custom-image-type)
- [Avatar Character Type](#avatar-character-type)
- [Group View Type](#group-view-type)
  - [Group with Images](#group-with-images)
  - [Group with Initials](#group-with-initials)
  - [Mixed Images and Initials](#mixed-images-and-initials)
  - [Custom Colors in Groups](#custom-colors-in-groups)
- [Choosing the Right Content Type](#choosing-the-right-content-type)
- [Common Issues and Solutions](#common-issues-and-solutions)

## Overview

The `ContentType` property determines what the Avatar View displays:

| Content Type | Purpose | Use Case |
|--------------|---------|----------|
| **Default** | Built-in vector image | Placeholder during loading |
| **Initials** | Text-based representation | When images aren't available |
| **Custom** | User-provided images | Profile pictures, photos |
| **AvatarCharacter** | Preset vector avatars | Fun, uniform anonymous users |
| **Group** | Multiple users (up to 3) | Team avatars, group chats |

## Default Type

The **Default** content type displays a built-in vector image when no other content is specified.

### When to Use
- Placeholder while loading actual content
- Generic user representation
- Testing or prototyping

### Implementation

**XAML:**
```xaml
<sfavatar:SfAvatarView 
    ContentType="Default"
    Background="OrangeRed"
    WidthRequest="50"
    HeightRequest="50"
    CornerRadius="25"
    Stroke="Black"
    StrokeThickness="1"
    HorizontalOptions="Center"
    VerticalOptions="Center" />
```

**C#:**
```csharp
var avatarView = new SfAvatarView
{
    ContentType = ContentType.Default,
    Background = Colors.OrangeRed,
    WidthRequest = 50,
    HeightRequest = 50,
    CornerRadius = 25,
    Stroke = Colors.Black,
    StrokeThickness = 1,
    HorizontalOptions = LayoutOptions.Center,
    VerticalOptions = LayoutOptions.Center
};
```

### Key Points
- No additional properties required
- Displays a generic user icon
- Background color can be customized
- Respects all standard appearance properties

## Initials Type

The **Initials** content type displays text characters based on a user's name. It supports both single and double character display.

### Key Properties

- **InitialsType** - `SingleCharacter` or `DoubleCharacter`
- **AvatarName** - The name to extract initials from
- **InitialsColor** - The color of the text

### Single Character Initials

Displays the first character of the provided name.

**XAML:**
```xaml
<sfavatar:SfAvatarView 
    ContentType="Initials"
    InitialsType="SingleCharacter"
    AvatarName="Alex"
    Background="CornflowerBlue"
    InitialsColor="White"
    WidthRequest="60"
    HeightRequest="60"
    CornerRadius="30" />
```

**C#:**
```csharp
var avatarView = new SfAvatarView
{
    ContentType = ContentType.Initials,
    InitialsType = InitialsType.SingleCharacter,
    AvatarName = "Alex",
    Background = Colors.CornflowerBlue,
    InitialsColor = Colors.White,
    WidthRequest = 60,
    HeightRequest = 60,
    CornerRadius = 30
};
```

**Result:** Displays "A"

### Double Character Initials

Displays two characters based on the name:
- **Single word:** First and last letters (e.g., "Alex" → "AX")
- **Multiple words:** First letter of first and last words (e.g., "John Doe" → "JD")

**XAML:**
```xaml
<sfavatar:SfAvatarView 
    ContentType="Initials"
    InitialsType="DoubleCharacter"
    AvatarName="John Doe"
    Background="OrangeRed"
    InitialsColor="White"
    WidthRequest="60"
    HeightRequest="60"
    CornerRadius="30" />
```

**C#:**
```csharp
var avatarView = new SfAvatarView
{
    ContentType = ContentType.Initials,
    InitialsType = InitialsType.DoubleCharacter,
    AvatarName = "John Doe",
    Background = Colors.OrangeRed,
    InitialsColor = Colors.White,
    WidthRequest = 60,
    HeightRequest = 60,
    CornerRadius = 30
};
```

**Results:**
- "John Doe" → "JD"
- "Alex" → "AX"
- "Sarah Jane Smith" → "SS"

### Customizing Initials Color

**XAML:**
```xaml
<sfavatar:SfAvatarView 
    ContentType="Initials"
    InitialsType="DoubleCharacter"
    AvatarName="Sarah"
    Background="LightGray"
    InitialsColor="DarkSlateGray"
    FontSize="20"
    FontAttributes="Bold" />
```

**C#:**
```csharp
var avatarView = new SfAvatarView
{
    ContentType = ContentType.Initials,
    InitialsType = InitialsType.DoubleCharacter,
    AvatarName = "Sarah",
    Background = Colors.LightGray,
    InitialsColor = Colors.DarkSlateGray,
    FontSize = 20,
    FontAttributes = FontAttributes.Bold
};
```

## Custom Image Type

The **Custom** content type allows you to display user-provided images.

### When to Use
- User profile pictures
- Contact photos
- Any custom image content

### Implementation

**XAML:**
```xaml
<sfavatar:SfAvatarView 
    ContentType="Custom"
    ImageSource="user_profile.png"
    WidthRequest="100"
    HeightRequest="100"
    CornerRadius="50"
    Stroke="Gray"
    StrokeThickness="2" />
```

**C#:**
```csharp
var avatarView = new SfAvatarView
{
    ContentType = ContentType.Custom,
    ImageSource = "user_profile.png",
    WidthRequest = 100,
    HeightRequest = 100,
    CornerRadius = 50,
    Stroke = Colors.Gray,
    StrokeThickness = 2
};
```

### Loading Images from Different Sources

**From Resources:**
```csharp
ImageSource = "user.png"
```

**From File System:**
```csharp
ImageSource = ImageSource.FromFile("/path/to/image.png")
```

**From URI:**
```csharp
ImageSource = ImageSource.FromUri(new Uri("https://example.com/avatar.jpg"))
```

**From Stream:**
```csharp
ImageSource = ImageSource.FromStream(() => new MemoryStream(imageBytes))
```

### Best Practices
- Place images in `Resources/Images` folder for embedded resources
- Use appropriate image sizes (don't load 4K images for 100px avatars)
- Consider caching for remote images
- Provide fallback to initials if image fails to load

## Avatar Character Type

The **AvatarCharacter** content type displays preset vector images from a built-in collection.

### Available Characters

Avatar1 through Avatar32 are available as preset options.

### Implementation

**XAML:**
```xaml
<sfavatar:SfAvatarView 
    ContentType="AvatarCharacter"
    AvatarCharacter="Avatar8"
    Background="DeepSkyBlue"
    WidthRequest="50"
    HeightRequest="50"
    CornerRadius="25"
    Stroke="Black"
    StrokeThickness="1" />
```

**C#:**
```csharp
var avatarView = new SfAvatarView
{
    ContentType = ContentType.AvatarCharacter,
    AvatarCharacter = AvatarCharacter.Avatar8,
    Background = Colors.DeepSkyBlue,
    WidthRequest = 50,
    HeightRequest = 50,
    CornerRadius = 25,
    Stroke = Colors.Black,
    StrokeThickness = 1
};
```

### Use Cases
- Anonymous user representation
- Guest accounts
- Gamification (assigning characters to users)
- Consistent placeholder avatars

### Dynamic Assignment

```csharp
// Assign random avatar character
var random = new Random();
var avatarNumber = random.Next(1, 33);
var avatarCharacter = (AvatarCharacter)Enum.Parse(typeof(AvatarCharacter), $"Avatar{avatarNumber}");

var avatarView = new SfAvatarView
{
    ContentType = ContentType.AvatarCharacter,
    AvatarCharacter = avatarCharacter
};
```

## Group View Type

The **Group** content type displays up to three images or initials in a single avatar view, perfect for representing teams or groups.

### Key Properties

- **GroupSource** - Collection of items to display
- **ImageSourceMemberPath** - Property path for images
- **InitialsMemberPath** - Property path for initials
- **BackgroundColorMemberPath** - Property path for background colors
- **InitialsColorMemberPath** - Property path for initials text colors

### Data Model Setup

Create a model class for group members:

```csharp
public class Employee
{
    public string Name { get; set; }
    public string ImageSource { get; set; }
    public Color BackgroundColor { get; set; }
    public Color InitialsColor { get; set; }
}
```

Create a ViewModel:

```csharp
public class EmployeeViewModel : INotifyPropertyChanged
{
    private ObservableCollection<Employee> _collectionImage;
    
    public ObservableCollection<Employee> CollectionImage
    {
        get => _collectionImage;
        set
        {
            _collectionImage = value;
            OnPropertyChanged(nameof(CollectionImage));
        }
    }
    
    public EmployeeViewModel()
    {
        CollectionImage = new ObservableCollection<Employee>
        {
            new Employee 
            { 
                Name = "Mike", 
                ImageSource = "mike.png", 
                BackgroundColor = Colors.Gray 
            },
            new Employee 
            { 
                Name = "Alex", 
                ImageSource = "alex.png", 
                BackgroundColor = Colors.Bisque 
            },
            new Employee 
            { 
                Name = "Ellana", 
                ImageSource = "ellana.png", 
                BackgroundColor = Colors.LightCoral 
            }
        };
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

### Group with Images

**XAML:**
```xaml
<ContentPage.BindingContext>
    <local:EmployeeViewModel />
</ContentPage.BindingContext>

<sfavatar:SfAvatarView 
    ContentType="Group"
    GroupSource="{Binding CollectionImage}"
    ImageSourceMemberPath="ImageSource"
    BackgroundColorMemberPath="BackgroundColor"
    WidthRequest="50"
    HeightRequest="50"
    CornerRadius="25"
    Stroke="Black"
    StrokeThickness="1"
    HorizontalOptions="Center"
    VerticalOptions="Center" />
```

**C#:**
```csharp
var viewModel = new EmployeeViewModel();

var avatarView = new SfAvatarView
{
    ContentType = ContentType.Group,
    GroupSource = viewModel.CollectionImage,
    ImageSourceMemberPath = "ImageSource",
    BackgroundColorMemberPath = "BackgroundColor",
    WidthRequest = 50,
    HeightRequest = 50,
    CornerRadius = 25,
    Stroke = Colors.Black,
    StrokeThickness = 1,
    BindingContext = viewModel
};
```

### Group with Initials

Display initials only (no images):

```csharp
public EmployeeViewModel()
{
    CollectionImage = new ObservableCollection<Employee>
    {
        new Employee { Name = "Mike", BackgroundColor = Colors.Gray },
        new Employee { Name = "Alex", BackgroundColor = Colors.Bisque },
        new Employee { Name = "Ellana", BackgroundColor = Colors.LightCoral }
    };
}
```

**XAML:**
```xaml
<sfavatar:SfAvatarView 
    ContentType="Group"
    GroupSource="{Binding CollectionImage}"
    InitialsMemberPath="Name"
    BackgroundColorMemberPath="BackgroundColor"
    WidthRequest="50"
    HeightRequest="50"
    CornerRadius="25"
    Stroke="Black"
    StrokeThickness="1" />
```

### Mixed Images and Initials

You can mix images and initials in the same group view:

```csharp
public EmployeeViewModel()
{
    CollectionImage = new ObservableCollection<Employee>
    {
        new Employee { ImageSource = "mike.png" },
        new Employee { Name = "Alex", BackgroundColor = Colors.White },
        new Employee { ImageSource = "ellana.png" }
    };
}
```

**XAML:**
```xaml
<sfavatar:SfAvatarView 
    ContentType="Group"
    GroupSource="{Binding CollectionImage}"
    ImageSourceMemberPath="ImageSource"
    InitialsMemberPath="Name"
    BackgroundColorMemberPath="BackgroundColor"
    WidthRequest="50"
    HeightRequest="50"
    CornerRadius="25" />
```

**Behavior:**
- If `ImageSource` is provided → displays image
- If `ImageSource` is empty/null → displays initials from `Name`

### Custom Colors in Groups

**Custom Initials Color:**
```xaml
<sfavatar:SfAvatarView 
    ContentType="Group"
    GroupSource="{Binding CollectionImage}"
    InitialsMemberPath="Name"
    InitialsColorMemberPath="InitialsColor"
    WidthRequest="50"
    HeightRequest="50"
    CornerRadius="25" />
```

**Custom Background Color:**
```xaml
<sfavatar:SfAvatarView 
    ContentType="Group"
    GroupSource="{Binding CollectionImage}"
    InitialsMemberPath="Name"
    BackgroundColorMemberPath="BackgroundColor"
    WidthRequest="50"
    HeightRequest="50"
    CornerRadius="25" />
```

## Choosing the Right Content Type

| Scenario | Recommended Type | Fallback |
|----------|------------------|----------|
| User has profile picture | Custom | Initials |
| No user image available | Initials | Default |
| Anonymous/guest users | AvatarCharacter | Default |
| Team/group representation | Group | Multiple separate avatars |
| Loading state | Default | N/A |
| Testing/prototyping | Default or AvatarCharacter | N/A |

## Common Issues and Solutions

### Issue: Initials Not Displaying

**Problem:** AvatarName is set but nothing shows

**Solutions:**
1. Verify ContentType is set to `Initials`
2. Check that AvatarName is not null or empty
3. Ensure InitialsColor contrasts with Background
4. Verify InitialsType is set

### Issue: Custom Image Not Loading

**Problem:** ContentType is Custom but image doesn't appear

**Solutions:**
1. Verify image exists in Resources/Images
2. Check ImageSource path is correct (case-sensitive)
3. Ensure ContentType is set to `Custom`
4. Check image build action is `MauiImage`

### Issue: Group View Shows Only One Item

**Problem:** GroupSource has multiple items but only one displays

**Solutions:**
1. Verify ContentType is set to `Group`
2. Check that GroupSource is an observable collection
3. Ensure member paths are correctly set
4. Verify collection has actual data

### Issue: Initials Show Wrong Characters

**Problem:** Double character shows unexpected letters

**Explanation:**
- Single word: First and last letters (not first two)
- Multiple words: First letters of first and last words

**Example:**
- "Alex" → "AX" (not "AL")
- "John Michael Doe" → "JD" (not "JM")

### Issue: Group View Performance

**Problem:** Slow rendering with large collections

**Solution:**
- Limit GroupSource to maximum 3 items (control limitation)
- Collections larger than 3 items only display first 3
- Consider using separate avatars for more than 3 users

## Advanced Patterns

### Dynamic Content Type Switching

```csharp
public void SetAvatarContent(SfAvatarView avatar, User user)
{
    if (!string.IsNullOrEmpty(user.ImageUrl))
    {
        avatar.ContentType = ContentType.Custom;
        avatar.ImageSource = ImageSource.FromUri(new Uri(user.ImageUrl));
    }
    else if (!string.IsNullOrEmpty(user.Name))
    {
        avatar.ContentType = ContentType.Initials;
        avatar.InitialsType = InitialsType.DoubleCharacter;
        avatar.AvatarName = user.Name;
        avatar.Background = GenerateColorFromName(user.Name);
    }
    else
    {
        avatar.ContentType = ContentType.Default;
        avatar.Background = Colors.Gray;
    }
}
```

### Consistent Color Generation for Initials

```csharp
public Color GenerateColorFromName(string name)
{
    var hash = name.GetHashCode();
    var random = new Random(hash);
    
    return Color.FromRgb(
        random.Next(100, 200),
        random.Next(100, 200),
        random.Next(100, 200)
    );
}
```

This ensures the same name always gets the same color, providing visual consistency.
