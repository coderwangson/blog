# Leetcode 20. Valid Parentheses 有效的括号

标签： `Leetcode`

---

题目地址：https://leetcode-cn.com/problems/valid-parentheses/solution/  

## 题目描述   

<p>给定一个只包括 <code>'('</code>，<code>')'</code>，<code>'{'</code>，<code>'}'</code>，<code>'['</code>，<code>']'</code>&nbsp;的字符串，判断字符串是否有效。</p>

<p>有效字符串需满足：</p>

<ol>
	<li>左括号必须用相同类型的右括号闭合。</li>
	<li>左括号必须以正确的顺序闭合。</li>
</ol>

<p>注意空字符串可被认为是有效字符串。</p>

<p><strong>示例 1:</strong></p>

<pre><strong>输入:</strong> "()"
<strong>输出:</strong> true
</pre>

<p><strong>示例&nbsp;2:</strong></p>

<pre><strong>输入:</strong> "()[]{}"
<strong>输出:</strong> true
</pre>

<p><strong>示例&nbsp;3:</strong></p>

<pre><strong>输入:</strong> "(]"
<strong>输出:</strong> false
</pre>

<p><strong>示例&nbsp;4:</strong></p>

<pre><strong>输入:</strong> "([)]"
<strong>输出:</strong> false
</pre>

<p><strong>示例&nbsp;5:</strong></p>

<pre><strong>输入:</strong> "{[]}"
<strong>输出:</strong> true</pre>  

## 算法思想  

非常常规的题目，在大学里面只要学过数据结构，我想这道题，都难不住你。可以使用一个栈，当是`( [ {`的时候入栈，当是`) ] }`的时候出栈，并进行比较，看是否匹配。如果不匹配则直接返回False，匹配则继续直到字符串结束，如果遍历完成后，栈也空则说明匹配，否则失败。  

## python代码  

```python
"""
Created on 2019/2/28 18:54
@File:有效的括号.py
@author: coderwangson
"""
"#codeing=utf-8"
class Solution(object):
    def isValid(self, s):
        """
        :type s: str
        :rtype: bool
        """
        l = []
        for i in s:
            if i in ['(','[','{']:
                l.append(i)
            else:
                if len(l)>0:
                    c =l.pop()
                else:
                    return False
                if (i ==')' and c=='(') or (i==']' and c=='[') or (i=='}' and c=='{'):
                    continue
                else:
                    return False
        return len(l)==0

print(Solution().isValid("]"))


```





