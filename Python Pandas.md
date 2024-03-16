# Creating, Loading, and Selecting Data with Pandas
## Create a DataFrame I
```python
df1 = pd.DataFrame({
    'name': ['John Smith', 'Jane Doe', 'Joe Schmo'],
    'address': ['123 Main St.', '456 Maple Ave.', '789 Broadway'],
    'age': [34, 28, 51]
})
```
This command creates a DataFrame called df1 that looks like this:
| Name       | Address         | Age |
|------------|-----------------|-----|
| John Smith | 123 Main St.    | 34  |
| Jane Doe   | 456 Maple Ave.  | 28  |
| Joe Schmo  | 789 Broadway    | 51  |

The columns **must be the same length** as the data that is being added to the DataFrame. If the data is not the same length, an error will be thrown.
## Create a DataFrame II
```python
df2 = pd.DataFrame([
    ['John Smith', '123 Main St.', 34],
    ['Jane Doe', '456 Maple Ave.', 28],
    ['Joe Schmo', '789 Broadway', 51]],
    columns=['name', 'address', 'age'])
```
This command creates a DataFrame called df2 that looks like the table from the previous example.

## Loading and Saving CSVs
```python
df = pd.read_csv('my-csv-file.csv')
df.to_csv('new-csv-file.csv')
```
## Inspect a DataFrame
```python
print(df.head())
# this will print the first five rows of the DataFrame
print(df.info())
# this will print a summary of the DataFrame
```
## Select Columns
```python
print(df['column_name'])
# this will print the column with the label column_name
print(df.column_name)
# if the column name has no spaces, you can also use this method to select a column
```
Selecting a single column will return a **Series**
## Selecting Multiple Columns
```python
print(df[['column_name1', 'column_name2']])
# take note of the double square brackets
```
Selecting multiple columns will return a **DataFrame**
## Select Rows
```python
print(df.iloc[0])
print(df.iloc[1:3])
```
Just like selecting a column name, if you select a single row, a **Series** will be returned. If you select multiple rows, a **DataFrame** will be returned.
## Selecting Rows with Logic I
```python
print(df[df.MyColumnName == desired_column_value])
```
## Selecting Rows with Logic II
```python
orders[(orders.first_name == 'Frances') & (orders.last_name == 'Palmer')]
# this will return all the rows where the first name is Frances and the last name is Palmer
orders[orders.first_name == 'Frances' & orders.last_name == 'Palmer']
# this will return an error
```
The second example will return an error because the order of operations is not clear.

The first example is the correct way to use logic to select rows.
You can also use the following logic operators:
- `==` (equal)
- `!=` (not equal)
- `<` (less than)
- `>` (greater than)
- `<=` (less than or equal to)
- `>=` (greater than or equal to)
- `&` (and)
- `|` (or)
- `~` (not)
## Selecting Rows with Logic III
```python
df[df.name.isin(['Martha Jones',
     'Rose Tyler',
     'Amy Pond'])]
```
## Setting indices
When we select a subset of a DataFrame using logic, we end up with non-consecutive indices. This is inelegant and makes it hard to use `.iloc()`. We can fix this using the method `.reset_index()`. For example:
```python
df = df.reset_index()
```
Note that the old index is added as a new column, and a new index is used.
```python
df = df.reset_index(drop=True)
```
This will drop the old index entirely.

# Lambda Functions
```python
add_two = lambda my_input: my_input + 2

print(add_two(3))
print(add_two(100))
print(add_two(-2))
```
This will output:
```
5
102
0
``` 
## Add Random
```python
import random

add_random = lambda num: num + random.randint(1,10)
print(add_random(5))
print(add_random(100))
print(add_random(-2))
```
`random.randint(a,b)` will return a random integer between a and b (inclusive).

# Modifying DataFrames
## Adding a Column I
```python
df['Quantity'] = [100, 150, 50, 35]
```
This will add a new column called 'Quantity' to the DataFrame. The first cell of this column is 100, the second is 150, the third is 50, and the fourth is 35.
| Product ID | Product Description | Cost to Manufacture | Price | Quantity |
|------------|---------------------|---------------------|-------|----------|
| 1          | 3 inch screw        | 0.50                | 0.75  | 100      |
| 2          | 2 inch nail         | 0.10                | 0.25  | 150      |
| 3          | hammer              | 3.00                | 5.50  | 50       |
| 4          | screwdriver         | 2.50                | 3.00  | 35       |
## Adding a Column II
```python
df['In Stock?'] = True
```
This will add a new column called 'In Stock?' to the DataFrame. Each cell in this column will be filled with the value True.
| Product ID | Product Description | Cost to Manufacture | Price | In Stock? |
|------------|---------------------|---------------------|-------|-----------|
| 1          | 3 inch screw        | 0.50                | 0.75  | True      |
| 2          | 2 inch nail         | 0.10                | 0.25  | True      |
| 3          | hammer              | 3.00                | 5.50  | True      |
| 4          | screwdriver         | 2.50                | 3.00  | True      |
## Adding a Column III
```python
df['Sales Tax'] = df.Price * 0.075
```
This will add a new column called 'Sales Tax' to the DataFrame. Each cell in this column will be filled with the value of 7.5% of the price of the product.
| Product ID | Product Description | Cost to Manufacture | Price | Sales Tax |
|------------|---------------------|----------------------|-------|-----------|
| 1          | 3 inch screw        | 0.50                 | 0.75  | 0.06      |
| 2          | 2 inch nail         | 0.10                 | 0.25  | 0.02      |
| 3          | hammer              | 3.00                 | 5.50  | 0.41      |
| 4          | screwdriver         | 2.50                 | 3.00  | 0.22      |
## Performing Column Operations
```python
df['Name'] = df.Name.apply(str.upper)
```
This will create a new column called 'Name' and convert all the names in the 'Name' column to uppercase.
|   Name     |         Email          |
|------------|------------------------|
| JOHN SMITH | john.smith@gmail.com  |
| JANE DOE   | jdoe@yahoo.com        |
| JOE SCHMO  | joeschmo@hotmail.com  |
## Applying a Lambda to a Column
```python
df['Email Provider'] = df.Email.apply(
    lambda x: x.split('@')[-1]
    )
```
This will create a new column called 'Email Provider' and fill it with the email provider present in the email address.

| Name       | Email                    | Email Provider |
|------------|--------------------------|----------------|
| JOHN SMITH | john.smith@gmail.com    | gmail.com      |
| Jane Doe   | jdoe@yahoo.com          | yahoo.com      |
| joe schmo  | joeschmo@hotmail.com    | hotmail.com    |
## Applying a Lambda to a Row
| Item          | Price | Is taxed? |
|---------------|-------|-----------|
| Apple         | 1.00  | No        |
| Milk          | 4.20  | No        |
| Paper Towels  | 5.00  | Yes       |
| Light Bulbs   | 3.75  | Yes       |

Suppose we want to add a new column called 'Price with Tax' to the DataFrame. We need to reference two columns to do this, so we'll use the following lambda function:
```python
df['Price with Tax'] = df.apply(lambda row:
     row['Price'] * 1.075
     if row['Is taxed?'] == 'Yes'
     else row['Price'],
     axis=1
)
```
`df.apply()` will take the lambda function and apply it to each row of the DataFrame. The result will be a new DataFrame. Don't forget to specify `axis=1`!
## Renaming Columns I
```python
df = pd.DataFrame({
    'name': ['John', 'Jane', 'Sue', 'Fred'],
    'age': [23, 29, 21, 18]
})
df.columns = ['First Name', 'Age']
```
This will edit existing column names. The columns will be renamed 'First Name' and 'Age'.
## Renaming Columns II
```python
df = pd.DataFrame({
    'name': ['John', 'Jane', 'Sue', 'Fred'],
    'age': [23, 29, 21, 18]
})
df.rename(columns={
    'name': 'First Name',
    'age': 'Age'},
    inplace=True)
```
Here we have to use the `inplace=True` argument to make the change without having to reassign the DataFrame to a new variable.

This is the preferred way to rename columns. It is more explicit and less prone to errors.

**Note:** If you misspell a column name, Pandas will not throw an error. It just won't do anything.