# Data Quality Report — Step 1: Raw Data Audit

Before any cleaning, the raw dataset was fully audited column by column. This report documents every issue found — it directly shaped the cleaning approach in [`methodology.md`](methodology.md).

**Dataset audited:** `data/raw/superstore_RAW.csv` — 9,994 rows × 21 columns, Latin-1 encoding (not UTF-8).

---

## Data Type Issues

| Column | Issue |
|---|---|
| `Order Date`, `Ship Date` | Stored as text strings rather than date values; must be converted before any time-based analysis |
| `Postal Code` | Stored as an integer, which strips leading zeros. This corrupted 51 postal codes belonging to Northeastern US states (e.g., Massachusetts `02138` became `2138`) |

## Duplicate Records

| Check | Result |
|---|---|
| Fully duplicated rows | 0 |
| Duplicate `Row ID` (primary key) | 0 |
| Same `Order ID` + `Product ID` appearing more than once | 8 order/product pairs (16 rows total), flagged for manual review |

Closer inspection (comparing full row values — Sales, Quantity, Discount — not just the ID pair) found that **only 1 of these 8 pairs was a true full-row duplicate** (`Order US-2014-150119`, Product `FUR-CH-10002965` — both rows identical in every field). The other 7 were legitimate separate purchases of the same product within the same order, with different quantities and sale amounts.

## Referential Consistency

| Check | Result |
|---|---|
| `Customer ID` → `Customer Name` consistency | Fully consistent, no conflicts |
| `Product ID` → `Product Name` consistency | 32 Product IDs are linked to two different product name variants, likely due to renaming or catalog updates over time |

## Business Logic Checks

| Check | Result |
|---|---|
| Orders shipped before they were placed | 0 (no logic errors) |
| Transactions with negative profit | 1,871 rows (~19% of all transactions) |
| Transactions with zero profit | 65 rows |
| Transactions with ≥50% discount **and** negative profit | 922 rows — a strong early signal that aggressive discounting is a key driver of losses |

## Outliers & Distribution Notes

| Field | Range | Note |
|---|---|---|
| `Sales` | $0.44 – $22,638.48 | Heavy right skew, typical for retail order data |
| `Quantity` | 1 – 14 units | No extreme outliers |
| `Discount` | 0% – 80% | High end (≥50%) flagged for deeper profitability analysis |

## Formatting Notes

- `Country` contains a single constant value (`United States`) across all 9,994 rows — no analytical value; flagged for removal
- 2,221 product names contain embedded commas, confirming the file requires correct quoted-CSV parsing (already handled correctly in the raw source file)
- File encoding is Latin-1, not UTF-8 — noted to avoid character corruption during processing

---

Every issue above was addressed during cleaning — see [`methodology.md`](methodology.md) for exactly how each one was fixed.
