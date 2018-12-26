# Leetcode 9. Palindrome Number 回文数

标签： Leetcode

---

题目地址：https://leetcode-cn.com/problems/palindrome-number/

## 题目描述  

判断一个整数是否是回文数。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。   

示例 1:
输入: 121
输出: true  

示例 2:
输入: -121
输出: false
解释: 从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。   

示例 3:
输入: 10
输出: false
解释: 从右向左读, 为 01 。因此它不是一个回文数。
     
## 算法描述     

这道题我是直接转成字符串做的，即使不转成字符串也并不是很难，如果不转成字符串就使用除法进行把每一位都取出，然后组成新的数字与原数字进行比较即可。  

虽然我是转为字符串的，但是这里也有一个小trick，就是使用了pyhon的切片功能，`s1 = s[::-1]` 就代表了把字符串倒转一遍。

## python代码表示  

```python  

class Solution(object):
    def isPalindrome(self, x):
        """
        :type x: int
        :rtype: bool
        """
        s = str(x)
        s1 = s[::-1]
        if s==s1:
            return True
        else:
            return False

            
```