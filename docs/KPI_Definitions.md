# KPI Definitions

This document defines the core KPIs used across the Power BI report and how to interpret them.

## Data Grain

- **Fact table grain:** *order line / transaction* (1 row per `Retail Order ID` in `fact_retail_order`).
- **Order-level KPIs:** calculated using **distinct `Order ID`** (because one `Order ID` can contain multiple line items).
- **Return definition:** an order is considered returned when `Returned = "Yes"` for that order context (see `dim_return`).

## Core KPIs (Executive Cards)

### Total Orders
**Meaning:** Number of distinct customer orders in the current filter context.  
**Formula (order-level):** `DISTINCTCOUNT(fact_retail_order[Order ID])`

### Total Line Items
**Meaning:** Number of distinct order lines / transactions.  
**Formula (line-level):** `DISTINCTCOUNT(fact_retail_order[Retail Order ID])`

### Revenue
**Meaning:** Total sales value (top-line).  
**Formula:** `SUM(fact_retail_order[Sales])`

### Profit
**Meaning:** Total profit value.  
**Formula:** `SUM(fact_retail_order[Profit])` *(or a derived measure if your model uses cost-based profit)*

### Profit Margin
**Meaning:** Profitability ratio.  
**Formula:** `DIVIDE([Profit], [Revenue])`

### Avg Delivery Days
**Meaning:** Average delivery time across line items in the current context.  
**Formula:** `AVERAGE(fact_retail_order[Days])`  
**Interpretation:** Higher values may indicate slower fulfillment risk.

## Return KPIs (Quality & Reverse Logistics)

### Returned Orders
**Meaning:** Number of orders that were returned.  
**Formula (order-level):** `DISTINCTCOUNT(fact_retail_order[Order ID])` filtered where `Returned = "Yes"`.

### Return Rate
**Meaning:** Percentage of orders returned.  
**Formula:** `DIVIDE([Returned Orders], [Total Orders])`

### Non-return Rate
**Meaning:** Percentage of orders not returned.  
**Formula:** `1 - [Return Rate]`

## “Overall” Reference (Benchmark Line)

In several visuals/tooltips, a benchmark line labeled **Overall** is used for comparison.

**Definition:** **Overall** removes the **Sales Rep** filter only, while keeping all other report filters (Date/Region/Product/Ship mode/Segment…).

**Why it matters:** When drilling into a Sales Rep, you want to compare:
- **Return rate (Rep)** vs
- **Return rate (Overall)** *(same context except Sales Rep)*

This helps distinguish “rep effect” from “product/logistics/region effect”.
