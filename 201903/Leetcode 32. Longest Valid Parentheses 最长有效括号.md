# Leetcode 32. Longest Valid Parentheses 最长有效括号

标签： `Leetcode`

---

题目地址： https://leetcode-cn.com/problems/longest-valid-parentheses/  

## 题目描述  

<p>给定一个只包含 <code>'('</code>&nbsp;和 <code>')'</code>&nbsp;的字符串，找出最长的包含有效括号的子串的长度。</p>

<p><strong>示例&nbsp;1:</strong></p>

<pre><strong>输入:</strong> "(()"
<strong>输出:</strong> 2
<strong>解释:</strong> 最长有效括号子串为 <code>"()"</code>
</pre>

<p><strong>示例 2:</strong></p>

<pre><strong>输入:</strong> "<code>)()())</code>"
<strong>输出:</strong> 4
<strong>解释:</strong> 最长有效括号子串为 <code>"()()"</code>
</pre>  

## 算法思想  

其实就是括号匹配的加强版，我们还按照括号匹配来做，我们从头遍历这个字符串，并且左括号则把左括号下标入栈，若右括则号出栈。并且用一个start标记当前串的开始点，不过在遍历过程中要注意几点：  

* 如果当前是右括号，但是栈空，那么说明匹配失败了，那也就不存在所谓的最长了，说明从这个地方断开了，所以我们把起始位置更新一下。  

* 如果当前是右括号，但是栈不为空，那么就从栈里面弹出一个，如果弹完后栈空了，那么就比较`i-start+1`与`maxlen`，取最大的。因为弹出来完了为空，说明刚好匹配完，则和串的起始位置做差。  

* 如果当前是右括号，但是栈不为空，那么就从栈里面弹出一个，如果弹完后栈没有空，则比较`i-l[-1]`与`maxlen`，取最大。因为弹出来后，栈里面还有，我们就先更新一下，因为如果后面还有更长的话也不影响，如果后面不能再匹配成功，那么我们也已经记录下了当前最大的了。`l[-1]`是当前栈顶的位置。  

## python代码   

```python  

class Solution(object):
    def longestValidParentheses(self, s):
        """
        :type s: str
        :rtype: int
        """
        if len(s) ==0:
            return 0
        # 模拟栈
        l = []
        start = 0
        maxlen = 0
        for i in range(len(s)):
            if s[i] =='(':
                l.append(i)
            else:
                if len(l) ==0:
                    start = i+1
                else:
                    tmp = l.pop()
                    if len(l) ==0:
                        maxlen = max(i-start+1,maxlen)
                    else:
                        maxlen = max(i-l[-1],maxlen)
        return maxlen

```




