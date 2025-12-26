# Model Notes (Short)

## Why this model structure?
This project uses a fact-centered (star-like) model built for interactive BI reporting at **order level** (1 row per Retail Order ID).  
Key operational fields:
- `Days` = **Actual delivery days**
- `Returned` = Yes/No (used for Return Rate)

## Light snowflake (intentional)
A few attributes are modeled as separate lookup tables (e.g., Category/Sub-category, Customer Segment).  
This keeps hierarchies explicit and simplifies field selection in some visuals. The trade-off is slightly higher relationship complexity than a pure star schema.

## Relationship principles
- Dimensions filter the fact table (single direction) wherever possible.
- `dim_calendar` is marked as the Date table for MoM/QoQ/YTD measures.
- Geography (Region/State/City) is sourced from the geography dimension to avoid ambiguity.

## Helper table for retention
`Customer_FirstPurchase` is a derived helper table to support retention/cohort views and is linked by Customer ID.

## Planned improvement (non-breaking)
A future enhancement is to consolidate lookup tables into core dimensions (e.g., put Category/Sub-category inside dim_product; Segment inside dim_customer).  
If refactoring is needed, it should be done via staged migration (new tables in parallel → migrate visuals → validate KPI totals → remove legacy tables).
