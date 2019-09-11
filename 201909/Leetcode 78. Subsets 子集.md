# Leetcode 78. Subsets 子集

标签： `Leetcode`

---

题目地址：https://leetcode-cn.com/problems/subsets/  

## 题目描述  

<p>给定一组<strong>不含重复元素</strong>的整数数组&nbsp;<em>nums</em>，返回该数组所有可能的子集（幂集）。</p>

<p><strong>说明：</strong>解集不能包含重复的子集。</p>

<p><strong>示例:</strong></p>

<pre><strong>输入:</strong> nums = [1,2,3]
<strong>输出:</strong>
[
  [3],
&nbsp; [1],
&nbsp; [2],
&nbsp; [1,2,3],
&nbsp; [1,3],
&nbsp; [2,3],
&nbsp; [1,2],
&nbsp; []
]</pre>  

## 解题思路  

### 逐个法  

通过观察，可以发现幂集的特点，如果要求`1 - n`的幂集，则先求`1- n-1`的幂集`A`，然后把`n`加如到`A`中的每个元素即可。比如`1 2 3`。幂集可以这样求：  

第一个幂集为`{[]}`    

把`1 2 3`中第一个元素加入幂集中--> `{[],[1]}`  

把`1 2 3`中第二个元素加入幂集中---> `{[],[1],[2],[1,2]}`  

把`1 2 3`中第三个元素加入幂集中---> `{[],[1],[2],[1,2],[3],[1,3],[2,3]...}`  

根据上面的方案则很容易写出相应代码，是一个双重循环。     

### 二进制模拟法  

因为幂集的个数刚好满足`2^n`,所以可以考虑用二进制来模拟，比如上面的`1 2 3`，可以用三位二进制占位`0 0 0`，如果对应的二进制为0，代表不取，如果为1，代表取，`0 1 1`代表一个`[2,3]`,根据这个规律写代码。  

代码外层循环代表`2^n`种可能,即从`0 - 2^n`,然后看每个数对应的二进制第几位为1，则把对应的元素添加进去即可。 

## python代码  

### 逐个法  
```python
class Solution(object):
    def subsets(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        # 初始为空集，之后没增加一个nums中的数字，则把res的所有都append这个数字
        # 比如nums 1 2 3 ，初始res = [] 遍历到nums中的1则res ->[[],[1]]
        # 遍历到2 则res ->[[],[1],[2],[1,2]]
        res  = []
        res.append([])
        for i in nums:
            for j in range(len(res)):
                res.append(res[j]+ [i])

        return res
```

### 二进制模拟法   

```python
class Solution(object):
    def subsets(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        # 刚好幂集是2^n 所以可以用二进制代表比如 0100代表第1个位置上面取
        res  = []
        for i in range(0,1<<len(nums)):# i相当于从 0000 - 1111
            tmp = []
            for j in range(0,len(nums)): # 现在有了i，每次遍历到j，只需要看i对应二进制在j位置是否为1
                if (i>>(j) & 1) ==1: # 用了小技巧，比如当前i为0100 遍历到的j = 1，那么i>>j为 0010 所以&1为0
                                            # 代表从右边第一位为0 （从0计数）
                    tmp.append(nums[j])
            res.append(tmp)
        return res
```






