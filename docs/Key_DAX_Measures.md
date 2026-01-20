# Key DAX Measures

This document lists the key DAX measures used in the Power BI report. It is generated from a DAX Studio export so you can review logic, reuse measures, and keep KPI definitions consistent across visuals.

_Generated: 2026-01-20 04:16_

## Notes

- Naming convention: most measures start with `M_` (Metric).
- Some measures are helper/icon measures (e.g., `M_icon_*`) used for KPI cards and conditional formatting.
- If a measure shows **Expression not available**, it means the expression was missing in the provided export and should be re-exported from DAX Studio.

## Sections

- Sales & Profitability (25)
- Orders (9)
- Customers (9)
- Products & Pareto (5)
- Delivery & Logistics (5)
- Returns (8)
- Parameters (1)
- Helper / KPI Icon Measures (9)

## Measures

## Sales & Profitability

### M_% revenue region

**Format string:** `0.00%;-0.00%;0.00%`  **Hidden:** No

```DAX
[M_% revenue region] :=
[M_revenue]/[M_revenue_all_region]
```

### M_%_revenue_year

**Format string:** `0.00%;-0.00%;0.00%`  **Hidden:** No

```DAX
[M_%_revenue_year] :=
[M_revenue] / [M_revenue_remove]
```

### M_avg revenue per order

**Hidden:** No

```DAX
[M_avg revenue per order] :=
[M_revenue]/[M_count order]
```

### M_cost

**Hidden:** No

```DAX
M_cost :=
SUMX(fact_retail_order,fact_retail_order[Quantity]*fact_retail_order[Unit CP])
```

### M_profit

**Hidden:** No

```DAX
M_profit :=
[M_revenue]-[M_cost]
```

### M_profit margin

**Format string:** `0.00%;-0.00%;0.00%`  **Hidden:** No

```DAX
[M_profit margin] :=
[M_profit]/[M_revenue]
```

### M_profit_MOM_card_kpi

**Hidden:** No

```DAX
-- M_profit_MOM_card_kpi :=
-- Expression not available in the provided export.
-- Re-export this measure from DAX Studio to capture the full DAX.
```

### M_profit_accmulating

**Hidden:** No

```DAX
M_profit_accmulating :=
CALCULATE([M_profit],dim_calendar[Date] <= MAX(dim_calendar[Date]))
```

### M_profit_current

**Hidden:** No

```DAX
-- M_profit_current :=
-- Expression not available in the provided export.
-- Re-export this measure from DAX Studio to capture the full DAX.
```

### M_profit_margin_MOM_card_kpi

**Hidden:** No

```DAX
-- M_profit_margin_MOM_card_kpi :=
-- Expression not available in the provided export.
-- Re-export this measure from DAX Studio to capture the full DAX.
```

### M_profit_margin_current

**Format string:** `0.00%;-0.00%;0.00%`  **Hidden:** No

```DAX
-- M_profit_margin_current :=
-- Expression not available in the provided export.
-- Re-export this measure from DAX Studio to capture the full DAX.
```

### M_profit_remove

**Hidden:** No

```DAX
M_profit_remove :=
CALCULATE( [M_revenue]-[M_cost], ALLEXCEPT(dim_calendar,dim_calendar[Year]))
```

### M_revenue

**Hidden:** No

```DAX
M_revenue :=
SUMX(fact_retail_order,fact_retail_order[Quantity]*fact_retail_order[Unit SP])
```

### M_revenue_MOM_card_kpi

**Format string:** `0.00%;-0.00%;0.00%`  **Hidden:** No

```DAX
-- M_revenue_MOM_card_kpi :=
-- Expression not available in the provided export.
-- Re-export this measure from DAX Studio to capture the full DAX.
```

### M_revenue_QTD

**Hidden:** No

```DAX
M_revenue_QTD :=
CALCULATE([M_revenue],DATESQTD(dim_calendar[Date]))
```

### M_revenue_QoQ

**Format string:** `0.00%;-0.00%;0.00%`  **Hidden:** No

```DAX
-- M_revenue_QoQ :=
-- Expression not available in the provided export.
-- Re-export this measure from DAX Studio to capture the full DAX.
```

### M_revenue_YTD

**Hidden:** No

```DAX
M_revenue_YTD :=
CALCULATE([M_revenue],DATESYTD(dim_calendar[Date]))
```

### M_revenue_accmulating

**Hidden:** No

```DAX
M_revenue_accmulating :=
CALCULATE([M_revenue],dim_calendar[Date] <= MAX(dim_calendar[Date]))
```

### M_revenue_all_region

**Hidden:** No

```DAX
M_revenue_all_region :=
CALCULATE( [M_revenue], ALL(dim_address))
```

### M_revenue_current

**Hidden:** No

```DAX
-- M_revenue_current :=
-- Expression not available in the provided export.
-- Re-export this measure from DAX Studio to capture the full DAX.
```

### M_revenue_remove

**Format string:** `0.00%;-0.00%;0.00%`  **Hidden:** No

```DAX
M_revenue_remove :=
CALCULATE( [M_revenue], ALLEXCEPT(dim_calendar,dim_calendar[Year]))
```

### M_sales per rep

**Hidden:** No

```DAX
[M_sales per rep] :=
[M_revenue]/DISTINCTCOUNT('dim_retail sales people'[Retail Sales People ID])
```

### M_sales per rep_MOM_card_kpi

**Hidden:** No

```DAX
-- [M_sales per rep_MOM_card_kpi] :=
-- Expression not available in the provided export.
-- Re-export this measure from DAX Studio to capture the full DAX.
```

### M_sales_per_rep_current

**Hidden:** No

```DAX
-- M_sales_per_rep_current :=
-- Expression not available in the provided export.
-- Re-export this measure from DAX Studio to capture the full DAX.
```

### M_unit sold

**Hidden:** No

```DAX
[M_unit sold] :=
SUM(fact_retail_order[quantity])
```

## Orders

### M_count completed orders

**Format string:** `#,0.00`  **Hidden:** No

```DAX
[M_count completed orders] :=
CALCULATE(DISTINCTCOUNT(fact_retail_order[Order ID]),dim_return[Returned] = "Completed")
```

### M_count order

**Format string:** `#,0.00`  **Hidden:** No

```DAX
[M_count order] :=
DISTINCTCOUNT(fact_retail_order[order id])
```

### M_total_orders_MOM_card_kpi

**Hidden:** No

```DAX
-- M_total_orders_MOM_card_kpi :=
-- Expression not available in the provided export.
-- Re-export this measure from DAX Studio to capture the full DAX.
```

### M_total_orders_current

**Hidden:** No

```DAX
-- M_total_orders_current :=
-- Expression not available in the provided export.
-- Re-export this measure from DAX Studio to capture the full DAX.
```

### M_win rate

**Format string:** `0.00%;-0.00%;0.00%`  **Hidden:** No

```DAX
[M_win rate] :=
[M_count completed orders]/[M_count order]
```

### M_win_orders_MOM_card_kpi

**Hidden:** No

```DAX
-- M_win_orders_MOM_card_kpi :=
-- Expression not available in the provided export.
-- Re-export this measure from DAX Studio to capture the full DAX.
```

### M_win_orders_current

**Format string:** `#,0`  **Hidden:** No

```DAX
-- M_win_orders_current :=
-- Expression not available in the provided export.
-- Re-export this measure from DAX Studio to capture the full DAX.
```

### M_win_rate_MOM_card_kpi

**Hidden:** No

```DAX
-- M_win_rate_MOM_card_kpi :=
-- Expression not available in the provided export.
-- Re-export this measure from DAX Studio to capture the full DAX.
```

### M_win_rate_current

**Format string:** `0.00%;-0.00%;0.00%`  **Hidden:** No

```DAX
-- M_win_rate_current :=
-- Expression not available in the provided export.
-- Re-export this measure from DAX Studio to capture the full DAX.
```

## Customers

### M_% revenue customer

**Format string:** `0.00%;-0.00%;0.00%`  **Hidden:** No

```DAX
[M_% revenue customer] :=
[M_revenue]/[M_revenue_all_customer]
```

### M_customer_retention_rate

**Format string:** `0.00%;-0.00%;0.00%`  **Hidden:** No

```DAX
-- M_customer_retention_rate :=
-- Expression not available in the provided export.
-- Re-export this measure from DAX Studio to capture the full DAX.
```

### M_new_customers

**Hidden:** No

```DAX
-- M_new_customers :=
-- Expression not available in the provided export.
-- Re-export this measure from DAX Studio to capture the full DAX.
```

### M_orders per customer

**Hidden:** No

```DAX
[M_orders per customer] :=
[M_count order]/[M_total customers]
```

### M_products per customer

**Hidden:** No

```DAX
[M_products per customer] :=
[M_count_product]/[M_total customers]
```

### M_revenue per customer

**Hidden:** No

```DAX
[M_revenue per customer] :=
[M_revenue]/[M_total customers]
```

### M_revenue_all_customer

**Hidden:** No

```DAX
M_revenue_all_customer :=
CALCULATE( [M_revenue], ALL(dim_customer))
```

### M_total customers

**Hidden:** No

```DAX
[M_total customers] :=
COUNT(dim_customer[customer id])
```

### M_total_new_customers

**Hidden:** No

```DAX
-- M_total_new_customers :=
-- Expression not available in the provided export.
-- Re-export this measure from DAX Studio to capture the full DAX.
```

## Products & Pareto

### M_% revenue product

**Format string:** `0.00%;-0.00%;0.00%`  **Hidden:** No

```DAX
[M_% revenue product] :=
[M_revenue]/[M_revenue_all_product]
```

### M_count_product

**Hidden:** No

```DAX
M_count_product :=
COUNT(dim_product[Product ID])
```

### M_revenue_all_product

**Hidden:** No

```DAX
M_revenue_all_product :=
CALCULATE( [M_revenue], ALL(dim_product))
```

### Pareto %

**Format string:** `0.00%;-0.00%;0.00%`  **Hidden:** No

```DAX
-- [Pareto %] :=
-- Expression not available in the provided export.
-- Re-export this measure from DAX Studio to capture the full DAX.
```

### Pareto CF

**Hidden:** No

```DAX
-- [Pareto CF] :=
-- Expression not available in the provided export.
-- Re-export this measure from DAX Studio to capture the full DAX.
```

## Delivery & Logistics

### M_avg delivery days

**Format string:** `#,0.00`  **Hidden:** No

```DAX
[M_avg delivery days] :=
AVERAGE(fact_retail_order[Days])
```

### M_avg delivery days Δ vs Overall

**Hidden:** No

```DAX
[M_avg delivery days Δ vs Overall] :=
[M_avg delivery days] - [M_avg delivery days_all_sales rep]
```

### M_avg delivery days_all_sales rep

**Format string:** `0.00%;-0.00%;0.00%`  **Hidden:** No

```DAX
[M_avg delivery days_all_sales rep] :=
CALCULATE( [M_avg delivery days], ALL('dim_retail sales people'))
```

### M_avg_delivery_days_MOM_card_kpi

**Hidden:** No

```DAX
-- M_avg_delivery_days_MOM_card_kpi :=
-- Expression not available in the provided export.
-- Re-export this measure from DAX Studio to capture the full DAX.
```

### M_avg_delivery_days_current

**Hidden:** No

```DAX
-- M_avg_delivery_days_current :=
-- Expression not available in the provided export.
-- Re-export this measure from DAX Studio to capture the full DAX.
```

## Returns

### M_Return Rate Δ Abs

**Hidden:** No

```DAX
[M_Return Rate Δ Abs] :=
ABS([M_Return Rate Δ vs Overall])
```

### M_Return Rate Δ vs Overall

**Format string:** `0.00%;-0.00%;0.00%`  **Hidden:** No

```DAX
[M_Return Rate Δ vs Overall] :=
[M_return rate] - [M_return rate_all_sales rep]
```

### M_return order

**Format string:** `#,0`  **Hidden:** No

```DAX
[M_return order] :=
CALCULATE(
```

### M_return rate

**Format string:** `0.00%;-0.00%;0.00%`  **Hidden:** No

```DAX
[M_return rate] :=
[M_return order]/DISTINCTCOUNT(fact_retail_order[Order ID])
```

### M_return rate_all_sales rep

**Format string:** `0.00%;-0.00%;0.00%`  **Hidden:** No

```DAX
[M_return rate_all_sales rep] :=
CALCULATE( [M_return rate], ALL('dim_retail sales people'))
```

### M_return rate_all_sub category

**Hidden:** No

```DAX
-- [M_return rate_all_sub category] :=
-- Expression not available in the provided export.
-- Re-export this measure from DAX Studio to capture the full DAX.
```

### M_return_rate_MOM_card_kpi

**Hidden:** No

```DAX
-- M_return_rate_MOM_card_kpi :=
-- Expression not available in the provided export.
-- Re-export this measure from DAX Studio to capture the full DAX.
```

### M_return_rate_current

**Format string:** `0.00%;-0.00%;0.00%`  **Hidden:** No

```DAX
-- M_return_rate_current :=
-- Expression not available in the provided export.
-- Re-export this measure from DAX Studio to capture the full DAX.
```

## Parameters

### Parameter Threshold Value

**Hidden:** No

```DAX
[Parameter Threshold Value] :=
SELECTEDVALUE('Parameter Threshold'[Parameter Threshold], 0.8)
```

## Helper / KPI Icon Measures

### M_icon_avg_delivery_days_mom

**Hidden:** No

```DAX
-- M_icon_avg_delivery_days_mom :=
-- Expression not available in the provided export.
-- Re-export this measure from DAX Studio to capture the full DAX.
```

### M_icon_profit_margin_mom

**Hidden:** No

```DAX
-- M_icon_profit_margin_mom :=
-- Expression not available in the provided export.
-- Re-export this measure from DAX Studio to capture the full DAX.
```

### M_icon_profit_mom

**Hidden:** No

```DAX
-- M_icon_profit_mom :=
-- Expression not available in the provided export.
-- Re-export this measure from DAX Studio to capture the full DAX.
```

### M_icon_return_rate_mom

**Hidden:** No

```DAX
-- M_icon_return_rate_mom :=
-- Expression not available in the provided export.
-- Re-export this measure from DAX Studio to capture the full DAX.
```

### M_icon_revenue_mom

**Hidden:** No

```DAX
-- M_icon_revenue_mom :=
-- Expression not available in the provided export.
-- Re-export this measure from DAX Studio to capture the full DAX.
```

### M_icon_sales per rep_mom

**Hidden:** No

```DAX
-- [M_icon_sales per rep_mom] :=
-- Expression not available in the provided export.
-- Re-export this measure from DAX Studio to capture the full DAX.
```

### M_icon_total_orders_mom

**Hidden:** No

```DAX
-- M_icon_total_orders_mom :=
-- Expression not available in the provided export.
-- Re-export this measure from DAX Studio to capture the full DAX.
```

### M_icon_win_orders_mom

**Hidden:** No

```DAX
-- M_icon_win_orders_mom :=
-- Expression not available in the provided export.
-- Re-export this measure from DAX Studio to capture the full DAX.
```

### M_icon_win_rate_mom

**Hidden:** No

```DAX
-- M_icon_win_rate_mom :=
-- Expression not available in the provided export.
-- Re-export this measure from DAX Studio to capture the full DAX.
```
