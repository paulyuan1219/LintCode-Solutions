# 1428 · 钥匙和房间
算法
中等
通过率
64%
题目
题解
笔记
讨论
排名
记录
描述
有 N 个房间，开始时你位于 0 号房间。每个房间有不同的号码：0，1，2，...，N-1，并且房间里可能有一些钥匙能使你进入下一个房间。

在形式上，对于每个房间 i 都有一个钥匙列表 rooms[i]，每个钥匙 rooms[i][j]由 [0,1，...，N-1] 中的一个整数表示，其中 N = rooms.length。 钥匙 rooms[i][j] = v 可以打开编号为 v 的房间。

最初，除 0 号房间外的其余所有房间都被锁住。

你可以自由地在房间之间来回走动。

如果能进入每个房间返回 true，否则返回 false。

背完这套刷题模板，真的不一样！

北大学霸令狐冲15年刷题经验总结的《算法小抄模板Cheat Sheet》助你上岸！

微信添加【jiuzhang0607】备注【小抄】领取


1 \leq rooms.length \leq 10001≤rooms.length≤1000
0 \leq rooms[i].length \leq 10000≤rooms[i].length≤1000
所有房间中的钥匙数量总计不超过 3000。
样例
样例 1:

输入: rooms = [[1],[2],[3],[]]
输出: true
解释:  
我们从 0 号房间开始，拿到钥匙 1。
之后我们去 1 号房间，拿到钥匙 2。
然后我们去 2 号房间，拿到钥匙 3。
最后我们去了 3 号房间。
由于我们能够进入每个房间，我们返回 true。
样例 2:

输入: rooms = [[1,3],[3,0,1],[2],[0]]
输出: false
解释: 
我们不能进入 2 号房间。


## 2022-08-25
第一感觉就是连通性问题，所以用bfs就可以了，没什么难度。

```python
from typing import (
    List,
)

from collections import deque

class Solution:
    """
    @param rooms: a list of keys rooms[i]
    @return: can you enter every room
    """
    def can_visit_all_rooms(self, rooms: List[List[int]]) -> bool:
        # Write your code here

        n = len(rooms)

        if n == 1:
            return True
        
        queue = deque([0])
        visited = set([0])

        while len(queue) > 0:
            curr_room = queue.popleft()

            for next_room in rooms[curr_room]:
                if next_room in visited:
                    continue
                
                queue.append(next_room)
                visited.add(next_room)
        
        return len(visited) == n

```