# 1911 · 地图分析
算法
中等
通过率
48%
题目
题解
笔记
讨论
排名
记录
描述
你现在手里有一份大小为 N x N 的网格grid，上面的每个单元格都用 0 和 1 标记好了。其中 0 代表海洋，1 代表陆地，请你找出一个海洋单元格，这个海洋单元格到离它最近的陆地单元格的距离是最大的。
我们这里说的距离是「曼哈顿距离」（ Manhattan Distance）：(x0, y0) 和 (x1, y1) 这两个单元格之间的距离是 |x0 - x1| + |y0 - y1| 。
如果网格上只有陆地或者海洋，请返回 -1。

背完这套刷题模板，真的不一样！

北大学霸令狐冲15年刷题经验总结的《算法小抄模板Cheat Sheet》助你上岸！

微信添加【jiuzhang0607】备注【小抄】领取


1 <= grid.length == grid[0].length <= 100
grid[i][j] 不是 0 就是 1

样例
样例 1：

输入： 
[[1,0,1],[0,0,0],[1,0,1]]
输出： 
2
解释： 
海洋单元格 (1, 1) 和所有陆地单元格之间的距离都达到最大，最大距离为 2。
样例 2：

输入： 
[[1,0,0],[0,0,0],[0,0,0]]
输出： 
4
解释： 
海洋单元格 (2, 2) 和所有陆地单元格之间的距离都达到

## 2022-08-31
感觉上，用最简单的bfs可以做，就是对于每一个sea location，我们使用bfs去求一个最近的距离，然后所有的距离里面求最大即可。但是这样好像效率不高。


```python
from typing import (
    List,
)

from collections import deque   

delta = [(1, 0), (-1, 0), (0, 1), (0, -1)]

class Solution:
    """
    @param grid: An array.
    @return: An integer.
    """
    def max_distance(self, grid: List[List[int]]) -> int:
        # Write your code here.
        N = len(grid)
    
    def bfs(self, grid, N, x, y, history):
        # (x, y) must be a sea location 
        queue = deque([(x, y)])
        visited = set([(x, y)])
        distance = 1

        while len(queue) > 0:
            x, y = queue.popleft()
            for dx, dy in delta:
                x_, y_ = x + dx, y + dy 

                if not (0 <= x_ < N and 0 <= y_ < N):
                    continue
                
                if grid[x_][y_] == 1:
                    

        


```


# 1996 · 判断教师是否拥有邮箱
数据库
入门
通过率
71%
题目
题解
笔记
讨论
排名
记录
描述
请编写 SQL 语句，对教师表 teachers 中教师是否拥有邮箱进行判断，最后返回教师姓名和邮箱，以及使用函数 ISNULL 、IFNULL 、COALESCE 判断结果。

表定义: teachers (教师表)

## 2022-08-31
```sql
SELECT `name`, `email`, ISNULL(`email`) AS `isnull_email`
	, IFNULL(`email`, 0) AS `ifnull_email`
	, COALESCE(`email`, 0) AS `coalesce_email`
FROM `teachers`;
```
