# Diagnose the Data
For Data to be Tidy, it must have:
- Each variable as a separate column.
- Each row as a separate observation.

For example, consider the following table:
| Account     | Checkings | Savings |
|-------------|-----------|---------|
| “12456543”  |    8500   |   8900  |
| “12283942”  |    6410   |   8020  |
| “12839485”  |   78000   |  92000  |
| ---         |    ---    |   ---   |

We want to transform this table into a tidy table.

| Account   | Account Type | Amount |
|-----------|--------------|--------|
| 12456543  | Checking     |  8500  |
| 12456543  | Savings      |  8900  |
| 12283942  | Checking     |  6410  |
| 12283942  | Savings      |  8020  |
| ---       | ---          |  ---   |

Pandas functions often used to diagnose the data:
- `.head()` display the first 5 rows of the dataframe.
- `.info()` display a summary of the dataframe.
- `.describe()` display the summary statistics of the dataframe.
- `.columns` display the column names of the dataframe.
- `.value_counts()` display the frequency of each unique value in the column.

# Dealing with Multiple Files
When dealing with multiple files, we can use the `glob` library to match file patterns. For example, to match all `.csv` files in the current directory, we can use the following code:
```python
import glob

files = glob.glob('*.csv')

df_list = []
for filename in files:
    data = pd.read_csv(filename)
    df_list.append(data)

df = pd.concat(df_list)

print(files)
```

# Reshaping Data
| Account     | Checkings | Savings |
|-------------|-----------|---------|
| “12456543”  |    8500   |   8900  |
| “12283942”  |    6410   |   8020  |
| “12839485”  |   78000   |  92000  |
| ---         |    ---    |   ---   |
Use the `melt` function to reshape the data:
```python
df_melted = pd.melt(frame=df, id_vars='Account', value_vars=['Checkings', 'Savings'], var_name='Account Type', value_name='Amount')
```
The resulting dataframe will be:
| Account   | Account Type | Amount |
|-----------|--------------|--------|
| 12456543  | Checking     |  8500  |
| 12456543  | Savings      |  8900  |
| 12283942  | Checking     |  6410  |
| 12283942  | Savings      |  8020  |
| ---       | ---          |  ---   |

# Dealing with Duplicates
To check for duplicates in a dataframe, we can use the `duplicated` function:
| Item   | Price  | Calories |
|--------|--------|----------|
| Banana | $1     | 105      |
| Apple  | $0.75  | 95       |
| Apple  | $0.75  | 95       |
| Peach  | $3     | 55       |
| Peach  | $4     | 55       |
```python
fruits.duplicated()
```
| id | value |
|----|-------|
| 0  | False |
| 1  | False |
| 2  | **True**  |
| 3  | False |
| 4  | False |

Use the `drop_duplicates` function to remove duplicates:
```python
fruits.drop_duplicates(inplace=True)
```
The two peach rows remain, but the duplicate apple row is removed. To remove duplicates based on a specific column, use the `subset` parameter:
```python
fruits.drop_duplicates(subset=['item'], inplace=True)
```

# Splitting by Index
| id   | birthday  |
|------|-----------|
| 1011 | "12241989"|
| 1112 | "10311966"|
| 1113 | "01052011"|

```python
# Create the 'month' column
df['month'] = df.birthday.str[0:2]

# Create the 'day' column
df['day'] = df.birthday.str[2:4]

# Create the 'year' column
df['year'] = df.birthday.str[4:]
```
| id   | birthday   | month | day | year |
|------|------------|-------|-----|------|
| 1011 | “12241989” | “12”  | “24”| “1989”|
| 1112 | “10311966” | “10”  | “31”| “1966”|
| 1113 | “01052011” | “01”  | “05”| “2011”|

# Splitting by Characters
| id   | type            |
|------|-----------------|
| 1011 | "user_Kenya"    |
| 1112 | "admin_US"      |
| 1113 | "moderator_UK"  |

```python
# Split the string and save it as `string_split`
string_split = df['type'].str.split('_')
 
# Create the 'usertype' column
df['usertype'] = string_split.str.get(0)
 
# Create the 'country' column
df['country'] = string_split.str.get(1)
```
| id   | type            | country | usertype  |
|------|-----------------|---------|-----------|
| 1011 | "user_Kenya"    | "Kenya" | "user"    |
| 1112 | "admin_US"      | "US"    | "admin"   |
| 1113 | "moderator_UK"  | "UK"    | "moderator" |

# Looking at Types
| Item        | Price  | Calories |
|-------------|--------|----------|
| Banana      | $1     | 105      |
| Apple       | $0.75  | 95       |
| Peach       | $3     | 55       |
| Clementine  | $2.5   | 35       |

```python
print(df.types)
```
```
item        object
price       object
calories     int64
dtype: object
```

# String Parsing I
| Item        | Price | Calories |
|-------------|-------|----------|
| Banana      | $1    | 105      |
| Apple       | $0.75 | 95       |
| Peach       | $3    | 55       |
| Peach       | $4    | 55       |
| Clementine  | $2.5  | 35       |

```python
# remove the dollar sign
fruit.price = fruit['price'].replace('[\$,]', '', regex=True)
# convert the price column to numeric
fruit.price = pd.to_numeric(fruit.price)
```

| Item        | Price | Calories |
|-------------|-------|----------|
| Banana      | 1     | 105      |
| Apple       | 0.75  | 95       |
| Peach       | 3     | 55       |
| Peach       | 4     | 55       |
| Clementine | 2.5   | 35       |

# String Parsing II
| date      | exerciseDescription   |
|-----------|-----------------------|
| 10/18/2018| Lunges - 30 reps      |
| 10/18/2018| Squats - 20 reps      |
| 10/18/2018| Deadlifts - 25 reps   |
| ...       | ...                   |
```python
split_df = df['exerciseDescription'].str.split('(\d+)', expand=True)
```
|           |  0          |  1   |  2     |
|-----------|-------------|------|--------|
| 0         | “lunges -“ | “30” | “reps” |
| 1         | “squats -“ | “20” | “reps” |
| 2         | “deadlifts -“ | “25” | “reps” |
| ...       | ...         | ...  | ...    |

```python
df.reps = pd.to_numeric(split_df[1])
df.exercise = split_df[0].replace('[\- ]', '', regex=True)
```

| date       | exerciseDescription  | reps | exercise   |
|------------|-----------------------|------|------------|
| 10/18/2018 | "lunges - 30 reps"      | 30   | "lunges"     |
| 10/18/2018 | "squats - 20 reps"      | 20   | "squats"     |
| 10/18/2018 | "deadlifts - 25 reps"   | 25   | "deadlifts"  |
| ...        | ...                   | ...  | ...        |

# Missing Values
| day   | bill  | tip | num_guests |
|-------|-------|-----|------------|
| "Mon" | 10.1  | 1   | 1          |
| "Mon" | 20.75 | 5.5 | 2          |
| "Tue" | 19.95 | 5.5 | NaN        |
| "Wed" | 44.10 | 15  | 3          |
| "Wed" | NaN   | 1   | 1          |

Not all functions handle missing values the same way. For example, the `mean` function will ignore missing values, while the `count` function will include them. 

## Method 1: drop all rows with missing values
```python
bill_df = bill_df.dropna()
```
| day   | bill | tip | num_guests |
|-------|------|-----|------------|
| "Mon"   | 10.1 | 1   | 1          |
| "Mon"   | 20.75| 5.5 | 2          |
| "Wed"   | 44.10| 15  | 3          |

If we wanted to remove every row with a `NaN` value in the `num_guests` column.
```python
bill_df = bill_df.dropna(subset=['num_guests'])
```
## Method 2: fill missing values with aggregate values
```python
bill_df = bill_df.fillna(value={'bill': bill_df['bill'].mean(),
    "num_guests": bill_df['num_guests'].mean()})
```
| day   | bill   | tip   | num_guests |
|-------|--------|-------|------------|
| "Mon"   | 10.1   | 1     | 1          |
| "Mon"   | 20.75  | 5.5   | 2          |
| "Tue"   | 19.95  | 5.5   | 1.75       |
| "Wed"   | 44.10  | 15    | 3          |
| "Wed"   | 23.725 | 1     | 1          |
