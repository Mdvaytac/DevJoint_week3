## Project Objective

This project focuses on building an end-to-end business intelligence solution using Microsoft Power BI. The main goal is to design a star schema data model, perform data import and transformation, create calculated fields and measures using DAX, and develop an interactive sales performance dashboard that provides actionable insights into sales, profit, customer behavior, product performance, and order delivery efficiency.

## Data Model Design (Star Schema)

The data model is built on a star schema architecture. The central fact table is FactSales, which stores individual sales transactions. It contains foreign keys that establish relationships with dimension tables, including DimDate2, DimProduct, DimCustomer, DimGeography, and DimEmployee. All relationships are configured as many-to-one, ensuring proper filter propagation from dimension tables to the fact table. This structure optimizes query performance and supports multi-dimensional analysis.

- FactSales: Stores sales transactions with measures such as SalesAmount, Profit, and Quantity, and includes foreign keys for OrderDate, Product, Customer, Geography, and Employee.

- DimDate2: Provides time-based attributes including Date, Day, Month, Quarter, IsHoliday, and IsWeekend.

- DimProduct: Contains product details such as ProductName, Category, SubCategory, Color, Size, ListPrice, and StandardCost.

- DimCustomer: Includes customer attributes like CustomerName, Surname, Email, Phone, Gender, and LoyaltyTier.

- DimGeography: Holds geographic information including City, Region, and Country.

- DimEmployee: Stores employee-related data such as EmployeeName, Role, and HireDate.

All tables were imported from a single source using Power Query.

## DAX Calculations

To enable deeper analysis, I created several calculated columns and measures.

Calculated Columns:

- OrderDate: Extracts the order date from the DimDate2 table using the RELATED function.
OrderDate = RELATED(DimDate2[Date])

- ShipDate: Extracts the shipment date from the DimDate2 table using the RELATED function.
ShipDate = RELATED(DimDate2[Date])

- DeliveryDelay: Computes the number of days between the order date and the shipment date using DATEDIFF.
DeliveryDelay = DATEDIFF(FactSales[OrderDate], FactSales[ShipDate], DAY)

- DelayCategory: Categorizes delivery delays into five groups using the SWITCH function: on time, 1-3 days, 4-7 days, 8-15 days, and 15+ days.
DelayCategory = SWITCH(TRUE(), FactSales[DeliveryDelay] <= 0, "on time", FactSales[DeliveryDelay] <= 3, "1-3 day", FactSales[DeliveryDelay] <= 7, "4-7 day", FactSales[DeliveryDelay] <= 15, "8-15 day", "15+ day")

Measures:

- ProfitMargin: Calculates the overall profit margin by dividing total profit by total sales amount using the DIVIDE function, returning 0 if the denominator is zero.
ProfitMargin = DIVIDE(SUM(FactSales[Profit]), SUM(FactSales[SalesAmount]), 0)

## Interactive Dashboard Elements

The dashboard includes a variety of visualizations designed to address specific business questions:

A map visual shows average profit by country, enabling geographic profitability comparison.

A column chart compares median profit, average total cost, and average sales amount across product categories, helping to assess category-level performance.

A line chart displays sales amount trends over years to identify growth or decline patterns.

A donut chart illustrates sales distribution by channel, highlighting the most revenue-generating channels.

A pie chart presents customer gender distribution for demographic analysis.

A column chart compares total product quantity shipped by delay category, offering insights into delivery performance.

## KPI Summary Cards

Key performance indicators are displayed prominently as summary cards, including total customers, total employees, average profit, average unit price, and profit margin. A dedicated card compares actual sales against a predefined target, clearly showing the percentage gap.

## Slicers (Filters)

Three interactive slicers allow users to filter data dynamically by country, product category, and date range. These filters enable users to explore data at different granularities and focus on specific segments of interest.

## Dashboard Design Principles

The layout follows a clear visual hierarchy, placing the most critical KPIs at the top, followed by detailed visualizations. A consistent color scheme is applied across all charts to reduce visual clutter and facilitate comparison. The dashboard is designed to be uncluttered, with only relevant metrics and visuals included to support informed decision-making.

## Conclusion

This Power BI project demonstrates a complete data modeling and visualization workflow, from importing data to creating a fully interactive dashboard. The integration of a star schema, DAX calculations, and purposefully selected visualizations results in a tool that allows stakeholders to monitor business performance, identify trends, and make data-driven decisions effectively.
