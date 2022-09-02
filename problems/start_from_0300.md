# 389 · 判断数独是否合法
算法
简单
通过率
33%
题目
题解
笔记
讨论
排名
记录
描述
请判定一个数独是否有效。

该数独可能只填充了部分数字，其中缺少的数字用 . 表示。

背完这套刷题模板，真的不一样！

北大学霸令狐冲15年刷题经验总结的《算法小抄模板Cheat Sheet》助你上岸！

微信添加【jiuzhang0607】备注【小抄】领取


一个合法的数独（仅部分填充）并不一定是可解的。我们仅需使填充的空格有效即可。
什么是 数独？

http://sudoku.com.au/TheRules.aspx
http://baike.baidu.com/subview/961/10842669.htm
样例
样例1:

输入:
["53..7....","6..195...",".98....6.","8...6...3","4..8.3..1","7...2...6",".6....28.","...419..5","....8..79"]
输出: true
样例说明: 
这个数独如下图所示，他是合法的。
图片

样例 2:

输入:
["53..75...","6..195...",".98....6.","8...6...3","4..8.3..1","7...2...6",".6....28.","...419..5","....8..79"]
输出: false
样例说明: 
这个数独如下图所示。他是不合法的因为他的第一行和第六列有两个5。
图片

## 2022-08-25
感觉不难，按照row, col, grid分别检验就可以了，但是第一次写居然报错？

```python
from typing import (
    List,
)

from collections import Counter

class Solution:
    """
    @param board: the board
    @return: whether the Sudoku is valid
    """
    def is_valid_sudoku(self, board: List[List[str]]) -> bool:
        # write your code here

        for index in range(9):
            tmp_list = self.get_elements_by_row(board, index)
            if not self.is_list_valid(board, tmp_list):
                return False

            tmp_list = self.get_elements_by_col(board, index)
            if not self.is_list_valid(board, tmp_list):
                return False
 
            tmp_list = self.get_elements_by_grid(board, index)
            if not self.is_list_valid(board, tmp_list):
                return False
 
        return True

    def get_elements_by_row(self, board, index):
        return board[index]

    def get_elements_by_col(self, board, index):
        return [board[_][index] for _ in range(len(board))]
    
    def get_elements_by_grid(self, board, index):
        row_base = (index // 3) * 3
        col_base = (index % 3) * 3

        tmp_list = []
        for i in range(row_base, row_base + 3):
            for j in range(col_base, col_base + 3):
                tmp_list.append(board[i][j])
        
        return tmp_list

    def is_list_valid(self, board, tmp_list):
        tmp_counter = Counter(tmp_list)

        for k, v in tmp_counter.items():
            if v >= 2:
                return False
        
        return True

        

```
```bash
Wrong Answer
0%
81 ms
时间消耗
·
6.00 MB
空间消耗
·
输入数据
["53..7....","6..195...",".98....6.","8...6...3","4..8.3..1","7...2...6",".6....28.","...419..5","....8..79"]
输出数据
false
期望答案
true

提示
Review your code and make sure your algorithm is correct. Wrong answer usually caused by typos if your algorithm is correct.
```

+ 好吧，上面的代码里面把`.`也加进去了，其实只需要在counter的时候不考虑这些符号即可。

改成下面的代码即可

```python
    def is_list_valid(self, board, tmp_list):
        tmp_counter = Counter(tmp_list)

        for k, v in tmp_counter.items():
            if k == '.':
                continue
            if v >= 2:
                return False
        
        return True
```