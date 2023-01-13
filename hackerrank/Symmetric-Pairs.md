# Symmetric Pairs 

### Problem: 

You are given a table, Functions, containing two columns: X and Y.

Table: **Functions** 

![image](https://user-images.githubusercontent.com/48019306/211860514-0bfb9ed8-eb5d-45ef-a79a-d91191995eb7.png)

Two pairs (X1, Y1) and (X2, Y2) are said to be symmetric pairs if X1 = Y2 and X2 = Y1.

Write a query to output all such symmetric pairs in ascending order by the value of X. List the rows such that X1 ≤ Y1.
   
### My answer:  

````sql 
    SELECT f.x, f.y 
      FROM functions f 
INNER JOIN functions ff 
        ON f.x = ff.y 
       AND f.y = ff.x
     GROUP BY f.x, f.y
    HAVING f.x < f.y 
        OR COUNT(f.x) > 1
     ORDER BY f.x; 
```` 
