### 在说正题之前需要先引进一下java的简单的**内存模型**
- 在java中可以简单的看作有堆、栈、还有方法区
- 堆中存储的就是对象实体，在栈中存储的就是指向对象的引用，而在方法区主要就是一些方法或者静态变量（可能不是特别准确，但可以简单的参考）
 比如以下一段代码：
 

```
 class A{
 static int b;
 int a;
 }
 public class Main{
 public static void main(String[]args){
 A a1 = new A();
 a1.b=1;
 a1.a=2;
 A a2 = new A();
 a2.b=3;
 a2.a = 4;
 A.b = 0;
 }
 }
```

上面的代码在内存中的分配模型大概为

![这里写图片描述](http://img.blog.csdn.net/20170610102825918?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMjg4ODg4Mzc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
所以你最后看到的b为最后一个更新的结果。

----
在基本介绍完其存储模型后，就可以解决list的数据不更新的问题，这个问题是我昨天帮同学解决问题中遇到的，问题描述如下。
问题描述：
有一个list集合存储数据模型，然后list里面存储的是User类，User是从Dao（数据库层）取出的数据，但是在取完所有的用户时，发现List列表里的东西都是最后一个用户，最后通过调试才找到问题所在，就是User中的所有字段，她都把它们设置为了static（/笑哭），虽然现在看来很好解决，但是当涉及到的层次很多时，这就可能成为一个致命的问题。
下面我通过简单分析一下为什么我的List不更新。

```
//伪代码 不能运行
class User{
public static string name;//开发不能用public，破坏了封装
}

mian(){
List<User> list = new List<>();
//假设加3个
while(i<3){
User u = new User();
u.name = Dao.SelectAUser().name;//从Dao层随便取一个用户并拿到名字（并且可以保证控制每次拿到的用户是不一样的）
list.add(u);//添加用户到列表
}

}
```
上面的代码只看mian中是没什么问题的，但是当运行后发现list中数据竟然都是同一个内容。
其真正的原因如下：

![这里写图片描述](http://img.blog.csdn.net/20170610105012413?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMjg4ODg4Mzc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)