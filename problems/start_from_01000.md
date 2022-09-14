# 161 · 旋转图像
算法
中等
通过率
41%
题目
题解
笔记
讨论
排名
记录
描述
给定一个N×N的二维矩阵表示图像，将图像顺时针旋转90度。



背完这套刷题模板，真的不一样！

北大学霸令狐冲15年刷题经验总结的《算法小抄模板Cheat Sheet》助你上岸！

微信添加【jiuzhang0607】备注【小抄】领取


样例
样例1：

输入:[[1,2],[3,4]]
输出:[[3,1],[4,2]]
样例 2:

输入:[[1,2,3],[4,5,6],[7,8,9]]
输出:[[7,4,1],[8,5,2],[9,6,3]]
挑战
能否在原地完成？

## 2022-09-14

Do a little bit math, we find that this operation can be achieved by 1) flip the matrix along the horizontal middle line and 2) flip the matrix along the diagnal line. That's it.

```python
from typing import (
    List,
)

class Solution:
    """
    @param matrix: a lists of integers
    @return: nothing
    """
    def rotate(self, matrix: List[List[int]]):
        # write your code here
        if not matrix:
            return 
        
        if not matrix[0]:
            return
        
        N = len(matrix)

        self.helper1(matrix, N)
        self.helper2(matrix, N)
    
    def swap(self, matrix, x1, y1, x2, y2):
        matrix[x1][y1], matrix[x2][y2] = matrix[x2][y2], matrix[x1][y1]

    def helper1(self, matrix, N):
        M = N // 2

        for i in range(M):
            for j in range(N):
                self.swap(matrix, i, j, N - 1 - i, j)

    def helper2(self, matrix, N):
        for i in range(N - 1):
            for j in range(i + 1, N):
                self.swap(matrix, i, j, j, i)


```
