# Leetcode 56. Merge Intervals 合并区间

标签： `Leetcode`

--- 

题目地址：  https://leetcode-cn.com/problems/merge-intervals/

## 题目描述  

<p>给出一个区间的集合，请合并所有重叠的区间。</p>

<p><strong>示例 1:</strong></p>

<pre><strong>输入:</strong> [[1,3],[2,6],[8,10],[15,18]]
<strong>输出:</strong> [[1,6],[8,10],[15,18]]
<strong>解释:</strong> 区间 [1,3] 和 [2,6] 重叠, 将它们合并为 [1,6].
</pre>

<p><strong>示例&nbsp;2:</strong></p>

<pre><strong>输入:</strong> [[1,4],[4,5]]
<strong>输出:</strong> [[1,5]]
<strong>解释:</strong> 区间 [1,4] 和 [4,5] 可被视为重叠区间。</pre>  

## 算法思想  

先把interval按照`start`进行排序，首先用一个tmp存储`intervals[0]`,然后遍历`intervals`看是否`tmp.end>=i.start`，因为已经按照start排过序，如果`tmp.end>=i.start`,说明二者相交，则更新tmp的`end`，否则说明不相交，那么后面也不可能有相交的，所以直接把tmp添加到结果中，并且更新tmp。  


## python代码 

```python
class Solution(object):

    def merge(self, intervals):
        """
        :type intervals: List[Interval]
        :rtype: List[Interval]
        """
        if intervals ==[]:return []
        # 先排序
        intervals.sort(key = lambda p:p.start)
        res = []
        tmp = intervals[0]
        for i in intervals:
            if tmp.end>=i.start:
                tmp.end = max(i.end,tmp.end)
            else:
                res.append(tmp)
                tmp = i
        res.append(tmp)
        return res

```



