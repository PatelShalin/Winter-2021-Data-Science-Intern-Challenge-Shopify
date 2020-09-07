# Winter-2021-Data-Science-Intern-Challenge-Shopify

#### Question 1: Given some sample data, write a program to answer the following: click here to access the required data set

On Shopify, we have exactly 100 sneaker shops, and each of these shops sells only one model of shoe. We want to do some analysis of the average order value (AOV). When we look at orders data over a 30 day window, we naively calculate an AOV of $3145.13. Given that we know these shops are selling sneakers, a relatively affordable item, something seems wrong with our analysis. 

#### Think about what could be going wrong with our calculation. Think about a better way to evaluate this data.

Since each order can have multiple sneakers being bought, calculating the AOV by using the order amount is not viable. The reason the AOV is so high is because of anomalies such as the orders where 2000 items are being bought for a total order value of 704000, hence this drives up the AOV. A better way to evaluate this data is adding a column called ‘price_per_item’ where for each row we take the order_amount / total_items. Now when we take the mean of the ‘price_per_item’ column, we obtain a much more reasonable average of 387.74

#### What metric would you report for this dataset?

The metric I would report for this dataset is the median. The median is more robust to outliers such as the 704000 order_amount, hence we will get a more accurate representation of the average order value.

#### What is its value?

The median order value according to order_amount is 284, which is much more reasonable than 3145.13 and more reasonable than 387.74

#### Question 2: For this question you’ll need to use SQL. Follow this link to access the data set required for the challenge. Please use queries to answer the following questions. Paste your queries along with your final numerical answers below.

#### How many orders were shipped by Speedy Express in total?

Answer: 54

SQL Query:
SELECT * FROM ORDERS WHERE ShipperID IS 1;

#### What is the last name of the employee with the most orders?

Answer: Peacock

SQL Query:
SELECT LastName FROM EMPLOYEES
INNER JOIN 
(SELECT MAX("NUM_ORDERS"), EmployeeID as "MOST_ORDERS" FROM
(SELECT COUNT(OrderID) as "NUM_ORDERS", EmployeeID
FROM Orders
GROUP BY EmployeeID))
ON EMPLOYEES.EmployeeID == "MOST_ORDERS";

#### What product was ordered the most by customers in Germany?

Answer: Steeleye Stout

SQL Query:
SELECT ProductName FROM PRODUCTS
INNER JOIN
(SELECT MAX("MOST"), ProductID as "MOST_BOUGHT" FROM
(SELECT Quantity as "MOST", ProductID FROM ORDERDETAILS WHERE OrderID IN
(SELECT OrderID FROM ORDERS WHERE CustomerID IN
(SELECT CustomerID FROM CUSTOMERS WHERE Country == "Germany"))
GROUP BY ProductID))
ON ProductID == "MOST_BOUGHT";
