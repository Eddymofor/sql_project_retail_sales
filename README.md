# Retal_Analysis Project

## Project Overview

**Project Title**: Retail Sales Analysis  
**Level**: Beginner  
**Database**: `p1_retail_db`

This project is designed to demonstrate MySQL skills and techniques typically used by data analysts to explore, clean, and analyze retail sales data. The project involves setting up a retail sales database, performing exploratory data analysis (EDA), and answering specific business questions through SQL queries. This project is ideal for those who are starting their journey in data analysis and want to build a solid foundation in S,MyQL.

## Objectives

1. **Set up a retail sales database**: Create and populate a retail sales database with the provided sales data.
2. **Data Cleaning**: Identify and remove any records with missing or null values.
3. **Exploratory Data Analysis (EDA)**: Perform basic exploratory data analysis to understand the dataset.
4. **Business Analysis**: Use MySQL to answer specific business questions and derive insights from the sales data.

## Project Structure

### 1. Database Setup

- **Database Creation**: The project starts by creating a database named `p1_retail_db`.
- **Table Creation**: A table named `retail_sales` is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.

CREATE DATABASE p1_retail_db;

CREATE TABLE retail_sales
(
    transactions_id INT PRIMARY KEY,
    sale_date DATE,	
    sale_time TIME,
    customer_id INT,	
    gender VARCHAR(10),
    age INT,
    category VARCHAR(35),
    quantity INT,
    price_per_unit FLOAT,	
    cogs FLOAT,
    total_sale FLOAT
);


### 2. Data Exploration & Cleaning

- **Record Count**: Determine the total number of records in the dataset.
- **Customer Count**: Find out how many unique customers are in the dataset.
- **Category Count**: Identify all unique product categories in the dataset.
- **Null Value Check**: Check for any null values in the dataset and delete records with missing data.


SELECT COUNT(*) FROM retail_sales;
SELECT COUNT(DISTINCT customer_id) FROM retail_sales;
SELECT DISTINCT category FROM retail_sales;

SELECT * FROM retail_sales
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;

DELETE FROM retail_sales
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;

### 3. Data Analysis & Findings

The following SQL queries were developed to answer specific business questions:

1. **Write a query to retrieve all columns from sales made on '2022-11-05**:
```MySQL
SELECT * 
FROM retail_sales
WHERE Sale_date = '2022-11-05';
```
2. **Write a query to retrieve all transactions where the category is clothing and the quantity sold is more than 4 in the month of Nov 2022**:
```MySQL
 SELECT *
   FROM retail_sales
   WHERE category = 'CLOTHING'
   AND
   sale_date ('YYY-MM') = '2022-11'
   AND
   Quantity>=4;
```
3. **Write a query to calculate the total sales (total_sale) for each category.**:
```MySQL
SELECT 
		Category,
        sum(total_sale) as net_sale,
        count(*) as total_orders
	FROM retail_sales
    GROUP BY 1;
```
4. **Write a query to find the Average age of Cusstomers who purchased items from the 'Beauty' Category.**:
```MySQL
 SELECT 
	ROUND(AVG(age), 2)
    fROM retail_sales
    WHERE Category = 'Beauty';
```
5. **Write a query to find all transactions where the total_sale is greater than 1000.**:
```MySQL
SELECT *
    FROM retail_sales
    WHERE
    Total_sale > 1000;
```
6. **Write a query to find the total number of transactions (transaction_id) made by each gender in each category.**:
```MySQL
SELECT 
		Category,
		Gender,
		COUNT(*) AS total_transactions
    FROM 
		retail_sales
    GROUP BY
		Category,
		Gender
    ORDER BY 1;
```
7. **Write a query to calculate the average sale for each month. Find out best selling month in each year**:
   
WITH monthly_sales AS (
    SELECT 
        YEAR(sale_date) AS sale_year,
        MONTH(sale_date) AS sale_month,
        AVG(total_sale) AS avg_monthly_sales
    FROM 
        retail_sales
    GROUP BY 
        YEAR(sale_date), MONTH(sale_date)
),
best_selling_months AS (
    SELECT 
        sale_year,
        sale_month,
        avg_monthly_sales
    FROM 
        monthly_sales
    WHERE 
        (sale_year, avg_monthly_sales) IN (
            SELECT 
                sale_year,
                MAX(avg_monthly_sales)
            FROM 
                monthly_sales
            GROUP BY 
                sale_year
        )
)
SELECT 
    sale_year,
    sale_month,
    avg_monthly_sales
FROM 
    best_selling_months
ORDER BY 
    sale_year, sale_month;
``
8. **Write a query to find the top 5 customers based on the highest total sales **:
```MySQL 
SELECT 
    customer_id,
    SUM(total_sale) as total_sales
FROM retail_sales
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5
```
9. **Write a query to find the number of unique customers who purchased items from each category.**:
```MySQL
 SELECT
		Category,
		COUNT(DISTINCT Customer_id) as Count_Unique_Customers
	FROM
		retail_sales
	GROUP BY Category;
```
10. **Write a query to create each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17)**:
```MySQL
WITH Hourly_sales AS 
    (SELECT *,
		CASE
			WHEN HOUR(sale_time) <12 THEN 'Morning'
            WHEN HOUR(sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
            ELSE 'Evening'
		END AS Shift
    FROM retail_sales
    )
SELECT
	Shift,
    COUNT(*) AS Total_orders
FROM  Hourly_sales
GROUP BY Shift
```

## Findings

- **Customer Demographics**: The dataset includes customers from various age groups, with sales distributed across different categories such as Clothing and Beauty.
- **High-Value Transactions**: Several transactions had a total sale amount greater than 1000, indicating premium purchases.
- **Sales Trends**: Monthly analysis shows variations in sales, helping identify peak seasons.
- **Customer Insights**: The analysis identifies the top-spending customers and the most popular product categories.

## Reports

- **Sales Summary**: A detailed report summarizing total sales, customer demographics, and category performance.
- **Trend Analysis**: Insights into sales trends across different months and shifts.
- **Customer Insights**: Reports on top customers and unique customer counts per category.

## Conclusion

This project serves as a comprehensive introduction to MySQL for data analysts, covering database setup, data cleaning, exploratory data analysis, and business-driven MySQL queries. The findings from this project can help drive business decisions by understanding sales patterns, customer behavior, and product performance.

## How to Use

1. **Clone the Repository**: Clone this project repository from GitHub.
2. **Set Up the Database**: Run the MySQL scripts provided in the `database_setup.mysql` file to create and populate the database.
3. **Run the Queries**: Use the MySQL queries provided in the `analysis_queries.mysql` file to perform your analysis.
4. **Explore and Modify**: Feel free to modify the queries to explore different aspects of the dataset or answer additional business questions.

## Author - EDWIN ACHU

This project is part of my portfolio, showcasing the MySQL skills essential for data analyst roles. If you have any questions, feedback, or would like to collaborate, feel free to get in touch!


Thank you and I look forward to connecting with you!
