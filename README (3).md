
### 1.Description ###

####&nbsp;&nbsp;&nbsp;&nbsp;DOL框架描述：####
#####**&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Distributed Operation Layer** : 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The distributed operation layer (DOL) is a software development framework to program parallel applications. The DOL allows to specify applications based on the Kahn process network model of computation and features a simulation engine based on SystemC. Moreover, the DOL provides an XML-based specification format to describe the implementation of a parallel application on a multi-processor systems, including binding and mapping.
![这里写图片描述](http://img.blog.csdn.net/20160926192525103)
<br>
### 2. How to install###
####&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;DOL安装笔记：####
 （1） 安装虚拟机Ubuntu

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;本次试验环境是在Linux下进行的，先安装虚拟机Ubuntu。下面是链接：

 - VMWARE教程：
[http://jingyan.baidu.com/article/0320e2c1ef9f6c1b87507bf6.html](http://jingyan.baidu.com/article/0320e2c1ef9f6c1b87507bf6.html)

 - Ubuntu下载：
 [http://www.ubuntu.com/download/desktop
](http://www.ubuntu.com/download/desktop)

<br>
（2） 配置环境

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;安装下面的环境：
```
$	sudo apt-get update`

$	sudo apt-get install ant

$ 	sudo apt-get install openjdk-7-jdk

$	sudo apt-get install unzip
```
<br>
 （3）下载及解压文件

 -  文件下载的链接：
[http://jingyan.baidu.com/article/c33e3f48a5c153ea15cbb5b2.html
](http://jingyan.baidu.com/article/0320e2c1ef9f6c1b87507bf6.html)

 - 解压文件：
 新建DOL文件夹的命令语句：
```
$	mkdir dol
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;将dolethz.zip解压到 dol文件夹中：
```
$	unzip dol_ethz.zip -d dol
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;解压systemc：
```
$	tar -zxvf systemc-2.3.1.tgz
```

 （4）编译systemc
 
 - 解压后进入systemc-2.3.1的目录下： 
```
$	cd systemc-2.3.1
```

 - 新建一个临时文件夹以及进入该文件夹：
 
```
$	mkdir objdir
$	cd objdir
```

 - 运行configure(能根据系统的环境设置一下参数，用于编译)：
```
$	../configure CXX=g++ --disable-async-updates
```

 - 编译：

```
$	sudo make install
```

 - 记录当前的工作路径：
```
$	pwd
```
 &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;
<br>
 （5）编译dol
 
 - 进入dol文件夹： 
```
$	cd ../dol
```
 - 修改：build_zip.xml文件
 
 找到下面的两条语句，并把YYY修改为上页的执行pwd命令后的结果：

```
<property name="systemc.inc" value="YYY/include"/>
<property name="systemc.lib" value="YYY/lib-linux/libsystemc.a"/>

```
 - 编译dol
```
$	ant -f build_zip.xml all
```
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;成功则显示build successful.下面是我build后的结果，可见build successful：
 <br>
 ![这里写图片描述](http://img.blog.csdn.net/20160926192719638)
 
 - 运行第一个例子
```
$	cd build/bin/main
$	ant -f runexample.xml -Dnumber=1
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这是我一开始运行第一个例子的结果：
<br>
![这里写图片描述](http://img.blog.csdn.net/20160926192908219)
<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;发现和TA给的PPT运行的结果不一样，虽然上方显示build successful了，也就相当于没有printf出来，后来的解决方法是这样的：把systemc-2.3.1文件夹里面的lib-linux也对应加上64，因为电脑是64位的，如下图所示：
![这里写图片描述](http://img.blog.csdn.net/20161018085422721)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;下面才是最终成功的结果：
![这里写图片描述](http://img.blog.csdn.net/20161018091354973)

### 3. Experimental experience###
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;本次实验进行的不是很顺利，困难重重。首先一开始由于重装了系统，虚拟机只能重新安装，可是装完之后，不知道哪里出问题了，一直拖不了文件，询问了很多人，也没有解决，自己百度了，说安装VMware Tools，我发现我的是安装好了的，于是就重新安装，照着网上的教程，重新安装VMware Tools，第一次，结果文件依旧拖不了；重装第二次，还是不行；当我得知有同学重装了5遍，我决定重装第三遍，结果文件还是拖不了。然后决定放弃，用U盘，试着把文件复制到虚拟机里面，可是，根本没有反应，识别不了我的U盘，真的不知道到底哪里出问题了。后来，请求同学帮助，才试出来，原来是因为我的设置里面，虚拟机的USB接口的兼容性是2.0，而我的U盘是3.0，这才改了过来，于是实现了文件的复制，接下来的配置DOL就进行的比较顺利，多谢TA耐心教导以及同学的耐心帮助，才使得问题得以解决。教会了我：遇到问题，要多找找解决办法，不要纠结于一种办法来解决问题，要懂得变通，不能太固执。


 

