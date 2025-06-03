# SQL Questions

## Levels
- 🟢 [Easy](https://github.com/PhilZambri/DataLemur-Interview-Questions/blob/main/SQL.md#-easy)
- 🟠 [Medium](https://github.com/PhilZambri/DataLemur-Interview-Questions/blob/main/SQL.md#-medium)
- 🔴 [Hard](https://github.com/PhilZambri/DataLemur-Interview-Questions/blob/main/SQL.md#-hard)

***

## 🟢 Easy

### 📌 [Twitter | Easy | Histogram of Tweets](https://datalemur.com/questions/sql-histogram-tweets)

Assume you're given a table Twitter tweet data, write a query to obtain a histogram of tweets posted per user in 2022. Output the tweet count per user as the bucket and the number of Twitter users who fall into that bucket.

```sql
WITH TweetsbyUserId AS (
  SELECT user_id, COUNT(msg)
  FROM tweets
  WHERE EXTRACT(YEAR FROM tweet_date) = 2022
  GROUP BY user_id
)

SELECT count AS tweet_bucket, COUNT(*) AS num_users
FROM TweetsbyUserId
GROUP BY count;
```

![image](https://github.com/user-attachments/assets/40ac45ee-1129-4126-bd90-57fb1544f617)

***

### 📌 [LinkedIn | Easy | Matching Skills](https://datalemur.com/questions/matching-skills)

Given a table of candidates and their skills, you're tasked with finding the candidates best suited for an open Data Science job. You want to find candidates who are proficient in Python, Tableau, and PostgreSQL.
Write a query to list the candidates who possess all of the required skills for the job. Sort the output by candidate ID in ascending order.

```sql
SELECT candidate_id 
FROM candidates
WHERE skill IN ('Python', 'Tableau', 'PostgreSQL')
GROUP BY candidate_id
HAVING COUNT(skill) = 3;
ORDER BY candidate_id;
```
If there were duplicates:

```sql
SELECT candidate_id 
FROM candidates
WHERE skill IN ('Python', 'Tableau', 'PostgreSQL')
GROUP BY candidate_id
HAVING COUNT(DISTINCT skill) = 3
ORDER BY candidate_id;
```

![image](https://github.com/user-attachments/assets/72f0404f-a7ee-4f77-9a5c-deab4884c8aa)

***

## 🟠 Medium


## 🔴 Hard
