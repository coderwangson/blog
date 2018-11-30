### 程序实现目的：
实现通过先序遍历来输入一个二叉树，在通过dfs的方法遍历二叉树，树的最后节点通过-1作表示结束。
比如这样的个二叉树：
![这里写图片描述](http://img.blog.csdn.net/20161120140307942)

其输入与输出如下：
![这里写图片描述](http://img.blog.csdn.net/20161120140348426)


#### 程序如下：
```
import java.util.Scanner;
//树的节点
class Node {
	public int value = 0;
	public Node leftNode = null;
	public Node rightNode = null;
}

public class DFS {
//通过dfs来遍历二叉树从根节点开始
	public static void Dfstree(Node root) {
		if (root.value == -1) {
			return;  //当遍历到最后一个为-1的节点是退出，也是递归的边界
		} else {
			Node node = root.leftNode;
			System.out.println(root.value + " ' s left node is :" + node.value);
			Dfstree(node);//首先遍历左子树
			node = root.rightNode;
			System.out
					.println(root.value + " ' s right node is :" + node.value);
			Dfstree(node);//然后遍历右子树

		}

	}
//通过dfs的方法输入二叉树 用-1节点表示树的叶子节点
	public static void InitTree(Node root) {
		if (root.value == -1) {
			return; //当你上次输入的为-1节点的子树时 将会引起退出，也就是递归的边界部分
		}

		else {
			Node node = new Node();
			System.out.print("input " + root.value + " 's  left Node :");
			node.value = new Scanner(System.in).nextInt();
			root.leftNode = node;
			InitTree(node);//遍历输入左子树
			System.out.print("input " + root.value + " 's  right Node :");
			node = new Node();
			node.value = new Scanner(System.in).nextInt();
			root.rightNode = node;
			InitTree(node);//遍历输入右子树
		}

	}

	public static void main(String[] args) {
		Node rootNode = new Node();
		rootNode.value = 1;
		InitTree(rootNode);
		Dfstree(rootNode);
	}
}

```
