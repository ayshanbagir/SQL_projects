# Trips and Users 

### Problem: 

Table: **Trips** 

![image](https://user-images.githubusercontent.com/48019306/210653736-9607316d-cf9a-47a0-b8b4-2deb1c7644c9.png)

id is the primary key for this table.
The table holds all taxi trips. Each trip has a unique id, while client_id and driver_id are foreign keys to the users_id at the Users table.
Status is an ENUM type of ('completed', 'cancelled_by_driver', 'cancelled_by_client').

Table: **Users**

![image](https://user-images.githubusercontent.com/48019306/210653875-3e4dfe07-a88b-44dd-ada5-fdadc3d1b500.png)

users_id is the primary key for this table.
The table holds all users. Each user has a unique users_id, and role is an ENUM type of ('client', 'driver', 'partner').
banned is an ENUM type of ('Yes', 'No').

The **cancellation rate** is computed by dividing the number of canceled (by client or driver) requests with unbanned users by the total number of requests with unbanned users on that day.

Write a SQL query to find the **cancellation rate** of requests with unbanned users (**both client and driver must not be banned**) each day between "2013-10-01" and "2013-10-03". Round Cancellation Rate to two decimal points.

Return the result table in any order.

### Answer: 

````sql 
SELECT t.request_at AS "day", 
       ROUND(SUM(CASE WHEN t.status IN ('cancelled_by_driver','cancelled_by_client') 
                            THEN 1 ELSE 0 END) / SUM(1), 2) "Cancellation Rate"
FROM trips t
JOIN users c ON c.users_id = t.client_id AND c.banned = 'No'
JOIN users d ON d.users_id = t.driver_id AND d.banned = 'No'
WHERE t.request_at BETWEEN '2013-10-01' AND '2013-10-03'
GROUP BY t.request_at;  
```` 
