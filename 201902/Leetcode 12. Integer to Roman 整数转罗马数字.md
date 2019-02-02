#Leetcode 12. Integer to Roman 整数转罗马数字

标签： `Leetcode`

---

题目地址：https://leetcode-cn.com/problems/integer-to-roman/  

## 题目描述  

<p>罗马数字包含以下七种字符：&nbsp;<code>I</code>，&nbsp;<code>V</code>，&nbsp;<code>X</code>，&nbsp;<code>L</code>，<code>C</code>，<code>D</code>&nbsp;和&nbsp;<code>M</code>。</p>

<pre><strong>字符</strong>          <strong>数值</strong>
I             1
V             5
X             10
L             50
C             100
D             500
M             1000</pre>

<p>例如， 罗马数字 2 写做&nbsp;<code>II</code>&nbsp;，即为两个并列的 1。12 写做&nbsp;<code>XII</code>&nbsp;，即为&nbsp;<code>X</code>&nbsp;+&nbsp;<code>II</code>&nbsp;。 27 写做&nbsp;&nbsp;<code>XXVII</code>, 即为&nbsp;<code>XX</code>&nbsp;+&nbsp;<code>V</code>&nbsp;+&nbsp;<code>II</code>&nbsp;。</p>

<p>通常情况下，罗马数字中小的数字在大的数字的右边。但也存在特例，例如 4 不写做&nbsp;<code>IIII</code>，而是&nbsp;<code>IV</code>。数字 1 在数字 5 的左边，所表示的数等于大数 5 减小数 1 得到的数值 4 。同样地，数字 9 表示为&nbsp;<code>IX</code>。这个特殊的规则只适用于以下六种情况：</p>

<ul>
	<li><code>I</code>&nbsp;可以放在&nbsp;<code>V</code>&nbsp;(5) 和&nbsp;<code>X</code>&nbsp;(10) 的左边，来表示 4 和 9。</li>
	<li><code>X</code>&nbsp;可以放在&nbsp;<code>L</code>&nbsp;(50) 和&nbsp;<code>C</code>&nbsp;(100) 的左边，来表示 40 和&nbsp;90。&nbsp;</li>
	<li><code>C</code>&nbsp;可以放在&nbsp;<code>D</code>&nbsp;(500) 和&nbsp;<code>M</code>&nbsp;(1000) 的左边，来表示&nbsp;400 和&nbsp;900。</li>
</ul>

<p>给定一个整数，将其转为罗马数字。输入确保在 1&nbsp;到 3999 的范围内。</p>

<p><strong>示例&nbsp;1:</strong></p>

<pre><strong>输入:</strong>&nbsp;3
<strong>输出:</strong> "III"</pre>

<p><strong>示例&nbsp;2:</strong></p>

<pre><strong>输入:</strong>&nbsp;4
<strong>输出:</strong> "IV"</pre>

<p><strong>示例&nbsp;3:</strong></p>

<pre><strong>输入:</strong>&nbsp;9
<strong>输出:</strong> "IX"</pre>

<p><strong>示例&nbsp;4:</strong></p>

<pre><strong>输入:</strong>&nbsp;58
<strong>输出:</strong> "LVIII"
<strong>解释:</strong> L = 50, V = 5, III = 3.
</pre>

<p><strong>示例&nbsp;5:</strong></p>

<pre><strong>输入:</strong>&nbsp;1994
<strong>输出:</strong> "MCMXCIV"
<strong>解释:</strong> M = 1000, CM = 900, XC = 90, IV = 4.</pre>

## 算法思想  

### 直接法  

就是按照题目给的一步一步实现即可。在关于4和9的时候要注意进行转换。比如一个数字是4开头的，那就先判断是400还是40还是4，我们可以把这个数字与400,40，4进行相减，这样的话我们就能的出来其为400多还是40多，然后就根据具体情况给出相应字符。  

对于其他的字符，我们则判断是否构成一个1000或者500等等，这样的话我们只需要整除即可，然后把余数进行递归计算。  

### 分类法  

从题目描述，我们能看出来其实数字分为了1-3开头的一类，4开头的一类，5开头的一类，6-8开头的一类（其中6、7、8算作是5和I的组合），9开头的一类。然后再把这些单个的与其对应基数组合就行了，比如`111`：第一个是1开头是1百则为C，第二个1为十则对应X，最后一个为个对应I，所以对应罗马为`CXI`。再比如`1485`,其中1对应的是千则为M，然后4对应百，但是由于是4，所以其组合应该为CD，然后8对应十位，由于8是大于5但有不是9，所以8中包含5的部分以及其他则为LXXX，然后最后5为个位，则为V，所以最终结果是 `MCDLXXXV`。    

从上面例子看出代码就是把数字分解成一位位，然后根据具体情况做累计。


## 代码  

### 直接法 

```python 

class Solution:
    def intToRoman(self, num):
        """
        :type num: int
        :rtype: str
        """
        dict_char = {1:'I',5:'V',10:'X',50:'L',100:'C',500:'D',1000:'M'}
        special_num = [4,9,40,90,400,900]
        special_num_char ={4:'IV',9:'IX',
                           40:'XL',90:'XC',
                           400:'CD',900:'CM'}
        if num == 0:
            return ''
        if str(num)[0]=='4':
            base = 400
            #看是400多还是40多（因为题目中要求不超过3999）
            while num -base<0:
                base=base//10
            #把剩下的部分进行递归
            return special_num_char[base] + Solution().intToRoman(num - base)
        if str(num)[0]=='9':
            base = 900
            while num - base < 0:
                base = base // 10
            return special_num_char[base] + Solution().intToRoman(num - base)

        keys = sorted(list(dict_char.keys()),reverse=True)
        # 看是多个1000还是多个500等等
        for base in keys:
            if num//base>0:
                i = num//base
                # 把余数进行递归
                return dict_char[base]*i+Solution().intToRoman(num%base)
```

### 分类法   

```python
class Solution:
    def intToRoman(self, num):
        """
        :type num: int
        :rtype: str
        """
        roman =['M','D','C','L','X','V','I']
        nums =[1000,500,100,50,10,5,1]
        res =''
        # 按照10这类去找，分为1-3  4 5 6-8 9 五类
        for i in range(0,len(nums),2):
            # 如果当前不够，比如当前num是200，但是开始是除1000，则为0所以不影响
            x = num//nums[i]
            if x<4:
                res+=(roman[i]*x)
            if x==4:
                res+=roman[i]+roman[i-1]
            if x==5:
                # 算5的那部分
                res+=roman[i-1]
            if x>5 and x<9:
                # 多于5的那部分
                res+=(roman[i-1]+roman[i]*(x-5))
            if x==9:
                res+=roman[i]+roman[i-2]
            num = num%nums[i]
        return res
```   


## 参考  


> https://www.cnblogs.com/grandyang/p/4123374.html



