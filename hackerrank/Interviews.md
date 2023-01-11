# Interviews

### Problem: 

Samantha interviews many candidates from different colleges using coding challenges and contests. 
Write a query to print the contest_id, hacker_id, name, and the sums of total_submissions, total_accepted_submissions, total_views, and total_unique_views for each contest sorted by contest_id. 
Exclude the contest from the result if all four sums are 0.

**Note**: A specific contest can be used to screen candidates at more than one college, but each college only holds 1 screening contest.

The following tables hold interview data:

**Contests**: The contest_id is the id of the contest, hacker_id is the id of the hacker who created the contest, and name is the name of the hacker.
  
![image](https://user-images.githubusercontent.com/48019306/211894138-663a705e-671b-421c-a446-5d42de946486.png)

| Project Name | Description | SQL Functions |
|---|---|---|
| | |

**Colleges**: The college_id is the id of the college, and contest_id is the id of the contest that Samantha used to screen the candidates.

![image](https://user-images.githubusercontent.com/48019306/211894270-3bba639f-16f0-4575-a944-b3c33b18978c.png)

**Challenges**: The challenge_id is the id of the challenge that belongs to one of the contests whose contest_id Samantha forgot, 
and college_id is the id of the college where the challenge was given to candidates.

![image](https://user-images.githubusercontent.com/48019306/211894505-ee415fe0-23e5-4e37-aa26-6d74d5171ced.png)

**View_Stats**: The challenge_id is the id of the challenge, total_views is the number of times the challenge was viewed by candidates, 
and total_unique_views is the number of times the challenge was viewed by unique candidates.

![image](https://user-images.githubusercontent.com/48019306/211894626-d85728b4-df56-4399-8d36-6029f2547301.png)

**Submission_Stats**: The challenge_id is the id of the challenge, total_submissions is the number of submissions for the challenge, 
and total_accepted_submission is the number of submissions that achieved full scores.

![image](https://user-images.githubusercontent.com/48019306/211894730-8e1bae0a-f091-497b-ab4d-3a22b24bbe4d.png)

### Answer: 

````sql 
WITH submissions AS (
 SELECT challenge_id, 
        SUM(total_submissions) total_submissions, 
        SUM(total_accepted_submissions) total_accepted_submissions
   FROM submission_stats 
  GROUP BY challenge_id
),
views AS (
 SELECT challenge_id, 
        SUM(total_views) total_views, 
        SUM(total_unique_views) total_unique_views
   FROM view_stats 
  GROUP BY challenge_id
)
   SELECT c.contest_id, c.hacker_id, c.name,
          SUM(total_submissions) total_submissions, 
          SUM(total_accepted_submissions) total_accepted_submissions, 
          SUM(total_views) total_views, 
          SUM(total_unique_views) total_unique_views
     FROM contests c
     JOIN colleges g 
       ON g.contest_id = c.contest_id
     JOIN challenges ch 
       ON ch.college_id = g.college_id
LEFT JOIN views v 
       ON v.challenge_id = ch.challenge_id
LEFT JOIN submissions s 
       ON s.challenge_id = ch.challenge_id
    GROUP BY c.contest_id, c.hacker_id, c.name
   HAVING SUM(total_submissions) + SUM(total_accepted_submissions) + 
          SUM(total_views) + SUM(total_unique_views) > 0
    ORDER BY 1;
```` 
