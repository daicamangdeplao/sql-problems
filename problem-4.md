# Problem 4

* The total score of a hacker is the sum of their maximum scores for all of the challenges.
* Write a query to print the ``hacker_id``, ``name``, and total score of the hackers ordered by the **descending score**.
* If more than one hacker achieved the same total score, then sort the result by **ascending** ``hacker_id``.
* Exclude all hackers with a total score of ``0`` from your result.

## Schemas

``Hackers``

| Column    | Type    |
| --------- | ------- |
| hacker_id | Integer |
| name      | String  |

``Submissions``

| Column        | Type    |
| ------------- | ------- |
| submission_id | Integer |
| hacker_id     | Integer |
| challenge_id  | Integer |
| score         | Integer |

## Appendix
