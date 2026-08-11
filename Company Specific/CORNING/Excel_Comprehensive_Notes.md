# Microsoft Excel — Comprehensive Study Notes
### For Corning OA Prep | JD3 (SAP Finance Data)
> Covers: Excel fundamentals, formulas, functions, PivotTables, Power Query, Power Pivot, DAX, VBA basics, data analysis best practices.

---

## 1. Excel Fundamentals

### 1.1 The Excel Interface
- **Workbook:** The entire Excel file (.xlsx, .xlsm, .xlsb)
- **Worksheet (Sheet):** Individual tabs within a workbook
- **Cell:** Intersection of a row and column (e.g., B3 = column B, row 3)
- **Range:** A group of cells (e.g., A1:D10, or non-contiguous A1,C3,E5)
- **Name Box:** Shows current cell address (top-left); used to navigate or name ranges
- **Formula Bar:** Shows cell content / formula
- **Ribbon:** Tabbed toolbar (Home, Insert, Formulas, Data, Review, View)

### 1.2 Cell References — Critical Concept
**Relative Reference (A1):** Adjusts automatically when copied to another cell.
- `=A1+B1` copied one row down becomes `=A2+B2`

**Absolute Reference ($A$1):** Locked — never changes when copied.
- `=$A$1+B1` copied down: `=$A$1+B2` (A1 stays fixed, B1 moves)

**Mixed Reference:**
- `$A1` — column locked, row adjusts
- `A$1` — row locked, column adjusts

**When to use absolute refs:** Tax rate in one cell used across many formulas, conversion factor, lookup table location.

**Shortcut:** F4 cycles through reference types when editing a formula.

### 1.3 Data Types
- **Numbers:** Right-aligned by default; used in calculations
- **Text:** Left-aligned; cannot be summed (even if it looks like a number)
- **Dates:** Stored as serial numbers (Jan 1, 1900 = 1); formatted to display as dates
- **Booleans:** TRUE / FALSE
- **Errors:** #N/A, #REF!, #DIV/0!, #VALUE!, #NAME?, #NUM!, #NULL!

### 1.4 Key Shortcuts
| Action | Shortcut |
|---|---|
| Select entire column | Ctrl + Space |
| Select entire row | Shift + Space |
| Go to last used cell | Ctrl + End |
| Fill down | Ctrl + D |
| Fill right | Ctrl + R |
| Enter formula in array | Ctrl + Shift + Enter (old) |
| AutoFilter toggle | Ctrl + Shift + L |
| Format cells dialog | Ctrl + 1 |
| Paste special | Ctrl + Alt + V |
| Name a range | Ctrl + F3 or Name Box |
| Evaluate formula step-by-step | F9 (in formula bar) |
| Absolute/relative reference toggle | F4 |
| Find & Replace | Ctrl + H |
| Go To Special | Ctrl + G → Special |

---

## 2. Essential Functions & Formulas

### 2.1 Math & Statistical
| Function | Syntax | Description |
|---|---|---|
| `SUM` | `=SUM(A1:A10)` | Adds all values |
| `SUMIF` | `=SUMIF(range, criteria, sum_range)` | Sum where condition met |
| `SUMIFS` | `=SUMIFS(sum_range, range1, crit1, range2, crit2)` | Sum with multiple conditions |
| `AVERAGE` | `=AVERAGE(A1:A10)` | Arithmetic mean |
| `AVERAGEIF` | `=AVERAGEIF(range, criteria, avg_range)` | Conditional average |
| `COUNT` | `=COUNT(A1:A10)` | Count numeric cells only |
| `COUNTA` | `=COUNTA(A1:A10)` | Count non-empty cells (any type) |
| `COUNTBLANK` | `=COUNTBLANK(A1:A10)` | Count empty cells |
| `COUNTIF` | `=COUNTIF(range, criteria)` | Count where condition met |
| `COUNTIFS` | `=COUNTIFS(range1, crit1, range2, crit2)` | Count with multiple conditions |
| `MAX` / `MIN` | `=MAX(A1:A10)` | Largest / smallest value |
| `LARGE` / `SMALL` | `=LARGE(array, k)` | kth largest/smallest value |
| `ROUND` | `=ROUND(number, digits)` | Round to n decimal places |
| `ROUNDUP` / `ROUNDDOWN` | `=ROUNDUP(number, digits)` | Always up / always down |
| `INT` | `=INT(4.9)` → 4 | Round down to integer |
| `MOD` | `=MOD(10, 3)` → 1 | Remainder after division |
| `ABS` | `=ABS(-5)` → 5 | Absolute value |
| `RAND` | `=RAND()` | Random number 0–1 |
| `RANDBETWEEN` | `=RANDBETWEEN(1,100)` | Random integer in range |

### 2.2 Lookup & Reference Functions

#### VLOOKUP (Classic — mostly replaced by XLOOKUP)
```
=VLOOKUP(lookup_value, table_array, col_index_num, [range_lookup])
```
- `lookup_value`: What to search for
- `table_array`: Where to search (first column must contain lookup values)
- `col_index_num`: Which column to return (1 = first column of table)
- `range_lookup`: FALSE = exact match (almost always use FALSE), TRUE = approximate match (sorted data)

**Limitations:** Can only look right; breaks if columns inserted; slower on large data.

#### HLOOKUP
Same as VLOOKUP but searches horizontally across a row.

#### INDEX + MATCH (More Powerful)
```
=INDEX(return_range, MATCH(lookup_value, lookup_range, 0))
```
- `MATCH` returns the **position** of the lookup value
- `INDEX` returns the value at that position in the return range
- Can look left, right, up, down — no column number needed
- Doesn't break when columns are inserted

**Example:**
```
=INDEX(C2:C100, MATCH("Widget A", A2:A100, 0))
```
Returns the value from column C where column A = "Widget A"

#### XLOOKUP (Modern — Excel 365/2019+)
```
=XLOOKUP(lookup_value, lookup_array, return_array, [if_not_found], [match_mode], [search_mode])
```
- Replaces both VLOOKUP and HLOOKUP
- Can search in any direction (left/right/up/down)
- Built-in not-found handling (4th argument)
- Returns arrays (can return multiple columns)
- Default = exact match (no need to specify FALSE)

#### MATCH
```
=MATCH(lookup_value, lookup_array, match_type)
```
- match_type: 0 = exact, 1 = less than (sorted ascending), -1 = greater than (sorted descending)
- Returns position number (not value)

#### INDEX
```
=INDEX(array, row_num, [col_num])
```
- Returns the value at a specific row/column position within a range

#### OFFSET
```
=OFFSET(reference, rows, cols, [height], [width])
```
- Returns a range offset from a starting cell
- Useful for dynamic ranges (e.g., data that grows)
- **Volatile** — recalculates every time anything in the workbook changes (use sparingly)

#### INDIRECT
```
=INDIRECT(ref_text)
```
- Returns a reference from a text string
- `=INDIRECT("Sheet2!A1")` — reference built as text
- Useful for referencing sheets whose names are in cells

#### CHOOSE
```
=CHOOSE(index_num, value1, value2, ...)
```
- Returns a value from a list based on index number
- `=CHOOSE(2, "Jan", "Feb", "Mar")` → "Feb"

### 2.3 Logical Functions

#### IF
```
=IF(logical_test, value_if_true, value_if_false)
```
**Nested IF:**
```
=IF(A1>90,"A", IF(A1>80,"B", IF(A1>70,"C","D")))
```

#### IFS (Excel 2016+)
```
=IFS(condition1, value1, condition2, value2, ..., TRUE, default)
```
Cleaner alternative to nested IFs.

#### IFERROR
```
=IFERROR(value, value_if_error)
```
- `=IFERROR(VLOOKUP(...), "Not Found")` — shows "Not Found" instead of #N/A

#### IFNA
```
=IFNA(value, value_if_na)
```
- Only catches #N/A errors (more specific than IFERROR)

#### AND / OR / NOT
```
=AND(condition1, condition2)   → TRUE only if ALL are true
=OR(condition1, condition2)    → TRUE if ANY is true
=NOT(condition)                → reverses TRUE/FALSE
```
**Combined with IF:**
```
=IF(AND(A1>100, B1="Active"), "Flag", "OK")
```

#### SWITCH
```
=SWITCH(expression, value1, result1, value2, result2, ..., default)
```
Like a CASE statement — cleaner than nested IFs for multiple exact-match conditions.

### 2.4 Text Functions
| Function | Example | Result |
|---|---|---|
| `LEFT` | `=LEFT("Hello",3)` | "Hel" |
| `RIGHT` | `=RIGHT("Hello",2)` | "lo" |
| `MID` | `=MID("Hello",2,3)` | "ell" |
| `LEN` | `=LEN("Hello")` | 5 |
| `FIND` | `=FIND("l","Hello")` | 3 (case-sensitive) |
| `SEARCH` | `=SEARCH("l","Hello")` | 3 (case-insensitive) |
| `SUBSTITUTE` | `=SUBSTITUTE("Hello","l","r")` | "Herro" |
| `REPLACE` | `=REPLACE("Hello",2,3,"i")` | "Hio" |
| `TRIM` | `=TRIM(" Hello ")` | "Hello" |
| `CLEAN` | `=CLEAN(text)` | Removes non-printable characters |
| `UPPER/LOWER/PROPER` | `=PROPER("hello world")` | "Hello World" |
| `CONCATENATE` | `=CONCATENATE(A1," ",B1)` | Joins text |
| `&` operator | `=A1&" "&B1` | Same as CONCATENATE |
| `TEXTJOIN` | `=TEXTJOIN(", ",TRUE,A1:A5)` | Joins with delimiter, ignores blanks |
| `TEXT` | `=TEXT(TODAY(),"DD-MMM-YYYY")` | "08-Aug-2026" |
| `VALUE` | `=VALUE("123")` | 123 (text to number) |
| `NUMBERVALUE` | `=NUMBERVALUE("1,234.5",",",".")` | Locale-aware text to number |

### 2.5 Date & Time Functions
| Function | Description |
|---|---|
| `TODAY()` | Current date (no time) |
| `NOW()` | Current date and time |
| `DATE(year,month,day)` | Build a date |
| `YEAR/MONTH/DAY(date)` | Extract components |
| `DATEDIF(start,end,unit)` | Difference in "Y","M","D","MD","YM","YD" |
| `EDATE(date,months)` | Add/subtract months |
| `EOMONTH(date,months)` | Last day of month |
| `WORKDAY(date,days)` | Add working days (excl. weekends) |
| `NETWORKDAYS(start,end)` | Count working days between dates |
| `WEEKDAY(date,2)` | Day of week (2 = Mon=1,Sun=7) |

### 2.6 Dynamic Array Functions (Excel 365)
These functions **spill** results into adjacent cells automatically.

| Function | Description |
|---|---|
| `FILTER(array, include, [if_empty])` | Returns rows matching condition |
| `SORT(array, [sort_index], [order])` | Sorts a range |
| `SORTBY(array, by_array, [order])` | Sort by another array |
| `UNIQUE(array, [by_col], [exactly_once])` | Unique values from a range |
| `SEQUENCE(rows, [cols], [start], [step])` | Generate number sequence |
| `RANDARRAY(rows, cols, min, max)` | Array of random numbers |

**Example — dynamic filtered list:**
```
=FILTER(A2:C100, B2:B100="Active", "No results")
```
Returns all rows where column B = "Active" — updates automatically as data changes.

### 2.7 Information Functions
| Function | Description |
|---|---|
| `ISNUMBER(value)` | TRUE if numeric |
| `ISTEXT(value)` | TRUE if text |
| `ISBLANK(value)` | TRUE if empty |
| `ISERROR(value)` | TRUE if any error |
| `ISNA(value)` | TRUE if #N/A |
| `TYPE(value)` | Returns 1=number,2=text,4=logical,16=error |
| `CELL("type",ref)` | Cell info (type, format, address) |

---

## 3. PivotTables

### 3.1 What is a PivotTable?
An interactive summary tool that aggregates, groups, and rearranges large datasets dynamically — without formulas. The most powerful Excel feature for data analysis.

**Creating a PivotTable:**
1. Click inside your data range (must have headers)
2. Insert → PivotTable
3. Choose: New Worksheet or Existing Worksheet
4. Use the Field List (right panel) to build your analysis

### 3.2 PivotTable Areas
- **Rows:** Dimension to display as row labels (e.g., Product, Region, Month)
- **Columns:** Dimension displayed as column headers (e.g., Year, Quarter)
- **Values:** What to aggregate (e.g., Sum of Revenue, Count of Orders, Average of Price)
- **Filters:** Field to filter the entire PivotTable (drop-down at top)

**Multiple fields in Rows:** Creates a hierarchy (e.g., Region → City → Store)

### 3.3 Value Field Settings
Right-click on a value → Value Field Settings:
- **Summarize by:** Sum, Count, Average, Max, Min, Product, Count Numbers, StdDev, Var
- **Show Values As:** % of Grand Total, % of Row Total, % of Column Total, Difference From, Running Total, Rank

### 3.4 Grouping Data
Right-click on a date field → Group:
- Group dates by: Days, Months, Quarters, Years (multiple can be selected)
- Group numbers: define bin size (e.g., 0–100, 100–200)
- Group text: manually select items → Group

### 3.5 Calculated Fields & Calculated Items
**Calculated Field:** Add a new field that computes from existing value fields.
- PivotTable Analyze → Fields, Items & Sets → Calculated Field
- Example: `= Revenue / Units` creates an "Average Price" field

**Calculated Item:** Add a new item within a row/column field.
- Example: Add "North+South" as a calculated item in Region field

### 3.6 Slicers and Timelines
**Slicer:** Visual, clickable filter buttons for a field.
- Insert → Slicer → Select field(s)
- Click buttons to filter; Ctrl+click for multi-select
- Can be connected to multiple PivotTables (right-click → Report Connections)

**Timeline:** Visual date filter (slider) specifically for date fields.
- Insert → Timeline
- Filter by: Days, Months, Quarters, Years

### 3.7 PivotChart
- Chart directly linked to a PivotTable — updates dynamically
- Insert → PivotChart (from PivotTable) or create together
- Has its own filter dropdowns

### 3.8 GETPIVOTDATA
When you click a PivotTable cell in a formula, Excel auto-generates:
```
=GETPIVOTDATA("Revenue", $A$3, "Region", "North", "Year", 2025)
```
- Fetches specific data from PivotTable by field names and values
- Robust — survives PivotTable layout changes (unlike direct cell references)
- Disable auto-generation: PivotTable Analyze → Options → uncheck "Generate GETPIVOTDATA"

### 3.9 PivotTable Best Practices
- Source data must be in **tabular format** (one row per record, no merged cells, no blank rows/columns)
- Use **Table** (Ctrl+T) as data source → PivotTable auto-expands when data added
- **Refresh:** Right-click → Refresh (or Data → Refresh All) after source data changes
- Use **Value Field Settings → Show Values As** for % calculations instead of helper columns
- Name your PivotTables (PivotTable Analyze → PivotTable Name) for GETPIVOTDATA formulas

---

## 4. Power Query (Get & Transform Data)

### 4.1 What is Power Query?
Power Query is Excel's built-in **ETL (Extract, Transform, Load) tool** — connect to data sources, clean and reshape data, and load it into Excel or the Data Model. Available in Excel 2016+ (Data → Get & Transform Data).

**Key advantage:** All transformation steps are recorded and replayable — refresh updates data automatically.

### 4.2 Data Sources Power Query Connects To
- Excel files (current workbook, other files)
- CSV / Text files
- Web pages (HTML tables)
- SQL databases (SQL Server, MySQL, PostgreSQL, Oracle)
- SharePoint lists and folders
- Azure (Blob Storage, SQL, Databricks)
- OData feeds (SAP, Dynamics)
- JSON / XML files
- Folder (combine multiple files — very powerful for monthly file consolidation)

### 4.3 Power Query Editor Interface
- **Queries pane (left):** All your queries listed
- **Preview (center):** Data preview — what your data looks like after transformations
- **Applied Steps (right):** Every transformation recorded as a step — can click any step to see data at that point, delete steps, reorder

### 4.4 Common Transformations

**Column operations:**
- Rename column (double-click header)
- Change data type (click type icon in header: ABC=text, 123=number, calendar=date)
- Remove columns (right-click → Remove)
- Split column (by delimiter, by number of characters)
- Add column (from existing: `= [Price] * [Qty]`)
- Extract: text before/after delimiter, first N characters, last N characters

**Row operations:**
- Filter rows (dropdown arrow on each column)
- Remove duplicates
- Remove blank rows
- Keep/remove top N rows
- Sort rows

**Data reshaping:**
- **Unpivot:** Convert wide format (months as columns) to tall format (one row per month) — essential for PivotTable-ready data
- **Pivot:** Convert tall to wide (opposite of unpivot)
- **Transpose:** Flip rows and columns
- **Group By:** Aggregate data (Sum, Count, Average by group — like SQL GROUP BY)

**Combining data:**
- **Merge Queries:** Like SQL JOIN — combine two tables on a matching column (Left Outer, Inner, Full Outer, etc.)
- **Append Queries:** Stack tables vertically (like SQL UNION ALL) — great for combining monthly sheets/files

**Text cleaning:**
- Trim (remove leading/trailing spaces)
- Clean (remove non-printable characters)
- Uppercase/Lowercase/Proper Case
- Replace values
- Remove specific characters

### 4.5 M Language (Power Query Formula Language)
Each step in Power Query generates M code behind the scenes.
- View/edit via: View → Advanced Editor
- M is functional and case-sensitive
- Each step references the previous step

Example:
```m
let
    Source = Excel.CurrentWorkbook(){[Name="SalesTable"]}[Content],
    #"Changed Type" = Table.TransformColumnTypes(Source,{{"Date", type date}, {"Revenue", type number}}),
    #"Filtered Rows" = Table.SelectRows(#"Changed Type", each [Region] = "North"),
    #"Grouped Rows" = Table.Group(#"Filtered Rows", {"Region"}, {{"Total Revenue", each List.Sum([Revenue]), type number}})
in
    #"Grouped Rows"
```

### 4.6 Loading Data
After transformations, choose how to load:
- **Table:** Load to Excel worksheet as a formatted table
- **PivotTable Report:** Load directly to PivotTable
- **Only Create Connection:** Don't load to sheet — use in Data Model only
- **Add to Data Model:** Load to Power Pivot for multi-table analysis

### 4.7 Power Query Best Practices
- Never load raw data to a worksheet when it needs cleanup — always use Power Query
- Use **"Close & Load To..."** → Only Create Connection + Add to Data Model for large datasets
- Use Folder connector to combine monthly files automatically
- Avoid using Row Number as a transformation — it changes on refresh
- Set data types explicitly (don't rely on auto-detection)
- Refresh: Data → Refresh All (or set automatic refresh on open: Query Properties → Refresh Every X minutes)

---

## 5. Power Pivot & Data Model

### 5.1 What is Power Pivot?
Power Pivot is an Excel add-in providing an **in-memory data model** that:
- Handles millions of rows (far beyond Excel's 1,048,576 row limit per sheet)
- Relates multiple tables (like a database)
- Supports DAX (Data Analysis Expressions) for calculated measures

**Enable:** File → Options → Add-ins → COM Add-ins → Microsoft Office Power Pivot

### 5.2 Data Model Relationships
Define how tables relate:
- One-to-many (e.g., one Customer → many Orders)
- Primary key (unique) in one table → Foreign key in another
- Create via: Power Pivot → Diagram View → drag fields to connect
- Or: Data → Relationships

Once related, PivotTables can pull from multiple tables without VLOOKUP.

### 5.3 DAX (Data Analysis Expressions)

**DAX is the formula language for Power Pivot, Power BI, and SSAS Tabular.**

#### DAX Calculated Column vs. Measure
**Calculated Column:** Computed row-by-row at data load; stored in the data model.
```dax
Profit = Sales[Revenue] - Sales[Cost]
```

**Measure:** Dynamic aggregation computed at query time based on filter context.
```dax
Total Revenue = SUM(Sales[Revenue])
```
**Always use measures for aggregations** — they respond to PivotTable filters correctly.

#### Key DAX Functions

**Aggregation:**
```dax
Total Sales = SUM(Sales[Revenue])
Avg Price = AVERAGE(Sales[Price])
Order Count = COUNTROWS(Sales)
Distinct Customers = DISTINCTCOUNT(Sales[CustomerID])
```

**CALCULATE — The Most Important DAX Function:**
Evaluates an expression in a **modified filter context**.
```dax
North Sales = CALCULATE(SUM(Sales[Revenue]), Sales[Region] = "North")
Prior Year Sales = CALCULATE(SUM(Sales[Revenue]), SAMEPERIODLASTYEAR(Calendar[Date]))
All Products Total = CALCULATE(SUM(Sales[Revenue]), ALL(Products))
```
`CALCULATE(expression, filter1, filter2, ...)` — removes existing filters and applies new ones.

**Time Intelligence:**
```dax
YTD Sales = TOTALYTD(SUM(Sales[Revenue]), Calendar[Date])
MTD Sales = TOTALMTD(SUM(Sales[Revenue]), Calendar[Date])
Prior Month = CALCULATE(SUM(Sales[Revenue]), PREVIOUSMONTH(Calendar[Date]))
YoY Growth = DIVIDE([Total Sales] - [Prior Year Sales], [Prior Year Sales])
```
Requires a proper Calendar/Date table with every date marked as date table.

**Filter Functions:**
```dax
ALL(table/column)     -- removes all filters
ALLEXCEPT(table, col) -- removes all filters except specified
FILTER(table, condition) -- returns filtered table
RELATED(column)       -- access related table column (like VLOOKUP)
RELATEDTABLE(table)   -- access related table rows
```

**DIVIDE:** Safe division (handles divide-by-zero):
```dax
Profit Margin = DIVIDE([Profit], [Revenue], 0)
```

**Logical:**
```dax
IF(condition, true_result, false_result)
SWITCH(expression, val1, res1, val2, res2, BLANK())
```

**Text:**
```dax
Full Name = CONCATENATE(Employee[FirstName], " " & Employee[LastName])
```

**BLANK() vs. 0:** In DAX, BLANK() is preferred over 0 for missing values — it doesn't appear in charts or summaries.

---

## 6. Data Validation & Protection

### 6.1 Data Validation
Restrict what users can enter in a cell:
- Data → Data Validation
- Allow: Whole number, Decimal, List (dropdown), Date, Time, Text Length, Custom (formula)

**Dropdown list:**
- Source: `=A1:A10` (range) or type values separated by commas
- Input message: shown when cell selected
- Error alert: shown when invalid data entered (Stop, Warning, Information)

**Custom validation with formula:**
- Allow only dates in the future: `=A1>TODAY()`
- Allow only unique values: `=COUNTIF($A$1:$A$100,A1)=1`

### 6.2 Conditional Formatting
Apply formatting rules based on cell values:
- Home → Conditional Formatting
- Highlight Cell Rules: greater than, less than, between, equal to, duplicate values
- Top/Bottom Rules: top 10%, bottom 10 items
- Data Bars, Color Scales, Icon Sets — visual indicators
- **New Rule → Use a formula:** Most powerful — `=$B2="Active"` highlights entire row

### 6.3 Sheet & Workbook Protection
- **Protect Sheet:** Prevent editing (can allow specific actions: sort, filter, select locked cells)
- **Protect Workbook:** Prevent adding/deleting sheets, moving sheets
- **Password:** Optional; without password, protection is easily removed
- **Lock specific cells:** Format Cells → Protection → uncheck Locked for editable cells, then protect sheet

---

## 7. Data Analysis Tools

### 7.1 Sorting & Filtering
**AutoFilter:** Ctrl+Shift+L — dropdown arrows on headers
- Filter by value, color, text/number conditions
- Multiple column filters work as AND (must satisfy all)

**Custom Sort:** Data → Sort — multiple levels (Sort by Region, then by Revenue descending)

**Advanced Filter:** Filter in-place or copy to another location using criteria range — supports complex OR conditions.

### 7.2 Subtotals
Data → Subtotals — insert SUM/AVERAGE/COUNT subtotals at group changes:
- Data must be sorted by the grouping column first
- Creates outline levels (collapse/expand with 1,2,3 buttons on left)
- **SUBTOTAL function:** `=SUBTOTAL(9,A1:A100)` — function_num 9 = SUM; respects visible rows only (ignores filtered-out rows). Unlike SUM which includes hidden rows.

### 7.3 What-If Analysis
**Goal Seek:** Find input value to achieve desired output.
- Data → What-If Analysis → Goal Seek
- "Set cell B10 to value 100,000 by changing cell B2"

**Scenario Manager:** Save multiple sets of input assumptions and switch between them.

**Data Table:** Sensitivity analysis — how output changes with one or two input variables.
- One-variable: vary one input, see effect on multiple outputs
- Two-variable: vary two inputs, see effect on one output

### 7.4 Solver
Advanced optimization (requires add-in):
- Maximize/minimize an objective cell by changing variable cells subject to constraints
- Used for linear programming, resource allocation, portfolio optimization

### 7.5 Statistical Analysis (Analysis ToolPak)
Enable: File → Options → Add-ins → Analysis ToolPak
- Descriptive Statistics: mean, median, mode, std dev, range
- Regression Analysis
- Histogram
- Correlation Matrix
- Moving Average
- Sampling

---

## 8. VBA Basics (Visual Basic for Applications)

### 8.1 What is VBA?
VBA is Excel's built-in programming language for automating tasks. Access via:
- Developer tab → Visual Basic (or Alt+F11)
- Developer tab → Macros → Record Macro (generates VBA automatically)

### 8.2 VBA Structure
```vba
Sub MacroName()
    ' This is a comment
    ' Your code here
    MsgBox "Hello World"
End Sub

Function FunctionName(arg1 As String) As String
    FunctionName = UCase(arg1)
End Function
```

### 8.3 Common VBA Operations
```vba
' Reference a cell
Range("A1").Value = "Hello"
Cells(1, 1).Value = "Hello"    ' Row 1, Column 1

' Loop through rows
Dim i As Integer
For i = 1 To 100
    If Cells(i, 1).Value > 1000 Then
        Cells(i, 2).Value = "High"
    End If
Next i

' Loop through a range
Dim cell As Range
For Each cell In Range("A1:A100")
    cell.Value = cell.Value * 1.1
Next cell

' Work with sheets
Sheets("Sheet1").Select
Sheets("Sheet2").Range("A1").Value = "Data"

' Copy/paste
Range("A1:D10").Copy Destination:=Sheets("Sheet2").Range("A1")

' Last row
Dim lastRow As Long
lastRow = Cells(Rows.Count, 1).End(xlUp).Row

' Show message / input
MsgBox "Process complete!"
Dim userInput As String
userInput = InputBox("Enter value:")

' Error handling
On Error GoTo ErrorHandler
    ' risky code
    Exit Sub
ErrorHandler:
    MsgBox "Error: " & Err.Description
```

### 8.4 When to Use VBA vs. Power Query vs. Formulas
- **Formulas:** Calculations, lookups, conditional logic — always prefer formulas for readability
- **Power Query:** Data import, cleaning, reshaping, combining sources — repeatable ETL
- **VBA:** UI automation (button clicks, form submission), complex loops, interactions between apps (Excel → Outlook), tasks Power Query/formulas can't do
- **Avoid VBA when:** Power Query or a formula can do the same thing — VBA is harder to maintain

---

## 9. Excel Best Practices for Finance & Data Analysis

### 9.1 Spreadsheet Design Principles
- **Separate inputs, calculations, and outputs** into distinct areas or sheets
- **Never hardcode values** inside formulas — put them in labeled input cells
- **Color-code:** Blue = inputs (user enters), Black = formulas (don't touch), Green = outputs
- **Document assumptions** with comments or a separate "Assumptions" sheet
- **Use named ranges** for important inputs (makes formulas readable: `=Revenue * TaxRate` instead of `=B5 * B12`)
- **One row, one record** — never merge cells in data areas

### 9.2 Avoiding Common Excel Errors
| Error | Cause | Fix |
|---|---|---|
| `#N/A` | VLOOKUP can't find value | Use IFERROR; check data types match |
| `#REF!` | Reference to deleted cells | Rebuild formula; avoid deleting referenced cells |
| `#DIV/0!` | Division by zero | Use `=IFERROR(A/B,0)` or `=IF(B=0,0,A/B)` |
| `#VALUE!` | Wrong data type | Check text vs. number; use VALUE() to convert |
| `#NAME?` | Misspelled function name | Check spelling; ensure add-ins enabled |
| `#NUM!` | Invalid numeric value | Check for negative square roots, impossible iterations |

### 9.3 Performance Optimization
- Use **XLOOKUP/INDEX-MATCH over VLOOKUP** on large datasets
- Avoid **volatile functions** (OFFSET, INDIRECT, NOW, TODAY, RAND) in large models — they recalculate constantly
- Use **Power Query** instead of loading millions of rows to a sheet
- Convert ranges to **Tables** (Ctrl+T) — structured references, auto-expansion
- Turn off **automatic calculation** (Formulas → Calculation Options → Manual) during heavy data entry — press F9 to calculate manually

### 9.4 Excel Tables (Ctrl+T)
Structured tables offer major advantages:
- **Structured references:** `=SalesTable[Revenue]` instead of `=$C$2:$C$100`
- **Auto-expand:** New rows automatically included in formulas, PivotTables
- **Total row:** Built-in summary row with function dropdown
- **Filtering/sorting:** Built in
- **Power Query integration:** Tables are ideal data sources

---

## 10. Excel in Finance Context (Relevant to JD3)

### 10.1 Common Finance Uses
- **Financial models:** DCF, budget vs. actual, sensitivity analysis
- **Reconciliation templates:** Side-by-side comparison with variance column
- **Aging reports:** VLOOKUP/XLOOKUP + DATEDIF for overdue analysis
- **Month-end trackers:** COUNTIFS/SUMIFS to monitor close checklist status
- **Data extraction from SAP:** Download SAP reports as .xlsx → clean with Power Query → analyze with PivotTables

### 10.2 Useful Finance Formulas
**Aging buckets (days overdue):**
```
=IF(D2="","Not Due", IF(TODAY()-D2<=30,"0-30 days", IF(TODAY()-D2<=60,"31-60 days","60+ days")))
```

**Year-over-Year variance:**
```
=IFERROR((C2-B2)/ABS(B2), "N/A")   ' % change; ABS handles negative base
```

**Running total:**
```
=SUM($B$2:B2)   ' Absolute start, relative end — copy down for running total
```

**SUMPRODUCT for weighted average / complex aggregation without helper columns:**
```
=SUMPRODUCT((Region="North")*(Revenue))   ' Sum Revenue where Region=North (no array entry)
=SUMPRODUCT(Units*Price)/SUM(Units)       ' Weighted average price
```

---

## 11. Key Terms Quick Reference

| Term | Meaning |
|---|---|
| Array Formula | Formula that operates on arrays; old = Ctrl+Shift+Enter; new = dynamic arrays spill automatically |
| DAX | Data Analysis Expressions — formula language for Power Pivot and Power BI |
| Dynamic Array | Excel 365 feature where formulas spill results into multiple cells |
| M Language | Power Query's formula language |
| Named Range | User-defined label for a cell or range |
| Power Pivot | Add-in for multi-table data models with DAX; handles millions of rows |
| Power Query | Built-in ETL tool for connecting, cleaning, and transforming data |
| Slicer | Visual filter control for PivotTables and Tables |
| Structured Reference | Table column reference like `Table[Column]` instead of `A2:A100` |
| SUBTOTAL | Function that operates only on visible (filtered) rows |
| Volatile Function | Recalculates on every workbook change (OFFSET, INDIRECT, RAND, NOW, TODAY) |
| VBA | Visual Basic for Applications — Excel's automation/macro programming language |

---
*End of Excel Comprehensive Notes — ~680 lines.*
*Read alongside: Shared_Excel.md (MCQs) for practice questions.*
