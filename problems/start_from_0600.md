# 633 · 寻找重复的数
算法
困难
通过率
50%
题目
题解
笔记
讨论
排名
记录
描述
给出一个数组 nums 包含 n + 1 个整数，每个整数是从 1 到 n (包括边界)，保证至少存在一个重复的整数。假设只有一个重复的整数，找出这个重复的数。

背完这套刷题模板，真的不一样！

北大学霸令狐冲15年刷题经验总结的《算法小抄模板Cheat Sheet》助你上岸！

微信添加【jiuzhang0607】备注【小抄】领取


1.不能修改数组(假设数组只能读)
2.只能用额外的O(1)的空间
3.时间复杂度小于O(n^2)
4.数组中只有一个重复的数，但可能重复超过一次

样例
样例 1:

输入:
[5,5,4,3,2,1]
输出:
5
样例 2:

输入:
[5,4,4,3,2,1]
输出:
4

## 2022-08-25
根据提议，在最极端的情况下，应该可以把每个元素放到每个元素自己的位置

举例来说，对于`[5,5,4,3,2,1]`来说，`n=5`，所以位置1-5应该就可以放1-5.而位置0就可以放多余的那一个。所以，我们只需要不停的替换位置0处的元素即可。

```python
from typing import (
    List,
)

class Solution:
    """
    @param nums: an array containing n + 1 integers which is between 1 and n
    @return: the duplicate one
    """
    def find_duplicate(self, nums: List[int]) -> int:
        # write your code here

        while nums[0] != nums[nums[0]]:
            a = nums[0]
            b = nums[a]
            nums[0], nums[a] = b, a 
        
        return nums[0]
```