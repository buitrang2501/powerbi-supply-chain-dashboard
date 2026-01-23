# Supply Chain & Sales Performance Dashboard (Power BI)

Interactive Power BI dashboard analyzing **sales performance** and **supply chain efficiency** (delivery speed + returns) using an order-level retail dataset.

> **Portfolio note:** This repository focuses on **data modeling (star schema)**, **Power Query ETL**, **DAX measures**, and **dashboard storytelling** for executive and operational decision-making.

---

## Business Context
Retail teams must balance revenue growth with supply chain reliability. **Delivery delays** and **high return rates** can reduce profitability through reverse-logistics costs, margin erosion, and customer churn risk.

This project provides a unified view of:
- **Revenue & profit trends** over time
- **Regional / product / segment performance**
- **Delivery-time patterns** (Actual delivery days)
- **Return-rate hotspots** by region and product

---

## Objectives
Using Power BI, the report enables:
- Monitor **revenue, profit, margin** across time and geography
- Identify **slow-delivery regions** and **high-return clusters**
- Explore performance by **product category/sub-category** and **customer segment**
- Support actions to **reduce returns**, **improve delivery SLAs**, and **protect margin**
- Forecast short-term revenue (Power BI **ETS** built-in forecasting)

---

## Dataset (Source & Attribution)
- Dataset is provided publicly by the original challenge source: **Mazhoc Data**.
- Download page: https://mazhocdata.tv/

**Important:** This repo does **not** redistribute the Excel dataset. Instead, it provides the Power BI report + documentation and links to the official source.

---

## What’s Inside the Report
### Pages
- **Overview:** KPIs (Revenue, Profit, Margin, Orders, Return Rate) + trend + top contributors  
- **Product:** Category / Sub-category performance, Pareto focus, margin risk  
- **Customer:** Segment mix, customer contribution & repeat behavior indicators  
- **Sales Rep:** Performance tracking by retail sales people  
- **Region:** Geo performance, delivery days and return-rate comparison  
- **Tooltips:** Drill-down tooltips to explain “why” behind return rate / delivery issues

### Highlights (Insights You Can Draw)
- Returns are concentrated in specific **regions** and **sub-categories** → prioritize root-cause analysis and action plans
- Longer delivery days can be investigated as a potential driver of **higher return risk**
- Some categories may show **high revenue but thin margins** → pricing/discount/returns governance opportunities
- Revenue is often concentrated in a small set of regions/products → operational improvements there yield outsized impact

> Exact values depend on filters/period selection and refresh.

---

## How to Open / Refresh
This repo publishes **PBIX** (not PBIT). To open and refresh:
1. Download the dataset Excel file from Mazhoc Data (link above)
2. Open: `powerbi/SupplyChain_Dashboard.pbix`
3. If prompted, update the Excel path (see **docs/Refresh_Guide.md**)
4. Refresh

📌 Detailed step-by-step guide: **docs/Refresh_Guide.md**

---

## Repository Structure
```text
.
├─ powerbi/            # Power BI report file(s)
├─ assets/             # Screenshots, model diagram used in README
└─ docs/               # DAX, KPI definitions, refresh notes, data dictionary
```

Recommended docs:
- `docs/Key_DAX_Measures.md`
- `docs/KPI_Definitions.md`
- `docs/Data_Dictionary.md`
- `docs/Refresh_Guide.md`

---

## Tools & Skills Demonstrated
- Power BI Desktop (Power Query, DAX)
- Star schema modeling (fact/dim)
- KPI design + executive dashboard storytelling
- Drill-down tooltips & exploratory analysis
- Time-series trend & forecasting (ETS)
- Supply chain & sales analytics

---

## License / Use
This project is shared for portfolio and learning purposes.  
Dataset copyright remains with the original publisher (Mazhoc Data / challenge owner).
