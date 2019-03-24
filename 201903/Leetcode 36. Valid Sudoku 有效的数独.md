# Leetcode 36. Valid Sudoku 有效的数独

标签： `Leetcode`

---

题目地址： https://leetcode-cn.com/problems/valid-sudoku/

## 题目描述  

<p>判断一个&nbsp;9x9 的数独是否有效。只需要<strong>根据以下规则</strong>，验证已经填入的数字是否有效即可。</p>

<ol>
	<li>数字&nbsp;<code>1-9</code>&nbsp;在每一行只能出现一次。</li>
	<li>数字&nbsp;<code>1-9</code>&nbsp;在每一列只能出现一次。</li>
	<li>数字&nbsp;<code>1-9</code>&nbsp;在每一个以粗实线分隔的&nbsp;<code>3x3</code>&nbsp;宫内只能出现一次。</li>
</ol>

<p><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Sudoku-by-L2G-20050714.svg/250px-Sudoku-by-L2G-20050714.svg.png" style="height: 250px; width: 250px;"></p>

<p><small>上图是一个部分填充的有效的数独。</small></p>

<p>数独部分空格内已填入了数字，空白格用&nbsp;<code>'.'</code>&nbsp;表示。</p>

<p><strong>示例&nbsp;1:</strong></p>

<pre><strong>输入:</strong>
[
  ["5","3",".",".","7",".",".",".","."],
  ["6",".",".","1","9","5",".",".","."],
  [".","9","8",".",".",".",".","6","."],
  ["8",".",".",".","6",".",".",".","3"],
  ["4",".",".","8",".","3",".",".","1"],
  ["7",".",".",".","2",".",".",".","6"],
  [".","6",".",".",".",".","2","8","."],
  [".",".",".","4","1","9",".",".","5"],
  [".",".",".",".","8",".",".","7","9"]
]
<strong>输出:</strong> true
</pre>

<p><strong>示例&nbsp;2:</strong></p>

<pre><strong>输入:</strong>
[
&nbsp; ["8","3",".",".","7",".",".",".","."],
&nbsp; ["6",".",".","1","9","5",".",".","."],
&nbsp; [".","9","8",".",".",".",".","6","."],
&nbsp; ["8",".",".",".","6",".",".",".","3"],
&nbsp; ["4",".",".","8",".","3",".",".","1"],
&nbsp; ["7",".",".",".","2",".",".",".","6"],
&nbsp; [".","6",".",".",".",".","2","8","."],
&nbsp; [".",".",".","4","1","9",".",".","5"],
&nbsp; [".",".",".",".","8",".",".","7","9"]
]
<strong>输出:</strong> false
<strong>解释:</strong> 除了第一行的第一个数字从<strong> 5</strong> 改为 <strong>8 </strong>以外，空格内其他数字均与 示例1 相同。
     但由于位于左上角的 3x3 宫内有两个 8 存在, 因此这个数独是无效的。</pre>

<p><strong>说明:</strong></p>

<ul>
	<li>一个有效的数独（部分已被填充）不一定是可解的。</li>
	<li>只需要根据以上规则，验证已经填入的数字是否有效即可。</li>
	<li>给定数独序列只包含数字&nbsp;<code>1-9</code>&nbsp;和字符&nbsp;<code>'.'</code>&nbsp;。</li>
	<li>给定数独永远是&nbsp;<code>9x9</code>&nbsp;形式的。</li>
</ul>  

## 算法思想  

就是用空间换时间，我们可以用给一个二维数组进行标注，在遍历的时候，遍历到某行某个数的时候，我们就在二维数组中标注，说明已经出现过了，则以后再次遇见，说明重复，由于我们要计算行、列、块所以就用三个进行标注。对于行列好说，第几行或者第几列根据下标一下就能看出来，比如我们遍历到了`board[i][j]`，那么显而易见是在第`i行第j列`，但是是第几个块就要进行数学运算了即在第`k = (i//3)*3+j//3`块，可以自行验证。  

## python代码  

```python 

class Solution:
    def isValidSudoku(self, board):
        """
        :type board: List[List[str]]
        :rtype: bool
        """
        # 每行的标记 比如rows[0]代表第一行 rows[0][x]代表第一出现了x这个数
        rows = [{} for i in range(9)]
        # 每列的标记
        cols = [{} for i in range(9)]
        # 每个小格子标记
        boxes = [{} for i in range(9)]
        # 遍历一遍，把每个数字对应到行，列以及格子里面标记
        for i in range(9):
            for j in range(9):
                if board[i][j]!='.':
                    nums = board[i][j]
                    # 对行标记 ，这里用个数，如果第一次出现，初始化为1 ，如果+2则说明第二次，则判断并且返回False
                    rows[i][nums] = rows[i].get(nums,0) +1
                    cols[j][nums] = cols[j].get(nums,0) +1
                    # k 代表第k个格子，根据i,j下标计算是属于哪个格子
                    k = (i//3)*3+j//3
                    boxes[k][nums] = boxes[k].get(nums,0)+1
                    if rows[i][nums]>1 or cols[j][nums]>1 or boxes[k][nums]>1:
                        return False
        return True

```




