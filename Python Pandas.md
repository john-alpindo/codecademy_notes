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

# Aggregates in Pandas
In Pandas, an **aggregate** function is a function where the values of multiple rows are grouped together as input on certain criteria to form a single value of more significant meaning or measurement such as a set, a bag or a list.

## Calculating Column Statistics
The general syntax for these calculations is:
```python
df.column_name.command()
```
The following commands are useful for calculating simple statistics:

| Command | Description                           |
|---------|---------------------------------------|
| `mean`    | Average of all values in column       |
| `std`     | Standard deviation                    |
| `median`  | Median                                |
| `max`     | Maximum value in column               |
| `min`     | Minimum value in column               |
| `count`   | Number of values in column            |
| `nunique` | Number of unique values in column     |
| `unique`  | List of unique values in column       |

## Calculating Aggregate Functions I
The general syntax for these calculations is:

```python
df.groupby('column1').column2.measurement()
```

| Student | Assignment Name | Grade |
| ------- | --------------- | ----- |
| Amy     | Assignment 1    | 75    |
| Amy     | Assignment 2    | 35    |
| Bob     | Assignment 1    | 99    |
| Bob     | Assignment 2    | 35    |
| ...     | ...             | ...   |

```python
grades = df.groupby('Student').Grade.mean()
```
This will return a `Series` with the mean of the grades each student got in the column 'Grade'.

| Student | Grade |
|---------|-------|
| Amy     | 80    |
| Bob     | 90    |
| Chris   | 75    |
| ...     | ...   |

## Calculating Aggregate Functions II
### .reset_index()
The `.groupby()` is powerful in that it allows us to calculate aggregate statistics. However, the result of this calculation is a `Series` object, not a `DataFrame` object. If we want to work with the data as a `DataFrame`, we can use the `reset_index()` method. This will transform our `Series` into a `DataFrame` and move the indices into their own column.
```python
df.groupby('column1').column2.measurement().reset_index()
```
| id | tea              | category | caffeine | price |
|----|------------------|----------|----------|-------|
| 0  | earl grey        | black    | 38       | 3     |
| 1  | english breakfast| black    | 41       | 3     |
| 2  | irish breakfast  | black    | 37       | 2.5   |
| 3  | jasmine          | green    | 23       | 4.5   |
| 4  | matcha           | green    | 48       | 5     |
| 5  | camomile         | herbal   | 0        | 3     |
```python
teas_counts = teas.groupby('category').id.count().reset_index()
teas_counts = teas_counts.rename(columns={"id": "counts"})
```
| | category | counts |
|-|----------|--------|
|0| black    | 3      |
|1| green    | 2      |
|2| herbal   | 1      |
|3| oolong   | 1      |
|...| ...    | ...    |

## Calculating Aggregate Functions III
If we want to perform more complex calculations, we can use the `.apply()` method to perform custom calculations. This method will apply a function to the entire group and return a single row per group.

| id    | name          | wage | category |
|-------|---------------|------|----------|
| 10131 | Sarah Carney  | 39   | product  |
| 14189 | Heather Carey | 17   | design   |
| 15004 | Gary Mercado  | 33   | marketing|
| 11204 | Cora Copaz    | 27   | design   |
| ...   | ...           | ...  | ...      |

```python
# np.percentile can calculate any percentile over an array of values
high_earners = df.groupby('category').wage
    .apply(lambda x: np.percentile(x, 75))
    .reset_index()
```
| | category | wage |
|-|----------|------|
|0| design   | 23   |
|1| marketing| 35   |
|2| product  | 48   |
|...| ...    | ...  |

## Calculating Aggregate Functions IV
To group more than one column, we can pass a **list** to the `.groupby()` method. The order of the columns passed as a list matters. The first column in the list will be the level of the new DataFrame's index. 

| Location      | Date      | Day of Week | Total Sales |
|---------------|-----------|-------------|-------------|
| West Village  | February 1| W           | 400         |
| West Village  | February 2| Th          | 450         |
| Chelsea       | February 1| W           | 375         |
| Chelsea       | February 2| Th          | 390         |

```python
df.groupby(['Location', 'Day of Week'])['Total Sales'].mean().reset_index()
```

| Location      | Day of Week | Total Sales |
|---------------|-------------|-------------|
| Chelsea       | M           | 402.50      |
| Chelsea       | Tu          | 422.75      |
| Chelsea       | W           | 452.00      |
| ...           | ...         | ...         |
| West Village  | M           | 390.00      |
| West Village  | Tu          | 400.00      |
| ...           | ...         | ...         |

## Pivot Tables
The general syntax for a pivot table is:
```python
df.pivot(columns='ColumnToPivot',
         index='ColumnToBeRows',
         values='ColumnToBeValues')
```

| Location      | Day of Week | Total Sales |
|---------------|-------------|-------------|
| Chelsea       | M           | 300         |
| Chelsea       | Tu          | 310         |
| Chelsea       | W           | 375         |
| Chelsea       | Th          | 390         |
| ...           | ...         | ...         |
| West Village  | Th          | 450         |
| West Village  | F           | 390         |
| West Village  | Sa          | 250         |
| ...           | ...         | ...         |

``` python
# First use the groupby statement:
unpivoted = df.groupby(['Location', 'Day of Week'])['Total Sales'].mean().reset_index()
# Now pivot the table
pivoted = unpivoted.pivot(
    columns='Day of Week',
    index='Location',
    values='Total Sales').reset_index()
```
**Note:** The `.reset_index()` method is used to move the indices into their own column.

| Location      | M   | Tu  | W   | Th  | F   | Sa  | Su  |
|---------------|-----|-----|-----|-----|-----|-----|-----|
| Chelsea       | 300 | 310 | 375 | 390 | 300 | 150 | 175 |
| West Village  | 300 | 310 | 400 | 450 | 390 | 250 | 200 |
| ...           | ... | ... | ... | ... | ... | ... | ... |