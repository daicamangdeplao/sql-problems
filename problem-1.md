# Question 1

Julia just finished conducting a coding contest, and she needs your help assembling the leaderboard! Write a query to print the respective ``hacker_id`` and ``name of hackers`` who achieved full scores for more than one challenge. Order your output in **descending order by the total number of challenges** in which the hacker earned a full score. If more than one hacker received full scores in same number of challenges, then sort them by **ascending** ``hacker_id``.

Supposedly, we have the schema ``Hackers``

| Column    | Type    |
| --------- | ------- |
| hacker_id | Integer |
| name      | String  |

``Difficulty``

| Column           | Type    |
| ---------------- | ------- |
| difficulty_level | Integer |
| score            | Integer |

``Challenges``

| Column           | Type    |
| ---------------- | ------- |
| challenge_id     | Integer |
| hacker_id        | Integer |
| difficulty_level | Integer |

``Submissions``

| Column        | Type    |
| ------------- | ------- |
| submission_id | Integer |
| hacker_id     | Integer |
| challenge_id  | Integer |
| score         | Integer |

## Challenges

* How to refer to multiple tables while using JOIN clause ``->`` nested JOIN clauses

## Solutions

```sql
(SELECT Submissions.submission_id, Submissions.hacker_id, Submissions.challenge_id, Submissions.score, Challenges.difficulty_level
FROM Submissions
LEFT JOIN Challenges ON Challenges.challenge_id = Submissions.challenge_id) jt1;

(SELECT jt1.submission_id, jt1.hacker_id, jt1.challenge_id, jt1.score
FROM jt1
JOIN Difficulty ON jt1.difficulty_level = Difficulty.difficulty_level
WHERE Difficulty.score = jt1.score) jt2;

SELECT Hackers.hacker_id, Hackers.name
FROM jt2
JOIN Hackers ON jt2.hacker_id = Hackers.hacker_id
GROUP BY Hackers.hacker_id, Hackers.name
HAVING count(Hackers.hacker_id) >= 2
ORDER BY count(Hackers.hacker_id) DESC, Hackers.hacker_id ASC;

--- Sum up
SELECT Hackers.hacker_id, Hackers.name
FROM (SELECT jt1.submission_id, jt1.hacker_id, jt1.challenge_id, jt1.score
      FROM (SELECT Submissions.submission_id, Submissions.hacker_id, Submissions.challenge_id, Submissions.score, Challenges.difficulty_level
            FROM Submissions
                     LEFT JOIN Challenges ON Challenges.challenge_id = Submissions.challenge_id) jt1
               JOIN Difficulty ON jt1.difficulty_level = Difficulty.difficulty_level
      WHERE Difficulty.score = jt1.score) jt2
JOIN Hackers ON jt2.hacker_id = Hackers.hacker_id
GROUP BY Hackers.hacker_id, Hackers.name
HAVING count(Hackers.hacker_id) >= 2
ORDER BY count(Hackers.hacker_id) DESC, Hackers.hacker_id ASC;
```

## Lessons Learned

* The aggregation function is not always at SELECT clause, but it can be placed at HAVING clause. The most important thing is about the column will be grouped when using the corresponding aggregation function.