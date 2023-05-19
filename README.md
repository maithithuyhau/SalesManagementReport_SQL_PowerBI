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
[DateKey]
      ,[FullDateAlternateKey] As Date
      ,[EnglishDayNameOfWeek]
      ,[WeekNumberOfYear] as weeknum
      ,[EnglishMonthName] as Month
      ,left ([EnglishMonthName],3) as Monthshort
      ,[MonthNumberOfYear] as MonthNo
      ,[CalendarQuarter] as Quarter
      ,[CalendarYear] as Year
  FROM [AdventureWorksDW2019].[dbo].[DimDate]
  Where [CalendarYear] >=2019
