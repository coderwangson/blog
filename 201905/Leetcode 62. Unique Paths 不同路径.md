# Leetcode 62. Unique Paths 不同路径

标签： `Leetcode`

---  

题目地址：https://leetcode-cn.com/problems/unique-paths/

## 题目描述  

<p>一个机器人位于一个 <em>m x n </em>网格的左上角 （起始点在下图中标记为“Start” ）。</p>

<p>机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为“Finish”）。</p>

<p>问总共有多少条不同的路径？</p>

<p><img src="https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/22/robot_maze.png"></p>

<p><small>例如，上图是一个7 x 3 的网格。有多少可能的路径？</small></p>

<p><strong>说明：</strong><em>m</em>&nbsp;和 <em>n </em>的值均不超过 100。</p>

<p><strong>示例&nbsp;1:</strong></p>

<pre><strong>输入:</strong> m = 3, n = 2
<strong>输出:</strong> 3
<strong>解释:</strong>
从左上角开始，总共有 3 条路径可以到达右下角。
1. 向右 -&gt; 向右 -&gt; 向下
2. 向右 -&gt; 向下 -&gt; 向右
3. 向下 -&gt; 向右 -&gt; 向右
</pre>

<p><strong>示例&nbsp;2:</strong></p>

<pre><strong>输入:</strong> m = 7, n = 3
<strong>输出:</strong> 28</pre>  

## 算法思想  

### 动态规划   

通过分析发现，下一步的可选择路径其实可以由左边和上面的决定，因为只能向右边和下边走，而初始的情况，对于第一列（单列）和第一行（单行）都只有一种路径，因为只能向下或者向右，而之后可以遍历，比如`dfs[1][1]`其实就是由`dfs[0][1]`和`dfs[1][0]`加起来的，所以得到`dfs[i][j] = dfs[i-1][j]+dfs[i][j-1]`这个递推，之后计算出这个矩阵即可，最终结果就是`dfs[m][n]`。   

### 数学法 

因为只能向右或者向下走，而从起始位置走到终点由于走的方式一样，所以一共要走的步数也是一样的一定是要走`m+n-2`步，并且里面有`m-1`步是向右走，有`n-1`步是向下走，所以变为组合问题，我们只需要确定其中的`m-1`或者`n-1`即可，所以变为了$C_{m+n-2}^{m-1}$,直接计算即可。  

## python代码 

### 动态规划    

```python

class Solution(object):
    def uniquePaths(self, m, n):
        """
        :type m: int
        :type n: int
        :rtype: int
        """
        dp = [[1]*m for i in range(n)]
        for i in range(1,n):
            for j in range(1,m):
                print(i,j)
                dp[i][j] = dp[i-1][j] + dp[i][j-1]

        return dp[n-1][m-1]
```

### 数学法   

```python
import math
class Solution(object):
    def uniquePaths(self, m, n):
        """
        :type m: int
        :type n: int
        :rtype: int
        """
        # 从所有的必须的步数中，找到多少中向右（或者向下的可能）也就是 $C_n^r$
        return int(math.factorial(m + n - 2) / math.factorial(m - 1) / math.factorial(n - 1))
```  

参考：  

https://juejin.im/post/5b927e795188255c6c621e11  

https://zhuanlan.zhihu.com/p/48681725  

https://www.zhihu.com/question/26094736




