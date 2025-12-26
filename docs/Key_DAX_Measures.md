Total Orders = DISTINCTCOUNT('fact_retail_order'[Retail Order ID])
Total Revenue = SUM('fact_retail_order'[Sales])
Total Profit = SUM('fact_retail_order'[Profit])
Profit Margin = DIVIDE([Total Profit], [Total Revenue])

Avg Delivery Time (Days) = AVERAGE('fact_retail_order'[Days])

Returned Orders =
CALCULATE(
    [Total Orders],
    'fact_retail_order'[Returned] = "Yes"
)

Return Rate = DIVIDE([Returned Orders], [Total Orders])
Non-return Rate = 1 - [Return Rate]
