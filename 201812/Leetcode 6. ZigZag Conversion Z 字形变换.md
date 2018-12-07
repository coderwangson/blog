# Leetcode 6. ZigZag Conversion Z 字形变换

标签： Leetcode

---

题目地址：https://leetcode-cn.com/problems/zigzag-conversion/  

## 题目描述：     

将一个给定字符串根据给定的行数，以从上往下、从左到右进行 Z 字形排列。

比如输入字符串为 "LEETCODEISHIRING" 行数为 3 时，排列如下：

    L   C   I   R
    E T O E S I I G
    E   D   H   N  
    
之后，你的输出需要从左往右逐行读取，产生出一个新的字符串，比如："LCIRETOESIIGEDHN"。

请你实现这个将字符串进行指定行数变换的函数：

string convert(string s, int numRows);
示例 1:

输入: s = "LEETCODEISHIRING", numRows = 3
输出: "LCIRETOESIIGEDHN"
示例 2:

输入: s = "LEETCODEISHIRING", numRows = 4
输出: "LDREOEIIECIHNTSG"
解释:
    
    L     D     R
    E   O E   I I
    E C   I H   N
    T     S     G  
    
    
**题目解释 很多人连题目都没读懂，其实我也觉得换成N字形变换比较哦好理解，就是把字符串摆成的形状是N，题目给出来的你如果看右视图就是Z字形了**  

## 算法描述   

这道题标注就是简单的题，确实如此，采用直接做的方式就可以，我原来是采用一个二维数组，遍历字符串的时候就填充到二维数组里面。遍历的时候分两种情况，一种是向下面走比如`s = "LEETCODEISHIRING", numRows = 4` ，刚开始的话先是L然后E如果填到一个二维数组里面就按照行即可，走到底后，按照对角线，这两种方式都很好用代码表示`d[m][n] = s[i] m+=1` `d[m][n] = s m-=1 n+=1`，但是在输出最终结果的时候，由于二维数组可能比较大，所以可能遍历一遍会超时。  

所以就采用一种小技巧，因为我们每一行可以直接用一个字符串表示，这样我们每次就相当从上到下往字符传里面添加值，走到底部，更改方向即可。   

如果上面描述没看懂的话没有关系，看下代码就懂了 。  

## python代码  

```python
class Solution(object):
    def convert(self, s, numRows):
        """
        :type s: str
        :type numRows: int
        :rtype: str
        """
        l = len(s)
        if numRows ==1:
            return s
        # 原先二维数组,但是由于二维数组过大，导致最后取出结果的时候超时
        #其实行直接用字符串更加方便，这样避免了取的时候再拼接
        z = ["" for i in range(numRows)]
        direction = 1
        m = 0
        # 规律就是从上往下到底，然后再回去
        for i in range(l):
            z[m]+=s[i]
            if m == 0:
                direction =1
            elif m ==numRows-1:
                direction = -1
            if direction ==1:
                m +=1
            else:
                m -=1
        return "".join(z)
```






