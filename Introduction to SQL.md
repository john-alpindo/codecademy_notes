# Manipulation
## CREATE TABLE
```sql
CREATE TABLE friends (
  id INTEGER,
  name TEXT,
  birthday DATE
);
```
## INSERT
```sql
INSERT INTO friends (id, name, birthday)
VALUES (1, 'Ororo Munroe', '1940-05-30');
```
## SELECT
```sql
SELECT * FROM friends;
```
## UPDATE
```sql
UPDATE friends
SET name = 'Storm'
WHERE id = 1;
```
## ALTER
```sql
ALTER TABLE friends
ADD COLUMN email TEXT;
```
## DELETE
```sql
DELETE FROM friends
WHERE id = 1;
```
## CONSTRAINTS
```sql
CREATE TABLE employees (
    id INTEGER PRIMARY KEY,
    name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE,
    address VARCHAR(200),
    hire_date DATE DEFAULT CURRENT_DATE
);
```
- **PRIMARY KEY** columns can be used to uniquely identify the row. Attempts to insert a row with an identical value to a row already in the table will result in a constraint violation which will not allow you to insert the new row.

- **UNIQUE** columns have a different value for every row. This is similar to **PRIMARY KEY** except a table can have many different **UNIQUE** columns.

- **NOT NULL** columns must have a value. Attempts to insert a row without a value for a **NOT NULL** column will result in a constraint violation and the new row will not be inserted.

- **DEFAULT** columns take an additional argument that will be the assumed value for an inserted row if the new row does not specify a value for that column.

# Queries
## SELECT
In the previous section, we learned that the **SELECT** statement is used to query data from a database, and the asterisk (*) symbol is used to select all columns. However, if we are only interested in specific columns, we can select them individually by their names, separated by commas.
```sql
SELECT name, email
FROM employees;
```
## AS
This can be especially useful when dealing with more complex queries.
```sql
SELECT name AS 'Employee Name',
    email AS 'Email'
FROM employees;
```
## DISTINCT
Used to return only distinct (different) values. In a table, a column may contain many duplicate values; and sometimes you only want to list the different (distinct) values.
```sql
SELECT DISTINCT column_name
FROM table_name;
```
## WHERE
The **WHERE** clause is used to filter records. The **WHERE** clause is used to extract only those records that fulfill a specified *condition*.
```sql
SELECT name
FROM employees
WHERE name = 'John Doe';
```
## LIKE
The **LIKE** operator is used in a **WHERE** clause to search for a specified pattern in a column.
```sql
SELECT *name*
FROM movies
WHERE name LIKE 'Se_en';
```
**_** means you can substitute any individual character here without breaking the pattern
```sql
SELECT *name*
FROM movies
WHERE name LIKE '%man%';
```
**%** represents a sequence of zero, one, or multiple characters
## IS NULL | IS NOT NULL
The **IS NULL** operator is used to test for empty values (NULL values).
```sql
SELECT name
FROM employees
WHERE email IS NULL;
```
## BETWEEN
The **BETWEEN** operator selects values within a given range. The values can be numbers, text, or dates.
```sql
SELECT *
FROM movies
WHERE name BETWEEN 'A' AND 'J';
```
In this example, the query will return all movies with names that start with a letter between A and up to J, but not including J.
```sql
SELECT *
FROM movies
WHERE year BETWEEN 1990 AND 1999;
```
In this example, the query will return all movies released in year from 1990 up to 1999, including both years.
## AND
The **AND** operator is used to filter records based on more than one condition.
```sql
SELECT *
FROM movies
WHERE year BETWEEN 1990 AND 1999
AND genre = 'comedy';
```
## OR
The **OR** operator displays a record if any of the conditions separated by **OR** is TRUE.
```sql
SELECT *
FROM movies
WHERE genre = 'romance'
   OR genre = 'comedy';
```
## ORDER BY
The **ORDER BY** keyword is used to sort the result-set in ascending or descending order.
```sql
SELECT *
FROM movies
WHERE imdb_rating > 8
ORDER BY year DESC;
```
## LIMIT
The **LIMIT** clause is used to specify the number of records to return.
```sql
SELECT *
FROM movies
ORDER BY imdb_rating DESC
LIMIT 3;
```
## CASE
The **CASE** statement goes through conditions and returns a value when the first condition is met.
```sql
SELECT name,
    CASE
        WHEN imdb_rating > 8 THEN 'Fantastic'
        WHEN imdb_rating > 6 THEN 'Poorly Received'
        ELSE 'Avoid at All Costs'
    END AS 'Review'
FROM movies;
```

# Aggregate Functions
## COUNT
The **COUNT** function returns the number of rows that matches a specified criterion.
```sql
SELECT COUNT(*)
FROM movies;
```
## SUM
The **SUM** function returns the total sum of a numeric column.
```sql
SELECT SUM(downloads)
FROM fake_apps;
```
## MAX | MIN
The **MAX** and **MIN** functions return the highest and lowest values in a selected column.
```sql
SELECT MAX(downloads)
FROM fake_apps;
```
## AVG
The **AVG** function returns the average value of a numeric column.
```sql
SELECT AVG(downloads)
FROM fake_apps;
```
## ROUND
The **ROUND** function rounds a number to a specified number of decimal places.
```sql
SELECT ROUND(price, 0)
FROM fake_apps;
```
## GROUP BY
The **GROUP BY** statement is often used with aggregate functions (COUNT, MAX, MIN, SUM, AVG) to group the result-set by one or more columns.
```sql
SELECT category,
  SUM(downloads)
FROM fake_apps
GROUP BY category;
```
This query will return the total number of downloads for each category of apps.
```sql
SELECT category,
  price,
  AVG(downloads)
FROM fake_apps
GROUP BY 1, 2;
```
This query will return the average number of downloads for each category of apps at each price point. Here, **1** and **2** are used to reference the first and second columns selected in the **SELECT** statement.
## HAVING
The **HAVING** clause was added to SQL because the **WHERE** keyword could not be used with aggregate functions.
```sql
SELECT price, 
   ROUND(AVG(downloads)),
   COUNT(*)
FROM fake_apps
GROUP BY price
HAVING COUNT(*) > 10;
```
This query will return the average number of downloads and the count of apps at each price point, but only where the count of apps is greater than 10.

## strftime
The **strftime** function is a powerful tool for querying time and date values. It allows you to extract the date and time from a timestamp, and to format the output. The **strftime** function takes two arguments:
- The first argument is the [format](https://www.sqlite.org/lang_datefunc.html) of the output.
- The second argument is the column that contains the timestamp.
```sql
SELECT strftime('%Y', timestamp)
FROM table_name;
```

# Multiple Tables
## JOIN | INNER JOIN
The **JOIN** clause is used to combine rows from two or more tables, based on a related column between them.
```sql
SELECT *
FROM orders
JOIN subscriptions
  ON orders.subscription_id = subscriptions.subscription_id;
```
The final result has all the columns from both tables but *only the rows* for which there is a match between the columns.
## LEFT JOIN
The **LEFT JOIN** keyword returns all records from the left table (table1), and the matched records from the right table (table2). The result is NULL from the right side if there is no match.
```sql
SELECT *
FROM newspaper
LEFT JOIN online
  ON newspaper.id = online.id;
```
## Primary Key
A **PRIMARY KEY** is a unique identifier for each record in a table. It can consist of a single column or multiple columns in combination.
```sql
CREATE TABLE table_name (
  column1 INTEGER PRIMARY KEY,
  column2 TEXT
);
```
## Foreign Key
A **FOREIGN KEY** is a field (or collection of fields) in one table that uniquely identifies a row of another table or the same table.
```sql
CREATE TABLE table_name (
  column1 INTEGER PRIMARY KEY,
  column2 INTEGER FOREIGN KEY REFERENCES other_table(column)
);
```
## CROSS JOIN
The **CROSS JOIN** keyword returns the Cartesian product of the two tables.
```sql
SELECT shirts.shirt_color,
   pants.pants_color
FROM shirts
CROSS JOIN pants;
```
## UNION
The **UNION** operator is used to stack the result sets of two or more **SELECT** statements on top of one another.
```sql
SELECT *
FROM table1
UNION
SELECT *
FROM table2;
```
SQL has strict rules for appending the result sets together:
- The number of columns must be the same in all queries.
- The columns must also have similar data types.
- The columns in each **SELECT** statement must also be in the same order.
## WITH
The **WITH** clause stores the result of a query in a temporary table using an alias. Multiple **WITH** clauses can be defined in a single query.
```sql
WITH previous_query AS (
  SELECT customer_id,
    COUNT(subscription_id) AS 'subscriptions'
  FROM orders
  GROUP BY customer_id
)
SELECT customers.customer_name,
  previous_query.subscriptions
FROM previous_query
JOIN customers
  ON previous_query.customer_id = customers.customer_id;
```