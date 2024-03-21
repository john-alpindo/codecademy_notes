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

# Introduction to Regular Expressions
Regular Expressions is a sequence of characters that define a search pattern. Usually this pattern is used by string searching algorithms for "find" or "find and replace" operations on strings, or for input validation.

## Literals
Literals are the simplest form of a regular expression. They will simply match the exact text. For example, the regular expression `python` will match the string `python` and not `pythons` or `Python`.

## Alternation `|`
The `|` character is used to define alternatives. For example, the regular expression `python|java` will match the string `python` or `java`.

## Character Sets `[]`
Character sets are used to match only one out of several characters. For example, the regular expression `[ae]` will match `a` or `e`.

## Wildcard `.`
The `.` character is used to match any character except a newline. For example, the regular expression `py.` will match `pyt`, `pyc`, `pyo`, `py!`, etc.

Let's saw we want to match any 9-letter word. We can use the regular expression `.........`.

### escape character `\`
The `\` character is used to escape special characters. For example, the regular expression `.` will match any character except a newline, but the regular expression `\.` will match a period.

## Ranges `[-]`
Ranges are used to match a single character out of a range of characters. For example, the regular expression `[a-z]` will match any lowercase letter.

**Note:** Just like character sets, within any characer set `[]` we only match one character.

## Shorthand Character Classes
Shorthand character classes are used to match common sets of characters. 
- `\w` matches any word character (equivalent to `[a-zA-Z0-9_]`)
- `\d` matches any digit (equivalent to `[0-9]`)
- `\s` matches any whitespace character (equivalent to `[\t\n\r\f\v]`)

For example, the regular expression `\d\s\w\w\w\w\w\w\w` will match a digit, followed by a whitespace character, followed by 7 word characters. Thus, the regular expression will match `3 monkeys`.

**Note:** The uppercase versions of these shorthand character classes are used to match the opposite of the lowercase versions.
- `\W` matches any non-word character (equivalent to `[^a-zA-Z0-9_]`)
- `\D` matches any non-digit (equivalent to `[^0-9]`)
- `\S` matches any non-whitespace character (equivalent to `[^\t\n\r\f\v]`)

## Grouping `()`
Grouping is used to define subexpressions within a regular expression.

For example, when using alternation, the regular expression `I love (cats|dogs)` will match the strings `I love cats` and `I love dogs`.

## Quantifiers `{}`
Quantifiers are used to define the quantity of the character or expression to match.

- `\w{3}` matches any three word characters
- `\w{1,3}` matches between one and three word characters

The regex `roa{3}r` will match the characters ro followed by 3 as, and then the character r, such as in the string roaaar.

## Quantifiers - Optional `?`
The `?` character is used to match the preceding character 0 or 1 times. For example, the regular expression `colou?r` will match `color` and `colour`.

The regex `The monkey ate a (rotten )?banana` will match both `The monkey ate a rotten banana` and `The monkey ate a banana`.

## Quantifiers - Zero or More `*`
The `*` character is used to match the preceding character 0 or more times. For example, the regular expression `go*gle` will match `ggle`, `gogle`, `google`, `gooogle`, etc.

## Quantifiers - One or More `+`
The `+` character is used to match the preceding character 1 or more times. For example, the regular expression `go+gle` will match `gogle`, `google`, `gooogle`, etc.

## Anchors `^` and `$`
The `^` character is used to match the start and the `$` character is used to match the end of a string.

For example, the regular expression `^Monkeys: my mortal enemy$` will match the string `Monkeys: my mortal enemy` and not `I love Monkeys: my mortal enemy`.

The `^` ensures that the matched string starts with `Monkeys` and the `$` ensures that the matched string ends with `enemy`.

## Resources
### RegExr Regular Expression Builder
[RegExr](https://regexr.com/) is an online tool to learn, build, and test regular expressions.