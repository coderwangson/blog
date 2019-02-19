# Leetcode 15. 3Sum 三数之和

标签（空格分隔）： `Leetcode`

---
题目地址：https://leetcode-cn.com/problems/3sum/   

## 题目描述

<p>给定一个包含 <em>n</em> 个整数的数组&nbsp;<code>nums</code>，判断&nbsp;<code>nums</code>&nbsp;中是否存在三个元素 <em>a，b，c ，</em>使得&nbsp;<em>a + b + c = </em>0 ？找出所有满足条件且不重复的三元组。</p>

<p><strong>注意：</strong>答案中不可以包含重复的三元组。</p>

<pre>例如, 给定数组 nums = [-1, 0, 1, 2, -1, -4]，

满足要求的三元组集合为：
[
  [-1, 0, 1],
  [-1, -1, 2]
]
</pre>  

## 算法思想   

这道题原来我想着是先固定一个，然后再使用两数之和做，但是会超时，所以便更改策略。这个题难点有两个，一个是寻找相加为0的三个数，另一个是不能重复。  

对于寻找三个相加为0的数字，可以直接暴力，但是同样效率也低，所以看到网上有先排序，然后固定一个，另外两个使用for循环从两头向中间进发。用代码可以这样表示：
```python
 while left<len(nums):
    # 定义mid和right向中间计算
    mid = left+1
    right = l-1
    while mid<right:
        # 满足三个数的和为0
        if nums[mid]+nums[right]==0-nums[left]:
            add()
        elif nums[mid]+nums[right]<0-nums[left]:
            mid+=1
        else:
            right-=1
        left+=1
```
可以看出就是固定一个（left），变二（mid，right），之所以能够从mid到right这样，是因为数组已经排好序了，所以才会如果`nums[mid]+nums[right]<0-nums[left]`, `mid+=1`。这段代码还有个细节就是如果`nums[left]>0`了，也可以直接结束循环，因为后面的肯定全部大于0，那么和不可能等于0的。  

但是这样还是不能去重复，刚开始想的去重复是存在一个set里面，虽然可以，但是网上有更好的方案，就是边处理边去重。  

在数组有序情况下，重复分两种重复。  

第一种是这一轮的left和上一轮的left的值一样，比如数组`[-1,-1,0,1,1]`,如果上一个循环从第一个-1开始，那么会得到有一个方案`[-1,0,1]`，而下一次循环会到第二个-1，那么由于都是-1，所以固定的第一个一样，而后面的情况也是一样的，所以如果第二个-1有满足条件的和，其和第一个-1一定会重复。  

第二种重复是对于mid和right来说的，如果当前找到了一对mid和right，而mid+1的值和mid的是同的，那么其也会重复。比如数组`[0,0,0,0,0]`,加入left = 0，mid = 1,right = 4,而mid+1的值和mid是同的，所以即使再出来一个right能和为0，其一定和上面的重复了（因为right只能为0），所以要把这些情况给去除掉。   

## python代码

```python
class Solution:
    def threeSum(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        # nums中三个数的和是0
        res = []
        nums = sorted(nums)
        left = 0
        l =len(nums)
        # left可以取所有的位置，和为0至少要有一个负数
        while left<l and nums[left]<=0:
            # 第一种重复如果当前的和上个是一样的，那么跳过，因为会出现重复
            # 比如 [-1,-1,0,1,1]第一个-1能和后面0,1结成，同样第二个-1的组合会是一样的
            if left>0 and nums[left]==nums[left-1]:
                left += 1
                continue
            # 定义mid和right向中间计算
            mid = left+1
            right = l-1
            while mid<right:
                # 满足三个数的和为0
                if nums[mid]+nums[right]==0-nums[left]:
                    res.append([nums[left],nums[mid],nums[right]])
                    mid+=1
                    right-=1
                    # 第二种重复
                #如果mid+1和mid一样，那么计算出来的结果一定同所以过滤掉
                    # 如 [0,0,0,0,0]
                    while mid<right and nums[mid]==nums[mid-1]:mid+=1
                    while mid<right and nums[right]==nums[right+1]:right-=1
                elif nums[mid]+nums[right]<0-nums[left]:
                    mid+=1
                else:
                    right-=1
            left+=1
        return res
```
## 参考  
[http://www.cnblogs.com/voidsky/p/5490801.html](http://www.cnblogs.com/voidsky/p/5490801.html)


