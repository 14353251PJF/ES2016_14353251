## Lab4-死锁 ##

姓名：彭靖芳 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;学号:14353251


###死锁###
死锁，就是指两个或两个以上的进程在执行过程中，由于竞争资源或由于彼此通信而造成的一种阻塞现象，它们都将无法进行下去。此时，称系统处于死锁状态或系统产生了死锁。

####死锁产生的4个必要条件：####

（1） 互斥：一个资源每次只能被一个进程使用。

（2） 占有并等待：一个进程因请求资源而被阻塞时，获得资源的进程对持有的资源保持不放。

（3） 非抢占：进程已获得资源，在未释放资源之前，不能被强行剥夺，只能在使用完时由自己释放。

（4） 循环等待：存在若干进程形成一种首尾相连的循环等待资源关系。


###实验过程###
1、先配置好java环境，配置好环境后，在cmd中输入javac，如果得到下面的结果，就说明环境配置成功，那么java才可以正常编译以及运行：

![这里写图片描述](http://img.blog.csdn.net/20161019213820354)

2、编写Deadlock.java代码
（以及解释产生死锁的原因）

下面是class A：

![这里写图片描述](http://img.blog.csdn.net/20161019214521770)

下面是class B：

![这里写图片描述](http://img.blog.csdn.net/20161019214653538)

可以通过代码看出来，class A以及class B函数都是使用了关键字 synchronized。

 - 关于关键字 synchronized：

	当它用来修饰一个方法或者一个代码块的时候，能够保证在同一时刻最多只有一个线程执行该段代码。
当一个线程访问object的一个synchronized同步代码块或同步方法时，其他线程对object中所有其它synchronized同步代码块或同步方法的访问将被阻塞。

其实加了synchronized这个关键字，主要的目的就是产生死锁条件：让系统最多只有一个线程在运行（属于互斥条件之一）。

B类用A的方法，A类用B的方法，也是产生死锁条件之一。

class A中：methodA（）函数，调用b.last（）函数，也就是输出“Inside B.last()”；last（）函数输出“Inside A.last()”。
class B中：methodB（）函数，调用a.last（）函数，也就是输出“Inside A.last()”；last（）函数输出“Inside B.last()”。

下面是class Deadlock函数：

![这里写图片描述](http://img.blog.csdn.net/20161019215838918)


一共有两个线程在Deadlock.java里面：main是主线程以及t线程，分别调用a.methodA(b)以及b.methodB(a)。

首先新建一个线程t，开始线程t，经过一个延时后，调用a中的methodA（b），也就执行了b.last函数，输出“Inside B.last()”；当主线程（main）调用a中的methodA函数的时候，此时，不允许别的线程调用a的last函数，但是线程t执行的时候，同时也调用了run函数，也就是b又调用了methodB函数。上面已经分析过了，methodB函数里面是a对last函数的调用，输出“Inside A.last()”，这样一来，A和B若同时竞争CPU资源，就可能会产生了冲突。

就像下图所示，a.methodA（b）与b.methodB（a）可能会并行发生：

![这里写图片描述](http://img.blog.csdn.net/20161020155447051)

分析得知，满足死锁产生的四个条件：

（1）互斥：run和t 线程同步进行，a.methodA（b）与b.methodB（a）可能会并行发生，会发生CPU资源的抢占。

（2）占有并等待：使用了key关键字synchronized，只有在执行完之后，才会释放资源。执行时，是占有资源不放的。

（3）非抢占：均不是抢占的。

（4）循环等待：t线程执行a.methodA(b)即b.last()，run执行b.methodB(a)即a.last()，这样一来，a等待着b释放资源，b等待着a释放资源。


于是产生了死锁。


3、实验截图

（1）当count设置为10000时：


![这里写图片描述](http://img.blog.csdn.net/20161020160536232)

第7次就停了。

（2）当count设置为7500时：

![这里写图片描述](http://img.blog.csdn.net/20161020160733027)

第34次就停了。
可见多少次停是随机的。

###实验感想###

本次试验，难度不大，加上之前操作系统学习了死锁，这是对前面知识的复习以及实践。发现count（相当于延时）的选取有讲究，可能会出现选的太大或者太小，不会出现死锁的情况，count太大了可能run函数已经执行完了，释放了资源，太小了，可能Deadlock（）也就是t线程执行完毕了，也释放了资源。主要就是延时的调节，让Deadlock（）函数与run（）函数同时执行，那么这个时候，定会产生死锁。








