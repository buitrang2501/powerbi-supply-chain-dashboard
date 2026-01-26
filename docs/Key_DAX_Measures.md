# Key DAX Measures

This file documents the main DAX measures used in the Power BI report.

**Notes**

- The model’s fact table is at **order-line** grain (1 row per `Retail Order ID`).

- KPIs labeled “Total Orders / Return Rate” are calculated at **order** grain using `Order ID` (distinct count), unless stated otherwise.

- For benchmark lines labeled **Overall**, the measure removes **Sales Rep** filter only (keeps other slicers).

## Core Volume Metrics

### M_count completed orders

```DAX

CALCULATE(DISTINCTCOUNT(fact_retail_order[Order ID]),dim_return[Returned] = "Completed")

```

### M_count order

```DAX

DISTINCTCOUNT(fact_retail_order[order id])

```

### M_count_product

```DAX

COUNT(dim_product[Product ID])

```

### M_orders per customer

```DAX

[M_count order]/[M_total customers]

```

### M_total_orders_current

```DAX

VAR current_month =
   MAX ( dim_calendar[Date] ) -- Tháng cuối cùng hiển thị
VAR total_orders_current =
   CALCULATE (
       [M_count order],
       FILTER (
           dim_calendar,
           dim_calendar[Year] = YEAR ( current_month )
               && dim_calendar[Month] = MONTH ( current_month )
       )
   )
RETURN
   total_orders_current

```

### M_win_orders_current

```DAX

VAR current_month =
   MAX ( dim_calendar[Date] )
VAR win_orders_current =
   CALCULATE (
       [M_count completed orders],
       FILTER (
           dim_calendar,
           dim_calendar[Year] = YEAR ( current_month )
               && dim_calendar[Month] = MONTH ( current_month )
       )
   )
RETURN
   win_orders_current

```


---

## Sales & Profitability

### M_% revenue customer

```DAX

[M_revenue]/[M_revenue_all_customer]

```

### M_% revenue product

```DAX

[M_revenue]/[M_revenue_all_product]

```

### M_% revenue region

```DAX

[M_revenue]/[M_revenue_all_region]

```

### M_%_revenue_year

```DAX

[M_revenue] / [M_revenue_remove]

```

### M_avg revenue per order

```DAX

[M_revenue]/[M_count order]

```

### M_cost

```DAX

SUMX(fact_retail_order,fact_retail_order[Quantity]*fact_retail_order[Unit CP])

```

### M_icon_profit_margin_mom

```DAX

VAR positive = UNICHAR(9650)
VAR negative = UNICHAR(9660)
VAR change_mom = [M_profit_margin_MOM_card_kpi]

VAR icon = IF ( change_mom >= 0, positive, negative )
VAR result = icon & " " & FORMAT ( change_mom, "0.0%" )

RETURN
   result

```

### M_icon_profit_mom

```DAX

VAR positive = UNICHAR(9650)
VAR negative = UNICHAR(9660)
VAR change_mom = [M_profit_MOM_card_kpi]

VAR icon = IF ( change_mom >= 0, positive, negative )
VAR result = icon & " " & FORMAT ( change_mom, "0.0%" )

RETURN
   result

```

### M_icon_revenue_mom

```DAX

VAR positive = UNICHAR(9650)
VAR negative = UNICHAR(9660)
VAR change_mom = [M_revenue_MOM_card_kpi]

VAR icon = IF(change_mom >=0, positive, negative)
VAR result = icon & " " & FORMAT(change_mom, "0.0%")

RETURN result

```

### M_icon_sales per rep_mom

```DAX

VAR positive = UNICHAR(9650)
VAR negative = UNICHAR(9660)
VAR change_mom = [M_sales per rep_MOM_card_kpi]

VAR icon = IF(change_mom >=0, positive, negative)
VAR result = icon & " " & FORMAT(change_mom, "0.0%")

RETURN result

```

### M_profit

```DAX

[M_revenue]-[M_cost]

```

### M_profit margin

```DAX

[M_profit]/[M_revenue]

```

### M_profit_MOM_card_kpi

```DAX

VAR current_month = MAX ( dim_calendar[Date] )
VAR previous_month = EOMONTH ( current_month, -1 )

VAR profit_current =
   CALCULATE (
       [M_profit],
       FILTER (
           dim_calendar,
           dim_calendar[Year] = YEAR ( current_month )
               && dim_calendar[Month] = MONTH ( current_month )
       )
   )

VAR profit_previous =
   CALCULATE (
       [M_profit],
       FILTER (
           ALL ( dim_calendar ),
           dim_calendar[Year] = YEAR ( previous_month )
               && dim_calendar[Month] = MONTH ( previous_month )
       )
   )

RETURN
   IF (
       ISBLANK ( profit_previous ),
       0,
       ( [M_profit_current] - profit_previous )
           / profit_previous
   )

```

### M_profit_accmulating

```DAX

CALCULATE([M_profit],dim_calendar[Date] <= MAX(dim_calendar[Date]))

```

### M_profit_current

```DAX

VAR current_month = MAX(dim_calendar[Date]) -- Thang cuoi cung ma minh hien thi ra

VAR profit_current = CALCULATE([M_profit], FILTER(dim_calendar, dim_calendar[Year] = YEAR(current_month) && dim_calendar[Month] = MONTH(current_month)))

RETURN profit_current

```

### M_profit_margin_MOM_card_kpi

```DAX

VAR current_month = MAX ( dim_calendar[Date] )
VAR previous_month = EOMONTH ( current_month, -1 )

VAR profit_margin_current =
   CALCULATE (
       [M_profit margin],
       FILTER (
           dim_calendar,
           dim_calendar[Year] = YEAR ( current_month )
               && dim_calendar[Month] = MONTH ( current_month )
       )
   )

VAR profit_margin_previous =
   CALCULATE (
       [M_profit margin],
       FILTER (
           ALL ( dim_calendar ),
           dim_calendar[Year] = YEAR ( previous_month )
               && dim_calendar[Month] = MONTH ( previous_month )
       )
   )

RETURN
   IF (
       ISBLANK ( profit_margin_previous ),
       0,
       ( [M_profit_margin_current] - profit_margin_previous )
           / profit_margin_previous
   )

```

### M_profit_margin_current

```DAX

VAR current_month =
   MAX ( dim_calendar[Date] ) -- Tháng cuối cùng hiển thị
VAR profit_margin_current =
   CALCULATE (
       [M_profit margin],
       FILTER (
           dim_calendar,
           dim_calendar[Year] = YEAR ( current_month )
               && dim_calendar[Month] = MONTH ( current_month )
       )
   )
RETURN
   profit_margin_current

```

### M_profit_remove

```DAX

CALCULATE( [M_revenue]-[M_cost], ALLEXCEPT(dim_calendar,dim_calendar[Year]))

```

### M_revenue

```DAX

SUMX(fact_retail_order,fact_retail_order[Quantity]*fact_retail_order[Unit SP])

```

### M_revenue per customer

```DAX

[M_revenue]/[M_total customers]

```

### M_revenue_MOM_card_kpi

```DAX

VAR current_month = MAX(dim_calendar[Date])
VAR previous_monht = EOMONTH(current_month,-1)

VAR revenue_current = CALCULATE([M_revenue], FILTER(dim_calendar, dim_calendar[Year] = YEAR(current_month) && dim_calendar[Month] = MONTH(current_month)))

VAR revenue_previous = CALCULATE([M_revenue], FILTER(ALL(dim_calendar),dim_calendar[Year] = YEAR(previous_monht) && dim_calendar[Month] = MONTH(previous_monht)))

RETURN IF(ISBLANK( revenue_previous),0, (revenue_current - revenue_previous)/revenue_previous)

```

### M_revenue_QTD

```DAX

CALCULATE([M_revenue],DATESQTD(dim_calendar[Date]))

```

### M_revenue_QoQ

```DAX

VAR current_date = MAX(dim_calendar[Date])
VAR previous_quarter_date = EDATE(current_date, -3)

VAR revenue_current =
   CALCULATE(
       [M_revenue],
       FILTER(
           ALL(dim_calendar),
           dim_calendar[Year] = YEAR(current_date) &&
           QUARTER(dim_calendar[Date]) = QUARTER(current_date)
       )
   )

VAR revenue_previous =
   CALCULATE(
       [M_revenue],
       FILTER(
           ALL(dim_calendar),
           dim_calendar[Year] = YEAR(previous_quarter_date) &&
           QUARTER(dim_calendar[Date]) = QUARTER(previous_quarter_date)
       )
   )

RETURN
   IF(
       ISBLANK(revenue_previous),
       0,
       DIVIDE(revenue_current - revenue_previous, revenue_previous)
   )

```

### M_revenue_YTD

```DAX

CALCULATE([M_revenue],DATESYTD(dim_calendar[Date]))

```

### M_revenue_accmulating

```DAX

CALCULATE([M_revenue],dim_calendar[Date] <= MAX(dim_calendar[Date]))

```

### M_revenue_all_customer

```DAX

CALCULATE( [M_revenue], ALL(dim_customer))

```

### M_revenue_all_product

```DAX

CALCULATE( [M_revenue], ALL(dim_product))

```

### M_revenue_all_region

```DAX

CALCULATE( [M_revenue], ALL(dim_address))

```

### M_revenue_current

```DAX

VAR current_month = MAX(dim_calendar[Date]) -- Thang cuoi cung ma minh hien thi ra

VAR revenue_current = CALCULATE([M_revenue], FILTER(dim_calendar, dim_calendar[Year] = YEAR(current_month) && dim_calendar[Month] = MONTH(current_month)))

RETURN revenue_current

```

### M_revenue_remove

```DAX

CALCULATE( [M_revenue], ALLEXCEPT(dim_calendar,dim_calendar[Year]))

```

### M_sales per rep

```DAX

[M_revenue]/DISTINCTCOUNT('dim_retail sales people'[Retail Sales People ID])

```

### M_sales per rep_MOM_card_kpi

```DAX

VAR current_month = MAX(dim_calendar[Date])
VAR previous_monht = EOMONTH(current_month,-1)

VAR sales_per_rep_current = CALCULATE([M_sales per rep], FILTER(dim_calendar, dim_calendar[Year] = YEAR(current_month) && dim_calendar[Month] = MONTH(current_month)))

VAR sales_per_rep_previous = CALCULATE([M_sales per rep], FILTER(ALL(dim_calendar),dim_calendar[Year] = YEAR(previous_monht) && dim_calendar[Month] = MONTH(previous_monht)))

RETURN IF(ISBLANK( sales_per_rep_previous),0, (sales_per_rep_current - sales_per_rep_previous)/sales_per_rep_previous)

```

### M_sales_per_rep_current

```DAX

VAR current_month = MAX(dim_calendar[Date]) -- Thang cuoi cung ma minh hien thi ra

VAR sales_per_rep_current = CALCULATE([M_sales per rep], FILTER(dim_calendar, dim_calendar[Year] = YEAR(current_month) && dim_calendar[Month] = MONTH(current_month)))

RETURN sales_per_rep_current

```


---

## Returns & Quality

### M_Return Rate Δ Abs

```DAX

ABS([M_Return Rate Δ vs Overall])

```

### M_Return Rate Δ vs Overall

```DAX

[M_return rate] - [M_return rate_all_sales rep]

```

### M_icon_return_rate_mom

```DAX

VAR positive = UNICHAR(9650)
VAR negative = UNICHAR(9660)
VAR change_mom = [M_return_rate_MOM_card_kpi]

VAR icon = IF ( change_mom >= 0, positive, negative )
VAR result = icon & " " & FORMAT ( change_mom, "0.0%" )

RETURN
   result

```

### M_return order

```DAX

CALCULATE(
   DISTINCTCOUNT( fact_retail_order[Order ID] ),
   dim_return[Returned] = "Returned")

```

### M_return rate

```DAX

[M_return order]/DISTINCTCOUNT(fact_retail_order[Order ID])

```

### M_return rate_all_sales rep

```DAX

CALCULATE( [M_return rate], ALL('dim_retail sales people'))

```

### M_return rate_all_sub category

```DAX

CALCULATE(
   [M_Return Rate],
   REMOVEFILTERS('dim_sub-category')
)

```

### M_return_rate_MOM_card_kpi

```DAX

VAR current_month = MAX ( dim_calendar[Date] )
VAR previous_month = EOMONTH ( current_month, -1 )

VAR return_rate_current =
   CALCULATE (
       [M_return rate],
       FILTER (
           dim_calendar,
           dim_calendar[Year] = YEAR ( current_month )
               && dim_calendar[Month] = MONTH ( current_month )
       )
   )

VAR return_rate_previous =
   CALCULATE (
       [M_return rate],
       FILTER (
           ALL ( dim_calendar ),
           dim_calendar[Year] = YEAR ( previous_month )
               && dim_calendar[Month] = MONTH ( previous_month )
       )
   )

RETURN
   IF (
       ISBLANK ( return_rate_previous ),
       0,
       ( [M_return_rate_current] - return_rate_previous )
           / return_rate_previous
   )

```

### M_return_rate_current

```DAX

VAR current_month = MAX(dim_calendar[Date]) -- Thang cuoi cung ma minh hien thi ra

VAR cancellation_rate_current = CALCULATE([M_return rate], FILTER(dim_calendar, dim_calendar[Year] = YEAR(current_month) && dim_calendar[Month] = MONTH(current_month)))

RETURN cancellation_rate_current

```


---

## Delivery & Logistics

### M_avg delivery days

```DAX

AVERAGE(fact_retail_order[Days])

```

### M_avg delivery days Δ vs Overall

```DAX

[M_avg delivery days] - [M_avg delivery days_all_sales rep]

```

### M_avg delivery days_all_sales rep

```DAX

CALCULATE( [M_avg delivery days], ALL('dim_retail sales people'))

```

### M_avg_delivery_days_MOM_card_kpi

```DAX

VAR current_month = MAX ( dim_calendar[Date] )
VAR previous_month = EOMONTH ( current_month, -1 )

VAR avg_cycle_current =
   CALCULATE (
       [M_avg delivery days],
       FILTER (
           dim_calendar,
           dim_calendar[Year] = YEAR ( current_month )
               && dim_calendar[Month] = MONTH ( current_month )
       )
   )

VAR avg_cycle_previous =
   CALCULATE (
       [M_avg delivery days],
       FILTER (
           ALL ( dim_calendar ),
           dim_calendar[Year] = YEAR ( previous_month )
               && dim_calendar[Month] = MONTH ( previous_month )
       )
   )

RETURN
   IF (
       ISBLANK ( avg_cycle_previous ),
       0,
       ( avg_cycle_current - avg_cycle_previous )
   )

```

### M_avg_delivery_days_current

```DAX

VAR current_month =
   MAX ( dim_calendar[Date] )
VAR avg_sales_cycle_current =
   CALCULATE (
       [M_avg delivery days],
       FILTER (
           dim_calendar,
           dim_calendar[Year] = YEAR ( current_month )
               && dim_calendar[Month] = MONTH ( current_month )
       )
   )
RETURN
   avg_sales_cycle_current

```

### M_icon_avg_delivery_days_mom

```DAX

VAR positive = UNICHAR(9650)
VAR negative = UNICHAR(9660)
VAR change_mom = [M_avg_delivery_days_MOM_card_kpi]

VAR icon = IF ( change_mom >= 0, positive, negative )
VAR result = icon & " " & FORMAT(change_mom,"0.00") & " days"

RETURN
   result

```


---

## Time Intelligence

### M_icon_total_orders_mom

```DAX

VAR positive = UNICHAR(9650)   -- ▲
VAR negative = UNICHAR(9660)   -- ▼
VAR change_mom = [M_total_orders_MOM_card_kpi]

VAR icon = IF ( change_mom >= 0, positive, negative )
VAR result = icon & " " & FORMAT ( change_mom, "0.0%" )

RETURN
   result

```

### M_icon_win_orders_mom

```DAX

VAR positive = UNICHAR(9650)
VAR negative = UNICHAR(9660)
VAR change_mom = [M_win_orders_MOM_card_kpi]

VAR icon = IF ( change_mom >= 0, positive, negative )
VAR result = icon & " " & FORMAT ( change_mom, "0.0%" )

RETURN
   result

```

### M_icon_win_rate_mom

```DAX

VAR positive = UNICHAR(9650)
VAR negative = UNICHAR(9660)
VAR change_mom = [M_win_rate_MOM_card_kpi]

VAR icon = IF ( change_mom >= 0, positive, negative )
VAR result = icon & " " & FORMAT ( change_mom, "0.0%" )

RETURN
   result

```

### M_total_orders_MOM_card_kpi

```DAX

VAR current_month = MAX ( dim_calendar[Date] )
VAR previous_month = EOMONTH ( current_month, -1 )

VAR total_orders_current =
   CALCULATE (
       [M_count order],
       FILTER (
           dim_calendar,
           dim_calendar[Year] = YEAR ( current_month )
               && dim_calendar[Month] = MONTH ( current_month )
       )
   )

VAR total_orders_previous =
   CALCULATE (
       [M_count order],
       FILTER (
           ALL ( dim_calendar ),
           dim_calendar[Year] = YEAR ( previous_month )
               && dim_calendar[Month] = MONTH ( previous_month )
       )
   )

RETURN
   IF (
       ISBLANK ( total_orders_previous ),
       0,
       ( total_orders_current - total_orders_previous )
           / total_orders_previous
   )

```

### M_win_orders_MOM_card_kpi

```DAX

VAR current_month = MAX ( dim_calendar[Date] )
VAR previous_month = EOMONTH ( current_month, -1 )

VAR win_orders_current =
   CALCULATE (
       [M_count completed orders],
       FILTER (
           dim_calendar,
           dim_calendar[Year] = YEAR ( current_month )
               && dim_calendar[Month] = MONTH ( current_month )
       )
   )

VAR win_orders_previous =
   CALCULATE (
       [M_count completed orders],
       FILTER (
           ALL ( dim_calendar ),
           dim_calendar[Year] = YEAR ( previous_month )
               && dim_calendar[Month] = MONTH ( previous_month )
       )
   )

RETURN
   IF (
       ISBLANK ( win_orders_previous ),
       0,
       ( win_orders_current - win_orders_previous )
           / win_orders_previous
   )

```

### M_win_rate_MOM_card_kpi

```DAX

VAR current_month = MAX ( dim_calendar[Date] )
VAR previous_month = EOMONTH ( current_month, -1 )

VAR win_rate_current =
   CALCULATE (
       [M_win rate],
       FILTER (
           dim_calendar,
           dim_calendar[Year] = YEAR ( current_month )
               && dim_calendar[Month] = MONTH ( current_month )
       )
   )

VAR win_rate_previous =
   CALCULATE (
       [M_win rate],
       FILTER (
           ALL ( dim_calendar ),
           dim_calendar[Year] = YEAR ( previous_month )
               && dim_calendar[Month] = MONTH ( previous_month )
       )
   )

RETURN
   IF (
       ISBLANK ( win_rate_previous ),
       0,
       ( win_rate_current - win_rate_previous )
           / win_rate_previous
   )

```


---

## Pareto & Concentration

### Pareto %

```DAX

IF (
   ISINSCOPE('dim_sub-category'[Sub-Category]),
   VAR selectedProductTypes = ALLSELECTED('dim_sub-category'[Sub-Category])
   VAR revenueByProductType = ADDCOLUMNS(selectedProductTypes, "@Revenue", [M_revenue])
   VAR currentProductRevenue = [M_revenue]
   VAR filteredHigherRevenue = FILTER(revenueByProductType, [@Revenue] >= currentProductRevenue)
   VAR cumulativeRevenue = SUMX(filteredHigherRevenue, [@Revenue])
   VAR totalSelectedRevenue = CALCULATE([M_revenue], selectedProductTypes)
   VAR paretoPercentage = DIVIDE(cumulativeRevenue, totalSelectedRevenue)
   RETURN paretoPercentage
)

```

### Pareto CF

```DAX

IF(
   [Pareto %] <= 'Parameter Threshold'[Parameter Threshold Value],
   "#4C8A8C",    -- Màu xanh: Trong ngưỡng Pareto (nhóm Top)
   "#CCCCCC"     -- Màu xám: Ngoài ngưỡng Pareto (nhóm Others)
)

```


---

## Customer Retention & Cohorts

### M_customer_retention_rate

```DAX

VAR currentMonthAfter = SELECTEDVALUE(month_after[Value])
VAR currentFirstMonthOrder = SELECTEDVALUE(dim_customer[First Date order (EOM)])
VAR totalInitialCustomers =
   CALCULATE(
       [M_total customers],
       FILTER(
           fact_retail_order,
           EOMONTH(fact_retail_order[Order Date], 0) = currentFirstMonthOrder
       )
   )
VAR retainedCustomers =
   CALCULATE(
       [M_total customers],
       FILTER(
           fact_retail_order,
           EOMONTH(fact_retail_order[order date], 0) = EOMONTH(currentFirstMonthOrder, currentMonthAfter)
       )
   )
RETURN
DIVIDE(retainedCustomers, totalInitialCustomers, 0)

```


---

## Other

### M_new_customers

```DAX

CALCULATE(
   COUNTROWS(fact_retail_order),
   fact_retail_order[Customer_type] = "new customer"
)

```

### M_products per customer

```DAX

[M_count_product]/[M_total customers]

```

### M_total customers

```DAX

COUNT(dim_customer[customer id])

```

### M_total_new_customers

```DAX

VAR _SelectedDate = SELECTEDVALUE(dim_calendar[Date])
RETURN
IF(
   NOT ISBLANK(_SelectedDate),
   CALCULATE(
       DISTINCTCOUNT(Customer_FirstPurchase[Customer ID]),
       FILTER(
           Customer_FirstPurchase,
           Customer_FirstPurchase[FirstPurchaseDate] = _SelectedDate
       )
   )
)

```

### M_unit sold

```DAX

SUM(fact_retail_order[quantity])

```

### M_win rate

```DAX

[M_count completed orders]/[M_count order]

```

### M_win_rate_current

```DAX

VAR current_month =
   MAX ( dim_calendar[Date] )
VAR win_rate_current =
   CALCULATE (
       [M_win rate],
       FILTER (
           dim_calendar,
           dim_calendar[Year] = YEAR ( current_month )
               && dim_calendar[Month] = MONTH ( current_month )
       )
   )
RETURN
   win_rate_current

```

### Parameter Threshold Value

```DAX

SELECTEDVALUE('Parameter Threshold'[Parameter Threshold], 0.8)

```
