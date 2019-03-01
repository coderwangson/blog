# Leetcode 19. Remove Nth Node From End of List 删除链表的倒数第N个节点

标签： `Leetcode`

---

题目地址：https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/   

## 题目描述  

<p>给定一个链表，删除链表的倒数第&nbsp;<em>n&nbsp;</em>个节点，并且返回链表的头结点。</p>

<p><strong>示例：</strong></p>

<pre>给定一个链表: <strong>1-&gt;2-&gt;3-&gt;4-&gt;5</strong>, 和 <strong><em>n</em> = 2</strong>.

当删除了倒数第二个节点后，链表变为 <strong>1-&gt;2-&gt;3-&gt;5</strong>.
</pre>

<p><strong>说明：</strong></p>

<p>给定的 <em>n</em>&nbsp;保证是有效的。</p>

<p><strong>进阶：</strong></p>

<p>你能尝试使用一趟扫描实现吗？</p>  

## 算法思想  

如果用两个循环，那么可以第一个循环定长度，第二个遍历到倒数第n个，但是效率有点低。这里给出一种遍历一趟的，我们可以采用两个工作指针，第一个开始的时候就工作，第二个在第一个工作了n个后再开始工作，这样的话，如果第一个指针指向尾部，那么第二个指针刚好在倒数第n的前面。  

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190301194749807.png)  

所以就是简单的一个策略就能变两趟为一趟了。  

还有个问题是，这个题的链表是没有头结点的，所以可能在操作起来比较麻烦，我的策略是先增加一个头结点，等到使用完后，返回l.next即可。

## python代码  

```python
"""
Created on 2019/2/27 22:17
@File:删除链表的倒数第N个节点.py
@author: coderwangson
"""
"#codeing=utf-8"


# Definition for singly-linked list.
class ListNode(object):
    def __init__(self, x):
        self.val = x
        self.next = None

class Solution(object):
    def removeNthFromEnd(self, head, n):
        """
        :type head: ListNode
        :type n: int
        :rtype: ListNode
        """
        # 增加头结点，因为没有头结点做起来不好做
        l = ListNode(-1)
        l.next = head
        i = 1
        r = l
        q = l.next
        while q.next:
            if i >=n:
                r = r.next
            q = q.next
            i += 1

        if r.next:
            r.next = r.next.next
        return l.next
```






