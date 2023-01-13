# Tree Node 

### Problem: 

Table: **Tree** 

![image](https://user-images.githubusercontent.com/48019306/211844675-06701e9b-4d52-440e-b7d7-7bfbbf71cb4c.png)

id is the primary key column for this table.
Each row of this table contains information about the id of a node and the id of its parent node in a tree.
The given structure is always a valid tree.

Each node in the tree can be one of three types:

"Leaf": if the node is a leaf node.

"Root": if the node is the root of the tree.

"Inner": If the node is neither a leaf node nor a root node.

Write an SQL query to report the type of each node in the tree.

Return the result table in any order.

### My answer:  

````sql 
SELECT id,
       CASE WHEN LEVEL = 1 THEN 'Root'
            WHEN CONNECT_BY_ISLEAF = 1 THEN 'Leaf'
            ELSE 'Inner' END type
FROM tree
START WITH p_id IS NULL
CONNECT BY p_id = PRIOR id
ORDER SIBLINGS BY id;  
```` 
