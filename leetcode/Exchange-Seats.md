# Exchange Seats  

### Problem: 
 
Table: **Seat** 

![image](https://user-images.githubusercontent.com/48019306/211846156-fd98e46d-3219-4e55-a89c-2f8667c13436.png)

id is the primary key column for this table.
Each row of this table indicates the name and the ID of a student.
id is a continuous increment.  

Write an SQL query to swap the seat id of every two consecutive students. If the number of students is odd, the id of the last student is not swapped.

Return the result table ordered by id in ascending order.

### Answer: 

````sql 
SELECT s.id, NVL(n.student, s.student) student
FROM (SELECT id, 
             CASE WHEN MOD(id, 2)=0 THEN id-1 ELSE id+1 END newid,
             student
      FROM seat) s
LEFT JOIN seat n 
       ON s.newid = n.id
ORDER BY 1;
```` 
