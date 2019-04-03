# Leetcode 42. Trapping Rain Water 接雨水

标签： `Leetcode`

---

题目地址：https://leetcode-cn.com/problems/trapping-rain-water/  
## 题目描述  

<p>给定&nbsp;<em>n</em> 个非负整数表示每个宽度为 1 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。</p>

<p><img src="https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/22/rainwatertrap.png" style="height: 161px; width: 412px;"></p>

<p><small>上面是由数组 [0,1,0,2,1,0,1,3,2,1,2,1] 表示的高度图，在这种情况下，可以接 6 个单位的雨水（蓝色部分表示雨水）。&nbsp;<strong>感谢 Marcos</strong> 贡献此图。</small></p>

<p><strong>示例:</strong></p>

<pre><strong>输入:</strong> [0,1,0,2,1,0,1,3,2,1,2,1]
<strong>输出:</strong> 6</pre>  

## 算法思想   

这道题看似和之前的一道`11 盛最多水的容器`很像，但是又不是一种类型的，不过这个题目也是找到方法做起来就很简单，关键是总结出来规律，因为我们无论多少个柱子排列，最后肯定有一个柱子最高，我们就把他找出来。  

然后就分成了两个部分，左部分和右部分，对于左部分来说，我们从左向右走，因为右边肯定有东西挡着（最高的那个），所以不用担心。 所以我们先以最左边的为左边界，然后遍历，如果遍历到的比底要高，则无法存水，所以更新底，否则有办法存水，则把这一个小格子的水加到面积里面。也就是下面这段代码：  

     root = height[0]
            for i in range(0,maxIndex):
                if height[i]>root : root = height[i]
                else: area += (root - height[i])  
                
右边同理即可。  

## python代码  

```python  

class Solution:
    def trap(self, height):
        """
        :type height: List[int]
        :rtype: int
        """
        if len(height)<=2:
            return 0

        maxIndex = height.index(max(height))
        area = 0

        root = height[0]
        for i in range(0,maxIndex):
            if height[i]>root : root = height[i]
            else: area += (root - height[i])

        root = height[len(height)-1]
        for i in reversed(range(maxIndex,len(height))):
            if height[i]>root : root = height[i]
            else: area += (root - height[i])
        return area

``` 

参考：  

https://www.cnblogs.com/felixfang/p/3713197.html




