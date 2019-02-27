# Leetcode 17. Letter Combinations of a Phone Number 电话号码的字母组合

标签： `Leetcode`

---

题目地址：https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/  

## 题目描述  

<p>给定一个仅包含数字&nbsp;<code>2-9</code>&nbsp;的字符串，返回所有它能表示的字母组合。</p>

<p>给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。</p>

<p><img src="http://upload.wikimedia.org/wikipedia/commons/thumb/7/73/Telephone-keypad2.svg/200px-Telephone-keypad2.svg.png"></p>

<p><strong>示例:</strong></p>

<pre><strong>输入：</strong>"23"
<strong>输出：</strong>["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
</pre>

<p><strong>说明:</strong><br>
尽管上面的答案是按字典序排列的，但是你可以任意选择答案输出的顺序。</p>  

## 算法思想  

常规想法，其实就可以看做是一个dfs（树的深度递归）问题，我们先用一个字典存储起来数组和字符的对应关系，然后用dfs递归，在dfs函数中`i`代表当前访问层数（其实就是digits的长度），然后把每个数字对应的字符遍历一遍即可。这是采用深度递归遍历做的，也可以使用广度遍历（这样需要使用到队列）。  

比如输入`23`则情况为：  

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190227213940875.png)  

这个树没有画完，所以就是看做树的遍历即可。注意边界，访问到边界的时候返回结果。   

## pyton代码  

```python  

"""
Created on 2019/2/26 18:59
@File:电话号码的字母组合.py
@author: coderwangson
"""
"#codeing=utf-8"

m = {"1": "", "2": "abc", "3": "def", "4": "ghi",
     "5": "jkl", "6": "mno", "7": "pqrs", "8": "tuv", "9": "wxyz"}
class Solution(object):


    def dfs(self,i, tmp, digits,res):
        if i == len(digits):
            res.append(tmp)
            return
        for l in m[digits[i]]:
            Solution().dfs(i + 1, tmp + l, digits,res)
    def letterCombinations(self, digits):
        """
        :type digits: str
        :rtype: List[str]
        """
        res =[]
        if len(digits)==0:
            return res
        else:
            Solution().dfs(0,"",digits,res)
            return res
print(Solution().letterCombinations("23"))


```





