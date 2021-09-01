---
layout: post
title: SQL Tips/Tricks
---

Get Cumulative Sum for each day.   
Source - https://stackoverflow.com/questions/31734599/sum-of-all-rows-prior-to-and-including-date-on-current-row-in-mysql  
Make sure t1 has single row for each date and or date/customer group combination.   

```sql
select 
  t1.date_id, t1.customer, sum(t2.order_count)
from 
  table1 t1
left join 
  table1 t2 on t1.customer = t2.customer
           and t1.date_id >= t2.date_id
group by 
  t1.date_id, t1.customer;
```     


How to use multiple CTEs in a single Query.  
Source - https://stackoverflow.com/questions/17032293/how-to-create-cte-which-uses-another-cte-as-the-data-to-further-limit    

```sql
with t1 as
(
  select * from tableA
  where name like '%A%'
),
t2 as
(
  select * from t1
  where object_id < 100
)
select * from t2;
```      

Pad Missing Dates in Table with Zero values  
Source - https://stackoverflow.com/questions/19075098/how-to-fill-missing-dates-by-groups-in-a-table-in-sql?rq=1   

```sql
SELECT p.date, COALESCE(a.value, 0), p.grp_no
  FROM
(  
    SELECT distinct tableA.date,tableB.grp_no
      FROM tableA
      CROSS JOIN (SELECT distinct grp_no FROM tableA) tableB
  ) 
) p LEFT JOIN TableA a
    ON p.grp_no = a.grp_no 
   AND p.date = a.date
```   
    
    







