# Leetcode 74. 搜索二维矩阵

标签： `Leetcode`

---

题目地址：https://leetcode-cn.com/problems/search-a-2d-matrix/  

## 题目描述  

<p>编写一个高效的算法来判断&nbsp;<em>m</em> x <em>n</em>&nbsp;矩阵中，是否存在一个目标值。该矩阵具有如下特性：</p>

<ul>
	<li>每行中的整数从左到右按升序排列。</li>
	<li>每行的第一个整数大于前一行的最后一个整数。</li>
</ul>

<p><strong>示例&nbsp;1:</strong></p>

<pre><strong>输入:</strong>
matrix = [
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
target = 3
<strong>输出:</strong> true
</pre>

<p><strong>示例&nbsp;2:</strong></p>

<pre><strong>输入:</strong>
matrix = [
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
target = 13
<strong>输出:</strong> false</pre>  

## 算法思想  

因为数组本身有序，并且我们是从里面找到一个数字，所以很容易就想到了二分查找，可以通过数组的行和列计算得到数组的元素个数，然后进行二分查找，如果我们获得了一个位置mid，我们通过`mid//n,mid%n`就可以映射会原来数组的位置，然后得到对应的数值与`target`比较即可，所以就是一个高级的二分查找。  

## python代码  

```python
class Solution(object):
    def searchMatrix(self, matrix, target):
        """
        :type matrix: List[List[int]]
        :type target: int
        :rtype: bool
        """
        if len(matrix)==0:return False
        m = len(matrix)
        n = len(matrix[0])
        l = m*n
        left = 0
        right = l-1
        while left<=right:
            mid = (left + right) //2
            i,j = mid//n,mid%n
            if matrix[i][j] == target:return True
            elif matrix[i][j]>target:right = mid -1
            else:left = mid+1
        return False
```   

欢迎大家关注我的微信公众号，未来上面会推送`python` `机器学习` `算法学习` `深度学习` `论文阅读` 以及偶尔的`小鸡汤`等内容。ようこそいらっしゃい！

![image](http://wx4.sinaimg.cn/large/005Dd0fOly1g48r27k7trj307607674r.jpg)







