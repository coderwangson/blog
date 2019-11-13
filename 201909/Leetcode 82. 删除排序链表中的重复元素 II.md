# Leetcode 82. 删除排序链表中的重复元素 II

标签：`Leetcode`

---

题目地址： https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list-ii/  

## 题目描述  

<p>给定一个排序链表，删除所有含有重复数字的节点，只保留原始链表中&nbsp;<em>没有重复出现&nbsp;</em>的数字。</p>

<p><strong>示例&nbsp;1:</strong></p>

<pre><strong>输入:</strong> 1-&gt;2-&gt;3-&gt;3-&gt;4-&gt;4-&gt;5
<strong>输出:</strong> 1-&gt;2-&gt;5
</pre>

<p><strong>示例&nbsp;2:</strong></p>

<pre><strong>输入:</strong> 1-&gt;1-&gt;1-&gt;2-&gt;3
<strong>输出:</strong> 2-&gt;3</pre>   

## 算法思想  

和上面的第80题，删除重复元素的原理一样，都是按照插入排序来做，但是这里是说如果重复的话，都要删除，所以要加一个判断，这里先用一个tmp临时存储上一个元素，然后当遍历到下一个待插入元素时，把待插入元素和tmp以及待插入元素的下个元素都比较一下，只有都不相等，才能插入。  

除此之外，因为是链表，所以用了一个小的技巧，用了一个带头指针的链表作为新的链表，这样方便操作。

## python代码  

```python
class Solution(object):
    def deleteDuplicates(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
        if head == None or head.next == None:
            return head
        new_head = ListNode(None)
        tmp = None
        r = new_head
        while head:
            next = head.next
            if (next ==None and head.val !=tmp) or (next!=None and head.val!=tmp and head.val !=next.val):
                r.next = head
                r = head
                r.next = None
            tmp = head.val
            head = next



        return new_head.next
```





