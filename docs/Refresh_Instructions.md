# docs/Refresh_Guide.md

## Purpose
This guide explains how to open and refresh the Power BI report/template when the dataset is not bundled in the repository.

---

## 1) Files You May Publish
### Option A — PBIX
- Includes layout + model + queries (may include data if not removed)
- Best for reviewers if you can publish with data (only if redistribution is allowed)

### Option B — PBIT (Template) ✅ Recommended for portfolio
- Includes layout + model + DAX measures + Power Query steps
- **Does NOT include data**
- Viewers must connect to the dataset file on their machine

> Important: If the viewer cannot locate the dataset path, Power BI may show a blank report (queries fail to load).

---

## 2) Prerequisites for Viewers
- Install **Power BI Desktop**
- Have access to dataset file:
  - `Supply Chain & Sales Datasets.xlsx` (or your provided workbook)

---

## 3) How to Reconnect Dataset (PBIT / PBIX with missing path)

### Step 1 — Open the report
- Open `.pbit` (or `.pbix`)
- When prompted, choose the dataset file (Excel)

### Step 2 — Update source path in Power Query
If it doesn’t prompt automatically:

1. Go to **Home → Transform data** (Power Query Editor)
2. Select any query table
3. Go to **Data source settings**
4. Choose the Excel source → **Change Source**
5. Browse and select the correct `.xlsx` file
6. Click **Close & Apply**

---

## 4) Refresh Steps
- In Power BI Desktop: **Home → Refresh**
- Wait for refresh completion
- Validate slicers and KPI cards update properly

---

## 5) Troubleshooting

### Problem A: Report opens but visuals are blank
Likely causes:
- dataset path not found
- wrong file selected (different column names)
Fix:
- Update data source settings (Section 3)
- Confirm the Excel file contains expected sheets/columns

### Problem B: Date slicer not working / time measures wrong
Fix:
- Confirm `dim_calendar` is marked as **Date table**
- Ensure date column is proper Date type

### Problem C: Buckets sort order is wrong (0–2, 15+, 3–5…)
Fix:
- In Data view: select `Delivery Days Bucket`
- Set **Sort by column** = `Delivery Bucket Sort`

---

## 6) Recommended Portfolio Note (README snippet)
Add a note in README:
- dataset not included (educational / licensing)
- template requires reconnecting Excel file
- this repo focuses on modeling, DAX, and dashboard storytelling
