# Leetcode 18. 4Sum 四数之和

标签： `Leetcode`

---

题目地址：https://leetcode-cn.com/problems/4sum/  

## 题目描述  

<p>给定一个包含&nbsp;<em>n</em> 个整数的数组&nbsp;<code>nums</code>&nbsp;和一个目标值&nbsp;<code>target</code>，判断&nbsp;<code>nums</code>&nbsp;中是否存在四个元素 <em>a，</em><em>b，c</em>&nbsp;和 <em>d</em>&nbsp;，使得&nbsp;<em>a</em> + <em>b</em> + <em>c</em> + <em>d</em>&nbsp;的值与&nbsp;<code>target</code>&nbsp;相等？找出所有满足条件且不重复的四元组。</p>

<p><strong>注意：</strong></p>

<p>答案中不可以包含重复的四元组。</p>

<p><strong>示例：</strong></p>

<pre>给定数组 nums = [1, 0, -1, 0, -2, 2]，和 target = 0。

满足要求的四元组集合为：
[
  [-1,  0, 0, 1],
  [-2, -1, 1, 2],
  [-2,  0, 0, 2]
]
</pre>  

## 算法思想  

这道题和三数之和是非常类似，其实还是双指针问题，可以先进行排序，然后把第一个数定着，后面的三个数就可以转换成三数之和的问题了，和三数之和的思想是一样的，可以参考https://blog.csdn.net/qq_28888837/article/details/87693792 这篇文章。  

## python代码  
```python

class Solution(object):
    def fourSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[List[int]]
        """
        nums.sort()
        res = []
        for i in range(0,len(nums)-3):
            if i>0 and nums[i]==nums[i-1]:
                continue
            for left in range(i+1,len(nums)-2):
                tmp_target = target -nums[i]
                if left>i+1 and nums[left-1] ==nums[left]:
                    continue
                mid = left+1
                right = len(nums)-1
                while mid<right:
                    if nums[mid]+nums[right]+nums[left]==tmp_target:
                        res.append([nums[i],nums[left],nums[mid],nums[right]])
                        mid+=1
                        right-=1
                        while mid<right and nums[mid-1]==nums[mid]:mid+=1
                        while mid<right and nums[right]==nums[right+1]:right-=1
                    elif nums[mid]+nums[right]+nums[left]<tmp_target:
                        mid+=1
                    else:
                        right-=1

        return res

```




