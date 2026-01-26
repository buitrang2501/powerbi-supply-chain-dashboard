# Data Model Notes

## Design Choice

The report uses a **star-like** model to support fast slicing and interactive drill-down.  
A small **snowflake** is used for Product (Category/Sub-category) to keep hierarchies clean.

## Fact Table

**fact_retail_order**  
- **Grain:** order line (1 row per `Retail Order ID`)
- Includes order IDs, dates, financial metrics, delivery days, return flag, and dimension keys.

## Dimensions (typical)

- **dim_calendar** (Date)
- **dim_product**, **dim_category**, **dim_sub_category**
- **dim_customer** / **dim_customer_segment**
- **dim_address** (Region/State/City)
- **dim_ship_mode**
- **dim_return**
- **dim_retail_sales_people**

## Relationship Notes

- Filter direction is generally **single** from dimensions → fact.
- Time intelligence uses `dim_calendar[Date]` as the primary date table.
