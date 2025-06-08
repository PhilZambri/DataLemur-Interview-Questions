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

### 📌 [Tesla | Easy | Unfinished Parts](https://datalemur.com/questions/tesla-unfinished-parts)

Tesla is investigating production bottlenecks and they need your help to extract the relevant data. 
Write a query to determine which parts have begun the assembly process but are not yet finished.

```sql
SELECT part, assembly_step 
FROM parts_assembly
WHERE finish_date IS NULL;
```

![image](https://github.com/user-attachments/assets/cf130096-9961-4b77-84ee-3c9317145a0d)

***

### 📌[NY Times | Easy | Laptop vs. Mobile Viewership](https://datalemur.com/questions/laptop-mobile-viewership)

Write a query that calculates the total viewership for laptops and mobile devices where mobile is defined as the sum of tablet and phone viewership. Output the total viewership for laptops as laptop_reviews and the total viewership for mobile devices as mobile_views.

My Solution:
```sql
WITH laptop(laptop_views) AS(
  SELECT COUNT(*)
  FROM viewership
  WHERE device_type = 'laptop'
),
  mobile(mobile_views) AS (
  SELECT COUNT(*)
  FROM viewership
  WHERE device_type IN ('tablet', 'phone')
)

SELECT laptop_views, mobile_views
FROM laptop, mobile
```

Alternative Solution:
```sql
SELECT 
  COUNT(*) FILTER (WHERE device_type = 'laptop') AS laptop_views,
  COUNT(*) FILTER (WHERE device_type IN ('tablet', 'phone'))  AS mobile_views 
FROM viewership;
```

![image](https://github.com/user-attachments/assets/e3f6fbda-e9c7-43a3-9e05-46fdeb4063b0)

***

### 📌 [Facebook | Easy | Average Post Hiatus (Part 1)](https://datalemur.com/questions/sql-average-post-hiatus-1)

Given a table of Facebook posts, for each user who posted at least twice in 2021, write a query to find the number of days between each user’s first post of the year and last post of the year in the year 2021. Output the user and number of the days between each user's first and last post.

My Solution:
```sql
SELECT user_id, EXTRACT(DAY FROM (MAX(post_date) - MIN(post_date))) AS days_between
FROM posts
WHERE EXTRACT(YEAR FROM post_date) = 2021
GROUP BY user_id
HAVING COUNT(*) >= 2;
```

![image](https://github.com/user-attachments/assets/88eeaa2c-dc98-42e9-b149-3b65b17d41ed)

***

### 📌

## 🟠 Medium


## 🔴 Hard

### 📌 [McKinsey | Hard | 3-Topping-Pizzas](https://datalemur.com/questions/pizzas-topping-cost)

Given a list of pizza toppings, consider all the possible 3-topping pizzas, and print out the total cost of those 3 toppings. Sort the results with the highest total cost on the top followed by pizza toppings in ascending order.  
Break ties by listing the ingredients in alphabetical order, starting from the first ingredient, followed by the second and third.

My Solution:
```sql
SELECT CONCAT_WS(',', p1.topping_name, p2.topping_name, p3.topping_name) AS pizza,
      p1.ingredient_cost + p2.ingredient_cost + p3.ingredient_cost AS total_cost
FROM pizza_toppings p1 CROSS JOIN pizza_toppings p2 CROSS JOIN pizza_toppings p3
WHERE p1.topping_name < p2.topping_name 
      AND p2.topping_name < p3.topping_name
ORDER BY total_cost DESC, pizza;
```

***

### 📌 [FAANG | Hard | Department vs Company Salary](https://datalemur.com/questions/sql-department-company-salary-comparison)

Write a query to compare the average salary of employees in each department to the company's average salary for March 2024. Return the comparison result as 'higher', 'lower', or 'same' for each department. Display the department ID, payment month (in MM-YYYY format), and the comparison result.

My Solution:
```sql
WITH employee_avg_salary AS(
  SELECT e.employee_id, name, salary, department_id, payment_date,
    avg(salary)OVER() AS overall_avg_salary,
    avg(salary)OVER(PARTITION BY department_id) AS dept_avg_salary
  FROM employee AS e JOIN salary AS s on e.employee_id = s.employee_id
  WHERE EXTRACT(MONTH FROM payment_date) = 3
)

SELECT DISTINCT department_id, TO_CHAR(payment_date, 'MM-YYYY') AS payment_date, 
  CASE
    WHEN dept_avg_salary < overall_avg_salary THEN 'lower'
    WHEN dept_avg_salary > overall_avg_salary THEN 'higher'
    ELSE 'same'
  END AS comparison
FROM employee_avg_salary;
```

Alternate Solution:
```sql
SELECT DISTINCT department_id,
       TO_CHAR(payment_date,'MM-YYYY'),
       CASE
            WHEN AVG(amount) OVER (PARTITION BY department_id) < AVG(amount) OVER () THEN 'lower'
            WHEN AVG(amount) OVER (PARTITION BY department_id) = AVG(amount) OVER () THEN 'same'
            WHEN AVG(amount) OVER (PARTITION BY department_id) > AVG(amount) OVER () THEN 'higher'
       END
FROM employee AS emp INNER JOIN salary AS s
ON emp.employee_id = s.employee_id
WHERE EXTRACT(MONTH FROM payment_date) = 3
      AND EXTRACT(YEAR FROM payment_date) = 2024;
```

![image](https://github.com/user-attachments/assets/76ee2acb-1b4d-4d86-90d8-acf4a498d81f)

***

### 📌 [Stripe | Hard | Repeated Payments](https://datalemur.com/questions/repeated-payments)

Sometimes, payment transactions are repeated by accident; it could be due to user error, API failure or a retry error that causes a credit card to be charged twice.  
Using the transactions table, identify any payments made at the same merchant with the same credit card for the same amount within 10 minutes of each other. Count such repeated payments.

My Solution:
```sql
WITH time_diff AS (
  SELECT transaction_id, merchant_id, credit_card_id, transaction_timestamp, amount,
    transaction_timestamp -
    LAG(transaction_timestamp)OVER(PARTITION BY merchant_id, credit_card_id, amount ORDER BY transaction_timestamp) AS prev_time_diff
  FROM transactions
)

SELECT COUNT(prev_time_diff) AS payment_count
FROM time_diff
WHERE prev_time_diff NOTNULL 
      AND EXTRACT(epoch FROM prev_time_diff)/60 <= 10;
```

Alternative Solution:
```sql
SELECT 
  COUNT(*) AS payment_count
FROM transactions AS t1
JOIN transactions AS t2
ON t1.merchant_id = t1.merchant_id
  AND t1.credit_card_id = t2.credit_card_id
  AND t1.amount = t2.amount
  AND t1.transaction_timestamp < t2.transaction_timestamp
  AND t2.transaction_timestamp - t1.transaction_timestamp <= INTERVAL '10 MINUTES';
```

![image](https://github.com/user-attachments/assets/cb414818-4188-4302-9e0b-0a045dff29b5)

***

### 📌 [Amazon | Hard | Server Utilization Time](https://datalemur.com/questions/total-utilization-time)

Amazon Web Services (AWS) is powered by fleets of servers. Senior management has requested data-driven solutions to optimize server usage.  
Write a query that calculates the total time that the fleet of servers was running. The output should be in units of full days.

My Solution:
```sql
WITH time_diff AS(
  SELECT server_id, status_time, session_status, 
    status_time - LAG(status_time, 1)OVER(PARTITION BY server_id ORDER BY status_time) AS prev_time
  FROM server_utilization
  ORDER BY server_id, status_time
)

SELECT FLOOR(EXTRACT(epoch FROM SUM(prev_time))/86400) AS total_uptime_days
FROM time_diff
WHERE session_status = 'stop';
```

Alternative Solution:
```sql
WITH time_diff AS(
  SELECT server_id, status_time, session_status, 
    status_time - LAG(status_time, 1)OVER(PARTITION BY server_id ORDER BY status_time) AS prev_time
  FROM server_utilization
  ORDER BY server_id, status_time
)

SELECT EXTRACT(DAY FROM JUSTIFY_HOURS(SUM(prev_time))) AS total_uptime_days
FROM time_diff
WHERE session_status = 'stop';
```

![image](https://github.com/user-attachments/assets/b67fb09c-385a-42a0-9163-d081c50165b3)

***

### 📌
