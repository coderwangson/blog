# Leetcode 63. Unique Paths II 不同路径 II

标签： `Leetcode`

---

题目地址：  https://leetcode-cn.com/problems/unique-paths-ii/

## 题目描述  

<p>一个机器人位于一个 <em>m x n </em>网格的左上角 （起始点在下图中标记为“Start” ）。</p>

<p>机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为“Finish”）。</p>

<p>现在考虑网格中有障碍物。那么从左上角到右下角将会有多少条不同的路径？</p>

<p><img src="https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/22/robot_maze.png" style="height: 183px; width: 400px;"></p>

<p>网格中的障碍物和空位置分别用 <code>1</code> 和 <code>0</code> 来表示。</p>

<p><strong>说明：</strong><em>m</em>&nbsp;和 <em>n </em>的值均不超过 100。</p>

<p><strong>示例&nbsp;1:</strong></p>

<pre><strong>输入:
</strong>[
&nbsp; [0,0,0],
&nbsp; [0,1,0],
&nbsp; [0,0,0]
]
<strong>输出:</strong> 2
<strong>解释:</strong>
3x3 网格的正中间有一个障碍物。
从左上角到右下角一共有 <code>2</code> 条不同的路径：
1. 向右 -&gt; 向右 -&gt; 向下 -&gt; 向下
2. 向下 -&gt; 向下 -&gt; 向右 -&gt; 向右
</pre>  

## 算法思想  

和上面的题目不同路径类似，可以直接使用动态规划来做，只是多了个条件，在进行动态规划的时候要进行判断，判断当前位置是否为障碍物，如果为障碍物，则清为0，因为不可能通过这个地方到达最终地址，所以路径也就为0个。除此之外还要注意的是，刚开始进行一次判断，如果没有图或者出门就是障碍，可以直接返回。  

## python代码  

```python
class Solution(object):
    def uniquePathsWithObstacles(self, obstacleGrid):
        """
        :type obstacleGrid: List[List[int]]
        :rtype: int
        """
        m = len(obstacleGrid)
        n = len(obstacleGrid[0])
        if m==0 or obstacleGrid[0][0]==1: # 没有路，或者出门就堵了
            return 0
        dp = [[1 for j in range(n)] for i in range(m)] #
        # 从0,0开始发散
        for i in range(m):
            for j in range(n):
                if obstacleGrid[i][j] == 1: dp[i][j] =0 # 如果障碍，则不能通过，所以置为0
                elif j==0: dp[i][j] = dp[i-1][j] # j==0即为最左边的一列，所以和上面相关
                elif i==0: dp[i][j] = dp[i][j-1]
                else: dp[i][j] = dp[i-1][j] + dp[i][j-1]
        return dp[m-1][n-1]
``` 

参考：  

https://blog.csdn.net/guoziqing506/article/details/51671722



