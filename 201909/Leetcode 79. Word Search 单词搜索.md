# Leetcode 79. Word Search 单词搜索

标签： `Leetcode`

---

题目地址： https://leetcode-cn.com/problems/word-search/  

## 题目描述  

<p>给定一个二维网格和一个单词，找出该单词是否存在于网格中。</p>

<p>单词必须按照字母顺序，通过相邻的单元格内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母不允许被重复使用。</p>

<p><strong>示例:</strong></p>

<pre>board =
[
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]

给定 word = "<strong>ABCCED</strong>", 返回 <strong>true</strong>.
给定 word = "<strong>SEE</strong>", 返回 <strong>true</strong>.
给定 word = "<strong>ABCB</strong>", 返回 <strong>false</strong>.</pre>  

## 算法思想  

这个题目和组合数，迷宫这类问题非常类似，都是按照`DFS`这种回溯的思想来做的，所以只要一种会做了，其他的都可以按照这种套路进行套即可。  

本题可以从任意一个位置出发，然后进行深度优先遍历，因为有4个方向，所以每次可以有4中选择，如果当先选择满足则继续，不满足则回溯。代码的结构如下：  

```
def exist(board):
    mark[][] = False # 和board对应，代表该位置没被访问
    for i:
        for j:
            dfs(i,j,board,mark)
def dfs(i,j,board,mark):
    for d in direction: # 四个方向
        new_x = i + d[0]
        new_y = i + d[1]
        if 该点没被访问并满足条件:
            mark[new_x][new_y]= True
            dfs()# 向下遍历
            mark[new_x][new_y]= False
        # 没有进入if则回溯
```  

## python代码  

```python

class Solution(object):
    direction = [[0,-1],[-1,0],[0,1],[1,0]]
    def exist(self, board, word):
        """
        :type board: List[List[str]]
        :type word: str
        :rtype: bool
        """
        m = len(board)

        n = len(board[0])
        mark = [[False for _ in range(n)] for j in range(m)]
        for i in range(m):
            for j in range(n):
                if self.search(board,word,mark,i,j,m,n,0):
                    return True
        return False


    def search(self,board,word,mark,x,y,m,n,index):
        if index == len(word)-1:
            return board[x][y] == word[index]
        if board[x][y] == word[index]:
            mark[x][y] =True
            for d in self.direction:
                new_x = x + d[0]
                new_y = y + d[1]
                if new_x>=0 and new_x < m and new_y>=0 and new_y<n and not mark[new_x][new_y] and self.search(board,word,mark,new_x,new_y,m,n,index+1):
                    return True
            mark[x][y] = False
        return False
```






