# [1280. 学生们参加各科测试的次数](https://leetcode.cn/problems/students-and-examinations)

[English Version](/solution/1200-1299/1280.Students%20and%20Examinations/README_EN.md)

## 题目描述

<!-- 这里写题目描述 -->

<p>学生表: <code>Students</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| student_id    | int     |
| student_name  | varchar |
+---------------+---------+
在 SQL 中，主键为 student_id（学生ID）。
该表内的每一行都记录有学校一名学生的信息。
</pre>

<p>&nbsp;</p>

<p>科目表: <code>Subjects</code></p>

<pre>
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| subject_name | varchar |
+--------------+---------+
在 SQL 中，主键为 subject_name（科目名称）。
每一行记录学校的一门科目名称。
</pre>

<p>&nbsp;</p>

<p>考试表: <code>Examinations</code></p>

<pre>
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| student_id   | int     |
| subject_name | varchar |
+--------------+---------+
这个表可能包含重复数据（换句话说，在 SQL 中，这个表没有主键）。
学生表里的一个学生修读科目表里的每一门科目。
这张考试表的每一行记录就表示学生表里的某个学生参加了一次科目表里某门科目的测试。
</pre>

<p>&nbsp;</p>

<p>查询出每个学生参加每一门科目测试的次数，结果按 <code>student_id</code> 和 <code>subject_name</code> 排序。</p>

<p>查询结构格式如下所示。</p>

<p>&nbsp;</p>

<p><strong>示例 1：</strong></p>

<pre>
<strong>输入：</strong>
Students table:
+------------+--------------+
| student_id | student_name |
+------------+--------------+
| 1          | Alice        |
| 2          | Bob          |
| 13         | John         |
| 6          | Alex         |
+------------+--------------+
Subjects table:
+--------------+
| subject_name |
+--------------+
| Math         |
| Physics      |
| Programming  |
+--------------+
Examinations table:
+------------+--------------+
| student_id | subject_name |
+------------+--------------+
| 1          | Math         |
| 1          | Physics      |
| 1          | Programming  |
| 2          | Programming  |
| 1          | Physics      |
| 1          | Math         |
| 13         | Math         |
| 13         | Programming  |
| 13         | Physics      |
| 2          | Math         |
| 1          | Math         |
+------------+--------------+
<strong>输出：</strong>
+------------+--------------+--------------+----------------+
| student_id | student_name | subject_name | attended_exams |
+------------+--------------+--------------+----------------+
| 1          | Alice        | Math         | 3              |
| 1          | Alice        | Physics      | 2              |
| 1          | Alice        | Programming  | 1              |
| 2          | Bob          | Math         | 1              |
| 2          | Bob          | Physics      | 0              |
| 2          | Bob          | Programming  | 1              |
| 6          | Alex         | Math         | 0              |
| 6          | Alex         | Physics      | 0              |
| 6          | Alex         | Programming  | 0              |
| 13         | John         | Math         | 1              |
| 13         | John         | Physics      | 1              |
| 13         | John         | Programming  | 1              |
+------------+--------------+--------------+----------------+
<strong>解释：</strong>
结果表需包含所有学生和所有科目（即便测试次数为0）：
Alice 参加了 3 次数学测试, 2 次物理测试，以及 1 次编程测试；
Bob 参加了 1 次数学测试, 1 次编程测试，没有参加物理测试；
Alex 啥测试都没参加；
John  参加了数学、物理、编程测试各 1 次。
</pre>

## 解法

### 方法一：两次连接 + 分组统计

我们可以先连接 `Students` 表和 `Subjects` 表，得到所有学生和所有科目的组合，然后再连接 `Examinations` 表，连接条件为 `student_id` 和 `subject_name`，这样就得到了每个学生参加每一门科目测试的次数，最后按 `student_id` 和 `subject_name` 分组统计即可。

<!-- tabs:start -->

```sql
# Write your MySQL query statement below
SELECT student_id, student_name, subject_name, COUNT(e.student_id) AS attended_exams
FROM
    Students
    JOIN Subjects
    LEFT JOIN Examinations AS e USING (student_id, subject_name)
GROUP BY 1, 3
ORDER BY 1, 3;
```

<!-- tabs:end -->

<!-- end -->
