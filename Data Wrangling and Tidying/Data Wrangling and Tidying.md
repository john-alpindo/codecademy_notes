# Introduction: Data Wrangling and Tidying
## Data Wrangling
Data wrangling is the process of cleaning and unifying messy and complex data sets for easy access and analysis. It is the process of transforming and mapping data from its raw form into another format with the intent of making it more appropriate and valuable for a variety of downstream purposes, such as analytics.

### read_csv()
This function is used to read a csv file and convert it into a DataFrame. The file path is passed as an argument to the function.
```python
import pandas as pd
df = pd.read_csv('data.csv')
```
### head()
This function is used to display the first n rows of the DataFrame. By default, it displays the first 5 rows.
```python
df.head()
```
### shape
This attribute returns a tuple representing the dimensionality of the DataFrame. The first element of the tuple represents the number of rows and the second element represents the number of columns.
```python
df.shape
# (rows, columns)
```
## Preliminary Data Cleaning
Data cleaning is the process of preparing data for analysis by removing or modifying data that is incorrect, incomplete, irrelevant, duplicated, or improperly formatted. It is the process of identifying and correcting errors in the data to improve data quality.

### drop_duplicates()
This function is used to remove duplicate rows from the DataFrame.
```python
new_df = df.drop_duplicates()
```

### map() and lower()
The map() function is used to map values from two series having one column common. The lower() function is used to convert the strings in the column to lowercase.
```python
df.column_name = map(str.lower, df.column_name)
```
map() applies the function to all the elements of the column.

### .rename()
This function is used to rename the columns of the DataFrame.
```python
df.rename(columns = {'old_name':'new_name'}, inplace = True)
```
## Data Types

### .dtypes
This attribute returns the data type of each column in the DataFrame.
```python
restaurants.dtypes
```
```
name         object
boro         object
cuisine      object
grade        object
latitude     float64
longitude    float64
url          object
dtype: object
```
### .nunique()
This function returns the number of unique elements in the column.
```python
restaurants.nunique() 
```
```
name         25
boro          4
cuisine      15
grade         1
latitude     24
longitude    24
url          16
```
## Missing Data

### isna() and sum()
The isna() function is used to check for missing values in the DataFrame. The sum() function is used to count the number of missing values in each column.
```python
# counts the number of missing values in each column 
restaurants.isna().sum()
```
```
name          0
boro          0
cuisine       0
grade        15
latitude      0
longitude     0
url           9
```
### .where()
This function is used to replace values in the DataFrame where the condition is True.
```python
# here our .where() function replaces latitude values less than 40 with NaN values
restaurants['latitude'] = restaurants['latitude'].where(restaurants['latitude'] < 40, np.nan) 

# here our .where() function replaces longitude values greater than -70 with NaN values
restaurants['longitude'] = restaurants['longitude'].where(restaurants['longitude'] > -70, np.nan)

# .sum() counts the number of missing values in each column
restaurants.isna().sum() 
```
```
name          0
boro          0
cuisine       0
grade        15
latitude      2
longitude     2
url           9
```
Now we have 2 missing values in the latitude and longitude columns.

### Characterizing Missingness with crosstab()
The crosstab() function is 
```python
pd.crosstab(

        # tabulates the boroughs as the index
        restaurants['boro'],  

        # tabulates the number of missing values in the url column as columns
        restaurants['url'].isna(), 

        # names the rows
        rownames = ['boro'],

        # names the columns 
        colnames = ['url is na']) 
```
```
url is na   False   True
boro        
Bronx         1   1
Brooklyn       2      4
Manhattan     11      2
Queens       2    2
```

## Removing prefixes
### str.lstrip()
This function is used to remove the leading characters from the string.
```python
# .str.lstrip('https://') removes the “https://” from the left side of the string
restaurants['url'] = restaurants['url'].str.lstrip('https://') 

# .str.lstrip('www.') removes the “www.” from the left side of the string
restaurants['url'] = restaurants['url'].str.lstrip('www.')
```
## Tidy Data

|      | boro         | 2000 | 2007 |
|------|--------------|------|------|
| 0    | Bronx        | 16392| 16195|
| 1    | Brooklyn     | 16713| 17017|
| 2    | Manhattan    | 27020| 27080|
| 3    | Queens       | 16692| 16883|
| 4    | Staten Island| 15143| 14951|

### .melt()
This function is used to transform or reshape data. It is used to unpivot the DataFrame from wide format to long format.
```python
annual_wage = annual_wage.melt(

      # which column to use as identifier variables
      id_vars=["boro"], 
      
      # column name to use for “variable” names/column headers (ie. 2000 and 2007) 
      var_name=["year"], 

      # column name for the values originally in the columns 2000 and 2007
      value_name="avg_annual_wage") 

print(annual_wage)
```
|       | boro           | year | avg_annual_wage |
|-------|----------------|------|-----------------|
| 0     | Bronx          | 2000 | 16392           |
| 1     | Brooklyn       | 2000 | 16713           |
| 2     | Manhattan      | 2000 | 27020           |
| 3     | Queens         | 2000 | 16692           |
| 4     | Staten Island  | 2000 | 15143           |
| 5     | New York City  | 2000 | 23496           |
| 6     | Bronx          | 2007 | 16195           |
| 7     | Brooklyn       | 2007 | 17017           |
| ---   | ---            | ---  | ---             |
