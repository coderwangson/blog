# Leetcode 44. Wildcard Matching 通配符匹配

标签： `Leetcode`

---

题目地址：https://leetcode-cn.com/problems/wildcard-matching/  

## 题目描述  

<p>给定一个字符串&nbsp;(<code>s</code>) 和一个字符模式&nbsp;(<code>p</code>) ，实现一个支持&nbsp;<code>'?'</code>&nbsp;和&nbsp;<code>'*'</code>&nbsp;的通配符匹配。</p>

<pre>'?' 可以匹配任何单个字符。
'*' 可以匹配任意字符串（包括空字符串）。
</pre>

<p>两个字符串<strong>完全匹配</strong>才算匹配成功。</p>

<p><strong>说明:</strong></p>

<ul>
	<li><code>s</code>&nbsp;可能为空，且只包含从&nbsp;<code>a-z</code>&nbsp;的小写字母。</li>
	<li><code>p</code>&nbsp;可能为空，且只包含从&nbsp;<code>a-z</code>&nbsp;的小写字母，以及字符&nbsp;<code>?</code>&nbsp;和&nbsp;<code>*</code>。</li>
</ul>

<p><strong>示例&nbsp;1:</strong></p>

<pre><strong>输入:</strong>
s = "aa"
p = "a"
<strong>输出:</strong> false
<strong>解释:</strong> "a" 无法匹配 "aa" 整个字符串。</pre>

<p><strong>示例&nbsp;2:</strong></p>

<pre><strong>输入:</strong>
s = "aa"
p = "*"
<strong>输出:</strong> true
<strong>解释:</strong>&nbsp;'*' 可以匹配任意字符串。
</pre>

<p><strong>示例&nbsp;3:</strong></p>

<pre><strong>输入:</strong>
s = "cb"
p = "?a"
<strong>输出:</strong> false
<strong>解释:</strong>&nbsp;'?' 可以匹配 'c', 但第二个 'a' 无法匹配 'b'。
</pre>

<p><strong>示例&nbsp;4:</strong></p>

<pre><strong>输入:</strong>
s = "adceb"
p = "*a*b"
<strong>输出:</strong> true
<strong>解释:</strong>&nbsp;第一个 '*' 可以匹配空字符串, 第二个 '*' 可以匹配字符串 "dce".
</pre>

<p><strong>示例&nbsp;5:</strong></p>

<pre><strong>输入:</strong>
s = "acdcb"
p = "a*c?b"
<strong>输入:</strong> false</pre>  

## 算法思想  

其实这个题目和之前的正则表达式匹配非常的像，这种题目或者用递归或者用动态规划，我这里采用的是动态规划，而对于动态规划，最关键的就是确定转移方程，类似`Leetcode 10`[这里][1]，我们仍然用dp[i][j]代表`s[0:i]`和`p[0:j]`的匹配情况，那么对于dp[0][0]来说，肯定为True，而对于其他的任意dp[i][j]我们则用下面的转移方程确定。  

首先我们先对p[j-1]分为两类，一类是`p[j-1]=="*"`,一类是`p[j-1]！="*"`。  

* 如果p[j-1]=="\*"，由于\*可以匹配任意字符或者什么都不匹配，所以dp[i][j] = dp[i][j-1] or dp[i-1][j]，其中dp[i][j - 1]代表\* 代表什么都不匹配，dp[i-1][j]代表* 匹配一个或多个。
* 而如果p[j-1]!="*"，则dp[i][j] = dp[i-1][j-1] and (s[i-1]==p[j-1] or p[j-1] =="?")，因为'?'可以匹配任意一个，所以就有s[i-1]==p[j-1] or p[j-1]。  

## python代码  

```python
class Solution:
    def isMatch(self, s, p):
        """
        :type s: str
        :type p: str
        :rtype: bool
        """
        n = len(s)
        m = len(p)
        dp = [[False for j in range(len(p) + 1)] for i in range(len(s) + 1)]
        dp[0][0] = True
        for i in range(n+1):
            #若p为空，则只有dp[0,0] =True
            for j in range(1,m+1):
                if p[j-1] =="*":
                    # dp[i][j - 1]代表* 代表什么都不匹配
                    # dp[i-1][j]代表* 匹配一个或多个，因为反复调用所以有多个的效果
                    # 一个为True则dp[i][j]为True
                    dp[i][j] = dp[i][j-1] or dp[i-1][j]
                else:
                    if i>0:
                        dp[i][j] = dp[i-1][j-1] and (s[i-1]==p[j-1] or p[j-1] =="?")

        return dp[n][m]
```


  [1]: https://blog.csdn.net/qq_28888837/article/details/89052274  
  
参考：  https://blog.csdn.net/qq_17550379/article/details/84191382