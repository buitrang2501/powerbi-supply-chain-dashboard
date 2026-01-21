# Refresh Guide (Power BI Template)

This repository provides a **Power BI template (.pbit)** for a supply chain & sales performance dashboard.  
To protect data privacy and respect dataset redistribution rights, **no raw data is included** in this repo.

Use this guide to connect the template to **your own dataset** and refresh the model safely.

---

## What you need

- **Power BI Desktop** (latest recommended)
- A dataset that follows the same schema as the template (see the required fields below)
- The template file: `powerbi/SupplyChain_Dashboard.pbit` (or your file name)

---

## Step 1 — Open the template

1. Download the `.pbit` file from the `powerbi/` folder.
2. Open it with **Power BI Desktop**.
3. A **Parameters** window may appear (depending on how the template was packaged).

> Note: If the report opens with empty visuals, that is expected until you connect a data source and refresh.

---

## Step 2 — Update data source (Power Query)

### Where is Power Query?
In Power BI Desktop:
- **Home** → **Transform data** → **Transform data**

This opens **Power Query Editor**.

### Update the source safely
In Power Query Editor:
1. In the left panel (**Queries**), select the main query (e.g., `Orders`, `SupplyChain`, or similar).
2. In the ribbon: **Home** → **Data source settings**
3. Select the current source → **Change Source…**
4. Point to your new source:
   - Excel / CSV / Folder / Database, etc.

Then:
- **Close & Apply** (top-left) to load data into the model.

✅ Best practice: Use a **parameter** (e.g., `pDataPath`) for file paths so you only update the path once.

---

## Step 3 — Refresh the report

After applying changes:
- Power BI Desktop → **Home** → **Refresh**

If refresh fails, review:
- Column names match
- Data types match (Date, numeric fields)
- Relationships are intact (Model view)

---

## Step 4 — Validate the model (quick checks)

Go to **Model view** and confirm:
- Fact table is connected to dimensions (Date / Product / Customer / Geography / Sales Rep / Ship Mode)
- Relationship keys are not broken (no missing IDs)

Go to **Report view** and verify:
- KPI cards show values
- Time-series charts render
- Filters/slicers respond correctly

---

## Required fields (minimum schema)

Your dataset should include fields equivalent to the following:

**Order & logistics**
- Retail Order ID (unique)
- Order ID
- Order Date
- Ship Date
- Ship Mode
- Days (Actual delivery days)
- Returned (Yes/No)

**Customer**
- Customer ID
- Customer Name
- Segment

**Geography**
- Country
- City
- State/Province
- Region
- Postal Code (optional)
- Latitude / Longitude (optional, for maps)

**Product**
- Product ID
- Category
- Sub-Category
- Product Name

**Commercial**
- Sales
- Profit
- Cost
- Discount
- Quantity
- Unit CP / Unit SP (optional)

**Sales Rep**
- Retail Sales People (Sales Rep)

---

## Troubleshooting

### A) The report opens blank / visuals show (Blank)
This usually means:
- No data source is connected yet, or
- Refresh hasn’t been run

Fix:
1. Update source in **Power Query**
2. **Close & Apply**
3. **Refresh**

### B) “Could not find file …” / path errors
Fix:
- **Transform data** → **Data source settings** → **Change Source**
- Avoid hard-coded local paths in M code
- Use a parameter (`pDataPath`) whenever possible

### C) Relationships broken / slicers not working
Fix:
- Check key columns exist and are the same data type
- Ensure Date fields are proper Date type
- Recreate relationships in **Model view** if needed

### D) Map visual not working
Fix:
- Ensure Region/State/City are valid geographic labels
- If Latitude/Longitude available, set data categories:
  - Latitude → Latitude
  - Longitude → Longitude

---

## Privacy & redistribution note

- This repository **does not contain raw data**.
- The `.pbit` template contains **report layout, data model structure, and DAX measures** only.
- Replace the source with your own dataset following the same schema.

If you plan to publish the report publicly, remove any customer-identifiable fields or apply masking/anonymization where appropriate.
