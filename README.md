# E-Commerce Analytics Case Study

A real-world MySQL-based data analysis project that simulates the role of a Data Analyst in a growing e-commerce company. The project extracts actionable insights from customer, product, order, and order details data to support marketing, sales, inventory, and customer retention strategies.

## Project Overview

As a Data Analyst in a dynamic e-commerce company, the objective is to leverage SQL to analyze transactional data and deliver insights that drive strategic decisions across departments:

- Customer Insights → Tailor marketing and personalization  
- Product Analysis → Evaluate performance and assortment strategy  
- Sales Optimization → Identify trends, opportunities, and improvement areas  
- Inventory Management → Optimize stock levels and availability  

This case study covers end-to-end analytics: data exploration, cleaning checks, segmentation, trend analysis, performance evaluation, and business recommendations.

## Business Problems Addressed

- Identifying high-potential cities/markets for marketing & logistics  
- Understanding customer engagement depth (one-time vs regular buyers)  
- Discovering premium product trends and category popularity  
- Tracking month-on-month sales growth/decline and average order value changes  
- Detecting fast-moving vs slow-moving products for inventory planning  
- Evaluating customer base growth and marketing effectiveness  
- Identifying underperforming products and strategic actions  

## Dataset Schema

| Table          | Key Columns                                      |
|----------------|--------------------------------------------------|
| Customers      | customer_id, name, location                      |
| Products       | product_id, name, category, price                |
| Orders         | order_id, order_date, customer_id, total_amount  |
| OrderDetails   | order_id, product_id, quantity, price_per_unit   |

## Key Analyses & Questions Covered

1. Data exploration & table descriptions  
2. Top 3 cities by customer count (key markets)  
3. Customer distribution by number of orders (engagement depth)  
4. Product analysis: high revenue with low average quantity (premium products)  
5. Category-wise unique customer reach & demand focus  
6. Month-on-month sales percentage change & largest decline  
7. Month-on-month average order value trend  
8. Fastest turnover products (high demand & restock priority)  
9. Low purchase rate products (<40% of customer base)  
10. Month-on-month customer base growth  
11. Peak sales months (restocking & staffing planning)  

## Technologies Used

- MySQL 8.0+  
- SQL (Joins, CTEs, Window Functions, Aggregations, Date functions, Subqueries, Ranking)

## Business Insights & Recommendations

- Focus marketing & logistics in top 3 customer cities  
- Target regular/high-frequency customers with loyalty programs  
- Promote premium products (high revenue, low quantity)  
- Increase stock & staffing during peak sales months  
- Push underperforming products with discounts, bundling, or removal  
- Monitor month-on-month trends to adjust pricing, promotions, and inventory  
- Accelerate customer acquisition in slow-growth months  

