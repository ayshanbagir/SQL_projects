# Print Prime Numbers

### Problem:

Write a query to print all prime numbers less than or equal to **1000**. Print your result on a single line, and use the ampersand (**&**) character as your separator (instead of a space).

### Answer:

````sql
WITH tab1(x) AS (
    SELECT LEVEL +1 FROM dual
CONNECT BY LEVEL < 1000),
tab2 AS (SELECT * FROM tab1 WHERE MOD(x, 2) <> 0 OR x = 2)
SELECT LISTAGG(x,'&') WITHIN GROUP (ORDER BY x) prime_nums
  FROM (SELECT * FROM tab2 t1
         WHERE NOT EXISTS(SELECT 1 FROM tab2 t2 
                           WHERE t2.x >= 3
                             AND t2.x <= SQRT(t1.x) 
                             AND MOD(t1.x, t2.x) = 0)); 
````
