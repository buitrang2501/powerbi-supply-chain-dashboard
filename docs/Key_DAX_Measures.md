# Key DAX Measures

This document lists the main DAX measures used in the Power BI report. It is generated from a DAX Studio export, so you can review logic, reuse measures, and track KPI definitions consistently across visuals.

Notes:
- Naming convention: measures typically start with `M_` (Metric).
- Some measures are helper/icon measures (e.g., `M_icon_*`) used for KPI cards and conditional formatting.
- Measure names with spaces are kept as-is to match the PBIX model.

---

-- ------------------------------------------------------------
-- SECTION: Sales & Profitability
-- ------------------------------------------------------------

-- Cost

```DAX
M_cost :=
    SUMX(fact_retail_order,fact_retail_order[Quantity]*fact_retail_order[Unit CP])
```
-- Icon Profit Margin MOM
```
M_icon_profit_margin_mom :=
    
    VAR positive = UNICHAR(9650)
    VAR negative = UNICHAR(9660)
    VAR change_mom = [M_profit_margin_MOM_card_kpi]
    
    VAR icon = IF ( change_mom >= 0, positive, negative )
    VAR result = icon & " " & FORMAT ( change_mom, "0.0%" )
    
    RETURN
       result
```
-- Icon Profit MOM
```
M_icon_profit_mom :=
    
    VAR positive = UNICHAR(9650)
    VAR negative = UNICHAR(9660)
    VAR change_mom = [M_profit_MOM_card_kpi]
    
    VAR icon = IF ( change_mom >= 0, positive, negative )
    VAR result = icon & " " & FORMAT ( change_mom, "0.0%" )
    
    RETURN
       result
```
-- Icon Revenue MOM
```
M_icon_revenue_mom :=
    
    VAR positive = UNICHAR(9650)
    VAR negative = UNICHAR(9660)
    VAR change_mom = [M_revenue_MOM_card_kpi]
    
    VAR icon = IF(change_mom >=0, positive, negative)
    VAR result = icon & " " & FORMAT(change_mom, "0.0%")
    
    RETURN result
```    
-- Profit
```
M_profit :=
    [M_revenue]-[M_cost]
```
-- Profit Margin
-- Format: 0.00%;-0.00%;0.00%
```
M_profit margin :=
    [M_profit]/[M_revenue]
```
-- Profit Accmulating
```
M_profit_accmulating :=
    CALCULATE([M_profit],dim_calendar[Date] <= MAX(dim_calendar[Date]))
```
-- Profit Current
M_profit_current :=
    
    
    VAR current_month = MAX(dim_calendar[Date]) -- Thang cuoi cung ma minh hien thi ra
    
    VAR profit_current = CALCULATE([M_profit], FILTER(dim_calendar, dim_calendar[Year] = YEAR(current_month) && dim_calendar[Month] = MONTH(current_month)))
    
    RETURN profit_current

-- Profit Margin Current
-- Format: 0.00%;-0.00%;0.00%
M_profit_margin_current :=
    
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

-- Profit Margin MOM Card KPI
M_profit_margin_MOM_card_kpi :=
    
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

-- Profit MOM Card KPI
M_profit_MOM_card_kpi :=
    
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

-- Profit Remove
M_profit_remove :=
    CALCULATE( [M_revenue]-[M_cost], ALLEXCEPT(dim_calendar,dim_calendar[Year]))

-- Revenue
M_revenue :=
    SUMX(fact_retail_order,fact_retail_order[Quantity]*fact_retail_order[Unit SP])

-- Revenue MOM Card KPI
-- Format: 0.00%;-0.00%;0.00%
M_revenue_MOM_card_kpi :=
    
    
    VAR current_month = MAX(dim_calendar[Date])
    VAR previous_monht = EOMONTH(current_month,-1)
    
    VAR revenue_current = CALCULATE([M_revenue], FILTER(dim_calendar, dim_calendar[Year] = YEAR(current_month) && dim_calendar[Month] = MONTH(current_month)))
    
    VAR revenue_previous = CALCULATE([M_revenue], FILTER(ALL(dim_calendar),dim_calendar[Year] = YEAR(previous_monht) && dim_calendar[Month] = MONTH(previous_monht)))
    
    RETURN IF(ISBLANK( revenue_previous),0, (revenue_current - revenue_previous)/revenue_previous)

-- Revenue QOQ
-- Format: 0.00%;-0.00%;0.00%
M_revenue_QoQ :=
    
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

-- Revenue QTD
M_revenue_QTD :=
    CALCULATE([M_revenue],DATESQTD(dim_calendar[Date]))

-- Revenue YTD
M_revenue_YTD :=
    CALCULATE([M_revenue],DATESYTD(dim_calendar[Date]))

-- ------------------------------------------------------------
-- SECTION: Revenue Share & Mix
-- ------------------------------------------------------------

-- % Revenue Product
-- Format: 0.00%;-0.00%;0.00%
M_% revenue product :=
    [M_revenue]/[M_revenue_all_product]

-- % Revenue Region
-- Format: 0.00%;-0.00%;0.00%
M_% revenue region :=
    [M_revenue]/[M_revenue_all_region]

-- % Revenue Year
-- Format: 0.00%;-0.00%;0.00%
M_%_revenue_year :=
    [M_revenue] / [M_revenue_remove]

-- Revenue Accmulating
M_revenue_accmulating :=
    CALCULATE([M_revenue],dim_calendar[Date] <= MAX(dim_calendar[Date]))

-- Revenue All Product
M_revenue_all_product :=
    CALCULATE( [M_revenue], ALL(dim_product))

-- Revenue All Region
M_revenue_all_region :=
    CALCULATE( [M_revenue], ALL(dim_address))

-- Revenue Current
M_revenue_current :=
    
    
    VAR current_month = MAX(dim_calendar[Date]) -- Thang cuoi cung ma minh hien thi ra
    
    VAR revenue_current = CALCULATE([M_revenue], FILTER(dim_calendar, dim_calendar[Year] = YEAR(current_month) && dim_calendar[Month] = MONTH(current_month)))
    
    RETURN revenue_current

-- Revenue Remove
-- Format: 0.00%;-0.00%;0.00%
M_revenue_remove :=
    CALCULATE( [M_revenue], ALLEXCEPT(dim_calendar,dim_calendar[Year]))

-- ------------------------------------------------------------
-- SECTION: Orders & Volume
-- ------------------------------------------------------------

-- Avg Revenue Per Order
M_avg revenue per order :=
    [M_revenue]/[M_count order]

-- Count Completed Orders
-- Format: #,0.00
M_count completed orders :=
    CALCULATE(DISTINCTCOUNT(fact_retail_order[Order ID]),dim_return[Returned] = "Completed")

-- Count Order
-- Format: #,0.00
M_count order :=
    DISTINCTCOUNT(fact_retail_order[order id])

-- Count Product
-- Format: 0
M_count_product :=
    COUNT(dim_product[Product ID])

-- Icon Total Orders MOM
M_icon_total_orders_mom :=
    
    VAR positive = UNICHAR(9650)   -- ▲
    VAR negative = UNICHAR(9660)   -- ▼
    VAR change_mom = [M_total_orders_MOM_card_kpi]
    
    VAR icon = IF ( change_mom >= 0, positive, negative )
    VAR result = icon & " " & FORMAT ( change_mom, "0.0%" )
    
    RETURN
       result

-- Icon Win Orders MOM
M_icon_win_orders_mom :=
    
    VAR positive = UNICHAR(9650)
    VAR negative = UNICHAR(9660)
    VAR change_mom = [M_win_orders_MOM_card_kpi]
    
    VAR icon = IF ( change_mom >= 0, positive, negative )
    VAR result = icon & " " & FORMAT ( change_mom, "0.0%" )
    
    RETURN
       result

-- Total Orders Current
-- Format: 0
M_total_orders_current :=
    
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

-- Total Orders MOM Card KPI
M_total_orders_MOM_card_kpi :=
    
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

-- Unit Sold
M_unit sold :=
    SUM(fact_retail_order[quantity])

-- Win Orders Current
-- Format: #,0
M_win_orders_current :=
    
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

-- Win Orders MOM Card KPI
M_win_orders_MOM_card_kpi :=
    
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

-- ------------------------------------------------------------
-- SECTION: Delivery Performance
-- ------------------------------------------------------------

-- Avg Delivery Days
-- Format: #,0.00
M_avg delivery days :=
    AVERAGE(fact_retail_order[Days])

-- Avg Delivery Days Delta Vs Overall
M_avg delivery days Δ vs Overall :=
    [M_avg delivery days] - [M_avg delivery days_all_sales rep]

-- Avg Delivery Days All Sales Rep
-- Format: 0.00%;-0.00%;0.00%
M_avg delivery days_all_sales rep :=
    CALCULATE( [M_avg delivery days], ALL('dim_retail sales people'))

-- Avg Delivery Days Current
M_avg_delivery_days_current :=
    
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

-- Avg Delivery Days MOM Card KPI
M_avg_delivery_days_MOM_card_kpi :=
    
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

-- Icon Avg Delivery Days MOM
M_icon_avg_delivery_days_mom :=
    
    VAR positive = UNICHAR(9650)
    VAR negative = UNICHAR(9660)
    VAR change_mom = [M_avg_delivery_days_MOM_card_kpi]
    
    VAR icon = IF ( change_mom >= 0, positive, negative )
    VAR result = icon & " " & FORMAT(change_mom,"0.00") & " days"
    
    RETURN
       result

-- ------------------------------------------------------------
-- SECTION: Returns
-- ------------------------------------------------------------

-- Icon Return Rate MOM
M_icon_return_rate_mom :=
    
    VAR positive = UNICHAR(9650)
    VAR negative = UNICHAR(9660)
    VAR change_mom = [M_return_rate_MOM_card_kpi]
    
    VAR icon = IF ( change_mom >= 0, positive, negative )
    VAR result = icon & " " & FORMAT ( change_mom, "0.0%" )
    
    RETURN
       result

-- Return Order
-- Format: #,0
M_return order :=
    CALCULATE(
       DISTINCTCOUNT( fact_retail_order[Order ID] ),
       dim_return[Returned] = "Returned")

-- Return Rate
-- Format: 0.00%;-0.00%;0.00%
M_return rate :=
    [M_return order]/DISTINCTCOUNT(fact_retail_order[Order ID])

-- Return Rate Delta Abs
M_Return Rate Δ Abs :=
    ABS([M_Return Rate Δ vs Overall])

-- Return Rate Delta Vs Overall
-- Format: 0.00%;-0.00%;0.00%
M_Return Rate Δ vs Overall :=
    [M_return rate] - [M_return rate_all_sales rep]

-- Return Rate All Sales Rep
-- Format: 0.00%;-0.00%;0.00%
M_return rate_all_sales rep :=
    CALCULATE( [M_return rate], ALL('dim_retail sales people'))

-- Return Rate All Sub Category
M_return rate_all_sub category :=
    
    CALCULATE(
       [M_Return Rate],
       REMOVEFILTERS('dim_sub-category')
    )

-- Return Rate Current
-- Format: 0.00%;-0.00%;0.00%
M_return_rate_current :=
    
    
    VAR current_month = MAX(dim_calendar[Date]) -- Thang cuoi cung ma minh hien thi ra
    
    VAR cancellation_rate_current = CALCULATE([M_return rate], FILTER(dim_calendar, dim_calendar[Year] = YEAR(current_month) && dim_calendar[Month] = MONTH(current_month)))
    
    RETURN cancellation_rate_current

-- Return Rate MOM Card KPI
M_return_rate_MOM_card_kpi :=
    
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

-- ------------------------------------------------------------
-- SECTION: Customer Metrics
-- ------------------------------------------------------------

-- % Revenue Customer
-- Format: 0.00%;-0.00%;0.00%
M_% revenue customer :=
    [M_revenue]/[M_revenue_all_customer]

-- Customer Retention Rate
-- Format: 0.00%;-0.00%;0.00%
M_customer_retention_rate :=
    
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

-- New Customers
-- Format: 0
M_new_customers :=
    
    CALCULATE(
       COUNTROWS(fact_retail_order),
       fact_retail_order[Customer_type] = "new customer"
    )

-- Orders Per Customer
M_orders per customer :=
    [M_count order]/[M_total customers]

-- Products Per Customer
M_products per customer :=
    [M_count_product]/[M_total customers]

-- Revenue Per Customer
M_revenue per customer :=
    [M_revenue]/[M_total customers]

-- Revenue All Customer
M_revenue_all_customer :=
    CALCULATE( [M_revenue], ALL(dim_customer))

-- Total Customers
-- Format: 0
M_total customers :=
    COUNT(dim_customer[customer id])

-- Total New Customers
-- Format: 0
M_total_new_customers :=
    
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

-- ------------------------------------------------------------
-- SECTION: Sales Rep Metrics
-- ------------------------------------------------------------

-- Icon Sales Per Rep MOM
M_icon_sales per rep_mom :=
    
    VAR positive = UNICHAR(9650)
    VAR negative = UNICHAR(9660)
    VAR change_mom = [M_sales per rep_MOM_card_kpi]
    
    VAR icon = IF(change_mom >=0, positive, negative)
    VAR result = icon & " " & FORMAT(change_mom, "0.0%")
    
    RETURN result

-- Sales Per Rep
M_sales per rep :=
    [M_revenue]/DISTINCTCOUNT('dim_retail sales people'[Retail Sales People ID])

-- Sales Per Rep MOM Card KPI
M_sales per rep_MOM_card_kpi :=
    
    
    VAR current_month = MAX(dim_calendar[Date])
    VAR previous_monht = EOMONTH(current_month,-1)
    
    VAR sales_per_rep_current = CALCULATE([M_sales per rep], FILTER(dim_calendar, dim_calendar[Year] = YEAR(current_month) && dim_calendar[Month] = MONTH(current_month)))
    
    VAR sales_per_rep_previous = CALCULATE([M_sales per rep], FILTER(ALL(dim_calendar),dim_calendar[Year] = YEAR(previous_monht) && dim_calendar[Month] = MONTH(previous_monht)))
    
    RETURN IF(ISBLANK( sales_per_rep_previous),0, (sales_per_rep_current - sales_per_rep_previous)/sales_per_rep_previous)

-- Sales Per Rep Current
M_sales_per_rep_current :=
    
    
    VAR current_month = MAX(dim_calendar[Date]) -- Thang cuoi cung ma minh hien thi ra
    
    VAR sales_per_rep_current = CALCULATE([M_sales per rep], FILTER(dim_calendar, dim_calendar[Year] = YEAR(current_month) && dim_calendar[Month] = MONTH(current_month)))
    
    RETURN sales_per_rep_current

-- ------------------------------------------------------------
-- SECTION: Pareto (80-20) for Sub-category
-- ------------------------------------------------------------

-- Pareto %
-- Format: 0.00%;-0.00%;0.00%
Pareto % :=
    
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

-- Pareto CF
Pareto CF :=
    
    IF(
       [Pareto %] <= 'Parameter Threshold'[Parameter Threshold Value],
       "#4C8A8C",    -- Màu xanh: Trong ngưỡng Pareto (nhóm Top)
       "#CCCCCC"     -- Màu xám: Ngoài ngưỡng Pareto (nhóm Others)
    )

-- ------------------------------------------------------------
-- SECTION: Parameters
-- ------------------------------------------------------------

-- Parameter Threshold Value
Parameter Threshold Value :=
    SELECTEDVALUE('Parameter Threshold'[Parameter Threshold], 0.8)

-- ------------------------------------------------------------
-- SECTION: Other / Helpers
-- ------------------------------------------------------------

-- Icon Win Rate MOM
M_icon_win_rate_mom :=
    
    VAR positive = UNICHAR(9650)
    VAR negative = UNICHAR(9660)
    VAR change_mom = [M_win_rate_MOM_card_kpi]
    
    VAR icon = IF ( change_mom >= 0, positive, negative )
    VAR result = icon & " " & FORMAT ( change_mom, "0.0%" )
    
    RETURN
       result

-- Win Rate
-- Format: 0.00%;-0.00%;0.00%
M_win rate :=
    [M_count completed orders]/[M_count order]

-- Win Rate Current
-- Format: 0.00%;-0.00%;0.00%
M_win_rate_current :=
    
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

-- Win Rate MOM Card KPI
M_win_rate_MOM_card_kpi :=
    
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
