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
```sql
INSERT INTO book
VALUES (
  'Postgres for Beginners',
  '0-5980-6249-1',
  25,
  4.99,
  'Learn Postgres the Easy Way',
  'Codecademy Publishing'
);
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
 Altering a table is done when you want to add, delete, or modify columns. The **ALTER TABLE** statement is used to add, delete, or modify columns in an existing table.
```sql
ALTER TABLE friends
ADD COLUMN email TEXT;
```
```sql
ALTER TABLE chapter
DROP COLUMN content;
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
Alias names can be used to rename a column or table in SQL. An alias only exists for the duration of the query. This can be especially useful when dealing with more complex queries.
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
```sql
SELECT modal_text,
  COUNT(DISTINCT CASE
    WHEN ab_group = 'control' THEN user_id
    END) AS 'control_clicks'
FROM onboarding_modals
GROUP BY 1
ORDER BY 1;
```
Here, the **CASE** statement is used as an argument for the **COUNT** function to count the number of distinct **user_id** values where **ab_group** is equal to **'control'**.
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
- This query will return the average number of downloads and the count of apps at each price point, but only where the count of apps is greater than 10.
- Alias names cannot be used in the **HAVING** clause, so you will have to use the original column name.

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
```sql
SELECT book.title AS book,
  chapter.title AS chapters
FROM book, chapter
WHERE book.isbn = chapter.book_isbn;
```
The final result has all the columns from both tables but *only the rows* for which there is a match between the columns.
```sql
SELECT book.title AS book_title,
  chapter.title AS chapter_title,
  page.content AS page_content
FROM book
JOIN chapter
  ON book.isbn = chapter.book_isbn
JOIN page
  ON chapter.id = page.chapter_id;
```
It's considered better practice to use explicit JOIN syntax for better readability and maintainability. Specially when dealing with multiple tables.
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
This query will return a combination of all shirts and pants, regardless of color.
```sql
SELECT shirts.shirt_color,
   pants.pants_color
FROM shirts, pants;
```
This query will return the same result as the previous one.
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

# Designing A Database Schema
## Database Schema
### Online Database Design Tools
- [DbDiagram.io](http://dbdiagram.io/) - a free, simple tool to draw ER diagrams by just writing code, designed for developers and data analysts.
- [SQLDBM](http://sqldbm.com/home) - SQL Database Modeler
- [DB Designer](http://dbdesigner.net/) - online database schema design and modeling tool
### Common Data Types
| Data Type  | Representation                                      | Value      | Display  |
|------------|-----------------------------------------------------|------------|----------|
| integer    | whole number                                        | 617        | 617      |
| decimal    | floating-point number                               | 26.17345   | 26.17345 |
| money      | fixed floating-point number with 2 decimal places  | 6.17       | $6.17    |
| boolean    | logic                                               | TRUE, FALSE| t, f     |
| char(n)    | fixed-length string removes trailing blanks        | '123 '     | '123'    |
| varchar(n) | variable-length string                              | '123 '     | '123 '   |
| text       | unlimited-length string                             | '123 '     | '123 '   |
## Database Keys
Keys enable you to establish relationships between tables and ensure the uniqueness of data.
### Primary Key
A unique identifier for each record in a table. It can consist of a single column or multiple columns in combination.
```sql
CREATE TABLE book (
  title varchar(100),
  isbn varchar(50) PRIMARY KEY,
  pages integer,
  price money,
  description varchar(256),
  publisher varchar(100)
);
```
### Key Validation
#### Information Schema
The **INFORMATION_SCHEMA** is a meta-database that holds information about your current database. The **INFORMATION_SCHEMA** is the place to look to find information about the structure of your database.
```sql
SELECT
  constraint_name, table_name, column_name
FROM
  information_schema.key_column_usage
WHERE
  table_name = 'book';
```
This query will validate the primary key for the **book** table.
### Composite Primary Key
A primary key that consists of multiple columns. This is useful when you want to ensure uniqueness across multiple columns.
```sql
CREATE TABLE popular_books (
  book_title varchar(100),
  author_name varchar(50),
  number_sold integer,
  number_previewed integer,
  PRIMARY KEY (book_title, author_name)
);
```
### Foreign Key
A field (or collection of fields) in one table that uniquely identifies a row of another table or the same table.
```sql
CREATE TABLE person (
  id integer PRIMARY KEY,
  name varchar(20),
  age integer
);

CREATE TABLE email (
  email varchar(20) PRIMARY KEY,
  person_id integer REFERENCES person(id),
  storage integer,
  price money
);
```
**Where should we assign this foreign key?** Is it best suited for the person table or the email table? To address this inquiry, we must examine the relationship between person and email. Is the creation of a person entry contingent upon the existence of an email entry? Typically not. A person might possess zero, one, or multiple email addresses. Therefore, when creating a record in the person table, it's not mandatory for a corresponding record to exist in the email table.

On the other hand, does the creation of an email entry necessitate the presence of a valid person entry? Usually, yes. It's inappropriate to generate an email address for a non-existent person. Consequently, the **foreign key should be placed in the email table**. This ensures that a valid record in the person table must already exist before adding a record to the email table.
## Database Relationships
### One-to-One Relationship
A one-to-one relationship is created when a record in one table is related to only one record in another table.
```sql
CREATE TABLE driver (
    license_id char(20) PRIMARY KEY,
    name varchar(20),
    address varchar(100),
    date_of_birth date
);      

CREATE TABLE license (
    id integer PRIMARY KEY,
    state_issued varchar(20),
    date_issued date,
    date_expired  date,
    license_id char(20) REFERENCES driver(license_id) UNIQUE
);
```
#### Implementing a One-to-One Relationship
- **UNIQUE** constraint on the foreign key in the table.
### One-to-Many Relationship
A one-to-many relationship is created when a record in one table is related to one or more records in another table.
```sql
CREATE TABLE person (
  id integer PRIMARY KEY,
  name varchar(20),
  age integer
);

CREATE TABLE email (
  email varchar(20) PRIMARY KEY,
  person_id integer REFERENCES person(id),
  storage integer,
  price money
);
```
#### Implementing a One-to-Many Relationship
- a foreign key in the table on the many side of the relationship.
### Many-to-Many Relationship
A many-to-many relationship is created when a record in one table can be related to one or more records in another table and vice versa.
```sql
CREATE TABLE student (
  id integer PRIMARY KEY,
  name varchar(20),
  age integer
);

CREATE TABLE course (
  id integer PRIMARY KEY,
  title varchar(20),
  department varchar(20)
);

CREATE TABLE student_course (
  student_id integer REFERENCES student(id),
  course_id integer REFERENCES course(id),
  PRIMARY KEY (student_id, course_id)
);
```
#### Implementing a Many-to-Many Relationship
- create a third table, known as a **cross-reference table**.
- foreign keys referencing the primary keys of the two member tables.
- a composite primary key made up of the two foreign keys.
#### Querying a Many-to-Many Relationship
```sql
SELECT column_one AS alias_one, 
  column_two AS alias_two
FROM table_one
INNER JOIN joined_table
ON table_one.primary_key = joined_table.foreign_key_one
INNER JOIN table_two
ON table_two.primary_key = joined_table.foreign_key_two
```
```sql
SELECT book.title AS book_title,
  author.name AS author_name,
  book.description AS book_description
FROM book
JOIN books_authors
  ON book.isbn = books_authors.book_isbn
JOIN author
  ON author.email = books_authors.author_email;
```
```sql
SELECT author.name AS author_name,
  author.email AS author_email,
  book.title AS book_title
FROM author
JOIN books_authors
  ON author.email = books_authors.author_email
JOIN book
  ON book.isbn = books_authors.book_isbn;
```
Here's a query to show the relationship between the **author** and **book** tables. The **books_authors** table is used to join the two tables together. The **books_authors** table contains the **isbn** and **email** columns, which are used to join the **book** and **author** tables, respectively.

# Constraints
Constraints are the rules enforced on data columns on a table. These are used to prevent the database from losing internal and external integrity.
## PostgreSQL Constraints
### PostgreSQL Data Types
| Name         | Description                                                  |
|--------------|--------------------------------------------------------------|
| boolean      | true/false                                                   |
| varchar      | text with variable length, up to n characters (if specified)|
| date         | calendar date                                                |
| integer      | whole number value between -2147483648 and +2147483647       |
| numeric(a, b)| decimal with total digits (a) and digits after the decimal point (b)|
| time         | time of day (no time zone)                                   |
- Data types don't prevent you from entering invalid data, but they do restrict the type of data that can be entered. For example, a phone_number column could be set to the varchar data type, but it could still contain non-numeric characters.
- PostgreSQL will try to interpret the data type based on the value you provide. If you provide a string, it will try to interpret the column as a string. If you provide a number, it will try to interpret the column as a number. For example, if you try to insert 1.5 into a column with the integer data type, PostgreSQL will round the number to 2.
### Nullability Constraints
#### NOT NULL
Ensures that a column cannot have a NULL value.
```sql
CREATE TABLE table_name (
  column_name data_type NOT NULL
);
```
Here's an example of a table with a **NOT NULL** constraint when creating a table.
### Improving Tables with Constraints
#### SET NOT NULL
```sql
ALTER TABLE table_name
ALTER COLUMN column_name SET NOT NULL;
```
#### DROP NOT NULL
```sql
ALTER TABLE table_name
ALTER COLUMN column_name DROP NOT NULL;
```
If there are already NULL values in the column, you will not be able to add a **NOT NULL** constraint until you have updated the column to have no NULL values.
#### Updating Columns to Have No NULL Values
```sql
UPDATE table_name
SET column_name = 'default value'
WHERE column_name IS NULL;
```
```sql
UPDATE speakers
SET organization = 'Unaffiliated'
WHERE organization IS NULL;
```
### Check Constraints
#### CHECK
In PostgreSQL, the **CHECK** constraint is used to limit the value range that can be placed in a column.
- Documentation: [PostgreSQL CHECK Constraint](https://www.postgresql.org/docs/16/ddl-constraints.html)
```sql
CREATE TABLE table_name (
  column_name data_type CHECK (condition)
);
```
#### ADD CHECK
Here's an example of a table with a **CHECK** constraint when creating a table.
```sql
ALTER TABLE table_name
ADD CHECK (condition);
```
```sql
ALTER TABLE attendees
ADD CHECK (standard_tickets_reserved + vip_tickets_reserved = total_tickets_reserved);
```
Here's an example of adding a **CHECK** constraint to an existing table.

### Unique Constraints
#### UNIQUE
Ensures that all values in a column are different.
```sql
CREATE TABLE table_name (
  column_name data_type UNIQUE
);
```
Here's an example of a table with a **UNIQUE** constraint when creating a table.
```sql
CREATE TABLE registrations (
    id integer NOT NULL,
    attendee_id integer NOT NULL,
    session_timeslot timestamp NOT NULL,
    talk_id integer NOT NULL,
    UNIQUE (attendee_id, session_timeslot)
);
```
#### ADD UNIQUE
```sql
ALTER TABLE locations
ADD UNIQUE (part_id, location);
```
This requirement is suggesting that **part_id** and **location** together must be unique. This means that you can have multiple rows with the same **part_id** or the same **location**, but you cannot have multiple rows with the same **part_id** and **location** together.
### Primary Key Constraints
The **PRIMARY KEY** constraint uniquely identifies each record in a table.
```sql
ALTER TABLE attendees
ADD PRIMARY KEY (id);
```
### Foreign Key Constraints
The **FOREIGN KEY** constraint is used to prevent actions that would destroy links between tables.
```sql
ALTER TABLE registrations
ADD FOREIGN KEY (talk_id)
REFERENCES talks (id);
```
Suppose we now want to enter a registration for talk_id = 100, but there is no talk with an id of 100. This would violate the foreign key constraint, and PostgreSQL would not allow the registration to be entered.
```sql
ALTER TABLE locations
ADD FOREIGN KEY (part_id) REFERENCES parts(id);
```
Make sure that parts table has a primary key constraint on the id column before adding the foreign key constraint.
### Foreign Keys - Cascading Changes
Cause the update or deletes to automatically propagate to any child tables that reference the parent table.
- **ON DELETE CASCADE** - When a referenced row is deleted, row(s) referencing it are automatically deleted.
```sql
ALTER TABLE talks
ADD FOREIGN KEY (speaker_id)
REFERENCES speakers (id) ON DELETE CASCADE;
```
If a speaker is deleted, all talks associated with that speaker will also be deleted.
- **ON UPDATE CASCADE** - When a referenced row is updated, row(s) referencing it are automatically updated.
```sql
ALTER TABLE speakers
ADD FOREIGN KEY (organization_id)
REFERENCES organizations (id) ON UPDATE CASCADE;
```
If an organization's id is updated, all speakers associated with that organization will also be updated.