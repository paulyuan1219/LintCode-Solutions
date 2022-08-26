# 1164 · 摆动序列
算法
中等
通过率
57%
题目
题解
笔记
讨论
排名
记录
描述
如果连续数字之间的差严格地在正和负之间交替，则这样的数字序列称为摆动序列。 第一个差值（如果存在）可以是正的也可以是负的。 少于两个元素的序列通常是摆动序列。

例如，[1,7,4,9,2,5]是一个摆动序列，因为连续数字的差(6,-3,5,-7,3)交替为正和负。 相反，[1,4,7,2,5]和[1,7,4,5,5]不是摆动序列，第一个是因为它的前两个连续数字的差是正的，而第二个是因为它的最后一个连续数字的差零。

给定一个整数序列，返回作为摆动序列的最长子序列的长度。 通过从原始序列中删除一些元素（和0）来获得子序列，使剩余元素保持其原始顺序。

背完这套刷题模板，真的不一样！

北大学霸令狐冲15年刷题经验总结的《算法小抄模板Cheat Sheet》助你上岸！

微信添加【jiuzhang0607】备注【小抄】领取


样例
例1:

输入: [1,7,4,9,2,5]
输出: 6
解释: 整个序列都是一个摆动序列。
例2:

输入: [1,17,5,10,13,15,10,5,16,8]
输出: 7
解释: 有若干个摆动子序列都满足这个长度。 其中一个为[1,17,10,13,10,16,8]。
挑战
尝试用O(n)时间复杂度完成。

## 2022-08-25
仔细分析一下，感觉是从所有的可能性找找到一种就可以了，所以直觉是使用动态规划即可。

传统的动态规划只需要令`dp[i]`等于以位置i的元素为结尾的最长子序列即可。但是这里，我们还应该保存这个位置在i的元素本身到底是最长子序列中的哪个位置？可以是中间，也就是最长子序列的开头，也可以是peak，也可以是valley。所以我们用了一个额外的数组b。
+ `b[i] == 0`, this is the first element in LIS
+ `b[i] == 1`, this means it is a peak point
+ `b[i] == -1`, this means it is a valley point

```python
from typing import (
    List,
)

class Solution:
    """
    @param nums: the given sequence
    @return: the length of the longest subsequence that is a wiggle sequence
    """
    def wiggle_max_length(self, nums: List[int]) -> int:
        # Write your code here

        if not nums:
            return 0
        
        if len(nums) < 2:
            return 1
        
        n = len(nums)
        dp = [0] * n
        b = [0] * n 

        dp[0] = 1
        b[0] = 0

        if nums[1] == nums[0]:
            dp[1] = 1
            b[1] = 0
        elif nums[1] > nums[0]:
            dp[1] = 2
            b[1] = 1
        else:
            dp[1] = 2
            b[1] = -1
        
        for i in range(2, n):
            if nums[i] == nums[i - 1]:
                dp[i] = dp[i - 1]
                b[i] = b[i - 1]
                continue
            
            if nums[i - 1] > nums[i]:
                if b[i - 1] >= 0:
                    dp[i] = dp[i - 1] + 1
                    b[i] = -1
                else:
                    dp[i] = dp[i - 1]
                    b[i] = b[i - 1]
                
                continue
            
            # nums[i - 1] < nums[i]
            if b[i - 1] <= 0:
                dp[i] = dp[i - 1] + 1
                b[i] = 1
            else:
                dp[i] = dp[i - 1]
                b[i] = b[i - 1]
        
        return max(dp)

```

