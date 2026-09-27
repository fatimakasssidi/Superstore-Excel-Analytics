# Analysis Findings — Metrics, Tables, and Insights

All figures below were cross-checked directly against `superstore_dashboard.xlsm`.

---

## Profit Margin by Category, Region, and Segment

**By Category**

| Category | Avg. Profit Margin |
|---|---|
| Technology | 16% |
| Office Supplies | 14% |
| Furniture | 4% |
| **Grand Total** | **12%** |

**By Category × Region**

| Category | Central | East | South | West |
|---|---|---|---|---|
| Furniture | **-19%** | 9% | 14% | 10% |
| Office Supplies | **-16%** | 21% | 16% | 29% |
| Technology | 18% | 13% | 19% | 15% |

**By Category × Segment**

| Category | Consumer | Corporate | Home Office |
|---|---|---|---|
| Furniture | 3% | 5% | 5% |
| Office Supplies | 13% | 14% | 17% |
| Technology | 15% | 16% | 17% |

**Insight:** Furniture is by far the least profitable category overall (4% average margin vs. 16% for Technology), and the problem is concentrated almost entirely in the **Central region**, where both Furniture (-19%) and Office Supplies (-16%) sell at a loss on average. The same categories are healthily profitable in the East, South, and West — pointing to a region-specific discounting or pricing issue rather than a category-wide weakness.

---

## Year-over-Year (YoY) Growth

| Year | Total Sales | YoY Sales Growth | Total Profit | YoY Profit Growth | Profit Margin | Margin Change vs. Prior Year |
|---|---|---|---|---|---|---|
| 2014 | $483,966.13 | — | $49,556.03 | — | 10.24% | — |
| 2015 | $470,532.51 | -2.78% | $61,618.60 | +24.34% | 13.10% | +2.86 pts |
| 2016 | $609,205.60 | +29.47% | $81,795.17 | +32.74% | 13.43% | +0.33 pts |
| 2017 | $733,215.26 | +20.36% | $93,439.27 | +14.24% | 12.74% | -0.68 pts |

**Insight:** Sales dipped slightly in 2015 (-2.78%) before growing strongly in 2016 (+29.47%) and continuing at a healthy pace in 2017 (+20.36%). Profit followed a similar but distinct pattern — growing even in 2015 (+24.34%) despite the sales dip, then slowing to +14.24% in 2017 even as sales grew faster (+20.36%). This shows up in the margin trend: profit margin actually **declined** by 0.68 percentage points in 2017, meaning the business grew revenue faster than it grew profit that year — worth watching alongside the discounting findings below.

---

## Customer Lifetime Value (CLV)

**Top customers by total sales (revenue-based ranking)**

| Customer | Total Sales | Total Profit |
|---|---|---|
| Sean Miller | $25,043.05 | **-$1,980.74** |
| Tamara Chand | $19,052.22 | $8,981.32 |
| Raymond Buch | $15,117.34 | $6,976.10 |
| Tom Ashbrook | $14,595.62 | $4,703.79 |
| Adrian Barton | $14,473.57 | $5,444.81 |

**Insight:** Ranking customers by revenue alone is misleading — **Sean Miller is the #1 customer by total sales, but is actually unprofitable overall (-$1,980.74)**, almost certainly due to heavy discounting on his orders. Meanwhile, Tamara Chand generates less revenue but is the most profitable customer by a wide margin ($8,981.32). Sorted by Sales, Sean Miller ranks #1; sorted by Profit, he drops to dead last — a high-revenue customer isn't automatically a high-value one.

---

## Discount-Impact Analysis

| Discount Bucket | Total Sales | Total Profit | Avg. Discount | # Transactions |
|---|---|---|---|---|
| No Discount | $1,087,908.47 | $320,987.60 | 0% | 4,798 |
| Low 1-19% | $81,927.87 | $10,448.17 | 12% | 146 |
| Medium 20-49% | $1,003,935.87 | $52,038.79 | 22% | 4,127 |
| **High 50%+** | $123,147.28 | **-$97,065.48** | 70% | 922 |

**Insight:** This table closes the loop on the Step 1 finding that 922 transactions combined a ≥50% discount with negative profit — at scale, these heavily discounted transactions collectively **lose $97,065.48**, compared to a healthy $320,987.60 profit on undiscounted sales. Profit margin erodes sharply as discount level rises: from a healthy 29.5% margin at no discount down to a **-78.8% average margin** in the High 50%+ bucket. This single view ties together every profitability issue found elsewhere in the analysis — the Central region losses and Sean Miller's unprofitability are, at root, a discounting problem.

---

## A Data Cleaning Correction Worth Noting

While building the growth analysis, a $281.37 discrepancy surfaced in an early monthly total, which led to re-examining the 8 `Order ID` + `Product ID` pairs flagged in Step 1. Closer inspection (comparing full row values, not just the ID pair) showed that only **1 of the 8 pairs was a true full-row duplicate**; the other 7 were legitimate separate purchases with different quantities and amounts. The cleaning step was confirmed to correctly remove only the single genuine duplicate row, validating the original Step 1 cleaning decision.

---

## Dashboard

The `Dashboard` sheet brings the findings above together in one interactive view:

- **4 Slicers** (Region, Category, Segment, Year) filtering every chart and table simultaneously
- **4 KPI cards**: Total Sales, Total Profit, Overall Profit Margin, Loss from Discounts
- **Annual Profit Trend chart**: Total Profit by Year, 2014–2017
- **Profit by Discount Tier chart**: visualizes the discount-impact table above, with the High 50%+ bar dropping below zero
- **Profit Margin heatmap tables**: Category × Region and Category × Segment
- **Top Customers table**: Sales vs. Profit ranking, side by side
