# 📊 Retail Sales Analysis Dashboard (Power BI)

## 📌 Overview
This project is a Power BI dashboard built to analyze retail sales performance, revenue trends, and business KPIs.

## 🎯 Objectives
-Import dataset
Inspect columns
Clean and transform in Power Query
Create RestaurantID
Finalize Restaurants table
Create Orders table
Create Customers table
Create Date table
Build relationships
Create calculated columns
Create base measures
Create advanced measures
Build Page 1
Build Page 2
Build Page 3
Build Page 4
Build Page 5
Add slicers
Add interactions
Final formatting
- Analyze total sales, profit, and order trends
- Identify top products and regions
- Track KPI metrics using DAX
- Provide interactive business insights

## 🛠 Tools Used
- Power BI Desktop
- Power Query
- DAX
- Excel

## 📊 Key Features
- KPI Cards (Sales, Profit, Orders)
- Region-wise analysis
- Monthly trend analysis
- Product performance breakdown
- Interactive slicers & filters

## 🧠 DAX Measures
CALCULATED COLUMNS

0. FilterCity = 
VAR val = 'Customer'[city]
VAR wordCount = LEN(val) - LEN(SUBSTITUTE(val, " ", "")) + 1
RETURN
IF(
    ISBLANK(val) || wordCount > 4 || LEN(val) > 50,
    BLANK(),
    val
)

1. In Restaurants

Cost Bucket =
SWITCH(
    TRUE(),
    Restaurants[CostForTwo] < 500, "Budget",
    Restaurants[CostForTwo] < 1000, "Mid Range",
    "Premium"
)


Rating Bucket =
SWITCH(
    TRUE(),
    Restaurants[Rating] >= 4.5, "Excellent",
    Restaurants[Rating] >= 4.0, "Good",
    Restaurants[Rating] >= 3.0, "Average",
    "Low"
)


2. In Orders


Delivery Status =
IF(Orders[DeliveryTimeMins] > 45, "Late", "On Time")



Order Month = FORMAT(Orders[OrderDate], "MMM YYYY")


CORE MEASURES


1. KPI measures

Total Orders = COUNT(Orders[OrderID])

Total Revenue = SUM(Orders[OrderValue])

Avg Order Value = DIVIDE([Total Revenue], [Total Orders])

Avg Delivery Time = AVERAGE(Orders[DeliveryTimeMins])

Total Customers = DISTINCTCOUNT(Orders[CustomerID])

Total Restaurants = DISTINCTCOUNT(Restaurants[RestaurantID])



2. Status measures

Delivered Orders =
CALCULATE([Total Orders], Orders[OrderStatus] = "Delivered")

Cancelled Orders =
CALCULATE([Total Orders], Orders[OrderStatus] = "Cancelled")

Cancellation % =
DIVIDE([Cancelled Orders], [Total Orders])

Late Orders =
CALCULATE([Total Orders], Orders[Delivery Status] = "Late")

Late Delivery % =
DIVIDE([Late Orders], [Total Orders])


3. Customer measures

Repeat Customers =
COUNTROWS(
    FILTER(
        VALUES(Orders[CustomerID]),
        CALCULATE(COUNT(Orders[OrderID])) > 1
    )
)

Repeat Customer % =
DIVIDE([Repeat Customers], [Total Customers])

Orders Per Customer =
DIVIDE([Total Orders], [Total Customers])


4. Restaurant performance measures

Revenue per Restaurant =
DIVIDE([Total Revenue], [Total Restaurants])

Avg Rating = AVERAGE(Restaurants[Rating])

Avg Votes = AVERAGE(Restaurants[Votes])


5. Time intelligence measures

Revenue YTD = TOTALYTD([Total Revenue], DateTable[Date])

Orders YTD = TOTALYTD([Total Orders], DateTable[Date])

Revenue Previous Month =
CALCULATE([Total Revenue], DATEADD(DateTable[Date], -1, MONTH))

Revenue Growth % =
DIVIDE([Total Revenue] - [Revenue Previous Month], [Revenue Previous Month])


## 📁 Files
- Retail_Sales.pbix (Power BI file)
- data/sales_data.csv
- screenshots/

## 🚀 How to Use
1. Open PBIX file in Power BI Desktop
2. Refresh dataset if required
3. Explore dashboard visuals

## 👤 Author
Subhankar Singh
Data Analyst | Power BI | SQL | Excel