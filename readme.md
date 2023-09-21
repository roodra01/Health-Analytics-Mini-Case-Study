
# 🩺 Health Analytics Mini Case Study 
We’ve just received an urgent request from the General Manager of Analytics at Health Co requesting our assistance with their analysis of the health.user_logs dataset.

[Here](https://docs.google.com/spreadsheets/d/1crZX9cPkl1fnjHvvnhfOGHPwX3uG4h9n2L89J97bkgU/edit?usp=sharing) is the dataset we are working with.

## Questions and Solutions

### 1. How many unique users exist in the logs dataset?
```sql   
SELECT COUNT(DISTINCT id)
FROM health.user_logs;
```
#### Explanation:
A simple select statment that counts the unqiue id's in user_log.

#### Answer:
![image](https://github.com/roodra01/Health-Analytics-Mini-Case-Study/assets/129188359/9ae29fb0-dc48-4597-80ed-91d98221abe8)

### 2. How many total measurements do we have per user on average?
```sql 

WITH user_measure_cout AS(
  SELECT
    id,
    COUNT(*) AS measure_count,
    COUNT(DISTINCT measure) as unique_measures
  FROM health.user_logs
  GROUP BY id )

SELECT
  ROUND(AVG(measure_count), 2)
FROM user_measure_cout;
```
#### Explanation:
First I created a CTE that grouped the id's to show how many times they show and how many times unique columns each id has. Then I just took the average of the numbers of times the ids showed up (avg of measure_count).

#### Answer:
![image](https://github.com/roodra01/Health-Analytics-Mini-Case-Study/assets/129188359/ad7e015c-11d0-45b5-a9be-7b5ae2137873)

### 3. What about the median number of measurements per user?
```sql 
WITH user_measure_cout AS(
SELECT
    id,
    COUNT(*) AS measure_count,
    COUNT(DISTINCT measure) as unique_measures
  FROM health.user_logs
  GROUP BY id )

SELECT
  PERCENTILE_CONT(measure_count, 0.5 ) over() AS median_value
FROM user_measure_cout
LIMIT 1;
```
#### Explanation:
I used ```PERCENTILE_CONT``` to find the median of the measurements per users.

#### Answer:
![image](https://github.com/roodra01/Health-Analytics-Mini-Case-Study/assets/129188359/123aeda5-b01b-4654-80d8-982474877707)

### 4. How many users have 3 or more measurements?
```sql 
WITH user_measure_cout AS(
SELECT
    id,
    COUNT(*) AS measure_count,
    COUNT(DISTINCT measure) as unique_measures
  FROM health.user_logs
  GROUP BY id )

SELECT
  COUNT(*)
FROM user_measure_cout
WHERE measure_count >= 3;
```
#### Explanation:
I counted the number of instances that an ID had three or more apprearences by using ```WHERE``` to show only cases where measure_count was greater than or equal to 3.
#### Answer:
![image](https://github.com/roodra01/Health-Analytics-Mini-Case-Study/assets/129188359/c5aa80fd-e704-4b13-8361-eb871aac714b)



### 5. How many users have 1,000 or more measurements?
```sql
WITH user_measure_cout AS(
SELECT
    id,
    COUNT(*) AS measure_count,
    COUNT(DISTINCT measure) as unique_measures
  FROM health.user_logs
  GROUP BY id )

SELECT
  COUNT(*)
FROM user_measure_cout
WHERE measure_count >= 1000;
```
#### Explanation:
Pretty much the same as the last question. Replace 3 with 1000.
#### Answer:
![image](https://github.com/roodra01/Health-Analytics-Mini-Case-Study/assets/129188359/c2083d88-eaf1-499f-b16d-c64f14b71a5e)


### 6. Have logged blood glucose measurements?
``` sql
SELECT
COUNT(DISTINCT ID)
FROM health.user_logs
WHERE measure = 'blood_glucose';
```
#### Explanation:
For this one we are looking for all the unique ids that have measure has blood_glucose. A simple ```WHERE``` helps us here.

#### Answer:
![image](https://github.com/roodra01/Health-Analytics-Mini-Case-Study/assets/129188359/1422e7d0-1757-46d9-a860-12b95b89c688)


### 7. Have at least 2 types of measurements?
``` SQL
WITH user_measure_cout AS(
SELECT
    id,
    COUNT(*) AS measure_count,
    COUNT(DISTINCT measure) as unique_measures
  FROM health.user_logs
  GROUP BY id )

SELECT
  COUNT(*)
FROM user_measure_cout
WHERE unique_measures >= 2;
```
#### Explanation:
Here we want unique_measures greater than or equal to 2 becuase if the measures are not unique we would include results that also include the same two type of measurements repeating.

#### Answer:
![image](https://github.com/roodra01/Health-Analytics-Mini-Case-Study/assets/129188359/69acbfae-552f-4fa0-a645-a6fb2ce804ad)


### 8. Have all 3 measures - blood glucose, weight and blood pressure?
``` SQL
WITH user_measure_cout AS(
SELECT
    id,
    COUNT(*) AS measure_count,
    COUNT(DISTINCT measure) as unique_measures
  FROM health.user_logs
  GROUP BY id )

SELECT
  COUNT(*)
FROM user_measure_cout
WHERE unique_measures = 3;
```

#### Explanation:
This one is very similar to the last problem. They're essentially the same except unique_measures must be 3 here.

#### Answer:
![image](https://github.com/roodra01/Health-Analytics-Mini-Case-Study/assets/129188359/33a312d2-02d2-4f56-9229-f48fc5c52d94)


### 9.  What is the median systolic/diastolic blood pressure values?
``` SQL
SELECT
PERCENTILE_CONT(systolic, 0.5 ) over() AS median_systolic,
PERCENTILE_CONT(diastolic, 0.5 ) over() AS median_diastolic,
FROM health.user_logs
WHERE measure= 'blood_pressure'
LIMIT 1;
```
#### Explanation: 
We are just looking for the medians of the systolic and diasolic columns here when the measure is blood_pressure. 

#### Answer:
![image](https://github.com/roodra01/Health-Analytics-Mini-Case-Study/assets/129188359/8e2ff767-0d82-47f8-9751-2d1f64a97f22)

