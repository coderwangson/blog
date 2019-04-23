# Leetcode 54. Spiral Matrix 螺旋矩阵

标签： `Leetcode`

---

题目地址：https://leetcode-cn.com/problems/spiral-matrix/  

## 题目描述   

<p>给定一个包含&nbsp;<em>m</em> x <em>n</em>&nbsp;个元素的矩阵（<em>m</em> 行, <em>n</em> 列），请按照顺时针螺旋顺序，返回矩阵中的所有元素。</p>

<p><strong>示例&nbsp;1:</strong></p>

<pre><strong>输入:</strong>
[
 [ 1, 2, 3 ],
 [ 4, 5, 6 ],
 [ 7, 8, 9 ]
]
<strong>输出:</strong> [1,2,3,6,9,8,7,4,5]
</pre>

<p><strong>示例&nbsp;2:</strong></p>

<pre><strong>输入:</strong>
[
  [1, 2, 3, 4],
  [5, 6, 7, 8],
  [9,10,11,12]
]
<strong>输出:</strong> [1,2,3,4,8,12,11,10,9,5,6,7]
</pre>


## 算法思想  

这个题目就是绕着一个矩阵走一圈，所以关键定义如何走以及标记走过的。对于走过的，我们可以标记为None，然后我们再定义一个方向，这样我们每次选择走的时候先看看是否对应的位置能走（如果为None则不能走），若能走则走过，不能走的话则换方向。  

这个地方有两个小点，是在评论区看到的，第一个是关于如何走，有个人的解答很巧妙：`matrix[(x+dx)%len(matrix)][(y+dy)%len(matrix[0])] ==None`，其中`dx dy`代表x和y的方向，比如 1或者0，他的关键在于取了一个余，这样就可以避免越界这个错误，并且在走到边界的时候相当于下一步是第一步，而第一步已经标记过，所以相当于走过了，就可以换方向。  

第二个点是如果换方向的话如何换，原来我的解答是用一个方向数组，每次加这个方向数组即可。而这个人是`dx,dy = dy,-dx`，你可以四个方向都试一下，刚好顺时针。妙   


## python代码  

```python
class Solution(object):
    def spiralOrder(self, matrix):
        """
        :type matrix: List[List[int]]
        :rtype: List[int]
        """

        res = []
        if len(matrix) == 0: return res
        x,y,dx,dy = 0,0,0,1
        for _ in range(len(matrix)*len(matrix[0])):
            res.append(matrix[x][y])
            # 走过的置一个特殊的
            matrix[x][y] = None
            # 在继续走下去之前，看看对应位置是否能走
            if matrix[(x+dx)%len(matrix)][(y+dy)%len(matrix[0])] ==None:
                # 走过了或者走到已经走过的，则换方向
                dx,dy = dy,-dx # 妙
            x+=dx
            y+=dy
        return res
```








