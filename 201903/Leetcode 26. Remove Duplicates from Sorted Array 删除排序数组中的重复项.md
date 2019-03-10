# Leetcode 26. Remove Duplicates from Sorted Array 删除排序数组中的重复项

标签： `Leetcode`

---

题目地址：https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/  

## 题目描述  
<p>给定一个排序数组，你需要在<strong><a href="http://baike.baidu.com/item/%E5%8E%9F%E5%9C%B0%E7%AE%97%E6%B3%95" target="_blank">原地</a></strong>删除重复出现的元素，使得每个元素只出现一次，返回移除后数组的新长度。</p>

<p>不要使用额外的数组空间，你必须在<strong><a href="https://baike.baidu.com/item/%E5%8E%9F%E5%9C%B0%E7%AE%97%E6%B3%95" target="_blank">原地</a>修改输入数组</strong>并在使用 O(1) 额外空间的条件下完成。</p>

<p><strong>示例&nbsp;1:</strong></p>

<pre>给定数组 <em>nums</em> = <strong>[1,1,2]</strong>, 

函数应该返回新的长度 <strong>2</strong>, 并且原数组 <em>nums </em>的前两个元素被修改为 <strong><code>1</code></strong>, <strong><code>2</code></strong>。 

你不需要考虑数组中超出新长度后面的元素。</pre>

<p><strong>示例&nbsp;2:</strong></p>

<pre>给定<em> nums </em>= <strong>[0,0,1,1,1,2,2,3,3,4]</strong>,

函数应该返回新的长度 <strong>5</strong>, 并且原数组 <em>nums </em>的前五个元素被修改为 <strong><code>0</code></strong>, <strong><code>1</code></strong>, <strong><code>2</code></strong>, <strong><code>3</code></strong>, <strong><code>4</code></strong>。

你不需要考虑数组中超出新长度后面的元素。
</pre>

<p><strong>说明:</strong></p>

<p>为什么返回数值是整数，但输出的答案是数组呢?</p>

<p>请注意，输入数组是以<strong>“引用”</strong>方式传递的，这意味着在函数里修改输入数组对于调用者是可见的。</p>

<p>你可以想象内部操作如下:</p>

<pre>// <strong>nums</strong> 是以“引用”方式传递的。也就是说，不对实参做任何拷贝
int len = removeDuplicates(nums);

// 在函数里修改输入数组对于调用者是可见的。
// 根据你的函数返回的长度, 它会打印出数组中<strong>该长度范围内</strong>的所有元素。
for (int i = 0; i &lt; len; i++) {
&nbsp; &nbsp; print(nums[i]);
}
</pre>  

## 算法思想  

其要求要在常数空间内完成，所以不能再新建一个数组。这里其实用了一个类似链表的思想，我们用`i,j`两个下标，一个代表新的数组的尾部，一个代表原数组的下标。所以我们只需要使用`j`进行遍历一遍，在遍历的过程中，如果这个元素和前面的不相等（由于元素是有序的），那么我们就插入到`i`为 下标的数组中，达到去重的作用。   

## python代码  

```python
class Solution(object):
    def removeDuplicates(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        j = 0
        if len(nums) ==0:
            return j
        for i in range(1,len(nums)):
            if nums[i] ==nums[j]:
                continue
            else:
                j+=1
                nums[j] = nums[i]
        return j+1
```




