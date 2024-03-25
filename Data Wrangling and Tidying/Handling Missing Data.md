# Introduction to Handling Missing Data
## Why It's Important to Handle Missing Data

- Missing data is a common problem in real life data. It can be caused by various reasons such as data corruption, data entry errors, data loss, etc.
- Missing data can lead to biased or inefficient analysis.
- Missing data can lead to a loss of information and can reduce the statistical power of the analysis.
- Missing data can lead to incorrect conclusions.

## Why Data Might Be Missing

### Systematic Causes
One of the most common reasons for missing data is that the data is missing systematically. This means that the data is missing for a reason. For example, a store must report the number of sales for each day. If the store is closed on Sundays, then the data for Sundays will be missing systematically.

### Privacy Concerns
Another common reason for missing data is that the data is missing due to privacy concerns. For example, a survey might ask for the respondent's email address or phone number. Some respondents might not want to provide this information, so the data will be missing.

### Information Loss
Another common reason for missing data is that the data is missing due to information loss. For example, a security camera trying to upload a video to the cloud might lose some frames due to a poor internet connection.

## Identifying and Checking for Missing Data
1. **Verify that the data was uploaded correctly.** Since most missing data is due to systematic causes, it is important to verify that the data was uploaded correctly.

2. **Try looking at smaller subsets of the data** to see if the missing data is systematic or random.

3. **Look at statistic for the entire dataset** to see if there are any missing values.

# Types of Missing Data
## How important is the missing data?
While every missing data point is important, some missing data points are more important than others. For example, if a missing data point is the only data point for a particular variable, then that missing data point is very important.
## Different Types of Missing Data
### Structurally Missing Data
This type of missing data occurs for some logical reason. This could be because the data is missing for a particular group of people, or because the data is missing for a particular time period.

| ParticipantID | AsthmaFlag | InhalerFrequency | InhalerBrand |
|---------------|------------|------------------|--------------|
| 100           | TRUE       | Twice daily      | Breathe-Rite |
| 101           | TRUE       | Once weekly      | Breathe-Rite |
| 102           | FALSE      |                  |              |
| 103           | TRUE       | Once daily       | Asthm-Away   |
| 104           | FALSE      |                  |              |

In the table above, the data for `InhalerFrequency` and `InhalerBrand` is missing for participants 102 and 104. This is structurally missing data because the data is missing for a particular group of people because they do not use an inhaler.

### Missing Completely at Random (MCAR)
This type of missing data occurs when there's no logical reason, no pattern, and no relationship between the missing data and the observed data.

For example, if a survey respondent accidentally skips a question, then the missing data is MCAR.

| ParticipantID | Walked | Steps (1,000) |
|---------------|--------|---------------|
| 25            | TRUE   | 2.1           |
| 43            | TRUE   | 15            |
| 61            | TRUE   | 6             |
| 62            | TRUE   |               |
| 78            | TRUE   | 3             |
| 84            | TRUE   |               |
| 90            | TRUE   | 0.5           |
| 102           | TRUE   | 1.5           |
| 110           | TRUE   | .01           |
| 115           | TRUE   | 4.1           |

In the table above, the data for `Steps (1,000)` is missing for participants 62 and 84. This is MCAR because there is no logical reason for the missing data. The missing data is not related to the observed data.

### Missing at Random (MAR)
The probability of missing data depends on the observed data - this is much more realistic than MCAR.

| ParticipantID | Height (cm) | Weight (kg) |
|---------------|-------------|-------------|
| 1             | 176         | 84          |
| 2             | 190         |             |
| 3             | 160         | 61          |
| 4             | 180         |             |
| 5             | 184         |             |
| 6             | 158         | 72          |
| 7             | 152         | 50          |
| 8             | 156         |             |
| 9             | 194         | 104         |
| 10            | 180         | 79          |

In the table above, the data for `Weight (kg)` is missing for participants 2, 4, 5, and 8. This is MAR because several scientific studies have show that individuals do not like to report their weight, expecially if they are overweight. But some individuals do not mind reporting their weight.

### Missing Not at Random (MNAR)
The probability of missing data depends on the unobserved data. This is the most difficult type of missing data to handle.

For example in a clinic study, the data for `Weight` is missing for participants who are overweight. But if we dive deeper, we see that the data is missing on the same day that the participants were weighed. In the clinic the scales are battery operated and the batteries were dead that day.

# Handling Missing Data with Deletion
## When is it safe to use deletion?
- it is either MAR or MCAR (missing at random or missing completely at random). However, if the percentage of missing data is high, then it is not safe to use deletion. Every dataset has a threshold for the percentage of missing data that can be deleted.
- the missing data is not important. For example, if the missing data is for a variable that is not important for the analysis, then it is safe to use deletion.

## Types of Deletion
### Listwise Deletion
Listwise deletion, also known as complete-case analysis, is a technique that removes all rows with missing data. This is the simplest and most common method of handling missing data.

| ParticipantNumber | Sex | Height (cm) | Weight (kg) | Education  |
|-------------------|-----|-------------|-------------|------------|
| 1                 | F   | 176         | 84          | Primary    |
| 2                 | M   | 190         |             | Secondary  |
| 3                 | F   | 160         | 61          |            |
| 4                 | F   | 180         | 78          | Primary    |
| 5                 | M   | 184         |             | Primary    |
| 6                 | F   | 158         | 72          |            |
| 7                 | M   | 152         | 50          | No degree  |
| 8                 | M   | 156         |             | Secondary  |
| 9                 | M   | 194         | 104         | Primary    |
| 10                | F   | 180         | 79          | No degree  |

```python
# Drop rows that have any missing data
data.dropna(inplace=True)
```
| ParticipantNumber | Sex | Height (cm) | Weight (kg) | Education |
|-------------------|-----|-------------|-------------|-----------|
| 1                 | F   | 176         | 84          | Primary   |
| 4                 | F   | 180         | 78          | Primary   |
| 7                 | M   | 152         | 50          | No degree |
| 9                 | M   | 194         | 104         | Primary   |
| 10                | F   | 180         | 79          | No degree |

### Pairwise Deletion
Pairwise deletion, as an alternative to listwise deletion, selectively removes rows with missing values only in the variables being directly analyzed. Unlike listwise deletion, it does not consider the presence of missing values in other variables and retains those rows.

ParticipantNumber | Sex | Height (cm) | Weight (kg) | Education
-----------------|-----|-------------|-------------|-----------
1                 | F   | 176         | 84          | Primary
2                 | M   | 190         |             | Secondary
3                 | F   | 160         | 61          | 
4                 | F   | 180         | 78          | Primary
5                 | M   | 184         |             | Primary
6                 | F   | 158         | 72          | 
7                 | M   | 152         | 50          | No degree
8                 | M   | 156         |             | Secondary
9                 | M   | 194         | 104         | Primary
10                | F   | 180         | 79          | No degree

Let's pretend that we are interested in the relationship between `Height` and `Education`. If we use listwise deletion, we would lose 5 rows of data. But if we use pairwise deletion, we would only lose 2 rows of data.

```python
data.dropna(subset=['Height (cm)', 'Education'], # only consider these columns
    inplace=True, # modify the DataFrame in place
    how='any') # remove rows with missing data in any of the specified columns
```

| ParticipantNumber | Sex | Height (cm) | Weight (kg) | Education |
|-------------------|-----|-------------|-------------|-----------|
| 1                 | F   | 176         | 84          | Primary   |
| 2                 | M   | 190         |             | Secondary |
| 4                 | F   | 180         | 78          | Primary   |
| 5                 | M   | 184         |             | Primary   |
| 7                 | M   | 152         | 50          | No degree |
| 8                 | M   | 156         |             | Secondary |
| 9                 | M   | 194         | 104         | Primary   |
| 10                | F   | 180         | 79          | No degree |

### Dropping Variables
We could also choose to exclude a variable altogether if it lacks sufficient data. However, it's generally not ideal to drop entire variables since having more data usually strengthens our analysis. Removing a variable should be a last resort, only done when it's missing a significant amount of data, typically at least 60%.

```python
data.drop('Weight (kg)', axis=1, inplace=True)
```
# Single Imputation
Imputation is the process of replacing missing values with substituted values. Single imputation is a method of imputation where the missing values are replaced by a single value. There are different methods of single imputation such as mean imputation, median imputation, mode imputation, and LOCF imputation.

## What is time-series data?
The data that is collected at different points in time is called time-series data. It is a sequence of observations that are recorded at regular time intervals. Time-series data is used to analyze the past data and forecast the future data.

## What can we do on MNR data?

### LOCF
LOCF stands for Last Observation Carried Forward. It is a method of imputation where the last observation is carried forward to the missing values. This method is used when the missing values are assumed to be the same as the last observed value.

| time                      | space_id | comfort   |
|---------------------------|----------|-----------|
| 2021-03-16 08:22:00+08:00 | 125      | Comfy     |
| 2021-03-16 09:37:00+08:00 | 125      | Not Comfy |
| 2021-03-18 08:11:00+08:00 | 125      | Not Comfy |
| 2021-03-18 09:48:00+08:00 | 125      | Comfy     |
| 2021-03-18 13:35:00+08:00 | 125      | Comfy     |
| 2021-03-18 15:23:00+08:00 | 125      | Comfy     |
| 2021-03-18 15:53:00+08:00 | 125      |           |
| 2021-03-18 18:58:00+08:00 | 125      | Comfy     |
| 2021-03-19 08:05:00+08:00 | 125      | Not Comfy |
| 2021-03-19 09:53:00+08:00 | 125      | Comfy     |

We can assume that the comfort level at 2021-03-18 15:53:00+08:00 is the same as the comfort level at 2021-03-18 15:23:00+08:00. This is because the last observed comfort level is Comfy.

- In `Pandas`, we can use the `ffill` method to carry forward the last observation to the missing values.
```python
df['comfort'].ffill(axis=0, inplace=True)
```
- In `NumPy`, we can use the `impyute` library to carry forward the last observation to the missing values.
```python
impyute.imputation.ts.locf(data, axis=0)
```
| time                      | space_id | comfort   |
|---------------------------|----------|-----------|
| 2021-03-16 08:22:00+08:00 | 125      | Comfy     |
| 2021-03-16 09:37:00+08:00 | 125      | Not Comfy |
| 2021-03-18 08:11:00+08:00 | 125      | Not Comfy |
| 2021-03-18 09:48:00+08:00 | 125      | Comfy     |
| 2021-03-18 13:35:00+08:00 | 125      | Comfy     |
| 2021-03-18 15:23:00+08:00 | 125      | Comfy     |
| 2021-03-18 15:53:00+08:00 | 125      | **Comfy**     |
| 2021-03-18 18:58:00+08:00 | 125      | Comfy     |
| 2021-03-19 08:05:00+08:00 | 125      | Not Comfy |
| 2021-03-19 09:53:00+08:00 | 125      | Comfy     |

### NOCB
NOCB stands for Next Observation Carried Backward. It is a method of imputation where the next observation is carried backward to the missing values. This method is used when the missing values are assumed to be the same as the next observed value.

time                      | space_id | comfort
-------------------------|----------|---------
2021-03-12 14:49:00+08:00|     2    | Comfy
2021-03-17 15:30:00+08:00|     2    | Comfy
2021-03-17 15:33:00+08:00|     2    | Comfy
2021-03-17 15:53:00+08:00|     2    | Not Comfy
2021-03-18 11:23:00+08:00|     2    | 
2021-03-18 11:38:00+08:00|     2    | Comfy
2021-03-18 12:01:00+08:00|     2    | Comfy
2021-03-18 13:49:00+08:00|     2    | Comfy
2021-03-18 14:22:00+08:00|     2    | Comfy
2021-03-18 15:24:00+08:00|     2    | Comfy
2021-03-18 15:59:00+08:00|     2    | Comfy

Since we can see that rest of the values are Comfy, we can assume that the comfort level at 2021-03-18 11:23:00+08:00 is the same as the comfort level at 2021-03-18 11:38:00+08:00.

```python
df['comfort'].bfill(axis=0, inplace=True)
```
```python
impyute.imputation.ts.nocb(data, axis=0)
```
time                      | space_id | comfort
-------------------------|----------|---------
2021-03-12 14:49:00+08:00|     2    | Comfy
2021-03-17 15:30:00+08:00|     2    | Comfy
2021-03-17 15:33:00+08:00|     2    | Comfy
2021-03-17 15:53:00+08:00|     2    | Not Comfy
2021-03-18 11:23:00+08:00|     2    | **Comfy**
2021-03-18 11:38:00+08:00|     2    | Comfy
2021-03-18 12:01:00+08:00|     2    | Comfy
2021-03-18 13:49:00+08:00|     2    | Comfy
2021-03-18 14:22:00+08:00|     2    | Comfy
2021-03-18 15:24:00+08:00|     2    | Comfy
2021-03-18 15:59:00+08:00|     2    | Comfy

### Other alternatives
#### BOCF
One such alternative is BOCF, which stands for Best Observation Carried Forward. This method is used the initial values to fill the missing values. 

For example, In this data, we are trying to study some measure of bacteria in a patient’s body after the patient receives a certain treatment.

| timestamp           | concentration |
|---------------------|---------------|
| 2021-09-01 08:00:00 | 100           |
| 2021-09-01 08:15:00 | 98            |
| 2021-09-01 08:30:00 | 97            |
| 2021-09-01 08:45:00 | 98            |
| 2021-09-01 09:00:00 | N/A           |
| 2021-09-01 09:15:00 | 96            |
| 2021-09-01 09:30:00 | 94            |
| 2021-09-01 09:45:00 | 93            |
| 2021-09-01 10:00:00 | 90            |

```python
# Isolate the first (baseline) value for our data
baseline = df['concentration'][0]

# Fill the missing values with the baseline value
df['concentration'].fillna(baseline, inplace=True)
```
#### WOCF
WOCF stands for Worst Observation Carried Forward. This method is used when the missing values are assumed to be the same as the worst observed value.

In this data, we are trying to study the pain level of a patient over the course of a therapy. We can assume that the pain level at 2021-09-05 is the same as the worst observed pain level.

| date       | pain |
|------------|------|
| 2021-09-01 | 8    |
| 2021-09-02 | 8    |
| 2021-09-03 | 9    |
| 2021-09-04 | 7    |
| 2021-09-05 | N/A  |
| 2021-09-06 | 6    |
| 2021-09-07 | 7    |
| 2021-09-08 | 5    |
| 2021-09-09 | 5    |

This is a great use case for WOCF. Since we are tracking a patient's pain level, with the goal of reducing it, we can assume that the pain level at 2021-09-05 is the same as the worst observed pain level, as this would be the most conservative approach.

```python
# Isolate the worst observed value for our data
worst_observed = df['pain'].max()

# replace the missing values with the worst observed value
df['pain'].fillna(value=worst_observed, inplace=True)
```
### What are the disadvantages of single imputation?
Single imputation methods are simple and easy to implement, but they have some disadvantages:

- Single imputation methods do not account for the uncertainty in the imputed values. The imputed values are treated as if they are known with certainty, which can lead to biased estimates and incorrect inferences.

- Single imputation methods can introduce bias into the data.

In general, single imputation can be an effective way to handle missing data, but it is important to be aware of the limitations and potential pitfalls of these methods.

# Multiple Imputation
Multiple imputation is a method of imputation where the missing values are replaced by multiple values. Multiple imputation, in particular, is used when we have missing data across multiple variables. After imputing the missing values multiple times, we can us an algorithm to combine the results into a single imputed dataset.

## When to use multiple imputation?
Multiple imputation is best for MAR data. With MAR data, there is an assumed relationship between the missing data and the observed data. Multiple imputation can help to preserve this relationship and reduce bias in the imputed data.

## How to use it
### InterativeImputer in scikit-learn
The `IterativeImputer` class in scikit-learn is a powerful tool for imputing missing values in a dataset. It uses a machine learning model to predict the missing values based on the observed data. The `IterativeImputer` class is flexible and can be used with a variety of machine learning models, such as linear regression, random forests, and support vector machines.

| X    | Y    | Z    |
|------|------|------|
| 5.4  | 18.0 | 7.6  |
| 13.8 | 27.4 | 4.6  |
| 14.7 |      | 4.2  |
| 17.6 | 18.3 |      |
|      | 49.6 | 4.7  |
| 1.1  | 48.9 | 8.5  |
| 12.9 |      | 3.5  |
| 3.4  | 13.6 |      |
|      | 16.1 | 1.8  |
| 10.2 | 42.7 | 4.7  |

```python
import numpy as np
from sklearn.experimental import enable_iterative_imputer
from sklearn.impute import IterativeImputer
import pandas as pd

# Create the dataset as a Python dictionary
d = {
    'X': [5.4,13.8,14.7,17.6,np.nan,1.1,12.9,3.4,np.nan,10.2],
    'Y': [18,27.4,np.nan,18.3,49.6,48.9,np.nan,13.6,16.1,42.7],
    'Z': [7.6,4.6,4.2,np.nan,4.7,8.5,3.5,np.nan,1.8,4.7]
}

dTest = {
    'X': [13.1, 10.8, np.nan, 9.7, 11.2],
    'Y': [18.3, np.nan, 14.1, 19.8, 17.5],
    'Z': [4.2, 3.1, 5.7,np.nan, 9.6]
}

# Create the pandas DataFrame from our dictionary
df = pd.DataFrame(data=d)
dfTest = pd.DataFrame(data=dTest)

# Create the IterativeImputer model to predict missing values
imp = IterativeImputer(max_iter=10, random_state=0)

# Fit the model to the test dataset
imp.fit(dfTest)

# Transform the model on the entire dataset
dfComplete = pd.DataFrame(np.round(imp.transform(df),1), columns=['X','Y','Z'])

print(dfComplete.head(10))
```

| X    | Y    | Z   |
|------|------|-----|
| 5.4  | 18.0 | 7.6 |
| 13.8 | 27.4 | 4.6 |
| 14.7 | **17.4** | 4.2 |
| 17.6 | 18.3 | **5.6** |
| **11.2** | 49.6 | 4.7 |
| 1.1  | 48.9 | 8.5 |
| 12.9 | **17.4** | 3.5 |
| 3.4  | 13.6 | **5.7** |
| **11.2** | 16.1 | 1.8 |
| 10.2 | 42.7 | 4.7 |
