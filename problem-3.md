# Problem 3

* Julia asked her students to create some coding challenges. Write a query to print the ``hacker_id``, ``name``, and **the total number of challenges created by each student**.
* Sort your results by the total number of challenges in **descending** order.
* If more than one student created the same number of challenges, then sort the result by ``hacker_id``.
* If more than one student created the same number of challenges and the count is less than the maximum number of challenges created, then exclude those students from the result.

## Schemas

``Hackers``

| Column        | Type    |
| ------------- | ------- |
| ``hacker_id`` | Integer |
| ``name``      | String  |

``Challenges``

| Column           | Type    |
| ---------------- | ------- |
| ``hacker_id``    | Integer |
| ``challenge_id`` | Integer |

## Example

## Solution

```sql
with cte(a, b, c, d) as
         (select h.hacker_id,
                 h.name,
                 count(c.challenge_id),
                 count(h.hacker_id) over (partition by count(c.challenge_id))
          from hackers h
                   join challenges c on h.hacker_id = c.hacker_id
          group by 1, 2
          order by 3 desc, 1)

select a, b, c
from cte
where d = 1
   or c = (select max(c) from cte);
```

## Challenges

## Lesson Learned

## Appendix

```sql
create table challenges
(
    hacker_id    integer,
    challenge_id integer
);

alter table challenges
    owner to postgres;

INSERT INTO public.challenges (hacker_id, challenge_id) VALUES (5077, 6165);
INSERT INTO public.challenges (hacker_id, challenge_id) VALUES (21283, 58302);
INSERT INTO public.challenges (hacker_id, challenge_id) VALUES (88255, 40587);
INSERT INTO public.challenges (hacker_id, challenge_id) VALUES (5077, 29477);
INSERT INTO public.challenges (hacker_id, challenge_id) VALUES (21283, 1220);
INSERT INTO public.challenges (hacker_id, challenge_id) VALUES (21283, 69514);
INSERT INTO public.challenges (hacker_id, challenge_id) VALUES (62743, 46561);
INSERT INTO public.challenges (hacker_id, challenge_id) VALUES (62743, 58077);
INSERT INTO public.challenges (hacker_id, challenge_id) VALUES (88255, 18483);
INSERT INTO public.challenges (hacker_id, challenge_id) VALUES (21283, 76766);
INSERT INTO public.challenges (hacker_id, challenge_id) VALUES (5077, 52382);
INSERT INTO public.challenges (hacker_id, challenge_id) VALUES (21283, 74467);
INSERT INTO public.challenges (hacker_id, challenge_id) VALUES (96196, 33625);
INSERT INTO public.challenges (hacker_id, challenge_id) VALUES (88255, 26053);
INSERT INTO public.challenges (hacker_id, challenge_id) VALUES (62743, 42665);
INSERT INTO public.challenges (hacker_id, challenge_id) VALUES (62743, 12859);
INSERT INTO public.challenges (hacker_id, challenge_id) VALUES (21283, 70094);
INSERT INTO public.challenges (hacker_id, challenge_id) VALUES (88255, 34599);
INSERT INTO public.challenges (hacker_id, challenge_id) VALUES (88255, 54680);
INSERT INTO public.challenges (hacker_id, challenge_id) VALUES (5077, 61881);

create table hackers
(
    hacker_id integer,
    name      varchar
);

alter table hackers
    owner to postgres;

INSERT INTO public.hackers (hacker_id, name) VALUES (5077, 'Rose');
INSERT INTO public.hackers (hacker_id, name) VALUES (21283, 'Angela');
INSERT INTO public.hackers (hacker_id, name) VALUES (62743, 'Frank');
INSERT INTO public.hackers (hacker_id, name) VALUES (88255, 'Patrick');
INSERT INTO public.hackers (hacker_id, name) VALUES (96196, 'Lisa');

```
