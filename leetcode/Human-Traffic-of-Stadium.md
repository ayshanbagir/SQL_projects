# Human Traffic of Stadium 
  
### Problem: 
  
Table: **Stadium** 

![image](https://user-images.githubusercontent.com/48019306/211852412-6ee36ceb-2659-471a-9be1-cced4cafc43f.png)

visit_date is the primary key for this table.
Each row of this table contains the visit date and visit id to the stadium with the number of people during the visit.
No two rows will have the same visit_date, and as the id increases, the dates increase as well.

Write an SQL query to display the records with three or more rows with **consecutive** id's, and the number of people is greater than or equal to 100 for each.

Return the result table ordered by visit_date in **ascending order**.

### My answer: 

````sql   
WITH cte AS (
    SELECT * 
      FROM (SELECT LAG(id) OVER (ORDER BY visit_date) prev_id,
                   id,
                   LEAD(id) OVER (ORDER BY visit_date) next_id
              FROM stadium 
             WHERE people >= 100)
     WHERE next_id - prev_id = 2),

cte2 AS (
    SELECT id FROM cte
     UNION
    SELECT prev_id FROM cte
     UNION
    SELECT next_id FROM cte)

SELECT s.id, 
       TO_CHAR(s.visit_date, 'yyyy-mm-dd') visit_date, 
       s.people
  FROM cte2 c
  JOIN stadium s 
    ON c.id = s.id
 ORDER BY visit_date;
```` 
