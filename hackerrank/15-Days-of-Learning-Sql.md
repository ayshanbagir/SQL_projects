# 15 Days of Learning SQL 

### Problem: 

Julia conducted a  days of learning SQL contest. The start date of the contest was March 01, 2016 and the end date was March 15, 2016.

Write a query to print total number of unique hackers who made at least  submission each day (starting on the first day of the contest), 
and find the hacker_id and name of the hacker who made maximum number of submissions each day. 
If more than one such hacker has a maximum number of submissions, print the lowest hacker_id. 
The query should print this information for each day of the contest, sorted by the date.  

The following tables hold contest data:

**Hackers** : The hacker_id is the id of the hacker, and name is the name of the hacker.

![image](https://user-images.githubusercontent.com/48019306/212161057-db117dbf-e64b-4e2a-a702-67d6ec4cd18a.png)

**Submissions**: The submission_date is the date of the submission, submission_id is the id of the submission, hacker_id is the id of the hacker who made the submission, 
and score is the score of the submission.  

![image](https://user-images.githubusercontent.com/48019306/212161131-118a1254-5c7c-4eb4-b80f-04b4a70e0836.png)

### My answer: 

````sql 
WITH hackers_cnt AS (
SELECT d.submission_date, COUNT(d.hacker_id) h_cnt
  FROM (SELECT DISTINCT submission_date, hacker_id FROM submissions) d
 WHERE (SELECT COUNT(*) 
          FROM (SELECT DISTINCT submission_date, hacker_id FROM submissions) d1 
         WHERE d1.submission_date <= d.submission_date 
           AND d1.hacker_id = d.hacker_id) = EXTRACT(day FROM d.submission_date)
 GROUP BY d.submission_date),
max_submission AS (
SELECT submission_date, hacker_id, name 
  FROM (SELECT s.submission_date, s.hacker_id, s.sub_cnt, h.name,
               ROW_NUMBER() OVER (PARTITION BY s.submission_date ORDER BY s.submission_date, s.sub_cnt DESC, s.hacker_id) r
          FROM (SELECT submission_date, hacker_id, COUNT(1) sub_cnt 
                  FROM submissions
                 GROUP BY submission_date, hacker_id) s
  JOIN hackers h ON h.hacker_id = s.hacker_id)
 WHERE r=1)
SELECT c.submission_date, c.h_cnt, m.hacker_id, m.name
  FROM hackers_cnt c 
  JOIN max_submission m 
    ON m.submission_date = c.submission_date
 ORDER BY 1;  
```` 
