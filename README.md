![forthebadge](https://forthebadge.com/images/badges/built-with-love.svg)
[![forthebadge](https://forthebadge.com/images/badges/open-source.svg)](https://forthebadge.com)


<p align="center">
    <img src="images\poster.gif" alt="healthcare-analytics">
</p>
<h2 align="center"> 🏥 Health Analytics Mini Case Study </h2>

<div align="center">

  [![Status](https://img.shields.io/badge/status-active-success.svg)]()
  [![Ask Me Anything !](https://img.shields.io/badge/Ask%20me-anything-1abc9c.svg)]() 
  [![Open Source? Yes!](https://badgen.net/badge/Open%20Source%20%3F/Yes%21/blue?icon=github)]()
  [![License](https://img.shields.io/badge/license-MIT-blue.svg)]()

</div>

<p align="center"> This is a health analytics mini case study from the <a href="https://www.datawithdanny.com/">Serious SQL</a> course by Danny Ma. The general manager at an healthcare firm has asked us a few questions that they'll like answered.
    <br> 
</p>

### ✨ Contents 

-  [🕵️ About ](#about)
-  [💻 Business Questions](#business-questions) 
-  [✍️ Contribution](#contribution)

------



#### 🕵️ About <a name='about'></a>

<p>
General Manager of Analytics at Health Co. has requested your assitance to  answer some business questions he has which he need to address in today's board meeting by analysing data provided to you about their users. 
</p>


#### 💻 Business Questions <a name='business-questions'></a>

|Question 1 | How many unique users exists in logs dataset? |
|--|--|


<details>
<summary><i>SQL Query</i></summary>

```sql
SELECT
  COUNT(DISTINCT id) as unique_users_count
FROM
  health.user_logs;
```

</details>

| unique_users_count | 
|--|
| 554 |

<br>

<details>
<summary><strong>Create TEMP Table for Q-2 to Q-8</strong></summary>

```sql
DROP TABLE IF EXISTS user_measure_count;
CREATE TEMP TABLE user_measure_count AS (
  SELECT
    id,
    COUNT(*) AS measure_count, 
    COUNT( DISTINCT measure) AS unique_measure , 
    SUM( COUNT( DISTINCT measure) ) as total_unique_measure
  FROM
    health.user_logs
  GROUP BY
    1
);
```

</details>
<br>

|Question 2 | How many total measurements do we have per user on average? |
|--|--|

<details>
<summary><i>SQL Query</i></summary>

```sql
SELECT
  ROUND(AVG(measure_count)) AS avg_measurement_per_user
FROM
  user_measure_count;
```

</details>


| avg_measurement_per_user | 
|--|
| 79 |


|Question 3 | What about the median number of measurements per user? |
|--|--|

<details>
<summary><i>SQL Query</i></summary>

```sql
SELECT
  PERCENTILE_CONT(0.5) WITHIN GROUP (
    ORDER BY
      measure_count
  ) AS median_value
FROM
  user_measure_count;
```

</details>

| median_value | 
|--|
| 2 |

|Question 4 | How many users have 3 or more measurements? |
|--|--|

<details>
<summary><i>SQL Query</i></summary>

```sql
SELECT
  COUNT(*) AS more_than_equal_3_count
FROM
  user_measure_count
WHERE
  measure_count >= 3;
```

</details>

| more_than_equal_3_count | 
|--|
| 209 | 

|Question 5 | How many users have 1,000 or more measurements? |
|--|--|

<details>
<summary><i>SQL Query</i></summary>

```sql
SELECT
  COUNT(*) AS more_than_equal_1000_count
FROM
  user_measure_count
WHERE
  measure_count >= 1000 ;
```


</details>

| more_than_equal_1000_count | 
|--|
| 5 | 

|Question 6 |  number and percentage of the active user base who have logged blood glucose measurements? |
|--|--|


<details>
<summary><i>SQL Query</i></summary>

```sql
WITH group_by_measure AS (
  SELECT
    DISTINCT id,
    measure
  FROM
    health.user_logs
)
SELECT
  measure,
  COUNT(*) AS frequency,
  ROUND (
    100 * COUNT(*) :: NUMERIC / SUM(COUNT(*)) OVER(),
    2
  ) AS percent
FROM
  group_by_measure
GROUP BY
  measure
```

</details>

| measure | frequency | percent | 
| -- | -- | -- | 
| blood_pressure| 123 | 15.22 | 
| **blood_glucose** | **325** | **40.22** | 
| weight        | 360 | 44.55 | 


|Question 7 | number and percentage of the active user base who have at least 2 types of measurements? |
|--|--|


<details>
<summary><i>SQL Query</i></summary>

```sql

SELECT
  '2_or_more' AS unique_measure_count , 
  COUNT(*) AS frequency , 
  ROUND(100 * COUNT(*) :: NUMERIC / AVG(total_uninque_measure), 2) AS percent 
FROM
  user_measure_count
WHERE 
  unique_measure >= 2
```

</details>


| unique_measure_count | frequency | percent | 
| -- | -- | -- | 
| 2_or_more | 204 | 25.25 | 

|Question 8 | number and percentage of the active user base who have all 3 measures - blood glucose, weight and blood pressure? |
|--|--|




<details>
<summary><i>SQL Query</i></summary>

```sql

SELECT
  'All_3' AS unique_measure_count , 
  COUNT(*) AS frequency , 
  ROUND(100 * COUNT(*) :: NUMERIC / AVG(total_uninque_measure), 2) AS percent 
FROM
  user_measure_count
WHERE 
  unique_measure = 3;
```

</details>


| unique_measure_count | frequency | percent | 
| -- | -- | -- | 
| All_3 | 50 | 6.19 | 


|Question 9 | For users that have blood pressure measurements, What is the median systolic/diastolic blood pressure values? |
|--|--|


<details>
<summary><i>SQL Query</i></summary>

```sql
SELECT
  PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY systolic) AS median_systolic,
  PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY diastolic) AS median_diastolic
FROM health.user_logs
WHERE measure = 'blood_pressure';
```

</details>

| median_systolic | median_diastolic | 
| -- | -- | 
| 126 | 79 | 


#### ✍️ Contribution <a name='contribution'></a>

Assume you are General Manager, What extra queries you might have ? Run your imagination and increase your business, don't worry I am here to analyse your data. 

If you find any mistake or want to add some more questions, feel free to contribute . 

#### [Thank You]() 

If you like project show your support by dropping a ⭐



