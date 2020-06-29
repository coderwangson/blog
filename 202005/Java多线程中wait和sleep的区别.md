# Java多线程中wait和sleep的区别

如果你了解过Java的多线程，一定知道如果让线程暂停可以通过wait或者sleep来完成，但是二者的区别又是什么呢。其实他们的主要区别是wait在暂停的时候会释放锁，而sleep不会释放锁，下面以代码给大家演示一下二者的不同之处。  

```
public class Wait_and_Sleep{
    public static void main(String[] args) {
        Object lock = new Object();
        new Thread(new Runnable(){
            @Override
            public void run(){
                synchronized(lock){
                    System.out.println("thread1 get lock");
                    try{
                        Thread.sleep(2000);
                    }catch(Exception e){}
                }
            }
        }).start();

        new Thread(new Runnable(){
            @Override
            public void run(){
                synchronized(lock){
                    System.out.println("thread2 get lock");                    
                }
            }
        }).start();
    }
}
```

上面的代码是利用sleep进行暂停的，终端的输出如下，在thread1得到锁后2秒才输出thread2，说明线程1在sleep的时候并没有释放锁。
```
thread1 get lock
thread2 get lock
```

我们将代码稍微改动一下，将sleep修改为wait：
```
public class Wait_and_Sleep{
    public static void main(String[] args) {
        Object lock = new Object();
        new Thread(new Runnable(){
            @Override
            public void run(){
                synchronized(lock){
                    System.out.println("thread1 get lock");
                    try{
                        lock.wait();
                    }catch(Exception e){}
                }
            }
        }).start();

        new Thread(new Runnable(){
            @Override
            public void run(){
                synchronized(lock){
                    System.out.println("thread2 get lock");
                    // lock.notifyAll();
                }
            }
        }).start();
    }
}
```
上面的代码是利用wait进行暂停的，终端的输出如下，和上面不同的是，当thread1进入wait以后，thread2立刻就执行了，说明thread1将锁释放掉了。
```
thread1 get lock
thread2 get lock
```  

另外需要注意的是，如果线程2中lock.notifyAll()是注释的话，那么线程1会陷入阻塞，因为没有被唤醒，只有加入了notifyAll()，才会被唤醒，然后重新拿锁。