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