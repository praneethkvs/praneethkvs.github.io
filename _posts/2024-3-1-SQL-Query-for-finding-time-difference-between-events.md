---
layout: post
title: SQL Query to find Time Difference between events 
---

If we have data for each user Id and Timestamps of different events performed by the user,   
we want to calculate the time elapsed between consecutive events for each user ID.  

Example Table of Data:    

![image](https://github.com/praneethkvs/praneethkvs.github.io/assets/25500916/f71db7ad-0e8b-4571-9e0d-d3560ab6becb)


SQL Query:  
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

The LAG function is used to get the previous row's event_timestamp_ct value for each user.  
The PARTITION BY clause is used to reset the window for each user_id_token.  
The ORDER BY clause inside the OVER clause is used to ensure the timestamps are processed in the correct order.  
The TIMESTAMPDIFF function calculates the difference in minutes between the current and previous timestamps.    

  
