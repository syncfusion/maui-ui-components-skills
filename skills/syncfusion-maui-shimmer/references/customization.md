# Customization & Animation for .NET MAUI Shimmer

`SfShimmer` exposes six properties to control the visual appearance and animation behavior of the shimmer effect.

---

## Fill

Changes the **background color** of the shimmer placeholder shapes.

- Default: `#F3EDF7` (light purple/lavender)

### XAML

```xml
<shimmer:SfShimmer Type="CirclePersona"
                   Fill="#89CFF0"
                   VerticalOptions="Fill">
    <StackLayout>
        <Label Text="Content is loaded!" />
    </StackLayout>
</shimmer:SfShimmer>
```

### C#

```csharp
SfShimmer shimmer = new SfShimmer()
{
    Type = ShimmerType.CirclePersona,
    Fill = Color.FromArgb("#89CFF0"),
    Content = new Label { Text = "Content is loaded!" }
};
this.Content = shimmer;
```

---

## WaveColor

Changes the **color of the animated wave highlight** that sweeps across the shimmer.

- Default: `#FFFBFE` (near-white)
- Choose a color slightly lighter or darker than `Fill` for a subtle effect, or use a contrasting color for a bold effect.

### XAML

```xml
<shimmer:SfShimmer Type="CirclePersona"
                   WaveColor="#89CFF0"
                   VerticalOptions="Fill">
    <StackLayout>
        <Label Text="Content is loaded!" />
    </StackLayout>
</shimmer:SfShimmer>
```

### C#

```csharp
SfShimmer shimmer = new SfShimmer()
{
    Type = ShimmerType.CirclePersona,
    WaveColor = Color.FromArgb("#89CFF0"),
    Content = new Label { Text = "Content is loaded!" }
};
this.Content = shimmer;
```

---

## WaveWidth

Controls the **width of the wave band** that sweeps across the shimmer.

- Default: `200`
- A smaller value (e.g., `50`) produces a narrow, sharp highlight. A larger value produces a wide, soft glow.

### XAML

```xml
<shimmer:SfShimmer Type="CirclePersona"
                   WaveColor="#89CFF0"
                   WaveWidth="50"
                   VerticalOptions="Fill">
    <StackLayout>
        <Label Text="Content is loaded!" />
    </StackLayout>
</shimmer:SfShimmer>
```

### C#

```csharp
SfShimmer shimmer = new SfShimmer()
{
    Type = ShimmerType.CirclePersona,
    WaveColor = Color.FromArgb("#89CFF0"),
    WaveWidth = 50,
    Content = new Label { Text = "Content is loaded!" }
};
this.Content = shimmer;
```

---

## WaveDirection

Controls the **direction** the wave travels across the shimmer.

| Value | Direction |
|---|---|
| `Default` | Top-left → bottom-right (diagonal) |
| `LeftToRight` | Left → right (horizontal) |
| `RightToLeft` | Right → left (horizontal) |
| `TopToBottom` | Top → bottom (vertical) |
| `BottomToTop` | Bottom → top (vertical) |

### XAML

```xml
<shimmer:SfShimmer Type="CirclePersona"
                   WaveDirection="RightToLeft"
                   VerticalOptions="Fill">
    <StackLayout>
        <Label Text="Content is loaded!" />
    </StackLayout>
</shimmer:SfShimmer>
```

### C#

```csharp
SfShimmer shimmer = new SfShimmer()
{
    Type = ShimmerType.CirclePersona,
    WaveDirection = ShimmerWaveDirection.RightToLeft,
    Content = new Label { Text = "Content is loaded!" }
};
this.Content = shimmer;
```

---

## RepeatCount

Specifies how many times the shimmer shape is **repeated vertically** in the control.

- Default: `1`
- Use this to simulate a list of identical placeholder items without building a custom view.

### XAML

```xml
<shimmer:SfShimmer Type="CirclePersona"
                   RepeatCount="3"
                   VerticalOptions="Fill">
    <StackLayout>
        <Label Text="Content is loaded!" />
    </StackLayout>
</shimmer:SfShimmer>
```

### C#

```csharp
SfShimmer shimmer = new SfShimmer()
{
    Type = ShimmerType.CirclePersona,
    RepeatCount = 3,
    Content = new Label { Text = "Content is loaded!" }
};
this.Content = shimmer;
```

---

## AnimationDuration

Controls **how long one wave cycle takes** in milliseconds.

- Default: `1000` ms (1 second)
- Lower values (e.g., `500`) produce a faster, more energetic animation.
- Higher values (e.g., `3000`) produce a slower, more gentle animation.

### XAML

```xml
<shimmer:SfShimmer Type="CirclePersona"
                   AnimationDuration="3000"
                   VerticalOptions="Fill">
    <StackLayout>
        <Label Text="Content is loaded!" />
    </StackLayout>
</shimmer:SfShimmer>
```

### C#

```csharp
SfShimmer shimmer = new SfShimmer()
{
    Type = ShimmerType.CirclePersona,
    AnimationDuration = 3000,
    Content = new Label { Text = "Content is loaded!" }
};
this.Content = shimmer;
```

---

## Combining Properties

All customization properties can be used together:

```xml
<shimmer:SfShimmer x:Name="shimmer"
                   Type="Article"
                   Fill="#E8F4FD"
                   WaveColor="#B3D9F7"
                   WaveWidth="80"
                   WaveDirection="LeftToRight"
                   RepeatCount="3"
                   AnimationDuration="1500"
                   VerticalOptions="Fill">
    <shimmer:SfShimmer.Content>
        <StackLayout Padding="16">
            <Label Text="Article content loaded!" />
        </StackLayout>
    </shimmer:SfShimmer.Content>
</shimmer:SfShimmer>
```
