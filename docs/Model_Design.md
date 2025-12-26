# Data Model Design (Star Schema)

## Fact
FactOrders
- Keys: Retail Order ID (or Order ID), Product ID, Customer ID, Date Key(s), Region Key, Ship Mode Key
- Measures: Sales, Profit, Cost, Discount, Quantity, Days, Returned

## Dimensions
DimDate
DimCustomer (Customer ID, Name, Segment)
DimProduct (Product ID, Category, Sub-Category, Product Name)
DimGeo (Region, State, City, Country, Postal Code, Lat/Long)
DimShipMode (Ship Mode)
DimSalesPeople (Retail Sales People)

## Notes
- Create DimDate from Order Date (and optionally Ship Date).
- Use single-direction relationships from dimensions → fact for stable measures.
- Confirm grain: one row per Retail Order ID.
