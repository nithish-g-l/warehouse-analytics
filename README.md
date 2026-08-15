# SQL Data Analysis Project

## Overview

This project focuses on **exploratory data analysis using SQL Server** to analyze customer, product, sales, and business performance data.

The analysis is performed on a **Gold Layer data warehouse**, using dimension and fact tables to generate meaningful business insights and KPIs.

The project demonstrates practical SQL skills including:

* Data exploration
* Dimension and measure analysis
* Aggregation
* Joins
* Ranking
* Time-series analysis
* Window functions
* Cumulative analysis
* Performance analysis
* Part-to-whole analysis
* Data segmentation
* Customer and product reporting

---

## Project Structure

```text
├── datasets/
│   ├── gold.dim_customers.csv
│   ├── gold.dim_products.csv
│   └── gold.fact_sales.csv
│
├── scripts/
│   ├── Analysis.sql
│   ├── DDL.sql
│   ├── customer_report.sql
│   ├── products_report.sql
│   └── placeholder
│
└── README.md
```

---

## Data Model

The project uses a simple **Star Schema** consisting of:

### Dimension Tables

#### `gold.dim_customers`

Contains customer-related information such as:

* Customer key
* Customer number
* First name
* Last name
* Gender
* Birthdate
* Country

#### `gold.dim_products`

Contains product-related information such as:

* Product key
* Product name
* Category
* Subcategory
* Cost
* Product-related attributes

### Fact Table

#### `gold.fact_sales`

Contains transactional sales data such as:

* Order number
* Order date
* Customer key
* Product key
* Sales amount
* Quantity
* Price

---

## Exploratory Data Analysis

The `Analysis.sql` script contains multiple analytical sections.

### 1. Database Exploration

Explores the database structure using:

* `INFORMATION_SCHEMA.TABLES`
* `INFORMATION_SCHEMA.COLUMNS`

This helps identify available tables, columns, and their structure.

---

### 2. Dimension Exploration

Analyzes unique values and categories from dimension tables.

Examples:

* Countries
* Product categories
* Product subcategories
* Products

```sql
SELECT DISTINCT country
FROM gold.dim_customers;
```

```sql
SELECT DISTINCT category, subcategory, product_name
FROM gold.dim_products
ORDER BY 1, 2, 3;
```

---

### 3. Date Exploration

Analyzes the overall sales period by identifying:

* First order date
* Last order date
* Total number of years covered

Customer age information is also explored using birth dates.

---

### 4. Measures Exploration

Calculates important business metrics such as:

* Total sales
* Total quantity sold
* Average selling price
* Total orders
* Total products
* Total customers
* Customers who have placed orders

Example:

```sql
SELECT SUM(sales_amount) AS total_sales
FROM gold.fact_sales;
```

---

## Magnitude Analysis

Analyzes business metrics across different dimensions.

Examples include:

* Customers by country
* Customers by gender
* Products by category
* Average product cost by category
* Revenue by category
* Revenue by customer
* Quantity sold by country

This helps understand the **distribution and magnitude of business activity**.

---

## Ranking Analysis

Identifies the best and worst-performing products.

### Top 5 Products

Finds the five products generating the highest revenue.

### Bottom 5 Products

Identifies the five products with the lowest sales revenue.

Example:

```sql
SELECT TOP 5
    p.product_name,
    SUM(f.sales_amount) AS total_revenue
FROM gold.fact_sales AS f
LEFT JOIN gold.dim_products AS p
    ON p.product_key = f.product_key
GROUP BY p.product_name
ORDER BY total_revenue DESC;
```

---

## Trends and Change Over Time

Analyzes sales performance over time using monthly aggregation.

Key metrics include:

* Monthly sales
* Monthly customers
* Monthly quantity sold

`DATETRUNC()` and date-based aggregation are used to analyze sales trends chronologically.

---

## Cumulative Analysis

Uses SQL **window functions** to calculate:

* Monthly sales
* Running total of sales
* Moving/average sales metrics

Example:

```sql
SUM(total_sales) OVER (
    ORDER BY order_date
) AS running_total
```

This helps understand how sales accumulate over time.

---

## Performance Analysis

Compares yearly product sales against:

* Average product sales
* Previous year's sales

SQL window functions such as:

* `AVG() OVER()`
* `LAG() OVER()`

are used to identify whether product performance is:

* Above Average
* Below Average
* Average
* Increasing
* Decreasing
* No Change

---

## Part-to-Whole Analysis

Determines how much each product category contributes to overall sales.

The analysis calculates:

* Category sales
* Overall sales
* Percentage contribution

Example:

```sql
SUM(total_sales) OVER() AS overall_sales
```

The percentage contribution is then calculated for each category.

This helps identify the categories that contribute the most to total revenue.

---

## Product Segmentation

Products are segmented based on their cost.

### Cost Segments

| Cost Range   | Segment    |
| ------------ | ---------- |
| `< 100`      | Below 100  |
| `100 - 1000` | 100-1000   |
| `> 1000`     | Above 1000 |

The analysis calculates the number of products belonging to each cost segment.

---

##  Customer Segmentation

Customers are segmented based on **spending behavior and customer lifespan**.

### VIP

* Customer lifespan ≥ 12 months
* Total spending ≥ 5,000

### Regular

* Customer lifespan ≥ 12 months
* Total spending < 5,000

### New

* Customer lifespan < 12 months

This segmentation helps identify valuable customers and understand customer behavior.

---

# Customer Report

The `customer_report.sql` script creates:

```text
gold.report_customers
```

The report provides customer-level KPIs and behavioral metrics.

### Key Metrics

* Customer name
* Age
* Age group
* Customer segment
* Last order date
* Recency
* Total orders
* Total sales
* Total quantity purchased
* Total products purchased
* Customer lifespan
* Average Order Value
* Average Monthly Spend

### Customer Segmentation

Customers are classified as:

* VIP
* Regular
* New

### Age Segmentation

Customers are grouped into age ranges such as:

* Under 20
* 20-29
* 30-39
* 40-49
* 50-59
* 60 and above

---

# Product Report

The `products_report.sql` script creates:

```text
gold.report_products
```

The report provides product-level KPIs and performance metrics.

### Key Metrics

* Product name
* Category
* Subcategory
* Cost
* Last sale date
* Recency
* Product segment
* Product lifespan
* Total orders
* Total sales
* Total quantity sold
* Total customers
* Average selling price
* Average Order Revenue
* Average Monthly Revenue

### Product Segmentation

Products are classified based on revenue:

* **High-Performer** → Sales > 50,000
* **Mid-Range** → Sales between 10,000 and 50,000
* **Low-Performer** → Sales < 10,000

---

## SQL Concepts Used

This project demonstrates practical usage of:

### SQL Fundamentals

* `SELECT`
* `WHERE`
* `GROUP BY`
* `ORDER BY`
* `DISTINCT`
* `CASE`
* `TOP`
* Aggregate functions

### Aggregate Functions

* `SUM()`
* `AVG()`
* `COUNT()`
* `MIN()`
* `MAX()`

### Joins

* `LEFT JOIN`

### Date Functions

* `YEAR()`
* `DATEDIFF()`
* `GETDATE()`
* `DATETRUNC()`

### Window Functions

* `SUM() OVER()`
* `AVG() OVER()`
* `LAG() OVER()`

### Other SQL Concepts

* Common Table Expressions (`CTE`)
* Views
* Data segmentation
* Ranking
* Running totals
* Moving averages
* Percentage calculations
* KPI calculations

---

## Key KPIs

The project calculates several important business KPIs:

| KPI                     | Description                            |
| ----------------------- | -------------------------------------- |
| Total Sales             | Overall revenue generated              |
| Total Orders            | Number of unique orders                |
| Total Quantity          | Number of items sold                   |
| Total Customers         | Number of customers                    |
| Average Selling Price   | Average price of products sold         |
| Average Order Value     | Average revenue per order              |
| Average Monthly Spend   | Average customer spending per month    |
| Recency                 | Months since last purchase             |
| Customer Lifespan       | Months between first and last purchase |
| Average Monthly Revenue | Average product revenue per month      |

---

## Tools & Technologies

* **SQL Server**
* **T-SQL**
* **SQL Server Management Studio (SSMS)**
* **Git & GitHub**
* CSV datasets

---

## Project Workflow

```text
Raw Sales & Dimension Data
          ↓
     Gold Layer
          ↓
 Exploratory Data Analysis
          ↓
 Business Metrics & KPIs
          ↓
 Customer Report
          ↓
 Product Report
          ↓
 Business Insights
```

---

## Project Objectives

The main objectives of this project are to:

1. Explore and understand the available data.
2. Analyze customer and product dimensions.
3. Calculate important business metrics.
4. Identify sales trends over time.
5. Rank products based on revenue.
6. Analyze product and customer performance.
7. Segment customers based on spending behavior.
8. Segment products based on cost and revenue.
9. Calculate business KPIs.
10. Create reusable customer and product reporting views.

---


