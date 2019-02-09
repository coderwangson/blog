# Leetcode 13.Roman to Integer 罗马数字转整数

标签（空格分隔）： `Leetcode`

---

题目地址：https://leetcode-cn.com/problems/roman-to-integer/  

## 题目描述  

<p>罗马数字包含以下七种字符:&nbsp;<code>I</code>，&nbsp;<code>V</code>，&nbsp;<code>X</code>，&nbsp;<code>L</code>，<code>C</code>，<code>D</code>&nbsp;和&nbsp;<code>M</code>。</p>

<pre><strong>字符</strong>          <strong>数值</strong>
I             1
V             5
X             10
L             50
C             100
D             500
M             1000</pre>

<p>例如， 罗马数字 2 写做&nbsp;<code>II</code>&nbsp;，即为两个并列的 1。12 写做&nbsp;<code>XII</code>&nbsp;，即为&nbsp;<code>X</code>&nbsp;+&nbsp;<code>II</code>&nbsp;。 27 写做&nbsp;&nbsp;<code>XXVII</code>, 即为&nbsp;<code>XX</code>&nbsp;+&nbsp;<code>V</code>&nbsp;+&nbsp;<code>II</code>&nbsp;。</p>

<p>通常情况下，罗马数字中小的数字在大的数字的右边。但也存在特例，例如 4 不写做&nbsp;<code>IIII</code>，而是&nbsp;<code>IV</code>。数字 1 在数字 5 的左边，所表示的数等于大数 5 减小数 1 得到的数值 4 。同样地，数字 9 表示为&nbsp;<code>IX</code>。这个特殊的规则只适用于以下六种情况：</p>

<ul>
	<li><code>I</code>&nbsp;可以放在&nbsp;<code>V</code>&nbsp;(5) 和&nbsp;<code>X</code>&nbsp;(10) 的左边，来表示 4 和 9。</li>
	<li><code>X</code>&nbsp;可以放在&nbsp;<code>L</code>&nbsp;(50) 和&nbsp;<code>C</code>&nbsp;(100) 的左边，来表示 40 和&nbsp;90。&nbsp;</li>
	<li><code>C</code>&nbsp;可以放在&nbsp;<code>D</code>&nbsp;(500) 和&nbsp;<code>M</code>&nbsp;(1000) 的左边，来表示&nbsp;400 和&nbsp;900。</li>
</ul>

<p>给定一个罗马数字，将其转换成整数。输入确保在 1&nbsp;到 3999 的范围内。</p>

<p><strong>示例&nbsp;1:</strong></p>

<pre><strong>输入:</strong>&nbsp;"III"
<strong>输出:</strong> 3</pre>

<p><strong>示例&nbsp;2:</strong></p>

<pre><strong>输入:</strong>&nbsp;"IV"
<strong>输出:</strong> 4</pre>

<p><strong>示例&nbsp;3:</strong></p>

<pre><strong>输入:</strong>&nbsp;"IX"
<strong>输出:</strong> 9</pre>

<p><strong>示例&nbsp;4:</strong></p>

<pre><strong>输入:</strong>&nbsp;"LVIII"
<strong>输出:</strong> 58
<strong>解释:</strong> L = 50, V= 5, III = 3.
</pre>

<p><strong>示例&nbsp;5:</strong></p>

<pre><strong>输入:</strong>&nbsp;"MCMXCIV"
<strong>输出:</strong> 1994
<strong>解释:</strong> M = 1000, CM = 900, XC = 90, IV = 4.</pre>

## 算法思想   

### 直接法  

先声明两个数字和罗马的对应表：   

    nums = [1000,500,100,50,10,5,1]
    romans = ['M','D','C','L','X','V','I']

从左向右依次计算，因为每个字符都对应的有相应的数字，所以加起来即可。要注意的是在相加之前要进行判断一下，看是否会出现`IV`之类的，通过观察，发现如果连续两个罗马字符相差两个或者一个的话，就会出现`IV XI`这种类型，所以进行判断，如果相差1或者2，我们就进行减法。代码见下面。   

### 加减法  

上面那种方法虽然能解决问题，但是每次进行判断，通过观察发现如果是`VI`型的话进行的操作是`1+5 = 6`,相反如果是`IV`型的话进行的是`-1+5 = 4`,并且对于罗马字符一般都是左边的是大的，右边比左边小，比如`111 =CXI `,都是从大单位到小单位，如果出现小单位到大单位比如`XIV = 14`（其中从`I`到`V`就是小单位（1）到大单位（5）），则说明要进行`-1+5`类型的操作。代码见下面。

## 代码实现  

### 直接法  

```python
class Solution:
    def romanToInt(self, s):
        """
        :type s: str
        :rtype: int
        """
        nums = [1000,500,100,50,10,5,1]
        romans = ['M','D','C','L','X','V','I']
        res = 0
        i = 0
        # 减一是防止最后一个溢出
        while i<len(s)-1:
            l = romans.index(s[i])
            k = romans.index(s[i+1])
            t =l -k
            if t ==1 or t ==2:
                res +=(nums[k]-nums[l])
                i+=2
            else:
                res+=nums[l]
                i+=1
        if i<len(s):
            return res+nums[romans.index(s[-1])]
        # 可能结尾是IV，其实V已经算了
        else:
            return res
```  

### 加减法 

```python
class Solution:
    def romanToInt(self, s):
        """
        :type s: str
        :rtype: int
        """
        map ={'M':1000,'D':500,
              'C':100,'L':50,
              'X':10,'V':5,
              'I':1}
        res = 0
        i = 0
        while i<len(s):
            if i==len(s)-1 or map[s[i]]> map[s[i+1]]:
                # 一般右边都会比左边小
                res+=map[s[i]]
                # 如果右边比左边大则准备用减法做4 或者9
            else:
                res-=map[s[i]]
            i+=1
        return res
```  

## 参考  

https://blog.csdn.net/net_wolf_007/article/details/51770112




