# Data Dictionary — Sample Superstore Dataset

Definitions for every field in the raw source file (`data/raw/superstore_RAW.csv`), 21 columns, 9,994 rows.

| Field | Type (raw) | Description |
|---|---|---|
| `Row ID` | Integer | Unique identifier for each row/line item. Guaranteed unique — the only safe key for true duplicate detection. |
| `Order ID` | Text | Identifier for a customer order. One order can contain multiple line items (rows), each a different product. |
| `Order Date` | Text (raw) | Date the order was placed. Stored as text in the raw file — converted to a proper Date type during cleaning. |
| `Ship Date` | Text (raw) | Date the order was shipped. Same text-to-date issue as `Order Date`. |
| `Ship Mode` | Text | Shipping method: Standard Class, Second Class, First Class, or Same Day. |
| `Customer ID` | Text | Unique identifier for a customer. |
| `Customer Name` | Text | Full name of the customer. One-to-one with `Customer ID` — no conflicts found. |
| `Segment` | Text | Customer segment: Consumer, Corporate, or Home Office. |
| `Country` | Text | Always "United States" for every row — constant field, removed during cleaning (no analytical value). |
| `City` | Text | City of the shipping address. |
| `State` | Text | US state of the shipping address. |
| `Postal Code` | Integer (raw) | ZIP code. Stored as a number in the raw file, which strips leading zeros on Northeastern ZIP codes (e.g., `02138` → `2138`) — fixed during cleaning. |
| `Region` | Text | Central, East, South, or West. |
| `Product ID` | Text | Unique identifier for a product. A small number of IDs were linked to two different name variants in the raw data (see `data_quality_report.md`). |
| `Category` | Text | Furniture, Office Supplies, or Technology. |
| `Sub-Category` | Text | More specific product grouping within a Category (e.g., Bookcases, Chairs, Binders). |
| `Product Name` | Text | Full product name/description. |
| `Sales` | Decimal | Revenue for the line item, in USD, after discount is applied. |
| `Quantity` | Integer | Number of units sold in the line item. |
| `Discount` | Decimal (0–1) | Discount applied to the line item, as a fraction (e.g., `0.3` = 30% off). |
| `Profit` | Decimal | Profit for the line item, in USD. Can be negative when a heavy discount pushes the sale below cost. |

## Derived fields (added during cleaning/analysis, not in the raw file)

| Field | Where added | Description |
|---|---|---|
| `Profit Margin` | Power Query custom column | `Profit / Sales`, calculated per row. |
| `Discount Bucket` | Power Query custom column | Categorizes each transaction as `No Discount`, `Low 1–19%`, `Medium 20–49%`, or `High 50%+`. |
| `Year` | Power Query / helper column | Extracted from `Order Date`, used for the YoY Growth summary. |
