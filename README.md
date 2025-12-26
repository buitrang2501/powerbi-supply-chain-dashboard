# Supply Chain & Sales Performance Dashboard (Power BI)

## Business Context
Retail businesses must balance revenue growth with supply chain efficiency. Delivery delays and high return rates directly reduce profitability through reverse logistics costs, lost margin, and customer churn risk.

This project analyzes order-level retail data (2014–2017) to evaluate revenue and profit performance alongside delivery speed (Actual delivery days) and returns. The outcome is an interactive Power BI dashboard designed for executive monitoring and operational drill-down.

---

## Objectives
Using Power BI, this project delivers:
- Sales & profitability analysis over time and across regions/products/customer segments.
- Delivery performance monitoring using **Actual delivery days**.
- Return-rate analysis to identify high-risk regions/products.
- Decision-oriented insights and recommendations to reduce returns and improve margin.

---

## Dataset
- Source file: `Supply Chain & Sales Datasets.xlsx`
- Grain: 1 row per **Retail Order ID** (order-level transaction)
- Key fields:
  - Commercial: Sales, Profit, Cost, Discount, Quantity, Unit CP, Unit SP
  - Supply chain: Ship Mode, **Days (Actual delivery days)**, Returned (Yes/No)
  - Dimensions: Customer (Segment), Product (Category/Sub-category), Geography (Region/State/City)

---

## Data Model (Star Schema)
Fact table:
- **fact_retail_order**: Sales, Profit, Cost, Discount, Quantity, Days, Returned, keys to dimensions

Dimensions:
- dim_calendar (Date)
- dim_customer (+ Segment)
- dim_product (+ Category, Sub-category)
- dim_address (Region/State/City/Postal Code)
- dim_ship_mode
- dim_retail_sales_people (Sales Rep)

> Model diagram: `/assets/data_model.png`

---

## KPI Definitions (Core)
- Total Orders = distinct count of Retail Order ID
- Revenue = sum of Sales
- Profit = sum of Profit
- Profit Margin = Profit / Revenue
- Avg Delivery Time (Days) = average of Days
- Returned Orders = count of orders where Returned = Yes
- **Return Rate = Returned Orders / Total Orders**
- Non-return Rate = 1 - Return Rate

> Full KPI logic: `/docs/KPI_Definitions.md` and `/docs/Key_DAX_Measures.md`

---

## Dashboard Pages
1) **Overview**
   - Revenue, Profit, Profit Margin, Return Rate, Total Orders
   - Revenue mix by customer segment & region
   - Monthly performance table with MoM signals

2) **Product**
   - Category & sub-category contribution
   - Pareto analysis (80/20) to prioritize operational focus
   - Profit vs Cost & margin risk by category

3) **Customer**
   - Customer base size, revenue per customer, orders per customer
   - Customer retention cohort view (repeat behavior)
   - Segment contribution and purchase distribution

4) **Sales Representative**
   - Revenue contribution and trend by sales rep
   - Performance breakdown with QoQ / YTD metrics

5) **Region**
   - Regional revenue trend and product mix
   - Geo distribution (state-level)
   - Return rate and delivery time view by region

6) **Tooltips**
   - Drill-down tooltips for product/customer/region to support exploratory analysis

---

## Results Snapshot (from dashboard)
Period: 02/01/2014 – 30/12/2017
- Orders: 5,009
- Revenue: 2.297M
- Profit: 286.4K
- Profit Margin: 12.47%
- **Return Rate: 15.97%**
- Customers: 793 (Avg revenue/customer: 2.90K)
- Revenue mix by segment: Consumer 50.56%, Corporate 30.74%, Home Office 18.7%
- Revenue by region: West 33.29% (highest), South 17.5% (lowest)
- Margin risk: Furniture margin ~2.49% vs Technology/Office Supplies ~17%

---

## Key Insights
1) **Return rate is material (15.97%)**, representing a meaningful profitability risk across the business.
2) **Delivery time is a likely driver of returns and margin**: prolonged delivery increases return probability and reduces effective margin due to reverse logistics and lost sales.
3) **Furniture shows a high-revenue but low-margin profile** (margin ~2.49%), suggesting discount/cost/return issues that require targeted controls.
4) **Revenue is concentrated in West (33.29%)**, so improving delivery speed and reducing returns in this region yields outsized impact.

---

## Recommendations
Short-term (0–3 months)
- Deploy exception monitoring: flag products/regions where Return Rate > baseline + threshold.
- Prioritize delivery improvement for high-revenue clusters (e.g., West) and high-risk categories (e.g., Furniture sub-categories).
- Introduce a “Return Watchlist” (Top N products by Return Rate × Revenue).

Mid-term (3–6 months)
- Review pricing/discount governance for low-margin categories; reassess discount strategy where margin erosion is persistent.
- Improve packaging/quality checks for sub-categories with elevated returns.

Next data enhancements
- Add carrier/warehouse-level data to validate root causes for delivery delays and returns.

---

## Repository Structure
- `/powerbi` : PBIX (or link in Releases) + screenshots
- `/assets`  : dashboard images + data model diagram
- `/docs`    : KPI definitions, DAX measures, model notes, refresh guide
- `/data`    : dataset (optional if permitted)

---

## Tools
Power BI (DAX, Power Query), Dimensional Modeling, Supply Chain Analytics
