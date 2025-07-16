create database e_commerce;

use e_commerce;

-- Analyze the data
-- You can analyze all the tables by describing their contents.
-- Task: Describe the Tables:
-- Customers
-- Products
-- Orders
-- OrderDetails

DESCRIBE customers;
DESCRIBE products;
DESCRIBE orders;
DESCRIBE order_details;

-- ------------------------------------------------------------------------------------------------------------------------------------
-- Market Segmentation Analysis
-- Identify the top 3 cities with the highest number of customers to determine key markets for 
-- targeted marketing and logistic optimization.

SELECT 
    Location, COUNT(*) AS Number_of_Customers
FROM
    customers
GROUP BY Location
ORDER BY Number_of_Customers DESC
LIMIT 3;

-- --------------------------------------------------------------------------------------------------------------------------------------
-- Top Cities
-- As per the last query's result, Which of the cities must be focused as a part of marketing strategies

-- Delhi, Chennai, Jaipur

-- -------------------------------------------------------------------------------------------------------------------------------------
-- Engagement Depth Analysis
-- Determine the distribution of customers by the number of orders placed. This insight will help in segmenting customers into one-time buyers, occasional shoppers,
-- and regular customers for tailored marketing strategies.

-- Hint:
-- Use the “Orders” table.
-- Return the result table which helps you to segment customers on the basis of the number of orders in ascending order.

with customer_count as (select customer_id,count(*) as Number_of_orders from orders
group by customer_id)
select Number_of_orders,count(*) as CustomerCount from customer_count
group by Number_of_orders
order by Number_of_orders;

-- ---------------------------------------------------------------------------------------------------------------------------------------
-- Trend
-- As per the Engagement Depth Analysis question, What is the trend of the number of customers v/s number of orders?

-- As the number of orders increases, customer count is decreases. 

-- ----------------------------------------------------------------------------------------------------------------------------------------
-- Customer Category
-- As per the Engagement Depth Analysis question, Which customers category does the company experiences the most?

-- Occasional Customers

-- -----------------------------------------------------------------------------------------------------------------------------------------
-- Purchase High Value Products
-- identify products where the average purchase quantity per order is 2 but with a high total revenue, suggesting premium product trends.

-- Hint:
-- Use “OrderDetails”.
-- Return the result table which includes average quantity and the total revenue in descending order.

SELECT 
    product_id,
    AVG(quantity) AS avgquantity,
    SUM(quantity * price_per_unit) AS totalrevenue
FROM
    order_details
GROUP BY product_id
HAVING AVG(quantity) = 2
ORDER BY totalrevenue DESC;

-- -------------------------------------------------------------------------------------------------------------------------------------------
-- Highest Revenue
-- Among products with an average purchase quantity of two, which ones exhibit the highest total revenue?

-- Product 1

-- -------------------------------------------------------------------------------------------------------------------------------------------
-- Category Wise Customer Reach
-- For each product category, calculate the unique number of customers purchasing from it. This will help understand which categories have wider appeal across the customer base.

-- Hint:
-- Use the “Products”, “OrderDetails” and “Orders” table.
-- Return the result table which will help you count the unique number of customers in descending order.

select * from products;
select * from order_details;
select * from orders;

SELECT 
    p.category,
    COUNT(DISTINCT o.customer_id) AS unique_customers
FROM
    products AS p
        JOIN
    order_details AS od ON p.product_id = od.product_id
        JOIN
    orders AS o ON o.order_id = od.order_id
GROUP BY p.category
ORDER BY unique_customers DESC;


-- ---------------------------------------------------------------------------------------------------------------------------------------------
-- Product Category
-- As per the last question, Which product category needs more focus as it is in high demand among the customers?

-- Electronics

-- ----------------------------------------------------------------------------------------------------------------------------------------------
-- Sales Trend Analysis
-- Analyze the month-on-month percentage change in total sales to identify growth trends.

-- Hint:
-- Use the “Orders” table.
-- Return the result table which will help you get the month (YYYY-MM), Total Sales and Percent Change of the total amount (Present month value- Previous month value/ Previous month value)*100.
-- The resulting change in percentage should be rounded to 2 decimal places.

WITH MonthlySales AS (
    SELECT 
        DATE_FORMAT(order_date, '%Y-%m') AS month,
        SUM(total_amount) AS totalsales
    FROM Orders
    GROUP BY DATE_FORMAT(order_date, '%Y-%m')
),
SalesChange AS (
    SELECT 
        month,
        totalsales,
        LAG(totalsales) OVER (ORDER BY month) AS prev_month_sales
    FROM MonthlySales
)
SELECT 
    month,
    totalsales,
    ROUND((totalsales - prev_month_sales) / prev_month_sales * 100, 2) AS percentchange
FROM SalesChange;

-- -------------------------------------------------------------------------------------------------------------------------------------------------
-- Largest Decline
-- As per Sales Trend Analysis question, During which month did the sales experience the largest decline?

-- Feb 2024

-- --------------------------------------------------------------------------------------------------------------------------------------------------
-- Sales Trend
-- As per Sales Trend Analysis question, What could be inferred about the sales trend from March to August?

-- Sales fluctuated with no clear trend.

-- ---------------------------------------------------------------------------------------------------------------------------------------------------
-- Average Order Value Fluctuation
-- Examine how the average order value changes month-on-month. Insights can guide pricing and promotional strategies to enhance order value.

-- Hint:

-- Use the “Orders” Table.
-- Return the result table which will help you get the month (YYYY-MM), Average order value and Change in the average order value (Present month value- Previous month value).
-- Both the resulting AvgOrderValue and ChangeInValue column should be rounded to two decimal places, with the final results ordered in descending order by ChangeInValue.

select * from orders;

with monthlyaverageordervalue as (
    select 
    date_format(order_date,'%Y-%m') as Month,
    avg(total_amount) as AvgOrderValue
    from Orders
    Group By date_format(order_date,'%Y-%m')
),
ordervaluechange as(
    select
    Month,
    AvgOrderValue,
    lag(AvgOrderValue) Over (order by month) as prev_month_avg
    from monthlyaverageordervalue

)
select
Month,
AvgOrderValue,
round((AvgOrderValue-prev_month_avg),2) as ChangeInValue
from ordervaluechange
order by ChangeInValue desc;

-- ------------------------------------------------------------------------------------------------------------------------------------------------------
-- Highest Change
-- As per last question, Which month has the highest change in the average order value?

-- December

-- ------------------------------------------------------------------------------------------------------------------------------------------------------
-- Inventory Refresh Rate
-- Based on sales data, identify products with the fastest turnover rates, suggesting high demand and the need for frequent restocking.

SELECT 
    product_id, COUNT(*) AS SalesFrequency
FROM
    Order_Details
GROUP BY product_id
ORDER BY SalesFrequency DESC
LIMIT 5;

-- -------------------------------------------------------------------------------------------------------------------------------------------------------
-- Highest Turnover
-- As per last question, Which product_id has the highest turnover rates and needs to be restocked frequently?

-- 7

-- --------------------------------------------------------------------------------------------------------------------------------------------------------
-- Low Engagement Products
-- List products purchased by less than 40% of the customer base, indicating potential mismatches between inventory and customer interest.

-- Hint:
-- Use the “Products”, “Orders”, “OrderDetails” and “Customers” table.
-- Return the result table which will help you get the product names along with the count of unique customers who belong to the lower 40% of the customer pool.

SELECT 
    a.product_id,
    a.Name,
    COUNT(DISTINCT b.customer_id) AS uniquecustomercount
FROM
    Products AS a
        JOIN
    order_details AS c ON a.product_id = c.product_id
        JOIN
    orders AS b ON b.order_id = c.order_id
GROUP BY a.product_id , a.name
HAVING uniquecustomercount < (SELECT 
        COUNT(*)
    FROM
        customers) * 0.40;

-- ---------------------------------------------------------------------------------------------------------------------------------------------------------
-- Purchase Rate
-- Why might certain products have purchase rates below 40% of the total customer base?

-- Poor visibility on the platform.

-- ----------------------------------------------------------------------------------------------------------------------------------------------------------
-- Strategic Action 
-- After running an analysis to identify products purchased by less than 40% of the customer base, it was found that a few products have lower purchase rates than expected.
-- What could be a strategic action to improve the sales of these underperforming products?

-- Implemented targeted marketing campaigns to raise awareness and interest.

-- ----------------------------------------------------------------------------------------------------------------------------------------------------------
-- Customer Acquistion Trends
-- Evaluate the month-on-month growth rate in the customer base to understand the effectiveness of marketing campaigns and market expansion efforts.

SELECT 
    firstPurchaseMonth, SUM(NewCustomers) AS TotalNewCustomers
FROM
    (SELECT 
        DATE_FORMAT(MIN(order_date), '%Y-%m') AS firstPurchaseMonth,
            COUNT(DISTINCT customer_id) AS NewCustomers
    FROM
        orders
    GROUP BY customer_id) AS MonthlyNewCustomers
GROUP BY firstPurchaseMonth
ORDER BY firstPurchaseMonth;

-- ----------------------------------------------------------------------------------------------------------------------------------------------------------
-- Growth Trend
-- As per last question, What can be inferred about the growth trend in the customer base from the result table?

-- It is downward trend which implies the marketing campaign are not much effective. 

-- ----------------------------------------------------------------------------------------------------------------------------------------------------------
-- Peak Sales Period Identification
-- Identify the months with the highest sales volume, aiding in planning for stock levels, marketing efforts, and staffing in anticipation of peak demand periods.

SELECT 
    DATE_FORMAT(order_date, '%Y-%m') AS Month,
    SUM(total_amount) AS TotalSales
FROM
    Orders
GROUP BY DATE_FORMAT(order_date, '%Y-%m')
ORDER BY TotalSales DESC
LIMIT 3;

-- -----------------------------------------------------------------------------------------------------------------------------------------------------------
-- Restocking
-- As per last question, Which months will require major restocking of product and increased staffs?

-- September, December.
-- -----------------------------------------------------------------------------------------------------------------------------------------------------------

