# Localization

## Enable Localization

Configure DataGrid for multiple languages:

```csharp
using Syncfusion.Maui.DataGrid;

// Set culture
System.Globalization.CultureInfo.CurrentUICulture = new System.Globalization.CultureInfo("fr-FR");
```

## Localize Resource Strings

Provide localized strings for DataGrid UI elements:

```csharp
// Create resource files
// - Resources.resx (default)
// - Resources.fr.resx (French)
// - Resources.de.resx (German)
```

## RTL Support

Enable right-to-left layout:

```csharp
dataGrid.FlowDirection = FlowDirection.RightToLeft;
```

## Localize Data Format

```csharp
dataGrid.Columns.Add(new DataGridNumericColumn
{
    MappingName = "Price",
    Format = "C", // Currency format respects current culture
    CultureInfo = new CultureInfo("de-DE") // German currency format
});

dataGrid.Columns.Add(new DataGridDateColumn
{
    MappingName = "OrderDate",
    Format = "d", // Short date format respects current culture
    CultureInfo = new CultureInfo("fr-FR") // French date format
});
```

## Common Cultures

- `en-US` - English (United States)
- `en-GB` - English (United Kingdom)
- `fr-FR` - French (France)
- `de-DE` - German (Germany)
- `es-ES` - Spanish (Spain)
- `ja-JP` - Japanese (Japan)
- `zh-CN` - Chinese (Simplified)
- `ar-SA` - Arabic (Saudi Arabia)


## Next Steps

- Read [getting-started.md](getting-started.md) for setup
- Read [columns.md](columns.md) for column formatting
