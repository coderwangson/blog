# Leetcode 3. Longest Substring Without Repeating Characters 无重复字符的最长子串

标签： Leetcode

---
题目地址：https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/  

## 题目描述   
给定一个字符串，请你找出其中不含有重复字符的最长子串的长度。
示例 1:

输入: "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
示例 2:

输入: "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
示例 3:

输入: "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是子串的长度，"pwke"是一个子序列，不是子串。  

## 算法思想  

### 哈希表法  

做了这几次题，发现哈希表还真是个好东西，因为哈希表的特殊存储结构，所以使得其检索起来比较快，所以在处理字符串数组的时候选择哈希表是不错的选择。  

思想是：  

1、遍历字符串如"pwwkew"  

2、用哈希表（python中可用字典）把字符和字符在的位置存起来如：{"p":0}  

3、在存起来之前要先看这个字符是否出现过。  
------3.1、如果没有，就存起来。  
------3.2、否则说明之前出现过，说明当前子串开始重复，所以记录当前的长度与当前最大长度比较，并且把哈希表置为空。并且把i回退到重复的位置。（比如pwabwuipq,如果i=4，那么说明开始重复了，和1位置的w重复了，所以要回退到1位置的后面，因为1后面的并没有重复）  

### 举个栗子   
初始化：
str = "pwawkew"  
dict ={}  
maxLength，tempLength = 0  

|i|dict|tempLength|maxLength|
|--|--|--|--|  
|0|{"p":0}|1|0|
|1|{"p":0,"w":1}|2|0|
|2|{"p":0,"w":1,"a":2}|3|0|
|3->2(回退回去)|{"p":0,"w":1,"a":2} ->{}|0|3(因为w在dict中有，所以开始重复，此时把tem与max比较，把max置为大的那个)|
|2|{"a":2}|0|3|
|...|   

因为有回退的操作，所以这个算法的效率并不是很高，提交后虽然AC但是时间为700ms。   

### python代码   

    # 784ms 
    class Solution(object):
        def lengthOfLongestSubstring(self, s):
            """
            :type s: str
            :rtype: int
            """
            dict = {}
            maxLength = 0
            temLength = 0
            i = 0
            while i<len(s):
                if s[i] in dict:
                    # 因为最后一行有i+=1所以这个地方不是回退到dict[s[i]]+1位置
                    i = dict[s[i]]
                    if temLength>maxLength:
                        maxLength = temLength
                    dict = {}
                    temLength = 0
                else:
                    dict[s[i]] = i
                    temLength+=1
                i+=1
            return (maxLength if(maxLength>temLength) else temLength)  
            
### 算法改进  

通过上面的运行可以看出非常耗时，并且也分析出来了原因，主要是因为在发现重复的时候进行了一步回退的操作，所以造成时间的浪费，那么我们可以考虑一下不进行回退，由于我们已经存储了之前重复的位置，所以重复元素之间的那段距离可以直接通过减法计算得到(temLength = i - dict[s[i]])。  

但是这样又会造成一个问题，由于那段距离我们是通过计算得到的，所以我们不能进行一个向dict里面放元素的操作，这样的话意味着我们dict的元素不能置为{}，还拿上面的那个例子str = "pwawkew" 来说，dict = {{"p":0,"w":1,"a":2}},这个时候w重复了，我们本来要把dict置为{}，但是现在我们直接计算了waw这段距离，所以我们不能把dict置为{}，因为不能保证后面是否还会有a，这样的话我们p也会存在里面，但是如果后面有p的话，其实并不算重复，**因为这个p在1的位置的w之前**，所以相当于已经删除了（因为我们计算的waw这段距离里面没有算p），所以这个时候我们的判断条件就要变了。  

如果我们遇到了一个dict里面有的元素，我们要看它是属于以前的用过的，还是刚才添加进来的，这里我们使用一个距离就能表示，因为如果这个重复的是之前的那么它的位置+tempLength肯定小于当前位置。  

![1.jpg](../img/Leetcode 3. Longest Substring Without Repeating Characters 无重复字符的最长子串_1.jpg)  

所以这个时候如果在字典中有检查到出现，那么需要再多一步判断，也就是3.2的操作进行多一步判断，这个重复的是在当前串中的，还是以前删掉的串中的。代码中就是if dict[s[i]] +temLength <i:这个判断。  

### python代码  

    # 60ms
    # ref  https://blog.csdn.net/littlebai07/article/details/79100081
    class Solution(object):
        def lengthOfLongestSubstring(self, s):
            """
            :type s: str
            :rtype: int
            """
            dict = {}
            maxLength = 0
            temLength = 0
            i = 0
            while i <len(s):
                # 不重复的了
                if (s[i] not in dict):
                    dict[s[i]] = i
                    temLength +=1
                else:
                    # 是和前面以前删除的重复了
                    if dict[s[i]] +temLength <i:
                        temLength +=1
                    # 真正重复了
                    else:
                        maxLength =maxLength if(maxLength>temLength) else temLength
                        temLength = i - dict[s[i]]
                    dict[s[i]] = i
                i+=1 
            return (maxLength if(maxLength>temLength) else temLength)  
            
参考：  

>https://blog.csdn.net/littlebai07/article/details/79100081





