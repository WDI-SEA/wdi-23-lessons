# Advanced SQL

## Objectives
* Describe the uses of advanced queries like subqueries and unions
* Demonstrate ability to order data
* Demonstrate ability to aggregate and combine data

## Order of SQL Clauses

![SQL Clauses](SQLClauses.png)

## Selecting specific data

Not equal - `<>`

```
- LIKE - SELECT * FROM students WHERE name LIKE '%';
- DISTINCT - SELECT DISTINCT name FROM students;
- ORDER BY - SELECT * FROM students ORDER BY name DESC;
- COUNT - SELECT count(*) FROM students;
- MAX - SELECT max(age) FROM students;
- MIN - SELECT min(age) FROM students;
- AND - SELECT * from students WHERE name = 'Elie' AND age = 26;
- OR - SELECT * from students WHERE name = 'Elie' OR name ='Mary';
- IN - SELECT * FROM students WHERE name IN ('Bob', 'Tom');
- NOT IN - SELECT * FROM students WHERE name NOT IN ('Bob', 'Tom');
- LIMIT - SELECT * FROM students LIMIT 2;
- OFFSET - SELECT * FROM students OFFSET 1;
- LIMIT + OFFSET - SELECT * FROM students LIMIT 2 OFFSET 1;
- % - SELECT * FROM students WHERE name LIKE '%b';
```

Let's create a *customers* table with the following data:

```sql
 id |  name   | age |  country  | salary 
----+---------+-----+-----------+--------
  1 | Ramesh  |  32 | Ahmedabad |   2000
  3 | Kaushik |  23 | Kota      |   2000
  2 | Ramesh  |  25 |           |   1500
  4 | Kaushik |  25 | Mumbai    |       
  5 | Hardik  |  27 | Bhopal    |   8500
  6 | Komal   |     |           |   4500
```

You can use these SQL statements to create it:

```sql
CREATE TABLE customers (
  id SERIAL PRIMARY KEY,
  name VARCHAR(25),
  age INTEGER,
  country VARCHAR(25),
  salary INTEGER
);

INSERT INTO customers (name, age, country, salary) VALUES ('Ramesh', 32, 'Ahmedabad', 2000);
INSERT INTO customers (name, age, country, salary) VALUES ('Kaushik', 23, 'Kota', 2000);
INSERT INTO customers (name, age, country, salary) VALUES ('Ramesh', 25, null, 1500);
INSERT INTO customers (name, age, country, salary) VALUES ('Kaushik', 25, 'Mumbai', null);
INSERT INTO customers (name, age, country, salary) VALUES ('Hardik', 27, 'Bhopal', 8500);
INSERT INTO customers (name, age, country, salary) VALUES ('Komal', null, null, 4500);
```

## COUNT()

COUNT() is an *aggregate function*.

"In database management an aggregate function is a function where the values of multiple rows are grouped together to form a single value of more significant meaning or measurement such as a set, a bag or a list." [Read more on wikipedia.](https://en.wikipedia.org/wiki/Aggregate_function)

We use an aggregate function to get the total count of customers in a table.
```sql
SELECT COUNT(*) FROM customers;
```

What about getting the count of something more specific in customer, such as the number of rows that have the age datapoint? 
```sql
SELECT COUNT(age) FROM customers;
```

## GROUP BY

GROUP BY is used to pull together identical data points. For example, say we just want to see the different ages we have in our customer table, without having to look through the duplicates too.
```sql
SELECT age FROM customers GROUP BY age;
```

What if we just want to know how many different ages we have? We can combine GROUP BY and COUNT():
```sql
SELECT age, COUNT(age) FROM customers GROUP BY age;
```

Or maybe we want the average salaries of the customers from each country:
```sql
SELECT country, AVG(salary) FROM customers GROUP BY country;
```

### Aliases

Aliases are a piece of a SQL query that allows you to temporarily rename a table or column for the current query.

```sql
SELECT country, avg(salary) AS avgSal FROM customers GROUP BY country;
```

### Alter Table Command

```sql
ALTER TABLE customers ADD COLUMN date DATE;

ALTER TABLE customers ALTER COLUMN name SET NOT NULL;

ALTER TABLE customers DROP date;
```

### Nested queries

What if I want to get names of customers with the highest salary.

Let's try it using WHERE

```sql
SELECT name, salary FROM customers
WHERE salary = MAX(salary);
```

That will give us an error, because MAX is an aggregate function and can't be used in WHERE.

This will return the maximum rating, which we need to feed into WHERE.

```sql
SELECT name, salary FROM customers
WHERE salary = (
	SELECT MAX(salary) FROM customers
);
```

### Conditionals

#### CASE Statement
The CASE statement is used when you want to display different things depending on the data that you've queried from the database. There's two different ways to structure a CASE statement shown below. Note that in the first example you can only compare against single values while in the second example you can use actual expressions for evaluation. Also note that CASE statements require an ELSE statement.

```sql
SELECT name,
	age, 
		CASE WHEN age<25
		THEN 'young adult'
		ELSE 'adult' 
		END AS age_group 
FROM customers;
```

### FOREIGN KEYS

This is where the **relational** part comes in! Foreign keys allow entries in one table to refer to entires in another table.

What are some examples of when this would be useful?
* (library) books table references an authors table
* (elementary school) a students table refereces a classes table, which references teachers table, which references a schools table, which references a districts table, etc.

Let's create a table to join with our customers:

```sql
CREATE TABLE orders (
	id SERIAL PRIMARY KEY,
	name VARCHAR(50),
  quantity INTEGER,
	customer_id INTEGER REFERENCES customers(id)
);
```

And let's put in some test data to play with:

```sql
INSERT INTO orders (name, quantity, customer_id) VALUES ('Books', 5, 1);
INSERT INTO orders (name, quantity, customer_id) VALUES ('DVDs', 1, 1);
INSERT INTO orders (name, quantity, customer_id) VALUES ('Pet Food', 2, 3);
INSERT INTO orders (name, quantity, customer_id) VALUES ('Laptop', 1, 5);
INSERT INTO orders (name, quantity, customer_id) VALUES ('Clothing', 3, 2);
INSERT INTO orders (name, quantity, customer_id) VALUES ('Batteries', 10, 2);
INSERT INTO orders (name, quantity, customer_id) VALUES ('Xylophones', 5, 2);
INSERT INTO orders (name, quantity, customer_id) VALUES ('Pencils', 2, 4);
```

### JOINs

INNER JOIN gives us the intersections of tables (or the rows that are the same in each table involved in the join).

We have our customers table and now an orders table. If we wanted to get a data set with all the customers and their orders, we could use a INNER JOIN command to relate the data and return the results:

```sql
SELECT * FROM customers
INNER JOIN orders
ON customers.id = orders.customer_id;
```

The INNER JOIN is the default type of JOIN, so we could simply use the command JOIN and omit the "INNER". Inner joins only returns the records from both tables where there is matching data. So the INNER JOIN will not include any customers if they haven't purchased anything.

LEFT JOIN is similar to INNER JOIN, except it will include *all* rows from the left table (the first table listed in the query).

```sql
SELECT * FROM customers
LEFT JOIN orders
ON customers.id = orders.customer_id;
```

Similarly, RIGHT JOIN will include all the rows from the right table.

```sql
SELECT * FROM customers
RIGHT JOIN orders
ON customer.id = orders.customer_id;
```
