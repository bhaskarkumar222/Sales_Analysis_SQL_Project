# Sales_Analysis_SQL_Project
## Project Overview
**Project Title:** Sales Analysis
* This project showcases basic SQL skills used by data analysts to explore, clean, and analyze retail sales data. It includes setting up a sales database, doing exploratory data analysis (EDA), and answering key business questions with SQL queries. It's perfect for beginners in data analysis. The goal is to help build a strong foundation in SQL.
## Objective
* **Set up a sales database:** Created and filled a retail sales database with the provided sales data.
* **Data Cleaning:** Found and removed any missing or incorrect data to ensure accuracy.
* **Exploratory Data Analysis (EDA):** Analyzed the data to understand key patterns and trends.
* **Business Analysis:** Used SQL to answer important business questions and gain insights from the sales data.
## Project Structure
### 1. Database Setup
* **Database Creation:** The project starts by creating a database named sql_sales_project.
* **Table Creation:** A table named retail_sales is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.
``` sql

CREATE DATABASE sql_sales_project;

USE sql_sales_project;

-- Create TABLE
DROP TABLE IF EXISTS retail_sales;
CREATE TABLE retail_sales
            (
                transaction_id INT PRIMARY KEY,	
                sale_date DATE,	 
                sale_time TIME,	
                customer_id	INT,
                gender	VARCHAR(15),
                age	INT,
                category VARCHAR(15),	
                quantity	INT,
                price_per_unit FLOAT,	
                cogs	FLOAT,
                total_sale FLOAT
            );

```
## 2. Data Exploration & Cleaning
* **Record Count:** Determine the total number of records in the dataset.
* **Customer Count:** Find out how many unique customers are in the dataset.
* **Category Count:** Identify all unique product categories in the dataset.
* **Null Value Check:** Check for any null values in the dataset and delete records with missing data.
```sql

SELECT 
    COUNT(*) 
FROM retail_sales;

-- Data Cleaning
SELECT * FROM retail_sales
WHERE transaction_id IS NULL;

SELECT * FROM retail_sales
WHERE sale_date IS NULL;

SELECT * FROM retail_sales
WHERE sale_time IS NULL;

SELECT * FROM retail_sales
WHERE 
    transaction_id IS NULL
    OR
    sale_date IS NULL
    OR 
    sale_time IS NULL
    OR
    gender IS NULL
    OR
    category IS NULL
    OR
    quantity IS NULL
    OR
    cogs IS NULL
    OR
    total_sale IS NULL;
    
-- Deleating NULL rows
DELETE FROM retail_sales
WHERE 
    transaction_id IS NULL
    OR
    sale_date IS NULL
    OR 
    sale_time IS NULL
    OR
    gender IS NULL
    OR
    category IS NULL
    OR
    quantity IS NULL
    OR
    cogs IS NULL
    OR
    total_sale IS NULL;
    
-- Data Exploration

-- How many sales we have?
SELECT COUNT(*) as total_sale FROM retail_sales;

-- How many uniuque customers we have ?

SELECT COUNT(DISTINCT customer_id) as total_sale FROM retail_sales;

SELECT DISTINCT category FROM retail_sales;

```
## 3. Data Analysis

#### Data Analysis & Business Key Problems & Answers

#### My Analysis & Findings
 1. Write a SQL query to retrieve all columns for sales made on '2022-11-05
 2. Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 10 in the month of Nov-2022
 3. Write a SQL query to calculate the total sales (total_sale) for each category.
 4. Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.
 5. Write a SQL query to find all transactions where the total_sale is greater than 1000.
 6. Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.
 7. Write a SQL query to calculate the average sale for each month. Find out best selling month in each year
 8. Write a SQL query to find the top 5 customers based on the highest total sales 
 9. Write a SQL query to find the number of unique customers who purchased items from each category.
 10. Write a SQL query to create each shift and number of orders (Example Morning <=12, Afternoon Between 12 & 17, Evening >17)

The following SQL queries were created to answer specific business questions.

**Q.1 Write a SQL query to retrieve all columns for sales made on '2022-11-05**

```sql
SELECT *
FROM retail_sales
WHERE sale_date = '2022-11-05';
```

**Q.2 Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 4 in the month of Nov-2022**

```sql
SELECT 
  *
FROM retail_sales
WHERE 
    category = 'Clothing'
    AND 
    TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
    AND
    quantity >= 4;
```

**Q.3 Write a SQL query to calculate the total sales (total_sale) for each category.**

``` sql
SELECT 
    category,
    SUM(total_sale) as net_sale,
    COUNT(*) as total_orders
FROM retail_sales
GROUP BY 1;
```

**Q.4 Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.**

```sql
SELECT
    ROUND(AVG(age), 2) as avg_age
FROM retail_sales
WHERE category = 'Beauty';
```

**Q.5 Write a SQL query to find all transactions where the total_sale is greater than 1000.**

```sql
SELECT * FROM retail_sales
WHERE total_sale > 1000;
```

**Q.6 Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.**

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
ORDER BY 1;
```

**Q.7 Write a SQL query to calculate the average sale for each month. Find out best selling month in each year.**

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
    RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) as rank1
FROM retail_sales
GROUP BY 1, 2
) as t1
WHERE rank1 = 1;
```

**Q.8 Write a SQL query to find the top 5 customers based on the highest total sales**

```sql
SELECT 
    customer_id,
    SUM(total_sale) as total_sales
FROM retail_sales
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5;
```

**Q.9 Write a SQL query to find the number of unique customers who purchased items from each category.**

```sql
SELECT 
    category,    
    COUNT(DISTINCT customer_id) as cnt_unique_cs
FROM retail_sales
GROUP BY category;
```


**Q.10 Write a SQL query to create each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17)**

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
GROUP BY shift;
```

**-- End of project**


### Key Findings:
* **Customer Demographics:** Customers spanned various age groups, with diverse product preferences across categories.
* **High-Value Transactions:** Noticed significant high-value transactions, suggesting the presence of premium customers.
* **Sales Trends:** Identified seasonal sales peaks through monthly trend analysis.
* **Customer Insights:** Identified the top-spending customers and most popular product categories.

### Reports Generated:
* **Sales Summary Report:** Summarized total sales, customer demographics, and product category performance.
* **Trend Analysis Report:** Analyzed sales trends across different months to track shifts and peak periods.
* **Customer Insights Report:** Highlighted top customers and the unique customer count for each product category.

* **Impact:** The project offered data-driven insights that could help businesses understand sales patterns, customer behavior, and product performance, supporting strategic decision-making.

### Conclusion:
This project provided valuable data-driven insights into customer demographics, high-value transactions, seasonal sales trends, and top product categories. The reports generated, including sales summaries and trend analyses, offer actionable information that can help businesses better understand their sales patterns and customer behaviors. These findings can be used to enhance strategic decision-making, optimize product offerings, and improve customer engagement.




















