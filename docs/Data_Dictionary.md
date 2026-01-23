# Data Dictionary

This document describes:
1) The **raw dataset fields** (Excel file from the challenge source)  
2) The **modeled star schema tables** used in the Power BI report (dim/fact)

---

## 1) Raw Dataset Fields (Excel)

> Grain: one row per order line / transaction (as provided by the challenge dataset)

### Identifiers
- **Retail Order ID**: Unique retail order identifier  
- **Order ID**: General order identifier  

### Dates
- **Order Date**: Date the order was placed  
- **Ship Date**: Date the order was shipped  

### Shipping / Logistics
- **Ship Mode**: Shipping method (Standard, Express, Same-Day)  
- **Days**: Actual delivery days (delivery lead time)  

### Customer
- **Customer ID**: Unique customer identifier  
- **Customer Name**: Full customer name  
- **Segment**: Customer segment (Consumer, Corporate, Home Office)  
- **Postal Code**: Postal / Zip code  
- **Country**: Country  
- **City**: City  
- **State**: State / Province  
- **Region**: Sales region  

### Geography (Coordinates)
- **Latitude**: Latitude  
- **Longitude**: Longitude  

### Product
- **Product ID**: Unique product identifier  
- **Category**: Product category  
- **Sub-Category**: Product sub-category  
- **Product Name**: Product name  

### Sales / Finance
- **Sales**: Revenue generated from the order  
- **Quantity**: Units sold  
- **Discount**: Discount rate or amount (depends on dataset definition)  
- **Profit**: Profit from the order  
- **Cost**: Original cost of the order  
- **Unit CP**: Unit cost price  
- **Unit SP**: Unit selling price  

### Returns
- **Returned**: Return status (Yes/No)

### Sales People
- **Retail Sales People**: Sales representative / assigned retail salesperson

---

## 2) Star Schema Tables (Power BI Model)

> The model is organized to support fast slicing by time, product, customer, and geography.

### Fact Table
**fact_retail_order** (transaction grain)
- Keys (typical): DateKey / ProductKey / CustomerKey / AddressKey / ShipModeKey / SalesPeopleKey / ReturnKey
- Measures (typical columns): Sales, Profit, Cost, Quantity, Discount, Days (delivery days)
- Flags: Returned (or ReturnKey mapping)

### Dimension Tables
**dim_calendar**
- Date, Year, Quarter, Month, MonthName, YearMonth (and other time attributes)

**dim_product**
- Product ID, Product Name
- Links to category/sub-category dims (if separated)

**dim_category**
- Category

**dim_sub-category**
- Sub-Category

**dim_customer_segment**
- Segment (Consumer, Corporate, Home Office)

**dim_address**
- Region, State, City, Postal Code, Country
- Latitude, Longitude (if kept at this level)

**dim_ship_mode**
- Ship Mode

**dim_return**
- Returned status (Yes/No or Returned/Not Returned)

**dim_retail_sales_people**
- Retail Sales People

---

## 3) Notes on IDs / Surrogate Keys
Some models add an internal **surrogate key** for dimension tables (e.g., CustomerKey, ProductKey).  
If you created IDs during Power Query:
- Keep keys stable (avoid re-numbering after refresh)
- Prefer deterministic keys where possible (e.g., hash of business key) if dataset changes over time

---

## 4) Where This Dictionary Is Used
- Refresh validation (ensure raw fields exist)
- Review of the model diagram (assets)
- Alignment with DAX measures in `docs/Key_DAX_Measures.md`
