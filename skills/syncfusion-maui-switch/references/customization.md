# Customization in .NET MAUI Switch

## Table of Contents
- [Overview](#overview)
- [Color Customization](#color-customization)
  - [Track Colors](#track-colors)
  - [Thumb Colors](#thumb-colors)
  - [State-Specific Colors](#state-specific-colors)
- [Sizing Customization](#sizing-customization)
  - [Track Dimensions](#track-dimensions)
  - [Thumb Dimensions](#thumb-dimensions)
  - [Border Thickness](#border-thickness)
  - [Corner Radius](#corner-radius)
- [Custom Icons and Paths](#custom-icons-and-paths)
  - [Adding Custom Icons](#adding-custom-icons)
  - [Icon Colors](#icon-colors)
  - [Path Data Guidelines](#path-data-guidelines)
- [Complete Customization Examples](#complete-customization-examples)
- [Best Practices](#best-practices)

## Overview

The .NET MAUI Switch (SfSwitch) provides extensive customization options through the `SwitchSettings` class. You can customize colors, dimensions, borders, corner radius, and even add custom icons to create unique switch designs that match your brand or theme.

**Customization is applied using Visual State Manager**, allowing different styles for different states (On, Off, Indeterminate, Hovered, Pressed, Disabled).

## Color Customization

### Track Colors

The track is the background rail that the thumb slides along. Customize it with these properties:

- **TrackBackground** (Brush): Fill color of the track
- **TrackStroke** (Color): Border color of the track

**Example:**
```xml
<buttons:SfSwitch IsOn="True">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="On">
                <VisualState.Setters>
                    <Setter Property="SwitchSettings">
                        <Setter.Value>
                            <buttons:SwitchSettings
                                TrackBackground="#4CAF50"
                                TrackStroke="#388E3C"/>
                        </Setter.Value>
                    </Setter>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</buttons:SfSwitch>
```

**C# Code:**
```csharp
SfSwitch sfSwitch = new SfSwitch();
SwitchSettings onStyle = new SwitchSettings();
onStyle.TrackBackground = new SolidColorBrush(Color.FromRgba("#4CAF50"));
onStyle.TrackStroke = Color.FromRgba("#388E3C");

VisualStateGroupList visualStateGroupList = new VisualStateGroupList();
VisualStateGroup commonStateGroup = new VisualStateGroup();

VisualState onState = new VisualState { Name = "On" };
onState.Setters.Add(new Setter 
{ 
    Property = SfSwitch.SwitchSettingsProperty, 
    Value = onStyle 
});

commonStateGroup.States.Add(onState);
visualStateGroupList.Add(commonStateGroup);
VisualStateManager.SetVisualStateGroups(sfSwitch, visualStateGroupList);
```

### Thumb Colors

The thumb is the circular element that slides to indicate state. Customize it with:

- **ThumbBackground** (Brush): Fill color of the thumb
- **ThumbStroke** (Color): Border color of the thumb

**Example:**
```xml
<buttons:SfSwitch>
    <buttons:SfSwitch.SwitchSettings>
        <buttons:SwitchSettings ThumbBackground="#FFFFFF" ThumbStroke="#E0E0E0" />
    </buttons:SfSwitch.SwitchSettings>
</buttons:SfSwitch>
```

**C# Code:**
```csharp
SwitchSettings settings = new SwitchSettings();
settings.ThumbBackground = new SolidColorBrush(Colors.White);
settings.ThumbStroke = Color.FromRgba("#E0E0E0");
```

### State-Specific Colors

Apply different colors for different states to provide clear visual feedback:

**Example: Different Colors for On/Off States**
```xml
<buttons:SfSwitch IsOn="False">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            
            <!-- On State: Blue theme -->
            <VisualState x:Name="On">
                <VisualState.Setters>
                    <Setter Property="SwitchSettings">
                        <Setter.Value>
                            <buttons:SwitchSettings
                                ThumbBackground="#FF029BFF"
                                ThumbStroke="#FF029BFF"
                                TrackBackground="#22029BFF"
                                TrackStroke="#FF029BFF"/>
                        </Setter.Value>
                    </Setter>
                </VisualState.Setters>
            </VisualState>
            
            <!-- Off State: Pink theme -->
            <VisualState x:Name="Off">
                <VisualState.Setters>
                    <Setter Property="SwitchSettings">
                        <Setter.Value>
                            <buttons:SwitchSettings
                                ThumbBackground="#FFFF0199"
                                ThumbStroke="#FFFF0199"
                                TrackBackground="#22FF0199"
                                TrackStroke="#FFFF0199"/>
                        </Setter.Value>
                    </Setter>
                </VisualState.Setters>
            </VisualState>
            
            <!-- Indeterminate State: Yellow theme -->
            <VisualState x:Name="Indeterminate">
                <VisualState.Setters>
                    <Setter Property="SwitchSettings">
                        <Setter.Value>
                            <buttons:SwitchSettings
                                ThumbBackground="#9ACB0D"
                                ThumbStroke="#9ACB0D"
                                TrackBackground="#DEF991"
                                TrackStroke="#9ACB0D"/>
                        </Setter.Value>
                    </Setter>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</buttons:SfSwitch>
```

**C# Implementation:**
```csharp
SfSwitch sfSwitch = new SfSwitch();

// On state colors
SwitchSettings onStyle = new SwitchSettings();
onStyle.TrackBackground = new SolidColorBrush(Color.FromRgba("#22029BFF"));
onStyle.ThumbBackground = new SolidColorBrush(Color.FromRgba("#FF029BFF"));
onStyle.TrackStroke = Color.FromRgba("#FF029BFF");
onStyle.ThumbStroke = Color.FromRgba("#FF029BFF");

// Off state colors
SwitchSettings offStyle = new SwitchSettings();
offStyle.TrackBackground = new SolidColorBrush(Color.FromRgba("#22FF0199"));
offStyle.ThumbBackground = new SolidColorBrush(Color.FromRgba("#FFFF0199"));
offStyle.TrackStroke = Color.FromRgba("#FFFF0199");
offStyle.ThumbStroke = Color.FromRgba("#FFFF0199");

// Indeterminate state colors
SwitchSettings indeterminateStyle = new SwitchSettings();
indeterminateStyle.TrackBackground = new SolidColorBrush(Color.FromRgba("#DEF991"));
indeterminateStyle.ThumbBackground = new SolidColorBrush(Color.FromRgba("#9ACB0D"));
indeterminateStyle.TrackStroke = Color.FromRgba("#9ACB0D");
indeterminateStyle.ThumbStroke = Color.FromRgba("#9ACB0D");

// Apply via Visual State Manager
VisualStateGroupList visualStateGroupList = new VisualStateGroupList();
VisualStateGroup commonStateGroup = new VisualStateGroup();

VisualState onState = new VisualState { Name = "On" };
onState.Setters.Add(new Setter { Property = SfSwitch.SwitchSettingsProperty, Value = onStyle });

VisualState offState = new VisualState { Name = "Off" };
offState.Setters.Add(new Setter { Property = SfSwitch.SwitchSettingsProperty, Value = offStyle });

VisualState indeterminateState = new VisualState { Name = "Indeterminate" };
indeterminateState.Setters.Add(new Setter { Property = SfSwitch.SwitchSettingsProperty, Value = indeterminateStyle });

commonStateGroup.States.Add(onState);
commonStateGroup.States.Add(offState);
commonStateGroup.States.Add(indeterminateState);

visualStateGroupList.Add(commonStateGroup);
VisualStateManager.SetVisualStateGroups(sfSwitch, visualStateGroupList);
this.Content = sfSwitch;
```

## Sizing Customization

### Track Dimensions

Control the size and shape of the track:

- **TrackWidthRequest** (double): Width of the track
- **TrackHeightRequest** (double): Height of the track

**Example:**
```xml
<buttons:SfSwitch>
    <buttons:SfSwitch.SwitchSettings>
        <buttons:SwitchSettings TrackWidthRequest="75" TrackHeightRequest="25" />
    </buttons:SfSwitch.SwitchSettings>
</buttons:SfSwitch>
```

### Thumb Dimensions

Control the size of the thumb:

- **ThumbWidthRequest** (double): Width of the thumb
- **ThumbHeightRequest** (double): Height of the thumb

**Example:**
```xml
<buttons:SfSwitch>
    <buttons:SfSwitch.SwitchSettings>
        <buttons:SwitchSettings ThumbWidthRequest="20" ThumbHeightRequest="20" />
    </buttons:SfSwitch.SwitchSettings>
</buttons:SfSwitch>

```

### Border Thickness

Control the width of borders:

- **TrackStrokeThickness** (double): Border width of the track
- **ThumbStrokeThickness** (double): Border width of the thumb

**Example:**
```xml
<buttons:SfSwitch>
    <buttons:SfSwitch.SwitchSettings>
        <buttons:SwitchSettings TrackStrokeThickness="2" ThumbStrokeThickness="2" />
    </buttons:SfSwitch.SwitchSettings>
</buttons:SfSwitch>
```

### Corner Radius

Create rounded or sharp corners:

- **TrackCornerRadius** (CornerRadius): Rounding of track corners
- **ThumbCornerRadius** (CornerRadius): Rounding of thumb corners

**Example:**
```xml
<!-- Fully rounded -->
<buttons:SfSwitch>
    <buttons:SfSwitch.SwitchSettings>
        <buttons:SwitchSettings TrackCornerRadius="25" ThumbCornerRadius="20" />
    </buttons:SfSwitch.SwitchSettings>
</buttons:SfSwitch>

<!-- Square corners -->
<buttons:SfSwitch>
    <buttons:SfSwitch.SwitchSettings>
        <buttons:SwitchSettings TrackCornerRadius="0" ThumbCornerRadius="0" />
    </buttons:SfSwitch.SwitchSettings>
</buttons:SfSwitch>

<!-- Partially rounded -->
<buttons:SfSwitch>
    <buttons:SfSwitch.SwitchSettings>
        <buttons:SwitchSettings TrackCornerRadius="4" ThumbCornerRadius="4" />
    </buttons:SfSwitch.SwitchSettings>
</buttons:SfSwitch>`
```

### Complete Sizing Example

**XAML:**
```xml
<buttons:SfSwitch>
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="On">
                <VisualState.Setters>
                    <Setter Property="SwitchSettings">
                        <Setter.Value>
                            <buttons:SwitchSettings
                                TrackWidthRequest="75"
                                TrackHeightRequest="25"
                                ThumbWidthRequest="10"
                                ThumbHeightRequest="10"
                                TrackStrokeThickness="2"
                                ThumbStrokeThickness="2"
                                TrackCornerRadius="4"
                                ThumbCornerRadius="4"/>
                        </Setter.Value>
                    </Setter>
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="Off">
                <VisualState.Setters>
                    <Setter Property="SwitchSettings">
                        <Setter.Value>
                            <buttons:SwitchSettings
                                TrackWidthRequest="75"
                                TrackHeightRequest="25"
                                ThumbWidthRequest="10"
                                ThumbHeightRequest="10"
                                TrackStrokeThickness="2"
                                ThumbStrokeThickness="2"
                                TrackCornerRadius="4"
                                ThumbCornerRadius="4"/>
                        </Setter.Value>
                    </Setter>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</buttons:SfSwitch>
```

**C# Code:**
```csharp
SfSwitch sfSwitch = new SfSwitch();

SwitchSettings customSize = new SwitchSettings();
customSize.TrackWidthRequest = 75;
customSize.TrackHeightRequest = 25;
customSize.ThumbWidthRequest = 10;
customSize.ThumbHeightRequest = 10;
customSize.TrackStrokeThickness = 2;
customSize.ThumbStrokeThickness = 2;
customSize.TrackCornerRadius = 4;
customSize.ThumbCornerRadius = 4;

// Apply to both On and Off states
VisualStateGroupList visualStateGroupList = new VisualStateGroupList();
VisualStateGroup commonStateGroup = new VisualStateGroup();

VisualState onState = new VisualState { Name = "On" };
onState.Setters.Add(new Setter { Property = SfSwitch.SwitchSettingsProperty, Value = customSize });

VisualState offState = new VisualState { Name = "Off" };
offState.Setters.Add(new Setter { Property = SfSwitch.SwitchSettingsProperty, Value = customSize });

commonStateGroup.States.Add(onState);
commonStateGroup.States.Add(offState);

visualStateGroupList.Add(commonStateGroup);
VisualStateManager.SetVisualStateGroups(sfSwitch, visualStateGroupList);
```

## Custom Icons and Paths

### Adding Custom Icons

You can add custom vector icons inside the thumb using the `CustomPath` property. This is useful for adding meaningful icons like checkmarks, X marks, or notification bells.

**Properties:**
- **CustomPath** (Path): Vector path data for the icon
- **IconColor** (Color): Color of the icon

### Icon Colors

**Example:**
```xml
<buttons:SfSwitch>
    <buttons:SfSwitch.SwitchSettings>
        <buttons:SwitchSettings IconColor="DarkGreen" CustomPath="M17.2558 12.7442L15.8333 11.3217V8.33341..." />
    </buttons:SfSwitch.SwitchSettings>
</buttons:SfSwitch>
```

### Path Data Guidelines

**Important:** Keep the CustomPath size within the thumb dimensions to avoid clipping.

**Example: Notification Bell Icon**
```xml
<buttons:SfSwitch IsOn="{x:Null}" AllowIndeterminateState="True">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            
            <!-- On State: Bell icon enabled -->
            <VisualState x:Name="On">
                <VisualState.Setters>
                    <Setter Property="SwitchSettings">
                        <Setter.Value>
                            <buttons:SwitchSettings
                                TrackHeightRequest="40"
                                TrackWidthRequest="80"
                                ThumbHeightRequest="30"
                                ThumbWidthRequest="30"
                                IconColor="DarkGreen"
                                CustomPath="M17.2558 12.7442L15.8333 11.3217V8.33341C15.8333 5.65258 14.0125 3.39425 11.5458 2.71508C11.3017 2.10008 10.705 1.66675 10 1.66675C9.295 1.66675 8.69833 2.10008 8.45417 2.71508C5.9875 3.39508 4.16667 5.65258 4.16667 8.33341V11.3217L2.74417 12.7442C2.66663 12.8215 2.60514 12.9133 2.56324 13.0144C2.52133 13.1156 2.49984 13.224 2.5 13.3334V15.0001C2.5 15.2211 2.5878 15.4331 2.74408 15.5893C2.90036 15.7456 3.11232 15.8334 3.33333 15.8334H16.6667C16.8877 15.8334 17.0996 15.7456 17.2559 15.5893C17.4122 15.4331 17.5 15.2211 17.5 15.0001V13.3334C17.5002 13.224 17.4787 13.1156 17.4368 13.0144C17.3949 12.9133 17.3334 12.8215 17.2558 12.7442V12.7442ZM15.8333 14.1667H4.16667V13.6784L5.58917 12.2559C5.6667 12.1787 5.72819 12.0868 5.7701 11.9857C5.812 11.8846 5.83349 11.7762 5.83333 11.6667V8.33341C5.83333 6.03591 7.7025 4.16675 10 4.16675C12.2975 4.16675 14.1667 6.03591 14.1667 8.33341V11.6667C14.1667 11.8884 14.2542 12.1001 14.4108 12.2559L15.8333 13.6784V14.1667ZM10 18.3334C10.5161 18.3341 11.0195 18.1739 11.4404 17.8752C11.8613 17.5765 12.1786 17.1541 12.3483 16.6667H7.65167C7.82139 17.1541 8.13873 17.5765 8.55959 17.8752C8.98045 18.1739 9.48392 18.3341 10 18.3334V18.3334Z"/>
                        </Setter.Value>
                    </Setter>
                </VisualState.Setters>
            </VisualState>
            
            <!-- Off State: Bell muted icon -->
            <VisualState x:Name="Off">
                <VisualState.Setters>
                    <Setter Property="SwitchSettings">
                        <Setter.Value>
                            <buttons:SwitchSettings
                                TrackHeightRequest="40"
                                TrackWidthRequest="80"
                                ThumbHeightRequest="30"
                                ThumbWidthRequest="30"
                                IconColor="Red"
                                CustomPath="M10.0003 18.3334C10.5164 18.3341 11.0198 18.1739 11.4407 17.8752C11.8616 17.5765 12.1789 17.1541 12.3486 16.6667H7.65197C7.82168 17.1541 8.13903 17.5765 8.55989 17.8752C8.98075 18.1739 9.48422 18.3341 10.0003 18.3334V18.3334ZM17.5003 15.0001V13.3334C17.5005 13.224 17.479 13.1156 17.4371 13.0144C17.3952 12.9133 17.3337 12.8215 17.2561 12.7442L15.8336 11.3217V8.33341C15.8336 5.65258 14.0128 3.39425 11.5461 2.71508C11.302 2.10008 10.7053 1.66675 10.0003 1.66675C9.2953 1.66675 8.69863 2.10008 8.45447 2.71508C7.35197 3.01841 6.40197 3.65508 5.66613 4.48758L3.08947 1.91091L1.91113 3.08925L16.9111 18.0892L18.0895 16.9109L16.9545 15.7759C17.114 15.7182 17.252 15.6128 17.3496 15.474C17.4472 15.3352 17.4998 15.1698 17.5003 15.0001V15.0001ZM10.0003 4.16675C12.2978 4.16675 14.167 6.03591 14.167 8.33341V11.6667C14.167 11.8884 14.2545 12.1001 14.4111 12.2559L15.8336 13.6784V14.1667H15.3453L6.83947 5.66091C7.6003 4.75425 8.72613 4.16675 10.0003 4.16675ZM5.58947 12.2559C5.667 12.1787 5.72849 12.0868 5.7704 11.9857C5.8123 11.8846 5.83379 11.7762 5.83363 11.6667V9.26758L4.22363 7.65758C4.1978 7.88091 4.16697 8.10341 4.16697 8.33341V11.3217L2.74447 12.7442C2.66693 12.8215 2.60544 12.9133 2.56354 13.0144C2.52163 13.1156 2.50014 13.224 2.5003 13.3334V15.0001C2.5003 15.2211 2.5881 15.4331 2.74438 15.5893C2.90066 15.7456 3.11262 15.8334 3.33363 15.8334H12.3995L10.7328 14.1667H4.16697V13.6784L5.58947 12.2559Z"/>
                        </Setter.Value>
                    </Setter>
                </VisualState.Setters>
            </VisualState>
            
            <!-- Indeterminate State: Bell with minus -->
            <VisualState x:Name="Indeterminate">
                <VisualState.Setters>
                    <Setter Property="SwitchSettings">
                        <Setter.Value>
                            <buttons:SwitchSettings
                                TrackHeightRequest="40"
                                TrackWidthRequest="80"
                                ThumbHeightRequest="30"
                                ThumbWidthRequest="30"
                                IconColor="#FB9604"
                                CustomPath="M17.2558 12.7442L15.8333 11.3217V8.33341C15.8333 5.65258 14.0125 3.39425 11.5458 2.71508C11.3017 2.10008 10.705 1.66675 10 1.66675C9.295 1.66675 8.69833 2.10008 8.45417 2.71508C5.9875 3.39508 4.16667 5.65258 4.16667 8.33341V11.3217L2.74417 12.7442C2.66663 12.8215 2.60514 12.9133 2.56324 13.0144C2.52133 13.1156 2.49984 13.224 2.5 13.3334V15.0001C2.5 15.2211 2.5878 15.4331 2.74408 15.5893C2.90036 15.7456 3.11232 15.8334 3.33333 15.8334H16.6667C16.8877 15.8334 17.0996 15.7456 17.2559 15.5893C17.4122 15.4331 17.5 15.2211 17.5 15.0001V13.3334C17.5002 13.224 17.4787 13.1156 17.4368 13.0144C17.3949 12.9133 17.3334 12.8215 17.2558 12.7442V12.7442ZM15.8333 14.1667H4.16667V13.6784L5.58917 12.2559C5.6667 12.1787 5.72819 12.0868 5.7701 11.9857C5.812 11.8846 5.83349 11.7762 5.83333 11.6667V8.33341C5.83333 6.03591 7.7025 4.16675 10 4.16675C12.2975 4.16675 14.1667 6.03591 14.1667 8.33341V11.6667C14.1667 11.8884 14.2542 12.1001 14.4108 12.2559L15.8333 13.6784V14.1667ZM10 18.3334C10.5161 18.3341 11.0195 18.1739 11.4404 17.8752C11.8613 17.5765 12.1786 17.1541 12.3483 16.6667H7.65167C7.82139 17.1541 8.13873 17.5765 8.55959 17.8752C8.98045 18.1739 9.48392 18.3341 10 18.3334V18.3334Z M6.69727 8.33325H13.3023V9.99992H6.69727V8.33325Z"/>
                        </Setter.Value>
                    </Setter>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</buttons:SfSwitch>
```

**C# Implementation:**
```csharp
SfSwitch sfSwitch = new SfSwitch();

SwitchSettings onStyle = new SwitchSettings();
onStyle.TrackWidthRequest = 80;
onStyle.TrackHeightRequest = 40;
onStyle.ThumbWidthRequest = 30;
onStyle.ThumbHeightRequest = 30;
onStyle.IconColor = Colors.DarkGreen;
onStyle.CustomPath = "M17.2558 12.7442L15.8333 11.3217V8.33341C15.8333 5.65258...";

// Apply to visual states...
```

**Important Note:** Keep the CustomPath size within the thumb dimensions (ThumbWidthRequest and ThumbHeightRequest) to prevent clipping.

## Complete Customization Examples

### Example 1: Material Design Style

```xml
<buttons:SfSwitch IsOn="True">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="On">
                <VisualState.Setters>
                    <Setter Property="SwitchSettings">
                        <Setter.Value>
                            <buttons:SwitchSettings
                                TrackBackground="#4CAF50"
                                TrackStroke="#4CAF50"
                                ThumbBackground="#FFFFFF"
                                ThumbStroke="#FFFFFF"
                                TrackWidthRequest="60"
                                TrackHeightRequest="30"
                                ThumbWidthRequest="24"
                                ThumbHeightRequest="24"
                                TrackCornerRadius="15"
                                ThumbCornerRadius="12"/>
                        </Setter.Value>
                    </Setter>
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="Off">
                <VisualState.Setters>
                    <Setter Property="SwitchSettings">
                        <Setter.Value>
                            <buttons:SwitchSettings
                                TrackBackground="#E0E0E0"
                                TrackStroke="#E0E0E0"
                                ThumbBackground="#FFFFFF"
                                ThumbStroke="#BDBDBD"
                                TrackWidthRequest="60"
                                TrackHeightRequest="30"
                                ThumbWidthRequest="24"
                                ThumbHeightRequest="24"
                                TrackCornerRadius="15"
                                ThumbCornerRadius="12"/>
                        </Setter.Value>
                    </Setter>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</buttons:SfSwitch>
```

### Example 2: iOS/Cupertino Style

```xml
<buttons:SfSwitch IsOn="True">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="On">
                <VisualState.Setters>
                    <Setter Property="SwitchSettings">
                        <Setter.Value>
                            <buttons:SwitchSettings
                                TrackBackground="#34C759"
                                TrackStroke="Transparent"
                                ThumbBackground="#FFFFFF"
                                ThumbStroke="Transparent"
                                TrackWidthRequest="51"
                                TrackHeightRequest="31"
                                ThumbWidthRequest="27"
                                ThumbHeightRequest="27"
                                TrackCornerRadius="15.5"
                                ThumbCornerRadius="13.5"
                                TrackStrokeThickness="0"
                                ThumbStrokeThickness="0"/>
                        </Setter.Value>
                    </Setter>
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="Off">
                <VisualState.Setters>
                    <Setter Property="SwitchSettings">
                        <Setter.Value>
                            <buttons:SwitchSettings
                                TrackBackground="#E9E9EA"
                                TrackStroke="#D1D1D6"
                                ThumbBackground="#FFFFFF"
                                ThumbStroke="Transparent"
                                TrackWidthRequest="51"
                                TrackHeightRequest="31"
                                ThumbWidthRequest="27"
                                ThumbHeightRequest="27"
                                TrackCornerRadius="15.5"
                                ThumbCornerRadius="13.5"
                                TrackStrokeThickness="1"
                                ThumbStrokeThickness="0"/>
                        </Setter.Value>
                    </Setter>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</buttons:SfSwitch>
```

### Example 3: Custom Brand Theme

```csharp
// Apply your brand colors
var brandPrimary = Color.FromRgba("#6200EE");
var brandSecondary = Color.FromRgba("#03DAC6");

SfSwitch brandedSwitch = new SfSwitch();

SwitchSettings onBrand = new SwitchSettings
{
    TrackBackground = new SolidColorBrush(brandPrimary),
    ThumbBackground = new SolidColorBrush(Colors.White),
    TrackStroke = brandPrimary,
    ThumbStroke = brandPrimary,
    TrackWidthRequest = 70,
    TrackHeightRequest = 35,
    ThumbWidthRequest = 28,
    ThumbHeightRequest = 28,
    TrackCornerRadius = 17.5,
    ThumbCornerRadius = 14,
    TrackStrokeThickness = 2,
    ThumbStrokeThickness = 2
};

SwitchSettings offBrand = new SwitchSettings
{
    TrackBackground = new SolidColorBrush(Color.FromRgba("#F0F0F0")),
    ThumbBackground = new SolidColorBrush(Colors.White),
    TrackStroke = Color.FromRgba("#CCCCCC"),
    ThumbStroke = Color.FromRgba("#CCCCCC"),
    TrackWidthRequest = 70,
    TrackHeightRequest = 35,
    ThumbWidthRequest = 28,
    ThumbHeightRequest = 28,
    TrackCornerRadius = 17.5,
    ThumbCornerRadius = 14,
    TrackStrokeThickness = 2,
    ThumbStrokeThickness = 2
};

// Apply via Visual State Manager
VisualStateGroupList visualStateGroupList = new VisualStateGroupList();
VisualStateGroup commonStateGroup = new VisualStateGroup();

VisualState onState = new VisualState { Name = "On" };
onState.Setters.Add(new Setter { Property = SfSwitch.SwitchSettingsProperty, Value = onBrand });

VisualState offState = new VisualState { Name = "Off" };
offState.Setters.Add(new Setter { Property = SfSwitch.SwitchSettingsProperty, Value = offBrand });

commonStateGroup.States.Add(onState);
commonStateGroup.States.Add(offState);
visualStateGroupList.Add(commonStateGroup);
VisualStateManager.SetVisualStateGroups(brandedSwitch, visualStateGroupList);
```

## Best Practices

### 1. Maintain Consistent Dimensions Across States
```xml
<!-- Good: Same sizes for all states -->
<VisualState x:Name="On">
    <VisualState.Setters>
        <Setter Property="SwitchSettings">
            <Setter.Value>
                <buttons:SwitchSettings TrackWidthRequest="60" TrackHeightRequest="30"/>
            </Setter.Value>
        </Setter>
    </VisualState.Setters>
</VisualState>
<VisualState x:Name="Off">
    <VisualState.Setters>
        <Setter Property="SwitchSettings">
            <Setter.Value>
                <buttons:SwitchSettings TrackWidthRequest="60" TrackHeightRequest="30"/>
            </Setter.Value>
        </Setter>
    </VisualState.Setters>
</VisualState>
```

### 2. Use Sufficient Color Contrast
- Ensure track and thumb colors are clearly distinguishable
- Test in both light and dark themes
- Follow WCAG accessibility guidelines

### 3. Keep Icon Paths Simple
- Use simple vector paths for better performance
- Test icons at actual render size
- Ensure icons are recognizable at small sizes

### 4. Test Responsiveness
```csharp
// Make switch size responsive to device
var screenWidth = DeviceDisplay.MainDisplayInfo.Width;
var switchWidth = screenWidth > 600 ? 80 : 60;

settings.TrackWidthRequest = switchWidth;
```

### 5. Consider Platform Conventions
- iOS users expect specific switch styling
- Android users expect Material Design patterns
- Consider using platform-specific styles with `OnPlatform`

### 6. Use Gradients Carefully
```xml
<!-- Linear gradient for track -->
<buttons:SwitchSettings>
    <buttons:SwitchSettings.TrackBackground>
        <LinearGradientBrush StartPoint="0,0" EndPoint="1,0">
            <GradientStop Color="#667eea" Offset="0.0"/>
            <GradientStop Color="#764ba2" Offset="1.0"/>
        </LinearGradientBrush>
    </buttons:SwitchSettings.TrackBackground>
</buttons:SwitchSettings>
```

### 7. Centralize Styling
```csharp
// Create reusable style configurations
public static class SwitchStyles
{
    public static SwitchSettings MaterialOn => new SwitchSettings
    {
        TrackBackground = new SolidColorBrush(Color.FromRgba("#4CAF50")),
        // ... other properties
    };
    
    public static SwitchSettings MaterialOff => new SwitchSettings
    {
        TrackBackground = new SolidColorBrush(Color.FromRgba("#E0E0E0")),
        // ... other properties
    };
}

// Apply in your pages
mySwitch.ApplyStyle(SwitchStyles.MaterialOn);
```

### 8. Performance Considerations
- Avoid excessively complex CustomPath data
- Reuse SwitchSettings objects when possible
- Test on low-end devices to ensure smooth animations

### 9. Accessibility
- Provide sufficient size for touch targets (minimum 44x44 points)
- Ensure color contrast meets accessibility standards
- Don't rely solely on color to convey state
