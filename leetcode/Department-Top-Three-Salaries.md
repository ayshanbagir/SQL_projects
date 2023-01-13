# Department Top Three Salaries  

### Problem: 

Table: **Employee** 

![image](https://user-images.githubusercontent.com/48019306/210480637-af68b658-1ae2-4fae-a36a-c88a6c9061bc.png)

id is the primary key column for this table.
departmentId is a foreign key of the ID from the **Department** table.
Each row of this table indicates the ID, name, and salary of an employee. It also contains the ID of their department.

Table: **Department**

![image](https://user-images.githubusercontent.com/48019306/210480756-2b762dea-bd6c-41d7-94c1-5a60ab33d0e5.png)

id is the primary key column for this table.
Each row of this table indicates the ID of a department and its name.
  
A company's executives are interested in seeing who earns the most money in each of the company's departments. A high earner in a department is an employee who has a salary in the top three unique salaries for that department.

Write an SQL query to find the employees who are high earners in each of the departments.

Return the result table in any order.   

### My answer: 

````sql 
SELECT department, employee, salary 
  FROM (SELECT d.name department, e.name employee, e.salary, 
               DENSE_RANK() OVER (PARTITION BY d.id ORDER BY e.salary DESC) slry
          FROM employee e
          JOIN department d 
            ON d.id = e.departmentid)
 WHERE slry <=3;  
```` 
