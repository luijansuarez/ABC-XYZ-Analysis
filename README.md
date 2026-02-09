# ABC–XYZ Inventory Analysis from Unstructured PDF Dispatch Receipts

## Overview
This project transforms a large set of **unstructured dispatch receipts (20,000 PDFs)** into a clean dataset and applies **ABC–XYZ inventory classification** to identify:
- High-impact SKUs that deserve tight service levels and replenishment control (“Unicorns”)
- Low-impact, highly uncertain SKUs that may be candidates for rationalization (“Bones”)
- How the entire product portfolio is distributed across the 9 ABC–XYZ segments

The result is an **automated, repeatable pipeline** that produces an **executive-ready PDF report** and portfolio visuals—designed to support better inventory decisions at scale.

---

## Why this matters (Operational impact)
Inventory teams often struggle with two common problems:
1) **Value concentration:** Which SKUs drive the most revenue?
2) **Demand uncertainty:** Which SKUs fluctuate the most and create planning risk?

By combining ABC (value) and XYZ (variability), this project helps operations:
- Prioritize attention and inventory investment on the SKUs that matter most
- Reduce working capital tied up in low-value, unpredictable items
- Improve service levels by aligning safety stock policies with demand behavior
- Create clearer communication between supply chain, finance, and leadership

---

## What the solution does
### 1) PDF-to-Data (Unstructured → Structured)
- Extracts key fields from each PDF receipt:
  - Order Number, Client, Dispatch Date
  - Line items: SKU, Quantity, Unit Price, Line Total
- Produces a master table with **one row per line item** (nearly 30,000 line items).

### 2) Data quality guardrails
- Validates and standardizes:
  - Dates into proper datetime format
  - Numeric fields into numeric types
- Computes a cross-check metric:
  - `computed_total = qty × price`
  - Compares against the PDF line total to detect inconsistencies

### 3) ABC Analysis (Value contribution)
- Aggregates revenue by SKU
- Computes revenue share and cumulative revenue share
- Assigns ABC categories using standard thresholds:
  - **A:** ~top 80% cumulative revenue
  - **B:** next ~15% (80%–95%)
  - **C:** remaining ~5% (95%–100%)

### 4) XYZ Analysis (Demand variability)
- Aggregates monthly demand per SKU
- Computes mean and standard deviation of monthly quantities
- Calculates **Coefficient of Variation (CV = σ / μ)** and assigns:
  - **X:** stable (CV ≤ 0.5)
  - **Y:** moderate variability (0.5 < CV ≤ 1.0)
  - **Z:** erratic (CV > 1.0)

### 5) Combined ABC–XYZ Classification
- Merges ABC and XYZ outputs into a single SKU classification table
- Identifies key strategy segments:
  - **AX (Unicorns):** high value + stable demand
  - **CZ (Bones):** low value + erratic demand

### 6) Automated Executive Reporting
Generates a PDF report containing:
- Brief business context of ABC–XYZ methodology
- Tables for **Unicorn SKUs (AX)** and **Bone SKUs (CZ)**
- Portfolio distribution visualization (bar chart + % table)

---

## Key deliverables
- **Structured dataset** ready for analysis (from unstructured PDFs)
- **ABC and XYZ classification tables**
- **Portfolio distribution chart**
- **Automated PDF report**: `ABC_XYZ_Report.pdf`

---

## How to run (high level)
1) Extract the PDF archive (orders.zip) into a local folder
2) Run the notebook/script to:
   - parse PDFs into a structured dataset
   - compute ABC and XYZ classifications
   - generate portfolio chart
   - export the executive PDF report

> Note: The workflow is designed to scale reliably across thousands of documents.

---

## Example outcomes (what you can learn)
- Whether revenue is concentrated in a small SKU set (classic Pareto) or spread across the portfolio
- Which SKUs are stable enough for leaner safety stock vs. which need protection
- Which low-value SKUs introduce unnecessary complexity and volatility
- Which categories deserve deeper root-cause analysis (e.g., AZ or CZ items)

---

## Skills demonstrated (relevant to supply chain roles)
- Inventory segmentation and prioritization (ABC–XYZ)
- Demand variability analysis (monthly aggregation + CV)
- Data extraction from real operational documents (PDF receipts)
- Building scalable, audit-friendly pipelines with quality checks
- Creating stakeholder-ready reporting artifacts (charts + executive PDF)

---

## Future improvements (optional extensions)
- Add AY/AZ “watchlists” when AX is limited (common in volatile portfolios)
- Incorporate lead times and service levels to recommend reorder points
- Track customer-level demand patterns to identify drivers of volatility
- Build a dashboard view for ongoing monitoring

