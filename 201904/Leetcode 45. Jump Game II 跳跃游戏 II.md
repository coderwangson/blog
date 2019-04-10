# Leetcode 45. Jump Game II 跳跃游戏 II

标签： `Leetcode`

---

题目地址：https://leetcode-cn.com/problems/jump-game-ii/  

## 题目描述  

<p>给定一个非负整数数组，你最初位于数组的第一个位置。</p>

<p>数组中的每个元素代表你在该位置可以跳跃的最大长度。</p>

<p>你的目标是使用最少的跳跃次数到达数组的最后一个位置。</p>

<p><strong>示例:</strong></p>

<pre><strong>输入:</strong> [2,3,1,1,4]
<strong>输出:</strong> 2
<strong>解释:</strong> 跳到最后一个位置的最小跳跃数是 <code>2</code>。
&nbsp;    从下标为 0 跳到下标为 1 的位置，跳&nbsp;<code>1</code>&nbsp;步，然后跳&nbsp;<code>3</code>&nbsp;步到达数组的最后一个位置。
</pre>

<p><strong>说明:</strong></p>

<p>假设你总是可以到达数组的最后一个位置。</p>  

## 算法思想  

### bfs法（超时）  

bfs其实就是看做一个树，因为每个点有多种跳跃可能，所以我们把这个变为树形结构，这样我们层次遍历这个树即可，层数其实就是我们的跳跃次数，如果在某层找到了最后一个，那么代表能跳到最后。树如下图：  

![image](https://ws1.sinaimg.cn/large/005Dd0fOly1g1xsld3jjbj307504x746.jpg)  

0位置因为能跳两下，所以可以调到1或者2,1的位置能跳3下，所以能跳到2 3 4。但是由于没有剪枝什么的，所以最终超时。不过这个思想可以有的。   

### 贪心法  

贪心法我觉得类似于之前的跳跃游戏I，主要是确定最远距离，有了最远距离，只要<=最远距离的，我们都是可达的，然后在可达里面找到下次能跳的最远。算法步骤如下：   

* 初始时我们不能跳，所以在0的位置，当前最远也是0.
* 我们用一个临时变量接受当前最远的距离。然后我们遍历所有<=临时变量的结点，并且在这里面找到一个更远的距离，更新当前最远的距离。
* 回到第二步，知道找到最远距离超过了表。

这里我觉得贪心的原因是我们每次确定了最大可达后，我们下次是在这个`最大可达内`找下下次的最大可达。  

## python 代码  

### bfs
```python
class Solution(object):
    def jump(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        #先尝试用BFS做用list做队列
        q = []
        q.append(0)
        n = len(nums)
        res = 0
        # 标记每层的起始结点位置和最后结点位置
        firstNode = 0
        lastNode = 1
        while len(q)>0:
            # 弹出一个
            v = q.pop(0)
            firstNode+=1
            if v == n-1:
                return res
            if nums[v]>0:
                next_level = list(range(v+1,v+1+nums[v]))
                if (n-1) in next_level:
                    return res+1
                q.extend(next_level)
            if firstNode == lastNode:
                res +=1
                firstNode = 0
                lastNode = len(q)
        return -1
```

### 贪心  

```python
class Solution(object):
    def jump(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        res = 0
        n = len(nums)
        cur = 0
        i = 0
        while cur<n-1:
            res+=1
            pre = cur
            while i<=pre:
                cur = max(cur,i+nums[i])
                i+=1
            if pre ==cur:return -1
        return res
```  

参考：  

http://www.cnblogs.com/grandyang/p/4373533.html  

https://www.jianshu.com/p/1c3ba23a522e  






