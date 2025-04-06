---
layout: post
title: SQL Queries Practice
---

**1. Find All Users that have more than x Consecutive days of logins.**

```sql
--Step -1: 
--The ROW_NUMBER() function assigns a sequential number to each user's login based on login_date.
--The grp column groups consecutive dates by subtracting the ROW_NUMBER() value from the login_date.  
--Consecutive dates will have the same grp value.

SELECT user_id, 
login_date,  
ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY login_date) as rn,
login_date - INTERVAL (ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY login_date)) DAY AS grp
FROM 'user_logins_fixed.csv'
ORDER BY user_id, login_date;
```


```sql
--Step - 2
--Groups records by user_id and grp, then calculates the start_date, end_date,  
--and total consecutive_days for each group.
--Filters only groups with at least 5 consecutive days (COUNT(*) >= 5).  

with t1 as (
SELECT user_id, 
login_date,  
ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY login_date) as rn,
login_date - INTERVAL (ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY login_date)) DAY AS grp
FROM 'user_logins_fixed.csv'
ORDER BY user_id, login_date)

Select user_id, 
--grp,
min(login_date) as start_date,
max(login_date) as max_date,
count(*) as num_consecutive_logins
from t1
group by user_id, grp
HAVING num_consec_logins >= 5
order by 1,4 desc
```



**2. If we have data for each user Id and Timestamps of different events performed by the user,   
we want to calculate the time elapsed between consecutive events for each user ID.**

Example Table of Data:    
![image](https://github.com/praneethkvs/praneethkvs.github.io/assets/25500916/f71db7ad-0e8b-4571-9e0d-d3560ab6becb)


```sql
SELECT
  user_id_token,
  event_timestamp_ct,
  LAG(event_timestamp_ct) OVER (PARTITION BY user_id_token ORDER BY event_timestamp_ct )
  AS previous_event_timestamp_ct,
  TIMESTAMPDIFF( MINUTE,LAG(event_timestamp_ct) OVER (PARTITION BY user_id_token ORDER BY event_timestamp_ct ),
  event_timestamp_ct ) AS duration_minutes
FROM
  your_table_name
ORDER BY
  user_id_token,
  event_timestamp_ct
```

Additional Comments:  
The **LAG** function is used to get the previous row's event_timestamp_ct value for each user.  
The **PARTITION** BY clause is used to reset the window for each user_id_token.  
The **ORDER BY** clause inside the OVER clause is used to ensure the timestamps are processed in the correct order.  
The **TIMESTAMPDIFF** function calculates the difference in minutes between the current and previous timestamps.    

  
