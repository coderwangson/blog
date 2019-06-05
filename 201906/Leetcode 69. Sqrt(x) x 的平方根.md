# Leetcode 69. Sqrt(x) x 的平方根

标签： `Leetcode`

---

题目地址:https://leetcode-cn.com/problems/sqrtx/  

## 题目描述  


<p>实现&amp;nbsp;<code>int sqrt(int x)</code>&amp;nbsp;函数。</p>

<p>计算并返回&amp;nbsp;<em>x</em>&amp;nbsp;的平方根，其中&amp;nbsp;<em>x </em>是非负整数。</p>

<p>由于返回类型是整数，结果只保留整数的部分，小数部分将被舍去。</p>

<p><strong>示例 1:</strong></p>

<pre><strong>输入:</strong> 4
<strong>输出:</strong> 2
</pre>

<p><strong>示例 2:</strong></p>

<pre><strong>输入:</strong> 8
<strong>输出:</strong> 2
<strong>说明:</strong> 8 的平方根是 2.82842..., 
&amp;nbsp;    由于返回类型是整数，小数部分将被舍去。
</pre>
" class="notranslate"><p>实现&nbsp;<code>int sqrt(int x)</code>&nbsp;函数。</p>

<p>计算并返回&nbsp;<em>x</em>&nbsp;的平方根，其中&nbsp;<em>x </em>是非负整数。</p>

<p>由于返回类型是整数，结果只保留整数的部分，小数部分将被舍去。</p>

<p><strong>示例 1:</strong></p>

<pre><strong>输入:</strong> 4
<strong>输出:</strong> 2
</pre>

<p><strong>示例 2:</strong></p>

<pre><strong>输入:</strong> 8
<strong>输出:</strong> 2
<strong>说明:</strong> 8 的平方根是 2.82842..., 
&nbsp;    由于返回类型是整数，小数部分将被舍去。
</pre>   

## 算法思想  

其实计算平方根并不是很难的事情，我们可以直接从1开始遍历，知道找到对应的结果即可，但是这道题的数据规模可能很大，所以直接这样计算会超时，所以需要进行一点小优化，这里就是把找适合的数字的过程不用从头遍历的方案，而采用二分法，因为我们遍历的数字是有序的，所以可以二分法找到对应的值。除了二分法之外，还可以使用牛顿法解决。  

## python代码  

```python
class Solution(object):
    def mySqrt(self, x):
        """
        :type x: int
        :rtype: int
        """
        left,right = 0,x
        while left<=right:
            mid = (left+right)//2
            if mid * mid >x:
                right = mid -1
            else:
                left = mid + 1
        return left-1
```





