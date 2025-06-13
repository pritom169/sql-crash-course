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

