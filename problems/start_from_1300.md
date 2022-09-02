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

## 2022-08-29
感觉上不难，分治法就可以了，但是好像直接写会超时

```python
from typing import (
    List,
)

class Solution:
    """
    @param preorder: List[int]
    @return: return a boolean
    """
    def verify_preorder(self, preorder: List[int]) -> bool:
        # write your code here
        if not preorder:
            return True
        
        return self.helper(preorder, 0, len(preorder) - 1)
    
    def helper(self, nums, start, end):
        if start >= end:
            return True 
        
        if start + 1 == end:
            return True
        

        index = start + 1
        while index <= end and nums[start] > nums[index]:
            index += 1
        
        if index > end:
            return self.helper(nums, start + 1, end)
        
        if min(nums[index:end + 1]) < nums[start]:
            return False 
        
        return self.helper(nums, start + 1, index - 1) and self.helper(nums, index, end)

```
下面这个解答应该是对的，慢慢看

```python
"""
思路1：暴力法，遍历当前待检查数组，找到第一个大于数组起始位置的位置i，则 i 为右子树根节点，然后递归判断左右子树。对于根节点，最坏情况下要遍历整个数组，时间为O（N），因为要递归检查每个节点，因此总的时间复杂度是O（N^2）。

我们先设一个最小值low，然后遍历数组，如果当前值小于这个最小值low，返回false，对于根节点，我们将其压入栈中，然后往后遍历，如果遇到的数字比栈顶元素小，说明是其左子树的点，继续压入栈中，直到遇到的数字比栈顶元素大，那么就是右边的值了，我们需要找到是哪个节点的右子树，所以我们更新low值并删掉栈顶元素，然后继续和下一个栈顶元素比较，如果还是大于，则继续更新low值和删掉栈顶，直到栈为空或者当前栈顶元素大于当前值停止，压入当前值，这样如果遍历完整个数组之前都没有返回false的话，最后返回true即可

...
下面这种方法使用了分治法，跟之前那道验证二叉搜索树的题Validate Binary Search Tree的思路很类似，我们在递归函数中维护一个下界lower和上届upper，那么当前遍历到的节点值必须在(lower, upper)区间之内，然后我们在给定的区间内搜第一个大于当前节点值的点，然后以此为分界，左右两部分分别调用递归函数，注意左半部分的upper更新为当前节点值val，表明左子树的节点值都必须小于当前节点值，而右半部分的递归的lower更新为当前节点值val，表明右子树的节点值都必须大于当前节点值，如果左右两部分的返回结果均为真，则整体返回真，
"""
```

我按照这个思路快速写了一个，居然报错

```python
from typing import (
    List,
)

class Solution:
    """
    @param preorder: List[int]
    @return: return a boolean
    """
    def verify_preorder(self, preorder: List[int]) -> bool:
        # write your code here
        if not preorder:
            return True
        
        return self.helper(preorder, 0, len(preorder) - 1, float('-inf'), float('inf'))
    
    def helper(self, nums, start, end, lower_bound, upper_bound):
        if start > end:
            return True

        if start == end:
            if lower_bound < nums[start] < upper_bound:
                return True
            return False
        
        if start + 1 == end:
            if lower_bound < min(nums[start], nums[end]) and max(nums[start], nums[end]) < upper_bound:
                return True
            return False
        
        index = start + 1
        while index <= end and nums[start] > nums[index]:
            index += 1
        
        if index > end:
            return self.helper(nums, start + 1, end, lower_bound, nums[start])
        
        return self.helper(nums, start + 1, index - 1, lower_bound, nums[start]) and self.helper(nums, index, end, nums[start], upper_bound)
```


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


debug了一下，下面的代码还是不行，会超时。。。

```python
class Solution:
    """
    @param preorder: List[int]
    @return: return a boolean
    """
    def verify_preorder(self, preorder: List[int]) -> bool:
        # write your code here
        if not preorder:
            return True
        
        return self.helper(preorder, 0, len(preorder) - 1, float('-inf'), float('inf'))
    
    def helper(self, nums, start, end, lower_bound, upper_bound):
        if start > end:
            return True

        if start == end:
            if lower_bound < nums[start] < upper_bound:
                return True
            return False
        
        if start + 1 == end:
            if lower_bound < min(nums[start], nums[end]) and max(nums[start], nums[end]) < upper_bound:
                return True
            return False
        
        index = start + 1
        while index <= end and nums[start] > nums[index]:
            if nums[index] < lower_bound:
                return False
            index += 1
        
        if index > end:
            return self.helper(nums, start + 1, end, lower_bound, nums[start])
        
        return self.helper(nums, start + 1, index - 1, lower_bound, nums[start]) and self.helper(nums, index, end, nums[start], upper_bound)
```
```bash
Time Limit Exceeded
85%
1114 ms
时间消耗
·
13.76 MB
空间消耗
·
输入数据下载 输入数据
size: 38889
md5: 0e86a0f3de374e44de49f0f3818a8d73
--- data is too long, we shortened it for display, here is preview ---
[7999,7998,7997,7996,7995,7994,7993,7992,7991,7990,7989,7988,7987,7986,7985,7984,7983,7982,7981,7980,7979,7978,7977,7976,7975,7974,7973,7972,7971,7970,7969,7968,7967,7966,7965,7964,7963,7962,7961,7960,7959,7958,7957,7956,7955,7954,7953,7952,7951,7950,7949,7948,7947,7946,7945,7944,7943,7942,7941,7940,7939,7938,7937,7936,7935,7934,7933,7932,7931,7930,7929,7928,7927,7926,7925,7924,7923,7922,7921,7920,7919,7918,7917,7916,7915,7914,7913,7912,7911,7910,7909,7908,7907,7906,7905,7904,7903,7902,7901,7900,7899,7898,7897,7896,7895,7894,7893,7892,7891,7890,7889,7888,7887,7886,7885,7884,7883,7882,7881,7880,7879,7878,7877,7876,7875,7874,7873,7872,7871,7870,7869,7868,7867,7866,7865,7864,7863,7862,7861,7860,7859,7858,7857,7856,7855,7854,7853,7852,7851,7850,7849,7848,7847,7846,7845,7844,7843,7842,7841,7840,7839,7838,7837,7836,7835,7834,7833,7832,7831,7830,7829,7828,7827,7826,7825,7824,7823,7822,7821,7820,7819,7818,7817,7816,7815,7814,7813,7812,7811,7810,7809,7808,7807,7806,7805,7804,7803,7802,7801,7800,7799,7798,7797,7796,779 ...
期望答案
true

提示
Your code ran too much time than we expected. Check your time complexity. Time limit exceeded usually caused by infinite loop if your time complexity is the best.
```

# 1310 · 数组除了自身的乘积
算法
中等
通过率
75%
题目
题解
笔记
讨论
排名
记录
描述
给定 n 个整数的数组 nums，其中 n > 1，返回一个数组输出，使得 output [i] 等于 nums 的所有除了 nums [i] 的元素的乘积。

背完这套刷题模板，真的不一样！

北大学霸令狐冲15年刷题经验总结的《算法小抄模板Cheat Sheet》助你上岸！

微信添加【jiuzhang0607】备注【小抄】领取


在不使用除法和O(n)时间内解决

样例
样例1

输入: [1,2,3,4]
输出: [24,12,8,6]
解释:
2*3*4=24
1*3*4=12
1*2*4=8
1*2*3=6
样例2

输入: [2,3,8]
输出: [24,16,6]
解释:
3*8=24
2*8=16
2*3=6
挑战
你可以用常数空间复杂度来解决这个问题吗？（注意：出于空间复杂度分析的目的，输出数组不算作额外空间）

标签
企业

## 2022-08-29

这道题以前做过，本以为很简单，没想到大意了

```python
from typing import (
    List,
)

class Solution:
    """
    @param nums: an array of integers
    @return: the product of all the elements of nums except nums[i].
    """
    def product_except_self(self, nums: List[int]) -> List[int]:
        # write your code here
        if not nums:
            return []
        
        if len(nums) == 1:
            return nums
        
        n = len(nums)
        ans = [0] * n 


        left_product = 1
        right_product = 1
        for num in nums:
            right_product = right_product * num
        
        
        for i in range(n):
            right_product = int(right_product / nums[i])
            ans[i] = left_product * right_product
            left_product = left_product* nums[i]
        
        return ans
```
+ 上面的代码为了追求空间复杂度，直接计算左右两边的乘积
+ 上面的代码会遇到除以0的问题
+ 第一次写的时候，没有使用int，导致输出会是小数
+ 所以稳妥起见，还是使用两个额外数组比较安全
+ 下面的代码一次过，木有问题。

```python
class Solution:
    """
    @param nums: an array of integers
    @return: the product of all the elements of nums except nums[i].
    """
    def product_except_self(self, nums: List[int]) -> List[int]:
        # write your code here
        if not nums:
            return []
        
        if len(nums) == 1:
            return nums
        
        n = len(nums)
        ans = [0] * n 
        left = [1] * n 
        right = [1] * n

        for i in range(1, n):
            left[i] = left[i - 1] * nums[i - 1]
        
        for i in range(n - 2, -1, -1):
            right[i] = right[i + 1] * nums[i + 1]
        
        for i in range(n):
            ans[i] = left[i] * right[i]
        
        return ans
```

参考了一个答案，发现直接用一个额外数组也可以做，整体的思路是一样的 呵呵

```python
class Solution:
    """
    @param nums: an array of integers
    @return: the product of all the elements of nums except nums[i].
    """
    def product_except_self(self, nums: List[int]) -> List[int]:
        # write your code here
        if not nums:
            return []
        
        if len(nums) == 1:
            return nums
        
        n = len(nums)
        ans = [1] * n 

        for i in range(1, n):
            ans[i] = ans[i - 1] * nums[i - 1]
        
        R = 1
        for i in range(n - 2, -1, -1):
            R = R * nums[i + 1]
            ans[i] = ans[i] * R
        
        return ans

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

