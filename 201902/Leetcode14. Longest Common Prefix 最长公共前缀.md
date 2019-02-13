# Leetcode14. Longest Common Prefix 最长公共前缀

标签： `Leetcode`

---

题目地址：https://leetcode-cn.com/problems/longest-common-prefix/  

## 题目描述   

<p>编写一个函数来查找字符串数组中的最长公共前缀。</p>

<p>如果不存在公共前缀，返回空字符串&nbsp;<code>""</code>。</p>

<p><strong>示例&nbsp;1:</strong></p>

<pre><strong>输入: </strong>["flower","flow","flight"]
<strong>输出:</strong> "fl"
</pre>

<p><strong>示例&nbsp;2:</strong></p>

<pre><strong>输入: </strong>["dog","racecar","car"]
<strong>输出:</strong> ""
<strong>解释:</strong> 输入不存在公共前缀。
</pre>

<p><strong>说明:</strong></p>

<p>所有输入只包含小写字母&nbsp;<code>a-z</code>&nbsp;。</p>  

## 算法思想   

### 两两比较法   

从题目描述可以看出所有我们可以每次取出两个字符串然后得到其公共前缀，然后把这个公共前缀再和下个字符串比较，这样又得到一个公共前缀，直到最后一个元素。  

### 缩短法  

先假设第一个元素就是公共前缀，然后把他在第二个字符串中得到索引，如果也是第二个的公共前缀则继续，否则把公共前缀缩短一个元素再进行判断，直到其为公共前缀为止。  

## python代码  

### 两两比较法   
```python
class Solution:
    def longestCommonPrefix(self, strs):
        """
        :type strs: List[str]
        :rtype: str
        """
        if len(strs)==0:
            return ""
        stemp = strs[0]
        i =1
        while i<len(strs):
            stemp = self.twoCommonPrefix(stemp,strs[i])
            i+=1
        return stemp

    def twoCommonPrefix(self,str1,str2):
        i = 0
        while i<len(str1) and i<len(str2):
            if str1[i]!=str2[i]:
                break
            else:
                i+=1
        return str1[0:i]
```

### 缩短法  
```python
class Solution:
    def longestCommonPrefix(self, strs):
        """
        :type strs: List[str]
        :rtype: str
        """
        if len(strs)==0:
            return ""
        stemp = strs[0]
        i =1
        while i<len(strs):
            # 如果不是公共的前缀，则stemp在strs中索引不是第0个
            # 注意是find
            while strs[i].find(stemp)!=0:
                # 那么我们把stemp依次缩短
                stemp = stemp[:-1]
                if stemp=="":
                    return stemp
            i+=1
        return stemp
``` 

## Notice  

这道题官方给出了好几种解答思路，都挺好的，可以参考https://leetcode-cn.com/problems/longest-common-prefix/solution/ 。




