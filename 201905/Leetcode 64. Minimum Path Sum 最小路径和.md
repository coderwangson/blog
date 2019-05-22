# Leetcode 64. Minimum Path Sum 最小路径和

标签： `Lettcode`

---

题目地址：https://leetcode-cn.com/problems/minimum-path-sum/  

## 题目描述  

<p>给定一个包含非负整数的 <em>m</em>&nbsp;x&nbsp;<em>n</em>&nbsp;网格，请找出一条从左上角到右下角的路径，使得路径上的数字总和为最小。</p>

<p><strong>说明：</strong>每次只能向下或者向右移动一步。</p>

<p><strong>示例:</strong></p>

<pre><strong>输入:</strong>
[
&nbsp; [1,3,1],
  [1,5,1],
  [4,2,1]
]
<strong>输出:</strong> 7
<strong>解释:</strong> 因为路径 1→3→1→1→1 的总和最小。
</pre>  

## 算法思想  

本题其实和前面的不同路径都属于同一类别的题目，可以用动态规划做，可以参考[不同路径][1]来做，只是对于不同路径来说，我们在dfs里面计算的时候，不需要判断，现在对于每个位置虽然同样的有两个地方（上方或者左方）可以到达，但是在到达之前先比较一下哪边的更小，我们取对应最小的即可，这样每次取到的都是最小的，最终结果也肯定就是最小的了，所以这里的dfs更新公式为：   

    dfs[i][j] = grid[i][j] + min(dfs[i-1][j],dfs[i][j-1])  
    
## python代码  

```python
import copy
class Solution(object):
    def minPathSum(self, grid):
        """
        :type grid: List[List[int]]
        :rtype: int
        """
        dfs = [[0]*len(grid[0]) for _ in range(len(grid))]
        dfs[0][0] = grid[0][0]
        for i in range(len(grid)):
            for j in range(len(grid[0])):
                if i==0 and j==0: dfs[i][j] = grid[i][j]
                elif j==0: dfs[i][j] = grid[i][j] + dfs[i-1][j]
                elif i==0: dfs[i][j] = grid[i][j] + dfs[i][j-1]
                else: dfs[i][j] = grid[i][j] + min(dfs[i-1][j],dfs[i][j-1])

        return dfs[i][j]
```



  [1]: https://blog.csdn.net/qq_28888837/article/details/90294940