# Leetcode 65. Valid Number 有效数字

标签： `Leetcode`

---

题目地址：https://leetcode-cn.com/problems/valid-number/  

## 题目描述  

<p>验证给定的字符串是否可以解释为十进制数字。</p>

<p>例如:</p>

<p><code>"0"</code>&nbsp;=&gt;&nbsp;<code>true</code><br>
<code>" 0.1 "</code>&nbsp;=&gt;&nbsp;<code>true</code><br>
<code>"abc"</code>&nbsp;=&gt;&nbsp;<code>false</code><br>
<code>"1 a"</code>&nbsp;=&gt;&nbsp;<code>false</code><br>
<code>"2e10"</code>&nbsp;=&gt;&nbsp;<code>true</code><br>
<code>" -90e3&nbsp; &nbsp;"</code>&nbsp;=&gt;&nbsp;<code>true</code><br>
<code>" 1e"</code>&nbsp;=&gt;&nbsp;<code>false</code><br>
<code>"e3"</code>&nbsp;=&gt;&nbsp;<code>false</code><br>
<code>" 6e-1"</code>&nbsp;=&gt;&nbsp;<code>true</code><br>
<code>" 99e2.5&nbsp;"</code>&nbsp;=&gt;&nbsp;<code>false</code><br>
<code>"53.5e93"</code>&nbsp;=&gt;&nbsp;<code>true</code><br>
<code>" --6 "</code>&nbsp;=&gt;&nbsp;<code>false</code><br>
<code>"-+3"</code>&nbsp;=&gt;&nbsp;<code>false</code><br>
<code>"95a54e53"</code>&nbsp;=&gt;&nbsp;<code>false</code></p>

<p><strong>说明:</strong>&nbsp;我们有意将问题陈述地比较模糊。在实现代码之前，你应当事先思考所有可能的情况。这里给出一份可能存在于有效十进制数字中的字符列表：</p>

<ul>
	<li>数字 0-9</li>
	<li>指数 - "e"</li>
	<li>正/负号 - "+"/"-"</li>
	<li>小数点 - "."</li>
</ul>

<p>当然，在输入中，这些字符的上下文也很重要。</p>

<p><strong>更新于 2015-02-10:</strong><br>
<code>C++</code>函数的形式已经更新了。如果你仍然看见你的函数接收&nbsp;<code>const char *</code> 类型的参数，请点击重载按钮重置你的代码。</p>  

## 算法思想  

这道题目主要是理清楚，要明白如果一个数是数字那么其要满足的规则，只要弄懂了这个，直接写规则就行了。从题目以及常识可以得出一个数字满足如下的几点：  

1. 开头可能有正负号   

2. 如果出现`e` 那么之前必须要有数字出现，并且之前没有过`e`  

3. 如果出现了`.`,那么也同样的之前不能有过`.`并且不能在`e`后面出现。  

4. 如果在非开头的位置出现了`+,-`,那么必须要紧邻着`e`。  

通过上面的条件可以筛选出几个变量用来标记，用`isNum`来标记当前是否为有效数字，用`hasE`标记是否出现过了`e`，用`hasDot`标记是否出现过了`.`,所以我们遍历的时候，如果遇见的字符是`0-9`,则直接标记`isNum = True`，如果遇见的是`e`，则先判断之前是否为有效数字以及是否出现过`e`，如果之前不是有效数字或者出现过`e`则返回`False`，否则可以把`hasE = True`以及`isNum = False`.同样的如果遇见的是`.`,需要判断之前是否出现过`.`以及`e`,如果都没有，可以把`hasDot`置位`True`。把上面的4个条件依次判断一下即可。  


## python代码  

```python

class Solution(object):
    def isNumber(self, s):
        """
        :type s: str
        :rtype: bool
        """
        s = s.strip() #去空格
        n = len(s)
        if n==0:return False
        i = 0
        isNum = False # 当前0-i是否为有效数字
        hasE = False # 到当前位置是否有e出现（因为只能出现一次）
        hasDot = False # 到当前位置是否有.出现（因为只能出现一次）
        if s[i]=='-' or s[i]=='+':i=+1 # 开始的时候可能有正负号，先判断
        while i<n:
            if s[i]>='0' and s[i]<='9': isNum = True #是数字
            elif s[i]=='e':
                if not isNum or hasE:return False # 当前还没有数字（E不能出现在开头）或者已经有了E
                hasE = True
                isNum = False #如果出现E了，后面没有数字，则为False
            elif s[i]=='.':
                if hasDot or hasE:return False # .只能出现一次，并且不能在e后面，可以在e前面
                hasDot = True
            elif s[i]=='-' or s[i]=='+':
                if s[i-1]!='e':return False # 第一个代表正负号的加减刚开始已经过滤了，所以后面的+-只能是E后面的
            else:return False # 其他字符
            i+=1
        return isNum
```  

参考：  

https://blog.csdn.net/jhbxlx/article/details/38108785




