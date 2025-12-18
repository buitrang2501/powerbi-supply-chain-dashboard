# Supply Chain Dashboard (Power BI)

## 1. Business Context
Supply chain managers need timely visibility into inventory status, supplier performance, and order fulfillment to minimize stock-outs, reduce delays, and improve operational efficiency.

This dashboard provides a consolidated view of key supply chain KPIs to support data-driven operational and managerial decisions.

## 2. Dataset
- Supply chain transactional data covering:
  - Orders
  - Inventory levels
  - Suppliers
  - Warehouses
  - Shipping performance
- Time period: [e.g. 2022–2024]
- Data size: approximately [X] records

## 3. Data Modeling
- Dimensional modeling (Star Schema):
  - **Fact tables**: Orders, Inventory Movements
  - **Dimension tables**: Date, Product, Supplier, Warehouse
- Measures implemented using DAX for operational KPIs

## 4. Key KPIs
- Inventory Turnover
- Order Fulfillment Rate
- On-time Delivery Rate
- Average Lead Time
- Stock-out Rate

## 5. Dashboard Preview
![Dashboard Overview](assets/dashboard_overview.png)

## 6. Key Insights
- Stock-out risks are concentrated in specific product categories and warehouses
- Supplier lead time variability has a direct impact on on-time delivery performance
- Slow-moving inventory contributes to lower inventory turnover in selected locations

## 7. Tools & Skills
- Power BI
- DAX
- Data Modeling (Fact & Dimension)
- Supply Chain & Operations Analytics
- Data Visualization

## 8. Use Case
This dashboard is designed for:
- Supply Chain Managers
- Operations Teams
- Business Analysts supporting logistics and inventory planning
