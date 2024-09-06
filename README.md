# Coffee Shop Sales Analysis Project

## Overview
This project is focused on analyzing sales data from a coffee shop to gain insights into sales trends, customer behavior, and product performance across different store locations. The analysis is done using SQL queries for data transformation, aggregation, and detailed insights, combined with visualizations for a better understanding of sales patterns.



## Key Features

### Data Cleaning and Transformation: 
The sales data was cleaned by converting date and time columns into the correct format, renaming columns, and transforming the data to ensure consistency.

### SQL-Based Analysis:
A series of SQL queries were used to extract meaningful insights from the data, including:
  - Total sales, total orders, and total quantity sold.
 - Month-over-month (MoM) growth and differences.
 - Sales trends based on time, location, and product categories.

### Visualizations:
Visuals were created to understand better trends like sales over time, top products, and store performance.

## Dataset
The dataset contains the following fields:

  - Transaction_id: A unique ID for each transaction.
  - Transaction_date: The date the transaction occurred.
  - Transaction_time: The time of the transaction.
  - Store_location: The location where the transaction took place.
  - Product_category: The category of the products sold.
  - Product_type: The specific type of product sold.
  - Unit_price: Price per unit of the product.
  - Transaction_qty: Quantity of products sold in a transaction.

## SQL Queries
  Here are some key SQL queries used in the analysis:
  
Total Sales for May:

  ```bash
      SELECT ROUND(SUM(unit_price * transaction_qty)) AS Total_Sales 
      FROM coffee_shop_sales 
      WHERE MONTH(transaction_date) = 5;

```
Total quantity sold KPI - mom difference and mom growth

 ```bash
SELECT 
    MONTH(transaction_date) AS month,
    ROUND(SUM(transaction_qty)) AS total_quantity_sold,
    (SUM(transaction_qty) - LAG(SUM(transaction_qty), 1) 
    OVER (ORDER BY MONTH(transaction_date))) / LAG(SUM(transaction_qty), 1) 
    OVER (ORDER BY MONTH(transaction_date)) * 100 AS mom_increase_percentage
FROM 
    coffee_shop_sales
WHERE 
    MONTH(transaction_date) IN (4, 5)   -- for April and May
GROUP BY 
    MONTH(transaction_date)
ORDER BY 
    MONTH(transaction_date);
```

Daily sales for the month selected

 ```bash
SELECT 
    DAY(transaction_date) AS day_of_month,
    ROUND(SUM(unit_price * transaction_qty),1) AS total_sales
FROM 
    coffee_shop_sales
WHERE 
    MONTH(transaction_date) = 5  -- Filter for May
GROUP BY 
    DAY(transaction_date)
ORDER BY 
    DAY(transaction_date);
```
-------> COMPARING DAILY SALES WITH AVERAGE SALES – IF GREATER THAN “ABOVE AVERAGE” and LESSER THAN “BELOW AVERAGE”

 ```bash
SELECT 
    day_of_month,
    CASE 
        WHEN total_sales > avg_sales THEN 'Above Average'
        WHEN total_sales < avg_sales THEN 'Below Average'
        ELSE 'Average'
    END AS sales_status,
    total_sales
FROM (
    SELECT 
        DAY(transaction_date) AS day_of_month,
        SUM(unit_price * transaction_qty) AS total_sales,
        AVG(SUM(unit_price * transaction_qty)) OVER () AS avg_sales
    FROM 
        coffee_shop_sales
    WHERE 
        MONTH(transaction_date) = 5  -- Filter for May
    GROUP BY 
        DAY(transaction_date)
) AS sales_data
ORDER BY 
    day_of_month;
```
#### Additional SQL queries covering aspects like store sales performance, daily sales trends, and sales comparison by weekdays and weekends can be found in the MY SQL Queries.docx file.

## Visualizations
Several visualizations have been created to represent the following insights:

Monthly Sales Trend: Track the rise or fall of sales over different months.
Top-Selling Products: Identify the best-performing products based on sales.
Store Performance: Analyze sales performance across different store locations.


