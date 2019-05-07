# Leetcode 59. Spiral Matrix II 螺旋矩阵 II

标签： `Leetcode`

---

题目地址：https://leetcode-cn.com/problems/spiral-matrix-ii/  

## 题目描述  

<p>给定一个正整数&nbsp;<em>n</em>，生成一个包含 1 到&nbsp;<em>n</em><sup>2</sup>&nbsp;所有元素，且元素按顺时针顺序螺旋排列的正方形矩阵。</p>

<p><strong>示例:</strong></p>

<pre><strong>输入:</strong> 3
<strong>输出:</strong>
[
 [ 1, 2, 3 ],
 [ 8, 9, 4 ],
 [ 7, 6, 5 ]
]</pre>  

## 算法思想  

和之前的那个螺旋矩阵几乎一样，把遍历变成输入进去即可，并没有什么不同的地方，同样用几个方向，根据res是否为None作为判断即可。详情可参考[旋转矩阵][1]这篇文章。  

## python代码  

```python
class Solution(object):
    def generateMatrix(self, n):
        """
        :type n: int
        :rtype: List[List[int]]
        """
        res = [[None]*n for _ in range(n)]

        x,y,nx,ny = 0,0,0,1
        for i in range(n*n):
            res[x][y] = i+1
            if not res[(x+nx)%n][(y+ny)%n] is None:# 说明来过了，换方向
                nx, ny = ny, -nx
            x+=nx
            y+=ny
        return res
```


  [1]: https://blog.csdn.net/qq_28888837/article/details/89460005