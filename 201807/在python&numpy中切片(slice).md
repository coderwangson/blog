转载自：[博客](https://www.cnblogs.com/Sinte-Beuve/p/6573246.html)
<div class="entry">
        <div id="cnblogs_post_body" class="blogpost-body cnblogs-markdown"><h1 id="在pythonnumpy中切片slice"> 在python&amp;numpy中切片(slice)</h1>
<p>上文说到了，<a href="http://www.cnblogs.com/Sinte-Beuve/p/6571717.html">词频的统计</a>在数据挖掘中使用的频率很高，而切片的操作同样是如此。在从文本文件或数据库中读取数据后，需要对数据进行预处理的操作。此时就需要对数据进行变换，切片，来生成自己需要的数据形式。</p>
<p>对于<strong>一维数组</strong>来说，python原生的list和numpy的array的切片操作都是相同的。无非是记住一个规则<strong><code>arr_name[start: end: step]</code></strong>,就可以了。</p>
<p>实例：<br />
<img src="https://images2015.cnblogs.com/blog/697102/201703/697102-20170318125958963-1322221608.png" /></p>
<p>下面是几个特殊的例子：</p>
<ul>
<li><code>[:]</code>表示复制源列表</li>
<li>负的index表示，从后往前。-1表示最后一个元素。</li>
</ul>
<p><img src="https://images2015.cnblogs.com/blog/697102/201703/697102-20170318130004370-1434598739.png" /></p>
<p>相对于一维数组而言，二维（多维）数组用的会更多。一般语法是<strong><code>arr_name[行操作, 列操作]</code></strong><br />
先随机产生一个3*4的数组。</p>
<div class="sourceCode"><pre class="sourceCode python"><code class="sourceCode python"><span class="op">in</span>:arr <span class="op">=</span> np.arange(<span class="dv">12</span>).reshape((<span class="dv">3</span>, <span class="dv">4</span>)) 

out:
array([[ <span class="dv">0</span>,  <span class="dv">1</span>,  <span class="dv">2</span>,  <span class="dv">3</span>],
       [ <span class="dv">4</span>,  <span class="dv">5</span>,  <span class="dv">6</span>,  <span class="dv">7</span>],
       [ <span class="dv">8</span>,  <span class="dv">9</span>, <span class="dv">10</span>, <span class="dv">11</span>]])</code></pre></div>
<ul>
<li>取行数据</li>
</ul>
<div class="sourceCode"><pre class="sourceCode python"><code class="sourceCode python">arr[i, :] <span class="co">#取第i行数据</span>
arr[i:j, :] <span class="co">#取第i行到第j行的数据</span></code></pre></div>
<ul>
<li>取列数据（注意数据格式）</li>
</ul>
<div class="sourceCode"><pre class="sourceCode python"><code class="sourceCode python"><span class="op">in</span>:arr[:,<span class="dv">0</span>] <span class="co"># 取第0列的数据，以行的形式返回的</span>
out:
array([<span class="dv">0</span>, <span class="dv">4</span>, <span class="dv">8</span>])

<span class="op">in</span>:arr[:,:<span class="dv">1</span>] <span class="co"># 取第0列的数据，以列的形式返回的</span>
out:
array([[<span class="dv">0</span>],
       [<span class="dv">4</span>],
       [<span class="dv">8</span>]])</code></pre></div>
<ul>
<li>取一个数据块</li>
</ul>
<div class="sourceCode"><pre class="sourceCode python"><code class="sourceCode python"><span class="co"># 取第一维的索引1到索引2之间的元素，也就是第二行 </span>
<span class="co"># 取第二维的索引1到索引3之间的元素，也就是第二列和第三列</span>
<span class="op">in</span>:arr[<span class="dv">1</span>:<span class="dv">2</span>, <span class="dv">1</span>:<span class="dv">3</span>] 

out: 
array([[<span class="dv">5</span>, <span class="dv">6</span>]])


 <span class="co"># 取第一维的全部 </span>
 <span class="co"># 按步长为2取第二维的索引0到末尾之间的元素，也就是第一列和第三列</span>
<span class="op">in</span>: arr[:, ::<span class="dv">2</span>]

out: 
array([[ <span class="dv">0</span>,  <span class="dv">2</span>],
       [ <span class="dv">4</span>,  <span class="dv">6</span>],
       [ <span class="dv">8</span>, <span class="dv">10</span>]])</code></pre></div>
<h2 id="参考文献">参考文献</h2>
<ul>
<li><a href="http://blog.csdn.net/liangzuojiayi/article/details/51534164" title="Python之numpy教程（二）：运算、索引、切片">Python之numpy教程（二）：运算、索引、切片</a></li>
<li><a href="http://www.codesec.net/view/457114.html" title="Numpy 笔记(二): 多维数组的切片(slicing)和索引">Numpy 笔记(二): 多维数组的切片(slicing)和索引</a></li>
</ul>
</div><div id="MySignature"></div>
        <div class="clear"></div>
        <div id="blog_post_info_block">
        <div id="blog_post_info">
        </div>
        <div class="clear"></div>
        <div id="post_next_prev"></div>
    </div>
</div>
</div>
