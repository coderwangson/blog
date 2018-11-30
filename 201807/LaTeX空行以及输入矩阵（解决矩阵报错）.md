## Lax空行以及输入矩阵  

### 空行  

空行可以使用  

	\\[2ex]  
	
其中 2ex 可以随便设为自己想要的距离。  

### 输入矩阵  

输入矩阵开始总是报错，后来我发现是因为没有什么宏包之类的，所以只需要引入宏包就行了。  

	\documentclass{article}
	\usepackage{amsmath,xcolor}
	\usepackage{array}
	\makeatletter
	\renewcommand*\env@matrix[1][*\c@MaxMatrixCols c]{%
	 \hskip -\arraycolsep
	 \let\@ifnextchar\new@ifnextchar
	 \array{#1}}
	\makeatother 
	\begin{document}
	\[
	\begin{pmatrix}[cc|c]
	 1 & 2 & 3\\
	 4 & 5 & 9
	\end{pmatrix}
	\]
	\[
	\begin{bmatrix}[*2cr@{\quad}|@{\quad}>{\bf\color{red}}r]
	 a & b & 1 & 4 \\
	 c & d & -2 & -3
	\end{bmatrix}
	\]
	\end{document}  
其中  

	\usepackage{amsmath,xcolor}
	\usepackage{array}
	\makeatletter
	\renewcommand*\env@matrix[1][*\c@MaxMatrixCols c]{%
	 \hskip -\arraycolsep
	 \let\@ifnextchar\new@ifnextchar
	 \array{#1}}
	\makeatother  
这一段就是对矩阵宏包的引入，只要引入后就可以使用\begin{bmatrix}进行矩阵输入了。  

### 参考  

> http://www.latexstudio.net/archives/6666.html
> http://bbs.ctex.org/forum.php?mod=viewthread&tid=48308
