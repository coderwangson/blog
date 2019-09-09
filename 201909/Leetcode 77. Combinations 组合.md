# Leetcode 77. Combinations 组合

标签 ： `Leetcode`

---
题目地址：  https://leetcode-cn.com/problems/combinations/
## 题目描述  
<div>
<p>给定两个整数 <em>n</em> 和 <em>k</em>，返回 1 ... <em>n </em>中所有可能的 <em>k</em> 个数的组合。</p>

<p><strong>示例:</strong></p>

<pre><strong>输入:</strong>&nbsp;n = 4, k = 2
<strong>输出:</strong>
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]</pre>
</div>  

## 算法思想  

这个主要利用回溯法进行处理，我们用`current`数组保存当前遍历到的，如果`len(current) ==k`,那就说明当前已经够了，否则，则把当前的节点加入到`current`里面，外面的for循环的起始是在原有基础上加一，说明了之前遍历过的不能遍历，并且保证没有重复比如`1 3`和`3 1`.  

## python代码  

```
class Solution(object):
    def dfs(self,i,n,k,res,current):
        if len(current) == k:
            res.append(copy.copy((current)))
        else:
            for tmp in range(i,n+1):
                current.append(tmp)
                self.dfs(tmp+1,n,k,res,current)
                current.pop()

    def combine(self, n, k):
        """
        :type n: int
        :type k: int
        :rtype: List[List[int]]
        """
        res = []
        self.dfs(1,n,k,res,[])
        return res
```




