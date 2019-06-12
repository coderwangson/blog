# Leetcode 70. Climbing Stairs 爬楼梯

标签： `Leetcode`

---

题目地址： https://leetcode-cn.com/problems/climbing-stairs/  

## 题目描述  

<p>假设你正在爬楼梯。需要 <em>n</em>&nbsp;阶你才能到达楼顶。</p>

<p>每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？</p>

<p><strong>注意：</strong>给定 <em>n</em> 是一个正整数。</p>

<p><strong>示例 1：</strong></p>

<pre><strong>输入：</strong> 2
<strong>输出：</strong> 2
<strong>解释：</strong> 有两种方法可以爬到楼顶。
1.  1 阶 + 1 阶
2.  2 阶</pre>

<p><strong>示例 2：</strong></p>

<pre><strong>输入：</strong> 3
<strong>输出：</strong> 3
<strong>解释：</strong> 有三种方法可以爬到楼顶。
1.  1 阶 + 1 阶 + 1 阶
2.  1 阶 + 2 阶
3.  2 阶 + 1 阶
</pre>


## 算法思想  

根据题目描述很容易得出爬1阶有一种方法，爬2阶有两种方法，根据前两个推断第三个，要想爬到第三阶，则上一次必须能爬到第2阶（则再爬一阶即可）或者爬到第一阶（则再爬两阶即可），所以爬到第三阶的可能情况就是爬到第二阶的加上爬到第一阶的，所以共有3种。   

同样的对于第4阶，则上一次必须能爬到第三阶或者第二阶（爬到第一阶不行，因为从第一阶没法一下到第4阶），所以到第4阶有5种方案。  

从上面分析可以看出就是斐波切纳数列，直接求即可，当然也可以倒着退，用递归。   

## python代码  

```python
class Solution(object):
    def climbStairs(self, n):
        """
        :type n: int
        :rtype: int
        """
        if n ==1:return 1
        if n==2:return 2
        f1 = 1
        f2 = 2
        for i in range(3,n+1):
            f = f1 + f2
            f1,f2 = f2,f
        return f
```  

可参考： http://www.sohu.com/a/149075950_684445




