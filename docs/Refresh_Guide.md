# Refresh Guide (Power BI PBIX)

This guide helps viewers open the PBIX report and refresh it with the **original Excel dataset** from the challenge source.

---

## 1) Prerequisites
- Install **Power BI Desktop** (latest stable version recommended)
- Download the challenge dataset Excel file from:
  - https://mazhocdata.tv/

> This repository does not redistribute the dataset. You must download the Excel file from the official source.

---

## 2) Open the Report
1. Download / clone this repo
2. Open Power BI file:
   - `powerbi/SupplyChain_Dashboard.pbix`

---

## 3) Fix the Dataset Path (Most Common Step)
When you first open the PBIX on a new machine, Power BI may not find the Excel file path.

### Option A — Change data source (recommended)
1. In Power BI Desktop: **File → Options and settings → Data source settings**
2. Select the Excel source (workbook path)
3. Click **Change Source…**
4. Browse to your downloaded Excel file
5. Click **OK**

### Option B — If you see “Apply changes” prompts
- Click **Apply changes**
- Then click **Refresh** on the Home ribbon

---

## 4) Refresh the Model
1. Go to **Home → Refresh**
2. Wait until the refresh completes
3. Verify visuals load (no “error” on cards/charts)

---

## 5) Troubleshooting

### A) Report opens but visuals are blank
Common causes:
- Dataset path not updated correctly
- Refresh was cancelled
- Dataset structure changed or sheet/table names don’t match

Fix:
- Re-check **Data source settings** and refresh again

### B) “Cannot find column / field” errors
Possible causes:
- Dataset file version differs from what the report expects
- Column names in the Excel file were renamed

Fix:
1. Open **Transform data** (Power Query)
2. Check the first query that reads the Excel workbook
3. Verify the selected sheet/table name and promoted headers
4. Update column references where needed
5. **Close & Apply**, then refresh

### C) Performance is slow during refresh
- Keep only necessary tables “loaded”
- Avoid duplicate `Excel.Workbook(...)` steps in multiple queries
- Prefer a staging query (read Excel once) and create *Reference* queries for dim/fact

---

## 6) What to Expect After Refresh
- All visuals and slicers become interactive
- KPIs (Revenue/Profit/Margin/Return Rate/Delivery Days) update based on filters
- Forecast visuals depend on the historical period and seasonality

---

## 7) Notes
If you adapt the dataset (new columns, extra sheets), update:
- `docs/Data_Dictionary.md`
- `docs/Key_DAX_Measures.md` (if DAX changes)
