# Key DAX Measures

This document lists the main DAX measures used in the Power BI report. It is generated from a DAX Studio export so you can review logic, reuse measures, and track KPI definitions consistently across visuals.

## Notes
- Naming convention: most measures start with `M_` (Metric).
- Some measures are helper/icon measures (e.g., `M_icon_*`) used for KPI cards and conditional formatting.
- Measure names are kept as-is to match the PBIX model.

## How to regenerate
1. Open the PBIX in Power BI Desktop.
2. Open **External Tools** > **DAX Studio**.
3. In **DMV**, run the `TMSCHEMA_MEASURES` query and export results to Excel/CSV.
4. Rebuild this file by pasting the exported expressions (or re-running your export script).

_Generated on 2026-01-19 17:10._

---

## Contents
- [Notes](#notes)
- [How to regenerate](#how-to-regenerate)
- [Contents](#contents)
- [Sales and Profitability](#sales-and-profitability)
- [Time Intelligence and Trends](#time-intelligence-and-trends)
- [Delivery Performance](#delivery-performance)
- [Returns](#returns)
- [Customer](#customer)
- [Product and Pareto](#product-and-pareto)
- [Region and Geography](#region-and-geography)
- [Appendix: Measures missing DAX expressions in the current export](#appendix-measures-missing-dax-expressions-in-the-current-export)

---

## Sales and Profitability

### % Revenue Year

> Format: `0.00%;-0.00%;0.00%`

```DAX
M_%_revenue_year :=
[M_revenue] / [M_revenue_remove]
```

### Avg Revenue Per Order

```DAX
M_avg revenue per order :=
[M_revenue]/[M_count order]
```

### Cost

```DAX
M_cost :=
SUMX(fact_retail_order,fact_retail_order[Quantity]*fact_retail_order[Unit CP])
```

### Count Completed Orders

> Format: `#,0.00`

```DAX
M_count completed orders :=
CALCULATE(DISTINCTCOUNT(fact_retail_order[Order ID]),dim_return[Returned] = "Completed")
```

### Count Order

> Format: `#,0.00`

```DAX
M_count order :=
DISTINCTCOUNT(fact_retail_order[order id])
```

### Profit

```DAX
M_profit :=
[M_revenue]-[M_cost]
```

### Profit Margin

> Format: `0.00%;-0.00%;0.00%`

```DAX
M_profit margin :=
[M_profit]/[M_revenue]
```

### Profit Accmulating

```DAX
M_profit_accmulating :=
CALCULATE([M_profit],dim_calendar[Date] <= MAX(dim_calendar[Date]))
```

### Profit Remove

```DAX
M_profit_remove :=
CALCULATE( [M_revenue]-[M_cost], ALLEXCEPT(dim_calendar,dim_calendar[Year]))
```

### Revenue

```DAX
M_revenue :=
SUMX(fact_retail_order,fact_retail_order[Quantity]*fact_retail_order[Unit SP])
```

### Revenue Accmulating

```DAX
M_revenue_accmulating :=
CALCULATE([M_revenue],dim_calendar[Date] <= MAX(dim_calendar[Date]))
```

### Revenue Remove

> Format: `0.00%;-0.00%;0.00%`

```DAX
M_revenue_remove :=
CALCULATE( [M_revenue], ALLEXCEPT(dim_calendar,dim_calendar[Year]))
```

### Sales Per Rep

```DAX
M_sales per rep :=
[M_revenue]/DISTINCTCOUNT('dim_retail sales people'[Retail Sales People ID])
```

### Unit Sold

```DAX
M_unit sold :=
SUM(fact_retail_order[quantity])
```

### Win Rate

> Format: `0.00%;-0.00%;0.00%`

```DAX
M_win rate :=
[M_count completed orders]/[M_count order]
```

### Parameter Threshold Value

```DAX
Parameter Threshold Value :=
SELECTEDVALUE('Parameter Threshold'[Parameter Threshold], 0.8)
```

---

## Time Intelligence and Trends

### Revenue QTD

```DAX
M_revenue_QTD :=
CALCULATE([M_revenue],DATESQTD(dim_calendar[Date]))
```

### Revenue YTD

```DAX
M_revenue_YTD :=
CALCULATE([M_revenue],DATESYTD(dim_calendar[Date]))
```

---

## Delivery Performance

### Avg Delivery Days

> Format: `#,0.00`

```DAX
M_avg delivery days :=
AVERAGE(fact_retail_order[Days])
```

### Avg Delivery Days Δ Vs Overall

```DAX
M_avg delivery days Δ vs Overall :=
[M_avg delivery days] - [M_avg delivery days_all_sales rep]
```

### Avg Delivery Days All Sales Rep

> Format: `0.00%;-0.00%;0.00%`

```DAX
M_avg delivery days_all_sales rep :=
CALCULATE( [M_avg delivery days], ALL('dim_retail sales people'))
```

---

## Returns

### Return Rate Δ Abs

```DAX
M_Return Rate Δ Abs :=
ABS([M_Return Rate Δ vs Overall])
```

### Return Rate Δ Vs Overall

> Format: `0.00%;-0.00%;0.00%`

```DAX
M_Return Rate Δ vs Overall :=
[M_return rate] - [M_return rate_all_sales rep]
```

### Return Order

> Format: `#,0`

```DAX
M_return order :=
CALCULATE(
```

### Return Rate

> Format: `0.00%;-0.00%;0.00%`

```DAX
M_return rate :=
[M_return order]/DISTINCTCOUNT(fact_retail_order[Order ID])
```

### Return Rate All Sales Rep

> Format: `0.00%;-0.00%;0.00%`

```DAX
M_return rate_all_sales rep :=
CALCULATE( [M_return rate], ALL('dim_retail sales people'))
```

---

## Customer

### % Revenue Customer

> Format: `0.00%;-0.00%;0.00%`

```DAX
M_% revenue customer :=
[M_revenue]/[M_revenue_all_customer]
```

### Orders Per Customer

```DAX
M_orders per customer :=
[M_count order]/[M_total customers]
```

### Products Per Customer

```DAX
M_products per customer :=
[M_count_product]/[M_total customers]
```

### Revenue Per Customer

```DAX
M_revenue per customer :=
[M_revenue]/[M_total customers]
```

### Revenue All Customer

```DAX
M_revenue_all_customer :=
CALCULATE( [M_revenue], ALL(dim_customer))
```

### Total Customers

> Format: `0`

```DAX
M_total customers :=
COUNT(dim_customer[customer id])
```

---

## Product and Pareto

### % Revenue Product

> Format: `0.00%;-0.00%;0.00%`

```DAX
M_% revenue product :=
[M_revenue]/[M_revenue_all_product]
```

### Count Product

> Format: `0`

```DAX
M_count_product :=
COUNT(dim_product[Product ID])
```

### Revenue All Product

```DAX
M_revenue_all_product :=
CALCULATE( [M_revenue], ALL(dim_product))
```

---

## Region and Geography

### % Revenue Region

> Format: `0.00%;-0.00%;0.00%`

```DAX
M_% revenue region :=
[M_revenue]/[M_revenue_all_region]
```

### Revenue All Region

```DAX
M_revenue_all_region :=
CALCULATE( [M_revenue], ALL(dim_address))
```

---

## Appendix: Measures missing DAX expressions in the current export
The following measures were present in the export file but had an empty `DAX Expression` cell. Re-export from DAX Studio to capture their expressions:

- `M_avg_delivery_days_current`
- `M_avg_delivery_days_MOM_card_kpi`
- `M_customer_retention_rate`
- `M_icon_avg_delivery_days_mom`
- `M_icon_profit_margin_mom`
- `M_icon_profit_mom`
- `M_icon_return_rate_mom`
- `M_icon_revenue_mom`
- `M_icon_sales per rep_mom`
- `M_icon_total_orders_mom`
- `M_icon_win_orders_mom`
- `M_icon_win_rate_mom`
- `M_new_customers`
- `M_profit_current`
- `M_profit_margin_current`
- `M_profit_margin_MOM_card_kpi`
- `M_profit_MOM_card_kpi`
- `M_return rate_all_sub category`
- `M_return_rate_current`
- `M_return_rate_MOM_card_kpi`
- `M_revenue_current`
- `M_revenue_MOM_card_kpi`
- `M_revenue_QoQ`
- `M_sales per rep_MOM_card_kpi`
- `M_sales_per_rep_current`
- `M_total_new_customers`
- `M_total_orders_current`
- `M_total_orders_MOM_card_kpi`
- `M_win_orders_current`
- `M_win_orders_MOM_card_kpi`
- `M_win_rate_current`
- `M_win_rate_MOM_card_kpi`
- `Pareto %`
- `Pareto CF`
