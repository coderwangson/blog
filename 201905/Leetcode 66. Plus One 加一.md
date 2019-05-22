# Leetcode 66. Plus One 加一

标签： `Leetcode`

---

题目地址：https://leetcode-cn.com/problems/plus-one/  

## 题目描述  

<p>给定一个由<strong>整数</strong>组成的<strong>非空</strong>数组所表示的非负整数，在该数的基础上加一。</p>

<p>最高位数字存放在数组的首位， 数组中每个元素只存储一个数字。</p>

<p>你可以假设除了整数 0 之外，这个整数不会以零开头。</p>

<p><strong>示例&nbsp;1:</strong></p>

<pre><strong>输入:</strong> [1,2,3]
<strong>输出:</strong> [1,2,4]
<strong>解释:</strong> 输入数组表示数字 123。
</pre>

<p><strong>示例&nbsp;2:</strong></p>

<pre><strong>输入:</strong> [4,3,2,1]
<strong>输出:</strong> [4,3,2,2]
<strong>解释:</strong> 输入数组表示数字 4321。
</pre>  

## 算法思想  

可以按照我们小时候学习的加法运算计算，从最后一位开始计算，并且用一个变量标记进位（初始把进位置为1，因为本题是计算加一的），我们把最后一位和进位相加得出结果`tmp`,则结果的最后一位为`tmp%10`，对应的新的进位为`tmp//10` ，向前遍历即可。最后判断进位是否大于0，如果大于的话，插入到最前面，否则直接返回结果。  

## python代码  

```python
class Solution(object):
    def plusOne(self, digits):
        """
        :type digits: List[int]
        :rtype: List[int]
        """
        i = len(digits)-1
        count = 1 # 进位
        while i>=0:
            tmp = digits[i]+count
            digits[i] = tmp % 10
            count = tmp // 10
            i-=1
        if count>0:
            digits.insert(0,count)
        return digits

```




