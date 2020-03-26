Sublime Text3 的 Markdown 实时预览全面总结  

文章转自：https://blog.csdn.net/qq_20011607/article/details/81370236  

## 0. 温习：插件安装方式，后面会反复用到    

1. 组合键Ctrl+Shift+P 调出命令面板
2. 输入Package Control: Install Package，回车
3. 在搜索框中输入要安装的包名（一个一个，不能同时安多个）
4. 静待几秒即可安装成功  

## 1. 插件介绍  

|插件|功能|
|--|--|
|MarkdownEditing|一个提高Sublime中Markdown编辑特性的插件|
|MarkdownPreview|Markdown转HTML，提供在浏览器中的预览功能|
|MarkdownLivePreview|提供在编辑框中实时预览的功能|
|LiveReload|一个提供md/html等文档的实时刷新预览的的插件|

## 2. MarkdownEditing  

顾名思义，Markdown编辑器，是Markdown写作者必备的插件，不仅可以高亮显示Markdown语法还支持很多编程语言的语法高亮显示。
特别注意：MarkdownEditing只针对 md\mdown\mmd\txt 格式文件才启用。  

### 特性  

MarkdownEditing 从视觉和便捷性上针对 Markdown文档的编辑进行了一系列的优化。如：

* 颜色方案仿 Byword及iA writer
* 自动匹配星号（*）、下划线（_）及反引号（`）
* 选中文本按下以上符号能自动在所选文本前后添加配对的符号
* 方便粗体、斜体和代码框的输入  

效果图：  

![效果图](https://img-blog.csdn.net/20180803020629933?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzIwMDExNjA3/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  

## 3. MarkdownLivePreview(不推荐)  

### 功能  

实时预览Markdown文件，左侧为md文件，右侧为预览结果。可配合MarkdownEditing一起使用。

## 使用  

MarkdownLivePreview默认关闭实时预览，既然安装这个插件了，那肯定是要用的。打开方式为在Preferences -> Package Settings -> MarkdownLivePreview -> Settings 的设置的右侧加一条 "markdown_live_preview_on_open": true,，重启sublime即可。

为什么不能直接将左侧对应的false改为true，这是因为左侧为默认配置，是不能改的（只读），右侧的编辑区才是用户自定义区。

## 效果图  

以下为配合MarkdownEditing的效果：  
![](https://img-blog.csdn.net/20180803030607658?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzIwMDExNjA3/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  

实际预览效果并不理想,很丑,而且不能横向滚动,也就是说如果一行显示不过来那你就看不到 了。偶然也会有些卡,所以其实推荐的是下面这个插件。  

## 4. MarkdownPreview   

### 功能  

1. 支持在浏览器中预览markdown文件
2. 将md文件导出为html代码
### 将md文件用浏览器预览——1.常规方法  

1. 组合键 Ctrl+Shift+P 调出命令面板
2. 输入mdp找到并选中Markdown Preview： Preview in Browser
3. 出现两个选项：github和markdown。任选其一即可，github是利用GitHub的在线API来解析.md文件，支持在线资源的预览，如在线图片它的解析速度取决于你的联网速度。该方式据说一天只能打开60次。markdown就是传统的本地打开，不支持在线资源的预览。
4. 默认浏览器中显示预览结果  
 
### 将md文件用浏览器预览——2.用快捷键打开   

Sublime Text支持自定义快捷键，Markdown Preview默认没有快捷键，我们可以自己为`Markdown Preview： Preview in Browser`设置快捷键。
方法是在`Preferences -> Key Bindings`打开的文件的右侧栏的中括号中添加一行代码：  
```
{ "keys": ["alt+m"], "command": "markdown_preview", "args": {"target": "browser", "parser":"markdown"}  }

```

这里：  

```
"alt+m" 可设置为自己喜欢的按键。
"parser": "markdown"也可设置为"parser":"github"，改为使用Github在线API解析markdown。
```

以上两种方式都有个问题：每次预览都要打开一个新的网页，而且需要手动操作。有没有一个方法，可以打开一个网页后，之后再也不用管，让它实时刷新预览呢？

有，还很简单，答案就是**MarkdownPreview+LiveReload！**
LiveReload是一个可实时刷新的插件，不仅可用于Markdown，也可用于HTML等。  

## 5. （最强）实时自动刷新预览：MarkdownPreview + LiveReload  

## 先安装并配置Markdown Preview  

如前Markdown Preview安装成功后，设置前文所述的快捷键（如需），打开其配置文件 Preferences -> Package Settings -> Markdown Preview -> Settings，检查左侧enable_autoreload条目是否为true，若是，跳过。若不是，右侧栏加一条下面这个后重启Sublime:
```
{
    "enable_autoreload": true
}
```

### 安装并配置LiveReload  

Ctrl+Shift+p, 输入 Install Package，输入LiveReload, 回车安装
安装成功后, 再次Ctrl+shift+p, 输入LiveReload: Enable/disable plug-ins, 回车, 选择 Simple Reload with delay (400ms)或者Simple Reload，两者的区别仅仅在于后者没有延迟。  

然后在`Preferences-package Settings-liveReload-settings user`，然后在改文件下输入：

```
{
    "enabled_plugins": [
    "SimpleReloadPlugin",
    "SimpleRefresh"
]

}  

```

这样默认就是打开了。 如果你的还是不能自动更新，可以尝试在chrome插件里面找一个叫做LiveReload的插件也可以。 




### 开始使用  

如前面提到的手动或者快捷键打开预览网页，之后便再也不用管它，只要你的sublime保存一次，网页那边就会自动刷新预览，美滋滋。

但是呢，有个遗留的问题：网页预览不能跟随你的sublime编辑位置，还需要你滑动页面。
在这一点上，CSDN-Markdown可以说很优秀了，本文就是在该编辑器下完成的。

作者：张渊猛  

## 更新加入MathJax  

参考自  https://blog.csdn.net/aiynmimi/article/details/86622567  

由于文件中需要编译大量的数学公式，就需要启用MathJax。但是折腾了好久啊，预览的时候就是没有效果！

然后在Stack Overflow上找到一个回答：
[how-to-enable-mathjax-rendering-in-sublime-text-markdown-preview](https://stackoverflow.com/questions/27972177/how-to-enable-mathjax-rendering-in-sublime-text-markdown-preview)

这里边被采纳的@VividD的答案已经比较早了，现在没有那个选项了！然后在下边有个回答说，要在设置中添加：  

```
{
    "enable_mathjax": true,
    "js": [
    "https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js",
            "res://MarkdownPreview/js/math_config.js",
    ],
}
```

之后，还要去编辑math_config.js。我这里没有去编辑math_config.js，（找不到这个文件！！！），然后在这个回答的评论中说到，[github.com/facelessuser/MarkdownPreview/issues/12](github.com/facelessuser/MarkdownPreview/issues/12)
这个issue，还需要添加：  

```  
"markdown_extensions": { 
    "pymdownx.arithmatex": { 
        "generic": true 
    } 
}
```

（下面这个问题我没遇见，是原博主的，我就拷贝过来以供参考）  

我在添加了这一设置之后发现添加的数学公式都能显示出来了！但是又发现了新的问题就是自动生成的目录`[TOC]`不显示了，而且代码格式全乱了！

然后猜测是不是这个设置的问题，就发现默认设置中有这一个`markdown_extensions`，而且是一大堆，所以我在想这里就设置了一个，就把默认的全部给覆盖掉了，所以我就把默认的全部复制过来：   

```
{
    "enable_mathjax": true,
    "js": [
    "https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js",
            "res://MarkdownPreview/js/math_config.js",
    ],
    "markdown_extensions": [
        // Python Markdown Extra with SuperFences.
        // You can't include "extra" and "superfences"
        // as "fenced_code" can not be included with "superfences",
        // so we include the pieces separately.
        "markdown.extensions.footnotes",
        "markdown.extensions.attr_list",
        "markdown.extensions.def_list",
        "markdown.extensions.tables",
        "markdown.extensions.abbr",
        "pymdownx.betterem",
        {
            "markdown.extensions.codehilite": {
                "guess_lang": false
            }
        },
        // Extra's Markdown parsing in raw HTML cannot be
        // included by itself, but "pymdownx" exposes it so we can.
        "pymdownx.extrarawhtml",

        // More default Python Markdown extensions
        {
            "markdown.extensions.toc":
            {
                "permalink": "\ue157"
            }
        },
        "markdown.extensions.meta",
        "markdown.extensions.sane_lists",
        "markdown.extensions.smarty",
        "markdown.extensions.wikilinks",
        "markdown.extensions.admonition",

        // PyMdown extensions that help give a GitHub-ish feel
        {
            "pymdownx.superfences": { // Nested fences and UML support
                "custom_fences": [
                    {
                        "name": "flow",
                        "class": "uml-flowchart",
                        "format": {"!!python/name": "pymdownx.superfences.fence_code_format"}
                    },
                    {
                        "name": "sequence",
                        "class": "uml-sequence-diagram",
                        "format": {"!!python/name": "pymdownx.superfences.fence_code_format"}
                    }
                ]
            }
        },
        {
            "pymdownx.magiclink": {   // Auto linkify URLs and email addresses
                "repo_url_shortener": true,
                "repo_url_shorthand": true
            }
        },
        "pymdownx.tasklist",     // Task lists
        {
            "pymdownx.tilde": {  // Provide ~~delete~~
                "subscript": false
            }
        },
        {
            "pymdownx.emoji": {  // Provide GitHub's emojis
                "emoji_index": {"!!python/name": "pymdownx.emoji.gemoji"},
                "emoji_generator": {"!!python/name": "pymdownx.emoji.to_png"},
                "alt": "short",
                "options": {
                    "attributes": {
                        "align": "absmiddle",
                        "height": "20px",
                        "width": "20px"
                    },
                    "image_path": "https://assets-cdn.github.com/images/icons/emoji/unicode/",
                    "non_standard_image_path": "https://assets-cdn.github.com/images/icons/emoji/"
                }
            }
        },
        {
            "pymdownx.arithmatex": { 
                "generic": true 
            }
        }
    ],
}

```

然后，发现美滋滋，数学公式能够显示，目录也能自动生成，代码格式也不乱了！

给看个例子，比如：  

```
**回归问题的典型性能衡量指标是均方根误差（RMSE）**

RMSE对应欧几里得范数，也称之为$\iota_{2}$范数，记作$\left \| . \right \|_{2}$，或者$\left \| . \right \|$

$$ RMSE(X,h) = \sqrt{\frac{1}{m}\sum_{i=1}^{m}(h(X^{(i)})-y^{(i)}})^{2} $$

但是如果有很多离群区域时，你可以考虑使用**平均绝对误差（平均绝对偏差）**

MAE对应$\iota_{1}$范数，记作$\left \| . \right \|_{1}$。有时它也被称为曼哈顿距离

$$ MAE(X,h) = \frac{1}{m}\sum_{i=1}^{m}\left | h(X^{(i)}) - y^{(i)} \right | $$
```
那么，上边对应的显示效果： 

![](https://img-blog.csdnimg.cn/20190124104917786.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2FpeW5taW1p,size_16,color_FFFFFF,t_70)












