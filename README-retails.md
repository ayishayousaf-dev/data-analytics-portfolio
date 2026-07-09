# Retail Sales & Customer Analytics Dashboard

Power BI dashboard analyzing retail sales performance across UAE regions, with a focus on revenue trends, product category performance, and customer segmentation.

## Business Problem
A multi-region retail business needs visibility into which regions, categories, and customer segments drive the most revenue and profit — and where discounting is eroding margin — to guide inventory and marketing decisions.

## Dataset
`retail_sales_data.csv` — 6,000 synthetic order-level records (Jan 2024–Jun 2025) covering:
- Order date, region (Dubai, Abu Dhabi, Sharjah, Ajman, RAK)
- Product category & item (Electronics, Fashion, Home & Living, Beauty, Groceries)
- Customer ID & segment (New, Returning, VIP, At-Risk)
- Quantity, unit price, discount, revenue, cost, profit
- Payment method

## Dashboard Pages
1. **Executive Summary** — Total revenue, profit, order count, YoY growth, KPI cards
2. **Regional Performance** — Revenue/profit by region on a map + bar chart, region vs category matrix
3. **Category & Product Deep Dive** — Top products by revenue and margin, category trend lines
4. **Customer Segmentation** — Revenue by segment, repeat purchase rate, at-risk customer value at stake

## Key DAX Measures

```dax
Total Revenue = SUM(retail_sales_data[Revenue])

Total Profit = SUM(retail_sales_data[Profit])

Profit Margin % = DIVIDE([Total Profit], [Total Revenue], 0)

Total Orders = DISTINCTCOUNT(retail_sales_data[OrderID])

Average Order Value = DIVIDE([Total Revenue], [Total Orders], 0)

Revenue MoM Growth % =
VAR CurrentRevenue = [Total Revenue]
VAR PriorRevenue =
    CALCULATE([Total Revenue], DATEADD(retail_sales_data[OrderDate], -1, MONTH))
RETURN DIVIDE(CurrentRevenue - PriorRevenue, PriorRevenue, 0)

Discount Impact = SUM(retail_sales_data[Discount])

Repeat Customer Revenue =
CALCULATE([Total Revenue], retail_sales_data[CustomerSegment] IN {"Returning", "VIP"})

At-Risk Revenue at Stake =
CALCULATE([Total Revenue], retail_sales_data[CustomerSegment] = "At-Risk")
```

## Key Insights (example narrative for interviews)
- Identified that Electronics drives the highest revenue but Beauty has the strongest margin, suggesting a re-balanced promotional calendar.
- Found VIP customers (12% of base) contribute a disproportionate share of profit, supporting a loyalty-program business case.
- Flagged At-Risk segment revenue exposure to prioritize retention campaigns.

## Tools Used
Power BI Desktop, DAX, Power Query (M) for data shaping

## How to Reproduce
1. Import `retail_sales_data.csv` into Power BI Desktop via Get Data → Text/CSV
2. Create a Date table and mark it as the date table for time intelligence
3. Build relationships and add the DAX measures above
4. Build visuals per the dashboard pages listed
