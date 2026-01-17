
---

```md
# docs/Data_Model_Notes.md

## Purpose
This document describes the Power BI semantic model used in the Supply Chain & Sales dashboard, including schema design, relationships, and modeling decisions.

---

## 1) Modeling Approach
The model follows a **Star Schema**:

- One central fact table: **fact_retail_order**
- Multiple dimension tables: date, customer, product, geography, ship mode, sales rep, return status

This design improves:
- performance
- DAX readability
- consistent filtering behavior across report pages

---

## 2) Tables & Roles

### Fact
**fact_retail_order**
- Keys: Retail Order ID, Order Date, Customer ID, Product ID, Ship Mode ID, Return Status ID, Sales Rep (or ID), Address (postal/state/region)
- Measures source fields: Sales, Profit, Cost, Discount, Quantity, Days

### Dimensions
**dim_calendar**
- Role: time intelligence (YTD/QTD/MoM/QoQ)
- Must be marked as “Date table” with `Date`

**dim_customer**
- Customer attributes: Customer Name, Customer ID, Segment, etc.

**dim_customer_segment** *(optional if separated)*
- Segment: Consumer / Corporate / Home Office

**dim_product**
- Product attributes: Product Name, Product ID, Sub-Category ID

**dim_category / dim_sub-category**
- Category & Sub-category descriptions

**dim_address**
- Location attributes: City, State, Region, Postal Code, Lat/Long

**dim_ship_mode**
- Ship Mode descriptions

**dim_return**
- Return flags: Returned (Yes/No), Status ID

**dim_retail_sales_people**
- Sales Rep attributes & mapping to region (if applicable)

---

## 3) Relationships (Expected)
- Each dimension has a **1-to-many** relationship to the fact table.
- Filter direction: **Single** (dimension → fact) is recommended for clarity.

Examples:
- dim_calendar[Date] (1) → fact_retail_order[Order Date] (*)
- dim_customer[Customer ID] (1) → fact_retail_order[Customer ID] (*)
- dim_product[Product ID] (1) → fact_retail_order[Product ID] (*)
- dim_ship_mode[Ship Mode ID] (1) → fact_retail_order[Ship Mode ID] (*)

---

## 4) Key Modeling Decisions

### 4.1 “Overall” Benchmark Design
Several pages/tooltips include an **Overall** line used as benchmark.

Rule:
- **Overall removes Sales Rep filter only**
- All other filters remain applied (Date/Region/Product/Ship mode/Segment…)

DAX uses:
- `REMOVEFILTERS(dim_retail_sales_people[Retail Sales People])`

This ensures fair comparison for Sales Rep drill-down without losing context.

---

### 4.2 Delivery Days Buckets
You use delivery days buckets for interpretability:
- 0–2, 3–5, 6–8, 15+ days

Best practice:
- Create:
  - `Delivery Days Bucket` (text)
  - `Delivery Bucket Sort` (numeric)
- Then sort bucket label by sort column to enforce correct order in visuals.

---

### 4.3 Customer Retention Cohort
Cohort is defined by:
- Customer’s `FirstPurchaseDate` (monthly)

Retention at month k:
- % of cohort customers who placed ≥1 order in month k after cohort month
- Calculated within current slicer context (commonly Date + Segment + Region)

---

## 5) Performance Notes
Recommended:
- keep fact table wide fields minimized
- avoid bi-directional relationships unless necessary
- use measures instead of calculated columns where possible

---

## 6) Visual QA Checklist
- Cross-filtering works consistently across pages
- Slicers (Date/Region/Category/Ship mode) affect KPI cards & visuals
- “Overall” stays stable when Sales Rep slicer changes (by design)
