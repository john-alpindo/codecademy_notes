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