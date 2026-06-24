# -Royal-Enfield-Sales-Dashboard
“Power BI dashboard analyzing Royal Enfield sales performance across models, cities, and months.”

# Royal Enfield Sales Dashboard

This Power BI dashboard visualizes sales performance for Royal Enfield motorcycles across Indian cities and models.

## Key Insights
- ₹14M total revenue, ₹3M gross profit
- Top model: Classic 350 (₹3.9M revenue)
- Top city: Bangalore (15.46% of total revenue)
- Monthly trend peaks in April and August

## Tools Used
- Power BI Desktop
- Excel (data source)
- DAX for calculations

## Files
- `RE_Sales_Dashboard.pbix` → Power BI report
- `sales_dataset.xlsx` → Sample dataset
- `dashboard_screenshot.png` → Preview image
- 
## Key Measures - DAX Formulas

-- 1. Total Revenue
Total Revenue = SUMX(Sales, Sales[Quantity] * Sales[UnitPrice])

-- 2. Total Bikes Sold
Total Bikes = SUM(Sales[Quantity])

-- 3. Gross Margin %
Gross Margin % =
DIVIDE(
    SUMX(Sales, Sales[Quantity] * (Sales[UnitPrice] - Sales[COGS])),
    [Total Revenue],
    0
)

-- 4. YTD Revenue
Revenue YTD =
TOTALYTD([Total Revenue], Sales[OrderDate])

-- 5. YoY Growth %
Revenue YoY % =
VAR PrevYear = CALCULATE([Total Revenue], SAMEPERIODLASTYEAR(Sales[OrderDate]))
RETURN DIVIDE([Total Revenue] - PrevYear, PrevYear, 0)

-- 6. Avg Selling Price
Avg Selling Price = DIVIDE([Total Revenue], [Total Bikes], 0)

-- 7. Top Model by Revenue
Top Model =
TOPN(1, VALUES(Sales[Model]), [Total Revenue], DESC)

-- 8. Dealer Rank
Dealer Rank = RANKX(ALL(Sales[DealerName]), [Total Revenue],, DESC)

## Calculated Columns - Row level

1. Revenue = Sales[Quantity] * Sales[UnitPrice]

2. GrossProfit = Sales[Revenue] - Sales[COGS] * Sales[Quantity]

3. MonthYear = FORMAT(Sales[OrderDate], "YYYY-MM")

4. ProfitMargin% = DIVIDE(Sales[GrossProfit], Sales[Revenue], 0)

5. PriceBand =
SWITCH(TRUE(),
    Sales[UnitPrice] < 200000, "Below 2L",
    Sales[UnitPrice] < 300000, "2L - 3L",
    "Above 3L"
)
