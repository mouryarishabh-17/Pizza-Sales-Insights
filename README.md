# 🍕 Pizza Sales Data Analysis - SQL Insights

This repository showcases a comprehensive SQL-based data analysis project focused on pizza sales data. The objective was to extract actionable insights, optimize business strategies, and drive data-driven decision-making for a hypothetical pizza restaurant.

# Project Overview
This project provides a robust set of SQL queries designed to analyze pizza_sales data, offering a deep dive into various aspects of the business. From understanding overall revenue and order volumes to identifying top-selling pizzas and daily/monthly sales patterns, these queries empower strategic insights. The analysis focuses on transforming raw transactional data into meaningful business intelligence, crucial for operational efficiency and growth.

# Key Business Questions Answered
This analysis provides answers to critical business questions, including:

What are the overall financial performance metrics (total revenue, average order value)?
What are the peak sales periods (daily and monthly trends)?
How do different pizza categories and sizes contribute to overall sales?
Which pizzas are the most and least popular by revenue, quantity, and total orders?
How can inventory, marketing, and pricing strategies be optimized based on sales performance?

# Key Performance Indicators (KPIs)
Core business metrics are calculated to provide a quick snapshot of performance:

# 1. Total Revenue: 
SELECT SUM(total_price) AS Total_Revenue FROM pizza_sales;

# 2. Average Order Value: 
SELECT (SUM(total_price) / COUNT(DISTINCT order_id)) AS Avg_order_Value FROM pizza_sales;

# 3. Total Pizzas Sold: 
SELECT SUM(quantity) AS Total_pizza_sold FROM pizza_sales;

# 4. Total Orders: 
SELECT COUNT(DISTINCT order_id) AS Total_Orders FROM pizza_sales;

# 5. Average Pizzas Per Order: 
SELECT CAST(CAST(SUM(quantity) AS DECIMAL(10,2)) /
CAST(COUNT(DISTINCT order_id) AS DECIMAL(10,2)) AS DECIMAL(10,2))
AS Avg_Pizzas_per_order
FROM pizza_sales;

# Trend Analysis
Understanding sales over time helps in staffing, promotions, and inventory management:

# 1. Daily Trend for Total Orders: 
Identifies peak days (e.g., Fridays and Saturdays often show higher order volumes).

SELECT DATENAME(DW, order_date) AS order_day, COUNT(DISTINCT order_id) AS total_orders
FROM pizza_sales
GROUP BY DATENAME(DW, order_date);

# 2. Monthly Trend for Orders: 
Highlights monthly fluctuations in demand.

SELECT DATENAME(MONTH, order_date) as Month_Name, COUNT(DISTINCT order_id) as Total_Orders
FROM pizza_sales
GROUP BY DATENAME(MONTH, order_date);

# Sales Distribution
Insight into which categories and sizes drive revenue:

# 1. % of Sales by Pizza Category: 
Reveals the revenue contribution of each pizza type (e.g., 'Classic', 'Supreme', 'Veggie', 'Chicken').

SELECT pizza_category, CAST(SUM(total_price) AS DECIMAL(10,2)) as total_revenue,
CAST(SUM(total_price) * 100 / (SELECT SUM(total_price) from pizza_sales) AS DECIMAL(10,2)) AS PCT
FROM pizza_sales
GROUP BY pizza_category;

# 2. % of Sales by Pizza Size: 
Shows customer preferences for pizza sizes (e.g., 'L' often being the most popular).

SELECT pizza_size, CAST(SUM(total_price) AS DECIMAL(10,2)) as total_revenue,
CAST(SUM(total_price) * 100 / (SELECT SUM(total_price) from pizza_sales) AS DECIMAL(10,2)) AS PCT
FROM pizza_sales
GROUP BY pizza_size
ORDER BY pizza_size;

# 3. Total Pizzas Sold by Pizza Category (Example: February): 
A specific query to demonstrate filtered analysis by month.

SELECT pizza_category, SUM(quantity) as Total_Quantity_Sold
FROM pizza_sales
WHERE MONTH(order_date) = 2
GROUP BY pizza_category
ORDER BY Total_Quantity_Sold DESC;

# Pizza Performance
Identifying top and bottom performers is crucial for menu optimization and marketing efforts:

# 1. Top 5 Pizzas by Revenue: 
Identifies high-value menu items (e.g., 'The Thai Chicken Pizza', 'The Barbecue Chicken Pizza').

SELECT Top 5 pizza_name, SUM(total_price) AS Total_Revenue
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Revenue DESC;

# 2. Bottom 5 Pizzas by Revenue: 
Pinpoints underperforming items that may need review or removal.

SELECT Top 5 pizza_name, SUM(total_price) AS Total_Revenue
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Revenue ASC;

# 3. Top 5 Pizzas by Quantity: 
Shows which pizzas are most frequently ordered.

SELECT Top 5 pizza_name, SUM(quantity) AS Total_Pizza_Sold
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Pizza_Sold DESC;

# 4. Bottom 5 Pizzas by Quantity: 
Further highlights less popular items.

SELECT TOP 5 pizza_name, SUM(quantity) AS Total_Pizza_Sold
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Pizza_Sold ASC;

# 5. Top 5 Pizzas by Total Orders: 
Indicates consistent favorites.

SELECT Top 5 pizza_name, COUNT(DISTINCT order_id) AS Total_Orders
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Orders DESC;

# 6. Bottom 5 Pizzas by Total Orders: 
Identifies items with low order frequency.

SELECT Top 5 pizza_name, COUNT(DISTINCT order_id) AS Total_Orders
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Orders ASC;

# Technologies Used

SQL (Transact-SQL): For querying and analyzing data.

Database Management System: Assumed to be a relational database (e.g., SQL Server, MySQL, PostgreSQL) capable of handling standard SQL functions.

# How to Use (For Technical Reviewers)

To execute these SQL queries, you will need access to a database containing a pizza_sales table. The table should include columns such as order_date, order_id, total_price, quantity, pizza_category, pizza_size, and pizza_name.

Simply copy and paste the desired query into your SQL client and execute. Queries can be adapted with WHERE clauses for specific filtering (e.g., WHERE pizza_category = 'Classic')


