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

