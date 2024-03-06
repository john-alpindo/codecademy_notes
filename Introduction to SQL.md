# Manipulation
## CREATE TABLE
```sql
CREATE TABLE customers (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    email VARCHAR(100),
    address VARCHAR(200)
);
```
## INSERT
```sql
INSERT INTO customers (id, name, email, address)
VALUES (2, 'Jane Smith', 'janesmith@example.com', '456 Elm St');
```
## SELECT
```sql
SELECT * FROM customers;
```
## ALTER
```sql
ALTER TABLE customers
ADD COLUMN age INT;
```
## UPDATE
```sql
UPDATE customers
SET age = 25
WHERE id = 2;
```
## DELETE
```sql
DELETE FROM customers
WHERE id = 2;
```
## CONSTRAINTS
```sql
CREATE TABLE employees (
    id INT PRIMARY KEY,
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