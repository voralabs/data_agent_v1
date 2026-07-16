# Data Agent Comparison: Snowflake vs. Databricks vs. BigQuery

Companion materials for a Substack post comparing the native conversational data agents of three cloud data platforms: Snowflake (Cortex Analyst), Databricks (AI/BI Genie), and BigQuery (Conversational Analytics). All three were tested on one identical single-table retail dataset.

Read the full write-up here: **[Substack post - link]**

## Contents

| Path | What it is |
|---|---|
| `question-set.md` | The 20 comparison questions, asked verbatim on every platform |
| `answer-key.md` | Canonical answers computed from the dataset, plus common wrong-answer signatures |
| `data/sales_orders.csv` | The dataset: synthetic retail sales, ~9,400 rows, 5,000 orders, 14 columns |

## Notes

The dataset is fully synthetic and contains deliberate traps: one row per line item (not per order), cancelled and returned orders, discounts, and a right-skewed order-value distribution. The questions are designed to test whether an agent navigates those traps, handles ambiguity, and admits when data doesn't exist.
