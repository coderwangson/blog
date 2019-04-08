# Leetcode 55. Jump Game 跳跃游戏

标签： `Leetcode`

---

题目地址：https://leetcode-cn.com/problems/jump-game/  

## 题目描述  

<p>给定一个非负整数数组，你最初位于数组的第一个位置。</p>

<p>数组中的每个元素代表你在该位置可以跳跃的最大长度。</p>

<p>判断你是否能够到达最后一个位置。</p>

<p><strong>示例&nbsp;1:</strong></p>

<pre><strong>输入:</strong> [2,3,1,1,4]
<strong>输出:</strong> true
<strong>解释:</strong> 从位置 0 到 1 跳 1 步, 然后跳 3 步到达最后一个位置。
</pre>

<p><strong>示例&nbsp;2:</strong></p>

<pre><strong>输入:</strong> [3,2,1,0,4]
<strong>输出:</strong> false
<strong>解释:</strong> 无论怎样，你总会到达索引为 3 的位置。但该位置的最大跳跃长度是 0 ， 所以你永远不可能到达最后一个位置。
</pre>  

## 算法思想  

这里用到了贪心的思想，因为我们只是看看是否能够到达最后一个位置，所以我们用一个`reach`代表当前能到达的最远位置，然后我们比那里nums，如果我们遍历到下标`i`的时候`i>reach`，说明i不可达，所以跳出，否则我们更新reach为`max(reach,i+nums[i])`，这样代表当前在i的位置时我能跳的最远的位置。最终我们看reach是否能够跳到最后的位置即可。  

## python代码  

```python  

class Solution(object):
    def canJump(self, nums):
        """
        :type nums: List[int]
        :rtype: bool
        """
        reach = 0
        for i in range(len(nums)):
            if i>reach:break
            reach = max(reach,i+nums[i])

        return reach>=len(nums)-1
```  

参考：http://www.cnblogs.com/grandyang/p/4371526.html




