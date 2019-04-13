# Leetcode 46. Permutations 全排列

标签： `Leetcode`

---

题目地址：https://leetcode-cn.com/problems/permutations/  

## 题目描述  

<p>给定一个<strong>没有重复</strong>数字的序列，返回其所有可能的全排列。</p>

<p><strong>示例:</strong></p>

<pre><strong>输入:</strong> [1,2,3]
<strong>输出:</strong>
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]</pre>  

## 算法思想 

### 啊哈算法  

这个算法来自于啊哈算法上面。我们可以把这个问题看做一个人手里拿了n张牌，要放到n个盒子里面，第一次他可以从n张牌里面任意选一张，第二次从任意的n-1里面选，知道手里只剩下一张牌，这样的话就把最后一张牌放到最后一个盒子里面，让后从最后一个盒子其把里面的牌拿出，但是只有一张，所以再退一个盒子，此时手里有两张，把另一张放到盒子里面，大概就是这样的思路，这里关键在于使用一个`book`来标记这张牌是否用过了，在递归的时候我们每次都判断是否用过，如果没有，我么把对应book标记为1，并且把这个牌放到盒子里面，然后继续递归。  

### 讨论区的一个   

他的方法也是递归，因为一个数组如`R1R2R3...`的所有全排列其实可以看做是`R1 perm(R2R3..)`,`R2 perm(R1R3...)`...,并且理解很到位，所以总结出了一个很简单的算法。  

### 另一种解法  

思想和上面一个类似，只是实现的时候做了一点改动。  

## python代码  

### 啊哈算法    

```python 
import copy
class Solution(object):
    def dfs(self,i,nums,book,tmp,res):
        if i == len(nums):
            res.append(copy.copy(tmp))


        for j in range(len(nums)):
            if book[j] == 0:
                book[j] =1
                tmp.append(nums[j])
                self.dfs(i+1,nums,book,tmp,res)
                tmp.pop(-1)
                book[j] =0


    def permute(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        book = [0 for i in nums]
        res = []
        tmp = []
        self.dfs(0,nums,book,tmp,res)
        return res
```

### 讨论区的一个   

```python
import copy
class Solution(object):

    def permute(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        if len(nums)==1:
            # 因为下面 y in 所以这里返回[]
            return [nums]
        res = []
        for x in nums:
            ys = nums+[]
            ys.remove(x)
            for y in self.permute(ys):
                res.append([x]+y)
        return res
```

### 另一种解法   

```python
import copy
class Solution(object):
    def perm(self,a,start,res):
        if start==len(a):
            res.append(copy.copy(a))
        for i in range(start,len(a)):
            a[start],a[i] = a[i],a[start]
            self.perm(a,start+1,res)
            a[start],a[i] = a[i],a[start]

    def permute(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        res = []
        self.perm(nums,0,res)
        return res
```

参考：  

https://leetcode-cn.com/problems/permutations/comments/  忘了二进制吧




