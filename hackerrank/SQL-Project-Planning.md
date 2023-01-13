# SQL Project Planning 

### Problem: 

You are given a table, Projects, containing three columns: Task_ID, Start_Date and End_Date. 
It is guaranteed that the difference between the End_Date and the Start_Date is equal to 1 day for each row in the table.

Table: **Projects** 

![image](https://user-images.githubusercontent.com/48019306/211888645-49598709-f5ec-4a0e-9bfc-5204cf311942.png)
  
If the End_Date of the tasks are consecutive, then they are part of the same project. 
Samantha is interested in finding the total number of different projects completed.

Write a query to output the start and end dates of projects listed by the number of days it took to complete the project in ascending order. 
If there is more than one project that have the same number of completion days, then order by the start date of the project.   

### My answer: 

````sql 
SELECT MIN(start_date) startdate, enddate 
  FROM (SELECT s.start_date,
              (SELECT MIN(p.end_date)
                 FROM projects p
            LEFT JOIN projects pp 
                   ON p.end_date = pp.start_date
                WHERE pp.end_date IS NULL
                  AND p.start_date >= s.start_date) enddate
          FROM projects s)
 GROUP BY enddate
 ORDER BY enddate - startdate, startdate;  
```` 

### My alternative Answer:

````sql
WITH data AS (
SELECT start_date, 
       end_date,
       NVL(LEAD(start_date) OVER (ORDER BY start_date) - end_date, 1) nxt,
       NVL(LAG(end_date) OVER (ORDER BY end_date) - start_date, -1) prv
  FROM projects)

SELECT start_date,
       (SELECT MIN(d2.end_date)
          FROM data d2 
         WHERE d2.start_date >= d1.start_date 
           AND d2.nxt > 0) end_date
  FROM data d1 
 WHERE prv < 0
 ORDER BY end_date - start_date, start_date;
 ````
 
