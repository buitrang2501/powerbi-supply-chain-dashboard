# Supply Chain & Sales Performance Dashboard (Power BI)

Interactive Power BI dashboard for **Supply Chain & Sales** analysis (2014–2017), focusing on **Revenue/Profit**, **Delivery Performance**, **Return Rate**, and **Customer Behavior** to support operational decisions.

![Dashboard Overview](assets/page_overview.png)

**Quick links**
- Report file: `powerbi/SupplyChain_Dashboard.pbix`
- Data model: `assets/data_model.png`
- Docs: `docs/` (KPI, DAX, Model Notes, Refresh Guide, Data Dictionary)

---

## 1) Business Context
Retail businesses must balance **revenue growth** with **supply chain efficiency**. Delivery delays and product returns can reduce profitability through:
- higher reverse-logistics costs,
- margin erosion,
- customer churn risk.

This project analyzes **transaction-level** retail data to uncover:
- what drives **delivery days** and **returns**,
- where profit is concentrated (or at risk),
- how **customer / segment / region / product / sales rep** contribute to performance.

---

## 2) Project Objectives
Using Power BI, this project delivers:
- **Sales & profitability** analysis over time (year/quarter/month) and across **regions / products / customer segments**
- **Delivery performance** monitoring using *Actual delivery days* and delivery buckets
- **Return-rate analysis** to identify high-risk **regions / products / customer segments / sales reps**
- **Forecasting** next period revenue using Power BI built-in forecasting (ETS)
- Actionable **business recommendations** for optimizing operations & improving profitability

---

## 3) Dataset & Data Source
This report is built from a public challenge dataset published by **MazHoc Data**.

- Challenge page (overview + dataset download): `https://mazhocdata.tv/showcase/`
- Dataset name: **Supply Chain & Sales Performance Dataset** (2014–2017)

> **Important (PBIX behavior)**
> - The uploaded **PBIX is viewable immediately** using the cached model + data inside the file.
> - If you want to **refresh / reproduce from the original dataset**, follow the step-by-step guide in `docs/Refresh_Guide.md`.

---

## 4) Data Grain & Definitions (to avoid confusion)
Although the dataset includes both *Order ID* and *Retail Order ID*, visuals may behave differently depending on which identifier is used.

**This project uses the following convention:**
- **Line-item grain (Fact table):** 1 row per `Retail Order ID`  
- **Order-level counting (when needed):** use `Order ID`

**Recommended KPIs for clarity**
- **Total Line Items** = DISTINCTCOUNT(`Retail Order ID`)
- **Total Orders** = DISTINCTCOUNT(`Order ID`)


---

## 5) Data Model (Star-like Schema)
**Fact table**
- `fact_retail_order`: Sales, Profit, Cost, Discount, Quantity, Days, Returned + keys to dimensions

**Dimensions**
- `dim_calendar` (Date)
- `dim_customer` (+ Segment)
- `dim_product` (+ Category, Sub-category)
- `dim_address` (Region/State/City/Postal Code)
- `dim_ship_mode`
- `dim_retail_sales_people` (Sales Rep)
- `dim_return` (Returned status)

📌 Model diagram: `assets/data_model.png`  
📌 Model explanation (incl. snowflake rationale if any): `docs/Data_Model_Notes.md`

---

## 6) KPI Definitions (Core)
Core KPIs used across the report:
- **Revenue** = SUM(`Sales`)
- **Profit** = SUM(`Profit`)
- **Profit Margin** = Profit / Revenue
- **Avg Delivery Days** = AVERAGE(`Days`)
- **Returned Line Items** = COUNT rows where Returned = "Yes"
- **Return Rate (Line)** = Returned Line Items / Total Line Items

Optional (Order-level) KPIs if enabled:
- **Returned Orders** = DISTINCTCOUNT(`Order ID`) where Returned = "Yes"
- **Return Rate (Order)** = Returned Orders / Total Orders

📌 KPI documentation:
- `docs/KPI_Definitions.md`
- `docs/Key_DAX_Measures.md`

---

## 7) Analytical Questions (per challenge requirement)
1. Average delivery time? Which region has slowest deliveries?
2. How does delivery time impact profitability?
3. Which region has the highest return rate?
4. Which product has the highest return rate & key causes?
5. Revenue and profit trends over time (year/quarter/month)?
6. Top 5 regions highest/lowest revenue?
7. Products highest/lowest sales?
8. Which customer segment contributes most to total revenue?
9. Seasonality: peak vs low sales periods?
10. Forecast next quarter revenue?

---

## 8) Dashboard Pages (What’s inside)
### 8.1 Overview
Executive view of:
- Revenue, Profit, Profit Margin, Return Rate, Orders/Line items
- Revenue mix by segment & region
- Delivery days vs profitability patterns
- Monthly performance table + MoM indicators

Screenshot: `assets/page_overview.png`

### 8.2 Product
- Cost vs Profit by Category (margin risk)
- Pareto (80/20) for revenue concentration
- Return drivers by Sub-category (ranking)

Screenshot: `assets/page_product.png`

### 8.3 Customer
- Customer base size + avg revenue/orders/products per customer
- Segment contribution
- Customer retention cohort view (repeat behavior)

Screenshot: `assets/page_customer.png`

### 8.4 Sales Representative
- Trend by rep
- Revenue & margin by rep
- Return rate comparison with fair baseline
- Performance table (QTD, QoQ, YTD)

Screenshot: `assets/page_sales_rep.png`

### 8.5 Region
- Return rate & avg delivery days by region
- Revenue mix by category across regions
- Geo distribution + drill-down tables (Region → City)

Screenshot: `assets/page_region.png`

### 8.6 Drill-through Tooltips (Root-cause style)
Dedicated tooltip page(s) to explain drivers of return-rate for the current filter context:
- Return rate delta vs baseline
- Breakdown by Sub-category
- Breakdown by Region / Segment / Delivery-days bucket

✅ **Baseline definition (“Overall”)**
- **Overall removes ONLY the Sales Rep filter**
- Other slicers remain applied (Date/Region/Category/Ship mode...), ensuring a fair comparison

Tooltip screenshot: `assets/tooltip_sales_rep.png`

### 8.7 Forecast Revenue
- Built-in forecasting (ETS) with confidence interval
- Used for planning inventory and logistics capacity

Screenshot: `assets/forecast_revenue.png`

---

## 9) Key Insights (summary)
1) **Returns are concentrated, not evenly distributed**  
Return risk clusters by **sub-category** and **segment**, suggesting targeted interventions outperform broad policies.

2) **Delivery impact is not always linear**  
Return spikes may occur in specific delivery windows/buckets, indicating expectation mismatch and category/segment interactions.

3) **Margin risk exists inside high-revenue areas**  
Some categories are more sensitive to discounting and returns; prioritizing them yields outsized ROI.

4) **Volume ≠ risk**  
High-revenue regions are not always the highest return-risk regions. The dashboard separates:
- where business impact is high, vs.
- where margin leakage is high.

5) **Sales Rep comparison needs a fair baseline**  
Using “Overall excluding Sales Rep filter” reduces bias due to different mixes (product/region/segment).

---

## 10) Recommendations
### Short-term (0–3 months)
- Implement a **Return Watchlist**: Top sub-categories by `Return Rate × Revenue`
- Alert thresholds: return rate > baseline + configurable parameter
- Prioritize operational checks for high-return sub-categories (packaging, product expectation mismatch, QA)

### Mid-term (3–6 months)
- Review pricing/discount policy where margin is fragile
- Improve delivery reliability for segments/buckets correlated with higher return risk
- Add a “reason for return” field (if available) to validate root causes

### Next data enhancements
- Carrier/warehouse + shipment events (actual vs planned)
- Return reason + product condition + refund/exchange outcome
- Customer lifetime value to quantify churn risk

---

## 11) How to View / Run
### View (no dataset needed)
1. Download `powerbi/SupplyChain_Dashboard.pbix`
2. Open in Power BI Desktop  
➡️ You can explore the report immediately using the cached model/data.

### Refresh (optional)
To refresh from the original public dataset:
- Follow `docs/Refresh_Guide.md`

---

## 12) Repository Structure
- `powerbi/` : PBIX report
- `assets/`  : dashboard screenshots + data model diagram
- `docs/`    : KPI definitions, DAX measures, model notes, refresh guide, data dictionary

---

## 13) Tools & Skills
**Power BI**, **Power Query (ETL)**, **DAX**, **Dimensional Modeling**, **Supply Chain Analytics**, **Data Visualization**
