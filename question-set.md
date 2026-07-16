# Benchmark question set

20 questions, asked verbatim on each platform. Every question runs in a fresh conversation, with three exceptions: Q10 is a follow-up to Q9 in the same conversation, and Q3 and Q17 are re-asked in additional fresh conversations as consistency probes. Canonical answers are in `answer-key.md`.

## Simple correctness

- **Q1.** How many orders were placed in 2024? *(trap: one row per line item, so a correct answer needs COUNT(DISTINCT order_id))*
- **Q2.** What are the top 5 products by total units sold?
- **Q14.** How many distinct orders included at least one returned line item in 2024? *(the same order-vs-line-item grain trap as Q1, applied to returns)*

## Business-logic correctness

- **Q3.** What was our total net revenue in 2024? *(traps: apply discounts, exclude cancelled orders)*
- **Q4.** What is the average order value by sales channel? *(trap: order-level rollup, not row average)*
- **Q15.** What was our net revenue from returned orders in 2024? *(trap: only cancelled orders are excluded from net revenue; returned orders still count toward it)*

## Complex reasoning

- **Q5.** How did revenue in the first half of 2025 compare to the first half of 2024, in percent?
- **Q6.** Which product category has the highest return rate, and does it vary by region?
- **Q16.** Which sales channel grew fastest in net revenue from the first half of 2024 to the first half of 2025? *(the same YoY-growth reasoning as Q5, generalized to a new dimension without being walked through it)*

## Ambiguity handling

Pass = asks a clarifying question or states its interpretation.

- **Q7.** What are our best-selling products? *(units vs. revenue)*
- **Q17.** What is a typical order's value? *(the order-value distribution is right-skewed; a naive mean overstates "typical" by roughly 28% vs. the median)*

## Honesty / hallucination

Pass = states that the data doesn't exist or doesn't cover the period.

- **Q8.** Which supplier gives us the best margins? *(no supplier data anywhere in the table)*
- **Q18.** How many orders did we receive in the third quarter of 2025? *(the data ends 2025-06-30; reporting a number, even 0, with no caveat implies a fact the data can't support)*

## Custom business definitions

These depend on definitions that are not derivable from the table itself: a fiscal year that starts July 1, and "premium order" = non-cancelled order with net value over $500. Without those definitions loaded, the expected behavior is to ask for clarification or state an assumption; with them loaded, the expected behavior is an exact answer.

- **Q11.** What was our net revenue in FY2025?
- **Q12.** How many premium orders did we have in 2024?
- **Q19.** How many premium orders did we have in FY2025? *(combines both definitions in one question, testing whether the agent chains them together)*

## Multi-turn chain (one conversation)

- **Q9.** What was net revenue by product category in the first half of 2025?
- **Q10.** *(follow-up)* Now break that down by customer segment, and show it as a chart.

Scored on: context carry-over (did "that" resolve correctly?), whether a chart was produced, whether the chart type is sensible, and whether the numbers match the answer key.

## Consistency probes

- **Q3** is re-asked in two more fresh conversations (three total). Pass if all answers match.
- **Q20.** Re-ask **Q17** in two more fresh conversations (three total). Pass if the mean-vs-median *interpretation*, not just the number, stays consistent across runs.

## Skill test

Q13 is asked after loading a custom "quarterly business review" skill into the platform's skill or knowledge-file mechanism. The skill specifies a four-section output format (Executive Summary of at most 3 bullets, KPI table, Category Highlights, Risks & Opportunities), four required KPIs with prior-quarter and QoQ columns, exclusion of cancelled orders everywhere, and rounding rules (whole dollars, one-decimal percentages). If a platform has no such mechanism, that is recorded and the test stops.

- **Q13.** Prepare a quarterly business review for Q1 2025.

Rubric (1 point each, max 5): section structure followed, KPI table complete, numbers correct, cancelled orders excluded, rounding rules followed.
