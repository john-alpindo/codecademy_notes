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