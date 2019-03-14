# Leetcode 29. Divide Two Integers 两数相除

标签： `Leetcode`

---

题目地址： https://leetcode-cn.com/problems/divide-two-integers/  

## 题目描述   

<p>给定两个整数，被除数&nbsp;<code>dividend</code>&nbsp;和除数&nbsp;<code>divisor</code>。将两数相除，要求不使用乘法、除法和 mod 运算符。</p>

<p>返回被除数&nbsp;<code>dividend</code>&nbsp;除以除数&nbsp;<code>divisor</code>&nbsp;得到的商。</p>

<p><strong>示例&nbsp;1:</strong></p>

<pre><strong>输入:</strong> dividend = 10, divisor = 3
<strong>输出:</strong> 3</pre>

<p><strong>示例&nbsp;2:</strong></p>

<pre><strong>输入:</strong> dividend = 7, divisor = -3
<strong>输出:</strong> -2</pre>

<p><strong>说明:</strong></p>

<ul>
	<li>被除数和除数均为 32 位有符号整数。</li>
	<li>除数不为&nbsp;0。</li>
	<li>假设我们的环境只能存储 32 位有符号整数，其数值范围是 [−2<sup>31</sup>,&nbsp; 2<sup>31&nbsp;</sup>− 1]。本题中，如果除法结果溢出，则返回 2<sup>31&nbsp;</sup>− 1。</li>
</ul>  

## 算法思想  

### 转化法  

因为题目要求不能使用除法或者乘法做，所以要转化为加减法，而我们之前学习过对数可以把除法转化为减法：  

$\frac{a}{b}=e^{ln\frac{a}{b}}=e^{lna-lnb}$ 于是出除法就转换为了普通的减法了以及指数运算了，而题目并没有说不能使用指数运算，所以运行通过。  

只是要注意一个是符号问题，我们先都转化为正数，之后通过判断来定符号，还有就是判断溢出。  

### 移位法  

这个方法是基于一种计算，通过观察我们发现任何一个数都能拆分成如下的样式：   

> 10 -> $2^1 * 3 + 2^0 * 3 $-> $\frac{10}{3} = 2^1 + 2^0 = 3$  
10 -> $2^2 * 2 + 2^0 * 2 $-> $\frac{10}{2} = 2^2 + 2^0 = 5$
10 -> $2^3 * 1 + 2^1 * 1 $-> $\frac{10}{1} = 2^3 + 2^1 = 10$  

所以我们能对被除数进行分解，比如10和2，首先我们确定2的最高次的系数，而$10 >2* 2^2$ 但是 $10<2* 2^3$  所以确定了最高次数为2，庵后用10 减去2* 2^2，然后做同样的动作，然后我们把所有系数加起来即最后结果。而对于乘2的指数都可以转为移位操作。所以得解。  



## python代码

### 转化法 
```python  
import math
class Solution(object):
    def divide(self, dividend, divisor):
        """
        :type dividend: int
        :type divisor: int
        :rtype: int
        """
        # 除法转为指数和对数减法
        if dividend ==0:
            return 0

        a = abs(dividend)
        b = abs(divisor)
        res = math.exp(math.log(a)-math.log(b))
        # ^ 异或 两个异号的话，两个条件肯定一个为假，一个为真 则异或为真
        if (divisor>0) ^ (dividend>0): res = -res
        if res >2**31-1: res = 2**31-1
        if res<-2**31: res = -2**31
        return int(res)
``` 
### 移位法  
```python  
class Solution(object):
    def divide(self, dividend, divisor):
        """
        :type dividend: int
        :type divisor: int
        :rtype: int
        """
        # 转为移位运算
        a = abs(dividend)
        b = abs(divisor)
        res = 0
        # 如果a<b 则必定为0
        while a>=b:
            tmp = b
            cur = 1
            while a>(tmp<<1):
                # tmp = tmp *2
                tmp = tmp<<1
                cur=cur<<1
            a-=tmp
            res+=cur

        # ^ 异或 两个异号的话，两个条件肯定一个为假，一个为真 则异或为真
        if (divisor>0) ^ (dividend>0): res = -res
        if res >2**31-1: res = 2**31-1
        if res<-2**31: res = -2**31
        return res
```  

## 参考  

https://zhuanlan.zhihu.com/p/47708379








