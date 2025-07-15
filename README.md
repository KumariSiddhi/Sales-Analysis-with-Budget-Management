 # Sales Analysis with Budget Management

## Overview

This project analyzes sales data and manages budgets for a telecom service provider. The goal is to minimize customer attrition and provide actionable insights using SQL Server and Power BI.

## Installation

Required software:
- Power BI Desktop
- Microsoft SQL Server 2019 Developer Edition
- SQL Server Management Studio

## Data Preparation

1. **Update the database** with the latest backup.
2. **Filter data** for the last year and current year.

## Data Cleansing & Transformation

Extract and transform only the required tables and fields:

- **Sales Fact Table**
- **Customer Details Table** (Dimension)
- **Product Details Table** (Dimension)
- **Calendar Table** (Dimension)
- **Budget Data** (from Excel)

### Example SQL Queries

**Calendar Table**
```sql
SELECT DateKey, FullDateAlternateKey AS Date, EnglishDayNameOfWeek AS Day, EnglishMonthName AS Month, LEFT(EnglishMonthName, 3) AS MonthShort, MonthNumberOfYear AS MonthNo, CalendarQuarter AS Quarter, CalendarYear AS Year
FROM AdventureWorksDW2019.dbo.DimDate
WHERE CalendarYear >= 2019
```

**Customer Table**
```sql
SELECT c.customerkey AS CustomerKey, c.firstname AS [First Name], c.lastname AS [Last Name], c.firstname + ' ' + lastname AS [Full Name], CASE c.gender WHEN 'M' THEN 'Male' WHEN 'F' THEN 'Female' END AS Gender, c.datefirstpurchase AS DateFirstPurchase, g.city AS [Customer City]
FROM AdventureWorksDW2019.dbo.DimCustomer AS c
LEFT JOIN dbo.dimgeography AS g ON g.geographykey = c.geographykey
ORDER BY CustomerKey ASC
```

**Product Table**
```sql
SELECT p.ProductKey, p.ProductAlternateKey AS ProductItemCode, p.EnglishProductName AS [Product Name], ps.EnglishProductSubcategoryName AS [Sub Category], pc.EnglishProductCategoryName AS [Product Category], p.Color AS [Product Color], p.Size AS [Product Size], p.ProductLine AS [Product Line], p.ModelName AS [Product Model Name], p.EnglishDescription AS [Product Description], ISNULL(p.Status, 'Outdated') AS [Product Status]
FROM AdventureWorksDW2019.dbo.DimProduct AS p
LEFT JOIN dbo.DimProductSubcategory AS ps ON ps.ProductSubcategoryKey = p.ProductSubcategoryKey
LEFT JOIN dbo.DimProductCategory AS pc ON ps.ProductCategoryKey = pc.ProductCategoryKey
ORDER BY p.ProductKey ASC
```

**Sales Fact Table**
```sql
SELECT ProductKey, OrderDateKey, DueDateKey, ShipDateKey, CustomerKey, SalesOrderNumber, SalesAmount
FROM AdventureWorksDW2019.dbo.FactInternetSales
WHERE LEFT(OrderDateKey, 4) >= YEAR(GETDATE()) - 2
ORDER BY OrderDateKey ASC
```

Export the cleansed tables to CSV using SQL Server Management Studio.

## Data Modelling

- Load CSVs into Power BI Desktop.
- Create a star schema: connect dimension tables to the fact table.
- Create measures:
  - **Sales**: `SUM(FACT_InternetSales[SalesAmount])`
  - **Budget**: `SUM(FACT_Budget[Budget])`
  - **Profit**: `[Sales] - [Budget]`

## Dashboard

Build three dashboards in Power BI:
- **Sales Overview**: KPIs, filters, category analysis, top customers/products, map visual.
- **Customer Details**: Top customers, monthly analysis, budget vs revenue.
- **Product Details**: Top products, sales over time, budget vs revenue.


## Conclusion

All the tasks mentioned in the BRD has been completed as per the specified acceptance criteria. Once the final report is prepared, we publish the dashboard to  Power BI service and schedule automated data refresh based on the client's specification.

This concludes a Business Analysis project of Customer Churn Prediction analysis.

©
