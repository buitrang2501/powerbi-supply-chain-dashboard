# Supply Chain & Sales Performance Dashboard (Power BI)

## 1. Business Context
Understanding supply chain operations and sales performance is critical for retail businesses to optimize logistics efficiency, reduce delivery delays, manage product returns, and improve profitability.

This project analyzes order-level transactional data (orders, customers, products, regions, shipping modes, delivery time, returns) and delivers an interactive Power BI dashboard to support both operational monitoring and revenue optimization.  
(Current repo already includes a star-schema approach and KPI-focused dashboarding; this version formalizes the dataset context, analytical questions, and decision recommendations.) 

---

## 2. Objectives & Requirements
Using Power BI, this project aims to:
- Analyze **revenue, profit, and sales performance** over time and across regions.
- Identify **drivers of delivery time** and **product return rate**.
- Build interactive dashboards to monitor **supply chain efficiency**.
- Provide **actionable recommendations** to optimize operations and increase revenue.

---

## 3. Dataset
**File**: `Supply Chain & Sales Datasets.xlsx`  
**Grain**: 1 row per *Retail Order ID* (order-level transaction).  
**Columns**: 29 fields covering Orders, Customers, Geography, Products, Sales, Profitability, Returns, and Delivery Days.

### 3.1 Key Fields (high level)
- Order timing: `Order Date`, `Ship Date`
- Logistics: `Ship Mode`, `Days` (actual delivery days)
- Return: `Returned` (Yes/No)
- Sales & profitability: `Sales`, `Profit`, `Cost`, `Discount`, `Quantity`, `Unit CP`, `Unit SP`
- Dimensions: Customer (Segment), Product (Category/Sub-Category), Geography (Region/State/City)

> Note: If the dataset cannot be committed due to licensing or file size constraints, place it under `/data` locally and use the refresh instructions in `/docs`.

---

## 4. Business Questions (Analysis Scope)
1) What is the average delivery time, and which region has the slowest deliveries?  
2) How does delivery time impact profitability?  
3) Which region has the highest return rate?  
4) Which product has the highest return rate, and what are the likely causes?  
5) What are the revenue & profit trends over time (Year/Quarter/Month)?  
6) Which top 5 regions have the highest/lowest revenue?  
7) Which products have the highest/lowest sales?  
8) Which customer segment contributes the most to total revenue?  
9) How does seasonality impact sales (peak vs low periods)?  
10) Forecast next quarter’s revenue based on historical trend.

---

## 5. Data Preparation (Power Query)
Main steps:
- Standardize date fields and data types.
- Handle missing/invalid values (especially `Days`, `Profit`, `Discount`).
- Remove duplicates using `Retail Order ID` / `Order ID` rules.
- Create a dedicated `DimDate` to enable robust time intelligence.

Deliverables are documented in `/docs`:
- `Data_Dictionary.md`
- `KPI_Definitions.md`
- `Model_Design.md`
- `Refresh_Instructions.md`

---

## 6. Data Model (Star Schema)
### 6.1 Proposed Model
- **FactOrders** (order-level metrics): Sales, Profit, Cost, Discount, Quantity, Days, Returned
- **DimDate**: calendar attributes for time intelligence
- **DimCustomer**: Customer ID, Segment, Customer Name
- **DimProduct**: Product ID, Category, Sub-Category, Product Name
- **DimGeo**: Region, State, City, Country, Postal Code (+ optional Lat/Long)
- **DimShipMode**: Ship Mode
- **DimSalesPeople**: Retail Sales People

> Model diagram screenshot should be placed at: `/assets/data_model.png`

---

## 7. KPI Framework (Core Measures)
- Total Orders
- Total Sales (Revenue)
- Total Profit
- Profit Margin
- Avg Delivery Days
- Return Rate
- Discount Rate
- Shipping Mode Efficiency (Delivery Days & Margin by Ship Mode)

KPI definitions + calculation logic are documented in: `/docs/KPI_Definitions.md`.

---

## 8. Dashboard Pages (Recommended Structure)
1) **Executive Overview**  
   - Revenue, Profit, Margin, Orders, Return Rate, Avg Delivery Days (with YoY/MoM)
2) **Delivery Performance**  
   - Delivery days by Region/Ship Mode, outlier detection, delay hotspots
3) **Returns & Profitability**  
   - Return rate by Region/Product, relationship between Delivery Days ↔ Return ↔ Margin
4) **Product & Customer Insights**  
   - Best/worst products, segment contribution, discount vs margin trade-off
5) **Forecast**  
   - Next quarter revenue forecast and confidence band (Power BI forecast)

---

## 9. Key Insights (Example Narrative)
- Delivery delays are concentrated in specific regions, indicating logistics bottlenecks and potential routing/fulfillment constraints.
- Longer delivery time correlates with reduced profit margin and increased return probability, suggesting service-level impacts customer behavior and cost.
- Certain categories show elevated return rates, implying quality/packaging/expectation misalignment.
- Revenue and profit exhibit seasonality, enabling proactive inventory and capacity planning.

> Replace these bullets with findings from your final dashboard screenshots.

---

## 10. Recommendations (Decision-Oriented)
### Short-term (0–3 months)
- Prioritize faster ship modes for high-margin products in delay-prone regions.
- Implement exception monitoring: alert on regions/products where Return Rate > baseline + threshold.

### Mid-term (3–6 months)
- Review SLA performance for underperforming regions and optimize fulfillment allocation.
- Improve packaging/quality checks for high-return categories.

### Data Enhancements (Next Iteration)
- Add carrier-level performance, warehouse capacity, and fulfillment lead-time to deepen root-cause analysis.

---

## 11. Repository Structure
- `/powerbi` : PBIX file(s) or template + screenshots
- `/assets`  : dashboard preview images + data model diagram
- `/docs`    : data dictionary, KPI definitions, modeling notes, refresh guide
- `/data`    : dataset file (optional, if allowed)

---

## 12. Tools & Skills
- Power BI (DAX, Power Query)
- Dimensional Modeling (Fact/Dimension, Star Schema)
- Supply Chain & Sales Analytics
- Dashboard Storytelling (Insights → Recommendations)

---

## 13. Dashboard Preview
Add key screenshots below:
- Overview page: `/assets/dashboard_overview.png`
- Delivery page: `/assets/dashboard_delivery.png`
- Returns page: `/assets/dashboard_returns.png`
- Forecast page: `/assets/dashboard_forecast.png`
