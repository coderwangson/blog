# Leetcode 52. N-Queens II N皇后 II

标签： `Leetcode`

---

题目地址：https://leetcode-cn.com/problems/n-queens-ii/  

## 题目描述  

<p><em>n&nbsp;</em>皇后问题研究的是如何将 <em>n</em>&nbsp;个皇后放置在 <em>n</em>×<em>n</em> 的棋盘上，并且使皇后彼此之间不能相互攻击。</p>

<p><img src="https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/12/8-queens.png" style="height: 276px; width: 258px;"></p>

<p><small>上图为 8 皇后问题的一种解法。</small></p>

<p>给定一个整数 <em>n</em>，返回 <em>n</em> 皇后不同的解决方案的数量。</p>

<p><strong>示例:</strong></p>

<pre><strong>输入:</strong> 4
<strong>输出:</strong> 2
<strong>解释:</strong> 4 皇后问题存在如下两个不同的解法。
[
&nbsp;[".Q..", &nbsp;// 解法 1
&nbsp; "...Q",
&nbsp; "Q...",
&nbsp; "..Q."],

&nbsp;["..Q.", &nbsp;// 解法 2
&nbsp; "Q...",
&nbsp; "...Q",
&nbsp; ".Q.."]
]
</pre>  

## 算法思想  

其实有了前面的八皇后问题的解决[八皇后I][1],这道题本质上比上面那个还要简单，因为只需要输出结果的个数，所以直接那一个全局变量，然后对于八皇后I，得到结果的时候是塞到res里面想，现在直接+=1即可。  

## python 代码  

```python
class Solution(object):
    res = 0
    def __init__(self):
        Solution.res = 0
    def check(self, tmp, i):
        for j in range(i):
            # 同一列
            if tmp[j] == tmp[i]:
                return False
            # 对角线
            if abs(tmp[j] - tmp[i]) == abs(i - j):
                return False
        return True

    def dfs(self, i, n, tmp):
        if i == n:
            Solution.res+=1
            return
        for p in range(n):
            tmp[i] = p
            if self.check(tmp, i):
                self.dfs(i + 1, n, tmp)
    def totalNQueens(self, n):
        """
        :type n: int
        :rtype: int
        """
        tmp =[0 for i in range(n)]
        self.dfs(0,n,tmp)
        return Solution.res

```


  [1]: https://blog.csdn.net/qq_28888837/article/details/89386935