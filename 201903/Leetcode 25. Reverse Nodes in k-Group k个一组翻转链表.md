# Leetcode 25. Reverse Nodes in k-Group k个一组翻转链表

标签（空格分隔）： `Leetcode`

---

题目地址：https://leetcode-cn.com/problems/reverse-nodes-in-k-group/  

## 题目描述  

<p>给出一个链表，每&nbsp;<em>k&nbsp;</em>个节点一组进行翻转，并返回翻转后的链表。</p>

<p><em>k&nbsp;</em>是一个正整数，它的值小于或等于链表的长度。如果节点总数不是&nbsp;<em>k&nbsp;</em>的整数倍，那么将最后剩余节点保持原有顺序。</p>

<p><strong>示例 :</strong></p>

<p>给定这个链表：<code>1-&gt;2-&gt;3-&gt;4-&gt;5</code></p>

<p>当&nbsp;<em>k&nbsp;</em>= 2 时，应当返回: <code>2-&gt;1-&gt;4-&gt;3-&gt;5</code></p>

<p>当&nbsp;<em>k&nbsp;</em>= 3 时，应当返回: <code>3-&gt;2-&gt;1-&gt;4-&gt;5</code></p>

<p><strong>说明 :</strong></p>

<ul>
	<li>你的算法只能使用常数的额外空间。</li>
	<li><strong>你不能只是单纯的改变节点内部的值</strong>，而是需要实际的进行节点交换。</li>
</ul>

## 算法思想  

通过题目可以看出来有两个要求：  

* k个一组  
* 翻转

对于k个一组，我们可以通过遍历即可，每隔k个，我们操作一次，而对于翻转，其实可以利用链表的头插法进行，我们对于k个结点，我们可以按照头插法的方法重新组织成一个链表，这样就达到了翻转的目的，然后把每k个再按照尾插法插入到结果链表中即可。  
在操作的时候注意，有些结点的值可能会被修改，所以提前存储，并且都最好加上头结点，这样方便插入操作。  

在代码中，我们用i控制个数，每次当i%k==0,我们就进行头插法，我们头插法传入的是k个结点的头和尾，并且最终放回一个翻转后的头结点。初始情况`pre = last`，这个时候说明有一个结点，所以i=1，然后开始遍历，在这个过程中`i+=1`。注意里面的一些pre_tmp这些都是先临时存储，因为每次我们头插结束，后面的结点都接不上了，所以先临时存储一下。

## python代码  

```python  

class Solution(object):
    def headInsertByN(self,pre,last):
        # 把pre->last之间的结点逆转
        # 其实就是把pre->last进行头插法，最终返回L即可
        L = ListNode(-1)
        L.next = None
        r = pre
        end = last.next
        while pre != end:
            p = pre
            pre = pre.next

            # 头插
            p.next = L.next
            L.next = p

        return L.next,r

    def reverseKGroup(self, head, k):
        """
        :type head: ListNode
        :type k: int
        :rtype: ListNode
        """
        # 遍历，每次把部分结点传入上面那个逆转的函数
        # 然后把所有逆转后的结点尾插即可
        L = ListNode(-1)
        L.next = None
        r = L
        pre = head
        last = pre
        i = 1
        while pre and last:
            if i%k ==0:
                pre_tmp = last.next
                p,r_temp = Solution().headInsertByN(pre,last)
                r.next = p
                r = r_temp
                pre = pre_tmp
                last = pre
                i = 1
                continue
            last = last.next
            i+=1
        r.next = pre
        return L.next
```  

参考：https://blog.csdn.net/qq_17550379/article/details/80696835





