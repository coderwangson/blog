# Leetcode 75. 颜色分类 

标签： `Leetcode`

--- 

题目地址：https://leetcode-cn.com/problems/sort-colors/

## 题目描述  

<p>给定一个包含红色、白色和蓝色，一共&nbsp;<em>n </em>个元素的数组，<strong><a href="https://baike.baidu.com/item/%E5%8E%9F%E5%9C%B0%E7%AE%97%E6%B3%95">原地</a></strong>对它们进行排序，使得相同颜色的元素相邻，并按照红色、白色、蓝色顺序排列。</p>

<p>此题中，我们使用整数 0、&nbsp;1 和 2 分别表示红色、白色和蓝色。</p>

<p><strong>注意:</strong><br>
不能使用代码库中的排序函数来解决这道题。</p>

<p><strong>示例:</strong></p>

<pre><strong>输入:</strong> [2,0,2,1,1,0]
<strong>输出:</strong> [0,0,1,1,2,2]</pre>

<p><strong>进阶：</strong></p>

<ul>
	<li>一个直观的解决方案是使用计数排序的两趟扫描算法。<br>
	首先，迭代计算出0、1 和 2 元素的个数，然后按照0、1、2的排序，重写当前数组。</li>
	<li>你能想出一个仅使用常数空间的一趟扫描算法吗？</li>
</ul>  

## 算法描述  

因为就对三个数字排序，可以利用快排中的一趟就能完成，我们用`i`表示0的右边界，用`j`表示2的左边界，用curr遍历。  

如果`nums[curr]==2` ,我们将curr的值和j的交换一下，并且更新j。  

如果`nums[curr]==0` ,我们将curr的值和i的交换一下，并且更新i。  

其他情况直接curr+=1。  

这里还有一个在`nums[curr]==0`的时候，不仅仅更新i，也更新一下curr，因为我们当前能确定了curr的位置的值是从前面过来的，所以肯定是1，因为0和2都被置换了，所以可以把curr更新。  

## python代码  

```python
class Solution(object):
    def sortColors(self, nums):
        """
        :type nums: List[int]
        :rtype: None Do not return anything, modify nums in-place instead.
        """
        i,j = 0,len(nums)-1 # i代表0的右边界（nums[i]!=0 nums[i-1]=0），j代表2的左边界
        curr = 0 # 当前遍历的位置
        while curr<=j:
            if nums[curr]==2:
                nums[curr],nums[j] = nums[j],nums[curr] # 把curr位置的元素和j换一下
                j-=1 # 换完因为nums[j]=2 所以边界左移
            elif nums[curr]==0:
                nums[curr], nums[i] = nums[i], nums[curr]
                i+=1 # 边界右移
                curr+=1 # 因为curr是从i那边过来的，所以交换过的curr也不可能为2了，所以curr可以直接右移了
            else:
                curr+=1 # 如果上面进行了交换数据，不能移动curr如[1,2,0] 只有不交换数据才能移动
                # 因为移动后当前的元素内容变了

        print(nums)

```  

欢迎大家关注我的微信公众号，未来上面会推送`python` `机器学习` `算法学习` `深度学习` `论文阅读` 以及偶尔的`小鸡汤`等内容。ようこそいらっしゃい！  
 
搜索 coderwangson 关注

![image](http://wx4.sinaimg.cn/large/005Dd0fOly1g48r27k7trj307607674r.jpg)






