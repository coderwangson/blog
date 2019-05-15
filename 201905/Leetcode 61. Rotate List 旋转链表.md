# Leetcode 61. Rotate List 旋转链表

标签： `Leetcode`

---

题目地址：https://leetcode-cn.com/problems/rotate-list/  

## 题目描述  

<p>给定一个链表，旋转链表，将链表每个节点向右移动&nbsp;<em>k&nbsp;</em>个位置，其中&nbsp;<em>k&nbsp;</em>是非负数。</p>

<p><strong>示例&nbsp;1:</strong></p>

<pre><strong>输入:</strong> 1-&gt;2-&gt;3-&gt;4-&gt;5-&gt;NULL, k = 2
<strong>输出:</strong> 4-&gt;5-&gt;1-&gt;2-&gt;3-&gt;NULL
<strong>解释:</strong>
向右旋转 1 步: 5-&gt;1-&gt;2-&gt;3-&gt;4-&gt;NULL
向右旋转 2 步: 4-&gt;5-&gt;1-&gt;2-&gt;3-&gt;NULL
</pre>

<p><strong>示例&nbsp;2:</strong></p>

<pre><strong>输入:</strong> 0-&gt;1-&gt;2-&gt;NULL, k = 4
<strong>输出:</strong> <code>2-&gt;0-&gt;1-&gt;NULL</code>
<strong>解释:</strong>
向右旋转 1 步: 2-&gt;0-&gt;1-&gt;NULL
向右旋转 2 步: 1-&gt;2-&gt;0-&gt;NULL
向右旋转 3 步:&nbsp;<code>0-&gt;1-&gt;2-&gt;NULL</code>
向右旋转 4 步:&nbsp;<code>2-&gt;0-&gt;1-&gt;NULL</code></pre>  

## 算法思想  

所谓的旋转就是向右移动多少位，对于链表的题目来说，移动位数，用两个结点就能在一遍遍历下完成，比如下面这个图片，我们如果想移动两位，只需要用p和q两个指针，q正常移动，p在q出发两步之后开始移动，所以两个差距就刚好是2，所以当q一定到尾部，p刚好就在q前面两个，我们这个时候把p.next=Null,q.next指向头，你会发现已经移位成功了。所以通过这个小技巧，我们通过单轮循环就能移动链表了。但是这个题目还有一点就是这个n可能超过链表长度，这个就更好解决了，直接取余即可。 

![image](http://ws2.sinaimg.cn/large/005Dd0fOly1g2zz9pb4mcj30ci03p3yb.jpg)  

## python代码  

```python

class Solution(object):
    def rotateRight(self, head, k):
        """
        :type head: ListNode
        :type k: int
        :rtype: ListNode
        """
        if head==None or head.next ==None:
            return head
        p = head
        l = 0 # 长度
        while p:
            l+=1
            p = p.next
        p = head
        q = head
        i = 0
        k = k%l
        while p.next:
            if i>=k:
                q = q.next
            p = p.next
            i+=1
        p.next = head
        head = q.next
        q.next = None
        return head

```





