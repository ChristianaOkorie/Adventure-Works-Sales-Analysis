# Adventure-Works-Sales-Analysis
Adventure Works Sales Report With SQL

# Power BI Overview Dashboard
<img width="1130" height="606" alt="image" src="https://github.com/user-attachments/assets/edd008f0-0fe4-42a8-b6d1-4a7e7a2ef648" />

#SQL Queries 
1) Total revenue, total orders and Avg order value
SELECT SUM(TotalDue) AS Total_Revenue ,
	   AVG(TotalDue) AS Avg_Revenue,
	   COUNT(SalesOrderID) AS Total_Order
FROM Sales.SalesOrderHeader

<img width="462" height="137" alt="image" src="https://github.com/user-attachments/assets/db627da5-c0b6-4414-9702-486f6ec65106" />

2)  Revenue trend year and month
SELECT
	YEAR(OrderDate) AS Year,
	MONTH(OrderDate) AS Month,
	SUM(TotalDue) AS Revenue
FROM Sales.SalesOrderHeader
GROUP BY YEAR(OrderDate), MONTH(OrderDate)
ORDER BY YEAR, MONTH

<img width="261" height="219" alt="image" src="https://github.com/user-attachments/assets/4986794a-6276-4f8c-a6f8-d2409782f419" />

3)




