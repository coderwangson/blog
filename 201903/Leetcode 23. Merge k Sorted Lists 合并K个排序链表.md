# Leetcode 23. Merge k Sorted Lists 合并K个排序链表

标签： `Leetcode`

---

题目地址：https://leetcode-cn.com/problems/merge-k-sorted-lists/  

## 题目描述   

<p>合并&nbsp;<em>k&nbsp;</em>个排序链表，返回合并后的排序链表。请分析和描述算法的复杂度。</p>

<p><strong>示例:</strong></p>

<pre><strong>输入:</strong>
[
&nbsp; 1-&gt;4-&gt;5,
&nbsp; 1-&gt;3-&gt;4,
&nbsp; 2-&gt;6
]
<strong>输出:</strong> 1-&gt;1-&gt;2-&gt;3-&gt;4-&gt;4-&gt;5-&gt;6</pre>  

## 算法思想    

### 两两归并

原来的想法是两两归并，但是，就是利用以前的合并两个链表的方法，但是我提交了发现效果不是很好。不过在使用两两归并的时候我觉得也可以先进行按照长度排序，只是我没这样做。  

除此之外，在两两归并方法中，还有几个注意的。  

* 在做的时候，由于是两两归并，所以可以先取一个，然后拿这个和其他归并，这样省得后面的判断
* 要注意判断是否lists为空    

###  排序法  

算法如下：  

* 遍历lists并把所有结点值都放入一个列表
* 对列表排序 
* 重建链表  

在排序法中，其实还可以利用优先队列等高级结构存储，这样就能省下排序这一步。以后可以再加。  

## python代码  

### 两两归并

```python  

"""
Created on 2019/3/5 19:53
@File:合并K个排序链表.py
@author: coderwangson
"""
"#codeing=utf-8"


# Definition for singly-linked list.
class ListNode(object):
    def __init__(self, x):
        self.val = x
        self.next = None


class Solution(object):
    @staticmethod
    def mergeTwoLists(l1, l2):
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

    def mergeKLists(self, lists):
        """
        :type lists: List[ListNode]
        :rtype: ListNode
        """
        # 先把 lists里面的按照长度排序，这样避免长的先合并，导致效率低
        # sorted(lists,key=len)
        if len(lists)>0:
            l1 = lists[0]
        i = 1
        while i<len(lists):
            l1 = Solution.mergeTwoLists(l1,lists[i])
            i+=1
        return l1



```  

### 排序法  

```python  

"""
Created on 2019/3/5 19:53
@File:合并K个排序链表.py
@author: coderwangson
"""
"#codeing=utf-8"


# Definition for singly-linked list.
class ListNode(object):
    def __init__(self, x):
        self.val = x
        self.next = None

class Solution(object):
    def mergeKLists(self, lists):
        """
        :type lists: List[ListNode]
        :rtype: ListNode
        """
        all_node = []
        for l in lists:
            p = l
            while p:
                all_node.append(p.val)
                p = p.next
        # 这样插入排序就可以头插法了
        all_node = sorted(all_node,reverse=True)
        h = ListNode(-1)
        h.next = None
        for n in all_node:
            node = ListNode(n)
            node.next = h.next
            h.next = node
        return h.next


```






