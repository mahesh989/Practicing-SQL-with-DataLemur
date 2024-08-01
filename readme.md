# SQL & Data Interview Preparation

This repository includes SQL practice exercises from [DataLemur](https://datalemur.com/questions?category=SQL), inspired by the guide "How to Ace the Data Science Interview," to help prepare for data science and data analyst interviews at top tech companies.

## Histogram of Tweets

Assume you're given a table `tweets` with the following columns:
- `tweet_id` (integer)
- `user_id` (integer)
- `msg` (string)
- `tweet_date` (timestamp)

Write a query to obtain a histogram of tweets posted per user in 2022. Output the tweet count per user as the bucket and the number of Twitter users who fall into that bucket.

For the complete question, [click here](https://datalemur.com/questions/sql-histogram-tweets).

### SQL Code:
```sql
WITH twitter_data AS (
    SELECT 
        user_id, 
        COUNT(tweet_id) AS number_of_tweets
    FROM 
        tweets
    WHERE
        date_part('year', tweet_date) = 2022
    GROUP BY 
        user_id
)
SELECT 
    number_of_tweets AS tweet_bucket,
    COUNT(user_id) AS users_num
FROM 
    twitter_data
GROUP BY 
    number_of_tweets
ORDER BY 
    number_of_tweets;
```    

## Data Science Skills

Given a table `candidates` with the following columns:
- `candidate_id` (integer)
- `skill` (varchar)

You need to find candidates who are proficient in Python, Tableau, and PostgreSQL. Write a query to list the candidates who possess all of the required skills for the job. Sort the output by candidate ID in ascending order.

For the complete question, [click here](https://datalemur.com/questions/sql-histogram-tweets).

### SQL Code:
```sql
SELECT 
    candidate_id
FROM 
    candidates
WHERE 
    skill IN ('Python', 'Tableau', 'PostgreSQL')
GROUP BY 
    candidate_id
HAVING 
    COUNT(skill) = 3
ORDER BY 
    candidate_id; 
```

## Page With No Likes [Facebook SQL Interview Question]

Assume you're given the following tables:

**pages Table:**
- `page_id` (integer)
- `page_name` (varchar)

**page_likes Table:**
- `user_id` (integer)
- `page_id` (integer)
- `liked_date` (datetime)

Write a query to return the IDs of the Facebook pages that have zero likes. The output should be sorted in ascending order based on the page IDs.

For the complete question, [click here](https://datalemur.com/questions/sql-page-with-no-likes).


### SQL Code:
```sql
SELECT
  p.page_id
FROM
  pages p
LEFT JOIN
  page_likes pl ON p.page_id = pl.page_id
WHERE
  pl.page_id IS NULL
ORDER BY
  p.page_id ASC;
```


## Tesla Unfinished Parts [Tesla SQL Interview Question]

Write a query to determine which parts have begun the assembly process but are not yet finished.

**parts_assembly Table:**
- `part` (string)
- `finish_date` (datetime)
- `assembly_step` (integer)

For the complete question, [click here](https://datalemur.com/questions/tesla-unfinished-parts).

### SQL Code:
```sql
SELECT 
  part, 
  assembly_step
FROM 
  parts_assembly
WHERE 
  finish_date IS NULL;
```
