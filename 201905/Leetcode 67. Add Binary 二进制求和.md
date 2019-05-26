# Leetcode 67. Add Binary 二进制求和

标签： `Leetcode`

---

题目地址：https://leetcode-cn.com/problems/add-binary/  

## 题目描述  

<p>给定两个二进制字符串，返回他们的和（用二进制表示）。</p>

<p>输入为<strong>非空</strong>字符串且只包含数字&nbsp;<code>1</code>&nbsp;和&nbsp;<code>0</code>。</p>

<p><strong>示例&nbsp;1:</strong></p>

<pre><strong>输入:</strong> a = "11", b = "1"
<strong>输出:</strong> "100"</pre>

<p><strong>示例&nbsp;2:</strong></p>

<pre><strong>输入:</strong> a = "1010", b = "1011"
<strong>输出:</strong> "10101"</pre>  

## 算法思想  

类似于上面的加1的题目，其实就是按照我们小时候的手动加法操作即可，用一个进位，然后从右向左边逐位计算。需要注意的是可能两个数字的长度不一样，这个时候需要把长的那个继续向前计算，如果有进位注意加上进位。  

## python代码  

```python
class Solution(object):
    def addBinary(self, a, b):
        """
        :type a: str
        :type b: str
        :rtype: str
        """
        res = ""
        la = len(a) - 1
        lb = len(b) - 1
        count = 0 # 进位
        while la>=0 and lb>=0:
            tmp = int(a[la]) + int(b[lb]) + count
            res = str(tmp%2) + res
            count = tmp//2
            la -=1
            lb -=1
        if la<0: # 都统一到a上面进行操作
            la = lb
            a = b
        while la>=0:
            tmp = int(a[la]) + count
            res = str(tmp % 2) + res
            count = tmp // 2
            la -= 1
        if count>0:
            res = str(count) + res

        return res

```



