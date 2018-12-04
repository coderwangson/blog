# Leetcode 5. Longest Palindromic Substring 最长回文子串

标签： Leetcode

---

题目地址：https://leetcode-cn.com/problems/longest-palindromic-substring/  

## 题目描述   

给定一个字符串 s，找到 s 中最长的回文子串。你可以假设 s 的最大长度为 1000。

示例 1：

输入: "babad"
输出: "bab"
注意: "aba" 也是一个有效答案。
示例 2：

输入: "cbbd"
输出: "bb"  

## 算法思想  

### 暴力法  

虽然说是暴力法，但是我也对其做了一点点优化，并不是完全就是寻找所有子串，然后判断子串是否为回文。  

这里是在遍历字符串的时候，把遍历到的字符存储到一个字典里面，然后遍历到下一个字符的时候先看看字符是否在字典里面，因为如果是长度大于2的回文串的话，这个串的首尾元素肯定是一样的，所以如果不在的话，可定不是回文。  

如果在的话，就进行判断这个字符到里面存在的字符之间的这个子串是否为回文的，并且记录其长度与当前最长子串的长度比较。  

这里有个地方要注意的是，字典在存储的时候，要把每个字符出现的所有位置都记录下来比如:`"abaca"`，记录a的话要记录为`{"a":[0,]}` ,这样的原因是由于每次出现的子串并不确定到哪个才是最长的。所以在检测的时候也要把所有位置的子串都检测一遍。  

### python代码  

```python
class Solution(object):
    def check(self,s,start,end):
        i,j = start,end
        while(i<j):
            if s[i] !=s[j]:
                return ""
            i+=1
            j-=1
        return s[start:end+1]
    def longestPalindrome(self, s):
        """
        :type s: str
        :rtype: str
        """
        # 最短也是一个
        best = s[0] if s!="" else ""
        dict = {}
        for i in range(len(s)):
            if s[i] not in dict:
                dict[s[i]] = [i]
            else:
                for start in dict[s[i]]:
                    temp = self.check(s,start,i)
                    if len(temp)>len(best):
                        best = temp
                dict[s[i]].append(i)
        return best
```   

这个代码虽然能够运行，但是提交的时候回出现超时。所以就找其他的方法解决。   

### 动态规划法  

对于可以进行动态规划的问题，其实关键就在于找到那个动态规划的算式。找到规划的规律，然后我们先解出小规模的问题的解，然后大规模问题的解就根据算式计算出来，所以一般动态规划都需要一个一维或者二维的矩阵，用于存储小规模问题的解，这样的话我们大规模问题的解可以在此基础上计算得到，所以也算是一种用空间换取时间的策略。  

对于本题来说，对于字符串str，我们使用`dp[i][j]=1`表示str[i:j]是回文子串,那么可以推得`dp[i+1][j-1]=1`,因为回文串把首尾删了肯定还是回文串，所以我们根据回文串把首尾删了还是回文串得到如果一个回文串加上一个首尾一样的字符那么构成的串还是回文串，所以得到状态转移方程：  

$$ dp[i][j]=
\begin{cases}
dp[i+1][j-1] && str[i]=str[j] \\
0   && str[i]\ne str[j] \\
\end{cases}
$$  

在刚开始的时候我们可以定义所有的`dp[i][i] =1`，因为单个元素一定是回文，并且还有`dp[i][i+1] =1 在str[i]=str[i+1]`，因为两个相等的子串也一定是回文的。  

由于题目是要我们输出一个子串，所以我们在进行构造转移方程的时候记得记录一下最长子串的开始位置以及长度，这样方便最终返回。

### 举个栗子  

如有子串`str = "babad"`  

则初始dp为  

| ` |0|1|2|3|4|
|--|--|--|--|--|
|0|1|0||||
|1||1|0|||
|2|||1|0||
|3||||1|0|
|4|||||1|  

其实在做完初始矩阵我们就是完成了一个最长长度最多为2的计算，从这个表格明显可以看出没有长度为2的回文子串，我们下面所需要做得就是按照状态方程，把整个表格的右上角填满，按照对角线的方式填，比如下次计算应该计算长度为3的了，那么计算`dp[0][2]`,因为str[0]=str[2],所以`dp[0][2]=dp[0+1][2-1] = 1`,可以看出其为回文从0开始长度为3的为回文串，所以我们也可以更新一下最长子串的标记`start = 0 longest = 3`，这样方便我们以后返回这个子串。  

最终dp为：  
| ` |0|1|2|3|4|
|--|--|--|--|--|
|0|1|0|1|0|0|
|1||1|0|1|0|
|2|||1|0|0|
|3||||1|0|
|4|||||1|   

### python代码实现   

```python  

class Solution(object):
    
    def longestPalindrome(self, s):
        """
        :type s: str
        :rtype: str
        """
        #动态规划 
        #ref https://www.cnblogs.com/mini-coconut/p/9074315.html
        length = len(s)
        #最长子串开始位置
        start = 0
        #最长子串长度  --所以最长子串为 s[start,longest]
        longest = 1
        d = [[0]*length for i in range(length)]
        for i in range(length):
            d[i][i] = 1
            if i+1 <length:
                if s[i] ==s[i+1]:
                    d[i][i+1] =1
                    start = i
                    longest = 2
        # 构造数组右三角，按对角线构造,就是每次外层for遍历的为一个斜的对角线
        for l in range(3,length+1):
            for i in range(0,length+1-l):
                j = i+l-1
                if s[i] == s[j] and d[i+1][j-1]:
                    d[i][j] = 1
                    start = i
                    longest = l
        return s[start:start+longest]
```  

参考：
>https://www.cnblogs.com/mini-coconut/p/9074315.html





