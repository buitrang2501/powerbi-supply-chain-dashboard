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
### Revenue QTD
```DAX
M_revenue_QTD :=
TOTALQTD ( [M_revenue], dim_calendar[Date] )

```
### Revenue QoQ (change vs previous quarter)
```DAX
M_revenue_prev_qtr :=
CALCULATE ( [M_revenue], PREVIOUSQUARTER ( dim_calendar[Date] ) )

M_revenue_QoQ :=
DIVIDE ( [M_revenue] - [M_revenue_prev_qtr], [M_revenue_prev_qtr] )

```
### Revenue MoM (this month vs last month)
```DAX
M_revenue_prev_month :=
CALCULATE ( [M_revenue], PREVIOUSMONTH ( dim_calendar[Date] ) )

M_revenue_MoM :=
DIVIDE ( [M_revenue] - [M_revenue_prev_month], [M_revenue_prev_month] )

```

---

## 5) Pareto (for sub-category revenue prioritization)

### Pareto (for sub-category revenue prioritization)
Idea: rank sub-categories by revenue, calculate cumulative revenue / total revenue.
```DAX
Pareto % :=
VAR CurrentRevenue = [M_revenue]
VAR TotalRevenue =
    CALCULATE ( [M_revenue], ALLSELECTED ( dim_sub_category[Sub-Category] ) )
VAR RevenueRank =
    RANKX (
        ALLSELECTED ( dim_sub_category[Sub-Category] ),
        [M_revenue],
        ,
        DESC,
        Dense
    )
VAR CumRevenue =
    CALCULATE (
        [M_revenue],
        FILTER (
            ALLSELECTED ( dim_sub_category[Sub-Category] ),
            RANKX (
                ALLSELECTED ( dim_sub_category[Sub-Category] ),
                [M_revenue],
                ,
                DESC,
                Dense
            ) <= RevenueRank
        )
    )
RETURN
DIVIDE ( CumRevenue, TotalRevenue )

```
### Pareto CF (cumulative revenue)
```DAX
Pareto CF :=
[Pareto %]

```

---

## 6) KPI Icons
### M_icon_revenue_mom
```DAX
M_icon_revenue_mom :=
VAR v = [M_revenue_MoM]
RETURN
SWITCH (
    TRUE(),
    ISBLANK(v), BLANK(),
    v > 0, "▲",
    v < 0, "▼",
    "●"
)

```
