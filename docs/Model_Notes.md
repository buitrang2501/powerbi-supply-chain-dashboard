# Model Notes (Power BI Data Model)

## 1) Purpose of the model
This Power BI model is designed to support interactive supply chain & sales analytics at **order level** (1 row per Retail Order ID) with drill-down by:
- Product (Category / Sub-category / Product)
- Customer (Customer / Segment)
- Geography (Region / State / City)
- Ship mode
- Sales representative
- Time (Date)

Key operational metrics include Revenue, Profit, Profit Margin, **Return Rate** (Returned = Yes/No), and **Avg Delivery Time (Days)** where `Days` represents **actual delivery days**.

---

## 2) Current schema overview
The model follows a **fact-centered design** (star-like schema) with a single main fact table:

### Fact table
- **fact_retail_order**
  - Measures: Sales, Profit, Cost, Discount, Quantity, Days, Returned
  - Keys: Date, Customer, Product, Address/Geo, Ship Mode, Sales Rep

### Dimension tables
- **dim_calendar**: Date table for time intelligence
- **dim_product**: Product ID, Product Name, Sub-category ID, Category ID
- **dim_category** + **dim_sub-category**: Lookup tables for product hierarchy
- **dim_customer** + **dim_customer_segment**: Customer attributes and segment
- **dim_address**: Country, Region, State, City, Postal Code, Latitude/Longitude
- **dim_ship_mode**: Standard / Express / Same-Day
- **dim_retail_sales_people**: Sales representative attributes
- **Customer_FirstPurchase**: Helper table for retention/cohort analysis

The model is intentionally built to support:
- Multi-page dashboards (Overview/Product/Customer/Sales Rep/Region)
- Drill-through tooltips by product/customer/region
- Retention/cohort views using first purchase date

---

## 3) Why some dimensions are “snowflaked”
Some attributes are normalized into separate lookup tables (e.g., Category/Sub-category and Customer Segment). This introduces a light **snowflake** pattern instead of a pure star schema.

### Reasoning (current trade-offs)
- The dataset originally provides hierarchical attributes (Category/Sub-category) that were modeled as separate tables to keep product hierarchy explicit.
- Segment and Return status are modeled separately for readability and easier filtering in visuals.

### Impact
- The current structure works well for interactive reporting but may slightly increase model complexity (more relationships, more places to manage fields).

---

## 4) Relationship & filtering principles
To ensure consistent and stable aggregations:
- Dimension tables filter the fact table (single direction) wherever possible.
- `dim_calendar` is marked as the Date table for MoM/QoQ/YTD measures.
- The grain of the model remains at **order level** (Retail Order ID).

---

## 5) Known limitations / improvement areas
While the model supports the current report effectively, the following are planned improvements for maintainability and performance:

### (A) Move toward a pure star schema
- Consider consolidating `dim_category` and `dim_sub-category` into `dim_product` so product hierarchy is contained in a single dimension table.
- Consider consolidating `dim_customer_segment` into `dim_customer` if segment is a simple attribute.

**Benefit:** fewer relationships, simpler field selection, reduced ambiguity risk.

### (B) Ensure “Region” is consistently sourced
- Region should ideally come from a single geography dimension (e.g., `dim_address`) to avoid confusion or filter path issues.
- Sales representative tables should be joined by Sales Rep key rather than by Region.

### (C) Return status as a field vs. dimension
- If only Yes/No is available, `Returned` can remain as a column in the fact table (simplest).
- A return dimension is optional and mainly for semantic clarity.

---

## 6) Safe migration plan (non-breaking approach)
Model refactoring can affect visuals and measures. To avoid breaking the report, any schema change should be done using a staged migration:

1. Save a new version (e.g., `SupplyChain_v2.pbix`)
2. Create new consolidated dimensions (e.g., `dim_product_v2`) in parallel
3. Add relationships in parallel (keep old tables temporarily)
4. Update visuals gradually to use the new dimensions
5. Validate totals & KPI consistency (Orders/Revenue/Profit/Return Rate)
6. Hide or remove legacy tables only after full validation

---

## 7) Notes for reviewers
This project focuses on **decision-support reporting**: the dashboard is designed to help identify:
- Regional revenue concentration and risk hotspots
- Return-rate drivers by product and region
- The relationship between delivery speed (Days) and profitability outcomes
- Product prioritization using Pareto analysis and cohort-style customer behavior views
