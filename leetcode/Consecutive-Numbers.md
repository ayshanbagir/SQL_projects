# Consecutive Numbers

### Problem:

Table: **Logs**

+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| num         | varchar |
+-------------+---------+
id is the primary key for this table.
id is an autoincrement column.

Write an SQL query to find all numbers that appear at least three times consecutively.

Return the result table in any order.

### Answer:

````sql
SELECT DISTINCT num 
FROM (SELECT num,
             CASE WHEN LAG(num) OVER (ORDER BY id) = num 
                   AND LEAD(num) OVER (ORDER BY id) = num THEN 1 ELSE 0 END c
        FROM logs) 
WHERE c = 1; 
````
