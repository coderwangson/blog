# Leetcode 41. First Missing Positive 缺失的第一个正数

标签： `Leetcode`

---

题目地址：https://leetcode-cn.com/problems/first-missing-positive/  

## 题目描述  

<p>给定一个未排序的整数数组，找出其中没有出现的最小的正整数。</p>

<p><strong>示例&nbsp;1:</strong></p>

<pre>输入: [1,2,0]
输出: 3
</pre>

<p><strong>示例&nbsp;2:</strong></p>

<pre>输入: [3,4,-1,1]
输出: 2
</pre>

<p><strong>示例&nbsp;3:</strong></p>

<pre>输入: [7,8,9,11,12]
输出: 1
</pre>

<p><strong>说明:</strong></p>

<p>你的算法的时间复杂度应为O(<em>n</em>)，并且只能使用常数级别的空间。</p>  

## 算法思想  

这道题其实刚开始真心看不懂，不过后来在一篇blog上看到，别人说把这个问题看做为桶排序问题即可，由于我之前了解过桶排序，所以一下子就明白了。  

你想，如果正常的情况下整数都是连续，那么排序后 `a[0] = 1`,`a[1] = 2 ..`，发现有一个规律，就是`a[i] = i+1`，所以这个就是这道题的精髓，而所谓的排序我们可以借助桶排序的思想，（因为我们最后的答案肯定是1~len(nums)，为1 就不说原因了，因为最小正整数，而len是因为，加入我们有一个结果超过了len，那就意味这我们这串数字是从 1 -len，而长度只有len，所以矛盾。）  

所以我们从头遍历这个数组，然后我们看是否nums[i] ==i+1如果不等，我们则看下nums[i]里面的值，把这个值赋到其应该有的位置，而我们前面说了`a[i] = i+1`,所以`a[i-1] = i`，即`nums[nums[i]-1] = nums[i]`，所以我们一直这样把nums[i]里面的值赋到其应该在的位置，并且在这个过程中也给nums[i]寻找其对应的值，而因为可能数组里面有重复值，所以在遇见nums[nums[i]-1]本身等于nums[i],这样即使交换也没意义，所以就直接把这个重复的置为0 。  

等到把数组整理好，只需要遍历即可，知道nums[i] != i+1 ，最后返回i+1 。  


### python代码  

```python
# 桶排序 你可以先用两个数组思考
# 现在就是把第一个数组里面的数据，向第二个数组里面进行桶排序，然后遍历桶排序完的，看哪个位置空，则说明是
# 而现在由于必须一个数组，所以就在一个数组上操作,并且把满足nums[i] = i+1 的进行填充（因为0的位置应该为1），使用交换两个数据的方法来操作
# 在交换时候注意https://leetcode-cn.com/problems/first-missing-positive/comments/  袁雷洋的分析

class Solution(object):
    def firstMissingPositive(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        length = len(nums)
        if length ==0:
            return 1
        for i in range(length):
            while nums[i]>0 and nums[i]!=i+1 and nums[i]<=length:
                index = nums[i] -1
                if nums[index] ==nums[i]:
                    nums[i] = 0
                    break
                t = (nums[index],nums[i])
                nums[i],nums[index] = t
        i = 0
        while i <length and nums[i]==i+1:
            i+=1
        return i+1
```  

参考：https://blog.csdn.net/weixin_38823568/article/details/81360534




