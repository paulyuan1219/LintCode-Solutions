# 3241 · 查询 Web 课程的学生数量预发布
数据库
入门
通过率
74%
题目
题解
笔记
讨论
排名
记录
该题目为预发布题目，如遇到任何问题，请及时通过"题目纠错"联系我们，我们会升级您的账号为VIP作为感谢。
描述
请你编写 SQL 语句，查询 coursers 表中课程名为 Web 的学生数量。

## 2022-08-31

```sql
select student_count
from courses
where name = 'Web';
```

