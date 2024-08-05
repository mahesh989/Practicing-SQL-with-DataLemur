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

## New York Times [SQL Interview Question]

Assume you're given the table on user viewership categorized by device type where the device types include laptop, tablet, phone, and tv.

Write a query that calculates the total viewership for laptops and mobile devices, where mobile is defined as the sum of tablet and phone viewership. Output the total viewership for laptops as `laptop_views` and the total viewership for mobile devices as `mobile_views`.

**viewership Table:**
- `device_type` (string)
- `viewership_count` (integer)

For the complete question, [click here](https://datalemur.com/questions/laptop-mobile-viewership).

### SQL Code:
```sql
WITH viewer_ship AS (
  SELECT 
    device_type,
    COUNT(*) AS each_count
  FROM 
    viewership
  GROUP BY 
    device_type
)
SELECT 
  SUM(CASE WHEN device_type = 'laptop' THEN each_count ELSE 0 END) AS laptop_views,
  SUM(CASE WHEN device_type IN ('tablet', 'phone') THEN each_count ELSE 0 END) AS mobile_views
FROM 
  viewer_ship;
```


## Average Post Hiatus (Part 1) [Facebook SQL Interview Question]

**Description**  
Given a table of Facebook posts, for each user who posted at least twice in 2021, write a query to find the number of days between each user’s first post of the year and last post of the year in 2021. Output the user and the number of days between each user's first and last post.

**posts Table**

- `user_id` (integer)
- `post_id` (integer)
- `post_content` (text)
- `post_date` (timestamp)

For the complete question, [click here](https://datalemur.com/questions/sql-average-post-hiatus-1).

**SQL Code:**

```sql
WITH number_of_day AS (
  SELECT 
    user_id, 
    MIN(post_date) AS first_date, 
    MAX(post_date) AS last_date
  FROM 
    posts
  WHERE 
    date_part('year', post_date) = 2021
  GROUP BY 
    user_id
)
SELECT 
  user_id,
  date_part('day', last_date - first_date) AS days_between
FROM 
  number_of_day
WHERE 
  date_part('day', last_date - first_date) > 0;
```


## Teams Power Users [Microsoft SQL Interview Question]

**Description**  
Write a query to identify the top 2 Power Users who sent the highest number of messages on Microsoft Teams in August 2022. Display the IDs of these 2 users along with the total number of messages they sent. Output the results in descending order based on the count of the messages.

**messages Table**

- `message_id` (integer)
- `sender_id` (integer)
- `receiver_id` (integer)
- `content` (varchar)
- `sent_date` (datetime)

For the complete question, [click here](https://datalemur.com/questions/tesla-unfinished-parts).

**SQL Code:**

```sql
SELECT 
  sender_id, 
  COUNT(sender_id) AS count_messages
FROM 
  messages
WHERE 
  date_part('year', sent_date) = 2022
  AND date_part('month', sent_date) = 8
GROUP BY 
  sender_id
ORDER BY 
  count_messages DESC
LIMIT 2;
```


## Duplicate Job Listings [LinkedIn SQL Interview Question]

**Description**  
Assume you're given a table containing job postings from various companies on the LinkedIn platform. Write a query to retrieve the count of companies that have posted duplicate job listings.

**Definition:**  
Duplicate job listings are defined as two job listings within the same company that share identical titles and descriptions.

**job_listings Table**

- `job_id` (integer)
- `company_id` (integer)
- `title` (string)
- `description` (string)

**For the complete question: [click here](https://datalemur.com/questions/duplicate-job-listings)**

**SQL Code:**

```sql
WITH duplicate_companies AS (
  SELECT 
    company_id,
    COUNT(*) AS duplicate_count
  FROM 
    job_listings
  GROUP BY 
    company_id, title, description
  HAVING 
    COUNT(*) > 1
)
SELECT 
  COUNT(DISTINCT company_id) AS duplicate_companies
FROM 
  duplicate_companies;
```