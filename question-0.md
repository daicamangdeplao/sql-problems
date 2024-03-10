# Question 0

Ketty gives Eve a task to generate a report containing three columns: Name, Grade and Mark. Ketty doesn't want the NAMES of those students who received a grade lower than 8.

* The report must be in **descending** order by grade -- i.e. higher grades are entered first. If there is more than one student with the same grade (8-10) assigned to them, order those particular students by their name alphabetically.

* Finally, if the grade is lower than 8, use "NULL" as their name and list them by their grades in **descending** order. If there is more than one student with the same grade (1-7) assigned to them, order those particular students by their marks in **ascending** order.

Suppossedly, we have the schema ``Students``

| Column | Type    |
| ------ | ------- |
| ID     | Integer |
| Name   | String  |
| Marks  | Integer |

with sample data

| ID  | Name     | Marks |
| --- | -------- | ----- |
| 1   | Julia    | 88    |
| 2   | Samantha | 68    |
| 3   | Maria    | 99    |
| 4   | Scarlet  | 78    |
| 5   | Ashley   | 63    |
| 6   | Jane     | 81    |

and a static mapping table, ``Grades``

| Grade | Min_Mark | Max_Mark |
| ----- | -------- | -------- |
| 1     | 0        | 9        |
| 2     | 10       | 19       |
| 3     | 20       | 29       |
| 4     | 30       | 39       |
| 5     | 40       | 49       |
| 6     | 50       | 59       |
| 7     | 60       | 69       |
| 8     | 70       | 79       |
| 9     | 80       | 89       |
| 10    | 90       | 99       |

## Challenges

* How to check if a value belongs to a range in a different table

## Solution

```SQL
(select s.name, g.grade, s.marks
from students s
    left join grades g 
        on s.marks between g.min_mark and g.max_mark
where g.grade >= 8)
union
(select null, g.grade, s.marks
from students s
    left join grades g 
        on s.marks between g.min_mark and g.max_mark
where g.grade < 8)
order by 2 desc, 1, 3
```

## Lessons Learned

* The condition in a ``JOIN`` statement should not only about equal comparison.

## References

* [9.2. Comparison Functions and Operators](https://www.postgresql.org/docs/current/functions-comparison.html)