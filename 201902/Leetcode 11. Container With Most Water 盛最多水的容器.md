#Leetcode 11. Container With Most Water 盛最多水的容器

标签： `Leetcode`

---

题目地址： https://leetcode-cn.com/problems/container-with-most-water/  
## 题目   
给定 n 个非负整数 a1，a2，...，an，每个数代表坐标中的一个点 (i, ai) 。在坐标内画 n 条垂直线，垂直线 i 的两个端点分别为 (i, ai) 和 (i, 0)。找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。
说明：你不能倾斜容器，且 n 的值至少为 2。  

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190202183902845.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODg4ODM3,size_16,color_FFFFFF,t_70)  

图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。  

示例:

    输入: [1,8,6,2,5,4,8,3,7]
    输出: 49   
    
## 算法思想  

### 暴力法  

从题目描述可以看出，决定整个面积的主要因素是比较短的那个，所以我们可以从第一个数据$a_1$开始作为一个边，然后与其他的边$a_j$组成一个区域，然后计算面积`area = (j-1)*min(a1,aj)`，这样我们把所有可能都遍历一遍即可，但是使用python由于效率问题，所以会超时。    

### 双指针法
该方法是官方给出的一种解答，它设置了两个指针，一个指在第一个边，一个指在最后一个边，这样的话构成一个面积，然后我们可以做的是往中间找，看是否有更加优的结果。那关键就在于怎么往中间走，由于我们知道这个决定当前面积的是那个比较短的，所以我们可以把比较短的那个指针往里面挪，这样我们就有机会得到更优面积。比如初始状态：`[10,2,10,1]`,刚开始双指针一个指向10一个指向1面积是（3*1），我们把较短那个（1）往里面挪则也指向10（第三个10）那么面积变为了（2*10）所以就增大了。  

但是如果我们移动的是较长那个，那么面积一定减小，因为如果较长那个移动到了一个同样较长的上面，那么由于向里面挪了，所以宽变小了，而如果移动到了一个更短的上面，那么同样最终宽也小了，高也小了。   

所以我们得出结论，双指针移动就是移动较短那个指针向内移动即可。

##  代码  

### 暴力法代码   

```pthon  

class Solution:
    def maxArea(self, height):
        """
        :type height: List[int]
        :rtype: int
        """
        area = 0
        for i in range(len(height)):
            for j in range(i+1,len(height)):
                width = j-i
                high = min(height[i],height[j])
                area = width * high if (width*high)>area else area
        return area
```  

### 双指针法  
```python
class Solution:
    def maxArea(self, height):
        """
        :type height: List[int]
        :rtype: int
        """
        area = 0
        i = 0
        j = len(height)-1
        while i<j:
            width = j-i
            high = min(height[i],height[j])
            newArea = width * high
            area = newArea if newArea>area else area
            if height[i]>height[j]:
                j-=1
            else:
                i+=1
        return area
```  

## 参考   

>https://www.cnblogs.com/wmx24/p/9043447.html






