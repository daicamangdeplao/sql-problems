# Problem 2

Harry Potter and his friends are at Ollivander's with Ron, finally replacing Charlie's old broken wand.

Hermione decides the best way to choose is by determining the minimum number of gold galleons needed to buy each non-evil wand of high power and age. Write a query to print the ``id``, ``age``, ``coins_needed``, and ``power of the wands`` that Ron's interested in, sorted in order of **descending power**. If more than one wand has same power, sort the result in order of **descending age**.

Supposedly, we have the schema ``Wands``

| Column       | Type    |
| ------------ | ------- |
| id           | Integer |
| code         | Integer |
| coins_needed | Integer |
| power        | Integer |

``Wands_Property``

| Column  | Type    |
| ------- | ------- |
| code    | Integer |
| age     | Integer |
| is_evil | Integer |

Given the sample data in [Appendix](#appendix), the expected result is

```sql
9 45 1647 10
12 17 9897 10
1 20 3688 8
15 40 6018 7
19 20 7651 6
11 40 7587 5
10 20 504 5
18 40 3312 3
20 17 5689 3
5 45 6020 2
14 40 5408 1
```

## Solution

```sql
SELECT W.id, WP.age, W.coins_needed, w.power
FROM Wands W                                                        --- for each record in the table of Wands
JOIN Wands_Property WP ON W.code = WP.code                          ---     take the corresponding record in the table of Wands_Property into account
WHERE WP.is_evil = 0                                                ---     the sum-up record must satisfy: is-not-evil
  AND W.coins_needed = (                                            ---         AND coins_needed of the current record is MIN -> MIN of what?
    SELECT MIN(subq_W.coins_needed)
    FROM wands subq_W
    JOIN Wands_Property subq_WP ON subq_WP.code = subq_W.code
    WHERE subq_WP.age = WP.age AND W.power = subq_W.power
)
ORDER BY W.power DESC, WP.age DESC;
```

## Lesson Learned

* How to interpret the SQL script

## Appendix

```sql
create table if not exists wands
(
    id           integer,
    code         integer,
    coins_needed integer,
    power        integer
);

alter table wands
    owner to postgres;

INSERT INTO public.wands (id, code, coins_needed, power) VALUES (1, 4, 3688, 8);
INSERT INTO public.wands (id, code, coins_needed, power) VALUES (2, 3, 9365, 3);
INSERT INTO public.wands (id, code, coins_needed, power) VALUES (3, 3, 7187, 10);
INSERT INTO public.wands (id, code, coins_needed, power) VALUES (4, 3, 734, 8);
INSERT INTO public.wands (id, code, coins_needed, power) VALUES (5, 1, 6020, 2);
INSERT INTO public.wands (id, code, coins_needed, power) VALUES (6, 2, 6773, 7);
INSERT INTO public.wands (id, code, coins_needed, power) VALUES (7, 3, 9873, 9);
INSERT INTO public.wands (id, code, coins_needed, power) VALUES (8, 3, 7721, 7);
INSERT INTO public.wands (id, code, coins_needed, power) VALUES (9, 1, 1647, 10);
INSERT INTO public.wands (id, code, coins_needed, power) VALUES (10, 4, 504, 5);
INSERT INTO public.wands (id, code, coins_needed, power) VALUES (11, 2, 7587, 5);
INSERT INTO public.wands (id, code, coins_needed, power) VALUES (12, 5, 9897, 10);
INSERT INTO public.wands (id, code, coins_needed, power) VALUES (13, 3, 4651, 8);
INSERT INTO public.wands (id, code, coins_needed, power) VALUES (14, 2, 5408, 1);
INSERT INTO public.wands (id, code, coins_needed, power) VALUES (15, 2, 6018, 7);
INSERT INTO public.wands (id, code, coins_needed, power) VALUES (16, 4, 7710, 5);
INSERT INTO public.wands (id, code, coins_needed, power) VALUES (17, 2, 8798, 7);
INSERT INTO public.wands (id, code, coins_needed, power) VALUES (18, 2, 3312, 3);
INSERT INTO public.wands (id, code, coins_needed, power) VALUES (19, 4, 7651, 6);
INSERT INTO public.wands (id, code, coins_needed, power) VALUES (20, 5, 5689, 3);

create table if not exists wands_property
(
    code    integer,
    age     integer,
    is_evil integer
);

alter table wands_property
    owner to postgres;

INSERT INTO public.wands_property (code, age, is_evil) VALUES (1, 45, 0);
INSERT INTO public.wands_property (code, age, is_evil) VALUES (2, 40, 0);
INSERT INTO public.wands_property (code, age, is_evil) VALUES (3, 4, 1);
INSERT INTO public.wands_property (code, age, is_evil) VALUES (4, 20, 0);
INSERT INTO public.wands_property (code, age, is_evil) VALUES (5, 17, 0);
```
