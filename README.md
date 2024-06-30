## Walmart Sales Data Analysis

This project explores Walmart's sales data to understand top-performing branches and products, sales trends across different products, and customer behavior. The aim is to study how sales strategies can be improved and optimized. The dataset was obtained from the Kaggle Walmart Sales Forecasting Competition. [source](https://www.kaggle.com/c/walmart-recruiting-store-sales-forecasting/overview)

## Project Overview

This analysis covers the following aspects of Walmart's sales data:

1. **Sum of Each Product's Sales**
2. **Sales Trends by Time of Day and Month**
3. **Key Performance Indicators (KPIs)**
4. **Sales Analysis by Gender**
5. **Total Sales Sum**

## Data Preparation

To enhance the analysis, several columns were added to the dataset:

Tables:

|Column|Description|Data Type|
|---|---|---|
|invoice_id  |Invoice of the sales made  |NVARCHAR(50)   |
|branch |Branch at which sales were made |NVARCHAR(50)   |
|city |The location of the branch  |NVARCHAR(50)   |
|customer_type |The type of the customer  |NVARCHAR(50)   |
|gender  |Gender of the customer making purchase  |NVARCHAR(50)   |
|product_line |Product line of the product sold  |NVARCHAR(50)   |
|unit_price |The price of each product |FLOAT   |
|quantity |The amount of the product sold  |TINYINT  |
|VAT |The amount of tax on the purchase |FLOAT  |
|total |The total cost of the purchase |FLOAT   |
|date  |	The date on which the purchase was made |DATE   |
|time |The time at which the purchase was made |TIME   |
|payment_method  |The total amount paid  |NVARCHAR(50)  |
|cogs  |Cost Of Goods sold  |FLOAT    |
|gross_margin_percentage  |Gross margin percentage |FLOAT    |
|gross_income |Gross Income  |FLOAT    |
|rating  |Rating |FLOAT    |
|time_of_day |Time of day |NVARCHAR(50)    |
|day_name |Day name|NVARCHAR(50)   |
|month_name |Month name|NVARCHAR(50)   |



## Add time of day

```sql
--- time_of_day column

ALTER TABLE sales ADD time_of_day VARCHAR(20);

UPDATE sales
SET time_of_day = 
    CASE 
        WHEN time BETWEEN '00:00:00' AND '11:59:59' THEN 'Morning'
        WHEN time BETWEEN '12:00:00' AND '15:59:59' THEN 'Afternoon'
        ELSE 'Evening'
    END;
```

## Add day name

```sql

--- day_name column

ALTER TABLE sales ADD day_name VARCHAR(10);

UPDATE sales
SET day_name = DATENAME(WEEKDAY, date);

```

## Add month name

```sql

--- month_name column

ALTER TABLE sales ADD month_name VARCHAR(10);

UPDATE sales
SET month_name = DATENAME(MONTH, date);

```


## 

--------------------------- Questions Answered ----------------------------
```sql

--- How many unique cities does the data have?

SELECT DISTINCT city FROM sales;


--- In which city is each branch?

SELECT DISTINCT branch, city FROM sales;


--- How many unique product lines does the data have?

SELECT COUNT(DISTINCT product_line) AS unique_product_lines FROM sales;


--- What is the most selling product line (by quantity)?

SELECT product_line, SUM(quantity) AS qty
FROM sales
GROUP BY product_line
ORDER BY qty DESC;


--- What is the most selling product line based on the number of transactions?

SELECT product_line, COUNT(product_line) AS cnt
FROM sales
GROUP BY product_line
ORDER BY cnt DESC;


--- What is the total revenue by month?

SELECT month_name AS month, SUM(total) AS total_revenue
FROM sales
GROUP BY month_name
ORDER BY total_revenue DESC;


--- What product line had the largest revenue?

SELECT product_line, SUM(total) AS largest_revenue
FROM sales
GROUP BY product_line
ORDER BY largest_revenue DESC;


--- What month had the largest COGS?

SELECT month_name AS month, SUM(cogs) AS cogs
FROM sales
GROUP BY month_name
ORDER BY cogs DESC;


--- What is the city with the largest revenue?

SELECT branch, city, SUM(total) AS total_revenue
FROM sales
GROUP BY city, branch
ORDER BY total_revenue DESC;


--- What is the average rating of each product line?

SELECT product_line, ROUND(AVG(rating), 2) AS avg_rating
FROM sales
GROUP BY product_line
ORDER BY avg_rating DESC;


--- Number of sales made in each time of the day per weekday?

SELECT day_name, time_of_day, COUNT(*) AS total_sales
FROM sales
GROUP BY day_name, time_of_day
ORDER BY day_name, total_sales DESC;


--- Which customer types bring the most revenue?

SELECT customer_type, SUM(total) AS total_revenue
FROM sales
GROUP BY customer_type
ORDER BY total_revenue DESC;

```






