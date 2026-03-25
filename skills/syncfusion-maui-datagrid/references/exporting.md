# Exporting

## Table of Contents
- [Export to Excel](#export-to-excel)
- [Export to PDF](#export-to-pdf)
- [Clipboard Operations](#clipboard-operations)

## Export to Excel

Add `Syncfusion.Maui.DataGridExport` package.

### Basic Excel Export

```csharp
using Syncfusion.Maui.DataGrid.Exporting;

DataGridExcelExportingController excelExport = new DataGridExcelExportingController();
DataGridExcelExportingOption option = new DataGridExcelExportingOption();
var excelEngine = excelExport.ExportToExcel(this.datagrid, option);            
var workbook = excelEngine.Excel.Workbooks[0];
MemoryStream stream = new MemoryStream();
workbook.SaveAs(stream);
workbook.Close();
excelEngine.Dispose();
string OutputFilename = "ExportFeature.xlsx";
SaveService saveService = new();
saveService.SaveAndView(OutputFilename, "application/vnd.openxmlformats-officedocument.spreadsheetml.sheet", stream);
```

### Export Options

```csharp
var option = new DataGridExcelExportingOption
{
    CanAllowOutlining = true,
    CanApplyGridStyle = true,
    CanExportColumnWidth = true,
    CanExportGroupSummary = true,
    CanExportGroups = true,
    CanExportHeader = true,
    CanExportStackedHeaders = true,
    CanExportTableSummary = true,
    CanExportUnboundRows = true,
    DefaultColumnWidth = 120,
    DefaultRowHeight = 50,
    ExportMode = ExportMode.Text,
    StartColumnIndex = 3,
    StartRowIndex = 2
};

var excelEngine = excelExport.ExportToExcel(this.datagrid, option);          
```

### Row Exporting

```csharp

DataGridExcelExportingController excelExport = new DataGridExcelExportingController();
excelExport.RowExporting += ExcelExport_RowExporting;
private void ExcelExport_RowExporting(object sender, DataGridRowExcelExportingEventArgs e)
{
    if (!(e.Record.Data is OrderInfo))
        return;

    if (e.RowType == ExportRowType.Record)
    {
        e.Range.CellStyle.ColorIndex = Syncfusion.XlsIO.ExcelKnownColors.Aqua;
    }
}
```

## Export to PDF

Add `Syncfusion.Maui.DataGridExport` package.

### Basic PDF Export

```csharp
using Syncfusion.Maui.DataGrid.Exporting;
using Syncfusion.Pdf;

    MemoryStream stream = new MemoryStream();
    DataGridPdfExportingController pdfExport = new DataGridPdfExportingController();
    DataGridPdfExportingOption option = new DataGridPdfExportingOption();
    var pdfDoc = new PdfDocument();
    pdfDoc = pdfExport.ExportToPdf(this.dataGrid, option);
    pdfDoc.Save(stream);
    pdfDoc.Close(true);
    SaveService saveService = new();
    saveService.SaveAndView("ExportFeature.pdf", "application/pdf", stream);
```

### PDF Export Options

```csharp
var option = new DataGridPdfExportingOption
{
    CanExportColumnWidth = true,
    CanExportRowHeight = true,
    CanExportAllPages = true,
    CanExportTableSummary = true,
    CanExportGroupSummary = true,
    CanFitAllColumnsInOnePage = false,
    StartPageIndex = 1,
    CanRepeatHeaders = true,
    CanExportGroups = true,
    CanExportHeader = false,
    CanExportStackedHeaders = true,
    CanExportUnboundRows = true,
    CanApplyGridStyle = true
};
```

## Clipboard Operations

### Copy to Clipboard

```csharp
// Copy selected rows
dataGrid.CopyOption = DataGridCopyOption.CopyData | DataGridCopyOption.IncludeHeaders;

// Copy specific selection
```

### Copy Options

```csharp
dataGrid.CopyOption = DataGridCopyOption.IncludeHeaders;
dataGrid.CopyOption  = DataGridCopyOption.CopyData;
dataGrid.CopyOption  = DataGridCopyOption.IncludeFormat;
dataGrid.CopyOption  = DataGridCopyOption.IncludeHiddenColumn;
dataGrid.CopyOption  = DataGridCopyOption.None;
dataGrid.CopyOption  = DataGridCopyOption.CutData;
```

### Paste from Clipboard

```csharp
dataGrid.PasteOption = DataGridPasteOption.PasteData | DataGridPasteOption.ExcludeFirstLine;
```

### Paste Options

```csharp
dataGrid.PasteOption = DataGridPasteOption.None ;
dataGrid.PasteOption = DataGridPasteOption.PasteData ;
dataGrid.PasteOption = DataGridPasteOption.ExcludeFirstLine;
dataGrid.PasteOption = DataGridPasteOption.IncludeHiddenColumn;
```

### Events

#### CopyContent

```csharp
this.dataGrid.CopyContent += dataGrid_CopyContent;

void dataGrid_CopyContent(object sender, DataGridCopyPasteEventArgs e)
{
    e.Handled = true;
}
```

#### PasteContent

```csharp
this.dataGrid.PasteContent += dataGrid_PasteContent;

void dataGrid_PasteContent(object sender, DataGridCopyPasteEventArgs e)
{
    e.Handled = true;
}
```

#### CopyCellContent

```csharp
this.dataGrid.CopyCellContent += dataGrid_CopyCellContent;

void dataGrid_CopyCellContent(object sender, DataGridCopyPasteCellEventArgs e)
{
    // e.RowData - Gets the row data of the cell that triggered the event.
    // e.ClipboardValue - Gets or sets the value to be copied to clipboard for the cell that triggered the event.
    // e.Handled - Gets or sets a value indicating whether the copy action is handled for the cell that triggered the event.
    // e.Column - Gets the column associated with the cell that triggered the event.
}
```

#### PasteCellContent

```csharp
this.dataGrid.PasteCellContent += dataGrid_PasteCellContent;

void dataGrid_PasteCellContent(object sender, DataGridCopyPasteCellEventArgs e)
{
    // e.RowData - Gets the row data of the cell that triggered the event.
    // e.ClipboardValue - Gets or sets the value from clipboard for the cell that triggered the event.
    // e.Handled - Gets or sets a value indicating whether the paste action is handled for the cell that triggered the event.
    // e.Column - Gets the column associated with the cell that triggered the event.
}
```

## Next Steps

- Read [advanced-features.md](advanced-features.md) for more features
- Read [performance-events.md](performance-events.md) for optimization
