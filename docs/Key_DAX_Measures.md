-- Core
Total Orders = DISTINCTCOUNT('FactOrders'[Retail Order ID])
Total Sales  = SUM('FactOrders'[Sales])
Total Profit = SUM('FactOrders'[Profit])
Profit Margin = DIVIDE([Total Profit], [Total Sales])

-- Delivery / Returns
Avg Delivery Days = AVERAGE('FactOrders'[Days])

Returned Orders =
CALCULATE(
    [Total Orders],
    'FactOrders'[Returned] = "Yes"
)

Return Rate = DIVIDE([Returned Orders], [Total Orders])

-- Time Intelligence (requires DimDate marked as Date table)
Sales MoM =
VAR PrevMonth = CALCULATE([Total Sales], DATEADD('DimDate'[Date], -1, MONTH))
RETURN [Total Sales] - PrevMonth

Sales MoM % =
VAR PrevMonth = CALCULATE([Total Sales], DATEADD('DimDate'[Date], -1, MONTH))
RETURN DIVIDE([Total Sales] - PrevMonth, PrevMonth)

-- Delivery Days Buckets (calculated column on FactOrders recommended)
Delivery Bucket =
SWITCH(
    TRUE(),
    'FactOrders'[Days] <= 2, "0–2 days",
    'FactOrders'[Days] <= 5, "3–5 days",
    'FactOrders'[Days] <= 9, "6–9 days",
    "10+ days"
)
