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