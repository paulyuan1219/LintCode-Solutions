# 730 · 所有子集的和
算法
简单
通过率
35%
题目
题解
笔记
讨论
排名
记录
描述
给一整数 n, 我们需要求前n个自然数形成的集合的所有可能子集中所有元素的和。

背完这套刷题模板，真的不一样！

北大学霸令狐冲15年刷题经验总结的《算法小抄模板Cheat Sheet》助你上岸！

微信添加【jiuzhang0607】备注【小抄】领取


样例
样例 1:

输入 : n = 2
输出 : 6
说明 : 
可能的子集为 {{1}, {2}, {1, 2}}. 
子集的元素和为 1 + 2 + 1 + 2 = 6
样例 2:

输入 : n = 3
输出 : 24
说明 :
可能的子集为 {{1}, {2}, {3}, {1, 2}, {1, 3}, {2, 3}, {1, 2, 3}}
子集的和为:
1 + 2 + 3 + (1 + 2) + (1 + 3) + (2 + 3) + (1 + 2 + 3) = 24
标签

## 2022-08-31

虽然是提到了全子集，但是用dfs做明显是不对的。分析后发现，给定n个数字，一共有$2^n$个子集。对于每一个单独的数字而言，正好出现在一半的子集中。换句话说，每一个单独的数字，它的出现的次数是$2^(n-1)$。所以简单的数学计算就可以解题

```python
class Solution:
    """
    @param n: the given number
    @return: Sum of elements in subsets
    """
    def sub_sum(self, n: int) -> int:
        # write your code here
        if n <= 0:
            return 0
        
        freq = 2 ** (n - 1)
        
        ans = 0
        for i in range(1, n + 1):
            ans += freq * i

        return ans
```