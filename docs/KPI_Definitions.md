# docs/KPI_Definitions.md

## Purpose
This document defines core KPIs used across the Power BI report.  
All KPIs are calculated at **order level** (grain = 1 row per `Retail Order ID`) unless stated otherwise.

---

## Data Grain & Conventions
- **Grain:** 1 row per `Retail Order ID` in `fact_retail_order`
- **Return definition:** An order is considered returned when `dim_return[Returned] = "Yes"` (or equivalent).
- **Delivery days:** `fact_retail_order[Days]` = actual delivery time in days.
- **Revenue/Profit:** taken directly from fact table fields (`Sales`, `Profit`) after applying report filters.

---

## Core KPIs (Executive Cards)

### 1) Total Orders
**Meaning:** Number of distinct retail orders in the current filter context.  
**Formula:** DistinctCount(`Retail Order ID`)  
**Use cases:** order volume trend, denominator for return rate.

### 2) Revenue
**Meaning:** Total sales value (top-line).  
**Formula:** Sum(`Sales`)  
**Use cases:** trend analysis, regional/product performance.

### 3) Profit
**Meaning:** Total profit value.  
**Formula:** Sum(`Profit`)

### 4) Profit Margin
**Meaning:** Profitability ratio.  
**Formula:** `Profit / Revenue`  
**Note:** Use `DIVIDE()` to avoid divide-by-zero errors.

### 5) Avg Delivery Days
**Meaning:** Average delivery time across orders in the current context.  
**Formula:** Average(`Days`)  
**Interpretation:** Higher values may indicate slower logistics / higher fulfillment risk.

---

## Return KPIs (Quality & Reverse Logistics)

### 6) Returned Orders
**Meaning:** Number of orders that were returned.  
**Formula:** DistinctCount(`Retail Order ID`) where Returned = "Yes"

### 7) Return Rate
**Meaning:** Percentage of orders returned.  
**Formula:** `Returned Orders / Total Orders`  
**Interpretation:** A key cost driver (reverse logistics, lost margin, customer dissatisfaction).

### 8) Non-return Rate
**Meaning:** Percentage of orders not returned.  
**Formula:** `1 - Return Rate`

---

## “Overall” Reference (Benchmark Line)
In several charts/tooltips, you plot a benchmark line labeled **Overall**.

### Definition
**Overall** removes **Sales Rep** filter only, while keeping all other report filters (Date/Region/Category/Ship mode/Segment…).

### Why this matters
When drilling into Sales Rep, you want to compare:
- **Return rate (Rep)** vs
- **Return rate (Overall)** (same context except Sales Rep)

This helps distinguish “rep effect” from “product/logistics/region effect”.

---

## Time Intelligence (Trends)

### 9) Revenue / Profit (YTD, QTD, QoQ, MoM)
- **YTD / QTD:** Standard cumulative metrics to show progress within year/quarter.
- **MoM / QoQ change:** Used for KPI cards (this month vs last month, this quarter vs last quarter).

---

## Driver Views (Root-cause style tooltips)
These visuals are used to investigate drivers behind return rate / delivery time:

- Return rate by **Sub-category**
- Return rate by **Ship mode**
- Return rate by **Delivery days bucket**
- Return rate by **Region**
- Return rate by **Customer segment**

**Best practice interpretation:**  
Always check both:
1) Return rate (risk) AND  
2) Order volume / revenue (impact)

High return rate but tiny volume may be less urgent than moderate return rate with huge volume.
