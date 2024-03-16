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