# Leetcode 73. 矩阵置零

标签： `Leetcode`

---

题目地址：  https://leetcode-cn.com/problems/set-matrix-zeroes/

## 题目描述  

<p>给定一个&nbsp;<em>m</em> x <em>n</em> 的矩阵，如果一个元素为 0，则将其所在行和列的所有元素都设为 0。请使用<strong><a href="http://baike.baidu.com/item/%E5%8E%9F%E5%9C%B0%E7%AE%97%E6%B3%95">原地</a></strong>算法<strong>。</strong></p>

<p><strong>示例&nbsp;1:</strong></p>

<pre><strong>输入:</strong> 
[
&nbsp; [1,1,1],
&nbsp; [1,0,1],
&nbsp; [1,1,1]
]
<strong>输出:</strong> 
[
&nbsp; [1,0,1],
&nbsp; [0,0,0],
&nbsp; [1,0,1]
]
</pre>

<p><strong>示例&nbsp;2:</strong></p>

<pre><strong>输入:</strong> 
[
&nbsp; [0,1,2,0],
&nbsp; [3,4,5,2],
&nbsp; [1,3,1,5]
]
<strong>输出:</strong> 
[
&nbsp; [0,0,0,0],
&nbsp; [0,4,5,0],
&nbsp; [0,3,1,0]
]</pre>

<p><strong>进阶:</strong></p>

<ul>
	<li>一个直接的解决方案是使用 &nbsp;O(<em>m</em><em>n</em>)&nbsp;的额外空间，但这并不是一个好的解决方案。</li>
	<li>一个简单的改进方案是使用 O(<em>m</em>&nbsp;+&nbsp;<em>n</em>) 的额外空间，但这仍然不是最好的解决方案。</li>
	<li>你能想出一个常数空间的解决方案吗？</li>
</ul>     

## 算法思想  

这个题目如果直接是用O(mn)的空间，其实相对比较容易做，我们可以直接再用一个矩阵用来标记对应位置的元素，然后先遍历源矩阵，如果对应位置为0则把标记矩阵的对应行和列都写为0 ，但这样使用空间为O(mn)。  

但是从上面描述可以看出，如果某行或者某列出现了0，整行和整列都会被置位0，所以其实可以直接用两个一位数组x，y对于x可以用来标记某行是否被置位0，对于y来标记某列是否被置为0。  

## python代码  

```python
class Solution(object):
    def setZeroes(self, matrix):
        """
        :type matrix: List[List[int]]
        :rtype: None Do not return anything, modify matrix in-place instead.
        """
        m,n = len(matrix),len(matrix[0])
        x,y = [0]*m,[0]*n # x[m]表示第m行为0 y[n]表示第n行为0
        for i in range(m):
            for j in range(n):
                if matrix[i][j] ==0:
                    x[i] = 1
                    y[j]= 1
        for i,x_i in enumerate(x):
            if x_i==1: # 说明这一整行要置为0
                for j in range(n):
                    matrix[i][j] = 0

        for i,y_i in enumerate(y):
            if y_i==1: # 说明这一整列要置为0
                for j in range(m):
                    matrix[j][i] = 0
```  

参考：  

https://blog.csdn.net/smile_watermelon/article/details/45505709  


欢迎大家关注我的微信公众号，未来上面会推送`python` `机器学习` `算法学习` `深度学习` `论文阅读` 以及偶尔的`小鸡汤`等内容。ようこそいらっしゃい！
![微信公众号](https://mmbiz.qpic.cn/mmbiz_jpg/jHLoMzblJGib3edEia7P3RicYib1HqcK5ItwKCibTW89mgx6KIbpgqQ2hJlWWbLuMhiclKZvjg1GD10HqIktoKEPo18g/0?wx_fmt=jpeg)






