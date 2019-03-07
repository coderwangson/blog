# Leetcode 24. Swap Nodes in Pairs 两两交换链表中的节点

标签： `Leetcode`

---

<p>给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。</p>

<p><strong>你不能只是单纯的改变节点内部的值</strong>，而是需要实际的进行节点交换。</p>

<p>&nbsp;</p>

<p><strong>示例:</strong></p>

<pre>给定 <code>1-&gt;2-&gt;3-&gt;4</code>, 你应该返回 <code>2-&gt;1-&gt;4-&gt;3</code>.
</pre>   

## 算法思想   

其实主要考察的就是链表，我的方案是先增加一个头结点，然后以头结点作为工作节点，每次需要做的就是把工作节点的后面两个结点交换位置，比如当前工作节点为`pre`,则每次只需要把`pre.next`和`pre.next.next`交换即可。可以通过如下语句交换：     

        p = pre.next
        q = p.next

        pre.next = q
        p.next = q.next
        q.next =p  
        
## python代码  

```python  

class Solution(object):
    def swapPairs(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
       #可以看做是遍历一遍，只是每此把遍历的这个结点的后两个交换下位置即可
        h = ListNode(-1)
        h.next = head
        pre = h
        # 如果后面有两个结点
        while pre.next and pre.next.next:
            p = pre.next
            q = p.next

            pre.next = q
            p.next = q.next
            q.next =p

            pre = p

        return h.next
```




