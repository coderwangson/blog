# Leetcode 72. 编辑距离

标签： `Leetcode`

---

题目地址： https://leetcode-cn.com/problems/edit-distance/  

## 题目描述  

<p>给定两个单词&nbsp;<em>word1</em> 和&nbsp;<em>word2</em>，计算出将&nbsp;<em>word1</em>&nbsp;转换成&nbsp;<em>word2 </em>所使用的最少操作数&nbsp;。</p>

<p>你可以对一个单词进行如下三种操作：</p>

<ol>
	<li>插入一个字符</li>
	<li>删除一个字符</li>
	<li>替换一个字符</li>
</ol>

<p><strong>示例&nbsp;1:</strong></p>

<pre><strong>输入:</strong> word1 = "horse", word2 = "ros"
<strong>输出:</strong> 3
<strong>解释:</strong> 
horse -&gt; rorse (将 'h' 替换为 'r')
rorse -&gt; rose (删除 'r')
rose -&gt; ros (删除 'e')
</pre>

<p><strong>示例&nbsp;2:</strong></p>

<pre><strong>输入:</strong> word1 = "intention", word2 = "execution"
<strong>输出:</strong> 5
<strong>解释:</strong> 
intention -&gt; inention (删除 't')
inention -&gt; enention (将 'i' 替换为 'e')
enention -&gt; exention (将 'n' 替换为 'x')
exention -&gt; exection (将 'n' 替换为 'c')
exection -&gt; execution (插入 'u')
</pre>  

## 解题思路   

刚开始拿到这个题目，其实毫无头绪，不知道从哪里下手，因为从一个字符串到另外一个字符串可能的情况太多了，所以不可能遍历一篇。  

所以就使用了最后绝招google，发现大家是使用动态规划做的，我们考虑两个单词`word1 word2` ,用`dp[i][j]`代表单词1中`[0-i)`到单词2中`[0-j)`中的距离（`)`代表不包含），所以现在关键在于确定递归式子，在初始情况下对与`dp[i][0]`可以直接初始化为`i`,因为只需要执行i次的删除操作即可，同样对于`dp[0][j]`只需要初始化`j`，对应执行`j`次插入操作。   

如果当前遍历到了`dp[i][j]`那么需要考虑`word1[i-1]和word2[j-1]`的关系，如果二者相等，那么毫无疑问`dp[i][j] = dp[i-1][j-1]`因为不需要做多余的修改（这里之所以是用`i-1`是因为`0-i`是不包含`i`的）。下面就可以考虑不等的情况了，如果不等，我们需要做的就是找到一种变换，能够使得距离最小，也就是从三种变换中取最小的那个操作，对于插入操作我们其实是在原有的基础上`dp[i][j-1]`增加一个操作，之所以是`dp[i][j-1]`是因为我们要在第一个单词后面插入一个任意的，所以我们先看第一个单词中[0-i)到第二个单词的[0-j-1]需要的步数，然后+1（在i的位置插入一个word2[j-1]）即可。同样的对于删除操作是在原有操作`dp[i-1][j]`上面加1，替换是在`dp[i-1][j-1]`上面加1，所以最后`dp[i][j] = min(dp[i][j-1] + 1,dp[i-1][j] +1,dp[i-1][j-1] + 1)`。  

![image](http://ws2.sinaimg.cn/large/005Dd0fOly1g3zvk9f940j30hy07zgor.jpg)  

## python代码  

```python
class Solution(object):
    def minDistance(self, word1, word2):
        """
        :type word1: str
        :type word2: str
        :rtype: int
        """
        rows,cols = len(word1),len(word2)
        # dp[i][j]代表第一个串的[0:i]到第二个串的[0:j]的距离（不包括i和j）
        dp = [[0]* (cols+1) for _ in range((rows+1))] # 避免两个空串
        # dp[0][j] = j以及dp[i][0] = i因为有个串是空串另个串需要对应长度的删除变化才能变为空串
        i,j = 0,0
        for i in range(1,rows+1):
            dp[i][0] = i
        for j in range(1,cols+1):
            dp[0][j] = j
        for i in range(1,rows+1):
            for j in range(1,cols+1):
                if word1[i-1] == word2[j-1]:dp[i][j] = dp[i-1][j-1]
                # 分三种情况取最小的  插入 dp[i][j-1] + 1  删除dp[i-1][j] +1 替换 dp[i-1][j-1] + 1
                else:dp[i][j] = min(dp[i][j-1] + 1,dp[i-1][j] +1,dp[i-1][j-1] + 1)
        return dp[i][j]
```  

参考：  

https://blog.csdn.net/Koala_Tree/article/details/80074722  


欢迎大家关注我的微信公众号，未来上面会推送`python` `机器学习` `算法学习` `深度学习` `论文阅读` 以及偶尔的`小鸡汤`等内容。ようこそいらっしゃい！
![微信公众号](https://mmbiz.qpic.cn/mmbiz_jpg/jHLoMzblJGib3edEia7P3RicYib1HqcK5ItwKCibTW89mgx6KIbpgqQ2hJlWWbLuMhiclKZvjg1GD10HqIktoKEPo18g/0?wx_fmt=jpeg)




