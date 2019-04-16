# Leetcode 49. Group Anagrams 字母异位词分组

标签： `Leetcode`

---

题目地址：https://leetcode-cn.com/problems/group-anagrams/  

## 题目描述   

<p>给定一个字符串数组，将字母异位词组合在一起。字母异位词指字母相同，但排列不同的字符串。</p>

<p><strong>示例:</strong></p>

<pre><strong>输入:</strong> <code>["eat", "tea", "tan", "ate", "nat", "bat"]</code>,
<strong>输出:</strong>
[
  ["ate","eat","tea"],
  ["nat","tan"],
  ["bat"]
]</pre>

<p><strong>说明：</strong></p>

<ul>
	<li>所有输入均为小写字母。</li>
	<li>不考虑答案输出的顺序。</li>
</ul>  

## 算法思想  

### 哈希+排序  

因为是字符串，所以如果对字符串排个序，如果排序后一样那么肯定就是属于同一组的，所以我们把拍过序的字符串作为key，构造一个字典，这样将来字典的值就是对应的答案，比如对于 'ate' 'eat' 二者的key都是 'aet'构造成字典就是{"aet":['ate','eat']}，但是在python中经过sorted后变为了list，而list不能hashable，所以把list转为touple或者字符串即可。  

###  计数法 

我觉得这个方法类似于桶排序，因为字母总共就26个，所以先声明一个大小为26，值为0数组`c`，然后遍历每个字符串的每个字符，然后把这个字符的位置的数组的值+=1，然后用这个数组构造一个字典，如果两个串用的字符一样，那么对应c也一样，比如`abbc`和`bbac`对应的肯定是`[121000...]`，因为a出现了一次，b出现了两次，c出现了一次。  

## python代码 

### 哈希+排序     

```python
class Solution(object):
    def groupAnagrams(self, strs):
        """
        :type strs: List[str]
        :rtype: List[List[str]]
        """
        d =  {}
        for s in strs:
            k = "".join(sorted(s))
            if d.get(k) ==None:
                d[k] =[]
            d[k].append(s)
        return d.values()
```

###  计数法   

```python
class Solution(object):
    def groupAnagrams(self, strs):
        """
        :type strs: List[str]
        :rtype: List[List[str]]
        """
        res = {}
        for s in strs:
            c = [0]*26
            for i in s:
                c[ord(i)-ord('a')]+=1
            k = tuple(c)
            if res.get(k) ==None:
                res[k] = []
            res[k].append(s)
        return res.values()
```  

参考： 

https://leetcode-cn.com/problems/group-anagrams/solution/





 