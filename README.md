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


Q1- Find the student(s) who scored the highest total marks in their department.

Solution 1-> Used Window function 

with t2 as (with t as (select student_name,s.dept_id, dept_name,sum(score) as mx from students1 as s left join departments1 as d on s.dept_id=d.dept_id left join marks m on m.student_id=s.student_id group by 1,2) select *, DENSE_RANK() OVER (PARTITION BY dept_id order BY mx DESC) as rnk from t) select * from t2  where rnk=1;



Solution 2-> Used basic joins  and  aggregate function

select student_name,s.dept_id, dept_name,sum(score) as mx from students1 as s left join departments1 as d on s.dept_id=d.dept_id left join marks m on m.student_id=s.student_id group by 1,2,3 having sum(score)=(select max(ttl) from (select s2.student_id,sum(score) as ttl from students1 as s2 left join departments1 as d2 on s2.dept_id=d2.dept_id left join marks m2 on m2.student_id=s2.student_id where d2.dept_name=d.dept_name  group by 1) as t)


Q2 —> Find the student(s) who scored the 2nd highest total marks in their department.

Solution 1-> Used Window function 

with t2 as (with t as (select student_name,s.dept_id, dept_name,sum(score) as mx from students1 as s left join departments1 as d on s.dept_id=d.dept_id left join marks m on m.student_id=s.student_id group by 1,2) select *, DENSE_RANK() OVER (PARTITION BY dept_id order BY mx DESC) as rnk from t) select * from t2  where rnk=2;


Solution 2-> Used basic joins  and  aggregate function

select student_name,s.dept_id, dept_name,sum(score) as mx from students1 as s left join departments1 as d on s.dept_id=d.dept_id left join marks m on m.student_id=s.student_id group by 1,2,3 having sum(score)=(select max(ttl) from (select s2.student_id,sum(score) as ttl from students1 as s2 left join departments1 as d2 on s2.dept_id=d2.dept_id left join marks m2 on m2.student_id=s2.student_id where d2.dept_name=d.dept_name group by 1 having sum(score)<( select max(ttl) from (select s3.student_id,sum(score) as ttl from students1 as s3 left join departments1 as d3 on s3.dept_id=d3.dept_id left join marks m3 on m3.student_id=s3.student_id where d3.dept_name=d.dept_name group by 1) as t)) as t);









