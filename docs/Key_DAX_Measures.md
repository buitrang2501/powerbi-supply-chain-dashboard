# docs/Key_DAX_Measures.md

## Purpose
This document contains the key DAX measures used in the report.  
Names below follow your naming convention with prefix `M_`.

> Note: table/column names may differ slightly in your model. Replace accordingly.

---

## 1) Core Measures

### Total Orders
```DAX
M_total_orders :=
DISTINCTCOUNT ( fact_retail_order[Retail Order ID] )

```
### Revenue
```DAX
M_revenue :=
SUM ( fact_retail_order[Sales] )

```
### Profit
```DAX
M_profit :=
SUM ( fact_retail_order[Profit] )

```
### Profit Margin
```DAX
M_profit_margin :=
DIVIDE ( [M_profit], [M_revenue] )

```

### Avg Delivery Days
```DAX
M_avg_delivery_days :=
AVERAGE ( fact_retail_order[Days] )

```
---

## 2) Return Measures

### Returned Orders
```DAX
M_return_orders :=
CALCULATE (
    DISTINCTCOUNT ( fact_retail_order[Retail Order ID] ),
    dim_return[Returned] = "Yes"
)

```
### Return Rate
```DAX
M_return_rate :=
DIVIDE ( [M_return_orders], [M_total_orders] )

```
### Non-return Rate
```DAX
M_non_return_rate :=
1 - [M_return_rate]

```

---

## 3) “Overall” Benchmark (remove Sales Rep filter only)

### Overall Return Rate (all sales reps)
```DAX
M_return_rate_all_sales_rep :=
CALCULATE (
    [M_return_rate],
    REMOVEFILTERS ( dim_retail_sales_people[Retail Sales People] )
)

```
### Return Rate Δ vs Overall
```DAX
M_return_rate_delta_vs_overall :=
[M_return_rate] - [M_return_rate_all_sales_rep]

```
### Overall Avg Delivery Days (all sales reps)
```DAX
M_avg_delivery_days_all_sales_rep :=
CALCULATE (
    [M_avg_delivery_days],
    REMOVEFILTERS ( dim_retail_sales_people[Retail Sales People] )
)

```
### Avg Delivery Days Δ vs Overall
```DAX
M_avg_delivery_days_delta_vs_overall :=
[M_avg_delivery_days] - [M_avg_delivery_days_all_sales_rep]

```
Interpretation:
Positive Δ means “worse than overall” (higher return rate / longer delivery) within the same context.

---

## 4) Time Intelligence (example)
Time Intelligence (example)

### Revenue YTD
```DAX
M_revenue_YTD :=
TOTALYTD ( [M_revenue], dim_calendar[Date] )

```
### Profit Margin
```DAX
M_profit_margin :=
DIVIDE ( [M_profit], [M_revenue] )

```
### Profit Margin
```DAX
M_profit_margin :=
DIVIDE ( [M_profit], [M_revenue] )

```
### Profit Margin
```DAX
M_profit_margin :=
DIVIDE ( [M_profit], [M_revenue] )

```
### Profit Margin
```DAX
M_profit_margin :=
DIVIDE ( [M_profit], [M_revenue] )

```
### Profit Margin
```DAX
M_profit_margin :=
DIVIDE ( [M_profit], [M_revenue] )

```
### Profit Margin
```DAX
M_profit_margin :=
DIVIDE ( [M_profit], [M_revenue] )

```
### Profit Margin
```DAX
M_profit_margin :=
DIVIDE ( [M_profit], [M_revenue] )

```
### Profit Margin
```DAX
M_profit_margin :=
DIVIDE ( [M_profit], [M_revenue] )

```
### Profit Margin
```DAX
M_profit_margin :=
DIVIDE ( [M_profit], [M_revenue] )

```
### Profit Margin
```DAX
M_profit_margin :=
DIVIDE ( [M_profit], [M_revenue] )

```
