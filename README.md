# Supply Chain & Sales Performance Dashboard (Power BI)

Interactive Power BI dashboard for **Supply Chain & Sales** analysis (2014–2017), focusing on **Revenue/Profit**, **Delivery Performance**, **Return Rate**, and **Customer behavior** to support operational decisions.

---

## 1) Business Context
Retail businesses must balance **revenue growth** with **supply chain efficiency**. Delivery delays and product returns can reduce profitability through:
- higher reverse-logistics cost,
- margin erosion,
- customer churn risk.

This project analyzes **order-level** retail data to uncover:
- what drives **delivery days** and **returns**,
- where profit is concentrated (or at risk),
- how customer/segment/region/product contribute to performance.

---

## 2) Project Objectives
Using Power BI, this project delivers:

- **Sales & profitability** analysis over time (year/quarter/month) and across **regions / products / customer segments**  
- **Delivery performance** monitoring using *Actual delivery days* and delivery buckets  
- **Return-rate analysis** to identify high-risk **regions / products / customer segments / sales reps**  
- **Forecasting** next period revenue using Power BI built-in forecasting (ETS)  
- Actionable **business recommendations** for optimizing operations & improving profitability

---

## 3) Dataset
**Grain:** 1 row per *Retail Order ID* (order-level transaction)

**Key fields**
- Commercial: `Sales`, `Profit`, `Cost`, `Discount`, `Quantity`, `Unit CP`, `Unit SP`
- Supply chain: `Ship Mode`, `Days (Actual delivery days)`, `Returned (Yes/No)`
- Dimensions: Customer (`Segment`), Product (`Category/Sub-category`), Geography (`Region/State/City`), Sales Rep

> **Data availability**  
The raw dataset is not included in this repository (educational dataset; redistribution rights unclear).  
This repository focuses on **data modeling, Power Query steps, DAX measures, dashboard design, and insights**.

---

## 4) Data Model (Star Schema)
**Fact table**
- `fact_retail_order`: Sales, Profit, Cost, Discount, Quantity, Days, Returned + keys

**Dimensions**
- `dim_calendar` (Date)
- `dim_customer` (+ Segment)
- `dim_product` (+ Category, Sub-category)
- `dim_address` (Region/State/City/Postal Code)
- `dim_ship_mode`
- `dim_retail_sales_people` (Sales Rep)
- `dim_return` (Returned status)

📌 Model diagram: `assets/data_model.png`

---

## 5) KPI Definitions (Core)
- **Total Orders** = DISTINCTCOUNT(`Retail Order ID`)
- **Revenue** = SUM(`Sales`)
- **Profit** = SUM(`Profit`)
- **Profit Margin** = Profit / Revenue
- **Avg Delivery Days** = AVERAGE(`Days`)
- **Returned Orders** = COUNT orders where Returned = "Yes"
- **Return Rate** = Returned Orders / Total Orders
- **Non-return Rate** = 1 - Return Rate

📌 DAX documentation:  
- `docs/KPI_Definitions.md`  
- `docs/Key_DAX_Measures.md`

---

## 6) Analytical Questions (per requirement)
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

## 7) Dashboard Pages (What’s inside)
### 7.1 Overview
Executive view of:
- Total Orders, Revenue, Profit, Profit Margin, Return Rate
- Revenue mix by segment & region
- Delivery days vs profit margin (relationship)
- Monthly performance table + MoM indicators

📌 Screenshot: `assets/page_overview.png`

### 7.2 Product
- Cost vs Profit by Category (margin risk)
- Pareto analysis (80/20) for revenue concentration
- Return rate by Sub-category (scrollable ranking)

📌 Screenshot: `assets/page_product.png`

### 7.3 Customer
- Total customers, avg revenue/orders/products per customer
- Segment breakdown
- Customer retention (cohort by FirstPurchaseDate)

📌 Screenshot: `assets/page_customer.png`

### 7.4 Sales Representative
- Sales trend by rep (time series)
- Revenue & profit margin by rep
- Order volume vs return rate by rep (compare reps quickly)
- Performance table (QTD, QoQ, YTD, etc.)

📌 Screenshot: `assets/page_sales_rep.png`

### 7.5 Region
- Returned rate & avg delivery days by region
- Revenue contribution by product category per region
- State-level geo distribution
- Regional performance drill-down table (Region → City)

📌 Screenshot: `assets/page_region.png`

### 7.6 Drill-through Tooltips (Root-cause style)
Dedicated tooltip page to explain drivers of return-rate for the current filter context:
- Return rate Δ vs Overall
- Breakdown by Sub-category
- Breakdown by Region / Customer segment / Delivery-days bucket

✅ **Definition of “Overall” in tooltip**  
**Overall removes ONLY the Sales Rep filter** (all other slicers remain applied: Date/Region/Category/Ship mode...).  
This avoids misleading baselines and makes “Rep vs Overall” comparable.

📌 Tooltip screenshot: `assets/tooltip_return_driver.png`

### 7.7 Forecast Revenue
- Built-in forecasting (ETS) with confidence interval
- Useful for planning inventory/supply chain capacity

📌 Screenshot: `assets/forecast_revenue.png`

---

## 8) Results Snapshot (from dashboard)
**Period:** 2014-02-01 → 2017-12-30  
- **Total Orders:** 5.01K  
- **Revenue:** 2.30M  
- **Profit:** 286.40K  
- **Profit Margin:** 12.47%  
- **Return Rate (overall):** 5.91%  
- **Total Customers:** 793  

(Values reflect dashboard filters = All)

---

## 9) Key Insights (Story you can present in portfolio)
1) **Returns are not evenly distributed**  
Return rate varies by **sub-category** and **customer segment**, implying operational issues are concentrated (not “random noise”).

2) **Delivery time has a non-linear relationship with returns**  
Returns can spike at specific delivery windows (e.g., certain buckets) rather than monotonically increasing — suggesting service expectations, product type, and customer segment interact.

3) **Margin risk exists despite healthy overall margin**  
Some categories can be high revenue but lower margin, making them more sensitive to discounting and returns. Prioritizing those areas yields outsized ROI.

4) **Region-level volume ≠ return risk**  
High-volume regions may not always have the highest return rate. The dashboard helps separate:
- **where the business is big** (impact),
- vs **where the business is leaking margin** (risk).

5) **Sales Rep performance should be evaluated with a fair baseline**  
Using “Overall excluding Sales Rep filter” provides a clean comparison to avoid misinterpretation due to different product mixes or regions.

---

## 10) Recommendations
### Short-term (0–3 months)
- Implement a **Return Watchlist**: Top sub-categories by `Return Rate × Revenue`
- Set alert thresholds: return rate > overall baseline + configurable parameter
- Prioritize operational checks for high-return sub-categories (packaging, product expectation mismatch, QA)

### Mid-term (3–6 months)
- Review pricing/discount policy in categories where margin is fragile
- Improve delivery reliability for segments/buckets correlated with higher return risk
- Add a “reason for return” field (if available) to validate root causes

### Next data enhancements
- Carrier/warehouse data, shipment events (actual vs planned), damage codes
- Return reason + product condition + refund/exchange outcome
- Customer lifetime value to quantify churn risk

---

## 11) How to Run the Report (Important)
### Option A — PBIX (recommended)
1. Download the dataset Excel file (as instructed by the challenge)
2. Open `powerbi/SupplyChain_Dashboard.pbix`
3. Update data source path (if prompted)
4. Refresh

### Option B — PBIT (template)
If you publish a `.pbit` template, viewers **must connect to the dataset** when opening it.  
If they cancel or cannot locate the file path, Power BI may show a blank/empty report because queries fail to load.

📌 Suggested: include `docs/Refresh_Guide.md` with screenshots:
- where to update the Excel path
- how to refresh
- required Power BI settings

---
## 12) Repository Structure

```text
.
├── powerbi/      # PBIX/PBIT report files
├── assets/       # Screenshots + model diagram used in README
├── docs/         # DAX, KPI definitions, refresh notes
└── README.md     # Project overview & insights
```
---

## 13) Tools & Skills Demonstrated
- Power BI (DAX, Power Query)
- Dimensional modeling (Star Schema)
- KPI design + executive dashboard storytelling
- Root-cause style tooltips & drill-down analysis
- Time series trend + forecasting (ETS)
- Supply chain & sales analytics

---

## Author
**Bui Trang**  
Data Analyst | Power BI | Supply Chain Analytics  

