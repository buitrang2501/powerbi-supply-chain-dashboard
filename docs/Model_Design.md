# Model Design

## Grain
- 1 row per Retail Order ID (order-level)

## Fact
fact_retail_order:
- measures: Sales, Profit, Cost, Discount, Quantity, Days, Returned
- keys: Date, Customer, Product, Address/Geo, Ship Mode, Sales Rep

## Notes
- Single direction filters from dimensions → fact
- dim_calendar marked as Date table
- Prefer star schema (avoid snowflake unless required)
