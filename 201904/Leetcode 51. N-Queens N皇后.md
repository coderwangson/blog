# Leetcode 51. N-Queens N皇后
 
标签： `Leetcode`

--- 

题目地址：https://leetcode-cn.com/problems/n-queens/  

## 题目描述  

<p><em>n&nbsp;</em>皇后问题研究的是如何将 <em>n</em>&nbsp;个皇后放置在 <em>n</em>×<em>n</em> 的棋盘上，并且使皇后彼此之间不能相互攻击。</p>

<p><img src="https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/12/8-queens.png"></p>

<p><small>上图为 8 皇后问题的一种解法。</small></p>

<p>给定一个整数 <em>n</em>，返回所有不同的&nbsp;<em>n&nbsp;</em>皇后问题的解决方案。</p>

<p>每一种解法包含一个明确的&nbsp;<em>n</em> 皇后问题的棋子放置方案，该方案中 <code>'Q'</code> 和 <code>'.'</code> 分别代表了皇后和空位。</p>

<p><strong>示例:</strong></p>

<pre><strong>输入:</strong> 4
<strong>输出:</strong> [
 [".Q..",  // 解法 1
  "...Q",
  "Q...",
  "..Q."],

 ["..Q.",  // 解法 2
  "Q...",
  "...Q",
  ".Q.."]
]
<strong>解释:</strong> 4 皇后问题存在两个不同的解法。
</pre>  

## 算法思想  

这是一道比较经典的题目，因为解空间比较大，所以肯定是用递归做，在递归的过程中进行剪枝。关键在于怎么递归。在这里使用一个一维数组`tmp`存储结果，`tmp[i] = j`代表把第i行的皇后放在第j个位置上，而每个皇后可以放得位置有n种，可以回忆起之前做全排列的套路，使用`for p in range(n)`代表每个位置的可能性，让偶赋给tmp[i]，并且进入下一层。在全排列的时候，我们用了一个`book`数组用来避免一个数字多次使用，同样这里我们八皇后也需要做判断，但是八皇后的判断比较复杂，我们要判断是否同行，同列，同对角线。对于同行来说是不可能的，因为tmp[i]代表第i行皇后，所以每行只能放一个，而同列的话，我们遍历一遍，看刚放上的皇后是否和之前的同列如果`tmp[j]==tmp[i]`则同，对于对角巷类似的判断`abs(tmp[j]-tmp[i]) ==abs(i-j)`则同对角线。  

这样的话我们tmp上面存储的就是结果，但是题目要求一个棋盘，所以我们在递归的过程中加入一个`tmp_pan`，如果某个位置确定放上了皇后，则把`tmp_pan[i][p] ='Q'`，等到递归后记得恢复到'.' 。  

## python 代码  

```python
import copy
class Solution(object):
    def check(self,tmp,i):
        for j in range(i):
            # 同一列
            if tmp[j]==tmp[i]:
                return False
            # 对角线
            if abs(tmp[j]-tmp[i]) ==abs(i-j):
                return False
        return True
    def dfs(self,i,n,tmp,tmp_pan,res):
        if i==n:
            res.append(list(map(lambda x:"".join(x),tmp_pan)))
            return res
        for p in range(n):
            tmp[i] = p
            if self.check(tmp,i):
                tmp_pan[i][p] ='Q'
                self.dfs(i+1,n,tmp,tmp_pan,res)
                tmp_pan[i][p] = '.'
    def solveNQueens(self, n):
        """
        :type n: int
        :rtype: List[List[str]]
        """
        tmp =[0 for i in range(n)]
        tmp_pan = [["."]*n for i in range(n)]
        res =[]
        self.dfs(0,n,tmp,tmp_pan,res)
        return res
```



