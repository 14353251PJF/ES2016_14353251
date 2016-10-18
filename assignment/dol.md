## Lab3-DOL实例分析&编程 ##

姓名：彭靖芳 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;学号:14353251

####**example 1**####

修改example1，使其输出三次方数：

 - 先来分析example1的代码：

	这是example1.xml文件，如下图所示：
	
 ![这里写图片描述](http://img.blog.csdn.net/20161018111033328)

	里面有三个进程，分别是：generator、consumer，square，
	
	 - **generator.c**
	
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这是定义进程，先初始化，也就是执行generator_init函数，然后再执行generator_fire函数：当前位置小于生产长度，则将当前下标写入到输出端，否则就销毁进程。让程序被发射、开火。执行length次后停下来。
	
	![这里写图片描述](http://img.blog.csdn.net/20161018112149490)

	 - **consumer.c**
	
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这是定义消费者进程，先初始化，也就是执行consumer_init函数，然后再执行consumer_fire函数：当前位置小于设定长度，则读出输入信号，并且打印，否则就销毁进程（停下来）。
	
![这里写图片描述](http://img.blog.csdn.net/20161018112622015)

	 - **square.c**
	
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这是定义平方进程square_fire函数：读入输入信号i，将其平方后写入到输出端，重复length次之后停下来。
	
![这里写图片描述](http://img.blog.csdn.net/20161018112745267)
		

	生产者进程的功能是不断生成从1到LENGTH的数，然后传递给生产者的输出端口，通过通道投递到平方进程square。 
　　
	平方进程相当于一个处理的模块，将生产者生成的数经过“处理”再传递到平方进程输出端口投递给消费者进程。本例子中的“处理”是将生产者传来的数进行平方运算。 
　　
消费者进程对最后的结果进行处理，在本例子中是将平方square处理后的数输出出来。 
	
	那么任务一就不难实现，只要将平方模块square中的主要处理操作公式“i=i∗i”改为“i=i∗i∗i”即可。

	最后结果为： 
	
　
![这里写图片描述](http://img.blog.csdn.net/20161018121606988)


	
	下面是得到的example1的.dot图：
	
![这里写图片描述](http://img.blog.csdn.net/20161018121810959)


####**example 2**####
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;修改example2，让3个square模块变成2个：
	
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;与example1原理差不多，只是在xml文件里面修改，使得3个square变为两个square，也就是把xml里面的N由原来的3改为2就可以了。由于可以迭代生成，也就是iterator代码，相当于循环，不断生成square，我们要实现任务二，就把iterator的次数改一下，改为N=2就可以了。迭代生成的代码里面本身就包含了进程process、通道sw_channel、连接connection，端口的生成也是迭代生成的。
		
原来的“3”：

	![这里写图片描述](http://img.blog.csdn.net/20161018122954927)
	
改为“2”：

![这里写图片描述](http://img.blog.csdn.net/20161018123031084)

ietartor：

![这里写图片描述](http://img.blog.csdn.net/20161018123137131)

 -运行example2.xml的结果：
 
![这里写图片描述](http://img.blog.csdn.net/20161018123355601)

可见结果正确。

下面是修改后的example2的.dot图：

![这里写图片描述](http://img.blog.csdn.net/20161018123533461)

于是，完成了本次实验。


####**实验感想**####

这次试验理解原理之后再做，不难，就改了两个值。TA上课讲的较慢，所以实验课就理解了。三个进程generator、consumer、square只是执行内部的功能，xml文件起的是把他们连接起来的作用。定义output、input、端口名字、通道定义（一条线就是一条通道）。连接各个模块，一条线对应两条连接。
Example1中，generator是产生0-19的整数，square就是对整数（输入）做平方处理，consumer就是输出结果。example1包含一个square进程，没有使用迭代iterator，example2包含三个square进程，结果也就是i^8，后来改为两个square，结果也就是i^4。本次试验不难懂，只要细心，自信看实验文档，仔细看代码，理解每一行，每一块代码就可以了。




