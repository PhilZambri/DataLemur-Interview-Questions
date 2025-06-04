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

### 📌 [Facebook | Easy | Page With No Likes](https://datalemur.com/questions/sql-page-with-no-likes)

Assume you're given two tables containing data about Facebook Pages and their respective likes (as in "Like a Facebook Page").
Write a query to return the IDs of the Facebook pages that have zero likes. The output should be sorted in ascending order based on the page IDs.

My Solution:
```sql
SELECT page_id 
FROM pages
WHERE page_id NOT IN (SELECT DISTINCT(page_id) FROM page_likes)
ORDER BY page_id;
```
Alternative Solution:
```sql
SELECT page_id
FROM pages
EXCEPT
SELECT page_id
FROM page_likes;
```

![image](https://github.com/user-attachments/assets/08ff8a7b-9459-428b-8231-51468b31e5d0)

***

## 🟠 Medium


## 🔴 Hard
