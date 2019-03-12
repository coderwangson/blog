# Leetcode 28. Implement strStr() 实现strStr()

标签： `Leetcode`

---

题目地址：https://leetcode-cn.com/problems/implement-strstr/   

## 题目描述   

<p>实现&nbsp;<a href="https://baike.baidu.com/item/strstr/811469" target="_blank">strStr()</a>&nbsp;函数。</p>

<p>给定一个&nbsp;haystack 字符串和一个 needle 字符串，在 haystack 字符串中找出 needle 字符串出现的第一个位置 (从0开始)。如果不存在，则返回&nbsp; <strong>-1</strong>。</p>

<p><strong>示例 1:</strong></p>

<pre><strong>输入:</strong> haystack = "hello", needle = "ll"
<strong>输出:</strong> 2
</pre>

<p><strong>示例 2:</strong></p>

<pre><strong>输入:</strong> haystack = "aaaaa", needle = "bba"
<strong>输出:</strong> -1
</pre>

<p><strong>说明:</strong></p>

<p>当&nbsp;<code>needle</code>&nbsp;是空字符串时，我们应当返回什么值呢？这是一个在面试中很好的问题。</p>

<p>对于本题而言，当&nbsp;<code>needle</code>&nbsp;是空字符串时我们应当返回 0 。这与C语言的&nbsp;<a href="https://baike.baidu.com/item/strstr/811469" target="_blank">strstr()</a>&nbsp;以及 Java的&nbsp;<a href="https://docs.oracle.com/javase/7/docs/api/java/lang/String.html#indexOf(java.lang.String)" target="_blank">indexOf()</a>&nbsp;定义相符。</p>   

## 算法思想  

其实就是字符串匹配的问题，在以前学习的时候使用过KMP算法，但是这个算法过于复杂，而因为本次使用的是`python`，而在python中我们取字符串可以有很强大的切片操作，所以我们就可以直接遍历一遍，然后切一个长度为目标值的字符串与目标比较，如果等则返回即可。注意如果目标为""则返回0。  

## python代码  

```python  

class Solution(object):
    def strStr(self, haystack, needle):
        """
        :type haystack: str
        :type needle: str
        :rtype: int
        """
        if len(needle) ==0:
            return 0
        for i in range(len(haystack)):
            if haystack[i] ==needle[0]:
                if haystack[i:i+len(needle)] == needle:
                    return i
        return -1
```




