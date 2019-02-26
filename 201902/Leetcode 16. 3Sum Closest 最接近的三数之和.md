# Leetcode 16. 3Sum Closest 最接近的三数之和

标签： `Leetcode`

---

题目地址：https://leetcode-cn.com/problems/3sum-closest/  

## 题目描述  
<p>给定一个包括&nbsp;<em>n</em> 个整数的数组&nbsp;<code>nums</code><em>&nbsp;</em>和 一个目标值&nbsp;<code>target</code>。找出&nbsp;<code>nums</code><em>&nbsp;</em>中的三个整数，使得它们的和与&nbsp;<code>target</code>&nbsp;最接近。返回这三个数的和。假定每组输入只存在唯一答案。</p>

<pre>例如，给定数组 nums = [-1，2，1，-4], 和 target = 1.

与 target 最接近的三个数的和为 2. (-1 + 2 + 1 = 2).
</pre>  

## 算法思想  

这道题和Leetcode 15三数之和非常类似，并且我觉得由于保证了结果的唯一性，所以不用去重，所以更加简单。同样的先对数组排序，这样能保证我们在搜索和的时候能够有策略的搜素。外层循环用一个left作为定的，内层循环用mid和right作为动的，用这三个的和与`target`进行比较，如果比之前的距离更小，那么我们就更新，如果距离为0，那么就可以退出程序了。如果三个的和比target大，那么我们的策略就是把right进行减一，如果三个数的和比target小，那么策略就是把letf加一。这样相当于把所有结果都进行了搜索，只是在搜索的时候有策略的搜索，而不是单纯的三层`for`循环。   

## python代码  

```python

class Solution(object):
    def threeSumClosest(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        nums = sorted(nums)
        Min = nums[0]+nums[1]+nums[2]
        for left in range(0,len(nums)-2):
            mid = left+1
            right = len(nums)-1
            while mid<right:
                tmp = nums[left]+nums[mid]+nums[right]
                if abs(target -tmp)<abs(target-Min):
                    Min = tmp
                if target ==Min:
                    return Min
                if tmp <target:
                    mid+=1
                else:
                    right-=1
        return Min
        
```






