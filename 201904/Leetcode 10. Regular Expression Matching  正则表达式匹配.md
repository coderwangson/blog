# Leetcode 10. Regular Expression Matching  正则表达式匹配

标签： `Leetcode`

---

题目地址：https://leetcode-cn.com/problems/regular-expression-matching/  

## 题目描述  

<p>给定一个字符串&nbsp;(<code>s</code>) 和一个字符模式&nbsp;(<code>p</code>)。实现支持 <code>'.'</code>&nbsp;和&nbsp;<code>'*'</code>&nbsp;的正则表达式匹配。</p>

<pre>'.' 匹配任意单个字符。
'*' 匹配零个或多个前面的元素。
</pre>

<p>匹配应该覆盖<strong>整个</strong>字符串&nbsp;(<code>s</code>) ，而不是部分字符串。</p>

<p><strong>说明:</strong></p>

<ul>
	<li><code>s</code>&nbsp;可能为空，且只包含从&nbsp;<code>a-z</code>&nbsp;的小写字母。</li>
	<li><code>p</code>&nbsp;可能为空，且只包含从&nbsp;<code>a-z</code>&nbsp;的小写字母，以及字符&nbsp;<code>.</code>&nbsp;和&nbsp;<code>*</code>。</li>
</ul>

<p><strong>示例 1:</strong></p>

<pre><strong>输入:</strong>
s = "aa"
p = "a"
<strong>输出:</strong> false
<strong>解释:</strong> "a" 无法匹配 "aa" 整个字符串。
</pre>

<p><strong>示例 2:</strong></p>

<pre><strong>输入:</strong>
s = "aa"
p = "a*"
<strong>输出:</strong> true
<strong>解释:</strong>&nbsp;'*' 代表可匹配零个或多个前面的元素, 即可以匹配 'a' 。因此, 重复 'a' 一次, 字符串可变为 "aa"。
</pre>

<p><strong>示例&nbsp;3:</strong></p>

<pre><strong>输入:</strong>
s = "ab"
p = ".*"
<strong>输出:</strong> true
<strong>解释:</strong>&nbsp;".*" 表示可匹配零个或多个('*')任意字符('.')。
</pre>

<p><strong>示例 4:</strong></p>

<pre><strong>输入:</strong>
s = "aab"
p = "c*a*b"
<strong>输出:</strong> true
<strong>解释:</strong>&nbsp;'c' 可以不被重复, 'a' 可以被重复一次。因此可以匹配字符串 "aab"。
</pre>

<p><strong>示例 5:</strong></p>

<pre><strong>输入:</strong>
s = "mississippi"
p = "mis*is*p*."
<strong>输出:</strong> false</pre>  

## 算法思想  

这道题是能想得到动态规划来做，而对于动态规划来说，主要就是状态方程的表示，所以也是本题的难点，我之前在做的时候，由于把一些概念搞混了，所以造成对题目的混淆。  

1. 要注意`*`可以匹配0个或多个，所以`x*`还能匹配到空 
2. 下面定义的`dp[i][j]`代表s[0:i]和p[0:j]匹配，也就是对于s从0开始取长度为i，以及对p从0开始取长度为j的串，二者匹配。  

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190404113431931.png)  

从上面示意图能看出来，都是在i,j前面的位置。  

构建转移方程   

其实转移方程就是个二维数组，首先我们先把二维数组全部置为False，代表都不匹配。然后我们先把边上的填充，即第一行和第一列，对于第一列，也就是当p为空的时候，那么只有当s为空才会为真，即`dp[0][0] = True`，对于第一行，代表当s为空的时候，p该如何匹配，由于`*`能够匹配0或者多个，所以如果s为空的时候，p为类似`x*y*`的结构就可以匹配的到，如果p为这样的结构，即`j>=2 and p[j-1]=* and dp[0][j-2]=True`  

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190404114313272.png)  

之所以要j>=2是因为`x*y*`结构最小长度为2，而之所以要` p[j-1]=*`是因为j代表长度，而p[j-1]就代表当前匹配的最后一个字符，所以要是`*`，而`dp[0][j-2] = True`则是因为要子结构要满足条件，比如当前如果dp[0][j-2] = True，那么由于j-1的位置为*,所以可以对j-2的位置进行0次重复，所以还是满足条件。  

进行完初始化，我们可以对任意的dp[i][j]进行遍历赋值，其实主要是看`p[j-1]`（之所以是j-1是因为j代表长度，j-1才代表最后一个元素位置）。可以分作两个大类:   

1. p[j-1] ="*"
2. p[j-1]! ="*"

因为p[j-1] ="\*"比较复杂，因为*代表的内容可以有两个意义，第一是代表0次重复，第二是多次重复。所以如果想把dp[i][j] =True，则需要考虑两种情况。  

*  如果\*代表零次：当`p[j-1] ="*"  and  dp[i][j-2]=True`,这个情况可以把`dp[i][j] =True`,此时`*`代表把p[j-2]元素重复零次。  

* 如果`*`代表多次：当`p[j-1] ="*"  and dp[i-1][j]=True and (p[j-2]==s[i-1] or p[j-2]=='.')` ，这个情况也可以把`dp[i][j] =True`,不过这个确实不好想到，也不好写出来，`dp[i-1][j]=True`代表着s[0:i-1]与p[0:j]是匹配的，之所以这里的p是[0:j]而不是[0:j-1]是因为p[j-1]="*"，而\*可以代表零次，而(p[j-2]==s[i-1] or p[j-2]=='.')说明在前面的基础上又加了一个，只要相等即可。  

* 而当p[j-1]! ="*"的时候就比较简单了，只需要比较对应位置相等，并且子结构是成立即可，所以是`dp[i-1][j-1] and (p[j-1]==s[i-1] or p[j-1]==".")`  

从上面也可以看出总共三种情况，而前两种是由一种分开的，而最难的是第二个情况，也就是当`*代表多次的时候`，理清楚就好了，有时候容易混乱。  

## python 代码  

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
        #############################
        ##########python二维数组坑！！！！！！！！！！！！！！！！############
        ############################
#         dp = [[False] *(m+1)]*(n+1)
#         print(dp)
#         dp[0][1] = True
#         print(dp)
        dp=[[False for j in range(len(p)+1)] for i in range(len(s)+1)]
        dp[0][0] =True
        for j in range(1,m+1):
            dp[0][j] = j>=2 and p[j-1]=="*" and dp[0][j-2] ==True
        
        for i in range(1,n+1):
            for j in range(1,m+1):
                if p[j-1]=="*":
                    dp[i][j] = dp[i][j-2] or (dp[i-1][j] and (s[i-1]==p[j-2] or p[j-2]==".")) 
         
                else:
                    dp[i][j] = (p[j-1]=="." or s[i-1]==p[j-1]) and dp[i-1][j-1]
        return dp[n][m]

```

参考： https://blog.csdn.net/weixin_39781462/article/details/82999610





