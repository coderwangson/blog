**题目描述**
反幻方

我国古籍很早就记载着

2 9 4
7 5 3
6 1 8

这是一个三阶幻方。每行每列以及对角线上的数字相加都相等。

下面考虑一个相反的问题。
可不可以用 1~9 的数字填入九宫格。
使得：每行每列每个对角线上的数字和都互不相等呢？


这应该能做到。
比如：
9 1 2
8 4 3
7 5 6

你的任务是搜索所有的三阶反幻方。并统计出一共有多少种。
旋转或镜像算同一种。

比如：
9 1 2
8 4 3
7 5 6

7 8 9 
5 4 1
6 3 2

2 1 9
3 4 8
6 5 7

等都算作同一种情况。

请提交三阶反幻方一共多少种。这是一个整数，不要填写任何多余内容。

###简单分析

 - 本题的思路并不是很复杂，主要就是简单的暴力求解，本次暴力使用了一下dfs（深度优先）的方法。
 - 然后就是暴力后的判断，要注意本题要求的是各行各列不等。
 - 判断不等的方法有很多，可以使用一个很长的if语句，也可以像我这样用一个for循环。
 - 最后结果 /8的原因是因为镜像旋转共8种，你可以自己尝试一下旋转，镜像一下
 - 还有一点如果初学者看不明白数组和那个幻方的关系的话可以看一下这个图
 - ![这里写图片描述](http://img.blog.csdn.net/20170516204526922?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMjg4ODg4Mzc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
 - 

### 奉上代码：
 

```
public class eg2 {
	public static int[] data = { 1, 2, 3, 4, 5, 6, 7, 8, 9 };
	public static int[] res = new int[data.length];
	public static boolean[] vis = new boolean[data.length];
	static int sum = 0;

	// 深度遍历暴力几乎就是模板，不同的暴力题只需要更换isok即可
	public static void dfs(int n) {
		if (n == res.length) {

			if (isOk()) {
				sum++;
			}

		} else {
			for (int i = 0; i < data.length; i++) {
				if (vis[i])
					continue;
				res[n] = data[i];
				vis[i] = true;
				dfs(n + 1);
				vis[i] = false;
			}

		}

	}

	private static boolean isOk() {
		// 判断该各行各列不相等用数组然后用for可以快一点
		int[] n = new int[9];
		n[1] = res[0] + res[1] + res[2];
		n[2] = res[3] + res[4] + res[5];
		n[3] = res[6] + res[7] + res[8];
		n[4] = res[0] + res[3] + res[6];
		n[5] = res[4] + res[1] + res[7];
		n[6] = res[2] + res[5] + res[8];
		n[7] = res[2] + res[4] + res[6];
		n[8] = res[0] + res[4] + res[8];

		for (int i = 1; i < n.length - 1; i++) {
			for (int j = i + 1; j < n.length; j++) {
				if (n[i] == n[j]) {
					return false;
				}
			}

		}
		return true;
	}

	public static void main(String[] args) {
		dfs(0);
		// 记得/8
		System.out.print(sum / 8);

	}

}

```

