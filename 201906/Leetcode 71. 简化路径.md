# Leetcode 71. 简化路径

标签： `Leetcode`

---
题目地址： https://leetcode-cn.com/problems/simplify-path/  

## 题目描述   

<div class="content__2ebE"><div html="<p>以 Unix 风格给出一个文件的<strong>绝对路径</strong>，你需要简化它。或者换句话说，将其转换为规范路径。</p>

<p>在 Unix 风格的文件系统中，一个点（<code>.</code>）表示当前目录本身；此外，两个点 （<code>..</code>）&amp;nbsp;表示将目录切换到上一级（指向父目录）；两者都可以是复杂相对路径的组成部分。更多信息请参阅：<a href=&quot;https://blog.csdn.net/u011327334/article/details/50355600&quot; target=&quot;_blank&quot;>Linux / Unix中的绝对路径 vs 相对路径</a></p>

<p>请注意，返回的规范路径必须始终以斜杠 <code>/</code> 开头，并且两个目录名之间必须只有一个斜杠 <code>/</code>。最后一个目录名（如果存在）<strong>不能</strong>以 <code>/</code> 结尾。此外，规范路径必须是表示绝对路径的<strong>最短</strong>字符串。</p>

<p>&amp;nbsp;</p>

<p><strong>示例 1：</strong></p>

<pre><strong>输入：&amp;quot;</strong>/home/&amp;quot;
<strong>输出：&amp;quot;</strong>/home&amp;quot;
<strong>解释：</strong>注意，最后一个目录名后面没有斜杠。
</pre>

<p><strong>示例 2：</strong></p>

<pre><strong>输入：&amp;quot;</strong>/../&amp;quot;
<strong>输出：&amp;quot;</strong>/&amp;quot;
<strong>解释：</strong>从根目录向上一级是不可行的，因为根是你可以到达的最高级。
</pre>

<p><strong>示例 3：</strong></p>

<pre><strong>输入：&amp;quot;</strong>/home//foo/&amp;quot;
<strong>输出：&amp;quot;</strong>/home/foo&amp;quot;
<strong>解释：</strong>在规范路径中，多个连续斜杠需要用一个斜杠替换。
</pre>

<p><strong>示例 4：</strong></p>

<pre><strong>输入：&amp;quot;</strong>/a/./b/../../c/&amp;quot;
<strong>输出：&amp;quot;</strong>/c&amp;quot;
</pre>

<p><strong>示例 5：</strong></p>

<pre><strong>输入：&amp;quot;</strong>/a/../../b/../c//.//&amp;quot;
<strong>输出：&amp;quot;</strong>/c&amp;quot;
</pre>

<p><strong>示例 6：</strong></p>

<pre><strong>输入：&amp;quot;</strong>/a//b////c/d//././/..&amp;quot;
<strong>输出：&amp;quot;</strong>/a/b/c&amp;quot;</pre>
" class="notranslate"><p>以 Unix 风格给出一个文件的<strong>绝对路径</strong>，你需要简化它。或者换句话说，将其转换为规范路径。</p>

<p>在 Unix 风格的文件系统中，一个点（<code>.</code>）表示当前目录本身；此外，两个点 （<code>..</code>）&nbsp;表示将目录切换到上一级（指向父目录）；两者都可以是复杂相对路径的组成部分。更多信息请参阅：<a href="https://blog.csdn.net/u011327334/article/details/50355600">Linux / Unix中的绝对路径 vs 相对路径</a></p>

<p>请注意，返回的规范路径必须始终以斜杠 <code>/</code> 开头，并且两个目录名之间必须只有一个斜杠 <code>/</code>。最后一个目录名（如果存在）<strong>不能</strong>以 <code>/</code> 结尾。此外，规范路径必须是表示绝对路径的<strong>最短</strong>字符串。</p>

<p>&nbsp;</p>

<p><strong>示例 1：</strong></p>

<pre><strong>输入："</strong>/home/"
<strong>输出："</strong>/home"
<strong>解释：</strong>注意，最后一个目录名后面没有斜杠。
</pre>

<p><strong>示例 2：</strong></p>

<pre><strong>输入："</strong>/../"
<strong>输出："</strong>/"
<strong>解释：</strong>从根目录向上一级是不可行的，因为根是你可以到达的最高级。
</pre>

<p><strong>示例 3：</strong></p>

<pre><strong>输入："</strong>/home//foo/"
<strong>输出："</strong>/home/foo"
<strong>解释：</strong>在规范路径中，多个连续斜杠需要用一个斜杠替换。
</pre>

<p><strong>示例 4：</strong></p>

<pre><strong>输入："</strong>/a/./b/../../c/"
<strong>输出："</strong>/c"
</pre>

<p><strong>示例 5：</strong></p>

<pre><strong>输入："</strong>/a/../../b/../c//.//"
<strong>输出："</strong>/c"
</pre>

<p><strong>示例 6：</strong></p>

<pre><strong>输入："</strong>/a//b////c/d//././/.."
<strong>输出："</strong>/a/b/c"</pre>
</div></div>  

## 算法思想  

通过题目可以看出如果遇见`.`则不管，遇见'..'则取上一个元素，这种结构很像栈，所以这里用一个栈来模拟，我们先把路径用`/`分开，之后我们遍历这个路径数组，如果遇见的是`.`，那么什么也不做，如果遇见的是'..'我们就弹出一个元素，相当于向上一级，最后我们把栈中所有的元素拼接在一起即可。  

## python代码  

```python
class Solution(object):
    def simplifyPath(self, path):
        """
        :type path: str
        :rtype: str
        """
        path = path.split('/')
        p = [] # 路径栈
        i = 0
        for i in path:
            if i =='.' or i=='': # 同级不管 split后会有空，因为//
                continue
            elif i=='..': # 上一级则弹出
                if len(p) > 0:p.pop()
            else: # 其他则入栈
                p.append(i)
        # 结果开头为 / 如果p里面有东西则用/连起来，没有则为无
        return "/" + "/".join(p)
```




