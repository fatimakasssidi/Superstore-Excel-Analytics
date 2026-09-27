# Methodology — Full End-to-End Workflow

This document covers exactly how the raw data was cleaned (Step 2) and how every metric was calculated (Step 3), matching what is actually built in `superstore_dashboard.xlsm`.

---

## Step 2: Data Cleaning

All issues identified in [`data_quality_report.md`](data_quality_report.md) were resolved in Excel using Power Query.

| Issue | Fix Applied | Method |
|---|---|---|
| `Order Date` / `Ship Date` stored as text | Converted both columns to proper **Date** type | Power Query → Change Type → Date |
| `Postal Code` losing leading zeros | Converted column to **Text**, then re-padded values shorter than 5 digits with a leading zero | Power Query → Change Type → Text, then `Text.PadStart([Postal Code], 5, "0")` |
| 8 duplicate `Order ID` + `Product ID` pairs (16 rows) | Compared full row values (Sales, Quantity, Discount) for each pair — 7 pairs were legitimate separate purchases and kept; 1 pair (`US-2014-150119`, `FUR-CH-10002965`) was a true full-row duplicate and had one of its two identical rows removed | Manual review of each pair, comparing all fields before deciding to keep or remove — tracked in the `DupOrderProduct` sheet |
| 32 `Product ID`s mapped to 2 product name variants | Standardized to a single canonical product name per ID | Selected `Product ID` + `Product Name`, removed duplicates to isolate unique ID/Name pairs, then used conditional formatting (highlight duplicate values) on `Product ID` to surface the 32 conflicting IDs; corrected each to one canonical name — tracked in the `ReplaceProductName` sheet |
| `Country` column constant across all rows | Removed the column entirely — no analytical value since every record is `United States` | Power Query → Remove Columns |

**Result:** A clean, analysis-ready dataset (`Cleaned Data` sheet, 9,993 rows after the one duplicate removal) with correct data types, consistent categorical values, and no structural inconsistencies.

---

## Step 3: Key Metrics — Calculation Approach

### Profit Margin
`=Profit / Sales`, added as a Power Query custom column on every transaction, then averaged by Category, Region, and Segment in PivotTables (`Pivot` sheet) to compare profitability across groups rather than just revenue volume.

### Year-over-Year (YoY) Growth — Sales and Profit
Built in the `YoY Growth` sheet:
1. Sales and Profit grouped by Year
2. Rows sorted **ascending by Year** — critical, since Power Query does not sort grouped output automatically, and an unsorted index produces incorrect period comparisons
3. An Index column plus a previous-row lookup used to calculate `(This Year − Last Year) / Last Year` for both Sales and Profit
4. The same sheet also calculates **Profit Margin change year over year**, as a percentage-point difference (`This Year's Margin − Last Year's Margin`) — not a growth rate, since percentage-point changes are subtracted, not divided

### Customer Lifetime Value (CLV)
Built directly with a PivotTable (`Customer Name` in Rows, `Sales` and `Profit` in Values, both summed) in the `Pivot` sheet, ranking customers by both total revenue and actual profitability.

### Discount-Impact Analysis
Transactions bucketed into discount tiers using a Power Query custom column:
```
if [Discount] = 0 then "No Discount"
else if [Discount] < 0.2 then "Low 1-19%"
else if [Discount] < 0.5 then "Medium 20-49%"
else "High 50%+"
```
Summarized by Sales and Profit in a PivotTable (`Pivot` sheet), directly extending the Step 1 finding that 922 transactions combined a ≥50% discount with negative profit.

### Annual Profit Trend (Dashboard chart)
The Dashboard's profit trend chart is built from a PivotTable summarizing **Total Profit by Year** (2014–2017) — a 4-point annual trend line, not a 12-month breakdown.

---

## KPI Cards — Actual Formulas Used

The `KPI cards` sheet pulls its values with the following formulas:

| KPI | Formula | Notes |
|---|---|---|
| Total Sales | `=GETPIVOTDATA("Sum of Sales",$A$3)` | Pulls from a PivotTable summary on the same sheet |
| Total Profit | `=GETPIVOTDATA("Sum of Profit",$A$3)` | Same pattern |
| Overall Profit Margin | `=B10/B9` | Total Profit ÷ Total Sales |
| YoY Sales Growth | `='YoY Growth'!F5` | Direct reference to the 2017 row on the `YoY Growth` sheet |
| YoY Profit Growth | `='YoY Growth'!H5` | Direct reference to the 2017 row on the `YoY Growth` sheet |

**Note on the YoY references:** these currently point to a fixed cell (`F5`/`H5`, the 2017 row). If new years are added to the `YoY Growth` sheet in the future, these two KPI cells would need to be manually updated to point to the new last row, or replaced with a dynamic formula such as `=INDEX('YoY Growth'!F:F, COUNT('YoY Growth'!A:A)+1)`.

---

## Dashboard

Built on the `Dashboard` sheet using:
- 4 Slicers (Region, Category, Segment, Year), each connected to all relevant PivotTables via Report Connections
- 4 KPI cards (Total Sales, Total Profit, Profit Margin, Loss from Discounts)
- 2 PivotCharts: Annual Profit Trend, and Profit by Discount Tier
- 2 PivotTables displayed as heatmap-style tables: Profit Margin by Category × Region, and by Category × Segment
- 1 Top Customers table comparing ranking by Sales vs. by Profit
