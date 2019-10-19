# Leetcode 81. Search in Rotated Sorted Array II 搜索旋转排序数组 II

标签： `Leetcode`

---

题目地址:  https://leetcode-cn.com/problems/search-in-rotated-sorted-array-ii/  

## 题目描述  

<p>假设按照升序排序的数组在预先未知的某个点上进行了旋转。</p>

<p>( 例如，数组&nbsp;<code>[0,0,1,2,2,5,6]</code>&nbsp;可能变为&nbsp;<code>[2,5,6,0,0,1,2]</code>&nbsp;)。</p>

<p>编写一个函数来判断给定的目标值是否存在于数组中。若存在返回&nbsp;<code>true</code>，否则返回&nbsp;<code>false</code>。</p>

<p><strong>示例&nbsp;1:</strong></p>

<pre><strong>输入:</strong> nums = [2<code>,5,6,0,0,1,2]</code>, target = 0
<strong>输出:</strong> true
</pre>

<p><strong>示例&nbsp;2:</strong></p>

<pre><strong>输入:</strong> nums = [2<code>,5,6,0,0,1,2]</code>, target = 3
<strong>输出:</strong> false</pre>

<p><strong>进阶:</strong></p>

<ul>
	<li>这是 <a href="https://leetcode-cn.com/problems/search-in-rotated-sorted-array/description/">搜索旋转排序数组</a>&nbsp;的延伸题目，本题中的&nbsp;<code>nums</code>&nbsp; 可能包含重复元素。</li>
	<li>这会影响到程序的时间复杂度吗？会有怎样的影响，为什么？</li>
</ul>  

## 算法思想  

这道题目类似于之前的Leetcode33，也是采取二分法，也要先判断是左边有序还是右边有序。但是由于这个题目会有重复，所以会出现`1 3 1 1 1`这样的数组，虽然`nums[start]<=nums[mid]`但是左边并没有序，所以为了解决这个问题，再前面加了一个过滤，如果`nums[start]==nums[mid]`就对start+=1，跳过该开始。   

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
                return True
            if nums[start] ==nums[mid]:# 解决1 3 1 1 1有问题 原来33题，关键在于确定一个有序，一个无序，但对于 1 3 1 1 1问题，会出现start == mid，但无序，所以先排除
                start +=1
                continue
            # 说明左半部分有序是递增，则先看target是否在左半边
            if nums[start]<nums[mid] :
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

        return False

```





