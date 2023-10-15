---
title: "SQL Notes"
classes: wide
header:
  teaser: /assets/images/Blogs/Learning-Notes/sql.png
  image: /assets/images/Blogs/Learning-Notes/sql.png
  og_image: /assets/images/Blogs/Learning-Notes/sql.png
ribbon: ForestGreen
description: "My cheat sheet for MySQL"
categories:
  - Notes
tags:
  - Notes
toc: true
---
بسم الله الرحمنن الرحيم, السلام عليكم ورحمة الله وبركاته

# Introduction

Hello world, hossam is here. Welcome everyone to my humble blog. In this post, I will write down all the MYSQL syntax that I encountered while studying MYSQL or through my solutions on the HackerRank website. I simply consider this publication to be my notes that make it easier for me to quickly review SQL syntax, which helps me prepare for job interviews, in addition to being a good opportunity to share information with others.

I wrote the Syntax here through my application of the [Mosh Hamidani course](https://codewithmosh.com/p/complete-sql-mastery) and some research and learning away from the course. You will also find all my solutions to SQL problems through the GitHub repository [hackerrank-sql-solutions](https://github.com/0xGhazy/hackerrank-sql-solutions)

**[Note]**: As long as you see these lines, it means that the post is under preparation and not yet completed. If there are any mistakes or misunderstandings of any information from my side, please inform me. Thank you in advance for your efforts.


## `ORDER BY` clause
used to order the results by specified column name, it's valid also for alies columns in MySQL
```sql
-- get all orders by total_price column (alies) = quantity * unit_price 
SELECT * FROM store.order_items WHERE order_id = 2 ORDER BY quantity * unit_price desc;
```

## `LIMIT` clause
used to limit the result by the number of records you wanna return.
limit clause must come at the end of your query.
```sql
--cto get the first 3 records
SELECT * FROM store.order_items WHERE order_id = 2 ORDER BY quantity * unit_price desc LIMIT 3;
```
you can use it as a pagination clause using the following syntax
```sql
-- here we will skip the first 6 reords
-- then we will return the 3 records after them
SELECT * FROM store.order_items WHERE order_id = 2 ORDER BY quantity * unit_price desc LIMIT 6, 3;
```

# Logical Operators


## - `AND`
TRUE if all the conditions separated by AND are TRUE.
```sql
SELECT name FROM STATION WHERE LAT_N > 38.7880 AND LAT_N < 137.2345;
```

## - `OR`
TRUE if any of the conditions separated by OR is TRUE.
```sql
SELECT name FROM STATION WHERE LAT_N > 38.7880 OR LAT_N < 137.2345;
```

## - `NOT`
Reverses the value of any other Boolean operator.
```sql
SELECT name FROM STATION WHERE NOT (LAT_N > 38.7880 AND LAT_N < 137.2345);
```

## - `IN`
TRUE if the operand is equal to one of a list of expressions.
use in logical operator to compare attributes with a list of values
```sql
SELECT * FROM Customers WHERE state in ('VA', 'FL', 'MX');
```
it can combined with the `NOT` operator to get all customers that are not from the specified states.
```sql
SELECT * FROM Customers WHERE state NOT IN ('VA', 'FL', 'MX');
```

## - `BETWEEN`
TRUE if the operand lies within the range of comparisons.	
we want to get all customers born after `1990/01/01` and before `2000/01/01`. we can use arithmetic operators such as `<=` or `<=` as follows.
```sql
SELECT * FROM Customers WHERE birth_date > '1990-01-01' AND birth_date < '2000-01-01';
```
or use the `between` operator directly 😃
```sql
SELECT * FROM Customers WHERE birth_date BETWEEN '1990-01-01' AND '2000-01-01';
```

## - `LIKE`
TRUE if the operand matches a pattern, especially with a wildcard. we use `_` to specify a character and `%` for any number of characters. Note that `%a` get all end with `a` and `a%` get all start with
```sql
-- get customer with phone ends with 9
SELECT * FROM store.customers WHERE phone LIKE '%9';


-- get customer with address contains Trail or Avenue
SELECT * FROM store.customers WHERE 
    address LIKE '%Trail%'
    OR
    address LIKE '%Avenue%';
```

## - `REGEXP`
to get all first name that contains `Elka` or `Ambur` using the pipe `|` that perform OR operation.
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
-- get all last names that contain characters in teh range [a-z]
-- this will retrn all records :)
SELECT * FROM store.customers WHERE last_name REGEXP "[a-z]";
```

## - `IS NULL`
to get records attributes with null values.

```sql
-- get all customers that have no phone number.
SELECT * FROM store.customers WHERE phone IS NULL;
```

---
