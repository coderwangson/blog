## markdown离线编辑器   

市面上有很多markdown编辑器有离线的，也有在线的，但是很多不支持数学公式的预览，后来我发现csdn的编辑器带的有公式预览的功能，所以我就想把这个编辑器提取出来，做一个(假)离线版本的，因为个人不是很喜欢登录，然后在写文章，所以便闲的无聊想抽取出来。  

观看了网页源代码后发现，csdn的markdown的编辑器主要是靠js进行解析，所以只需要那些js文件就行了，因为这里我是一个假离线版本，所以那些js文件我没有下载，仍然是从csdn官网获取，而这个编辑器主要不同就是能够直接免登录编辑，然后界面你可以diy一下，仅此而已。  

附上源码，你只需要把下面保存成html就能直接使用你自己的markdown编辑器了，并且实现对数学公式实时预览，可以通过导出，导出成md或者html。缺点就是不能再进行图片导入了，如果刚需，你需要一个图床，先把图片上传到图床上面，然后用url方式插入图片。  
  

	```
	
	
	
	<!DOCTYPE html>
	<html lang="en">
	<head>
	    <meta name="author" content="Benoit Schweblin">
	    <meta name="viewport" content="user-scalable=no, width=device-width, initial-scale=1, maximum-scale=1">
	    <meta name="mobile-web-app-capable" content="yes">
	    <meta name="apple-mobile-web-app-capable" content="yes">
	    <meta name="apple-mobile-web-app-status-bar-style" content="black">
	    <meta name="referrer" content="always">
	    <script type='text/javascript' src='https://csdnimg.cn/release/phoenix/write/tingyun-rum-559c80716d.js'></script>
	    <title></title>
	    <link rel="stylesheet" href="https://static-blog.csdn.net/mdeditor/public/res-min/themes/header.css?20180508162946">
	    <link rel="stylesheet" href="https://csdnimg.cn/release/phoenix/template/css/markdown_views-ea0013b516.css" />
	    <script type="text/javascript">
	            var artId = 0;
	            var get_article_address = "https://mp.csdn.net/mdeditor/getArticle";
	            var set_article_address = "https://mp.csdn.net/mdeditor/saveArticle";
	            var upload_image_address = "https://mp.csdn.net/UploadImage";
	            window.isSwitch = false;
	            window.baseDir = 'https://static-blog.csdn.net/mdeditor/public/res-min';
	            var require = {
	                baseUrl: window.baseDir
	            };
	            var codeStyle = '';
	            
	    </script>
	    <script src="https://static-blog.csdn.net/mdeditor/public/res/bower-libs/jquery/jquery.min.js?v6"></script>
	    <script type="text/javascript" src="https://csdnimg.cn/rabbit/exposure-click/main-1.0.2.js"></script>
	    <script src="https://static-blog.csdn.net/mdeditor/public/res/meditor.bundle.js?v6"></script>
	    <script src="https://static-blog.csdn.net/mdeditor/public/res-min/csdn_label.js?20180508162951"></script>
	    <script src="https://static-blog.csdn.net/mdeditor/public/res-min/require.js" data-main="https://static-blog.csdn.net/mdeditor/public/res-min/main_new.js?20180716155636"></script>
	    <script type="text/javascript" src="https://csdnimg.cn/release/phoenix/write/first_guide-06c52b327b.js"></script>
	</head>
	
	<body style="overflow: hidden">
	    <div class="loading"></div>
	    <header>
	        <div class="container">
	            <div class="navbar-collapse-md">
	                <ul class="navbar-nav-md">
	                    <li class="nav-item nav-avatar">
	                        <a href="https://blog.csdn.net/qq_28888837" target="_blank">
	                            <img class="nav-avatar-img" src="https://avatar.csdn.net/D/2/4/3_qq_28888837.jpg" width="56"
	                                height="56">
	                        </a>
	                    </li>
	                </ul>
	            </div>
	        </div>
	           </header>
	   
	
	    <main id="meditor_box">
	        <div class="title-box clearfix d-flex">
	            <input type="text" id="txtTitle" class="input-file-title" maxlength="100" placeholder="输入文章标题">
	            
	        </div>
	        <div id="editorBox" style="position:relative;width:100%;height: 100%">
	            <div id="csdnEditor" ></div>
	            <div class="fullOptBox">
	            
	            </div>
	        </div>
	        
	        </div>
	        <div class="mask"></div>
	        <div class="alert-container" id="switchHtmlMode">
	            <div class="pos-box">
	                <a class="btn-close">
	                    <i class="xheIcon icon-guanbi"></i>
	                </a>
	                <div class="title title-warning">提示</div>
	                <div class="content">如果选择切换编辑器，你将丢失在此编写的文章内容！建议保存到草稿箱后再进行切换。 </div>
	                <div class="opt-box text-right">
	                    <a class="btn-smail c-blue" href="https://mp.csdn.net/postedit" target="_self">确认切换</a>
	                    <button class="btn-smail c-red btn-cancel">放弃</button>
	                </div>
	            </div>
	        </div>
	        <div class="alert-container" id="createNewMode">
	            <div class="pos-box">
	                <a class="btn-close">
	                    <i class="xheIcon icon-guanbi"></i>
	                </a>
	                <div class="title title-warning">提示</div>
	                <div class="content">创建新的文章你将丢失当前编辑内容，可选择保存草稿存储当前编辑内容。</div>
	                <div class="opt-box text-right">
	                    <button class="btn-smail c-blue btn-caogao btn-blog-save">保存为草稿</button>
	                    <button class="btn-smail c-red btn-new">写新文章</button>
	                </div>
	            </div>
	        </div>
	    </main>
	</body>
	
	</html>
	```






