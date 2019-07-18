# Leetcode 76. Minimum Window Substring  最小覆盖子串

标签： `Leetcode`

---

题目地址：https://leetcode-cn.com/problems/minimum-window-substring/  

## 题目描述  

<p>给你一个字符串 S、一个字符串 T，请在字符串 S 里面找出：包含 T 所有字母的最小子串。</p>

<p><strong>示例：</strong></p>

<pre><strong>输入: S</strong> = "ADOBECODEBANC", <strong>T</strong> = "ABC"
<strong>输出:</strong> "BANC"</pre>

<p><strong>说明：</strong></p>

<ul>
	<li>如果 S 中不存这样的子串，则返回空字符串 <code>""</code>。</li>
	<li>如果 S 中存在这样的子串，我们保证它是唯一的答案。</li>
</ul>  


## 算法思想  

这个问题是在一个大的字符串里面找小的字符串，所以可以考虑使用滑动窗口来做。并且我们对待匹配的字符串并不关心字符的前后位置，只关心字符个数，所以可以用hash表把字符情况存储起来，等到遍历的时候进行更新。  

我们用`left right`代表滑动窗口的左边和右边，用`m`代表目标字符串中的数据情况，`count`代表当前匹配的长度。保证窗口的右边不超过字符串的长度即可。遍历的时候我们先看当前遍历到的字符是否在m中，如果在的话，需要更新m，并且根据m中对应字符的大小，来确定当前是否需要更新`count`。如果我们的`m[s[right]]>=0`则需要更新count，否则不更新（因为如果<0 则代表目标子串中对应的这个字符早已匹配完了，所以即使再有多的也无用）。  

最后根据count和目标子串比较，如果相等，说明当前匹配成功，所以和以前的长度比较，更新。同时也要更新`left`，同样这里会有一个对`m[s[left]] > 0`的判断，这个类似上面的right，因为如果`>0`则代表把重复的用完了，下面再用的话就真的用到了目标串里面的了，所以下面需要更新`count`。  

## python代码   

```python
class Solution(object):
    def minWindow(self, s, t):
        """
        :type s: str
        :type t: str
        :rtype: str
        """
        left,right= 0,0
        minLen = len(s)+1 # 初始化的时候为最大，这样后面能更新
        m = {}
        res = ""
        for i in t:
            m[i] = m.get(i,0) + 1# 因为只是要统计t中出现字符，不需要顺序一致，所以可以用hash存储
        # 开始滑动窗口先找右，再滑动左
        count = 0 # 表示t中有多少个在s中，不包括s中多的重复的
        while right<len(s):
            if s[right] in m:
                m[s[right]] -=1 # 匹配掉了
                if m[s[right]]>=0:# 必须要>=0  比如 ABBBC 和ABC 当匹配到第一个B的时候count需要+=1，但是第二个的时候m["B"] = -1此时不需要count+=1了，因为重复了
                    count+=1
                while count == len(t): # 说明这个窗口里面包含了所有
                    if right -left + 1<minLen:
                        minLen = right -left + 1
                        res = s[left:right+1]
                    if s[left] in m: # 滑动left之前先看这个被滑动的是不是会影响count
                        m[s[left]] +=1
                        if m[s[left]] > 0 : # 说明滑动到了最后一个，把之前囤积的都滑动没了,对count更新
                            count -=1
                    left +=1
            right +=1
        return res

```  

参考：  

https://blog.csdn.net/weixin_41958153/article/details/81623474



欢迎大家关注我的微信公众号，未来上面会推送`python` `机器学习` `算法学习` `深度学习` `论文阅读` 以及偶尔的`小鸡汤`等内容。ようこそいらっしゃい！  
 
搜索 coderwangson 关注

![image](http://wx4.sinaimg.cn/large/005Dd0fOly1g48r27k7trj307607674r.jpg)

