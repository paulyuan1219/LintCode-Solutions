# 1307 · 验证二叉搜索树中的前序序列
算法
中等
通过率
54%
题目
题解
笔记
讨论
排名
记录
描述
给定一组数字，验证它是否是二叉搜索树的正确的前序遍历序列。

您可以假设序列中的每个数字都是唯一的。

跟进：
你能在常数的空间复杂度内完成吗？

背完这套刷题模板，真的不一样！

北大学霸令狐冲15年刷题经验总结的《算法小抄模板Cheat Sheet》助你上岸！

微信添加【jiuzhang0607】备注【小抄】领取


样例
样例1

输入: preorder = [1,3,2]
输出: true
样例2

输入: preorder = [1,2]
输出: true

## 2022-08-31
首先参考了下面的笔记，感觉懂了 https://www.cnblogs.com/grandyang/p/5327635.html

其先序遍历的结果是 {5, 2, 1, 3, 6}，先设一个最小值 low，然后遍历数组，如果当前值小于这个最小值 low，返回 false，对于根节点，将其压入栈中，然后往后遍历，如果遇到的数字比栈顶元素小，说明是其左子树的点，继续压入栈中，直到遇到的数字比栈顶元素大，那么就是右边的值了，需要找到是哪个节点的右子树，所以更新 low 值并删掉栈顶元素，然后继续和下一个栈顶元素比较，如果还是大于，则继续更新 low 值和删掉栈顶，直到栈为空或者当前栈顶元素大于当前值停止，压入当前值，这样如果遍历完整个数组之前都没有返回 false 的话，最后返回 true 即可，

但是写出来的代码有错。

```python
class Solution:
    # @param {integer[]} preorder
    # @return {boolean}
    def verifyPreorder(self, preorder):
        if not preorder:
            return True 
        
        if len(preorder) <= 2:
            return True

        low = float('-inf')
        stack = [preorder[0]]

        for i in range(1, len(preorder)):
            if not stack or preorder[i] < stack[-1]:
                stack.append(preorder[i])
                continue
            
            if preorder[i] <= low:
                return False
            while len(stack) > 0 and stack[-1] < preorder[i]:
                low = stack[-1]
                stack.pop()

        return True
        

```
```bash
Wrong Answer
64%
102 ms
时间消耗
·
6.68 MB
空间消耗
·
输入数据
[10,7,4,8,6,40,23]
输出数据
true
期望答案
false

```

好吧，还是上面的代码，把几个语句顺序改一下，就对了，说明刚刚的解答还是没有完全看懂 哈哈

```python
class Solution:
    # @param {integer[]} preorder
    # @return {boolean}
    def verifyPreorder(self, preorder):
        if not preorder:
            return True 
        
        if len(preorder) <= 2:
            return True

        low = float('-inf')
        stack = [preorder[0]]

        for i in range(1, len(preorder)):
            if preorder[i] <= low:
                return False

            while len(stack) > 0 and stack[-1] < preorder[i]:
                low = stack[-1]
                stack.pop()

            if not stack or preorder[i] < stack[-1]:
                stack.append(preorder[i])
                continue
            

        return True
```

现在感觉理解了，但是这个是单调栈的解法，正常情况是想不到的。。。

下面这种方法使用了分治法，跟之前那道验证二叉搜索树的题 Validate Binary Search Tree 的思路很类似，在递归函数中维护一个下界 lower 和上界 upper，那么当前遍历到的节点值必须在 (lower, upper) 区间之内，然后在给定的区间内搜第一个大于当前节点值的点，以此为分界，左右两部分分别调用递归函数，注意左半部分的 upper 更新为当前节点值 val，表明左子树的节点值都必须小于当前节点值，而右半部分的递归的 lower 更新为当前节点值 val，表明右子树的节点值都必须大于当前节点值，如果左右两部分的返回结果均为真，则整体返回真，参见代码如下

```c++
class Solution {
public:
    /**
     * @param preorder: List[int]
     * @return: return a boolean
     */
    bool verifyPreorder(vector<int>& preorder) {
        return helper(preorder, 0, preorder.size() - 1, INT_MIN, INT_MAX);
    }
    bool helper(vector<int>& preorder, int start, int end, int lower, int upper) {
        if (start > end) return true;
        int val = preorder[start], i = 0;
        if (val <= lower || val >= upper) return false;
        for (i = start + 1; i <= end; ++i) {
            if (preorder[i] >= val) break;
        }
        return helper(preorder, start + 1, i - 1, lower, val) && helper(preorder, i, end, val, upper);
    }
};
```

一样的代码，我写成python看看行不行, it does not pass
```python
class Solution:
    # @param {integer[]} preorder
    # @return {boolean}
    def verifyPreorder(self, preorder):
        if not preorder:
            return True 
        
        if len(preorder) <= 2:
            return True

        return self.helper(preorder, 0, len(preorder) - 1, float('-inf'), float('inf'))


    def helper(self, preorder, start, end, lower_bound, upper_bound):
        if start >= end: # it should be start > end
            return True 
        
        pivot = preorder[start]

        if pivot <= lower_bound or pivot >= upper_bound:
            return False
        
        i = start + 1
        while i <= end:
            if preorder[i] > pivot:
                break 
            i += 1
        
        return self.helper(preorder, start + 1, i - 1, lower_bound, pivot) and self.helper(preorder, i, end, pivot, upper_bound)
    

```
```bash
Wrong Answer
64%
1114 ms
时间消耗
·
13.83 MB
空间消耗
·
输入数据
[10,7,4,8,6,40,23]
输出数据
true
期望答案
false
```

+ 好吧，对照了一下，就是递归的出口的一个判断错了，应该改成`start > end`
+ 改了以后，会超时，这个就是python本身的问题了，呵呵

# 1311 · 二叉搜索树的最近公共祖先
算法
简单
通过率
67%
题目
题解
笔记
讨论
排名
记录
描述
给定一个二叉搜索树, 找到该树中两个指定节点的最近公共祖先。

百度百科中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”

例如，给定如下二叉搜索树: root ={6,2,8,0,4,7,9,#,#,3,5}

图片

背完这套刷题模板，真的不一样！

北大学霸令狐冲15年刷题经验总结的《算法小抄模板Cheat Sheet》助你上岸！

微信添加【jiuzhang0607】备注【小抄】领取


所有节点的值都是唯一的。
p、q 为不同节点且均存在于给定的二叉搜索树中。
样例
样例 1:

输入:
{6,2,8,0,4,7,9,#,#,3,5}
2
8
输出: 6 
解释: 节点 2 和节点 8 的最近公共祖先是 6。
样例 2:

输入:
{6,2,8,0,4,7,9,#,#,3,5}
2
4
输出: 2
解释: 节点 2 和节点 4 的最近公共祖先是 2, 因为根据定义最近公共祖先节点可以为节点


## 2022-08-25

第一感觉不难，因为所有的节点都存在，而且值都是unique的，所以只要分别找到p和q的路径，然后两个路径求交集即可。

```python
"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None
"""

class Solution:
    """
    @param root: root of the tree
    @param p: the node p
    @param q: the node q
    @return: find the LCA of p and q
    """
    def lowestCommonAncestor(self, root, p, q):
        # write your code here

        if root == p or root == q:
            return root 
        
        path_p = [root]
        path_q = [root]

        self.getPath(root, p, path_p)
        self.getPath(root, q, path_q)  

        n = min(len(path_p), len(path_q))

        for i in range(n - 1):
            if path_p[i] == path_q[i] and path_p[i + 1] != path_q[i + 1]:
                return path_p[i]
            
        return path_p[n - 1]

    def getPath(self, root, target, path):
        if root == target:
            return 

        if root.val > target.val:
            path.append(root.left)
            self.getPath(root.left, target, path)
        else:
            path.append(root.right)
            self.getPath(root.right, target, path)        
```



# 1353 · 根节点到叶节点求和
算法
中等
通过率
73%
题目
题解
笔记
讨论
排名
记录
描述
给定仅包含来自0-9的数字的二叉树，每个根到叶路径可以表示数字。举个例子：root-to-leaf路径1-> 2-> 3，它代表数字123，找到所有根到叶的数的总和

背完这套刷题模板，真的不一样！

北大学霸令狐冲15年刷题经验总结的《算法小抄模板Cheat Sheet》助你上岸！

微信添加【jiuzhang0607】备注【小抄】领取


叶节点是没有子节点的节点

样例
样例1

输入: {1,2,3}
输出: 25
解释:
    1
   / \
  2   3
路径 1->2 表示数字 12.
路径 1->3 表示数字 13.
因此, sum = 12 + 13 = 25.
样例2

输入: {4,9,0,5,1}
输出: 1026
解释:
    4
   / \
  9   0
 / \
5   1
路径 4->9->5 表示数字 495.
路径 4->9->1 表示数字 491.
路径 4->0 表示数字 40.
因此, sum = 495 + 491 + 40 = 1026.

## 2022-08-25
感觉就是套用dfs就可以了。但是第一次居然写错了？

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
    @param root: the root of the tree
    @return: the total sum of all root-to-leaf numbers
    """
    def sum_numbers(self, root: TreeNode) -> int:
        # write your code here
        path = []
        paths = []
        self.dfs(root, path, paths)
        return sum(paths)

    def dfs(self, root, path, paths):
        if not root:
            paths.append(int(''.join(path)))
            return 
        
        path.append(str(root.val))
        self.dfs(root.left, path, paths)
        self.dfs(root.right, path, paths)
        path.pop()

```
```bash
Wrong Answer
0%
81 ms
时间消耗
·
5.92 MB
空间消耗
·
输入数据
{1,2,3}
输出数据
50
期望答案
25

提示
Review your code and make sure your algorithm is correct. Wrong answer usually caused by typos if your algorithm is correct.
```

+ ok，瞬间反应过来了，当root为空的时候，我们就加一次，所以对于叶子结点，我们其实对他的左右空孩子都做了一次加法，所以最终结果都是多加了一倍。
+ 所以只要针对叶子结点专门处理就可以了。

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
    @param root: the root of the tree
    @return: the total sum of all root-to-leaf numbers
    """
    def sum_numbers(self, root: TreeNode) -> int:
        # write your code here
        if not root:
            return 0
            
        path = []
        paths = []
        self.dfs(root, path, paths)
        return sum(paths)

    def dfs(self, root, path, paths):
        if root.left is None and root.right is None:
            paths.append(int(''.join(path)))
            return 
        
        if root.left:
            path.append(str(root.val))
            self.dfs(root.left, path, paths)
            path.pop()

        if root.right:
            path.append(str(root.val))
            self.dfs(root.right, path, paths)
            path.pop()

```
```bash
Wrong Answer
0%
81 ms
时间消耗
·
6.05 MB
空间消耗
·
输入数据
{1,2,3}
输出数据
2
期望答案
25

```

+ 奇怪啊。
+ 好吧，其实就是在遇到叶子结点的时候，我们忘记加入当前叶子结点的value了。小bug

```python
        if root.left is None and root.right is None:
            path.append(str(root.val))
            paths.append(int(''.join(path)))
            path.pop()
            return 
```

Data Science Developer, Software Developer, DevOps & Machine Learning

