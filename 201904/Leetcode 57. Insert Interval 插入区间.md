# Leetcode 57. Insert Interval 插入区间

标签： `Leetcode`

---

题目地址： https://leetcode-cn.com/problems/insert-interval/  

## 题目描述    

<p>给出一个<em>无重叠的 ，</em>按照区间起始端点排序的区间列表。</p>

<p>在列表中插入一个新的区间，你需要确保列表中的区间仍然有序且不重叠（如果有必要的话，可以合并区间）。</p>

<p><strong>示例&nbsp;1:</strong></p>

<pre><strong>输入:</strong> intervals = [[1,3],[6,9]], newInterval = [2,5]
<strong>输出:</strong> [[1,5],[6,9]]
</pre>

<p><strong>示例&nbsp;2:</strong></p>

<pre><strong>输入:</strong> intervals = <code>[[1,2],[3,5],[6,7],[8,10],[12,16]]</code>, newInterval = <code>[4,8]</code>
<strong>输出:</strong> [[1,2],[3,10],[12,16]]
<strong>解释:</strong> 这是因为新的区间 <code>[4,8]</code> 与 <code>[3,5],[6,7],[8,10]</code>&nbsp;重叠。
</pre>

## 算法思想  

其实这道题和之前的[合并区间][1]非常类似，之前的合并区间也是先排序，然后再相邻的合并，这里是已经排序好了，所以我们关键在找到新区间应该插入的位置，这里我们以start为基准进行二分查找，找到的位置就是应该插入的位置，之后我们把这个新区间插入应该的位置，然后向后扩展，向前扩展即可。注意的是合并后要把后面和前面的元素删除掉。   


## python代码 

```python

class Solution(object):
    def midSearch(self,intervals,newInterval):
        left = 0
        right = len(intervals)-1
        while left <=right:
            mid = (left + right)//2
            if intervals[mid][0] == newInterval[0]:
                return mid
            elif intervals[mid][0] <newInterval[0]:
                left = mid+1
            else:
                right = mid -1
        return right + 1

    def insert(self, intervals, newInterval):
        """
        :type intervals: List[Interval]
        :type newInterval: Interval
        :rtype: List[Interval]
        """
        if len(intervals)==0:
            return [newInterval]
        i = self.midSearch(intervals,newInterval)
        intervals.insert(i,newInterval)
        # 向后合并
        for tmp in intervals[i+1:]:
            if newInterval[1] < tmp[0]:
                break
            else:
                # 合并删除
                newInterval[1] = max(tmp[1],newInterval[1]) # 向后合并，只需要考虑end那个，因为start是已经排过序的
                intervals.remove(tmp)

        # 向前合并
        for tmp in intervals[-(len(intervals)-i+1)::-1]:
            if tmp[1] < newInterval[0]:
                break
            else:
                newInterval[1] = max(tmp[1],newInterval[1])
                newInterval[0] = tmp[0]# 需要更新下start，因为tmp的start肯定比new的小
                intervals.remove(tmp)
        return intervals
```


  [1]: https://blog.csdn.net/qq_28888837/article/details/89526641