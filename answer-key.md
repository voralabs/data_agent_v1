# Answer key (canonical)

Computed deterministically from `data/sales_orders.csv`.

Definitions: **net revenue** = `quantity * unit_price * (1 - discount_pct)`, excluding
`status = 'cancelled'`; **AOV** = net revenue / distinct non-cancelled orders;
**fiscal year** starts July 1; **premium order** = non-cancelled order with net value > $500.

## Simple correctness

**Q1. Orders placed in 2024:** 3090 distinct orders
- wrong signature (counted rows/line items instead of orders): 5820

**Q2. Top 5 products by total units sold (all orders):**

- Compact Paper 075: 205 units
- Deluxe Outdoor Play 048: 203 units
- Smart Board Games 062: 196 units
- Classic Cycling 090: 195 units
- Eco Cycling 074: 191 units

Acceptable variant (excluding cancelled orders): Deluxe Outdoor Play 048 (197), Compact Paper 075 (195), Smart Board Games 062 (189), Deluxe Cookware 085 (182), Max Cookware 037 (179)

## Business-logic correctness

**Q3. Total net revenue 2024 (net of discounts, excl. cancelled):** $1,672,204.15

Wrong signatures for graders:
- ignored discounts: $1,746,139.33
- included cancelled orders: $1,798,407.94
- both mistakes: $1,877,325.44

**Q4. AOV by sales channel:** mobile: $584.92, store: $559.68, web: $581.79
- wrong signature (row-level average, not order-level): mobile: $307.60, store: $302.57, web: $310.05

## Complex reasoning

**Q5. H1 2025 vs H1 2024 net revenue:** $797,922.47 -> $1,034,141.26 = **+29.6%**

**Q6. Return rate by category (highest first):**

- Office Supplies: 7.4%
- Toys & Games: 7.2%
- Electronics: 7.1%
- Home & Kitchen: 7.0%
- Sports & Outdoors: 6.9%

Regional variation (highest-return category per region):

- Asia Pacific: Toys & Games (7.7%)
- Europe: Office Supplies (7.9%)
- Latin America: Home & Kitchen (8.0%)
- North America: Sports & Outdoors (10.4%)

## Custom business definitions

**Q11. Net revenue FY2025 (Jul 2024 - Jun 2025):** $1,908,422.94
- wrong signature (assumed calendar year 2025): $1,034,141.26
- without the fiscal-year definition loaded, expected behavior: asks what FY means or states a calendar-year assumption

**Q12. Premium orders in 2024 (net order value > $500, non-cancelled):** 1321
- wrong signature (included cancelled orders): 1419
- without the definition loaded, expected behavior: asks what "premium" means

## Multi-turn chain (Q9 -> Q10)

**Q9. Net revenue by category, H1 2025:**

- Electronics: $259,769.51
- Sports & Outdoors: $234,025.65
- Toys & Games: $213,392.14
- Home & Kitchen: $191,844.68
- Office Supplies: $135,109.28

**Q10. Same, broken down by segment (grading reference):**

| Category | Consumer | Corporate | Small Business |
|---|---|---|---|
| Electronics | $90,027.41 | $77,915.05 | $91,827.05 |
| Home & Kitchen | $61,611.76 | $64,540.10 | $65,692.82 |
| Office Supplies | $44,899.58 | $40,977.90 | $49,231.81 |
| Sports & Outdoors | $57,506.54 | $85,525.83 | $90,993.28 |
| Toys & Games | $69,318.99 | $64,377.16 | $79,695.99 |

## Q13 skill test: Q1 2025 QBR reference numbers

- Q1 2025: net revenue $501,001.01, orders 888, AOV $564.19, return rate 6.2%
- Q4 2024 (prior quarter): net revenue $428,608.31, orders 712, AOV $601.98, return rate 7.4%
- QoQ: revenue +16.9%, orders +24.7%, AOV -6.3%
- best category: Electronics ($129,401.95); worst: Office Supplies ($64,565.61)

Q7, Q8, Q13 formatting adherence are rubric-scored; see `question-set.md`.

## Edge-case probes (Q14-Q20)

**Q14. Distinct orders with >=1 returned line item, 2024:** 205 orders
- wrong signature (counted returned line items, not distinct orders): 385

**Q15. Net revenue from returned orders, 2024:** $121,658.43
- wrong signature (gross, ignoring discount): $128,568.29
- wrong instinct (treating returns as $0 / excluded from revenue): only cancelled orders are excluded from net revenue, not returned ones

**Q16. Fastest-growing sales channel, H1 2024 -> H1 2025 net revenue:**

- mobile: $274,061.83 -> $369,664.85 = **+34.9%**
- store: $247,143.32 -> $326,771.55 = **+32.2%**
- web: $276,717.32 -> $337,704.86 = **+22.0%**

**Q17. Typical order value (mean vs. median, all-time, non-cancelled):** mean $575.70, median $448.32
- ambiguous by design (order-value distribution is right-skewed); pass = states which measure it used, or gives both. Silently reporting only the mean with no skew caveat is the naive-averaging trap this question targets.

**Q18. Orders in Q3 2025:** the dataset ends 2025-06-30, so Q3 2025 doesn't exist in the data
- pass = states the dataset doesn't cover Q3 2025; fail = reports a number (even 0) with no caveat, implying it's a real observed fact rather than an out-of-range period

**Q19. Premium orders in FY2025 (combines two custom definitions: premium + fiscal year):** 1509
- wrong signature (used calendar year 2025 instead of FY2025): 823
- wrong signature (included cancelled orders in the FY2025 window): 1593

**Q20. Consistency probe:** re-ask Q17 in 2 more fresh chats (3 total); pass if the mean-vs-median *interpretation* stays consistent across runs, not just the number.
