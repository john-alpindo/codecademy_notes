# Window Functions
Window functions are a powerful tool in SQL that allow you to perform calculations across a set of rows related to the current row. This is similar to the concept of an aggregate function, but instead of collapsing the rows into a single value, the window function returns a value for each row. This is useful for calculating running totals, moving averages, and other calculations that depend on the ordering of the rows.

## Window Function Syntax
```sql
SELECT 
   month,
   change_in_followers,
   SUM(change_in_followers) OVER (
      ORDER BY month
   ) AS 'running_total'
FROM
   social_media
WHERE
   username = 'instagram';
```

| Month | Change in Followers | Running Total |
|-------|---------------------|--------------|
|   1   |        6.27         |     6.27     |
|   2   |        4.71         |    10.98     |
|   3   |        5.73         |    16.71     |
|   4   |        5.43         |    22.14     |
|   5   |        4.99         |    27.13     |
| --- | --- | --- |

The window function is defined in the `OVER` clause, which comes after the column you want to perform the calculation on. The `ORDER BY` clause inside the `OVER` clause specifies the order of the rows in the window. In this example, `SUM` is the window function, and it calculates the running total of the change_in_followers column, ordered by the month column.

## PARTITION BY
The `PARTITION BY` clause is used to break the data into separate windows based on the values in one or more columns. This is useful for calculating aggregates and other window functions on a per-group basis. For example, you can calculate the running total of change_in_followers for each username in the social_media table.

```sql
SELECT 
   username,
   month,
   change_in_followers,
   SUM(change_in_followers) OVER (
      PARTITION BY username
      ORDER BY month
   ) AS 'running_total'
FROM
    social_media;
```
| username      | month | change_in_followers | running_total_followers_change |
|---------------|-------|---------------------|--------------------------------|
| aliaabhatt    | 1     | 1.15                | 1.15                           |
| aliaabhatt    | 2     | 1.16                | 2.31                           |
| aliaabhatt    | 3     | 1.52                | 3.83                           |
| ---           | ---   | ---                 | ---                            |
| arianagrande  | 1     | 3.69                | 3.69                           |
| arianagrande  | 2     | 4.4                 | 8.09                           |
| arianagrande  | 3     | 4.93                | 13.02                          |
| ---           | ---   | ---                 | ---                            |
| codecademy    | 1     | 0.03                | 0.03                           |
| codecademy    | 2     | 0.01                | 0.04                           |
| codecademy    | 3     | 0.02                | 0.06                           |
| ---           | ---   | ---                 | ---                            |

## FIRST_VALUE and LAST_VALUE
The `FIRST_VALUE` and `LAST_VALUE` functions return the first and last value in the window, respectively. These functions are useful for finding the first and last values in a time series, or for finding the first and last values in a group.

```sql
SELECT username,
   posts,
   FIRST_VALUE (posts) OVER (
      PARTITION BY username 
      ORDER BY posts
   ) AS 'fewest_posts'
FROM social_media;
```

| username      | posts | fewest_posts |
|---------------|-------|--------------|
| aliaabhatt    | 5     | 5            |
| aliaabhatt    | 7     | 5            |
| ---           | ---   | ---          |
| aliaabhatt    | 14    | 5            |
| aliaabhatt    | 25    | 5            |
| ---           | ---   | ---          |
| codecademy    | 0     | 0            |
| codecademy    | 1     | 0            |
| ---           | ---   | ---          |
| codecademy    | 2     | 0            |
| codecademy    | 3     | 0            |

```sql
SELECT username,
   posts,
   LAST_VALUE (posts) OVER (
      PARTITION BY username 
      ORDER BY posts
      RANGE BETWEEN UNBOUNDED PRECEDING AND
      UNBOUNDED FOLLOWING
   ) AS 'fewest_posts'
FROM social_media;
```
| username      | posts | fewest_posts |
|---------------|-------|--------------|
| aliaabhatt    | 5     | 25           |
| aliaabhatt    | 7     | 25           |
| ---           | ---   | ---          |
| aliaabhatt    | 14    | 25           |
| aliaabhatt    | 25    | 25           |
| ---           | ---   | ---          |
| codecademy    | 0     | 3            |
| codecademy    | 1     | 3            |
| ---           | ---   | ---          |
| codecademy    | 2     | 3            |
| codecademy    | 3     | 3            |

The `RANGE BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING` clause is used to specify the range of rows in the window. In this example, the range is unbounded, so the window includes all rows in the partition.

## LAG
The `LAG` function returns the value of a column in the previous row of the window. 

LAG takes three arguments:
- The column you want to retrieve the value from (required).
- The number of rows before the current row (optional, default is 1).
- The default value to return if the offset row does not exist (optional).

```sql
SELECT
   artist,
   week,
   streams_millions,
   streams_millions - LAG(streams_millions, 1, streams_millions) OVER (
      PARTITION BY artist
      ORDER BY week 
   ) streams_millions_change,
   chart_position,
   LAG(chart_position, 1, chart_position) OVER (
      PARTITION BY artist
      ORDER BY week 
   ) - chart_position AS position_change
FROM streams
WHERE artist = 'Lady Gaga';
```

| artist    | week | streams_millions | streams_millions_change | chart_position | position_change |
|-----------|------|------------------|-------------------------|----------------|-----------------|
| Lady Gaga | 1    | 15.4             | 0.0                     | 106            | 0               |
| Lady Gaga | 2    | 15.2             | -0.2                    | 112            | -6              |
| Lady Gaga | 3    | 16.6             | 1.4                     | 98             | 14              |
| Lady Gaga | 4    | 21.0             | 4.4                     | 75             | 23              |
| Lady Gaga | 5    | 64.0             | 43.0                    | 10             | 65              |
| Lady Gaga | 6    | 36.0             | -28.0                   | 24             | -14             |
| Lady Gaga | 7    | 30.5             | -5.5                    | 36             | -12             |
| Lady Gaga | 8    | 27.0             | -3.5                    | 47             | -11             |

## LEAD
The `LEAD` function returns the value of a future row in the window instead of a past row like `LAG`.

```sql
SELECT
   artist,
   week,
   streams_millions,
   LEAD(streams_millions, 1) OVER (
      PARTITION BY artist
      ORDER BY week
   ) - streams_millions AS 'streams_millions_change',
   chart_position,
   chart_position - LEAD(chart_position, 1) OVER (
      PARTITION BY artist
      ORDER BY week
   ) AS 'chart_position_change'
FROM
   streams;
```

## ROW_NUMBER
Used to assign a unique number to each row in the window. This is useful for ranking rows, selecting the top N rows, and other operations that require a unique identifier for each row.

```sql
SELECT 
   ROW_NUMBER() OVER (
      ORDER BY streams_millions
   ) AS 'row_num', 
   artist, 
   week,
   streams_millions
FROM
   streams;
```
`row_num = 1` is the row with the smallest streams_millions value, `row_num = 2` is the row with the second smallest streams_millions value, and so on.

When the row values are the same, the `ROW_NUMBER` function will assign a unique number to each row, but the order of the rows with the same value is not guaranteed.

## RANK
The `RANK` function is similar to `ROW_NUMBER`, but it assigns the same rank to rows with the same value. For example, if two rows have the same streams_millions value, they will both have the same rank.

```sql
SELECT 
   RANK() OVER (
      PARTITION BY week
      ORDER BY streams_millions DESC
   ) AS 'rank', 
   artist, 
   week,
   streams_millions
FROM
   streams;
```
## NTILE
Used to divide the rows in the window into a specified number of buckets, or tiles. This is useful for dividing the rows into equal-sized groups, for example, to calculate quartiles or percentiles.

```sql
SELECT 
   NTILE(5) OVER (
      ORDER BY streams_millions DESC
   ) AS 'weekly_streams_group', 
   artist, 
   week,
   streams_millions
FROM
   streams;
```