# View & Refresh Guide (Power BI PBIX)

This guide explains:
- how to **view** the report immediately, and
- how to **refresh** it using the original Excel dataset from the public challenge source.

## 1) View the Report (No dataset needed)

The PBIX in this repository is provided in **Import mode** and already contains a processed snapshot of the data.  
You can open and explore the visuals without downloading the dataset.

1. Install **Power BI Desktop** (latest stable version recommended)
2. Download / clone this repo
3. Open: `powerbi/SupplyChain_Dashboard.pbix`

## 2) Refresh from the Original Dataset (Optional)

If you want to rebuild/refresh the model from the original Excel file:

1. Download the dataset Excel file from the challenge source (Mazhoc Data).
2. In Power BI Desktop, go to:
   - **File → Options and settings → Data source settings**
3. Select the Excel source (workbook path) → **Change Source…**
4. Browse to your downloaded Excel file → **OK**
5. Click **Refresh** (Home ribbon)

## 3) Troubleshooting

### A) Report opens but visuals are blank
Common causes:
- Dataset path not updated correctly
- Refresh was cancelled
- Dataset structure changed (sheet/table names don’t match)

Fix:
- Re-check **Data source settings** and refresh again.

### B) “Cannot find column / field” errors
Possible causes:
- Dataset file version differs from what the report expects
- Column names were renamed

Fix:
1. Open **Transform data** (Power Query)
2. Check the first query that reads the Excel workbook
3. Verify selected sheet/table name and promoted headers
4. Update column references if needed
5. **Close & Apply**, then refresh

### C) Refresh is slow
Best practices:
- avoid duplicate `Excel.Workbook(...)` reads in multiple queries
- use a **staging query** (read Excel once) and create *Reference* queries for dim/fact

## 4) Notes

If you adapt the dataset (new columns/sheets), update:
- `docs/Data_Dictionary.md`
- `docs/Key_DAX_Measures.md` (if DAX changes)
