# Leetcode 50. Pow(x, n)  Pow(x, n)

标签： `Leetcode`

---

题目地址：https://leetcode-cn.com/problems/powx-n/  

## 题目描述   

<p>实现&nbsp;<a href="https://www.cplusplus.com/reference/valarray/pow/" target="_blank">pow(<em>x</em>, <em>n</em>)</a>&nbsp;，即计算 x 的 n 次幂函数。</p>

<p><strong>示例 1:</strong></p>

<pre><strong>输入:</strong> 2.00000, 10
<strong>输出:</strong> 1024.00000
</pre>

<p><strong>示例&nbsp;2:</strong></p>

<pre><strong>输入:</strong> 2.10000, 3
<strong>输出:</strong> 9.26100
</pre>

<p><strong>示例&nbsp;3:</strong></p>

<pre><strong>输入:</strong> 2.00000, -2
<strong>输出:</strong> 0.25000
<strong>解释:</strong> 2<sup>-2</sup> = 1/2<sup>2</sup> = 1/4 = 0.25</pre>

<p><strong>说明:</strong></p>

<ul>
	<li>-100.0 &lt;&nbsp;<em>x</em>&nbsp;&lt; 100.0</li>
	<li><em>n</em>&nbsp;是 32 位有符号整数，其数值范围是&nbsp;[−2<sup>31</sup>,&nbsp;2<sup>31&nbsp;</sup>− 1] 。</li>
</ul>  

## 算法思想 

可能最直白的方法就是直接遍历一轮，然后累乘，但是这样可能会超时，所以根据乘法的规律来做。比如我们要计算`x^n`其实相当于先计算`a = x^(n//2)`，最后的答案就是a的平方或者a的平法`*x`，比如要计算`x^4`则为（x^2）^2,如果要计算x^5则为（x^2）^2*x,是否要乘x与n是奇偶有关，如果偶数则不乘，否则乘。其实就相当于二分做了。   

除此之外要注意的是，我们刚开始都把n当做整数，之后如果n是负数则变为1/结果。

## python 代码 

```python
class Solution(object):
    def myPow(self, x, n):
        """
        :type x: float
        :type n: int
        :rtype: float
        """
        if n ==0:return 1
        half = self.myPow(x,abs(n)//2)
        res = 1
        if n%2 == 0:res = half*half # 比如 2^4 = 2^2 * 2^2
        else: res = half*half*x # 比如2^5 = 2^2*2^2 *2
        # 因为在python中 -1//2 = -1
        return res if n>0 else 1/res
```






