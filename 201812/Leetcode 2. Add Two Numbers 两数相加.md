# Leetcode 2. Add Two Numbers 两数相加

标签： Leetcode

---  
题目地址：https://leetcode-cn.com/problems/add-two-numbers/

##  题目描述  

给出两个非空的链表用来表示两个非负的整数。其中，它们各自的位数是按照 逆序 的方式存储的，并且它们的每个节点只能存储一位数字。  

如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。  

您可以假设除了数字 0 之外，这两个数都不会以 0 开头。  

示例：  
输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 0 -> 8
原因：342 + 465 = 807  

## 算法描述  

这道题比较简单，其实就是考察了单链表的遍历，然后遍历的时候相加，然后记得有一个进位，关键就是细节的处理，比如进位的操作，如果最后都遍历完要记得处理进位，以及当一个表遍历结束后记得怎么处理或者初始的时候一个表就是空表该怎么处理。所以思想就是遍历一遍，然后把对应位置的元素相加并且加上进位的元素，再对10进行整除以及取余，整除作为下一次的进位，余数作为当前位置的加的输出。   

对比答案得出的两个细节问题：  
1. 在存储结果的时候，我存在res中我做的是不带头结点的链表，这样的话插入操作会分第一个结点或者是其他结点，所以比较麻烦，但是答案用的是带头结点的做插入操作，然后最后返回了一个res.next即可。  
2. 在进行处理的时候我是在两个表一个为空的时候再单独处理另外一个，但是答案是在一个表为空的时候把那个取出的值变为0，这样可以统一操作，使得代码更加简洁。  


## python代码  

    # Definition for singly-linked list.
    class ListNode(object):
        def __init__(self, x):
            self.val = x
            self.next = None
    class Solution(object):
        def addTwoNumbers(self, l1, l2):
            """
            :type l1: ListNode
            :type l2: ListNode
            :rtype: ListNode
            """
            p = l1
            q = l2
            res = ListNode(0)
            r = res
            s = 0 # 进位
            # 当然也可以做成带头结点返回为res.next即可
            while(p!=None or q!=None):
                p_v = 0 if p==None else p.val
                q_v = 0 if q==None else q.val
                r_v = (p_v+q_v+s)%10
                s = (p_v+q_v+s)//10
                r.next = ListNode(r_v)
                r = r.next
                r.next = None
                # 如果不为None的时候继续遍历，否则停止遍历
                p =p if p==None else p.next
                q =q if q==None else q.next
            if s !=0:
                r.next = ListNode(s)
                r = r.next
                r.next = None
            return res.next
    l1 = ListNode(0)
    l2 = ListNode(1)
    l2.next = ListNode(8)      
    s = Solution()
    print(s.addTwoNumbers(l1, l2))   








