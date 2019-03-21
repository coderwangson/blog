# Leetcode 34. Find First and Last Position of Element in Sorted Array 在排序数组中查找元素的第一个和最后一个位置

标签： `Leetcode`

---

题目地址：https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/  

## 题目描述  

<p>给定一个按照升序排列的整数数组 <code>nums</code>，和一个目标值 <code>target</code>。找出给定目标值在数组中的开始位置和结束位置。</p>

<p>你的算法时间复杂度必须是&nbsp;<em>O</em>(log <em>n</em>) 级别。</p>

<p>如果数组中不存在目标值，返回&nbsp;<code>[-1, -1]</code>。</p>

<p><strong>示例 1:</strong></p>

<pre><strong>输入:</strong> nums = [<code>5,7,7,8,8,10]</code>, target = 8
<strong>输出:</strong> [3,4]</pre>

<p><strong>示例&nbsp;2:</strong></p>

<pre><strong>输入:</strong> nums = [<code>5,7,7,8,8,10]</code>, target = 6
<strong>输出:</strong> [-1,-1]</pre>  

## 算法思想 

只要看到`O(logn)`，对于这种搜索的题目，肯定就是二分查找了，所以我们可以先通过二分查找找到一个值等于`target`的，然后用这个值把数组一分为二，我们对左右分别在进行二分查找，这次二分查找的目的是找到两个边界。比如对于左边来说，我们要找到一个`mid`使得`nums[mid]!=target`但是`nums[mid]+1==target`的，这样就找到了左边界。  

但是在这个处理的过程中可能会有问题，比如`333333`,加入我们的target是3，那么我们就找不到一个`nums[mid]!=target`但是`nums[mid]+1==target`的，所以这个时候我们就根据情况进行确定，具体看注释。  


## python代码  

```python
class Solution(object):
    def searchRange(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        start = 0
        end = len(nums) -1
        while start<=end:
            mid = (start+end)//2
            if target == nums[mid]:
                break
            elif target<nums[mid]:
                end = mid-1
            else:
                start = mid+1
        # 如果没有进if
        else:
            return [-1,-1]
        # 如果没有进上面的else，则说明进了if的break，则说明有戏
        # 则再次进行二分对左右都进行二分
        # 左边则找一个元素，这个元素不是target，但是这个元素的下个元素是target
        left = 0
        right = mid-1
        res = []
        while left <=right:
            mid = (right+left) //2
            if nums[mid]!=target and nums[mid+1] ==target:
                break
            # 因为nums[mid]是不可能>target的，所以就这两种情况
            if nums[mid]<target:
                left = mid+1
            elif nums[mid] ==target:
                right = mid-1
        # 说明没有找到一个nums[mid]!=target and nums[mid+1] ==target,那就是mid为边界了
        if left>right:
            res.append(mid)
        # 说明有满足条件的，那么mid+1为边界
        else:
            res.append(mid+1)
        left = mid+1
        right = len(nums) -1
        while left<=right:
            mid = (left+right) //2
            if nums[mid]!=target and nums[mid-1]==target:
                break
            if nums[mid] >target:
                right= mid-1
            elif nums[mid]==target:
                left = mid+1
        if left>right:
            res.append(mid)
        else:
            res.append(mid-1)
        return res
```




