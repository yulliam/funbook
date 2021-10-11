# synchronized 参数 及其含义

转自：https://blog.csdn.net/a1064072510/article/details/84065646

这个想必大家都不陌生，java里面的重量级锁。用来控制线程安全的。在long And long ，我刚开始接触java的时候 ，我就对这个关键词好奇颇深。尤其是 它的参数，有this的 也有静态变量的。网上对这个参数解释又太过术语话。

## 例如：

```
作用于方法时，锁住的是对象的实例(this)；

当作用于静态方法时，锁住的是Class实例，又因为Class的相关数据存储在永久带PermGen（jdk1.8则是metaspace），永久带是全局共享的，因此静态方法锁相当于类的一个全局锁，会锁所有调用该方法的线程；

当作用于一个静态类的时候，不管是不是本身内部的静态类，还是别人的静态类，都可以完成锁住的效果（ps 上锁的时候，相当于一群人 拿把锁找个东西上锁

synchronized作用于一个对象实例时，锁住的是所有以该对象为锁的代码块。
```



这不管是对于初学者，还是老鸟 。都感觉沉长无味。

最近，我的项目涉及到这一块，我对这个方法有了顿悟，写下来 传之后世
我对这个参数的理解是这样的。

```java
synchronized(获取锁的地方){
工作内容
}
```


是的 ，你们没看错 （）里面的就是一个“获取锁的地方”。{ }里面是工作内容。 他们有这几条关系。

* 获取锁的地方 和 工作内容 是不必需要 有任何关联的。 可以有关联，但没有关联 也没事

* 获取锁的地方 ：只是一个让synchronized 整体 放置锁的地方，一个“获取锁的地方” 只有一把锁。

* 工作内容： 要进行工作的**前提是 synchronized整体，找到一个“获取锁的地方”，在 “获取锁的地方” 获取到了锁， 如果 “获取锁的地方”的锁，已经被别人拿去了，那么就只能等待别人 把锁还给 “获取锁的地方”。然后 再获取锁。

* synchronized 会使用运行工作内容的前提，必须获取到锁。这个意思呢，就是例如，当前线程synchronized 获取到了objec A的锁，那么其他线程还可以更改 A 进行操作么?。 这个得到分两种情况，如果其他线程代码 是被synchronized 所包裹的，那么只能等 objec A被释放了，才能更改。如果，其他线程代码没有被synchronized包裹，她可以不需要有锁，就可以对objec A进行操作。

  

## 解释
  先打个比方，我们把 synchronized这个看成一个整体，那么在多线程的时候，就会有很多个 synchronized整体。 这个整体开始工作的前提是 它可以从 “获取锁的地方”拿到锁。那么他就可以做工作了。如果 想要 “获取锁的地方”的锁，已经被别人拿走了。那么只能等别人把锁换回来，才行。。

上面这个还是有点抽象。就这么说吧 在程序运行多线程的，会产生好多synchronized整体。就叫小明，小红，小芳，小花…等。他们现在手中没有锁，只有手中有锁的时候 才可以工作。 现在小明，小红他们来到工作的地方。获取锁的地方就是路边的一棵树：：注意现在路边只有这一棵树。小红抢先一步将树上的锁拿走了。然后小红工作去了（她的工作 不用和树有关系，她可以砍树，也可以去路边扫地 ）。小明，小芳等其他人，因为没地方获取锁（只有一棵树 并且树的锁 已经被小红拿走了）。所以只能在原地等待 ，小红工作做完了 把锁还给树 ，小明，小芳才有机会进行工作。、

注意： 上面是路边只有一棵树，如果存在好几颗树的话，那么小明，小红他们就可以同时放置获取锁 一个数对应一个锁，多个树就多个锁。这个对应于类的实例 （实例可以有好多个）



## 验证
我们首先定义一个Runnable ，因为 多线程的话 最好还是用Runnable,至于为什么呢？百度去。
```java
  static class LockRunable implements Runnable {
        private int num;
        private LockNormal lockNormal;
        public LockRunable(int num, LockNormal lockNormal) {
            this.num = num;
            this.lockNormal = lockNormal;
        }
        @Override
        public void run() {
            while (true) {

                    if (num <= 0) {
                        break;
                    }
                    try {
                        Thread.sleep(5);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    System.out.println(Thread.currentThread().getName() + "    " + num--);
    
            }
        }
    }
```

这个类呢 接受一个整数，一个LockNormal我自己定义的类，在run里面呢先对 num进行判断小于等于0的话，就退出，否则减一。 运行一下
```java
  LockNormal lockNormal = new LockNormal();
        LockRunable one = new LockRunable(15, lockNormal);
        new Thread(one).start();
        new Thread(one).start();
        new Thread(one).start();
        new Thread(one).start();
        new Thread(one).start();
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181114114025614.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ExMDY0MDcyNTEw,size_16,color_FFFFFF,t_70)

num竟然变成负的了，这是因为 五个线程同时对他进行操作，造成的。要避免这种情况，只能一个线程操作的时候，别的线程就不能操作。

我们给程序加锁试一下。首先定义一个 静态成员，静态成员有一个特点是系统初始化的，全局只有一个实例。就相当于上面的 只有一棵树。
```java
static final transient Object lock = new Object();
```
对 工作内容加锁
```java
    @Override
        public void run() {
            while (true) {
                synchronized (lock) {
                    if (num <= 0) {
                        break;
                    }
                    try {
                        Thread.sleep(5);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    System.out.println(Thread.currentThread().getName() + "    " + num--);
                }
            }
        }
    }
```

运行

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181114114606978.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ExMDY0MDcyNTEw,size_16,color_FFFFFF,t_70)

这次就和预期结果相等了。但是 我们可发现 都是都是同一个线程进行操作的，这是因为 sleep时候不会释放锁，其他线程也无法进行操作

传入实例
```java
    static class LockRunable implements Runnable {

        private int num;
    
        private LockNormal lockNormal;
    
        public LockRunable(int num, LockNormal lockNormal) {
            this.num = num;
            this.lockNormal = lockNormal;
        }
    
        @Override
        public void run() {
            while (true) {
                synchronized (lockNormal) {
                    if (num <= 0) {
                        break;
                    }
                    try {
                        Thread.sleep(5);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    System.out.println(Thread.currentThread().getName() + "    " + num--);
                }
            }
        }
    }
```

结果：

也是同样的结果，如果 我们传入this的话 运行还是一样的结果。这就是上面树的问题了，不管是传入的实例，还是this本身实例。只有能保证它们本身是唯一的，也就是“获取锁的地方”只有一个，同一时间内只有一个人能成功获取到锁 进行工作。就和锁静态成员是一样的效果如果我们 new 两个Runnable的话，并且传入this实例的话，就会有两个“获取锁的地方” ，可以有两个人同时工作。

总结
```java
synchronized(放锁的地方){
工作内容
}
```

只要我们保证“获取锁的地方”是唯一的，那么在同一时刻，就只能有一个工作内容会被执行。如果，“获取锁的地方”不是唯一的（一个类new很多实例，锁住this实例，那么同一时刻就会有很多放锁的地方），在同一时刻就会有好多 工作内容被执行。
