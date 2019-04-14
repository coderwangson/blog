# Leetcode 43. Multiply Strings 字符串相乘

标签： `Leetcode`

---

题目地址： https://leetcode-cn.com/problems/multiply-strings/  

## 题目描述  

<p>给定两个以字符串形式表示的非负整数&nbsp;<code>num1</code>&nbsp;和&nbsp;<code>num2</code>，返回&nbsp;<code>num1</code>&nbsp;和&nbsp;<code>num2</code>&nbsp;的乘积，它们的乘积也表示为字符串形式。</p>

<p><strong>示例 1:</strong></p>

<pre><strong>输入:</strong> num1 = "2", num2 = "3"
<strong>输出:</strong> "6"</pre>

<p><strong>示例&nbsp;2:</strong></p>

<pre><strong>输入:</strong> num1 = "123", num2 = "456"
<strong>输出:</strong> "56088"</pre>

<p><strong>说明：</strong></p>

<ol>
	<li><code>num1</code>&nbsp;和&nbsp;<code>num2</code>&nbsp;的长度小于110。</li>
	<li><code>num1</code> 和&nbsp;<code>num2</code> 只包含数字&nbsp;<code>0-9</code>。</li>
	<li><code>num1</code> 和&nbsp;<code>num2</code>&nbsp;均不以零开头，除非是数字 0 本身。</li>
	<li><strong>不能使用任何标准库的大数类型（比如 BigInteger）</strong>或<strong>直接将输入转换为整数来处理</strong>。</li>
</ol>  

## 算法思想  

这道题如果直接转为数字相乘可能会造成超时之类的，所以采用二分法来做，我们现在假设有两个数`x y`,那么我们可以把两个数写作如下的形式：  

`x = a* 10^m +b` `y = c*10^n + d`  

其中`m n`可以为任意的数字，这里我们把他们看做数字长度的一半。  

所以`x*y = 10^(m+n)*a*c + 10^m *a*d + 10^n * b*c + b*d` 

发现把大整数x y变成了长度为一半的a b c d的计算，所以我们采用递归，直到规模缩小到可被计算即可。  

Notes：这里还可以优化，就是把四个乘法转化为3个乘法，可以进行数学意义上的化简。  

### python代码  

```python
class Solution(object):
    def calc(self,x,y):
        m = len(x)//2
        n = len(y)//2
        if len(x) ==0 or len(y) ==0:
            return 0
        if m ==0 and n==0:
            return int(x)*int(y)

        tmp_m_10 = 1
        tmp_n_10 = 1
        for i in range(len(x)-m):
            tmp_m_10 *=10
        for i in range(len(y)-n):
            tmp_n_10 *=10
        tmp_mn_10 = tmp_m_10 * tmp_n_10
        a = x[0:m]
        b = x[m:]
        c = y[0:n]
        d = y[n:]
        return tmp_mn_10*self.calc(a,c) + tmp_m_10 * self.calc(a,d) + tmp_n_10 * self.calc(b,c) + self.calc(b,d)
    def multiply(self, num1, num2):
        """
        :type num1: str
        :type num2: str
        :rtype: str
        """
        return str(self.calc(num1,num2))
```





