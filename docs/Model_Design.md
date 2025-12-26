# Model Design (Power BI)

## 1) Objective
This data model is built to support an interactive Supply Chain & Sales dashboard with drill-down by:
- Time (Order Date)
- Product hierarchy (Category → Sub-category → Product)
- Customer (Customer → Segment)
- Geography (Region/State/City)
- Ship mode
- Sales representative

The model is optimized for BI reporting (Power BI) with consistent KPI definitions for Revenue, Profit, Profit Margin, **Return Rate**, and **Avg Delivery Time (Days)**.

---

## 2) Data Grain
**Fact grain:** 1 row per **Retail Order ID** (order-level transaction).

Key operational field:
- `Days` = **Actual delivery days**
- `Returned` = Yes/No (used to compute Return Rate)

---

## 3) Tables & Roles

### 3.1 Fact Table
**fact_retail_order**
- **Keys / Foreign keys (logical):**
  - Date Key (from Order Date via dim_calendar)
  - Customer ID
  - Product ID
  - Address / Geo key (Region/State/City/Postal Code)
  - Ship Mode
  - Retail Sales People (Sales Rep)
- **Measures / numerical fields:**
  - Sales, Profit, Cost
  - Quantity, Discount
  - Unit CP, Unit SP
  - Days (Actual delivery days)
  - Returned (Yes/No)

---

### 3.2 Dimension Tables (Reporting Dimensions)
**dim_calendar**
- Date table for time intelligence (MoM/QoQ/YTD)
- Joined to fact via Order Date (recommended)

**dim_customer**
- Customer ID, Customer Name
- Linked to a segment lookup table (see below)

**dim_customer_segment** *(lookup / snowflake)*
- Segment (Consumer, Corporate, Home Office)
- Normalized for semantic clarity

**dim_product**
- Product ID, Product Name
- Linked to product hierarchy tables (Category/Sub-category) (see below)

**dim_category** *(lookup / snowflake)*
- Category

**dim_sub_category** *(lookup / snowflake)*
- Sub-category

**dim_address** (Geography)
- Country, Region, State, City, Postal Code
- Optional: Latitude/Longitude for map visuals

**dim_ship_mode**
- Standard / Express / Same-Day

**dim_retail_sales_people**
- Sales representative attributes used for sales rep performance views

---

### 3.3 Analytical Helper Table
**Customer_FirstPurchase**
- Derived helper table to support retention/cohort analysis
- Linked to dim_customer by Customer ID (recommended)

---

## 4) Schema Pattern
The model is **fact-centered (star-like)** with **light snowflaking** for:
- Product hierarchy (Category/Sub-category)
- Customer segment

This design supports the current dashboard requirements without refactoring existing visuals.  
A future improvement could consolidate hierarchy lookups into dim_product and segment into dim_customer to move closer to a pure star schema.

---

## 5) Relationship Principles (Recommended)
- Dimensions filter the fact table using **single-direction** relationships (Dim → Fact).
- Avoid ambiguous filter paths (ensure Region comes from a single geography dimension).
- Mark `dim_calendar` as the Date table to enable time intelligence.

---

## 6) KPI Layer (Where Measures Live)
Measures are created in a dedicated measure table and built from fact fields, for example:
- Total Orders (distinct count Retail Order ID)
- Total Revenue (sum Sales)
- Total Profit (sum Profit)
- Profit Margin (Profit / Revenue)
- Avg Delivery Time (average Days)
- Return Rate (Returned Yes / Total Orders)

See:
- `/docs/KPI_Definitions.md`
- `/docs/Key_DAX_Measures.md`

---

## 7) Future Enhancement (Non-breaking Migration Plan)
If model simplification is needed, apply a staged approach:
1) Create consolidated dimension tables in parallel (e.g., dim_product_v2 with Category/Sub-category included)
2) Add relationships in parallel
3) Migrate visuals gradually
4) Validate KPI totals (Orders/Revenue/Profit/Return Rate)
5) Remove legacy lookup tables only after validation

See `/docs/Model_Notes.md` for details.
