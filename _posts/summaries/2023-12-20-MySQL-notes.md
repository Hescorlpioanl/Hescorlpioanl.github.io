---
title: "MySQL Cheat sheet"
classes: wide
header:
  teaser: /assets/images/summaries/sql.png
ribbon: ForestGreen
description: "My notes during study Complete SQL Mastery course"
categories:
  - Summaries
tags:
  - Database
toc: true
toc_sticky: true
---

<br>
<div align="center">

بسم الله الرحمن الرحيم <br> احييكم بتحية الإسلام السلام عليكم ورحمة الله تعالى وبركاته

</div>
<br>
# Introduction

Hello world, Hossam is here. Welcome everyone to my humble blog. In this post, I will write down all the MYSQL syntax that I encountered while studying MYSQL or through my solutions on the HackerRank website and LeetCode. I simply consider this publication to be my notes that make it easier for me to quickly review SQL syntax, which helps me prepare for job interviews, in addition to being a good opportunity to share information with others.

I wrote the Syntax here through my application of the [Mosh Hamidani course](https://codewithmosh.com/p/complete-sql-mastery) and some research and learning away from the course. You will also find all my solutions to SQL problems through the GitHub repository [hackerrank-sql-solutions](https://github.com/0xGhazy/hackerrank-sql-solutions)

**[Note]**: As long as you see these lines, it means that the post is under preparation and not yet completed. If there are any mistakes or misunderstandings of any information from my side, please inform me. Thank you in advance for your efforts.

---

# Retrieving Data From a Single Table

## Select statement

Use to retrieve columns from tables. each selection must end with `;`

```sql
-- Return all user's records from user table
SELECT * FROM USERS;

-- return 1, 2
SELECT 1, 2;

-- return first_name, last_name, points columns
SELECT first_name, last_name, points FROM customers;

-- return customer name, points, and points * 5 as 5-times points
SELECT name, points, points * 5 AS '5-times points' FROM customers;

-- return the unique names from the customer's table.
SELECT DISTINCT name FROM customers;
```

## Where clause

We can select some fields based on the condition after the `WHERE`. `<>` and `!=` for the not equal and `=` for equality.

```sql
SELECT *
FROM customers
WHERE points > 3000;
```

## Order by clause

Used to order the results by specified column name, it's valid also for allies columns in MySQL, and for unselected columns too.

```sql
-- get all orders by total_price column (allies) = quantity * unit_price
SELECT * FROM store.order_items WHERE order_id = 2 ORDER BY quantity * unit_price desc;
```

you can also order the result by the column position like this:

```sql
SELECT first_name, last_name, state FROM customers
WHERE state = 'FL'
ORDER BY 1, 2; --1: firstname, 2: last_name
```

## Limit clause

used to limit the result by the number of records you wanna return.
limit clause must come at the end of your query.

```sql
-- to get the first 3 records
SELECT * FROM store.order_items
WHERE order_id = 2
ORDER BY quantity * unit_price desc
LIMIT 3;
```

you can use it as a pagination clause using the following syntax.

```sql
-- Here we will skip the first 6 records
-- then we will return the 3 records after them
SELECT * FROM store.order_items
WHERE order_id = 2
ORDER BY quantity * unit_price desc
LIMIT 6, 3; --skip the first 6 records and then get the next 3 records only
```

## Operators

### AND

TRUE if all the conditions separated by AND are TRUE.

```sql
SELECT name FROM STATION WHERE LAT_N > 38.7880 AND LAT_N < 137.2345;
```

### OR

TRUE if any of the conditions separated by OR is TRUE.

```sql
SELECT name FROM STATION WHERE LAT_N > 38.7880 OR LAT_N < 137.2345;
```

### NOT

Reverses the value of any other Boolean operator.

```sql
SELECT name FROM STATION WHERE NOT (LAT_N > 38.7880 AND LAT_N < 137.2345);
```

### IN

TRUE if the operand is equal to one of a list of expressions.
use in logical operator to compare attributes with a list of values

```sql
SELECT * FROM Customers WHERE state in ('VA', 'FL', 'MX');
```

it can combined with the `NOT` operator to get all customers that are not from the specified states.

```sql
SELECT * FROM Customers WHERE state NOT IN ('VA', 'FL', 'MX');
```

### BETWEEN

TRUE if the operand lies within the range of comparisons.
we want to get all customers born after `1990/01/01` and before `2000/01/01`. we can use arithmetic operators such as `<=` or `<=` as follows.

```sql
SELECT * FROM Customers WHERE birth_date > '1990-01-01' AND birth_date < '2000-01-01';
```

or use the `between` operator directly 😃

```sql
SELECT * FROM Customers WHERE birth_date BETWEEN '1990-01-01' AND '2000-01-01';
```

### LIKE

TRUE if the operand matches a pattern, especially with a wildcard. we use `_` to specify a character and `%` for any number of characters. Note that `%a` get all end with `a` and `a%` get all start with

```sql
-- Get a customer with phone ends at 9
SELECT * FROM store.customers WHERE phone LIKE '%9';


-- Get a customer with an address containing Trail or Avenue
SELECT * FROM store.customers WHERE
    address LIKE '%Trail%'
    OR
    address LIKE '%Avenue%';
```

### REGEXP

to get all first names that contain `Elka` or `Ambur` using the pipe `|` that perform OR operation.

```sql
SELECT * FROM store.customers WHERE first_name REGEXP "ELKA|AMBUR";
```

`$` indicates the end of the result to get all last names that end with `EY`, and `ON`

```sql
SELECT * FROM store.customers WHERE last_name REGEXP "EY|ON$";
```

last names that start with specified regex. the `^` wildcard indicates the start of the string.

```sql
SELECT * FROM store.customers WHERE last_name REGEXP "^MY|se";
```

get the last name that contains `B` followed by `R` or `U`.
here we didn't use the pipe `|` because we use the `[]` which allows us to specify the characters we need.

```sql
SELECT * FROM store.customers WHERE last_name REGEXP "B[ru]";
```

if you wanna search for a characters/numbers range use `[start-end]`

```sql
-- get all last names that contain characters in the range [a-z]
-- this will return all records :)
SELECT * FROM store.customers WHERE last_name REGEXP "[a-z]";
```

### IS NULL

to get records attributes with null values.

```sql
-- get all customers that have no phone number.
SELECT * FROM store.customers WHERE phone IS NULL;
```

---

# Retrieving Data From Multiple Tables

## Inner Join

returns records that have matching values in both tables

```sql
SELECT order_id, first_name, last_name
FROM orders
JOIN customers
ON orders.customer_id = customers.customer_id;
```

you may face an error when you try to select `customer_id` because it exists in the two tables so in this case you need to use allies such as:

```sql
SELECT order_id, o.customer_id, first_name, last_name
FROM orders o
JOIN customers c
ON o.customer_id = c.customer_id;
```

## Joining across databases

```sql
SELECT *
FROM order_items oi
	 -- We add here the database name.
JOIN sql_inventory.products p
ON oi.product_id = p.product_id;
```

## Self join

A self-join is a regular join, but the table is joined with itself.

```sql
SELECT
	e.employee_id,
	e.first_name,
	m.first_name AS 'manager name'
FROM employees e -- for employees
JOIN employees m -- for managers
	ON e.reports_to = m.employee_id;
```

## Multiple join

join multiple tables with each other

```sql
SELECT
	e.employee_id,
	e.first_name,
	m.first_name AS 'manager name'
FROM orders o
JOIN customers c
	ON o.customer_id = c.customer_id
JOIN order_status os
	ON o.status = os.order_status_id;
```

We can combine some join conditions like the following:

```sql
SELECT *
FROM order_items oi
JOIN order_item_notes oin
ON oi.order_id = oin.order_id
AND oi.product_id = oin.product_id
```

## Implicit Join

Making join without writing `join`, if you forget to write the condition we will get a cross join.

```sql
SELECT *
FROM orders o, customers c
WHERE o.customer_id = c.customer_id;
```

## Outer Join

### Left join

Returns all records from the left table, and the matched records from the right table

```sql
SELECT *
FROM orders o
LEFT OUTER JOIN customers c
	ON c.customer_id = o.customer_id;
```

### Right join

Returns all records from the right table, and the matched records from the left table

```sql
SELECT *
FROM orders o
RIGHT OUTER JOIN customers c
	ON c.customer_id = o.customer_id;
```

the `outer` keyword is optional so you can skip it if you wanna. you can also use the outer join to join multiple tables and you can combine inner and outer joins like this:

```sql
SELECT *
FROM customers c
RIGHT JOIN orders o -- outer join
	ON o.customer_id = c.customer_id
JOIN shippers sh
	ON o.shipper_id = sh.shipper_id
ORDER BY c.customer_id;
```

to convert from lest join to outer join or the opposite you can just change the order of tables customers <==> orders, and it'll be done 😃.

## Self outer join

```sql
SELECT
	e.employee_id,
	e.first_name,
	m.first_name AS 'manager name'
FROM employees e -- for employees
LEFT JOIN employees m -- for managers
	ON e.reports_to = m.employee_id;
```

## Using clause

is useful when both the tables share a column of the same name on which they join.

```sql
SELECT *
FROM customers c
RIGHT JOIN orders o
	USING(customer_id)
JOIN shippers sh
	USING(shipper_id)
ORDER BY c.customer_id;
```

if you have multiple conditions use it as _USING(col1, col2)_.

## Natural joins

let the database engine determine the column name to join automatically, it's may lead to unexpected results because we have no control over it.

```sql
SELECT *
FROM customers c
JOIN shippers sh;
```

## Cross join

used to map every record from Table 1 with every record of Table 2, it is used in the real world in mapping sizes with colors in the cloths industry.

```sql
SELECT *
FROM customers c
CROSS JOIN products p;

-- Also we can use the Implicit from id it
SELECT *
FROM customers c, products p;
```

## Unions

The `UNION` operator is used to combine the result set of two or more `SELECT` statements.

```sql
SELECT customer_id, first_name, points, "Bronze" as "type"
FROM customers
where points <= 2000
UNION
SELECT customer_id, first_name, points, "Silver" as "type"
FROM customers
where points between 2000 AND  3000
UNION
SELECT customer_id, first_name, points, "Gold" as "type"
FROM customers
where points > 3000
ORDER BY first_name;
```

## Column Attributes

|    column     | description                                                        |
| :-----------: | :----------------------------------------------------------------- |
|   Datatype    | determine the column data type                                     |
|      PK       | Stands for `primary key`                                           |
|      NN       | Stands for `Not Null` used for required fields                     |
|      UQ       | Stands for `Unique`                                                |
|      AI       | Stands for `Auto Increment` usually used with the primary key      |
| Default Value | Specify the default value of the field if you didn't pass anything |

---

# Inserting, Updating, and Deleting Data

## Create Record

```sql
-- insert one record
INSERT INTO customers (
	first_name,
	last_name,
	dob,
	phone
	)
VALUES ("Hossam", "Hamdy", "1990-01-05", NULL);



-- insert many records
INSERT INTO customers (
	first_name,
	last_name,
	dob,
	phone
	)
VALUES ("Hossam", "Hamdy", "1990-01-05", NULL),
	   ("Hassan", "Aly", "1990-01-05", NULL),
	   ("Root", "Toor", "1990-01-05", NULL);
```

```sql
-- Create a new table of an old table
CREATE TABLE new_orders AS SELECT * from orders;
```

`Note` if you use this technique make sure that the copied attribute is copied using values only, if an attribute is `PK` or `AI` it'll be a normal day.

## Update

```sql
-- update one record
UPDATE customers
SET first_name = "root", last_name = "toor", dob = Null, phone = Null
WHERE id = 1;

-- to update multi-record is the same syntax
UPDATE orders
SET price = 1000
WHERE customer_id = 1;
-- If we have more than one record for customer 1 it'll be updated.
```

You can also use the `IN` operator

```sql
UPDATE orders
SET price = 1000
WHERE customer_id IN (1, 2, 3);
```

## Sub queries

```sql
UPDATE orders
SET price = 1000

-- If we don't know the customer ID but we know the name then we can use the Sub queries
WHERE customer_id =
			(SELECT customer_id
			FROM customers
			WHERE name = "hossam")
			;
```

## Delete

```sql
-- This will delete all records in the customer's table.
DELETE FROM customers;

-- This will delete all records in the customer table.
DELETE FROM customers
WHERE customer_id = 5; -- also you can use the Sub queries here.
```

---

# Summarizing Data

here you'll learn how to write queries that can summarize data and answer some business questions.

## Aggregate Functions

### MAX()

applied on Numbers, String, and date

```sql
SELECT MAX(invoice_total) as "Highest" FROM invoices;
```

### MIN()

```sql
SELECT MIN(invoice_total) as "Lowest" FROM invoices;
```

### AVG()

```sql
SELECT AVG(invoice_total) as "Average" FROM invoices;
```

### SUM()

```sql
SELECT SUM(invoice_total) as "Total" FROM invoices;
```

### COUNT()

only perform on `Non-Null` values.

```sql
SELECT SUM(invoice_total) as "Total" FROM invoices;

-- COUNT(*) to get the total of records
```

## Group By clause

The `GROUP BY` statement groups rows that have the same values into summary rows, like "find the number of customers in each country".

```sql
SELECT COUNT(CustomerID), Country
FROM Customers
GROUP BY Country;
```

## Having

The `HAVING` clause is used in conjunction with the `GROUP BY` clause to filter the results of a query based on the aggregated values. It allows you to specify a condition that filters the grouped rows returned by a `GROUP BY` query.

```sql
SELECT column1, COUNT(*)
FROM your_table
GROUP BY column1
HAVING COUNT(*) > 1;
```

## Rollup

used to get the accumulative value of the aggregating functions, this is only available for MySQL.

```sql
SELECT
	state,
	city,
	SUM(invoice_total) AS total_sales
FROM invoices
GROUP BY state, city WITH ROLLUP
```

## ALL keyword

Used to return all values that meet the condition, the normal select will return a single value, but with this approach, we will get a list of results that met `_condition_`

```sql
SELECT _column_name(s)_
FROM _table_name_
WHERE _column_name operator_ ALL (
			SELECT _column_name_  
			FROM _table_name_
			WHERE _condition_
);
```

## ANY keyword

`ANY` means that the condition will be true if the operation is true for any of the values in the range. It's equal to using the `IN` operator.

```sql
SELECT *
FROM clients
WHERE client_id ANY (
	SELECT client_id
	FROM invoices
	GROUP BY client_id
	HAVING COUNT(*) >= 2
)
```

## Correlated Sub queries

```sql
SELECT *
FROM invoicing.invoices inv
WHERE inv.invoice_total > (
    SELECT AVG(invoice_total)
    FROM invoicing.invoices
    WHERE inv.client_id = client_id
)
```

## EXISTS operator

The `EXISTS` operator is used to test for the existence of any record in a sub-query.
this operator is better than using the `IN` operator because the `IN` operator will return a list and then examine the condition, but `EXISTS` will return true to the condition and then return the row that matches the condition.

```sql
SELECT SupplierName
FROM Suppliers
WHERE EXISTS (
	SELECT ProductName
	FROM Products
	WHERE Products.SupplierID = Suppliers.supplierID
		   AND Price < 20);
```

## Sub queries in select clause

```sql
SELECT
	client_id,
    name,
    (
		SELECT SUM(invoice_total)
		FROM invoices i
		WHERE client_id = c.client_id
	) AS total_sales,
	(
		SELECT AVG(invoice_total)
		FROM invoices
	) AS average,
	(SELECT total_sales - average) AS difference
FROM clients c;
```

## Sub queries in from clause

With this, we can deal with the result table as a database table so we can perform select, aggregation functions, joins, and anything else.

```sql
SELECT *
FROM (
	SELECT
		client_id,
	    name,
	    (
			SELECT SUM(invoice_total)
			FROM invoices i
			WHERE client_id = c.client_id
		) AS total_sales,
		(
			SELECT AVG(invoice_total)
			FROM invoices
		) AS average,
		(SELECT total_sales - average) AS difference
	FROM clients c;
) AS sales_summary
```

---

# Essential MySQL Functions

## Numeric functions

```sql
-- Round the number
SELECT ROUND(5.123456789, 2) -- 5.12
SELECT ROUND(5.123456789, 5) -- 5.12345

-- returns the smallest than or equal to the passed number
SELECT CEILING(5.7) -- 6

SELECT FLOOR(5.2) -- 5

-- Remove the rest of the number
SELECT TRUNCATE(5.12345, 2) -- 5.12 and

SELECT ABS(-5.2) -- 5.2 returns the positive value

SELECT RAND() -- returns a random function between 0 and 1.
```

- [SQL numeric functions](https://www.tutorialspoint.com/sql/sql-numeric-functions.htm)

## String functions

```sql
-- convert to lower and upper case
SELECT UPPER("hello") -- return HELLO
SELECT LOWER("HELLO") -- return hello


-- Remove any whitespace before or after the word
SELECT LTRIM("  HELLO") -- "HELLO"
SELECT RTRIM("HELLO  ") -- "HELLO"
SELECT TRIN (" HELLO  ") -- "HELLO"


-- get chars from the left side
SELECT LEFT("HELLO WORLD", 4) -- HELL
-- get chars from the right side
SELECT RIGHT("ROOT", 2) -- OT


-- Get string from the start to the end position
-- The end position is optional
SELECT SUBSTRING("HOW ARE YOU", 8, 11); -- YOU


-- search about char in string
-- If the search string is not in the string it'll return 0
SELECT LOCATE("D", "HELLO WORD")


-- HELLO WORLD
SELECT REPLACE("TELLO WORLD", "TELLO", "HELLO")


-- HI, HOSSAM
SELECT CONCAT("HI, ", "HOSSAM")
```

- [SQL string functions](https://www.tutorialspoint.com/sql/sql-string-functions.htm)

## Date functions

```sql
SELECT NOW(), CURDATE(), CURTIME()
```

| NOW()               | CURDATE()  | CURTIME() |
| ------------------- | ---------- | --------- |
| 2023-12-18 10:40:15 | 2023-12-18 | 10:40:15  |

```sql
SELECT YEAR(NOW())-- 2023
SELECT MONTH(NOW()) -- 12
SELECT DAY(NOW()) -- 18

-- You can use HOUR, MINUTE, and SECOND functions too, all of them return integers.

-- Also you can use DAYNAME
SELECT DAYNAME(NOW())  -- Monday
SELECT MONTHNAME(NOW()) -- December


SELECT EXTRACT (YEAR FROM NOW()) -- 2023



-- <<perform calculations on date and time>> --
-- add
SELECT DATE_ADD(NOW(), INTERVAL 1 YEAR) -- 2024-12-18 10:55:53
SELECT DATE_ADD(NOW(), INTERVAL -1 YEAR)--2022-12-18 10:56:21

-- def
SELECT DATEDIFF('2019-01-05', '2019-01-01') -- 4
SELECT TIME_TO_SEC('09:00') - TIME_TO_SEC('09:02') -- 120 s
```

- [SQL date functions](https://www.tutorialspoint.com/sql/sql-date-functions.htm)
- [SQL date formatting](https://www.w3schools.com/sql/sql_dates.asp)

## IFNULL and COALESCE

```sql
-- If the test value is null it'll be replaced with NONE
IFNULL(test_vaule, "NONE")

-- Return the first non-null value in a list
SELECT COALESCE(NULL, NULL, NULL, 'W3Schools.com', NULL, 'Example.com');
```

## IF function

```sql
SELECT
	product_id,
    name,
    COUNT(*) AS orders,
				--    true case, false case
    IF(COUNT(*) > 1, 'Many times', 'Once') AS frequency
FROM products
JOIN order_items USING(product_id)
GROUP BY product_id, name
```

## CASE operator

```sql
SELECT
	CONCAT(first_name, ' ', last_name),
    points,
    CASE
		WHEN points >  3000 THEN "Gold"
        WHEN points >= 2000 THEN "Silver"
        ELSE "Bronze"
	END AS category
FROM customers
```

---

# Views

view is a virtual table based on the result set of an SQL statement.

## Create View

```sql
CREATE VIEW view_name AS
-- your script starts here
SELECT
	name,
	phone,
	email
FROM users
```

after creating a view we can deal with it as a table in the database. Views don't store data it's a virtual table, it's just a `view` of the data.

## Altering views

```sql
-- replace view if exist
CREATE OR REPLACE VIEW sales_by_client AS
SELECT
	c.client_id,
	s.name,
	SUM(invoice_total) AS total_sales
FROM clients c
JOIN invoices i USING(client_id)
GROUP BY client_id, name
```

## Update views

if we don't have `DISTINCT`, `Aggregate function`, `GROUP BY`, `HAVING`, and `UNION` then we can update the table using this view.

```sql
UPDATE invoices_with_balance
SET due_date = DATE_ADD(due_date, INTERVAL 2 DAY)
WHERE invoice_id = 1
```

## WITH OPTION CHECK

The `WITH CHECK OPTION` clause can be given for an updatable view to prevent inserts to rows for which the `WHERE` clause in the *`select_statement`* is not true. It also prevents updates to rows for which the `WHERE` clause is true but the update would cause it to be not true (in other words, it prevents visible rows from being updated to nonvisible rows).

```sql
CREATE TABLE t1 (a INT);
CREATE VIEW v1 AS SELECT * FROM t1 WHERE a < 2
WITH CHECK OPTION;
CREATE VIEW v2 AS SELECT * FROM v1 WHERE a > 0
WITH LOCAL CHECK OPTION;
CREATE VIEW v3 AS SELECT * FROM v1 WHERE a > 0
WITH CASCADED CHECK OPTION;
```

## Views Benefits

- Simplify queries
- Adding an abstraction layer that reduces the impact of changes

```
here we create a table once and apply all changes to a view to map these changes.
```

- restrict access to data.

---

# Stored Procedures (SP)

A `stored procedure` is a prepared SQL code that you can save, so the code can be reused over and over again.

## Create SP

```sql
DELIMITER $$  -- use any sign you want
CREATE PROCEDURE get_clients()
BEGIN
	SELECt * FROM clients;
END $$
DELIMITER ;
```

to call the SP we use the `CALL` keyword

```sql
CALL get_clients()
```

## DROP the stored procedure

```sql
DROP PROCEDURE IF EXISTS get_payments;
```

## Passing parameters

```sql
DELIMITER $$
CREATE PROCEDURE get_clients_by_state(
	state CHAR(2)
)
BEGIN
	SELECT * FROM clients c
	WHERE c.state = statue;
END $$
DELIMITER ;
```

set a parameter default value

```sql
DELIMITER $$
CREATE PROCEDURE get_clients_by_state(
	state CHAR(2)
)
BEGIN
	IF state IS NULL THEN
		SET state = 'CA'; -- set the value you want
	END IF;

	SELECT * FROM clients c
	WHERE c.state = statue;
END $$
DELIMITER ;
```

## Validating SP parameters

```sql
DELIMITER $$
CREATE PROCEDURE make_payment(
	invoice_id INT,
	payment_amount DECIMAL(9, 2),
	payment_date DATE
)
BEGIN
	IF payment_amount <= 0 THEN
	-- '22003' from the link below
		SIGNAL SQLSTATE '22003'
		SET MESSAGE_TEXT = "Invalid payment amount";
	END IF;

	-- your update code goes here

END $$
DELIMITER ;
```

## Variables

```sql
-- user/session variables
SET @name = "Root";
```

```sql
-- local vars: any var that is defined inside a function or stored procedure
-- if we didn't assign 0 it'll be NULL
DECLARE VAR_NAME TYPE DEFAULT 0
```

to copy the value from select to your local variables we use the `INTO` keyword.

```sql
CREATE PROCEDURE get_risk_factor()
BEGIN
	DECLARE risk_factor DECIMAL(9, 2) DEFAULT 0;
	DECLARE invoices_total DECIMAL(9, 2);
	DECLARE invoices_count INT;

	SELECT COUNT(*), SUM(invoice_total)
	INTO invoices_count, invoices_total
	FROM invoices;

	SET risk_factor = invoices_total / invoices_count * 5;
	SELECT risk_factor;
END
```

## Functions

functions is like the stored procedure but it can only return a single value.

```sql
CREATE FUNCTION get_risk_factor_for_client(
	client_id INT
) RETURNS int(11)

READ SQL DATA

BEGIN
	DECLARE risk_factor DECIMAL(9, 2) DEFAULT 0;
	DECLARE invoices_total DECIMAL(9, 2);
	DECLARE invoices_count INT;
	SELECT COUNT(*), SUM(invoice_total)
	INTO invoices_count, invoices_total
	FROM invoices i;
	WHERE i.client_id = client_id;

	SET risk_factor = invoices_total / invoices_count * 5;
	RETURN risk_factor;
END
```

In SQL, a deterministic function is a function whose output is entirely determined by its input parameters

```sql
CREATE FUNCTION get_risk_factor_for_client(
	client_id INT
) RETURNS int(11)

READ SQL DATA
DETERMINISTIC

BEGIN
	DECLARE risk_factor DECIMAL(9, 2) DEFAULT 0;
	DECLARE invoices_total DECIMAL(9, 2);
	DECLARE invoices_count INT;
	SELECT COUNT(*), SUM(invoice_total)
	INTO invoices_count, invoices_total
	FROM invoices i;
	WHERE i.client_id = client_id;

	SET risk_factor = invoices_total / invoices_count * 5;
	RETURN risk_factor;
END
```

---

# Triggers and Events

## Triggers

a block of code gets executed after/before the (`insert`, `update`, `delete`) actions. the trigger can't be used on its table because it will trigger an infinite loop. We can use triggers for logging and auditing

```sql
DELIMITER $$

CREATE TRIGGER payments_after_insert
	AFTER INSERT ON payments
	FOR EACH ROW
BEGIN
	UPDATE invoices
	SET payment_total = payment_total + NEW.amount
	WHERE invoice_id = NEW.invoice_id;
END $$

DELIMITER ;
```

to print all triggers in the database

```sql
SHOW TRIGGERS
```

To drop a trigger

```sql
DROP TRIGGER IF EXISTS trigger_name;
```

## Events

block of SQL code that gets executed according to a schedule.

```sql
-- to get MySQL variables
SHOW VARIABLES;

-- to set a variable value
SET GLOBAL event_scheduler = OFF/ON;
```

create a new trigger, this will create a trigger that runs every year and delete last year's audit logs from the `payments_audit` table

```sql
DELIMITER $$

CREATE EVENT yearly_delete_stale_audit_rows
ON SCHEDULE
-- AT '2019-05-01' -- for run on a specified time.
EVERY 1 YEAR STARTS '2023-01-01' ENDS '2029-01-01'
DO BEGIN
	DELETE FROM payments_audit
	WHERE action_date < NOW() - INTERVAL 1 YEAR;
END $$
```

how to view, drop, and manipulate events:

```sql
-- View events
SHOW EVENTS

-- drop event
DROP EVENT IF EXISTS event_name;

-- alter events
ALTER EVENT event_name [DISABLE/ENABLE];
```

---

# Transactions and Concurrency

## Transactions

Transaction is a group of SQL statements that represent a single unit of work. All are completed successfully or all fail like bank transactions.

### COMMIT

The `COMMIT` statement is used to `permanently save` the changes made during a transaction. When you issue a `COMMIT`, all the changes made during the transaction become permanent and cannot be rolled back.

```sql
START TRANSACTION;

INSERT INTO orders (customer_id, order_date, status)
VALUES (1, '2019-01-01', 1);

INSERT INTO order_items
VALUES (1, 1, 1, 1);

COMMIT;
```

### ROLLBACK

The `ROLLBACK` statement is used to undo the changes made during a transaction that has not been committed yet. If there is an error or any reason you want to discard the changes made during a transaction, you issue a `ROLLBACK`.

```sql
START TRANSACTION;

INSERT INTO orders (customer_id, order_date, status)
VALUES (1, '2019-01-01', 1);

INSERT INTO order_items
VALUES (1, 1, 1, 1);

ROLLBACK;
```

### SAVEPOINT (Partial Rollback)

You can use `savepoints` to create points within transactions which you can later roll back. This allows for more granular control over rolling back only part of a transaction.

```sql
START TRANSACTION;
-- Your SQL statements here
SAVEPOINT my_savepoint;
-- More SQL statements
ROLLBACK TO my_savepoint;
-- Continue or ROLLBACK again
COMMIT;
```

### Locking

When you try to run a transaction with some rows, if any other transaction tries to deal with these rows in the first transaction it'll wait until the first transaction is done then it will be executed and commit the changes. this is the default behavior in MySQL.

## Common concurrency problems

### Lost updates

this happens when two transactions try to update the same record, For example:
first wants to update `customer_name` and is not committed yet, and the second wants to update `phone_number` and also it's not committed yet.

![[../../imgs/Pasted image 20231219113839.png]]

In this case if MySQL didn't use the `lock`, the second transaction will override the first transaction changes. But by default, MySQL uses locks to prevent this behavior.

### Dirty Reads

A dirty read occurs in a database when a transaction reads data that has been modified by another transaction but has not yet been committed

![[../../imgs/Pasted image 20231219114334.png]]

### NON-Repeating

A non-repeatable read occurs when a transaction reads the same data multiple times within the same transaction, and between those reads, another transaction modifies or deletes the data.

![[../../imgs/Pasted image 20231219114756.png]]

### Phantom Reads

A phantom read is a phenomenon that can occur in database transactions, particularly in scenarios where multiple transactions are concurrently modifying a range of data. A phantom read happens when a transaction retrieves a set of rows that match a certain condition, but between the initial read and a subsequent read or modification, another transaction inserts or deletes rows that also match the condition. As a result, the second read in the first transaction includes rows that were not present in the initial read.

![[../../imgs/Pasted image 20231219115015.png]]

Each problem has its isolation level to solve it.

## Isolation

### levels

|       Name       |                            Prevented Issue                            |        Note         |
| :--------------: | :-------------------------------------------------------------------: | :-----------------: |
| READ UNCOMMITTED |                                   -                                   |          -          |
|  READ COMMITTED  |                             `Dirty Reads`                             |          -          |
| REPEATABLE READ  |         `Lost Updates`, `Dirty Reads`, `NON-Repeating Reads`          | MySQL Default Level |
|   SERIALIZABLE   | `Lost Updates`, `Dirty Reads`, `NON-Repeating Reads`, `Phantom Reads` |          -          |

The more isolation level the less performance we get.

### READ UNCOMMITTED

Didn't prevent any concurrency problem, you will face the `Dirty Reads` issue because here you read Uncommitted changes from other transactions.

```sql
SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
START TRANSACTION;
SELECT points FROM customers WHERE customer_id = 1;
COMMIT;
```

### READ COMMITTED

This isolation level prevents `Dirty Reads` but you may face the `NON-Repeating Reads` issue if another transaction changes a value you are reading now and commits the changes.

```sql
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;
START TRANSACTION;

-- For example, this will return 20
SELECT points FROM customers WHERE customer_id = 1;

-- another transaction runs at this moment

-- this will return 30
SELECT points FROM customers WHERE customer_id = 1;
COMMIT;
```

Another transaction starts to change the value

```sql
START TRANSACTION;
UPDATE customers
SET points = 20
WHERE customer_id = 1;
COMMIT;
```

### REPEATABLE READ

Will solve `Dirty Reads`, and `NON-Repeating Reads` issues. this is the default option of isolation in MySQL. If the first transaction runs and is not committed yet it will return a value, in this case, if another transaction runs and updates the customer value and makes a new customer located in `VA` it will not be included in the result of the first transaction because it's not committed yet.

```sql
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;
START TRANSACTION;
SELECT * FROM customers WHERE state = 'VA';
COMMIT;
```

The second transaction will make a new record valid for the criteria above, but it'll not be in the result and this is the `phantom read` issue.

```sql
START TRANSACTION;
UPDATE customers
SET state = 'VA'
WHERE customer_id = 1;
COMMIT;
```

### SERIALIZABLE

This transaction isolation level solves all concurrency issues. if the following transaction starts to update the customer with the `customer_id` 3 and is not committed yet. if the second transaction starts at this moment it'll wait for the first transaction to be committed then it will start.

```sql
START TRANSACTION;
UPDATE customers
SET state = 'VA'
WHERE customer_id = 3;
COMMIT;
```

```sql
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;
START TRANSACTION;
SELECT * FROM customers WHERE state = 'VA';
COMMIT;
```

it will fix all the concurrency issues above.

## Deadlock

A deadlock in MySQL, as in other relational database systems, occurs when two or more transactions are blocked indefinitely, each waiting for the other to release a lock. In simpler terms, a deadlock is a situation where transactions are unable to proceed because each holds a lock that the other needs to continue.

Deadlocks in Steps:

1. **Transaction A acquires a lock on resource X.**
2. **Transaction B acquires a lock on resource Y.**
3. **Transaction A now tries to acquire a lock on resource Y (which Transaction B holds), and Transaction B tries to acquire a lock on resource X (which Transaction A holds).**
4. **Both transactions are now waiting for each other to release the lock, resulting in a deadlock.**

```sql
-- Transaction A
START TRANSACTION;
UPDATE table1 SET column1 = 'new_value' WHERE id = 1; -- Acquires a lock on row with id = 1

-- Transaction B (concurrently)
START TRANSACTION;
UPDATE table2 SET column2 = 'new_value' WHERE id = 2; -- Acquires a lock on row with id = 2

-- Transaction A (later)
UPDATE table2 SET column2 = 'new_value' WHERE id = 2; -- Tries to acquire a lock on row with id = 2 (held by Transaction B)

-- Transaction B (later)
UPDATE table1 SET column1 = 'new_value' WHERE id = 1; -- Tries to acquire a lock on row with id = 1 (held by Transaction A)
```

We can `avoid` the deadlocks not prevent them, To `avoid` deadlocks, consider the following best practices:

1. Follow the same order in the transaction code to reduce the deadlock.
2. Make transactions code short as possible to take less time.
3. Make Transactions run in scheduled time to run in non-peak time.

---

# Data types

## String type

|    Type    |                   Size / Max Size                    |           Potential Usage           |
| :--------: | :--------------------------------------------------: | :---------------------------------: |
|  CHAR(x)   |                     Fixed-length                     |            Country Code             |
| VARCHAR(x) | Max: 65535, characters (~64KB) - (50-255) good range | phone, usernames, passwords ...etc. |
| MEDIUMTEXT |                       Max 16MB                       |  JSON, CSV, medium size text books  |
|  LONGTEXT  |                       Max 4GB                        |  Textbooks, and years of log files  |
|  TINYTEXT  |                     max 255 Byte                     |              255 chars              |
|    TEXT    |                       Max 64KB                       |              64K char               |

English Language: `1 Byte`
European (Middle-Eastern): `2 Bytes`
Asian: `3 Bytes`

## Integer type

|       Type       |  Size   |
| :--------------: | :-----: |
|     TINTINT      | 1 Bytes |
| UNSIGNED TINTINT |    -    |
|     AMALLINT     | 2 Bytes |
|    MEDIUMINT     | 3 Bytes |
|       INT        | 4 Bytes |
|      BIGINT      | 8 Bytes |

Integer datatypes have a `Zero Fill` option which allows you to pad the unused part of the integer and set them to `zeros`. For example:

```sql
-- Number 1 will appear like this.
INT(4) --> 0001
```

**`NOTE:` This only affects how values will be displayed not how it will be stored**

## Fixed/Floating Point type

|                      Type                       | Size |
| :---------------------------------------------: | :--: |
| DECIMAL (number_length, digits_after_precision) |  -   |
|                       DEC                       |  -   |
|                     NUMERIC                     |  -   |
|                      FIXED                      |  -   |
|                      FLOAT                      |  4B  |
|                     DOUBLE                      |  8B  |

## Boolean type

|     Type     |  Possible values  |
| :----------: | :---------------: |
| BOOL/BOOLEAN | TRUE/FALSE or 1/0 |

## Enum and Set type

In MySQL, the `ENUM` data type is used to define a column that can have a set of predefined values. Each value in the `ENUM` must be one of the possible enumeration values specified at the time of column creation.

```sql
CREATE TABLE colors (
	color_name ENUM('Red', 'Green', 'Blue', 'Yellow', 'Black', 'White')
);
```

In MySQL, the `SET` data type is similar to `ENUM` but allows a column to contain zero or more values from a predefined set. Sets are case Sensitivity

```sql
CREATE TABLE hobbies (
	hobbies_set SET('Reading', 'Writing', 'Drawing', 'Coding', 'Traveling')
);
```

If the set of possible values is likely to change over time, it might be better to use a reference table. If there are a large number of possible values, or if the values are more descriptive, a reference table might be a better choice.

## Date and Time type

|   Type    |      Size       |
| :-------: | :-------------: |
|   DATE    |        -        |
|   TIME    |        -        |
| DATETIME  |       8B        |
| TIMESTAMP | 4B (up to 2038) |
|   YEAR    |                 |

If you'll store dates after the 2038 year it's better to use the `DATETIME` instead of `TIMESTAMP`. Read more about the year 2038 problem 😃.

## Blob type

Used for storing binary data

|    Type    | Size |
| :--------: | :--: |
|  TINYBLOB  | 225B |
|    BLOB    | 65KB |
| MEDIUMBLOB | 16MB |
|  LONGBLOB  | 4GB  |

it's not the best practice to store binary data in your database because this approach does the following:

- Increase database size
- Slow the backup process
- Slow the performance
- More effort to write code that reads/writes images to the database

## JSON type

```sql
UPDATE products
SET properties = '
{
	"name": "root",
	"password": "toor"
}
'
WHERE product_id = 1;

-- or --

UPDATE products
SET properties = JSON_OBJECT(
	'name', 'root',
	'password', 'toor',
	'numbers', JSON_ARRAY(1, 2, 3),
	'data', JSON_OBJECT('name', 'test')
)
WHERE product_id = 1;
```

All of these ways produce the following JSON object

```json
{
  "name": "root",
  "password": "toor",
  "numbers": [1, 2, 3],
  "data": {
    "name": "test"
  }
}
```

to get data from the array attributes we can use the following ways

```sql
-- $ for the current JSON document
-- . for accessing the passed key
SELECT JSON_EXTRACT(properties, '$.name')
FROM products
WHERE product_id = 1;

-- OR --

-- $ for the current JSON document
-- . for accessing the passed key
SELECT properties -> '$.name'       --> return "root"
-- SELECT properties -> '$.numbers[0]' --> return 1
-- SELECT properties -> '$.data.name'  --> return "test"

-- if you want to get the value without the "" use ->>
-- this will be used too in the comparing in WHERE clause.
SELECT properties ->> '$.name'       --> return "root"

FROM products
WHERE product_id = 1;
```

To update a JSON object we use the `JSON_SET`

```sql
UPDATE products
SET properties = JSON_SET(
	properties,          -- the JSON object will be updated
	"$.name", "Hossam"
)
WHERE product_id = 1;
```

To remove data from JSON we use the `JSON_REMOVE`, this will return a new JSON object with the new data without the removed item.

```sql
UPDATE products
SET properties = JSON_REMOVE(
	properties,          -- the JSON object will be updated
	"$.name"
)
WHERE product_id = 1;
```

---

# Designing Databases

Skipped for now

---

# Indexing for High Performance

## Indexes

`Indexes` are a powerful tool used in the background of a database to speed up querying. When we create a new index on a table attribute we create a new lookup method to search over the table records using the given indexed attribute such as the following image:

![Pasted image 20231220103950](https://github.com/0xGhazy/0xGhazy/assets/60070427/5fce1fb0-4e04-4c1e-9af4-526ebad3c4c2)

in this case, if we wanna search for customers with `state` it's faster than usual because indexes are stored in memory not on the desk. the select won't test all records it will search in the indexed state binary tree and get a reference to the row.

**Cost of indexes**

- Increase the size of the database because it's stored next to the table.
- Slow down the write operation.

So we need to use indexes with `performance-critical` queries. Don't use it everywhere and in the designing phase, it's used only if we have a slow query.

Create Index

```sql
CREATE INDEX idx ON customers(points);
```

### Prefix index

When you create an index you can prefix the string index with the numbers of chars to be indexed, The following code will create an index that searches on the first 10 chars of the customer's last name.

```sql
CREATE INDEX idx ON customers(last_name(10));
```

to define the number to create an index we use the following code to get a satisficed number of unique values to be identified based on our passed number.

```sql
SELECT
	COUNT(DISTINCT LEFT(last_name, 1)) -- 25 unique value
	COUNT(DISTINCT LEFT(last_name, 5)) -- 966 unique value
	COUNT(DISTINCT LEFT(last_name, 10)) -- 996 unique value
FROM customers;
```

since 5 gives you 966 unique values and 10 gives you 996 -small improvement- we'll choose the `5` to be our indexed length of the customer `last_name`.

### Full-text index

```sql
CREATE FULLTEXT INDEX idx_title_body ON posts (title, body);
```

To search in the text-indexed text we can use the following code

```sql
SELECT *
FROM posts
WHERE MATCH(title, body) AGAINST ("search value");
```

To display the record matching score you can select `MATCH(title, body) AGAINST ("search value")` and it will return the score in the range from 0 to 1.

The default mode for the search the it the `Natural language` mode, to enable the `Boolean mode` such like this:

```sql
SELECT *
FROM posts
WHERE MATCH(title, body) AGAINST ("search value" IN BOOLEAN MODE);
```

Using `-value` will return the records match the search value excluding the value that after `-`, And the word after `+` it will be included in the search result, `"value"`
By surrounding the search value that means this value **must** be included in the result.

### Composite index

```sql
CREATE INDEX idx_state_points ON customers(state, points);
```

the order of the columns **matter**. We have 2 rules to apply here:

- Put the most frequently used columns first.
- Put the column with the higher cardinality to narrow the search range.

![Pasted image 20231220133020](https://github.com/0xGhazy/0xGhazy/assets/60070427/38411dd6-4f60-4d5a-b532-054d8ac46dba)

Use these rules reasonably, and choose the one that will fit your case. To choose a specific index to run in your search use the following code:

```sql
SELECT customers
USE INDEX(index_name)
WHERE state LIKE 'search_criteria' AND
	  last_name LIKE 'search_criteria';
```

## When indexes are ignored?

**Example 1:**

```sql
SELECT customer_id
FROM customers
WHERE state = `CA` OR points > 1000;
```

![Pasted image 20231220135446](https://github.com/0xGhazy/0xGhazy/assets/60070427/693cae44-4487-4dc5-9b75-e25516bf2ab2)

**Explain Example 1**
In the previous example, if you have a composite index on points and states, The code will test all records in your database. This case isn't a `full-table` scan its a `full-index` it reads data from memory not from the disk. to solve a problem like this we have to divide the query and union the result together.

```sql
EXPLAIN
	SELECT customer_id
	FROM customers
	WHERE state = `CA`
	UNION
	SELECT customer_id
	FROM customers
	WHERE points > 1000;
```

![Pasted image 20231220135419](https://github.com/0xGhazy/0xGhazy/assets/60070427/d7f17bb9-c5bc-455b-b45c-5c544a996275)

**Example 2:**

```sql
EXPLAIN
	SELECT customer_id
	FROM customers
	WHERE points + 10 > 2010;
```

![Pasted image 20231220135935](https://github.com/0xGhazy/0xGhazy/assets/60070427/7c51689b-bf97-428b-9859-3bbbf0a81aff)

**Explain Example 2**
this example will test the whole database records against the specified where clause but it's also a `full-index` scan. to solve this problem we can change the criteria in the where clause to `points > 2000` it'll give the same result but faster than the above code result.

```sql
EXPLAIN
	SELECT customer_id
	FROM customers
	WHERE points > 2000;
```

![Pasted image 20231220140037](https://github.com/0xGhazy/0xGhazy/assets/60070427/4e34d5e5-4a31-4daa-96f3-e40d0a42e0c4)

## Sorting data with indexes

If you have a composite index such as `INDEX(state, points)` you can perform sorting in the following rules:

1. You can sort data based on `state` value

```sql
SELECT customer_id
FROM customers
ORDER BY state
```

2. You can sort data based on `state`, and `points` values

```sql
SELECT customer_id
FROM customers
ORDER BY state, points
```

3. You can sort data based on the reverse order of `both` columns

```sql
SELECT customer_id
FROM customers
ORDER BY state DESC, points DESC
```

4. You can sort data based on the `points` only

```sql
SELECT customer_id
FROM customers
-- this case is the fastest one because data is already sorted by the state before.
ORDER BY points
```

To know the cost of the query you can use the following command to show the status variable with specified criteria

```sql
SHOW STATUS LIKE 'last_query_cost';
```

If you try to select something other than the `state`, `value`, and `customer_id`, You'll perform a `full-table` because the indexes only have their column to look up with a reference to the `record id` as we mentioned before.

**Before creating new indexes check the existing ones.**

---

## Securing Databases

Skipped for now
