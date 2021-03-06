# Shopify-Data-Science-Challenge
Shopify Data Science Technical Challenge Submission (Winter 2022)

<h1>Question 1</h1>

<h4><ins>a) Think about what could be going wrong with our calculation. Think about a better way to evaluate this data.</ins></h4>

I believe that the main reason why the calculation was wrong is thatthe numbers used in the calculation were wrong. The average order value is calculated by dividing
the total value revenue by the number of items sold. So, in the current case, the total revenue is $15,725,640 and the total number of orders is 5,000.

This gives an AOV = $15,725,640 / 5,000 = $3,145.13. This is incorrect as 5,000 is the total number of transactions but not the number of items sold. 
The total number of items sold by all 100 shops is 43,936. This now gives an AOV = $15,725,640 / 43,936 = $357.92. 
This is value is much more likely as shoes are a relatively affordable item.

<h4><ins>b) What metric would you report for this dataset? </ins></h4>
  
A metric I would provide from this dataset is the average daily items sold. This is a useful metric to know because it allows the shops to gauge the amount of inventory that will be purchased each day. This can then help the shoe companies understand how many shoes to have in stock every day as well as help forecast when inventory will be low. Moreover, this metric allows us to understand the average daily revenue as the average daily revenue would be equal to the average order value multiplied by the average daily items sold.

<h4><ins>c) What is its value? </ins></h4>

The average daily items sold is 1,464.53. This leads you to have an average daily revenue of $524,184.58 ($357.92 x 1,464.53 = 524,184.58).

**<ins>Note:</ins>** After looking at the values I obtained I looked at the dataset provided to check if it matched. After some further analysis, it looked like these values were higher than expected, so I decided to obtain the average order value, average daily items sold, and average daily revenue per shop. This data can be found in "2019 Winter Data Science Intern Challenge Data Set (Per Shop).xlsx". So, when looking at this data we can understand why these metrics were still higher than expected, and this was because of outliers. From this new perspective, we can see that the shop with id number 78 has an average order value of $25,725. This was substantially higher when compared to the other shops which ranged between $90 and $400. Furthermore, the shop with the id number 42 has an average daily item sold of 1,135.43. This too was substantially higher when compared to the other shops which ranged between 2 and 5. Due to these two shops, we got metrics that were higher than expected. I also decided to graph these metrics to confirm these suspicions (below):

<img src="https://github.com/jaykodes/Shopify-DS-Challenge/blob/main/daily_items.png">

The red line represents the average daily items sold value of 1,464.53.

<img src="https://github.com/jaykodes/Shopify-DS-Challenge/blob/main/daily_revenue.png">

The red line represents the average daily revenue value of $524,184.58.

From the graphs above we can see an increase in average daily items sold on certain days which corresponds to the dates where shop #42 sold shoes, which then also, in part with shop #78 average order value, is the primary reasons for the anomalies in the average daily revenue chart. All of the code required for these computations and graphs can be found in "shopify.py"

<h2>Question 2:</h2>

<h4><ins>a) How many orders were shipped by Speedy Express in total?</ins></h4>

Answer: **54**

SQL Code: 
```
SELECT COUNT(ShipperID) AS SpeedyExpressTotalOrder
FROM Orders
WHERE ShipperID =
(
    SELECT ShipperID
    FROM Shippers
    WHERE ShipperName = "Speedy Express"
)
```
<h4><ins>b) What is the last name of the employee with the most orders?</ins></h4>

Answer: **Peacock**

SQL Code:
```
SELECT LastName
FROM Employees
WHERE EmployeeID =
(
    SELECT EmployeeID
    FROM Orders
    GROUP BY EmployeeID
    ORDER BY COUNT(EmployeeID) DESC
    LIMIT 1
)
```
<h4><ins>c) What product was ordered the most by customers in Germany?</ins></h4>

Answer: **Boston Crab Meat**<br>
<ins>Note:</ins> This assumes that we are looking at the overall quantity

SQL Code:
```
SELECT ProductName
FROM Products
WHERE ProductID =
(
    SELECT ProductID
    FROM OrderDetails
    INNER JOIN Orders ON OrderDetails.OrderID = Orders.OrderID
    INNER JOIN Customers ON Orders.CustomerID = Customers.CustomerID
    WHERE Country = "Germany"
    GROUP BY ProductID
    ORDER BY SUM(Quantity) DESC
    LIMIT 1
)
```

Answer: **Gorgonzola Telino**<br>
<ins>Note:</ins> This assumes that we are looking at the frequency of orders

SQL Code:
```
SELECT ProductName
FROM Products
WHERE ProductID =
(
    SELECT ProductID
    FROM OrderDetails
    INNER JOIN Orders ON OrderDetails.OrderID = Orders.OrderID
    INNER JOIN Customers ON Orders.CustomerID = Customers.CustomerID
    WHERE Country = "Germany"
    GROUP BY ProductID
    ORDER BY COUNT(ProductID) DESC
    LIMIT 1
)
```
