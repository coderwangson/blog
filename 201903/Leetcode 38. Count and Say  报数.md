# Leetcode 38. Count and Say  报数

标签： `Leetcode`

---  

题目地址：https://leetcode-cn.com/problems/count-and-say/  

## 题目描述  


<p>报数序列是一个整数序列，按照其中的整数的顺序进行报数，得到下一个数。其前五项如下：</p>

<pre>1.     1
2.     11
3.     21
4.     1211
5.     111221
</pre>

<p><code>1</code>&nbsp;被读作&nbsp;&nbsp;<code>"one 1"</code>&nbsp;&nbsp;(<code>"一个一"</code>) , 即&nbsp;<code>11</code>。<br>
<code>11</code> 被读作&nbsp;<code>"two 1s"</code>&nbsp;(<code>"两个一"</code>）, 即&nbsp;<code>21</code>。<br>
<code>21</code> 被读作&nbsp;<code>"one 2"</code>, &nbsp;"<code>one 1"</code>&nbsp;（<code>"一个二"</code>&nbsp;,&nbsp;&nbsp;<code>"一个一"</code>)&nbsp;, 即&nbsp;<code>1211</code>。</p>

<p>给定一个正整数 <em>n</em>（1 ≤&nbsp;<em>n</em>&nbsp;≤ 30），输出报数序列的第 <em>n</em> 项。</p>

<p>注意：整数顺序将表示为一个字符串。</p>

<p>&nbsp;</p>

<p><strong>示例&nbsp;1:</strong></p>

<pre><strong>输入:</strong> 1
<strong>输出:</strong> "1"
</pre>

<p><strong>示例 2:</strong></p>

<pre><strong>输入:</strong> 4
<strong>输出:</strong> "1211"
</pre>  

## 算法思想  

从题目描述可以看出来，下一个数的报数是基于上一个的，就是把上一个数找到，然后进行一下转换即可，比如我要找到4，那么我就找到3的，然后把3的进行一下转换，而要计算3则需要2，这里又有递归的思想，所以伪代码为：  

    countSay(n):
        # 先计算最简单的
        如果 n ==1:返回"1"
        如果 n ==2:返回"11"
        return func(countSay(n-1))

而这里关键在于func怎么定义，从题目描述可以看出，func就是从头到尾遍历一遍，看看这个数出现几次，然后就念出来即可，比如`111`为`31`，而`1`为`11`因为有一个1。  

## python代码   

```python  

class Solution(object):

    def decode(self,s):
        res = ''
        l = len(s)
        i = 0
        t = 1
        while i<l:
            if i+1<l and s[i]==s[i+1]:
                t+=1
                i+=1
            else:
                res+=(str(t)+s[i])
                i +=1
                t = 1
        return res
    def countAndSay(self, n):
        """
        :type n: int
        :rtype: str
        """
        # 找最简单的
        if n==1:
            return "1"
        if n ==2:
            return "11"
        # 如果找5-> 其实就是先找到4的，然后把4按照报数解码即可，所以递归
        return self.decode(self.countAndSay(n-1))

```



