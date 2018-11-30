##Java贪吃蛇

**一.    首先对贪吃蛇所需要的类进行分析，并将其类抽象出来。**

 - **1.需要有一块场地供游戏使用 class PlayGround extends JFrame **
 - 2.场地上需要有地图 class Map
 - 3.场地上内容需要实时更新，所以需要一个线程 class FlashThread implements  Runnable 来对场地刷新
 - 4.因为要通过键盘对蛇控制，所以需要一个键盘监听器 class KeyListen extends KeyAdapter
 - 5.场地中还需要有一条蛇  class Snake
 *以上便是目前所需要的基本的类*
 
 **二 . 然后 对所需要的类进行分析抽象，并且了解其依赖关系**
 
 - 1.对于场地类来说，场地为一个矩形窗体，然后进行划分划分为ROW行COL列的网格，并且对每个网格大小进行规定。
 - ![这里写图片描述](http://img.blog.csdn.net/20170511195825054?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMjg4ODg4Mzc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

这个便是场地的基本模型。

 - 2.然后场地中需要有一条蛇Snake snake，需要地图Map map，需要另一个线程实时刷新FlashThread flashThread，需要一个键盘监听器KeyListen keyListen 。
 - 3.对于蛇类来说，可以将一条蛇分为多个节点构成，所以再增加一个class Node。
 - 4.对于地图类，由于地图中主要是一些障碍物以及边界、食物等，所以可以将地图分解为由多个点构成，每个点就是场地网格中的一格，所以再增加一个class Block类。
 
 ![这里写图片描述](http://img.blog.csdn.net/20170511201118984?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMjg4ODg4Mzc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
 

  以上便是初步抽象后的内容。
  
---------
**三. 重点代码编制，以及每个类的各个字段分析**

 - 1.**场地类**
    场地类是最基础的类，场地类主要作为各个游戏的显示空间，所有内容都将在场地中显示，所以场地类继承与JFrame，因为场地类可以作为多行多列的表格，所以有字段
    public static final int ROW = 30;//行数
	public static final int COL = 30;//列数
	public static final int BLOCK_SIZE = 15;//每个网格大小
	场地类中最重要的方法便是从JFram中继承的paint方法，因为它起到绘制场地的作用，并且每次对场地刷新也是通过paint方法，所以对paint方法的重写至关重要，paint中主要有地图的绘制，蛇的绘制，因为paint方法在每次的刷新中会用到，而每次的刷新就代表着蛇移动了新的位置，所以paint中还要对蛇新的位置判断，看蛇是否已经死了，或者看蛇是否吃到了果子。
	@Override
	public void paint(Graphics g) {
		snake.eat(map);//蛇是否吃到了果子
		snake.isAlive(map);//重画后蛇是否活着
		map.draw(g);//重画地图
		snake.draw(g);//重画蛇
	}
	

 - 2.场地类除了重画外还有一些初始化的操作，可以放在构造函数中（有些当然也可以放在paint中）
 // 构造函数，对场地类做一些初始化
	public PlayGround() {
		snake = new Snake();// 初始化一条蛇
		this.setLocation(200, 200);// 设置窗口位置
		this.setResizable(false);// 窗口大小不可变
		this.setSize(COL * BLOCK_SIZE, ROW * BLOCK_SIZE);// 窗口的大小由网格计算得到
		this.setDefaultCloseOperation(DISPOSE_ON_CLOSE);// 将关闭按钮开启
	}

 - 3.然后再写一个startGame方法作为整个游戏开始的入口
 

 - 4.写一个测试类，先测试一下当前的情况
 
 

```
public class Test {
	public static void main(String[] args) {
		new PlayGround().startGame();
	}

}

```
测试结果如下：
![这里写图片描述](http://img.blog.csdn.net/20170511204328780?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMjg4ODg4Mzc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

 - 5.当前只显示了一个白色的窗体，里面却没有任何内容，那是因为你的地图和蛇的draw方法没有进行书写，所以下面的任务就是书写蛇与地图了。
 - 6.**蛇类**由开始的分析可知蛇类是由许多Node构成的所以首先写一个Node类：Node类代表着蛇的身体节点，所以每个节点应该有位置，因为蛇在按照一定的方向移动，而蛇的方向是由Node构成的，所以Node还具有方向（方向用枚举标识 
 `//代表蛇每个节点的方向
public enum Dir {
	UP, DOWN, LEFT, RIGHT
}`），并且每个节点都会有一个前节点和后节点，这样才能连成一条蛇。所以Node节点的基本结构为（字段最好私有化）

```
//蛇的身体节点
public class Node {
	Dir dir = Dir.LEFT;// 每个节点的方向
	int x, y;// 蛇的位置，x、y具体含义可以看最上面网格图
	Node pre = null;// 每个节点的前一个节点
	Node next = null;// 每个节点的后一个节点
}

```
同时为了画蛇，其实就是将其所有节点画出，所以同时给Node一个方法draw

```
   //将节点画出
	public void draw(Graphics g) {
		Color c = g.getColor();//与最后一个g.setColor(c);一起使用保护颜色现场
		g.setColor(Color.BLACK);
		//每个节点用矩形标识
		g.fillRect(PlayGround.BLOCK_SIZE * x, PlayGround.BLOCK_SIZE * y,
				PlayGround.BLOCK_SIZE, PlayGround.BLOCK_SIZE);
		g.setColor(c);
	}
```

```
public class Snake {
	// 蛇头给出默认位置与默认放行
	Node head = new Node(20, 15, Dir.LEFT);
	// 初始蛇尾与蛇头是一个
	Node tail = head;
	// 蛇的生命状态
	boolean alive = true;
	// 将蛇显示出来
	public void draw(Graphics g) {
		// TODO Auto-generated method stub
	}

}
```

其实蛇的draw方法主要就是将所有节点显示出来。所以为

```
	public void draw(Graphics g) {
		// TODO Auto-generated method stub
		//显示所有节点
		for (Node n = head; n != null; n = n.next) {
			n.draw(g);
		}

	}
```
此时在运行测试代码则看到一个长为一的小蛇出来了
![这里写图片描述](http://img.blog.csdn.net/20170511210521716?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMjg4ODg4Mzc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

 

 - 7.我想你也发现了虽然有了蛇，但是没有地图呀，并且蛇也不会动，所以我们便继续前行，先把地图做出来。从开始的分析我们知道其实地图只是由一些像素块组成的罢了，所以类似于写蛇先写节点，所以写地图先写像素块class Block ，类似Node节点，Block也需要x，y，但是由于各个像素块之间没硬联系，所以便不需要pre与next了，但同时每个像素块可代表的含义又各不相同，所以需要用一个标记来说明这个像素快代表的内容（比如食物，障碍，边界。。。用枚举表示
 - `//每个Block代表的含义
public enum Mark {
	Egg, Edg, Block
}
`），同时为了画地图，其实就是把所有像素块画出来，所以给Block一个draw方法，但是画像素块时要注意，不同标记（Mark）则可以画不同样子来区分。
所以Block的基本框架为

```
public class Block {
	private int x, y;
	private Mark mark = Mark.Block;// 标识普通砖块0 边界1 蛋2

	// 将Block画出
	public void draw(Graphics g) {
	}
}
```
draw方法为

```
// 将Block画出
	public void draw(Graphics g) {
		Color c = g.getColor();
		g.setColor(Color.YELLOW);
		// 蛋
		if (mark == Mark.Egg) {
			g.fillOval(x * PlayGround.BLOCK_SIZE, y * PlayGround.BLOCK_SIZE,
					PlayGround.BLOCK_SIZE, PlayGround.BLOCK_SIZE);

		}
		// 砖
		else if (mark == Mark.Block) {
			g.fillRect(x * PlayGround.BLOCK_SIZE, y * PlayGround.BLOCK_SIZE,
					PlayGround.BLOCK_SIZE, PlayGround.BLOCK_SIZE);
		}
		// 边界
		else {
			g.setColor(Color.BLUE);
			g.fillRect(x * PlayGround.BLOCK_SIZE, y * PlayGround.BLOCK_SIZE,
					PlayGround.BLOCK_SIZE, PlayGround.BLOCK_SIZE);

		}
		g.setColor(c);
	}
```

然后便可以构建地图了，其实地图就是一系列像素点的集合。
所以地图的框架为

```
public class Map {
	// 一系列像素点的集合
	public List<Block> block = null;
	public void draw(Graphics g) {
		// TODO Auto-generated method stub

	}

}

```

   而所有的像素点可以在Map的构造函数中初始化，而地图的draw其实为所有Block的draw所以代码如下
   

```
public class Map {
	// 一系列像素点的集合
	public List<Block> block = null;

	// 初始化时添加所有Block
	public Map() {
		block = new ArrayList<Block>();
		addRow(16, 1, 13);
		addRow(16, 17, 14);
		addCol(1, 15, 13);
		addCol(22, 15, 9);
		addCol(17, 7, 5);
		addCol(23, 7, 6);
		// 添加蛋
		block.add(new Block(8, 4, Mark.Egg));
		block.add(new Block(14, 13, Mark.Egg));
		block.add(new Block(5, 24, Mark.Egg));
		block.add(new Block(22, 7, Mark.Egg));
	}

	// 将地图画出
	public void draw(Graphics g) {
		Color c = g.getColor();
		g.setColor(Color.GRAY);
		// 画背景
		g.fillRect(0, 0, PlayGround.COL * PlayGround.BLOCK_SIZE, PlayGround.ROW
				* PlayGround.BLOCK_SIZE);
		// 画障碍
		for (Block b : block) {
			b.draw(g);
		}
		g.setColor(c);
	}

	// 添加行障碍 第y行 第 x列 长为 l
	public void addRow(int y, int x, int l) {
		for (int k = 0; k < l; k++) {
			// 加砖块时 砖块的x在变因为是加的行，所以行不变列递增
			block.add(new Block(x + k, y));
		}

	}

	// 添加列障碍 第y行 第 x列 高为 l
	public void addCol(int y, int x, int l) {
		for (int k = 0; k < l; k++) {
			// 加砖块时 砖块的y在变因为是加的列，所以列不变行递增
			block.add(new Block(x, y + k));
		}
	}
}

```

   如果addrow（）如addcol方法看着有吃里，可以参考这个图或者自己一个一个的加 
   ![这里写图片描述](http://img.blog.csdn.net/20170511213050632?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMjg4ODg4Mzc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

此时运行的结果应该为
![这里写图片描述](http://img.blog.csdn.net/20170511213200898?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMjg4ODg4Mzc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
这似乎像那么回事了，现在就剩下让它动起来了。
今天有点累，明天继续。
