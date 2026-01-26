# Data Dictionary

This document describes:
1) the **raw dataset fields** (Excel file from the challenge source), and  
2) the **modeled tables** used in the Power BI report (fact/dim schema).

## 1) Raw Dataset Fields (Excel)

**Grain:** one row per **order line / transaction** (as provided by the challenge dataset).

### Identifiers
- **Retail Order ID**: unique order-line identifier *(one row per line item)*  
- **Order ID**: general order identifier *(may repeat across multiple line items)*

### Dates
- **Order Date**: date the order was placed  
- **Ship Date**: date the order was shipped

### Shipping / Logistics
- **Ship Mode**: shipping method (Standard, Express, Same-Day)  
- **Days**: actual delivery days (lead time)

### Customer
- **Customer ID**, **Customer Name**
- **Segment**: Consumer / Corporate / Home Office
- **Postal Code**, **City**, **State**, **Region**, **Country**

### Geography (Coordinates)
- **Latitude**, **Longitude**

### Product
- **Product ID**, **Product Name**
- **Category**, **Sub-Category**

### Sales / Finance
- **Sales**, **Quantity**, **Discount**
- **Profit**, **Cost**
- **Unit CP** (cost price), **Unit SP** (selling price)

### Returns
- **Returned**: Yes/No

### Sales People
- **Retail Sales People**: assigned sales representative

## 2) Power BI Model Tables (Fact/Dim)

> The model is designed for fast slicing by **time, product, customer, geography, ship mode, and sales rep**.

### Fact Table

**fact_retail_order** *(order-line grain)*  
- Typical numeric columns: `Sales`, `Profit`, `Cost`, `Quantity`, `Discount`, `Days`  
- Typical business keys: `Retail Order ID`, `Order ID`, `Order Date`, `Ship Date`  
- Links to dimensions via business keys (or surrogate keys if you created them in Power Query)

### Dimension Tables (examples)

- **dim_calendar**: Date, Year, Quarter, Month, YearMonth
- **dim_product**: Product ID, Product Name  
  - optionally snowflaked into: **dim_category**, **dim_sub_category**
- **dim_customer** / **dim_customer_segment**: Customer + Segment
- **dim_address**: Region, State, City, Postal Code, Country (+ Latitude/Longitude if kept here)
- **dim_ship_mode**: Ship Mode
- **dim_return**: Returned status (Yes/No)
- **dim_retail_sales_people**: Sales Rep

## 3) Derived Columns (Created in Power Query / DAX)

Depending on your implementation, common derived fields include:

- **YearMonth**: for monthly trending
- **Delivery Bucket**: e.g., 0–2, 3–5, 6–8, 9–14, 15+ days
- **FirstPurchaseMonth / CohortMonth**: for retention cohorts
- **MonthIndex**: number of months since cohort month

## 4) Notes on IDs / Surrogate Keys

If you created IDs during Power Query:
- keep keys **stable** (avoid re-numbering after refresh)
- prefer deterministic keys where possible (e.g., based on business key)
