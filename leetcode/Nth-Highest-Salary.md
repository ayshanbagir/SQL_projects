# Nth Highest Salary

### Problem:

Table: **Employee**

![image](https://user-images.githubusercontent.com/48019306/210471265-506fddd8-2823-4d96-b3a9-f4c5198a9ef5.png)

id is the primary key column for this table.
Each row of this table contains information about the salary of an employee.

Write an SQL query to report the **nth** highest salary from the Employee table. If there is no nth highest salary, the query should report **null**.

### My answer:

````sql
CREATE FUNCTION getNthHighestSalary(N IN NUMBER) RETURN NUMBER IS
result NUMBER;
BEGIN
    SELECT SUM(CASE WHEN ROWNUM = N THEN salary END) INTO result
     FROM (SELECT DISTINCT salary
            FROM employee
            ORDER BY salary DESC);
    RETURN result;
END; 
````
