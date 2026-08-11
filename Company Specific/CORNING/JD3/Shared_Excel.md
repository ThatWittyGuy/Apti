# Shared — Microsoft Excel MCQs
### Applies to: JD3 (SAP Finance Data), any JD mentioning Excel, data analysis, reporting
> 35 Questions | 🟢 Easy | 🟡 Medium | 🔴 Hard

---

**Q1.** 🟢 What does the `SUM` function do in Excel?
- A) Counts the number of cells with values
- B) Adds up all numeric values in a specified range
- C) Returns the highest value in a range
- D) Concatenates text values in a range

**Answer: B**

---

**Q2.** 🟢 What is the correct syntax of an Excel `IF` function?
- A) `=IF(value, condition, true)`
- B) `=IF(logical_test, value_if_true, value_if_false)`
- C) `=IF(true, false, condition)`
- D) `=IF(value_if_true, logical_test, value_if_false)`

**Answer: B**

---

**Q3.** 🟢 What does `VLOOKUP` do?
- A) Looks up a value in a row and returns a result from a specified column above
- B) Searches for a value in the first column of a table and returns a value from a specified column in the same row
- C) Looks up all values matching a criteria in a vertical list
- D) Returns the vertical position (row number) of a value in a range

**Answer: B**

---

**Q4.** 🟢 What is a "PivotTable" in Excel?
- A) A table that rotates data 90 degrees
- B) An interactive tool that summarizes, aggregates, and reorganizes large datasets — allowing users to group, filter, and analyze data by different dimensions without formulas
- C) A table template for financial pivot reports
- D) A linked table that updates from an external database

**Answer: B**

---

**Q5.** 🟢 What does `COUNTIF(range, criteria)` do?
- A) Counts all cells in the range regardless of value
- B) Counts the number of cells in a range that meet a specified condition (e.g., `=COUNTIF(A1:A10, ">100")`)
- C) Sums values that meet a condition
- D) Returns the count of unique values in a range

**Answer: B**

---

**Q6.** 🟢 What is the difference between a relative reference (`A1`) and an absolute reference (`$A$1`) in Excel?
- A) Absolute references update when the sheet is refreshed; relative stay fixed
- B) Relative references adjust automatically when a formula is copied to another cell; absolute references stay fixed regardless of where the formula is copied
- C) They are identical — the `$` sign is just formatting
- D) Absolute references work only in PivotTables

**Answer: B**

---

**Q7.** 🟢 What does `AVERAGE(range)` calculate?
- A) The most frequently occurring value in the range
- B) The arithmetic mean of all numeric values in the range
- C) The middle value when sorted (median)
- D) The sum divided by the count of non-empty cells only

**Answer: B**

---

**Q8.** 🟢 What is the keyboard shortcut to apply a filter in Excel?
- A) `Ctrl + F`
- B) `Ctrl + Shift + L`
- C) `Alt + D + F`
- D) `Ctrl + Alt + F`

**Answer: B**

---

**Q9.** 🟡 What is the difference between `VLOOKUP` and `HLOOKUP`?
- A) VLOOKUP is for text; HLOOKUP is for numbers
- B) VLOOKUP searches vertically (down a column) and returns from a column to the right; HLOOKUP searches horizontally (across a row) and returns from a row below
- C) VLOOKUP is more accurate; HLOOKUP is faster
- D) They are the same — only the syntax differs

**Answer: B**

---

**Q10.** 🟡 What is `INDEX(array, row_num, col_num)` used for?
- A) Returning the index number of a value in a range
- B) Returning the value at a specific row and column intersection within a range — often combined with MATCH for flexible lookups
- C) Indexing a table for faster searching
- D) Returning the position of the first non-empty cell

**Answer: B**

---

**Q11.** 🟡 What does `MATCH(lookup_value, lookup_array, match_type)` return?
- A) The value that matches the lookup criteria
- B) The relative position (row or column number) of a value within a range — used with INDEX for a more flexible alternative to VLOOKUP
- C) TRUE or FALSE depending on whether the value exists
- D) The count of matching values in the range

**Answer: B**

---

**Q12.** 🟡 What is the advantage of `INDEX(MATCH())` over `VLOOKUP`?
- A) INDEX/MATCH is simpler to write
- B) INDEX/MATCH can look up values to the left of the lookup column, handles column insertions without breaking, and is generally faster on large datasets — VLOOKUP can only return values to the right of the lookup column
- C) VLOOKUP handles multiple matches; INDEX/MATCH does not
- D) INDEX/MATCH works only with sorted data

**Answer: B**

---

**Q13.** 🟡 What is `SUMIF(range, criteria, sum_range)` used for?
- A) Summing a range only if the total exceeds a threshold
- B) Summing values in `sum_range` only for rows where `range` meets the `criteria` — e.g., sum all sales where region = "East"
- C) Summing values conditionally across multiple sheets
- D) Returning the sum of the top N values meeting a criteria

**Answer: B**

---

**Q14.** 🟡 What is `SUMIFS` and how does it differ from `SUMIF`?
- A) SUMIFS sums values across multiple sheets; SUMIF sums one sheet
- B) SUMIFS allows multiple criteria across multiple ranges simultaneously; SUMIF handles only one condition — e.g., sum sales where region = "East" AND product = "Widget"
- C) They are identical — SUMIFS is just the newer name
- D) SUMIFS works with text; SUMIF works with numbers

**Answer: B**

---

**Q15.** 🟡 What is `IFERROR(value, value_if_error)` used for?
- A) Checking if a formula contains an error in syntax
- B) Returning a custom value (e.g., 0, "N/A", blank) instead of showing an error message (#N/A, #DIV/0!, etc.) when a formula produces an error — making reports cleaner
- C) Logging formula errors for debugging
- D) Converting error values to text

**Answer: B**

---

**Q16.** 🟡 What is conditional formatting in Excel?
- A) Formatting that only applies when certain software conditions are met
- B) A feature that automatically applies formatting (color, font, icons) to cells based on their value or a formula — used to visually highlight trends, outliers, or status (e.g., red for negative, green for positive)
- C) A way to conditionally apply number formats (date, currency)
- D) Formatting that is applied only when a condition in a different sheet is true

**Answer: B**

---

**Q17.** 🟡 What is a "named range" in Excel and why is it useful?
- A) A range of cells with the sheet name prepended
- B) A user-defined label assigned to a cell or range (e.g., naming B2:B13 "Monthly_Sales") — making formulas more readable, reducing errors, and enabling cross-sheet references without complex address notation
- C) A range automatically named by Excel based on headers
- D) A range of cells containing names (text data)

**Answer: B**

---

**Q18.** 🟡 What does `TEXT(value, format_text)` do in Excel?
- A) Converts text to a number using a format code
- B) Converts a numeric value to text with a specified format (e.g., `=TEXT(TODAY(),"DD-MMM-YYYY")` returns "08-Aug-2026") — useful for concatenating dates or numbers into text strings
- C) Searches for specific text within a cell value
- D) Returns the length of a text string

**Answer: B**

---

**Q19.** 🟡 What is `XLOOKUP` and how does it improve on `VLOOKUP`?
- A) XLOOKUP is an older function; VLOOKUP is more modern
- B) XLOOKUP (Excel 365/2019+) searches any range, returns values from any direction (left or right), handles not-found cases natively, supports wildcard and exact match modes, and returns arrays — making VLOOKUP largely obsolete
- C) XLOOKUP only works with Excel Online; VLOOKUP works in all versions
- D) XLOOKUP is for cross-sheet lookups; VLOOKUP is for single-sheet

**Answer: B**

---

**Q20.** 🟡 What is "Power Query" in Excel?
- A) A premium Excel add-in for advanced statistical analysis
- B) A built-in ETL tool in Excel (and Power BI) that allows users to connect to data sources, transform and clean data, and load it into Excel — without complex formulas or VBA
- C) An Excel feature for running SQL queries on worksheets
- D) A query language for Excel PivotTables

**Answer: B**

---

**Q21.** 🟡 What is a "slicer" in Excel PivotTables?
- A) A tool for splitting a large spreadsheet into smaller files
- B) A visual, interactive filter control that allows users to filter PivotTable (or PivotChart) data by clicking buttons representing dimension values — more user-friendly than dropdown filters
- C) A data validation tool for restricting cell input
- D) A chart type that slices a pie chart into segments

**Answer: B**

---

**Q22.** 🟡 What is the difference between `COUNT`, `COUNTA`, and `COUNTBLANK`?
- A) They are identical functions
- B) `COUNT` counts cells with numbers; `COUNTA` counts all non-empty cells (any type); `COUNTBLANK` counts empty cells
- C) `COUNT` counts all cells; `COUNTA` counts text only; `COUNTBLANK` counts zero values
- D) `COUNTA` is for arrays; `COUNT` is for single ranges

**Answer: B**

---

**Q23.** 🟡 What does `CONCATENATE` (or `&`) do in Excel?
- A) Combines multiple ranges into one range
- B) Joins two or more text strings into one — e.g., `="Hello" & " " & "World"` returns `"Hello World"`; `TEXTJOIN` is a more powerful modern alternative
- C) Counts and aggregates values from multiple cells
- D) Merges duplicate values in a column into one

**Answer: B**

---

**Q24.** 🔴 What is a "dynamic array formula" in Excel 365?
- A) A formula that automatically updates when the spreadsheet is opened
- B) A formula that returns multiple values and automatically "spills" them into adjacent cells — enabled by functions like `FILTER`, `SORT`, `UNIQUE`, `SEQUENCE` — replacing the need for Ctrl+Shift+Enter array formulas
- C) A formula linked to a dynamic external data source
- D) A formula that recalculates dynamically based on a timer

**Answer: B**

---

**Q25.** 🔴 What does the `FILTER` function (Excel 365) do?
- A) Applies AutoFilter to a range
- B) Returns a filtered subset of a range based on one or more conditions — dynamically spilling only rows that meet the criteria; e.g., `=FILTER(A2:C100, B2:B100="East")` returns all rows where region is East
- C) Filters out error values from a range
- D) A PivotTable filter applied via formula

**Answer: B**

---

**Q26.** 🔴 What is "VBA" (Visual Basic for Applications) in Excel?
- A) A visualization tool built into Excel
- B) A programming language embedded in Excel (and other Office apps) enabling users to write macros and automate repetitive tasks, build custom functions, and create interactive forms
- C) A data validation framework for Excel
- D) A version control system for Excel files

**Answer: B**

---

**Q27.** 🔴 What is the difference between a "macro" and a "VBA module" in Excel?
- A) Macros are written in Python; VBA modules are in Visual Basic
- B) A macro is a recorded or written sequence of actions (typically a Sub in VBA) that automates a task; a VBA module is the code container (within the VBA Editor) holding one or more macros and functions
- C) Macros can only record clicks; VBA modules can only write formulas
- D) They are identical — macro is just the common name for a VBA module

**Answer: B**

---

**Q28.** 🔴 What is `OFFSET(reference, rows, cols, [height], [width])` used for?
- A) Moving a cell's content to a different location
- B) Returning a reference to a range that is offset from a starting cell by a specified number of rows and columns — used for dynamic ranges in formulas and charts that automatically expand with data
- C) Calculating the offset between two date values
- D) Shifting a PivotTable's position on the sheet

**Answer: B**

---

**Q29.** 🔴 What is `INDIRECT(ref_text)` used for in Excel?
- A) Returning an indirect (approximate) match
- B) Returning a reference specified by a text string — enabling formulas to dynamically reference different sheets or ranges based on cell values (e.g., `=INDIRECT(A1&"!B2")` references cell B2 on a sheet named in A1)
- C) Checking if a cell indirectly depends on another
- D) Linking to an external workbook indirectly

**Answer: B**

---

**Q30.** 🔴 What is "Power Pivot" in Excel?
- A) A powerful PivotTable with additional chart types
- B) An Excel add-in providing an in-memory data model that handles millions of rows, enables relationships between multiple tables, and supports DAX (Data Analysis Expressions) for advanced calculated measures — extending PivotTable capabilities significantly
- C) A premium SAP-to-Excel connector
- D) A pivot table that automatically refreshes from SAP

**Answer: B**

---

**Q31.** 🔴 What is "DAX" (Data Analysis Expressions) in Power Pivot/Power BI?
- A) A deprecated Excel formula language
- B) A formula language for defining custom calculations in Power Pivot and Power BI — including calculated columns and measures using functions like `CALCULATE`, `SUMX`, `RELATED`, and time intelligence functions
- C) A data access extension for connecting Excel to APIs
- D) A SQL dialect used in Power Query

**Answer: B**

---

**Q32.** 🟡 What is "data validation" in Excel?
- A) Checking data quality using Python scripts
- B) A feature that restricts what a user can enter in a cell — defining rules like allowed values (dropdown list, number range, date) and displaying error messages for invalid input
- C) Validating Excel formulas for syntax errors
- D) A macro that checks all cells for blank values

**Answer: B**

---

**Q33.** 🟡 What does `ROUND(number, num_digits)` do?
- A) Rounds a number up to the nearest integer always
- B) Rounds a number to a specified number of decimal places (positive num_digits) or to the left of the decimal (negative num_digits) using standard rounding rules
- C) Rounds a number down always (floor function)
- D) Returns the nearest even number

**Answer: B**

---

**Q34.** 🟡 What is the purpose of "freeze panes" in Excel?
- A) Protecting selected cells from editing
- B) Locking specific rows or columns (e.g., headers) so they remain visible while scrolling through a large dataset
- C) Freezing a formula so it doesn't recalculate
- D) Preventing the workbook from being modified

**Answer: B**

---

**Q35.** 🔴 What is the `GETPIVOTDATA` function used for?
- A) Retrieving all PivotTable data into a flat table
- B) Extracting specific data from a PivotTable using field names and values — automatically generated when you click a PivotTable cell in a formula; useful for building reports that reference PivotTable values robustly even when the pivot layout changes
- C) Importing data from an external pivot database
- D) Converting a PivotTable into a flat data range

**Answer: B**

---
*35 Questions | Shared — Microsoft Excel.*
