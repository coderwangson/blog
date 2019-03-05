# Leetcode 21. Merge Two Sorted Lists 合并两个有序链表

标签： `Leetcode`

---

题目地址：https://leetcode-cn.com/problems/merge-two-sorted-lists/  

## 题目描述  

<p>将两个有序链表合并为一个新的有序链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。&nbsp;</p>

<p><strong>示例：</strong></p>

<pre><strong>输入：</strong>1-&gt;2-&gt;4, 1-&gt;3-&gt;4
<strong>输出：</strong>1-&gt;1-&gt;2-&gt;3-&gt;4-&gt;4
</pre>   

## 算法思想  

很常规的题目，如果学习过数据结构的话，就是把两个链表遍历一遍，然后根据大小去生成一个新的链表，只是写代码的时候要注意一些小细节，比如我做的时候是把结果插入到一个带的链表里面，这样使得处理起来更加方便。   

## python代码  

```python  

"""
Created on 2019/3/1 19:53
@File:合并两个有序链表.py
@author: coderwangson
"""
"#codeing=utf-8"


# Definition for singly-linked list.
class ListNode(object):
    def __init__(self, x):
        self.val = x
        self.next = None

class Solution(object):
    def mergeTwoLists(self, l1, l2):
        """
        :type l1: ListNode
        :type l2: ListNode
        :rtype: ListNode
        """
        # 返回结点的头结点
        h =ListNode(-1)
        h.next = None
        r = h

        while l1 and l2:
            if l1.val<l2.val:
                r.next = l1
                r = l1
                l1 = l1.next
            else:
                r.next =l2
                r = l2
                l2 = l2.next
        if l2:
            l1 = l2
        while l1:
            r.next = l1
            r = l1
            l1 = l1.next
        r.next = None
        return h.next

```  






