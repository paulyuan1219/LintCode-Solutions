# 2008 · 查询两门课的课程信息
数据库
入门
通过率
90%
题目
题解
笔记
讨论
排名
记录
描述
请编写 SQL 语句，从 courses 表中选取课程名为 'System Design' 或 'Django' 的课程信息，如果这两门课程同时存在，请将这两门课程的信息全部返回。

## 2022-08-31
```sql
SELECT *
from courses
where name in ('System Design', 'Django');

SELECT * 
FROM `courses`
WHERE `name` = "System Design"
OR `name` = "Django";
```


# 2016 · 更新指定教师的邮箱
数据库
入门
通过率
85%
题目
题解
笔记
讨论
排名
记录
描述
请编写 SQL 语句，将教师表 teachers 中教师名 (name) 为 Linghu Chong 的邮箱 (email) 更新为 "linghu.chong@lintcode.com" 。

## 2022-08-31
```sql
update teachers
set email = 'linghu.chong@lintcode.com'
where name = 'Linghu Chong';
```





# 2020 · 更新选择人工智能的学生人数
数据库
入门
通过率
78%
题目
题解
笔记
讨论
排名
记录
描述
请编写 SQL 语句，将课程表 courses 中人工智能课 (Artificial Intelligence) 的学生人数修改为 500 人。

## 2022-08-31
```sql
update courses
set student_count = 500
where name = 'Artificial Intelligence';
```




# 2037 · 查询 2020 年 8 月前的课程名和课程日期
数据库
入门
通过率
59%
题目
题解
笔记
讨论
排名
记录
描述
请编写 SQL 语句，从课程表 courses 中查询 2020 年 8 月前的课程名和创建日期，并为创建日期的列名起别名为 created_date (日期指 created_at 中不包括具体时间的部分)。

## 2022-08-31
```sql
SELECT name, DATE(created_at) as created_date
FROM courses
where created_at < "2020-08-01"
```



# 2040 · 查询教师 id 不为 3 且人数大于 800 的课程
## 2022-08-31
```sql
SELECT * 
from courses
where teacher_id != 3 and student_count > 800;
```

# 2056 · 将教师表中年龄大于 20 的数据复制到另一张表中预发布
## 2022-08-31
```sql
INSERT INTO `teachers_bkp`
SELECT * 
FROM `teachers`
WHERE `age` > 20
```


# 2072 · 查询年龄不大于 20 岁的教师所教的所有课程的课程名预发布
数据库
中等
通过率
76%
题目
题解
笔记
讨论
排名
记录
该题目为预发布题目，如遇到任何问题，请及时通过"题目纠错"联系我们，我们会升级您的账号为VIP作为感谢。
描述
请编写 SQL 语句， 联合教师表（teachers）和课程表（courses），查询课程表中年龄不大于 20 岁的教师所教的所有课程的课程名（name）。

## 2022-08-31
```sql
select courses.name
from teachers, courses
where teachers.id = courses.teacher_id and teachers.age <= 20;
```

