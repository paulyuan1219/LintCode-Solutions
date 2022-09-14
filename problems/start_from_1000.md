# 1006 · 子域名访问计数
算法
简单
通过率
70%
题目
题解
笔记
讨论
排名
记录
描述
诸如discuss.lintcode.com这样的域名由各种子域名构成。最顶层是com，下一层是lintcode.com，最底层是discuss.lintcode.com.当访问discuss.lintcode.com时，会隐式访问子域名lintcode.com和com.

现给出域名的访问计数格式为“空格 地址”。 示例：9001 discuss.lintcode.com.

给出计数列表cpdomains. 返回每个子域名（包含父域名）的访问次数（与输入格式相同，顺序任意）.

背完这套刷题模板，真的不一样！

北大学霸令狐冲15年刷题经验总结的《算法小抄模板Cheat Sheet》助你上岸！

微信添加【jiuzhang0607】备注【小抄】领取


cpdomains的长度不超过100.
每个域名的长度不超过100.
会有1到2个.包含在每个域名中.
任何域名的访问计数都不会超过10000.
返回结果的条目顺序随意.
样例
样例 1:

输入： 
["9001 discuss.lintcode.com"]
输出： 
["9001 discuss.lintcode.com", "9001 lintcode.com", "9001 com"]
解释： 
只有一个域名："discuss.lintcode.com". 如题所述，  
子域名"lintcode.com"和"com"也会被访问. 所以一共要访问9001次.

## 2022-09-02

通过这道题，可以掌握python defaultdict的用法

```python
from collections import defaultdict

class Solution:
    """
    @param cpdomains: a list cpdomains of count-paired domains
    @return: a list of count-paired domains
             we will sort your return value in output
    """
    def subdomain_visits(self, cpdomains: List[str]) -> List[str]:
        # Write your code here
        ht = defaultdict(lambda: 0)

        for record in cpdomains:
            fields = record.split(' ')
            count = int(fields[0])
            domain_parts = fields[1].split('.')
            n = len(domain_parts)

            for i in range(n):
                domain = ".".join(domain_parts[i:])
                ht[domain] += count
        
        ans = []
        for domain, count in ht.items():
            ans.append(f"{count} {domain}")
        
        return ans
```

接下来自己写了一个java的版本，居然报错

```java
public class Solution {
    /**
     * @param cpdomains: a list cpdomains of count-paired domains
     * @return: a list of count-paired domains
     *          we will sort your return value in output
     */
    public List<String> subdomainVisits(String[] cpdomains) {
        // Write your code here
        Map<String, Integer> ht = new HashMap<String, Integer>();

        for( String line: cpdomains){
            String[] fields = line.split(" ");
            int count = Integer.parseInt(fields[0]);
            String[] domain_parts = fields[1].split("."); // "\\."
            int n = domain_parts.length;

            for(int i = 0; i < n; i ++){
                String domain = String.join(".", Arrays.copyOfRange(domain_parts, i, n));

                if(ht.containsKey(domain)){
                    ht.put(domain, ht.get(domain) + count);
                } else {
                    ht.put(domain, count);
                }
            }
        }

        List<String> ans = new ArrayList<String>();

        for(Map.Entry<String, Integer> entry: ht.entrySet()){
            String tmp_line = String.format("%d %s", entry.getValue(), entry.getKey());
            ans.add(tmp_line);
        }

        return ans;
    }
}
```
```bash
Wrong Answer
输入数据
["9001 discuss.lintcode.com"]
输出数据
[]

期望答案
["9001 com","9001 discuss.lintcode.com","9001 lintcode.com"]
提示
Review your code and make sure your algorithm is correct. Wrong answer usually caused by typos if your algorithm is correct.
```
好吧，仔细看了一下，原来是split时候的字符需要转义，加上这个就对了。

好吧，这样发现java还是要多多练习啊。

