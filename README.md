# 📘 SQL Notes

## Students Table
```sql
mysql> select * from students1;
+------------+--------------+---------+---------------+
| student_id | student_name | dept_id | year_of_study |
+------------+--------------+---------+---------------+
|          1 | Laxman Kumar |       1 |             4 |
|          2 | Priya Verma  |       1 |             3 |
|          3 | Sahil Yadav  |       1 |             2 |
|          4 | Rohit Sharma |       2 |             4 |
|          5 | Aditya Patel |       2 |             2 |
|          6 | Ankit Singh  |       2 |             3 |
|          7 | Neha Gupta   |       1 |             1 |
|          8 | Rahul Mehra  |       3 |             3 |
|          9 | Kavita Rao   |       3 |             4 |
+------------+--------------+---------+---------------+
9 rows in set (0.003 sec)

mysql> select * from departments1;
+---------+------------------------+
| dept_id | dept_name              |
+---------+------------------------+
|       1 | Computer Science       |
|       2 | Electrical Engineering |
|       3 | Mechanical Engineering |
+---------+------------------------+
3 rows in set (0.001 sec)

mysql> select * from marks;
+------------+---------+-------+
| student_id | subject | score |
+------------+---------+-------+
|          1 | DBMS    |    95 |
|          1 | OS      |    90 |
|          2 | DBMS    |    85 |
|          2 | OS      |    92 |
|          3 | DBMS    |    80 |
|          3 | OS      |    75 |
|          4 | DBMS    |    88 |
|          4 | OS      |    78 |
|          5 | DBMS    |    70 |
|          5 | OS      |    82 |
|          6 | DBMS    |    76 |
|          6 | OS      |    81 |
|          7 | DBMS    |    89 |
|          7 | OS      |    95 |
|          8 | DBMS    |    92 |
|          8 | OS      |    85 |
|          9 | DBMS    |    99 |
|          9 | OS      |    91 |
+------------+---------+-------+
18 rows in set (0.003 sec)

Q1 — Find the student(s) who scored the highest total marks in their department
✅ Solution 1 (Using Window Function)
WITH t AS (
    SELECT 
        student_name,
        s.dept_id,
        dept_name,
        SUM(score) AS mx
    FROM students1 AS s
    LEFT JOIN departments1 AS d 
        ON s.dept_id = d.dept_id
    LEFT JOIN marks m 
        ON m.student_id = s.student_id
    GROUP BY student_name, s.dept_id, dept_name
),
t2 AS (
    SELECT 
        *,
        DENSE_RANK() OVER (
            PARTITION BY dept_id 
            ORDER BY mx DESC
        ) AS rnk
    FROM t
)
SELECT *
FROM t2
WHERE rnk = 1;

✅ Solution 2 (Using Joins + Aggregate Function)
SELECT 
    student_name,
    s.dept_id,
    dept_name,
    SUM(score) AS mx
FROM students1 AS s
LEFT JOIN departments1 AS d 
    ON s.dept_id = d.dept_id
LEFT JOIN marks m 
    ON m.student_id = s.student_id
GROUP BY student_name, s.dept_id, dept_name
HAVING SUM(score) = (
    SELECT MAX(ttl)
    FROM (
        SELECT 
            s2.student_id,
            SUM(score) AS ttl
        FROM students1 AS s2
        LEFT JOIN departments1 AS d2 
            ON s2.dept_id = d2.dept_id
        LEFT JOIN marks m2 
            ON m2.student_id = s2.student_id
        WHERE d2.dept_name = d.dept_name
        GROUP BY s2.student_id
    ) AS t
);

Q2 — Find the student(s) who scored the 2nd highest total marks in their department
✅ Solution 1 (Using Window Function)
WITH t AS (
    SELECT 
        student_name,
        s.dept_id,
        dept_name,
        SUM(score) AS mx
    FROM students1 AS s
    LEFT JOIN departments1 AS d 
        ON s.dept_id = d.dept_id
    LEFT JOIN marks m 
        ON m.student_id = s.student_id
    GROUP BY student_name, s.dept_id, dept_name
),
t2 AS (
    SELECT 
        *,
        DENSE_RANK() OVER (
            PARTITION BY dept_id 
            ORDER BY mx DESC
        ) AS rnk
    FROM t
)
SELECT *
FROM t2
WHERE rnk = 2;

✅ Solution 2 (Using Joins + Aggregate Function)
SELECT 
    student_name,
    s.dept_id,
    dept_name,
    SUM(score) AS mx
FROM students1 AS s
LEFT JOIN departments1 AS d 
    ON s.dept_id = d.dept_id
LEFT JOIN marks m 
    ON m.student_id = s.student_id
GROUP BY student_name, s.dept_id, dept_name
HAVING SUM(score) = (
    SELECT MAX(ttl)
    FROM (
        SELECT 
            s2.student_id,
            SUM(score) AS ttl
        FROM students1 AS s2
        LEFT JOIN departments1 AS d2 
            ON s2.dept_id = d2.dept_id
        LEFT JOIN marks m2 
            ON m2.student_id = s2.student_id
        WHERE d2.dept_name = d.dept_name
        GROUP BY s2.student_id
        HAVING SUM(score) < (
            SELECT MAX(ttl)
            FROM (
                SELECT 
                    s3.student_id,
                    SUM(score) AS ttl
                FROM students1 AS s3
                LEFT JOIN departments1 AS d3 
                    ON s3.dept_id = d3.dept_id
                LEFT JOIN marks m3 
                    ON m3.student_id = s3.student_id
                WHERE d3.dept_name = d.dept_name
                GROUP BY s3.student_id
            ) AS t
        )
    ) AS t
);
