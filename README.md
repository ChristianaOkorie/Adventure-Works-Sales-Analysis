# Adventure-Works-Sales-Analysis
Adventure Works Sales Report With SQL

# Power BI Overview Dashboard
<img width="1130" height="606" alt="image" src="https://github.com/user-attachments/assets/edd008f0-0fe4-42a8-b6d1-4a7e7a2ef648" />


# SQL Queries 
# 1) Total revenue, total orders and Avg order value
SELECT SUM(TotalDue) AS Total_Revenue ,
	   AVG(TotalDue) AS Avg_Revenue,
	   COUNT(SalesOrderID) AS Total_Order
FROM Sales.SalesOrderHeader

<img width="462" height="137" alt="image" src="https://github.com/user-attachments/assets/db627da5-c0b6-4414-9702-486f6ec65106" />

# 2)  Revenue trend year and month
SELECT
	YEAR(OrderDate) AS Year,
	MONTH(OrderDate) AS Month,
	SUM(TotalDue) AS Revenue
FROM Sales.SalesOrderHeader
GROUP BY YEAR(OrderDate), MONTH(OrderDate)
ORDER BY YEAR, MONTH

<img width="261" height="219" alt="image" src="https://github.com/user-attachments/assets/4986794a-6276-4f8c-a6f8-d2409782f419" />

# 3) Top 10 customers by revenue
SELECT TOP 10
	CustomerID,
	ROUND(SUM(TotalDue), 2) AS Revenue 
FROM Sales.SalesOrderHeader 
GROUP BY CustomerID
ORDER BY Revenue DESC;

<img width="347" height="287" alt="image" src="https://github.com/user-attachments/assets/fc24dc4a-8271-488f-9109-f7383c5b26ba" />

# 4) Count of New user vs return user
WITH CustType AS
	(
	SELECT 
	CustomerID,
	CASE 
	WHEN COUNT(SalesOrderID) =1 THEN 'New User'
	ELSE 'Return User'
	END AS Customer_Status
FROM Sales.SalesOrderHeader
GROup BY customerID
	)
SELECT Customer_Status, COUNT(*) AS CustomerStatusCount
FROM CustType
GROUP BY Customer_Status;

<img width="339" height="154" alt="image" src="https://github.com/user-attachments/assets/ba087964-09bd-4603-9059-c7a3dd4cd97a" />

# 5) Avg quantity per order
WITH QtyPerOrder AS

(SELECT
salesorderid,
COUNT(orderqty) AS total_order
FROM Sales.SalesOrderDetail 
GROUP BY SalesOrderID
)

SELECT 
    AVG(total_order) AS AvgItemsPerOrder
FROM QtyPerOrder

<img width="328" height="144" alt="image" src="https://github.com/user-attachments/assets/8aff7ab1-b6fa-44d5-8868-9cf9c33b3d54" />

# 6) Month on month growth rate
WITH MonthlySales AS 
(
	SELECT 
		YEAR(OrderDate) AS Year,
		MONTH(OrderDate) AS Month,
		SUM(TotalDue) AS Revenue
	FROM Sales.SalesOrderHeader
	GROUP BY Year(OrderDate), Month(OrderDate)
)
SELECT
	Year,
	Month,
	ROUND(Revenue,2) Revenue,
	ROUND(LAG(Revenue) OVER(ORDER BY Year, Month),2) AS PreviousMonth,
	ROUND((Revenue - LAG(Revenue) OVER(ORDER BY Year, Month)) * 100
	/ (LAG(Revenue) OVER(ORDER BY Year, Month)),2) AS GrowthRate
FROM MonthlySales;

<img width="400" height="346" alt="image" src="https://github.com/user-attachments/assets/28a8afb4-a26c-4031-8ed5-3d9dd28c091c" />

# 7) Segmentation of customers
SELECT 
	CustomerID, 
	SUM(TotalDue) AS Revenue,
CASE 
	 WHEN SUM(TotalDue) > 100000  THEN 'Big Customer'
	 WHEN SUM(TotalDue) BETWEEN 50000 AND 100000 THEN 'Medium Customer'
	 ELSE 'Small Customer'
END AS CustomerSegment
FROM Sales.SalesOrderHeader
GROUP BY CustomerID;

<img width="487" height="371" alt="image" src="https://github.com/user-attachments/assets/3c3b9fec-b5d5-4c08-88cb-dd564b534b90" />

# 8) Sales by salesperson  WIP
SELECT	p.BusinessEntityID AS 'EmployeeID',
	p.FirstName, 
	p.LastName,
	p.FirstName + ' ' + LastName AS 'FullName',
	ROUND(SUM(oh.TotalDue),2) AS Revenue
FROM Person.Person p JOIN
		Sales.SalesOrderHeader oh ON oh.SalesPersonID = P.BusinessEntityID
GROUP BY p.BusinessEntityID, p.FirstName, p.LastName
ORDER BY Revenue;

<img width="573" height="364" alt="image" src="https://github.com/user-attachments/assets/29dc5236-6d61-41a3-9688-9fa1e974b21d" />










