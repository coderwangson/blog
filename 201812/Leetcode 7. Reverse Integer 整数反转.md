# Leetcode 7. Reverse Integer 整数反转

标签： Leetcode

---

题目地址：https://leetcode-cn.com/problems/reverse-integer/

## 题目描述  
给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。
示例 1:
输入: 123
输出: 321

示例 2:
输入: -123
输出: -321

示例 3:
输入: 120
输出: 21
注意:
假设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为 [−231,  231 − 1]。请根据这个假设，如果反转后整数溢出那么就返回 0。  

## 算法思想

其实整数翻转这种题属于入门的题目，可以直接使用除余法把整数进行分解得到，但是在这里我使用了一个小trick，我们可以把整数转换为字符串， 然后借助于字符串比较强大的库，我们进行翻转，二者效果一样的。在python中对字符串翻转可以使用切片的方法，我们把最后一个步长修改为-1即可。  
一点需要注意的是要分这个整数是否带有符号，做一个小判断就就可以了。

## python代码实现  

```python 
class Solution(object):
    def reverse(self, x):
        """
        :type x: int
        :rtype: int
        """
        s = str(x)
        flag = s[0]
        if flag =="-":
            s = s[:0:-1]
            t = int(s)
            return -t if -t>=-2**31 else 0
        else:    
            s = s[::-1]
            t = int(s)
            return t if t<=2**31-1 else 0
```  







