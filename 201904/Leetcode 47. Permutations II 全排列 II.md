# Leetcode 47. Permutations II 全排列 II

标签： `Leetcode`

---

题目地址：https://leetcode-cn.com/problems/permutations-ii/  
## 题目描述  

<p>给定一个可包含重复数字的序列，返回所有不重复的全排列。</p>

<p><strong>示例:</strong></p>

<pre><strong>输入:</strong> [1,1,2]
<strong>输出:</strong>
[
  [1,1,2],
  [1,2,1],
  [2,1,1]
]</pre>  

## 算法思想  

其实这道题和前面的组合数类似，只是多了个去重的操作，这个去重的操作其实你可以先对数组排序，让后遍历的时候注意一点，类似于之前的四个数之和或者组合总数来去重，但是在操作的时候发现有问题，不过可以参考我组合总和的博客的dfs去重的方法，这里我采用的是直接set去重，所以就是直接前面的代码，然后加一个set去重即可。  

## python代码  

```python
import copy
class Solution(object):
    def perm(self,a,start,res):
        if start==len(a):
            res.append(copy.copy(a))
        for i in range(start,len(a)):
            a[start],a[i] = a[i],a[start]
            self.perm(a,start+1,res)
            a[start],a[i] = a[i],a[start]

    def permuteUnique(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        res = []
        self.perm(nums,0,res)
        res = [tuple(x) for x in res]
        res = list(set(res))
        res = [list(x) for x in res]
        return res
```



