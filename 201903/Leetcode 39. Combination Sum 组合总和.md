# Leetcode 39. Combination Sum 组合总和

标签： `Leetcode`

---

题目地址: https://leetcode-cn.com/problems/combination-sum/  
## 题目描述   

<p>给定一个<strong>无重复元素</strong>的数组&nbsp;<code>candidates</code>&nbsp;和一个目标数&nbsp;<code>target</code>&nbsp;，找出&nbsp;<code>candidates</code>&nbsp;中所有可以使数字和为&nbsp;<code>target</code>&nbsp;的组合。</p>

<p><code>candidates</code>&nbsp;中的数字可以无限制重复被选取。</p>

<p><strong>说明：</strong></p>

<ul>
	<li>所有数字（包括&nbsp;<code>target</code>）都是正整数。</li>
	<li>解集不能包含重复的组合。&nbsp;</li>
</ul>

<p><strong>示例&nbsp;1:</strong></p>

<pre><strong>输入:</strong> candidates = <code>[2,3,6,7], </code>target = <code>7</code>,
<strong>所求解集为:</strong>
[
  [7],
  [2,2,3]
]
</pre>

<p><strong>示例&nbsp;2:</strong></p>

<pre><strong>输入:</strong> candidates = [2,3,5]<code>, </code>target = 8,
<strong>所求解集为:</strong>
[
&nbsp; [2,2,2,2],
&nbsp; [2,3,3],
&nbsp; [3,5]
]</pre>  

## 算法思想   

一看到这种要对一个数组所有情况都要做考虑，肯定就是用递归做了，优先考虑dfs的方法，关键在于边界条件以及在如何进行递归的设计，我们可以把target传入到递归中，每次把target减去递归到的数值，那么边界就是target与0作比较，如果小于0，则说明没找到，return，如果等于0 ，说明是一个解，其他情况则继续进行递归遍历，而每次递归我们的候选值是从所有candidates里面选择任何一个，所以需要一个for循环。那么递归设计就完成了。其实所有dfs的代码都非常的像，但是有时候解释性又不是很好，所以记着这个套路，会用即可，典型的比如全排列问题。  

## python代码  

```python

import copy
class Solution(object):
    def dfs(self,start,candidates,target,out,res):
        # 不满足条件
        if target<0:
            return
        # 刚好满足
        if target ==0:
            res.append(copy.copy(out))
        # 从start开始遍历一遍，由于可以多个，所以是从start
        for i in range(start,len(candidates)):
            out.append(candidates[i])
            self.dfs(i,candidates,target - candidates[i],out,res)
            out.remove(candidates[i])

    def combinationSum(self, candidates, target):
        """
        :type candidates: List[int]
        :type target: int
        :rtype: List[List[int]]
        """
        res = []
        out = []
        candidates = sorted(candidates)
        self.dfs(0,candidates,target,out,res)

        return res
```  

参考： https://www.cnblogs.com/ariel-dreamland/p/9138630.html




