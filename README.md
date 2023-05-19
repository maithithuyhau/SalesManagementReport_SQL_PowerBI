# SalesManagementReport_SQL_PowerBI
## User stories & Business requests
An executive sales report for sales managers was the business request for this data analyst project. 
User stories were established to complete delivery and guarantee that acceptance criteria were upheld throughout the project based on the request that was made by the company.
<img width="353" alt="user story" src="https://github.com/maithithuyhau/SalesManagementReport_SQL_PowerBI/assets/93932176/0c1f3865-60fa-4586-acef-f61cfbb9058d">

## Data Transformation & Cleaning (SQL)
The following tables were retrieved using SQL to generate the appropriate data model for conducting analysis and meeting the business needs outlined in the user stories.

Sales budgets, one data source, were sent to the process in Excel format and afterwards joined in the data model.

The necessary SQL statements for data cleansing and transformation are listed below.

### DIM_Calendar:
      SELECT
      ,[DateKey]
      ,[FullDateAlternateKey] As Date
      ,[EnglishDayNameOfWeek]
      ,[WeekNumberOfYear] as weeknum
      ,[EnglishMonthName] as Month
      ,left ([EnglishMonthName],3) as Monthshort
      ,[MonthNumberOfYear] as MonthNo
      ,[CalendarQuarter] as Quarter
      ,[CalendarYear] as Year
      FROM [AdventureWorksDW2019].[dbo].[DimDate]
      ,Where [CalendarYear] >=2019
     
### DIM_Products:
      SELECT 
       p.[ProductKey], 
       p.[ProductAlternateKey] AS ProductItemCode, 
      p.[EnglishProductName] AS [Product Name], 
      ps.EnglishProductSubcategoryName AS [Sub Category], -- Joined in from Sub Category Table
      pc.EnglishProductCategoryName AS [Product Category], -- Joined in from Category Table
      p.[Color] AS [Product Color], 
      p.[Size] AS [Product Size], 
      p.[ProductLine] AS [Product Line], 
      p.[ModelName] AS [Product Model Name], 
      p.[EnglishDescription] AS [Product Description], 
      ISNULL (p.Status, 'Outdated') AS [Product Status] 
      FROM 
      [AdventureWorksDW2019].[dbo].[DimProduct] as p
      LEFT JOIN dbo.DimProductSubcategory AS ps ON ps.ProductSubcategoryKey = p.ProductSubcategoryKey 
      LEFT JOIN dbo.DimProductCategory AS pc ON ps.ProductCategoryKey = pc.ProductCategoryKey 
      order by 
      p.ProductKey asc
### DIM_Customers
      SELECT 
       c.customerkey AS CustomerKey, 
       c.firstname AS [First Name], 
       c.lastname AS [Last Name], 
      c.firstname + ' ' + lastname AS [Full Name], 
      CASE c.gender WHEN 'M' THEN 'Male' WHEN 'F' THEN 'Female' END AS Gender,
      c.datefirstpurchase AS DateFirstPurchase, 
      g.city AS [Customer City] -- Joined in Customer City from Geography Table
      FROM 
      [AdventureWorksDW2019].[dbo].[DimCustomer] as c
      LEFT JOIN dbo.dimgeography AS g ON g.geographykey = c.geographykey 
      ORDER BY 
      CustomerKey ASC -- Ordered List by CustomerKey
### FACT_InternetSales
      SELECT 
      [ProductKey], 
      [OrderDateKey], 
      [DueDateKey], 
      [ShipDateKey], 
      [CustomerKey], 
      [SalesOrderNumber], 
      [SalesAmount]
      FROM 
       [AdventureWorksDW2019].[dbo].[FactInternetSales]
      WHERE 
       LEFT (OrderDateKey, 4) >= YEAR(GETDATE()) -2 -- Ensures we always only bring two years of date from extraction.
      ORDER BY
      OrderDateKey ASC
## Modeling Data
- The data model after the cleaned and prepped tables were read into Power BI is shown in the screenshot below.

- This data model also demonstrates the connections between FACT_Budget, FACT_InternetSales, and other essential DIM tables.
<img width="373" alt="01" src="https://github.com/maithithuyhau/SalesManagementReport_SQL_PowerBI/assets/93932176/5a5954a6-16a4-4d27-8b7a-1327ad1bcd42">
## Management Dashboard for Sales
One page serves as the dashboard and overview for the completed sales management dashboard, while the other two pages combine tables to provide the essential details and visualizations to show sales over time, by customers, and by items.
<img width="643" alt="dashboard" src="https://github.com/maithithuyhau/SalesManagementReport_SQL_PowerBI/assets/93932176/cd6b4c9b-a45c-46ab-9bb8-b247896b832f">



