# Leetcode 37. Sudoku Solver 解数独

标签： `Leetcode`

---

题目地址：https://leetcode-cn.com/problems/sudoku-solver/

## 题目描述  

<p>编写一个程序，通过已填充的空格来解决数独问题。</p>

<p>一个数独的解法需<strong>遵循如下规则</strong>：</p>

<ol>
	<li>数字&nbsp;<code>1-9</code>&nbsp;在每一行只能出现一次。</li>
	<li>数字&nbsp;<code>1-9</code>&nbsp;在每一列只能出现一次。</li>
	<li>数字&nbsp;<code>1-9</code>&nbsp;在每一个以粗实线分隔的&nbsp;<code>3x3</code>&nbsp;宫内只能出现一次。</li>
</ol>

<p>空白格用&nbsp;<code>'.'</code>&nbsp;表示。</p>

<p><img src="http://upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Sudoku-by-L2G-20050714.svg/250px-Sudoku-by-L2G-20050714.svg.png"></p>

<p><small>一个数独。</small></p>

<p><img src="http://upload.wikimedia.org/wikipedia/commons/thumb/3/31/Sudoku-by-L2G-20050714_solution.svg/250px-Sudoku-by-L2G-20050714_solution.svg.png"></p>

<p><small>答案被标成红色。</small></p>

<p><strong>Note:</strong></p>

<ul>
	<li>给定的数独序列只包含数字&nbsp;<code>1-9</code>&nbsp;和字符&nbsp;<code>'.'</code>&nbsp;。</li>
	<li>你可以假设给定的数独只有唯一解。</li>
	<li>给定数独永远是&nbsp;<code>9x9</code>&nbsp;形式的。</li>
</ul>  

## 算法思想  

其实算法的思想倒是挺好想的到的，因为是一个网格，所以肯定是遍历一遍，而由于这么大的量，所以能想的到递归遍历，然后遍历的时候我们对为'.'的进行填数，并且判断当前的数组是否满足数独的条件，所以就是把递归里面加条件判断。伪代码大概是这样。  

    dfs():
        找到为'.'的位置
        如果没有为'.'的位置，则说明结束，返回Ture
        
        找到'.',则从1-9中找到一个数字，看是否能填到数独里面，如果能，则填入，不能则继续找。
           如果填入了，我们就继续dfs下去，看是否能成功，如果能成功则返回True。
        如果所有数字都遍历完，还没找到，则不可能成功了，返回False  
        
这里关键是如何判断这个数字是否能填到数独里面，我原来是利用以前的数独是否有效的那个函数，但是后来发现每次都调用太慢了，便在评论区发现了一个人，他是直接拿了判断数独是否有效的那个数组，之后判断只需要查表即可。  

除此之外，由于是递归遍历，其实理解起来还是相对不好理解的。  

## python代码  

```pyhthon  
class Solution(object):
    def dfs(self,board,row,col,block,i,j):
        while board[i][j]!='.':
            if j<8:
                j+=1
            else:
                i+=1
                j = 0
            # 遍历完
            if i>8:
                return True
        k = (i//3)*3+j//3
        for nums in '123456789':
            # 看行，列，块是否被用
            # 这样说明第一次用
            if row[i].get(nums,0)==0 and col[j].get(nums,0)==0 and block[k].get(nums,0)==0:
                row[i][nums] =1
                col[j][nums] =1
                block[k][nums] =1
                board[i][j] =nums
                if self.dfs(board,row,col,block,i,j):
                    return True
                # 说明递归失败，则把标注取消
                else:
                    row[i][nums] = 0
                    col[j][nums] = 0
                    block[k][nums] = 0
                    board[i][j] ='.'
        # 如果遍历完还诶有，则失败
        return False



    def solveSudoku(self, board):
        """
        :type board: List[List[str]]
        :rtype: None Do not return anything, modify board in-place instead.
        """
        # 先对已经使用过的数字做下标记，这样方便以后dfs的时候不每次都进行循环，只需要取出即可
        row =[{} for i in range(9)]
        col = [{} for i in range(9)]
        block = [{} for i in range(9)]
        for i in range(9):
            for j in range(9):
                if board[i][j]!='.':
                    nums = board[i][j]
                    # 对第i行进行标注 ,如果为1 说明第一次，为二说明第二次
                    row[i][nums] = row[i].get(nums,0)+1
                    col[j][nums] = col[j].get(nums,0)+1
                    k = (i//3)*3 + j//3
                    block[k][nums] = block[k].get(nums,0)+1
        self.dfs(board,row,col,block,0,0)
        return board

```  

参考 ：

https://leetcode-cn.com/problems/sudoku-solver/comments/





