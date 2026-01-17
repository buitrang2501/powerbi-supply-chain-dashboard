# Key DAX Measures (Power BI)

This document lists the **core DAX measures** used in the *Supply Chain & Sales Performance Dashboard*.  
Measures are organized by purpose and written in a reusable, GitHub-friendly format.

> **Naming convention:** `M_` prefix for measures.  
> **Model assumption:** fact table = `fact_retail_order`, date table = `dim_calendar`.  
> If your column/table names differ, replace accordingly.

---

## Table of Contents
- [1. Executive KPIs](#1-executive-kpis)
- [2. Return & Quality Metrics](#2-return--quality-metrics)
- [3. Delivery Metrics](#3-delivery-metrics)
- [4. "Overall" Benchmark (Remove Sales Rep Filter)](#4-overall-benchmark-remove-sales-rep-filter)
- [5. Time Intelligence (Month / Quarter / Year)](#5-time-intelligence-month--quarter--year)
- [6. Pareto Analysis (80/20)](#6-pareto-analysis-8020)
- [7. Customer Metrics & Retention](#7-customer-metrics--retention)
- [8. Utility Measures](#8-utility-measures)
- [9. Notes & Best Practices](#9-notes--best-practices)

---

## 1. Executive KPIs

### Total Orders

M_total_orders :=
DISTINCTCOUNT ( fact_retail_order[Retail Order ID] )

// Revenue
M_revenue :=
SUM ( fact_retail_order[Sales] )

// Revenue (Overall - remove Sales Rep only)
M_revenue_remove :=
CALCULATE (
    [M_revenue],
    REMOVEFILTERS ( dim_retail_sales_people[Retail Sales People] )
)

// Revenue YTD
M_revenue_YTD :=
TOTALYTD ( [M_revenue], dim_calendar[Date] )

// Revenue QTD
M_revenue_QTD :=
TOTALQTD ( [M_revenue], dim_calendar[Date] )

// Revenue QoQ (based on QTD)
M_revenue_QoQ :=
VAR CurrQ = [M_revenue_QTD]
VAR PrevQ =
    CALCULATE ( [M_revenue_QTD], DATEADD ( dim_calendar[Date], -1, QUARTER ) )
RETURN
    DIVIDE ( CurrQ - PrevQ, PrevQ )

// % Revenue by Year (within current filters)
M_%_revenue_year :=
DIVIDE (
    [M_revenue],
    CALCULATE ( [M_revenue], ALL ( dim_calendar[Year] ) )
)

// % Revenue by Region
M_% revenue region :=
DIVIDE (
    [M_revenue],
    CALCULATE ( [M_revenue], ALL ( dim_address[Region] ) )
)

// % Revenue by Product (Category level)
M_% revenue product :=
DIVIDE (
    [M_revenue],
    CALCULATE ( [M_revenue], ALL ( dim_category[Category] ) )
)

// % Revenue by Customer Segment
M_% revenue customer :=
DIVIDE (
    [M_revenue],
    CALCULATE ( [M_revenue], ALL ( dim_customer_segment[Segment] ) )
)

// Profit
M_profit :=
SUM ( fact_retail_order[Profit] )

// Profit (Overall - remove Sales Rep only)
M_profit_remove :=
CALCULATE (
    [M_profit],
    REMOVEFILTERS ( dim_retail_sales_people[Retail Sales People] )
)

// Profit Margin
M_profit margin :=
DIVIDE ( [M_profit], [M_revenue] )

// Cost
M_cost :=
SUM ( fact_retail_order[Cost] )

// Unit Sold
M_unit sold :=
SUM ( fact_retail_order[Quantity] )

// Total Orders
M_count order :=
DISTINCTCOUNT ( fact_retail_order[Retail Order ID] )

// Count Product (distinct product)
M_count_product :=
DISTINCTCOUNT ( fact_retail_order[Product ID] )

// Returned Orders
M_return orders :=
CALCULATE (
    DISTINCTCOUNT ( fact_retail_order[Retail Order ID] ),
    fact_retail_order[Returned] = "Yes"
)

// Completed Orders (non-return)
M_count completed orders :=
CALCULATE (
    DISTINCTCOUNT ( fact_retail_order[Retail Order ID] ),
    fact_retail_order[Returned] <> "Yes"
)

// Return Rate
M_return rate :=
DIVIDE ( [M_return orders], [M_count order] )

// Return Rate (Overall - remove Sales Rep only)
M_return_rate_remove :=
CALCULATE (
    [M_return rate],
    REMOVEFILTERS ( dim_retail_sales_people[Retail Sales People] )
)

// Return Rate Delta vs Overall
M_return rate Δ vs Overall :=
[M_return rate] - [M_return_rate_remove]

// Win Rate (Non-return Rate)
M_win rate :=
1 - [M_return rate]

// Avg Delivery Days
M_avg delivery days :=
AVERAGE ( fact_retail_order[Days] )

// Avg Delivery Days (Overall - remove Sales Rep only)
M_avg_delivery_days_all_sales rep :=
CALCULATE (
    [M_avg delivery days],
    REMOVEFILTERS ( dim_retail_sales_people[Retail Sales People] )
)

// Avg Delivery Days Delta vs Overall
M_avg delivery days Δ vs Overall :=
[M_avg delivery days] - [M_avg_delivery_days_all_sales rep]

// Avg Revenue per Order
M_avg revenue per order :=
DIVIDE ( [M_revenue], [M_count order] )

// Total Customers
M_total customers :=
DISTINCTCOUNT ( dim_customer[Customer ID] )

// Orders per Customer
M_orders per customer :=
DIVIDE ( [M_count order], [M_total customers] )

// Products per Customer
M_products per customer :=
DIVIDE ( [M_unit sold], [M_total customers] )

// Sales per rep (Revenue under current Sales Rep filter context)
M_sales per rep :=
[M_revenue]

// New Customers (alias - based on first purchase date)
M_new_customers :=
CALCULATE (
    DISTINCTCOUNT ( Customer_FirstPurchase[Customer ID] ),
    KEEPFILTERS ( VALUES ( Customer_FirstPurchase[FirstPurchaseDate] ) )
)

// Total New Customers (recommended: new customers whose FirstPurchaseDate is within current date selection)
M_total_new_customers :=
VAR MinD = MIN ( dim_calendar[Date] )
VAR MaxD = MAX ( dim_calendar[Date] )
RETURN
    CALCULATE (
        DISTINCTCOUNT ( Customer_FirstPurchase[Customer ID] ),
        FILTER (
            ALL ( Customer_FirstPurchase[FirstPurchaseDate] ),
            Customer_FirstPurchase[FirstPurchaseDate] >= MinD
                && Customer_FirstPurchase[FirstPurchaseDate] <= MaxD
        )
    )

// Current (latest date in current filter context) - Revenue
M_revenue_current :=
VAR MaxDate = MAX ( dim_calendar[Date] )
RETURN
    CALCULATE ( [M_revenue], dim_calendar[Date] = MaxDate )

// Current (latest date in current filter context) - Profit
M_profit_current :=
VAR MaxDate = MAX ( dim_calendar[Date] )
RETURN
    CALCULATE ( [M_profit], dim_calendar[Date] = MaxDate )

// Current (latest date in current filter context) - Profit Margin
M_profit_margin_current :=
VAR MaxDate = MAX ( dim_calendar[Date] )
RETURN
    CALCULATE ( [M_profit margin], dim_calendar[Date] = MaxDate )

// Current (latest date in current filter context) - Avg Delivery Days
M_avg_delivery_days_current :=
VAR MaxDate = MAX ( dim_calendar[Date] )
RETURN
    CALCULATE ( [M_avg delivery days], dim_calendar[Date] = MaxDate )

// Total Orders Current (latest date in current filter context)
M_total_orders_current :=
VAR MaxDate = MAX ( dim_calendar[Date] )
RETURN
    CALCULATE ( [M_count order], dim_calendar[Date] = MaxDate )

// Win Orders Current (latest date in current filter context)
M_win_orders_current :=
VAR MaxDate = MAX ( dim_calendar[Date] )
RETURN
    CALCULATE ( [M_count completed orders], dim_calendar[Date] = MaxDate )

// Win Rate Current (latest date in current filter context)
M_win_rate_current :=
VAR MaxDate = MAX ( dim_calendar[Date] )
RETURN
    CALCULATE ( [M_win rate], dim_calendar[Date] = MaxDate )

// Profit Accumulating (YTD)
M_profit_accmulating :=
TOTALYTD ( [M_profit], dim_calendar[Date] )

// Sales per rep current (latest date in current filter context)
M_sales_per_rep_current :=
VAR MaxDate = MAX ( dim_calendar[Date] )
RETURN
    CALCULATE ( [M_sales per rep], dim_calendar[Date] = MaxDate )

// KPI MoM - Revenue
M_revenue_MOM_card_kpi :=
VAR Curr = [M_revenue]
VAR Prev = CALCULATE ( [M_revenue], DATEADD ( dim_calendar[Date], -1, MONTH ) )
RETURN
    DIVIDE ( Curr - Prev, Prev )

// KPI MoM - Profit
M_profit_MOM_card_kpi :=
VAR Curr = [M_profit]
VAR Prev = CALCULATE ( [M_profit], DATEADD ( dim_calendar[Date], -1, MONTH ) )
RETURN
    DIVIDE ( Curr - Prev, Prev )

// KPI MoM - Profit Margin (delta)
M_profit_margin_MOM_card_kpi :=
VAR Curr = [M_profit margin]
VAR Prev = CALCULATE ( [M_profit margin], DATEADD ( dim_calendar[Date], -1, MONTH ) )
RETURN
    Curr - Prev

// KPI MoM - Total Orders
M_total_orders_MOM_card_kpi :=
VAR Curr = [M_count order]
VAR Prev = CALCULATE ( [M_count order], DATEADD ( dim_calendar[Date], -1, MONTH ) )
RETURN
    DIVIDE ( Curr - Prev, Prev )

// KPI MoM - Avg Delivery Days (delta)
M_avg_delivery_days_MOM_card_kpi :=
VAR Curr = [M_avg delivery days]
VAR Prev = CALCULATE ( [M_avg delivery days], DATEADD ( dim_calendar[Date], -1, MONTH ) )
RETURN
    Curr - Prev

// KPI MoM - Sales per rep
M_sales per rep_MOM_card_kpi :=
VAR Curr = [M_sales per rep]
VAR Prev = CALCULATE ( [M_sales per rep], DATEADD ( dim_calendar[Date], -1, MONTH ) )
RETURN
    DIVIDE ( Curr - Prev, Prev )

// KPI MoM - Win Orders
M_win_orders_MOM_card_kpi :=
VAR Curr = [M_count completed orders]
VAR Prev = CALCULATE ( [M_count completed orders], DATEADD ( dim_calendar[Date], -1, MONTH ) )
RETURN
    DIVIDE ( Curr - Prev, Prev )

// KPI MoM - Win Rate (delta)
M_win_rate_MOM_card_kpi :=
VAR Curr = [M_win rate]
VAR Prev = CALCULATE ( [M_win rate], DATEADD ( dim_calendar[Date], -1, MONTH ) )
RETURN
    Curr - Prev

// Icons (use UNICHAR to avoid font issues in copy/paste)
M_icon_revenue_mom :=
IF ( [M_revenue_MOM_card_kpi] >= 0, UNICHAR ( 9650 ), UNICHAR ( 9660 ) )

M_icon_profit_mom :=
IF ( [M_profit_MOM_card_kpi] >= 0, UNICHAR ( 9650 ), UNICHAR ( 9660 ) )

M_icon_profit_margin_mom :=
IF ( [M_profit_margin_MOM_card_kpi] >= 0, UNICHAR ( 9650 ), UNICHAR ( 9660 ) )

M_icon_total_orders_mom :=
IF ( [M_total_orders_MOM_card_kpi] >= 0, UNICHAR ( 9650 ), UNICHAR ( 9660 ) )

M_icon_sales per rep_mom :=
IF ( [M_sales per rep_MOM_card_kpi] >= 0, UNICHAR ( 9650 ), UNICHAR ( 9660 ) )

M_icon_return_rate_mom :=
IF ( [M_return rate] <= CALCULATE ( [M_return rate], DATEADD ( dim_calendar[Date], -1, MONTH ) ), UNICHAR ( 9650 ), UNICHAR ( 9660 ) )

M_icon_avg_delivery_days_mom :=
IF ( [M_avg_delivery_days_MOM_card_kpi] <= 0, UNICHAR ( 9650 ), UNICHAR ( 9660 ) )

M_icon_win_orders_mom :=
IF ( [M_win_orders_MOM_card_kpi] >= 0, UNICHAR ( 9650 ), UNICHAR ( 9660 ) )

M_icon_win_rate_mom :=
IF ( [M_win_rate_MOM_card_kpi] >= 0, UNICHAR ( 9650 ), UNICHAR ( 9660 ) )

// Parameter Threshold (What-if parameter)
Parameter Threshold Value :=
SELECTEDVALUE ( 'Parameter Threshold'[Parameter Threshold Value], 0.8 )

// Pareto (80-20) for Sub-category - cumulative fraction
Pareto CF :=
VAR CurrentSub = SELECTEDVALUE ( 'dim_sub-category'[Sub-Category] )
VAR CurrentRank =
    RANKX (
        ALLSELECTED ( 'dim_sub-category'[Sub-Category] ),
        [M_revenue],
        ,
        DESC,
        Dense
    )
VAR TotalRevenue =
    CALCULATE ( [M_revenue], ALLSELECTED ( 'dim_sub-category'[Sub-Category] ) )
VAR CumRevenue =
    SUMX (
        FILTER (
            ADDCOLUMNS (
                ALLSELECTED ( 'dim_sub-category'[Sub-Category] ),
                "Rank", RANKX ( ALLSELECTED ( 'dim_sub-category'[Sub-Category] ), [M_revenue], , DESC, Dense ),
                "Rev", CALCULATE ( [M_revenue] )
            ),
            [Rank] <= CurrentRank
        ),
        [Rev]
    )
RETURN
    DIVIDE ( CumRevenue, TotalRevenue )

// Pareto (80-20) for Sub-category - percentage
Pareto % :=
[Pareto CF]

// Customer Retention (Cohort)
M_customer_retention_rate :=
VAR CohortMonth = SELECTEDVALUE ( Customer_FirstPurchase[FirstPurchaseDate] )
VAR K = SELECTEDVALUE ( month_after[Value], 0 )
VAR CohortCustomers =
    CALCULATETABLE (
        VALUES ( Customer_FirstPurchase[Customer ID] ),
        Customer_FirstPurchase[FirstPurchaseDate] = CohortMonth
    )
VAR StartDate = EDATE ( CohortMonth, K )
VAR EndDate = EOMONTH ( StartDate, 0 )
VAR ActiveCustomers =
    CALCULATE (
        DISTINCTCOUNT ( fact_retail_order[Customer ID] ),
        TREATAS ( CohortCustomers, fact_retail_order[Customer ID] ),
        DATESBETWEEN ( dim_calendar[Date], StartDate, EndDate )
    )
VAR CohortSize = COUNTROWS ( CohortCustomers )
RETURN
    DIVIDE ( ActiveCustomers, CohortSize )
