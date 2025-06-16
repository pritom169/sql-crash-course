# What is Database?
A Database is a collection of data stored in a format that can easily be accessed

## Relational Database
In relational databases you store data in tables and tabled are connected via relations

## What is SQL?
SQL is the query language to query and modify our data

# Learn by Doing
```sql
CREATE TABLE customers (
    customer_id INT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    email VARCHAR(100),
    city VARCHAR(50),
    registrtion_date DATE
);

CREATE TABLE PRODUCTS (
    product_id INT PRIMARY KEY,
    PRODUCT_NAME VARCHAR(100),
    CATEGORY VARCHAR(50),
    PRICE DECIMAL(10,2),
    STOCK_QUANTITY INT
);

CREATE TABLE ORDERS (
    ORDER_ID INT PRIMARY KEY,
    CUSTOMER_ID INT,
    ORDER_DATE DATE,
    TOTAL_AMOUNT DECIMAL(10,2),
    STATUS VARCHAR(20),
    FOREIGN KEY (CUSTOMER_ID) REFERENCES CUSTOMERS(CUSTOMER_ID)
);

CREATE TABLE ORDER_ITEMS (
    ORDER_ITEM_ID INT AUTO_INCREMENT PRIMARY KEY,
    ORDER_ID INT NOT NULL,
    PRODUCT_ID INT NOT NULL,
    QUANTITY INT NOT NULL,
    UNIT_PRICE DECIMAL(10,2) NOT NULL,
    FOREIGN KEY (ORDER_ID) REFERENCES ORDERS(ORDER_ID),
    FOREIGN KEY (PRODUCT_ID) REFERENCES PRODUCTS(PRODUCT_ID)
);
```

This SQL command is creating a database schema for an e-commerce or retail system. It defines four interconnected tables:
- CUSTOMERS table - Stores customer information including ID, name, email, city, and registration date. The customer_id serves as the primary key.
- PRODUCTS table - Contains product catalog data with product ID, name, category, price, and stock quantity. The product_id is the primary key.
- ORDERS table - Records order headers with order ID, which customer placed it, order date, total amount, and status. It references the customers 
table through a foreign key relationship.
- ORDER_ITEMS table - Stores the individual line items for each order, including which products were ordered, quantities, and unit prices. 
This table links orders to products through foreign key relationships.

Data types in this example:
- INT - Integer numbers (whole numbers like 1, 100, -50)
- VARCHAR(n) - Variable-length character strings up to n characters
- DATE - Stores dates (year-month-day format)
- DECIMAL(10,2) - Fixed-point decimal numbers (10 total digits, 2 after decimal point)
- AUTO_INCREMENT - Special attribute that automatically generates sequential numbers

Main categories of SQL data types:

Numeric Types:

* INT, SMALLINT, BIGINT (integers of different sizes)
* DECIMAL/NUMERIC (exact decimal numbers)
* FLOAT, DOUBLE (floating-point numbers)
* BIT (binary digits)

String/Text Types:

* VARCHAR (variable-length strings)
* CHAR (fixed-length strings)
* TEXT, LONGTEXT (large text blocks)
* BLOB (binary large objects for files/images)

Date/Time Types:

* DATE (dates only)
* TIME (time only)
* DATETIME/TIMESTAMP (date and time combined)
* YEAR (just the year)

Other Types:

* BOOLEAN/BOOL (true/false values)
* JSON (for storing JSON documents)
* ENUM (predefined list of values)

# BASICS OF SQL
## Writing Simple Queries (Select, From , Where)
1. Write a query to retrieve all customer names and emails from the customers table.
2. Write a query to find all products that cost more than $75.00.
3. Write a query to find all orders placed after July 1, 2023, with a total amount greater than $150.00.

```sql
SELECT FIRST_NAME, LAST_NAME, EMAIL
FROM CUSTOMERS;

SELECT PRODUCT_NAME, PRICE
FROM PRODUCTS
WHERE PRICE > 50.00;

SELECT *
FROM ORDERS
WHERE ORDER_DATE >= '2023-07-01'
AND TOTAL_AMOUNT > 100;
```

## Sorting Data (Order By)
1. Write a query to list all products sorted by price from lowest to highest.
2. Write a query to show all products sorted by price from highest to lowest.
3. Write a query to display all customers sorted first by city (alphabetically), then by last name (alphabetically).
```sql
SELECT PRODUCT_NAME, PRICE
FROM PRODUCTS
ORDER BY PRICE;

SELECT PRODUCT_NAME, PRICE
FROM PRODUCTS
ORDER BY PRICE DESC;

SELECT FIRST_NAME, LAST_NAME, CITY
FROM CUSTOMERS
ORDER BY CITY ASC, LAST_NAME DESC;
```

## Filtering with Conditions (AND, OR, NOT)
1. Write a query to find all products in the 'Electronics' category that cost less than $500.00.
2. Write a query to find all products that are either in the 'Electronics' category OR in the 'Books' category.
3. Write a query to find all customers who are NOT from 'New York'.
4. Write a query to find all orders that are either 'Shipped' or 'Delivered' AND have a total amount of at least $50.00.

```sql
SELECT * FROM PRODUCTS
WHERE CATEGORY = 'ELECTRONICS'
	AND PRICE < 500;

SELECT * FROM PRODUCTS
WHERE CATEGORY IN ('ELECTRONICS', 'SPORTS');

SELECT * FROM CUSTOMERS
WHERE CITY != 'NEW YORK';

SELECT * FROM ORDERS
WHERE STATUS IN ('SHIPPED', 'COMPLETED')
AND TOTAL_AMOUNT >= 50;
```

## Using Aliases (AS for columns and tables)
1. Write a query to display customer names with column aliases 'FirstName' and 'LastName', and email as 'CustomerEmail'.
2. Write a query using table aliases to join customers and orders tables, showing customer names and order dates.
3. Write a query that shows product names and their prices, plus a calculated column showing a 10% discounted price (aliased as 'discounted_price').

```sql
SELECT FIRST_NAME AS FirstName,
	LAST_NAME AS LastName,
	EMAIL AS CustomerEmail
FROM CUSTOMERS;

SELECT C.FIRST_NAME, C.LAST_NAME, O.ORDER_DATE
FROM CUSTOMERS AS C
INNER JOIN ORDERS AS O ON C.CUSTOMER_ID = O.CUSTOMER_ID;

SELECT P.PRODUCT_NAME, P.PRICE, (P.PRICE * 0.9) AS DISCOUNTED_PRICE
FROM PRODUCTS AS P;
```

> INNER JOIN is a SQL operation that combines rows from two or more tables based on a 
related column between them. It only returns records that have matching values in 
both tables.

# Working with Data
## Aggregating Data (COUNT, SUM, AVG, MIN, MAX)
1. Write a query to count the total number of customers in the database.
2. Write a query to calculate the total revenue from all orders.
3. Write a query to find the average price of all products.
4. Write a query to find both the lowest and highest priced products.
5. Write a query that shows the total number of products, average price, and total 
inventory (sum of stock_quantity) in one result.

```sql
SELECT COUNT(*) AS TOTAL_CUSTOMERS
FROM CUSTOMERS;

SELECT SUM(TOTAL_AMOUNT) AS TotalAmount
FROM ORDERS;

SELECT AVG(PRICE) AS AVERAGEPRICE
FROM PRODUCTS;

SELECT MIN(PRICE) AS LOWEST_PRICED_PRODUCT,
	MAX(PRICE) AS HIGHEST_PRICED_PRODUCT
FROM PRODUCTS;

SELECT COUNT(*) AS TOTAL_AMOUNT_OF_PRODUCTS,
	AVG(PRICE) AS AVG_PRICE,
    SUM(STOCK_QUANTITY) AS TOTAL_INVENTORY
FROM PRODUCTS;
```

## Grouping Data
1. Write a query to count how many customers are in each city.
2. Write a query to calculate the total revenue for each product category (using order_items and products tables).
3. Write a query to count how many orders exist for each order status.

```sql
SELECT CITY, COUNT(*) as CUSTOMER_COUNT FROM CUSTOMERS
GROUP BY CITY;

SELECT P.CATEGORY,
	COUNT(DISTINCT OI.ORDER_ID) AS ORDER_COUNT,
    SUM(OI.QUANTITY * OI.UNIT_PRICE) AS CATEGORY_REVENUE
FROM ORDER_ITEMS AS OI
JOIN PRODUCTS AS P ON P.PRODUCT_ID = OI.PRODUCT_ID
GROUP BY P.CATEGORY;

SELECT COUNT(ORDER_ID) AS TOTAL_ORDERS,
		STATUS AS ORDER_STATUS
FROM ORDERS
GROUP BY STATUS;
```

> A JOIN in SQL is an operation that combines rows from two or more tables based on a related 
column between them. When you use JOIN without any prefix (just the keyword JOIN), it's 
technically an INNER JOIN. It returns only rows where there's a match in both tables.
Rows that don't have matches in both tables are excluded from the results.

## Filtering Aggregated Results (HAVING)
1. Write a query to find all cities that have more than 5 customers.
2. Write a query to find product categories where the average price is above $100.00.
3. Write a query to find customers who have spent more than $1000.00 in total across all their orders.

```sql
SELECT CITY, COUNT(*) AS TOTAL_COUNT
FROM CUSTOMERS
GROUP BY CITY
HAVING COUNT(*) >= 1;

SELECT CATEGORY, AVG(PRICE) AS AVG_PRICE
FROM PRODUCTS
GROUP BY CATEGORY
HAVING AVG(PRICE) >= 20;

SELECT C.FIRST_NAME, C.LAST_NAME, SUM(O.TOTAL_AMOUNT) AS TOTAL_PURCHASE
FROM CUSTOMERS AS C
JOIN ORDERS AS O ON O.CUSTOMER_ID = C.CUSTOMER_ID
GROUP BY C.FIRST_NAME, C.LAST_NAME
HAVING SUM(O.TOTAL_AMOUNT) >= 50;
```

> The HAVING clause in SQL is used to filter the results of a GROUP BY operation. Unlike the 
WHERE clause (which filters individual rows before grouping), HAVING filters aggregated results 
after grouping. 

## Dealing with NULL Values (IS NULL, COALESCE)
1. Write a query to find all products that have NULL stock_quantity.
2. Write a query to find all customers who don't have an email address (NULL email).
3. Write a query to display all products with their stock quantity, showing 0 instead of NULL for products 
with no stock.
4. Write a query that shows customer_id and contact information, using email first, then phone if email is 
NULL, or 'No contact info' if both are NULL.

```sql
SELECT * 
FROM PRODUCTS
WHERE STOCK_QUANTITY IS NULL;

SELECT *
FROM CUSTOMERS
WHERE EMAIL IS NULL;

SELECT PRODUCT_NAME, coalesce(STOCK_QUANTITY, 0) AS CURRENT_STOCK
FROM PRODUCTS;

SELECT
	CUSTOMER_ID,
    COALESCE (EMAIL, FIRST_NAME, LAST_NAME, 'NO CONTACT INFO') AS CONTACT_INFO
FROM CUSTOMERS;
```

> What COALESCE does, COALESCE(value1, value2, value3, ...) returns the first non-NULL value from left to right
If stock_quantity is NULL → returns 0 , If stock_quantity has any actual value (including 0) → returns that value

- Product with stock_quantity = 5 → shows 5
- Product with stock_quantity = 0 → shows 0
- Product with stock_quantity = NULL → shows 0

## Joining Tables
### Inner Join
- Write a query using INNER JOIN to show customer names along with their order information (order_id, 
order_date, total_amount).

```sql
SELECT C.FIRST_NAME, C.LAST_NAME, O.ORDER_ID, O.ORDER_DATE, O.TOTAL_AMOUNT
FROM CUSTOMERS AS C
INNER JOIN ORDERS AS O ON O.CUSTOMER_ID = C.CUSTOMER_ID;
```
> Returns only rows that have matching values in both tables.

> When we write join, the command "INNER JOIN" has to come after "From"

### Left Join
- Write a query using LEFT JOIN to show all customers and their order count, including customers who haven't 
placed any orders.

```sql
SELECT C.FIRST_NAME, C.LAST_NAME, COUNT(distinct O.CUSTOMER_ID) AS ORDER_COUNT
FROM CUSTOMERS AS C
LEFT JOIN ORDERS AS O ON O.CUSTOMER_ID = C.CUSTOMER_ID
GROUP BY C.CUSTOMER_ID;
```
> Left join returns all rows from the left table, plus matching rows from the right table. NULL for non-matches.

> One other note, all aggegated commands should be followed by group by.

### Right Join
- Write a query using RIGHT JOIN to show all orders with customer information, including orders that might 
not have associated customer records.

```sql
SELECT C.FIRST_NAME, C.LAST_NAME, O.ORDER_ID
FROM CUSTOMERS AS C
RIGHT JOIN ORDERS AS O ON C.CUSTOMER_ID = O.CUSTOMER_ID;
```
> Right join returns all rows from the right table, plus matching rows from the left table. 
NULL for non-matches.

### Cross Join
- Write a query using CROSS JOIN to create a result set showing every product name paired with every 
distinct category in the products table.
```sql
SELECT DISTINCT
    p1.product_name,
    p2.category
FROM products p1
CROSS JOIN PRODUCTS AS p2;
```

> Returns the cartesian product—every row in the first table combined with every row
in the second table and vise versa. 

### Combining Data from Multiple Tables Using Foreign Keys
- Write a query that joins all four tables to show complete order details including: customer name 
(concatenated), order date, product name, quantity, unit price, and calculated line total.
```sql
SELECT
	CONCAT(C.FIRST_NAME , ' ' , C.LAST_NAME) AS CUSTOMER_NAME,
    O.ORDER_DATE,
    P.PRODUCT_NAME,
    OI.QUANTITY,
    OI.UNIT_PRICE,
    (OI.QUANTITY * OI.UNIT_PRICE) AS LINE_TOTAL
FROM CUSTOMERS AS C
JOIN ORDERS AS O ON O.CUSTOMER_ID = C.CUSTOMER_ID
JOIN ORDER_ITEMS AS OI ON OI.ORDER_ID = O.ORDER_ID
JOIN PRODUCTS AS P ON P.PRODUCT_ID = OI.PRODUCT_ID;
```

## Subqueries and Common Table Expressions (CTEs)
### Subqueries in SELECT Clause
- Write a query that shows each customer's first name, last name, and their average order value (calculated using a subquery in the SELECT clause).
```sql
select c.first_name, c.last_name,
	(select avg(total_amount)
    from orders o
    where o.customer_id = c.customer_id) as avg_order_value
from customers as c;
```

### Subqueries in FROM Clause
- Write a query that uses a subquery in the FROM clause to first calculate average price by category, then filters to show only categories with average price above $50.
```sql
select CATEGORY, AVG_PRICE
	FROM (SELECT CATEGORY, AVG(PRICE) AS AVG_PRICE
	FROM PRODUCTS
	GROUP BY CATEGORY
	) AS CATEGORY_STATS
WHERE AVG_PRICE > 10;
```

### Subqueries in WHERE Clause
- Write a query to find all customers who have placed at least one order with a total amount above the overall average order amount.

```sql
SELECT DISTINCT CONCAT(C.FIRST_NAME, ' ', C.LAST_NAME) AS FULL_NAME
FROM CUSTOMERS AS C
WHERE C.CUSTOMER_ID IN (
	SELECT CUSTOMER_ID
    FROM ORDERS
    WHERE TOTAL_AMOUNT > (SELECT AVG(TOTAL_AMOUNT) FROM ORDERS)
);
```

### Common Table Expressions (WITH)
1. Write a query using CTEs to:
   * First calculate each customer's order count and lifetime value
   * Then assign customer segments based on lifetime value (VIP >= $5000, Gold >= $1000, Silver >= $100, Bronze < $100)
   * Finally display the results ordered by lifetime value descending

```sql
WITH CUSTOMER_LIFETIME_ORDERS AS (
	SELECT C.CUSTOMER_ID, CONCAT(C.FIRST_NAME, ' ', C.LAST_NAME) AS FULL_NAME,
			COUNT(O.ORDER_ID) AS TOTAL_ORDER_COUNT, SUM(O.TOTAL_AMOUNT) AS LIFETIME_ORDER_VALUE
	FROM CUSTOMERS AS C
    JOIN ORDERS AS O ON O.CUSTOMER_ID = C.CUSTOMER_ID
    GROUP BY C.CUSTOMER_ID, C.FIRST_NAME, C.LAST_NAME
),
CUSTOMER_LIFETIME_VALUE AS (
	SELECT *,
		CASE
			WHEN LIFETIME_ORDER_VALUE >= 5000 THEN 'VIP'
            WHEN LIFETIME_ORDER_VALUE >= 1000 THEN 'GOLD'
            WHEN LIFETIME_ORDER_VALUE >= 100 THEN 'SILVER'
            ELSE 'BRONZE'
		END AS ORDER_CLASS
	FROM CUSTOMER_LIFETIME_ORDERS
)

SELECT * FROM CUSTOMER_LIFETIME_VALUE
ORDER BY LIFETIME_ORDER_VALUE DESC;
```

## Window Functions
### Ranking and Row Numbering (ROW_NUMBER, RANK, DENSE_RANK)
1. Write a query using ROW_NUMBER() to assign a unique sequential number to products 
ordered by price (highest to lowest).
2. Write a query using RANK() to rank products by price (highest to lowest), allowing for ties.
3. Write a query using DENSE_RANK() to rank products by price (highest to lowest) without 
gaps in ranking numbers.
```sql
-- ROW_NUMBER
SELECT
	PRODUCT_NAME,
    CATEGORY,
    PRICE,
    ROW_NUMBER() OVER (ORDER BY PRICE DESC) AS PRICE_RANK
FROM PRODUCTS;

-- RANK
SELECT 
	PRODUCT_ID,
    PRODUCT_NAME,
    CATEGORY,
    RANK() OVER (ORDER BY PRICE DESC) AS PRICE_RANK
FROM PRODUCTS;

-- DENSE RANK
SELECT
	PRODUCT_ID,
    PRODUCT_NAME,
    PRICE,
    DENSE_RANK() OVER (ORDER BY PRICE DESC) AS PRICE_RANK
FROM PRODUCTS;
```
### Calculating Running Totals and Moving Averages
1. Write a query to calculate a running total of order amounts, ordered by order date.
2. Write a query to calculate a 3-day moving average of order amounts (current day plus 2 
preceding days).

```SQL
SELECT
	ORDER_DATE,
    TOTAL_AMOUNT,
    SUM(TOTAL_AMOUNT) OVER (ORDER BY ORDER_DATE) AS RUNNING_TOTAL
FROM ORDERS;

SELECT
	ORDER_DATE,
    TOTAL_AMOUNT,
    AVG(TOTAL_AMOUNT) OVER (
		ORDER BY ORDER_DATE
        ROWS BETWEEN 2 PRECEDING AND CURRENT ROW
    ) AS MOVING_AVG_3DAY
FROM ORDERS;
```

### Partitioning Data (PARTITION BY)
1. Write a query to rank products within each category by price (highest to lowest).
2. Write a query showing each customer's orders with a running total of their purchases, 
resetting for each customer.

```sql
--Rank by each category
SELECT
    ORDER_DATE,
    UNIT_PRICE,
    RANK() OVER (PARTITION BY CATEGORY ORDER BY ORDER_DATE) AS CATEGORY_RANK
FROM ORDER_ITEMS;

--Rank by partition
SELECT
	C.CUSTOMER_ID,
	CONCAT(C.FIRST_NAME, ' ', C.LAST_NAME) AS CUSTOMER_NAME,
    RANK() OVER (PARTITION BY C.CUSTOMER_ID ORDER BY O.ORDER_DATE) AS CUSTOMER_RUNNING_TOTAL
FROM ORDERS AS O
JOIN CUSTOMERS C ON O.CUSTOMER_ID = C.CUSTOMER_ID;
```