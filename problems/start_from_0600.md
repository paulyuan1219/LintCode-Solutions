# 627 · 最长回文串
算法
简单
通过率
45%
题目
题解
笔记
讨论
排名
记录
描述
给出一个包含大小写字母的字符串。求出由这些字母构成的最长的回文串的长度是多少。

数据是大小写敏感的，也就是说，"Aa" 并不会被认为是一个回文串。

背完这套刷题模板，真的不一样！

北大学霸令狐冲15年刷题经验总结的《算法小抄模板Cheat Sheet》助你上岸！

微信添加【jiuzhang0607】备注【小抄】领取


假设字符串的长度不会超过 100000。

样例
样例 1:

输入 : s = "abccccdd"
输出 : 7
说明 : 
一种可以构建出来的最长回文串方案是 "dccaccd"。
标签
企业

## 2022-08-31
java practice

```java
import java.util.*;
import java.util.Hashtable;


public class Solution {
    /**
     * @param s: a string which consists of lowercase or uppercase letters
     * @return: the length of the longest palindromes that can be built
     */
    public int longestPalindrome(String s) {
        // write your code here

        if (s == null){
            return 0;
        }

        if (s.length() <= 1){
            return s.length();
        }

        HashTable ht = HashTable();

        for(int i = 0; i < s.length(); i ++){
            char c = s.charAt(i);
            if (ht.containsKey(c)){
                ht.put(c, ht.get(c) + 1);
            } else{
                ht.put(c, 1);
            }
        }

        int ans = 0;
        boolean is_odd_met = false;

        Enumeration ks = ht.keys();
        while(ks.hasMoreElements()){
            char k = (char) ks.nextElement();
            int freq = ht.get(k);

            if (freq % 2 == 0){
                ans += freq;
                continue;
            }

            if (!is_odd_met){
                ans += freq;
                is_odd_met = true;
                continue;
            }

            ans += (freq - 1);
        }

        return ans;
    }
}


```
```bash
Compile Error
异常信息
/code/Solution.java:21: error: cannot find symbol
        HashTable ht = HashTable();
        ^
  symbol:   class HashTable
  location: class Solution
/code/Solution.java:21: error: cannot find symbol
        HashTable ht = HashTable();
                       ^
  symbol:   method HashTable()
  location: cl
```


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