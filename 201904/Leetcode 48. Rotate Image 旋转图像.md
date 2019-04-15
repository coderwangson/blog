# Leetcode 48. Rotate Image 旋转图像  

标签： `Leetcode`

---

题目地址：https://leetcode-cn.com/problems/rotate-image/  

## 题目描述  

<p>给定一个 <em>n&nbsp;</em>×&nbsp;<em>n</em> 的二维矩阵表示一个图像。</p>

<p>将图像顺时针旋转 90 度。</p>

<p><strong>说明：</strong></p>

<p>你必须在<strong><a href="https://baike.baidu.com/item/%E5%8E%9F%E5%9C%B0%E7%AE%97%E6%B3%95" target="_blank">原地</a></strong>旋转图像，这意味着你需要直接修改输入的二维矩阵。<strong>请不要</strong>使用另一个矩阵来旋转图像。</p>

<p><strong>示例 1:</strong></p>

<pre>给定 <strong>matrix</strong> = 
[
  [1,2,3],
  [4,5,6],
  [7,8,9]
],

<strong>原地</strong>旋转输入矩阵，使其变为:
[
  [7,4,1],
  [8,5,2],
  [9,6,3]
]
</pre>

<p><strong>示例 2:</strong></p>

<pre>给定 <strong>matrix</strong> =
[
  [ 5, 1, 9,11],
  [ 2, 4, 8,10],
  [13, 3, 6, 7],
  [15,14,12,16]
], 

<strong>原地</strong>旋转输入矩阵，使其变为:
[
  [15,13, 2, 5],
  [14, 3, 4, 1],
  [12, 6, 8, 9],
  [16, 7,10,11]
]
</pre>

## 算法思想

我觉得就是找规律吧，我也是看的别人的规律，把一个图像旋转不容易，但是把图像旋转转化为先转置一下再镜像一下即可。所以原问题就转化为先转置再镜像，而这两个都可以原地做的。  

## python代码

```python
class Solution(object):
    def rotate(self, matrix):
        """
        :type matrix: List[List[int]]
        :rtype: None Do not return anything, modify matrix in-place instead.
        """
        # 先转置，再镜像
        # 转置 遍历下三角，转置到上三角
        for i in range(len(matrix)):
            for j in range(0,i):
                matrix[i][j],matrix[j][i] =(matrix[j][i],matrix[i][j])
        # 镜像
        for i in range(len(matrix)):
            for j in range(0,len(matrix[i])//2):
                t = len(matrix)-1-j
                matrix[i][j],matrix[i][t] = (matrix[i][t],matrix[i][j])
```  

参考： https://leetcode-cn.com/problems/rotate-image/comments/


