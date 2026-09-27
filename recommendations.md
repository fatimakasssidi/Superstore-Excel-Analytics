# Recommendations — Next-Step Business Analysis

Based on the findings in [`analysis_findings.md`](analysis_findings.md), here are concrete next steps a retail business could take. These are analytical recommendations drawn from the data patterns found — not verified against real-world implementation feasibility or company-specific constraints.

---

## 1. Tighten discount policy above 50%, starting with Furniture and Office Supplies in the Central region

**Why:** Transactions with a 50%+ discount lose $97,065.48 in aggregate (-78.8% average margin), and this pattern is heavily concentrated in Furniture (-19% margin) and Office Supplies (-16% margin) specifically in the Central region — while the same categories are profitable everywhere else.

**Suggested action:** Introduce a discount approval threshold (e.g., manager sign-off required above 30-40%) specifically for Furniture and Office Supplies orders shipping to the Central region, rather than a blanket policy change across all regions/categories.

---

## 2. Review high-revenue, low-profit customer accounts individually

**Why:** Sean Miller is the single largest customer by revenue ($25,043.05) but is unprofitable overall (-$1,980.74), almost certainly due to discount terms on his account.

**Suggested action:** Audit the discount terms applied to top-10-by-revenue customers specifically, not just top-10-by-profit. Any customer appearing high on the revenue list but low (or negative) on the profit list is a candidate for renegotiated pricing terms.

---

## 3. Investigate the 2017 margin decline despite revenue growth

**Why:** 2017 sales grew 20.36% year-over-year, but profit only grew 14.24%, and profit margin actually declined 0.68 percentage points — the only year in the dataset where margin moved in the opposite direction of the discounting trend appears to explain.

**Suggested action:** Break down 2017 specifically by discount tier and category to see whether the margin decline is broad-based or concentrated in a few segments (similar to the Central region finding) — this dataset supports that follow-up analysis directly.

---

## 4. Use the annual trend, not month-to-month swings, for planning

**Why:** The dashboard's profit trend is tracked at the annual level (2014–2017), showing a clear and consistent growth pattern each year. This is the right level of granularity for strategic planning; shorter-period comparisons in retail data are typically far more volatile and less reliable for that purpose.

**Suggested action:** If month-level seasonality analysis is needed in the future (e.g., for inventory or staffing planning), it should be rebuilt as its own dedicated analysis with enough historical months to smooth out volatility — a natural next phase for this project.

---

## 5. Extend the profitability lens to Products, not just Customers and Categories

**Why:** This analysis identified profitability problems at the category, region, and customer level, but did not drill into individual products. Given how concentrated the Furniture/Central-region problem is, it's likely a small number of specific products are driving most of the loss.

**Suggested action:** Build a Product-level profitability view (similar to the CLV analysis, but for `Product Name`/`Product ID`) to identify whether losses are broad across the Furniture category or concentrated in a handful of specific items.
