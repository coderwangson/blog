# Leetcode 31. Next Permutation 下一个排列

标签： `Leetcode`

---

题目地址：https://leetcode-cn.com/problems/next-permutation/  

## 题目描述   

<p>实现获取下一个排列的函数，算法需要将给定数字序列重新排列成字典序中下一个更大的排列。</p>

<p>如果不存在下一个更大的排列，则将数字重新排列成最小的排列（即升序排列）。</p>

<p>必须<strong><a href="https://baike.baidu.com/item/%E5%8E%9F%E5%9C%B0%E7%AE%97%E6%B3%95" target="_blank">原地</a></strong>修改，只允许使用额外常数空间。</p>

<p>以下是一些例子，输入位于左侧列，其相应输出位于右侧列。<br>
<code>1,2,3</code> → <code>1,3,2</code><br>
<code>3,2,1</code> → <code>1,2,3</code><br>
<code>1,1,5</code> → <code>1,5,1</code></p>  


## 算法思想   

这道题的想法并不是我自己想出来的，也是参考[题解][1]进行做的，只是改写`python`的。答案的意思是，`如果一个数列本身是逆序，我们肯定就找不到更大的了`，所以我们就从最后一个元素向前面找（也就是个位），如果两个相邻元素，左边比右边大，那么接着往前找（因为这样不可能有更大的了），直到找到一个点，左边比右边小，说明这个点是转折点，因为这个点小，所以只需要找到比这个点大的一个点，然后互换，则得到一个更大的序列，而题目要求是下一个更大的序列，所以我么需要从更大的序列中选出一个最小的，而我们后面这些元素本身是逆序，所以只需要翻转一下即可，比如`x985`的中的最小的为`x589`(x就是刚才交换的那个点)。  

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190317083905771.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODg4ODM3,size_16,color_FFFFFF,t_70)  

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190317084226234.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODg4ODM3,size_16,color_FFFFFF,t_70)  

从图中可以看出，把`a[i-1]`和`a[j]`互换，然后把x后面的逆序即可。关键在于找到`a[i-1]和a[j]`，其实就是搜索一下即可。  

## python代码  

```python
#ref https://leetcode-cn.com/problems/next-permutation/solution/
class Solution(object):
    def nextPermutation(self, nums):
        """
        :type nums: List[int]
        :rtype: None Do not return anything, modify nums in-place instead.
        """
        # 如果用直接用i作为基准，那么需要判断i-1是否越过0，所以用i_1作为基准--
        # len(nums)-1 是最后一个下标
        i_1 = len(nums)-1 -1 # i_1 代表i-1
        while i_1>= 0 and nums[i_1]>=nums[i_1+1]:
            i_1 -=1
        # 找到一个刚好比i_1大的值
        if i_1>=0:
            j = len(nums)-1
            while j>=0 and nums[j]<=nums[i_1]:
                j-=1
            nums[i_1],nums[j] = nums[j],nums[i_1]
        # 从i开始反转一下（变得为最小了） 这样才是下一个
        i = i_1 + 1
        self.reverse(nums,i,len(nums)-1)
        return None
    def reverse(self,nums,i,j):
        while i<j:
            nums[i],nums[j] = nums[j],nums[i]
            i+=1
            j-=1
            
```


  [1]: https://leetcode-cn.com/problems/next-permutation/solution/