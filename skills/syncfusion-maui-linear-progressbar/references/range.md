# Range Customization

The range of a Linear ProgressBar defines the minimum and maximum values that represent the entire span of progress. By default, the range is 0 to 100, but you can customize it to match your specific requirements.

## Default Range

By default, the Linear ProgressBar uses:
- **Minimum**: 0
- **Maximum**: 100
- **Progress**: Values between 0 and 100

```xaml
<!-- Default range: 0-100 -->
<progressBar:SfLinearProgressBar Progress="75"/>
```

```csharp
// Default range: 0-100
var progressBar = new SfLinearProgressBar
{
    Progress = 75  // 75%
};
```

## Custom Range Properties

### Minimum Property

Defines the starting value of the range.

```csharp
progressBar.Minimum = 0;  // Default
```

### Maximum Property

Defines the ending value of the range.

```csharp
progressBar.Maximum = 100;  // Default
```

## Factor Values (0.0 to 1.0)

A common customization is using decimal factor values instead of percentages.

### Implementation

```xaml
<progressBar:SfLinearProgressBar Minimum="0" 
                                 Maximum="1"
                                 Progress="0.75"/>
```

```csharp
var progressBar = new SfLinearProgressBar
{
    Minimum = 0,
    Maximum = 1,
    Progress = 0.75  // Represents 75%
};
```

### When to Use Factor Values

- **Scientific calculations**: When working with normalized values
- **Mathematical operations**: When progress is calculated as a ratio
- **Probability/Statistics**: When displaying likelihood (0.0 = 0%, 1.0 = 100%)
- **Consistency with APIs**: When your data source uses 0-1 range

## Custom Numeric Ranges

You can use any numeric range that makes sense for your data.

### Range 0-500

```xaml
<progressBar:SfLinearProgressBar Minimum="0" 
                                 Maximum="500"
                                 Progress="375"/>
```

```csharp
// Progress from 0 to 500
var progressBar = new SfLinearProgressBar
{
    Minimum = 0,
    Maximum = 500,
    Progress = 375  // 75% (375/500)
};
```

### Range 1-10

```csharp
// Rating system 1-10
var ratingBar = new SfLinearProgressBar
{
    Minimum = 1,
    Maximum = 10,
    Progress = 7.5  // 7.5 out of 10
};
```

### Negative Ranges

```csharp
// Temperature range -20 to 40
var temperatureBar = new SfLinearProgressBar
{
    Minimum = -20,
    Maximum = 40,
    Progress = 15  // 15 degrees
};
```

### Score/Points System

```csharp
// Game score progress
var scoreProgress = new SfLinearProgressBar
{
    Minimum = 0,
    Maximum = 1000,  // Max score
    Progress = 650   // Current score
};
```

## Common Mistakes

❌ **Mixing ranges and gradient stops**
```csharp
// WRONG: Range is 0-1, but gradient stops use 0-100
var progressBar = new SfLinearProgressBar
{
    Minimum = 0,
    Maximum = 1,
    Progress = 0.75
};
progressBar.GradientStops.Add(new ProgressGradientStop { Color = Colors.Red, Value = 0 });
progressBar.GradientStops.Add(new ProgressGradientStop { Color = Colors.Green, Value = 100 }); // Wrong!
```

✅ **Matching gradient stops to range**
```csharp
// RIGHT: Gradient stops match 0-1 range
var progressBar = new SfLinearProgressBar
{
    Minimum = 0,
    Maximum = 1,
    Progress = 0.75
};
progressBar.GradientStops.Add(new ProgressGradientStop { Color = Colors.Red, Value = 0 });
progressBar.GradientStops.Add(new ProgressGradientStop { Color = Colors.Green, Value = 1 }); // Correct!
```

❌ **Progress outside range**
```csharp
// WRONG: Progress exceeds maximum
var progressBar = new SfLinearProgressBar
{
    Minimum = 0,
    Maximum = 100,
    Progress = 150  // Exceeds max!
};
```

✅ **Validate progress bounds**
```csharp
// RIGHT: Clamp progress to valid range
public void SetProgress(double value)
{
    progressBar.Progress = Math.Clamp(value, progressBar.Minimum, progressBar.Maximum);
}
```