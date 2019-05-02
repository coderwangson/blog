# Leetcode 58. 最后一个单词的长度 Length of Last Word

标签： `Leetcode`

---

题目地址：https://leetcode-cn.com/problems/length-of-last-word/  

## 题目描述  

<p>给定一个仅包含大小写字母和空格&nbsp;<code>' '</code>&nbsp;的字符串，返回其最后一个单词的长度。</p>

<p>如果不存在最后一个单词，请返回 0&nbsp;。</p>

<p><strong>说明：</strong>一个单词是指由字母组成，但不包含任何空格的字符串。</p>

<p><strong>示例:</strong></p>

<pre><strong>输入:</strong> "Hello World"
<strong>输出:</strong> 5
</pre>  

## 算法描述  

相对比较简单的一道题目，如果不想做，可以通过一些内置函数，比如`strip split`直接开盒即用。也可以自己进行编写代码，从最后一个元素遍历，找到最右边不为空的位置，从这个位置向前找，直到一个为空的位置，这两个之间的长度就是最后一个单词的长度。   

## python代码  

```python  
class Solution(object):
    def lengthOfLastWord(self, s):
        """
        :type s: str
        :rtype: int
        """
        l = 0
        i = len(s) - 1 # 最后一个位置
        # 找到最右边不为' '的
        while i>=0 and s[i]==' ':
            i-=1
        # 找到最后一个单词的头部
        while i>=0 and s[i]!=' ':
            l+=1
            i-=1
        return l
```



