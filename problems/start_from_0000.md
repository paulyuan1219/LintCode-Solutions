# 14 · 二分查找
算法
简单
通过率
39%
题目
题解
笔记
讨论
排名
记录
描述
给定一个排序的整数数组（升序）和一个要查找的整数 target，用O(logn)O(logn)的时间查找到target第一次出现的下标（从0开始），如果target不存在于数组中，返回-1。

背完这套刷题模板，真的不一样！

北大学霸令狐冲15年刷题经验总结的《算法小抄模板Cheat Sheet》助你上岸！

微信添加【jiuzhang0607】备注【小抄】领取


样例
样例 1：

输入：

数组 = [1,4,4,5,7,7,8,9,9,10]
target = 1
输出：

0
解释：

第一次出现在第0个位置。

样例 2：

输入：

数组 = [1, 2, 3, 3, 4, 5, 10]
target = 3
输出：

2
解释：

第一次出现在第2个位置

样例 3：

输入：

数组 = [1, 2, 3, 3, 4, 5, 10]
target = 6
输出：

-1
解释：

没有出现过6， 返回-1

挑战
如果数组中的整数个数超过了2^{32}2 
32，你的算法是否会出错？

## 2022-08-31
java practice

```java
public class Solution {
    /**
     * @param nums: The integer array.
     * @param target: Target to find.
     * @return: The first position of target. Position starts from 0.
     */
    public int binarySearch(int[] nums, int target) {
        // write your code here
        if (nums == null || nums.length == 0){
            return -1;
        }

        int start = 0;
        int end = nums.length - 1;

        while (start + 1 < end){
            int mid = start + (end -start) / 2;

            if (nums[mid] == target){
                end = mid;
            }
            else if (nums[mid] < target){
                start = mid;
            }
            else{
                end = mid;
            }
        }

        if(nums[start] == target){
            return start;
        }

        if(nums[end] == target){
            return end;
        }

        return -1;
    }

}
```

# 95 · 验证二叉查找树
算法
中等
通过率
28%
题目
题解
笔记
讨论
排名
记录
描述
给定一个二叉树，判断它是否是合法的二叉查找树(BST)

一棵BST定义为：

节点的左子树中的值要严格小于该节点的值。
节点的右子树中的值要严格大于该节点的值。
左右子树也必须是二叉查找树。
一个节点的树也是二叉查找树。
背完这套刷题模板，真的不一样！

北大学霸令狐冲15年刷题经验总结的《算法小抄模板Cheat Sheet》助你上岸！

微信添加【jiuzhang0607】备注【小抄】领取


样例
样例 1：

输入：

tree = {-1}
输出：

true
解释：

二叉树如下(仅有一个节点）:
        -1
这是二叉查找树。
样例 2：

输入：

tree = {2,1,4,#,#,3,5}
输出：

true
解释：

        二叉树如下：
          2
         / \
        1   4
           / \
          3   5
这是二叉查找树。

## 2022-08-22
经典的分治法就可以做，没难度

```python
from lintcode import (
    TreeNode,
)

"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None
"""

class Solution:
    """
    @param root: The root of binary tree.
    @return: True if the binary tree is BST, or false
    """
    def is_valid_b_s_t(self, root: TreeNode) -> bool:
        # write your code here
        if not root:
            return True 
        
        ans_result, ans_min, ans_right = self.helper(root)
        return ans_result

    
    def helper(self, root):
        if not root:
            return (True, float("-inf"), float("inf"))
        
        left_result, left_min, left_max = self.helper(root.left)
        right_result, right_min, right_max = self.helper(root.right)

        if not left_result or not right_result:
            return (False, float("-inf"), float("inf"))
        
        # here, we know both left and right are True 

        # check left child 
        if left_max != float("inf"):
            if left_max >= root.val:
                return (False, float("-inf"), float("inf"))
        else:
            left_min = root.val 
        
        if right_min != float("-inf"):
            if right_min <= root.val:
                return (False, float("-inf"), float("inf"))
        else:
            right_max = root.val 
        
        return (True, left_min, right_max)

```

