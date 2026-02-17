# Retail Sales Analysis SQL & POWER BI Project

## Project Overview

**Project Title**: Retail Sales Analysis  
**Level**: Medium  
**Database**: `sql_project1_db`

This project demonstrates real-world SQL skills and data analysis techniques used by data analysts to explore, clean, and analyze retail sales data.

The project covers the complete analytics workflow:

Database creation

Data cleaning

Exploratory Data Analysis (EDA)

Business-driven SQL queries

Interactive Power BI Dashboard visualization

This project is part of my portfolio showcasing practical SQL and Business Intelligence skills required for Data Analyst roles.

## Objectives

1. **Set up a retail sales database**: Create and populate a retail sales database with the provided sales data.
2. **Data Cleaning**: Identify and remove any records with missing or null values.
3. **Exploratory Data Analysis (EDA)**: Perform basic exploratory data analysis to understand the dataset.
4. **Business Analysis**: Use SQL to answer specific business questions and derive insights from the sales data.

## Project Structure

### 1. Database Setup

- **Database Creation**: The project starts by creating a database named `p1_retail_db`.
- **Table Creation**: A table named `retail_sales` is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.

```sql
CREATE DATABASE sql_project_1;

DROP TABLE IF EXISTS retail_sales;
CREATE TABLE retail_sales (
    transactions_id INTEGER PRIMARY KEY,
    sale_date DATE NOT NULL,
    sale_time TIME NOT NULL,
    customer_id INTEGER NOT NULL,
    gender VARCHAR(10) NOT NULL,
    age INTEGER,
    category VARCHAR(50),
    quantity INTEGER,
    price_per_unit NUMERIC(10,2),
    cogs NUMERIC(10,2),
    total_sale NUMERIC(12,2)
);

```

### 2. Data Exploration & Cleaning

- **Record Count**: Determine the total number of records in the dataset.
- **Customer Count**: Find out how many unique customers are in the dataset.
- **Category Count**: Identify all unique product categories in the dataset.
- **Null Value Check**: Check for any null values in the dataset and delete records with missing data.

```sql
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
```

### 3. Data Analysis & Findings

The following SQL queries were developed to answer specific business questions:

1. **Write a SQL query to retrieve all columns for sales made on '2022-11-05**:
```sql
SELECT *
FROM retail_sales
WHERE sale_date = '2022-11-05';
```

2. **Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 4 in the month of Nov-2022**:
```sql
SELECT 
  *
FROM retail_sales
WHERE 
    category = 'Clothing'
    AND 
    TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
    AND
    quantity >= 4
```

3. **Write a SQL query to calculate the total sales (total_sale) for each category.**:
```sql
SELECT 
    category,
    SUM(total_sale) as net_sale,
    COUNT(*) as total_orders
FROM retail_sales
GROUP BY 1
```

4. **Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.**:
```sql
SELECT
    ROUND(AVG(age), 2) as avg_age
FROM retail_sales
WHERE category = 'Beauty'
```

5. **Write a SQL query to find all transactions where the total_sale is greater than 1000.**:
```sql
SELECT * FROM retail_sales
WHERE total_sale > 1000
```

6. **Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.**:
```sql
SELECT 
    category,
    gender,
    COUNT(*) as total_trans
FROM retail_sales
GROUP 
    BY 
    category,
    gender
ORDER BY 1
```

7. **Write a SQL query to calculate the average sale for each month. Find out best selling month in each year**:
```sql
SELECT 
       year,
       month,
    avg_sale
FROM 
(    
SELECT 
    EXTRACT(YEAR FROM sale_date) as year,
    EXTRACT(MONTH FROM sale_date) as month,
    AVG(total_sale) as avg_sale,
    RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) as rank
FROM retail_sales
GROUP BY 1, 2
) as t1
WHERE rank = 1
```

8. **Write a SQL query to find the top 5 customers based on the highest total sales **:
```sql
SELECT 
    customer_id,
    SUM(total_sale) as total_sales
FROM retail_sales
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5
```

9. **Write a SQL query to find the number of unique customers who purchased items from each category.**:
```sql
SELECT 
    category,    
    COUNT(DISTINCT customer_id) as cnt_unique_cs
FROM retail_sales
GROUP BY category
```

10. **Write a SQL query to create each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17)**:
```sql
WITH hourly_sale
AS
(
SELECT *,
    CASE
        WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
        WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
        ELSE 'Evening'
    END as shift
FROM retail_sales
)
SELECT 
    shift,
    COUNT(*) as total_orders    
FROM hourly_sale
GROUP BY shift
```

📈 4️⃣ Power BI Dashboard

In addition to SQL analysis, an interactive Power BI dashboard was developed to visualize business insights.

Dashboard Features

📌 Total Sales KPI

📌 Total Profit KPI (total_sale - cogs)

📌 Total Transactions

📌 Average Order Value

📌 Sales by Category

📌 Monthly Sales Trend

📌 Gender-wise Sales Distribution

📌 Top 5 Customers

📌 Sales by Shift

📌 Age Group Analysis

📌 Interactive Filters (Date, Category, Gender)

This dashboard enables dynamic exploration of sales performance and customer behavior.

🔍 Key Findings

Clothing and Beauty categories contributed the highest revenue.

Several high-value transactions (>1000) indicate premium purchases.

Monthly sales trends reveal seasonal fluctuations.

Evening shifts showed higher transaction volume.

Certain customers significantly contributed to overall revenue.

🛠 Tools & Technologies

PostgreSQL

SQL (Advanced Queries, CTE, Window Functions)

Power BI

Data Cleaning Techniques

Data Visualization

🚀 Project Highlights

End-to-end data analysis workflow

Business-focused SQL problem solving

Real-world KPI development

Interactive dashboard creation

Strong foundation for Data Analyst roles

## Conclusion

This project serves as a comprehensive introduction to SQL for data analysts, covering database setup, data cleaning, exploratory data analysis, and business-driven SQL queries. The findings from this project can help drive business decisions by understanding sales patterns, customer behavior, and product performance.

## How to Use

1. **Clone the Repository**: Clone this project repository from GitHub.
2. **Set Up the Database**: Run the SQL scripts provided in the `retail_sales_project1.sql` file to create and populate the database.
3. **Run the Queries**: Use the SQL queries provided in the `retail_sales_project1.sql` file to perform your analysis.
4. **Explore and Modify**: Feel free to modify the queries to explore different aspects of the dataset or answer additional business questions.

## Author - Ahammad Muvaj

This project is part of my portfolio, showcasing the SQL skills essential for data analyst roles. If you have any questions, feedback, or would like to collaborate, feel free to get in touch!

### Stay Updated and Connect with me in Social Media.

For more content on SQL, data analysis, and other data-related topics, make sure to follow me on social media:

- **Instagram**: [Follow me on Instagram](https://www.instagram.com/ahammad_muvaj/)
- **LinkedIn**: [Connect with me professionally](www.linkedin.com/in/ahammad-muvaj-388340316)

Thank you for your support, and I look forward to connecting with you!
