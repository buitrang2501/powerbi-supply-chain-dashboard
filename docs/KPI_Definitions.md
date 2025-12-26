# KPI Definitions

## Commercial
- Total Revenue = SUM(Sales)
- Total Profit = SUM(Profit)
- Profit Margin = Total Profit / Total Revenue

## Orders & Returns
- Total Orders = DISTINCTCOUNT(Retail Order ID)
- Returned Orders = Total Orders where Returned = "Yes"
- Return Rate = Returned Orders / Total Orders
- Non-return Rate = 1 - Return Rate

## Delivery
- Avg Delivery Time (Days) = AVERAGE(Days)
- Delivery Bucket (optional):
  0–2, 3–5, 6–9, 10+ days
