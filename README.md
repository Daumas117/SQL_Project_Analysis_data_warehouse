# 📊 SQL_Project_Analysis_data_warehouse


## 📝 Project Objective

The objective of this project is to do an Exploratory Data Analysis (EDA) to the information of a Data Warehouseby project previously worked on - https://github.com/Daumas117/SQL_Project_data_warehouse -.

## 🏗️ Data Architecture

![Data_Flow_Diagram](images/data_flow_diagram.png)

## **Database Exploration**

```sql
-- Database Exploration

select
	TABLE_CATALOG,
	TABLE_SCHEMA,
	TABLE_NAME,
	TABLE_TYPE
from INFORMATION_SCHEMA.tables; -- Retrieve a  list of all tables in the DB.
```
![Data_Flow_Diagram](images/1_Data_Exploration.png) 

We will be working with the following views:

- gold.dim_customers
- gold.dim_products
- gold.fact_sales


## **Dimension Exploration**

```sql
SELECT DISTINCT
	category,
	subcategory,
	product_name
FROM gold.dim_products
ORDER BY category, subcategory, product_name
```
![Data_Flow_Diagram](images/2_Dimension_Exploration.png)

With this information, we can assume that we will be working with the following format of information.

![Data_Flow_Diagram](images/Dimensions_Exploration.png)

## **Data Exploration**

```sql
-- Find the Total Sales

SELECT sum(sales_amount) as total_sales
FROM gold.fact_sales

-- Find how many items are sold.

SELECT sum(quantity) as total_items_sold
FROM gold.fact_sales

-- Find the average selling price

SELECT avg(price) as avg_price
FROM gold.fact_sales

-- Find the Total number of Orders

SELECT count(order_number) as total_number_orders
FROM gold.fact_sales
SELECT DISTINCT count(order_number) as total_number_orders
FROM gold.fact_sales

-- Find the total number of products
SELECT count(product_name) as total_product
FROM gold.dim_products

-- Find the total number of customers
SELECT count(customer_id) as total_number_customers
FROM gold.dim_customers

-- Find the total number of customers that has placed an order
SELECT count(DISTINCT customer_key) AS total_customers 
FROM gold.fact_sales;
```

![Data_Flow_Diagram](images/3_Data_Exploration.png)

With this information, we can create a report. We can have the information spread out, but we can summarize everything in a unique query.

```sql

-- Generate a Report that shows all key metrics of the business
SELECT 'Total Sales' as measure_name, sum(sales_amount) as measure_value FROM gold.fact_sales
UNION ALL 
SELECT 'Total Quantity' as measure_name, sum(quantity) as measure_value FROM gold.fact_sales
UNION ALL
SELECT 'Average Price', AVG(price) FROM gold.fact_sales
UNION ALL
SELECT 'Total Orders', COUNT(DISTINCT order_number) FROM gold.fact_sales
UNION ALL
SELECT 'Total Products', COUNT(DISTINCT product_name) FROM gold.dim_products
UNION ALL
SELECT 'Total Customers', COUNT(customer_key) FROM gold.dim_customers;
```

![Data_Flow_Diagram](images/4_Generated_Report.png)

