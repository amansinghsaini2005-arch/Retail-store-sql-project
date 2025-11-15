🛍️ Retail Store Sales Analysis — SQL Project

A complete SQL-based data analysis project designed to explore, clean, and analyze retail sales data.
This project demonstrates skills in data cleaning, exploratory data analysis (EDA), and business insights generation using SQL.

📌 Project Overview

In this project, we analyze a retail store dataset containing sales transactions, customer details, product categories, and sale metadata.

This README includes:

Database & table creation

Data cleaning

Data exploration

10+ business problem SQL queries

Insights on customers, categories, and sales performance

🗂️ Database & Table Creation
CREATE DATABASE retail_store;

CREATE TABLE retail_sales (
    transaction_id INT PRIMARY KEY,
    sale_date DATE,
    sale_time TIME,
    customer_id INT,
    gender VARCHAR(15),
    age INT,
    category VARCHAR(15),
    quantity INT,
    price_per_unit FLOAT,
    cogs FLOAT,
    total_sale FLOAT
);

🧼 Data Cleaning
SELECT *
FROM retail_sales
WHERE 
    transaction_id IS NULL
    OR sale_date IS NULL
    OR sale_time IS NULL
    OR customer_id IS NULL
    OR gender IS NULL
    OR age IS NULL
    OR category IS NULL
    OR quantity IS NULL
    OR price_per_unit IS NULL
    OR cogs IS NULL
    OR total_sale IS NULL;


Delete invalid records:

DELETE FROM retail_sales
WHERE 
    transaction_id IS NULL
    OR sale_date IS NULL
    OR sale_time IS NULL
    OR gender IS NULL
    OR category IS NULL
    OR quantity IS NULL
    OR cogs IS NULL
    OR total_sale IS NULL;

🔍 Data Exploration
Total Sales Count
SELECT COUNT(*) AS total_sales FROM retail_sales;

Unique Customers
SELECT COUNT(DISTINCT customer_id) AS unique_customers FROM retail_sales;

All Product Categories
SELECT DISTINCT category FROM retail_sales;

📊 Business Questions & SQL Solutions
1️⃣ Sales on a Specific Date
SELECT *
FROM retail_sales
WHERE sale_date = '2020-11-05';

2️⃣ Clothing orders with quantity > 4 (Nov 2022)
SELECT *
FROM retail_sales
WHERE category = 'Clothing'
  AND DATE_FORMAT(sale_date, '%Y-%m') = '2022-11'
  AND quantity >= 4;

3️⃣ Total Sales per Category
SELECT 
    category,
    SUM(total_sale) AS net_sale,
    COUNT(*) AS total_orders
FROM retail_sales
GROUP BY category;

4️⃣ Avg Age of Beauty Category Shoppers
SELECT ROUND(AVG(age), 2) AS avg_age
FROM retail_sales
WHERE category = 'Beauty';

5️⃣ Transactions with Sales > 1000
SELECT *
FROM retail_sales
WHERE total_sale > 1000;

6️⃣ Gender-wise Transactions per Category
SELECT 
    category,
    gender,
    COUNT(*) AS total_trans
FROM retail_sales
GROUP BY category, gender
ORDER BY category;

7️⃣ Best-Selling Month of Each Year
SELECT 
    year,
    month,
    avg_sale
FROM (
    SELECT 
        EXTRACT(YEAR FROM sale_date) AS year,
        EXTRACT(MONTH FROM sale_date) AS month,
        AVG(total_sale) AS avg_sale,
        RANK() OVER (
            PARTITION BY EXTRACT(YEAR FROM sale_date)
            ORDER BY AVG(total_sale) DESC
        ) AS rnk
    FROM retail_sales
    GROUP BY 
        EXTRACT(YEAR FROM sale_date),
        EXTRACT(MONTH FROM sale_date)
) t1
WHERE rnk = 1;

8️⃣ Top 5 Customers by Total Sales
SELECT 
    customer_id,
    SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY customer_id
ORDER BY total_sales DESC
LIMIT 5;

9️⃣ Unique Customers per Category
SELECT 
    category,
    COUNT(DISTINCT customer_id) AS cnt_unique_cs
FROM retail_sales
GROUP BY category;

🔟 Orders by Shift (Morning, Afternoon, Evening)
WITH hourly_sale AS (
    SELECT *,
        CASE
            WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
            WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
            ELSE 'Evening'
        END AS shift
    FROM retail_sales
)
SELECT 
    shift,
    COUNT(*) AS total_orders
FROM hourly_sale
GROUP BY shift;

🎯 Conclusion

This SQL project demonstrates:

✔️ Real-world data cleaning
✔️ Business-driven analysis
✔️ Use of advanced SQL (Window Functions, CASE, Aggregations)
✔️ Insights useful for retail strategy
