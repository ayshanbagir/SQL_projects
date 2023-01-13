# Consecutive Numbers

### Problem:

Table: **Logs**

![image](https://user-images.githubusercontent.com/48019306/210469081-32a8174c-7be5-4612-a9d0-c71adedbdc33.png)

id is the primary key for this table.
id is an autoincrement column.

Write an SQL query to find all numbers that appear at least three times consecutively.

Return the result table in any order.

### My answer:

````sql
SELECT DISTINCT num 
FROM (SELECT num,
             CASE WHEN LAG(num) OVER (ORDER BY id) = num 
                   AND LEAD(num) OVER (ORDER BY id) = num THEN 1 ELSE 0 END c
        FROM logs) 
WHERE c = 1; 
````
