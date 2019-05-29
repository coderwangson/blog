# Leetcode 68. 文本左右对齐 Text Justification

标签： `Leetcode`

---

题目地址：https://leetcode-cn.com/problems/text-justification/  

## 题目描述  

<p>给定一个单词数组和一个长度&amp;nbsp;<em>maxWidth</em>，重新排版单词，使其成为每行恰好有&amp;nbsp;<em>maxWidth</em>&amp;nbsp;个字符，且左右两端对齐的文本。</p>

<p>你应该使用&amp;ldquo;贪心算法&amp;rdquo;来放置给定的单词；也就是说，尽可能多地往每行中放置单词。必要时可用空格&amp;nbsp;<code>&amp;#39; &amp;#39;</code>&amp;nbsp;填充，使得每行恰好有 <em>maxWidth</em>&amp;nbsp;个字符。</p>

<p>要求尽可能均匀分配单词间的空格数量。如果某一行单词间的空格不能均匀分配，则左侧放置的空格数要多于右侧的空格数。</p>

<p>文本的最后一行应为左对齐，且单词之间不插入<strong>额外的</strong>空格。</p>

<p><strong>说明:</strong></p>

<ul>
	<li>单词是指由非空格字符组成的字符序列。</li>
	<li>每个单词的长度大于 0，小于等于&amp;nbsp;<em>maxWidth</em>。</li>
	<li>输入单词数组 <code>words</code>&amp;nbsp;至少包含一个单词。</li>
</ul>


<p>给定一个单词数组和一个长度&nbsp;<em>maxWidth</em>，重新排版单词，使其成为每行恰好有&nbsp;<em>maxWidth</em>&nbsp;个字符，且左右两端对齐的文本。</p>

<p>你应该使用“贪心算法”来放置给定的单词；也就是说，尽可能多地往每行中放置单词。必要时可用空格&nbsp;<code>' '</code>&nbsp;填充，使得每行恰好有 <em>maxWidth</em>&nbsp;个字符。</p>

<p>要求尽可能均匀分配单词间的空格数量。如果某一行单词间的空格不能均匀分配，则左侧放置的空格数要多于右侧的空格数。</p>

<p>文本的最后一行应为左对齐，且单词之间不插入<strong>额外的</strong>空格。</p>

<p><strong>说明:</strong></p>

<ul>
	<li>单词是指由非空格字符组成的字符序列。</li>
	<li>每个单词的长度大于 0，小于等于&nbsp;<em>maxWidth</em>。</li>
	<li>输入单词数组 <code>words</code>&nbsp;至少包含一个单词。</li>
</ul>

<p><strong>示例:</strong></p>

<pre><strong>输入:</strong>
words = ["This", "is", "an", "example", "of", "text", "justification."]
maxWidth = 16
<strong>输出:</strong>
[
&nbsp; &nbsp;"This &nbsp; &nbsp;is &nbsp; &nbsp;an",
&nbsp; &nbsp;"example &nbsp;of text",
&nbsp; &nbsp;"justification. &nbsp;"
]
</pre>

<p><strong>示例&nbsp;2:</strong></p>

<pre><strong>输入:</strong>
words = ["What","must","be","acknowledgment","shall","be"]
maxWidth = 16
<strong>输出:</strong>
[
&nbsp; "What &nbsp; must &nbsp; be",
&nbsp; "acknowledgment &nbsp;",
&nbsp; "shall be &nbsp; &nbsp; &nbsp; &nbsp;"
]
<strong>解释: </strong>注意最后一行的格式应为 "shall be    " 而不是 "shall     be",
&nbsp;    因为最后一行应为左对齐，而不是左右两端对齐。       
     第二行同样为左对齐，这是因为这行只包含一个单词。
</pre>

<p><strong>示例&nbsp;3:</strong></p>

<pre><strong>输入:</strong>
words = ["Science","is","what","we","understand","well","enough","to","explain",
&nbsp;        "to","a","computer.","Art","is","everything","else","we","do"]
maxWidth = 20
<strong>输出:</strong>
[
&nbsp; "Science &nbsp;is &nbsp;what we",
  "understand &nbsp; &nbsp; &nbsp;well",
&nbsp; "enough to explain to",
&nbsp; "a &nbsp;computer. &nbsp;Art is",
&nbsp; "everything &nbsp;else &nbsp;we",
&nbsp; "do &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;"
]
</pre>  

## 算法思想 

这道题目主要就是条件理清楚，遍历一遍`words`，每次找几个单词长度之和是小于`maxWidth`的（这里注意，当找第二个单词的时候，长度多加1，预备一个空格把单词隔开）。当找到这一串单词后，就可以向单词里面插入空格了，先计算总共可以插入的多余空格，然后把这些空格均分，而剩下的空格依次插入到左边。比如选了3个单词，我们有7个空格待用，那么每个均分`7//(3-1) = 3`，多余的为`7%(3-1) = 1`，所以多余这1个就插入左边，因为题目说了左边要不右边多。  

除此之外还要注意，最后一波只需要单词间隔开即可。而最后一波就是跳出循环那一波，因为如果循环结束还没有凑够`maxWidth`，那么一定就是最后一波了。  

## python代码 

```python
class Solution(object):
    def fullJustify(self, words, maxWidth):
        """
        :type words: List[str]
        :type maxWidth: int
        :rtype: List[str]
        """
        if not words: # 如果words是None或者[]
            return []
        res,row =[],[]
        l = 0
        i = 0
        while i<len(words):
            if len(words[i]) + l<=maxWidth: # 如果之前的长度加上当前的单词不超过max
                l +=(len(words[i])+1) # 要了的话后面补一个空格(+1)，为了和下一个单词分开
                row.append(words[i])
                i+=1
                continue
            else: # 当前加上第i个位置的单词words[i]超过了max,则把row里面的进行整理
                if len(row)==1: # 只有一个的话，直接右边全补空
                    res.append(str(row[0])+' '*(maxWidth-len(row[0])))
                else:# >=2个单词
                    total_space = maxWidth -(l- len(row)) #看总共应该插入多少个空格，因为l中本身每个单词后面是带的有空格的，所以要先-len(row)
                    word_space = total_space //(len(row)-1) #每个单词均分几个空格
                    more_space = total_space %(len(row)-1) # 均分后还多出来的空格（都放到左边，因为题目中说了左侧放的要多余右侧）
                    tmp = ''
                    for word in row[:-1]:
                        if more_space >0:
                            tmp +=str(word)+' '*(word_space + 1) # 1就是从more_space里面拿的，因为多的都放在左边，所以每个左边的单词都多拿一个
                            more_space -=1
                        else:
                            tmp += str(word) + ' ' * (word_space) # 相当于more_space用完了，相当+0

                    tmp +=str(row[-1]) # 每行最后一个单词最后没有空格
                    res.append(tmp)
                row = []
                l = 0


        # 最后一行---当i访问到最后一个单词的时候
        tmp = ' '.join(row) # 单词之间用' '隔开即可
        res.append(tmp + (maxWidth - len(tmp))*' ') # 后面补空格

        return res

```  

参考：  

https://blog.csdn.net/qq_17550379/article/details/85013613




