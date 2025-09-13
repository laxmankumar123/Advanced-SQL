------SQL Notes-----------
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









