# Leetcode 33. Search in Rotated Sorted Array  搜索旋转排序数组

标签： `Leetcode`

---

题目地址：https://leetcode-cn.com/problems/search-in-rotated-sorted-array/  

## 题目描述  

<p>假设按照升序排序的数组在预先未知的某个点上进行了旋转。</p>

<p>( 例如，数组&nbsp;<code>[0,1,2,4,5,6,7]</code>&nbsp;可能变为&nbsp;<code>[4,5,6,7,0,1,2]</code>&nbsp;)。</p>

<p>搜索一个给定的目标值，如果数组中存在这个目标值，则返回它的索引，否则返回&nbsp;<code>-1</code>&nbsp;。</p>

<p>你可以假设数组中不存在重复的元素。</p>

<p>你的算法时间复杂度必须是&nbsp;<em>O</em>(log&nbsp;<em>n</em>) 级别。</p>

<p><strong>示例 1:</strong></p>

<pre><strong>输入:</strong> nums = [<code>4,5,6,7,0,1,2]</code>, target = 0
<strong>输出:</strong> 4
</pre>

<p><strong>示例&nbsp;2:</strong></p>

<pre><strong>输入:</strong> nums = [<code>4,5,6,7,0,1,2]</code>, target = 3
<strong>输出:</strong> -1</pre>  

## 算法思想  

这道题我使用的是`python`做的，其实我直接遍历一遍循环，虽然复杂度到达了`O(n)`但是仍然通过了，所以如果只是想通过，则直接单层for循环即可。  

而这道题目明确要求了要在`O(logn)`复杂度，而对于查找来说，如果想要达到这个复杂度，可定要使用二分查找，只是这里的二分有点小技巧，要注意判断。  

通过题目描述我们知道其实就是把一个有序的经过了旋转，而通过旋转的结果能够看出就是把一个数组变成了两个升序数组，所以我们如果进行二分的话,`mid`划分后两个数组的情况肯定是一个是升序，一个是乱序。  

比如`4567823`，如果二分，则mid在的位置是7，所以左边是有序，而右边是无序。所以我们就看`target`是不是在**有序**的这一边，如果是的话，就直接按照二分做即可，如果不是，我们则更新下一个边界即可。  

## python代码
 
```python  

class Solution(object):
    def search(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        # 进行旋转，其实就是变为了两个递增序列，所以我们如果进行二分，则
        # 有两种情况，一种是刚好在一个递增序列中，一种是跨了两个序列，我们
        # 主要处理在递增序列的情况
        # ----->---|-->
        # ----|--->--->
        start = 0
        end = len(nums) -1
        while start<=end:
            mid = (start+end)//2
            if nums[mid] == target:
                return mid
            # 说明左半部分有序是递增，则先看target是否在左半边
            if nums[start]<=nums[mid]:
                # 说明目标在左半递增的那边，就是正常二分，更新end
                if nums[start]<=target and target<nums[mid]:
                    end = mid-1
                # 说明不是在做半边递增那边
                else:
                    start = mid+1
            # 左半边无序的话，那么右半边一定有序，先看target是否在右半边
            else:
                #看target是否在右半边
                if nums[mid]<target and target<=nums[end]:
                    start = mid+1
                else:
                    end = 0 if mid-1<0 else mid-1

        return -1

```  

## 参考  

https://blog.csdn.net/whdAlive/article/details/80432797





