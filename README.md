# Supply Chain Analysis Dashboard (Power BI)

## 1. Business Context
Retail supply chain operations require continuous monitoring of delivery performance, return behavior, shipping efficiency, and regional logistics effectiveness to reduce delays, minimize operational costs, and improve customer satisfaction.

This project analyzes end-to-end supply chain operations for a retail business using real-world transactional data. An interactive Power BI dashboard was developed to support real-time monitoring and data-driven operational decision-making.

---

## 2. Dataset
- Retail supply chain transactional data including:
  - Order transactions
  - Products and product hierarchy (category, sub-category)
  - Customers and customer segments
  - Shipping modes and return status
  - Geographic information (city, state, region)
  - Sales and logistics attributes
- Time period: [e.g. 2022–2024]
- Data size: multi-table dataset with order-level transactional records

---

## 2.1 Data Preparation
The original dataset was provided as a single denormalized transactional table containing order, customer, product, logistics, geographic, and sales attributes.

Data preparation involved cleaning and standardizing the data, including handling missing values, correcting inconsistencies, and removing duplicate records. The dataset was then transformed into a dimensional model by defining the appropriate analytical grain and separating fact and dimension tables to ensure data consistency, reliable KPI calculation, and scalable analysis in Power BI.

---

## 3. Data Modeling
- Dimensional modeling using a star schema design
- Central fact table:
  - Retail order transactions capturing order-level metrics such as sales, cost, discount, profit, delivery duration, and return status
- Supporting dimension tables:
  - Date (calendar)
  - Product (including category and sub-category)
  - Customer and customer segment
  - Address (city, state, region)
  - Sales people
  - Ship mode
  - Return status
- DAX measures implemented to calculate operational and logistics performance KPIs

---

## 4. Key KPIs
The dashboard focuses on core supply chain and logistics performance indicators, including:
- Total Orders
- Revenue & Profit Impact
- Profit Margin
- Return / Cancellation Rate
- Average Delivery Time
- Shipping Mode Efficiency
- Regional Logistics Performance
- Order Volume & Margin Trends

---

## 5. Dashboard Preview
![Dashboard Overview](assets/dashboard_overview.png)

---

## 6. Key Insights
- Delivery delays are concentrated in specific regions, indicating regional logistics bottlenecks that may require improvements in routing or fulfillment processes.
- Certain product categories exhibit higher return rates, suggesting potential issues related to product quality, packaging, or customer expectation management.
- Shipping mode selection has a measurable impact on both delivery time and profit margin, highlighting opportunities to balance cost and service level.
- Order volume and profit margin show clear monthly and seasonal patterns, supporting the need for proactive capacity and inventory planning.
- Regional differences in logistics performance reveal opportunities to standardize best practices and optimize supply chain operations.

---

## 7. Tools & Skills Applied
- Power BI
- DAX
- Dimensional Data Modeling (Fact & Dimension)
- Supply Chain & Logistics Analytics
- Data Cleaning and Data Preparation
- Data Visualization and Dashboard Design

---

## 8. Use Case
This dashboard is designed for:
- Supply Chain and Operations Managers
- Logistics and Fulfillment Teams
- Business and Data Analysts supporting operational performance monitoring

---

## Data Availability
The raw dataset is not included in this repository due to data size and confidentiality considerations.  
This project focuses on data cleaning, data modeling, KPI design, analytical insights, and dashboard development using Power BI.
