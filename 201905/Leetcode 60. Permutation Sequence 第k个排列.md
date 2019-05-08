# Leetcode 60. Permutation Sequence 第k个排列

标签： `Leetcode`

---

题目地址：https://leetcode-cn.com/problems/permutation-sequence/comments/82926  

## 题目描述   

<p>给出集合&nbsp;<code>[1,2,3,…,<em>n</em>]</code>，其所有元素共有&nbsp;<em>n</em>! 种排列。</p>

<p>按大小顺序列出所有排列情况，并一一标记，当&nbsp;<em>n </em>= 3 时, 所有排列如下：</p>

<ol>
	<li><code>"123"</code></li>
	<li><code>"132"</code></li>
	<li><code>"213"</code></li>
	<li><code>"231"</code></li>
	<li><code>"312"</code></li>
	<li><code>"321"</code></li>
</ol>

<p>给定&nbsp;<em>n</em> 和&nbsp;<em>k</em>，返回第&nbsp;<em>k</em>&nbsp;个排列。</p>

<p><strong>说明：</strong></p>

<ul>
	<li>给定<em> n</em>&nbsp;的范围是 [1, 9]。</li>
	<li>给定 <em>k&nbsp;</em>的范围是[1, &nbsp;<em>n</em>!]。</li>
</ul>

<p><strong>示例&nbsp;1:</strong></p>

<pre><strong>输入:</strong> n = 3, k = 3
<strong>输出:</strong> "213"
</pre>

<p><strong>示例&nbsp;2:</strong></p>

<pre><strong>输入:</strong> n = 4, k = 9
<strong>输出:</strong> "2314"
</pre>  

## 算法描述  

最初我也想着利用之前的全排列先进行排列，拿到结果后对第`k`个进行输出，但是很遗憾超时，所以后来看了别人的方法，发现了一种很巧妙的方法。其实类似于我们内存的寻址。比如当前的数组是`[1,2,3,4]`，我们要计算第k个，我们发现这个数组有如下规律：   

```
1+prem([2,3,4])
2+prem([1,3,4])
3+prem([1,2,4])
4+perm([1,2,3])
```  
而我们有知道对应每一个`perm`来说有`n!`个结果，所以如果我们要第`k`个，我们可以很清楚知道他在第`k/(n-1)!`块，在块内的第`k%(n-1)!`个，而对于快内的第几个，我们同样可以按照上面的方法做下去。知道得到最终结果。   

其实理解起来也很容易，但是参考别人代码的时候，有一个地方不是很明白，在循环之前都进行了`k-1`，后来经过推算知道了，因为k是个数，是从1开始的，而块（数组索引）是从0开始的，所以k先减去1做统一。比如我们从`[1,2]`里面取第`k=1`个，如果k不减1，那么先进行`k/(2-1)! = 1,k%(2-1) = 1`，则结果不正确。  

## python代码  

```python
class Solution(object):

    def getPermutation(self, n, k):
        """
        :type n: int
        :type k: int
        :rtype: str
        """
        nums = [str(i) for i in range(1,n+1)]
        if k==1:
            return "".join(nums)
        fact = 1
        for i in range(1,n):
            fact *=i # (n-1) !
        round = n - 1 # 轮数
        # k 是从1开始，而索引从0开始，所以k-=1
        # 比如 [1,2] 我们如果k = 1的话， 则k//1 = 1 k%1 = 1,可以发现将会以2开头，所以出错

        k -= 1
        finalRes = []
        while round >= 0:
            index = int(k / fact)
            k %= fact
            finalRes.append(nums[index])
            nums.remove(nums[index])
            if round > 0:
                fact /= round
            round -= 1
        return "".join(finalRes)
``` 

参考：   

https://segmentfault.com/a/1190000018246661  

https://blog.csdn.net/qq_17550379/article/details/84959851




