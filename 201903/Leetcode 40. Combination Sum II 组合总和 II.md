# Leetcode 40. Combination Sum II 组合总和 II

标签： `Leetcode`

---

题目地址：https://leetcode-cn.com/problems/combination-sum-ii/  

## 题目描述   

<p>给定一个数组&nbsp;<code>candidates</code>&nbsp;和一个目标数&nbsp;<code>target</code>&nbsp;，找出&nbsp;<code>candidates</code>&nbsp;中所有可以使数字和为&nbsp;<code>target</code>&nbsp;的组合。</p>

<p><code>candidates</code>&nbsp;中的每个数字在每个组合中只能使用一次。</p>

<p><strong>说明：</strong></p>

<ul>
	<li>所有数字（包括目标数）都是正整数。</li>
	<li>解集不能包含重复的组合。&nbsp;</li>
</ul>

<p><strong>示例&nbsp;1:</strong></p>

<pre><strong>输入:</strong> candidates =&nbsp;<code>[10,1,2,7,6,1,5]</code>, target =&nbsp;<code>8</code>,
<strong>所求解集为:</strong>
[
  [1, 7],
  [1, 2, 5],
  [2, 6],
  [1, 1, 6]
]
</pre>

<p><strong>示例&nbsp;2:</strong></p>

<pre><strong>输入:</strong> candidates =&nbsp;[2,5,2,1,2], target =&nbsp;5,
<strong>所求解集为:</strong>
[
&nbsp; [1,2,2],
&nbsp; [5]
]</pre>  


## 算法思想  

本题很类似`Leetcode 39`，只是做了更多的限制，比如每个数字只能用一次，还有就是解的集合里面不能有重复。  

所以关键在于在递归的时候注意进行设计，比如同一个位置不能重复使用，那么我们在进行dfs的时候要进行`i+1`，这样的话，就不会一直使用同一个位置的元素了，体现在`dfs(i+1,candidates,target-candidates[i],out,res)`这一句上面。  

而对于最终结果可能有重复来说，我们有两种解决方案，一种是在进行递归的时候就解决掉，我们在进行dfs之前对数组排序，所有我么在dfs中如果发现`i!=start and candidates[i-1]==candidates[i]`，就说明连续两个元素是相同的，比如`1 1 2 3 4`，如果target是6，那么我们通过第一个1可以得到两个结果` 1  1 4` 以及 `1 2 3` 而当我们遍历到下一个，因为还是1，所以结果肯定在第一个1里面，所以就跳过，在代码中体现在:  

    if i!=start and candidates[i-1]==candidates[i]:
        continue  
        
不过这种方式可能理解起来，或者不容易想到，所以还可以采用第二种，就是在得到结果后进行去重，这里就用到了对list去重的方法，因为这个是list里面还是list，所以不能直接用set进行去重，因为list unable hash，所以我们先把list里面的list转为tuple，然后再set，最后再转为list即可。   

## python代码  

### dfs里面去重   

```python

import copy
class Solution(object):
    def dfs(self,start,candidates,target,out,res):
        if target<0:
            return
        if target ==0:
            res.append(copy.copy(out))
        for i in range(start,len(candidates)):
            # candidates[i-1]==candidates[i] 避免 1 1 2 3 4 比如有了 1 2 4 避免第二个 1 2 4
            if i!=start and candidates[i-1]==candidates[i]:
                continue
            out.append(candidates[i])
            # i+1 避免对同一个位置多次取
            self.dfs(i+1,candidates,target-candidates[i],out,res)
            out.remove(candidates[i])
    def combinationSum2(self, candidates, target):
        """
        :type candidates: List[int]
        :type target: int
        :rtype: List[List[int]]
        """

        out = []
        res = []
        candidates = sorted(candidates)
        self.dfs(0,candidates,target,out,res)

        return res
```  

### 结果set去重  

```python  

import copy
class Solution(object):
    def dfs(self,start,candidates,target,out,res):
        if target<0:
            return
        if target ==0:
            res.append(copy.copy(out))
        for i in range(start,len(candidates)):
            # candidates[i-1]==candidates[i] 避免 1 1 2 3 4 比如有了 1 2 4 避免第二个 1 2 4
            # if i!=start and candidates[i-1]==candidates[i]:
            #     continue
            out.append(candidates[i])
            # i+1 避免对同一个位置多次取
            self.dfs(i+1,candidates,target-candidates[i],out,res)
            out.remove(candidates[i])
    def combinationSum2(self, candidates, target):
        """
        :type candidates: List[int]
        :type target: int
        :rtype: List[List[int]]
        """

        out = []
        res = []
        candidates = sorted(candidates)
        self.dfs(0,candidates,target,out,res)
        res = [tuple(x) for x in res]
        res = list(set(res))
        res = [list(x) for x in res]
        return res

```  

参考： https://blog.csdn.net/wenqiwenqi123/article/details/80196635







