# Leetcode 4. Median of Two Sorted Arrays 寻找两个有序数组的中位数

标签： Leetcode

---
题目地址：https://leetcode-cn.com/problems/median-of-two-sorted-arrays/  
## 题目描述  
给定两个大小为 m 和 n 的有序数组 nums1 和 nums2。
请你找出这两个有序数组的中位数，并且要求算法的时间复杂度为 $O(log(m + n))$。
你可以假设 nums1 和 nums2 不会同时为空。
示例 1:
nums1 = [1, 3]
nums2 = [2]

则中位数是 2.0 

示例 2:

nums1 = [1, 2]
nums2 = [3, 4]
则中位数是 (2 + 3)/2 = 2.5  

## 题目解释  

这个题目有一种变形可以解释成求Topk问题，比如给两个有序数组，给出一种算法求出第k小元素，对于中位数问题相当于求第（m+n）/2个元素，这道题的难点不在于题目，而在于其复杂度，我想$O(m+n)$ 的算法还是比较容易想的到的，直接把两个有序表合并，然后无论是求Top几我们都能轻松的求出来，但是这样的话和题目要求不符，因为题目要求$O(log(m + n))$。  

这里给出一种中心扩散法，我没有计算其复杂度是多少，但是提交上去其耗时为68 ms并且也被AC了，除了这种方法还有一种叫做manacher（马拉车）的算法，有兴趣的话可以自行了解。   

## 算法思想  

### 中心扩散法  
  
对于两个有序数组要取出来topk，肯定是在第一个数组里面取几个，第二个数组里面也取出来几个，如果二者取出来的和刚好是k，就找到了topk的第k个。  
那么我们可以假定在第一个数组里面取到了L1这个位置，L1后面的那个元素假定为R1，那么根据我们前面所说第二个数组取到的肯定就是k-L1 -2 。  

比如：`a1 = [1,5,7,9]  a2 = [2,10,18]`,如果想取top4 ，并且假如L1为1这个位置，代表a1中取了2个元素，那么a2中也只能再取2元素，位置为 = 4 -L1-1-1。  

从上面例子我们也看出来了为什么叫做中心扩散了，如果是求中位数的话，我们一般把`L1 = n/2`，然后基于这个点，再做调整。  

还拿上面那个例子来说，经过一次取肯定不行，因为既然是取top 4那么我们要求取到的元素左边都要小于右边的，这个对于L1和R1是肯定的，因为a1本身有序，但是L2和R1以及L1和R2就不一定了，比如上面例子a2[L2] >a1[R1]，所以我们就需要作出相应调整，我们把L2向左扩散就行了，就是把L2向左移一个位置，因为L2向左移了，所以对应L1应该向右移，这样几次操作就会找到topk的位置。  

两个小细节问题：  

1. 两个数组的和是奇数还是偶数，比如`[1,2],[3,4]` ，这样的话最终中位数的值是（2+3/2），如果是`[1,2],[3,4,5]`结果就是（3），所以最后要做判断。左边的取最大的，右边用最小的。  
2. 如果L1或者L2移动到小于0的位置了，那么就把对应位置的值赋为-inf（负无穷），同样如果超过了长度就把R对应位置赋为inf。  
### 举个栗子  

初始情况：  
a1 = [1,2] a2 = [3,4]  

a1长度n = 2，a2长度m = 2  

初始化:
`L1 = n/2 =1，R1 = L1+1=2
L2 = (m+n)/2 - L1-1-1 = -1,R2 = L2+1 = 0  
L1_v = a1[L1] = 2,R1_v = inf  
L2_v = -inf,R2_v = 3`  

然后进行比较因为`L1_v<R2_v and L2_v<R2_v`所以刚好取到了，又因为是偶数所以最终结果就是`(max(L1_v,L2_v)+min(R1_v,R2_v))/2 = 2.5`  

## python代码  

代码里面有些小细节的处理，比如每次都把第一个数组置位最短的，因为扩散是从第一个开始的，这样可以节省时间，还有就是如果有一个为空的怎么办。  
还有注意python中整除操作。 

```python
# 68 ms
class Solution(object):
    def findMedianSortedArrays(self, nums1, nums2):
        """
        :type nums1: List[int]
        :type nums2: List[int]
        :rtype: float
        """
        # 中心扩散法
        # > ref https://blog.csdn.net/hk2291976/article/details/51107778
        # nums1是最短的那个
        n = len(nums1)
        m = len(nums2)
        if m<n:
            return self.findMedianSortedArrays(nums2,nums1)
        # 避免有个为空
        if n == 0:
            #m//2 -(m+1)%2因为是奇数的话(m+1)%2 =0偶数的话等于1，相当于用数学避免一个ifelse
            return  (nums2[m//2] +nums2[m//2 -(m+1)%2])/2.0
        L1 = n//2
        R1 = L1+1
        L2 = (m+n)//2 - (L1+1)-1
        R2 = L2+1
        L1_v = nums1[L1] if L1>-1 else float("-inf")
        R1_v = nums1[R1] if R1<n else float("inf")
        L2_v = nums2[L2] if L2>-1 else float("-inf")
        R2_v = nums2[R2] if R2<m else float("inf")
        # 两个都满足扩散结束
        while L1_v >R2_v or L2_v>R1_v:
            # L1向左边扩散并且重新计算其他的值
            if L1_v >R2_v:
                L1 = L1 -1
                R1 = R1-1
                R1_v = L1_v
                L1_v = nums1[L1] if L1>-1 else float("-inf")
                L2 = L2+1
                R2 = R2 +1
                L2_v = R2_v
                R2_v = nums2[R2] if R2<m else float("inf")
             # L2向右边扩散并且重新计算其他的值
            else:
                L2 = L2 -1
                R2 = R2 -1
                R2_v = L2_v
                L2_v = nums2[L2] if L2>-1 else float("-inf")
                L1 = L1+1
                R1 = R1+1
                L1_v = R1_v
                R1_v = nums1[R1] if R1<n else float("inf")
        if (m+n) %2 !=0:
            return min(R1_v,R2_v)
        else:
            return (max(L1_v,L2_v)+min(R1_v,R2_v))/2.0
```   

参考：
我是参考这篇博客写出来的，并且该博客给出来了另一种优化的方法，就是可以避免奇偶，但是由于把数组扩大了，所以耗时较长，测试为112 ms，不过他的博客写的比较清楚，所以如果我写的看不太懂的话，可以参考这个博文，然后回来用我这个代码。  

>https://blog.csdn.net/hk2291976/article/details/51107778









