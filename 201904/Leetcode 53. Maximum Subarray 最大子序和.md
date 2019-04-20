# Leetcode 53. Maximum Subarray 最大子序和

标签： `Leetcode`

---

题目地址：https://leetcode-cn.com/problems/maximum-subarray/  

## 题目描述  

<p>给定一个整数数组 <code>nums</code>&nbsp;，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。</p>

<p><strong>示例:</strong></p>

<pre><strong>输入:</strong> [-2,1,-3,4,-1,2,1,-5,4],
<strong>输出:</strong> 6
<strong>解释:</strong>&nbsp;连续子数组&nbsp;[4,-1,2,1] 的和最大，为&nbsp;6。
</pre>

<p><strong>进阶:</strong></p>

<p>如果你已经实现复杂度为 O(<em>n</em>) 的解法，尝试使用更为精妙的分治法求解。</p>


## 算法思想   

### 暴力法  

应为要找一个最大的子序列和，我们就用双重for循环去找，外层i代表起点，j代表终点，这样找到最大的即可，但是使用python出现超时。  

### 扫描法  

使用扫描法只需要一遍遍历即可，先把nums[0]置位当前最大的，当前累加的记为curr，然后开始遍历nums，主要是看curr，如果curr<0的话，我们就不应该把curr继续累加下去，因为`无论下一个数字是什么，加上一个负数肯定会比之前小的`，所以如果curr<0我们就把curr赋值为当前的值，否则加上当前的数，然后比较当前的累加和最大的，找出当前最大。   

### 动态规划法 

既然利用动态规划，必须要有一个专一矩阵，这里我们定义sums[]为一个转移矩阵，定义sums[i]代表以`第i个为结尾`的最大子序列和。注意是以第i个为结尾，所以`最后一个元素一定是nums[i]`。所以`sums[i] = max(nums[i],sums[i-1]+nums[i])`(因为最后肯定是nums[i]，所以最后的结果要么是只有nums[i],要么就是前面的最大和加上当前的nums[i])，其实和前面的扫描法就很类似了，这里的意思也就是看sums[i-1]是否大于0，其实这里的sums数组也可以不要，因为每次只需要上一个元素，所以用一个元素保留即可，这里为了便于理解，还是作为数组。    

## python 代码  

### 暴力法   

```python
class Solution(object):
    def maxSubArray(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        max_sum = nums[0]
        for i in range(len(nums)):
            sum = 0
            for j in range(i,len(nums)):
                sum+=nums[j]
                if sum>max_sum:
                    max_sum = sum
        return max_sum
```

### 扫描法

```python
class Solution(object):
    def maxSubArray(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        curr = nums[0]
        max_sum = nums[0]
        for i in nums[1:]:
            if curr<0:
                curr= i
            else:
                curr +=i
            if curr>max_sum:max_sum = curr
        return max_sum

```

### 动态规划法   

```python
class Solution(object):
    def maxSubArray_with_sums(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        sums = []
        max_sum = nums[0]
        sums.append(nums[0]) # sums[0] = nums[0]
        for i in range(1,len(nums)):
            t = max(sums[i-1]+nums[i],nums[i])
            if t>max_sum:
                max_sum = t
            sums.append(t)
        return max_sum
    # 不用sums数组
    def maxSubArray(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        max_sum = nums[0]
        sums = nums[0] # 把sums看做sums[i-1]因为只需要sums[i-1]的信息
        for i in range(1,len(nums)):
            t = max(sums+nums[i],nums[i])
            if t>max_sum:
                max_sum = t
            sums = t
        return max_sum
```  

参考：  

https://blog.csdn.net/zwzsdy/article/details/80029796



