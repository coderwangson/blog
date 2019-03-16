# Leetcode 30. Substring with Concatenation of All Words 串联所有单词的子串

标签： `Leetcode`

---

题目地址： https://leetcode-cn.com/problems/substring-with-concatenation-of-all-words/  

## 题目描述    

<p>给定一个字符串&nbsp;<strong>s&nbsp;</strong>和一些长度相同的单词&nbsp;<strong>words。</strong>找出 <strong>s </strong>中恰好可以由&nbsp;<strong>words </strong>中所有单词串联形成的子串的起始位置。</p>

<p>注意子串要与&nbsp;<strong>words </strong>中的单词完全匹配，中间不能有其他字符，但不需要考虑&nbsp;<strong>words&nbsp;</strong>中单词串联的顺序。</p>

<p>&nbsp;</p>

<p><strong>示例 1：</strong></p>

<pre><strong>输入：
  s =</strong> "barfoothefoobarman",
<strong>  words = </strong>["foo","bar"]
<strong>输出：</strong><code>[0,9]</code>
<strong>解释：</strong>
从索引 0 和 9 开始的子串分别是 "barfoor" 和 "foobar" 。
输出的顺序不重要, [9,0] 也是有效答案。
</pre>

<p><strong>示例 2：</strong></p>

<pre><strong>输入：
  s =</strong> "wordgoodgoodgoodbestword",
<strong>  words = </strong>["word","good","best","word"]
<code><span style=""><strong>输出：</strong></span>[]</code>
</pre>  

## 算法思想  

这道题刚开始就没读懂，后来才整明白了，就是把words里面的词能随便串联起来，然后在s中找到起始位置。对于思想我就直接暴力法了。  

从s的第一个元素开始，每次取`len(words[0])`长度的子串`s'`，因为words里面的都是长度一样的。我们就看words里面有没有子串`s``，如果有则移除，没有的话说明不能被串联，所以从s的第二的继续遍历。  

这个地方关键在于怎么看words里面有没有子串`s'`，原先我是直接利用python中的列表有一个not in的操作，所以我就判断s` not in words，如果不在的话，就直接跳出了，如果在的话就把他移除出去，这样等到结束，如果刚好移除完，则能串联。 但是这样效率很低，因为not in或者remove的算法其实相当于一次遍历，所以我就采用了字典这个数据结构，这样在字典中查找元素就非常的快了，字典的key就是words里面的词，而value就是这个词出现的个数。  

## python代码  

### 直接list  

```python
# 相对而言 list的方法中not in 不如字典的，因为字典是hash的，所以空间换时间
# 而注意在变为dict的时候，key不能同，所以只能用 个数来表示了
import copy
class Solution(object):
    def findSubstring(self, s, words):
        """
        :type s: str
        :type words: List[str]
        :rtype: List[int]
        """
        if s=="" or len(words)==0:
            return []
        n = len(words)
        m = len(words[0])
        res = []
        # 从每个可能的起点遍历
        for i in range(len(s)-m*n+1):
            tmp_words = copy.copy(words)
            # 把字符串s 从i开始构造成n个长度为m的子串列表
            for j in range(n):
                if s[i + j * m:i + j * m + m] not in tmp_words:break
                tmp_words.remove(s[i + j * m:i + j * m + m])
            else:
                res.append(i)
        return res
```


### 字典法

```python  

class Solution(object):
    def findSubstring(self, s, words):
        """
        :type s: str
        :type words: List[str]
        :rtype: List[int]
        """
        if s=="" or len(words)==0:
            return []
        n = len(words)
        m = len(words[0])
        map1 ={}
        res = []
        for i in words: map1[i] =1 if i not in map1 else map1[i]+1
        # 从每个可能的起点遍历
        for i in range(len(s)-m*n+1):
            map2 ={}
            for j in range(n):
                # 从字符串里面从起点依次取子串 看是否在字典map1中
                tmp = s[i+j*m:i+j*m+m]
                # 如果没有，则直接跳过，因为匹配不到的
                if tmp not in map1:break
                # 如果有，则先把用过的暂存到map2中，并且标记为1，这样下次再次使用的时候进行比较，如果>1，则说明使用了两次
                # 则跳过
                map2[tmp] =1 if tmp not in map2 else map2[tmp]+1
                # 说明使用了两次，则跳出
                if map2[tmp] >map1[tmp]: break
            # 说明没有break过
            else:
                res.append(i)
        return res

```   

## 参考  

https://www.cnblogs.com/grandyang/p/4521224.html




