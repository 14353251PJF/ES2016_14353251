## **Ubunt14.04下安装ROS**##

##**安装ROS**##
####**配置Ubuntu的软件源**####
配置Ubuntu要求允许接受restricted、universe和multiverse的软件源,可以根据下面的链接配置:

[http://wiki.ros.org/jade/Installation/Ubuntu](http://wiki.ros.org/jade/Installation/Ubuntu)

要注意这里给的是Ubunt14.04的链接，15.04也可以按照这个链接安装ROS。

Ubunt15.10以及16.04参照下面的链接：

[http://wiki.ros.org/kinetic/Installation/Ubuntu](http://wiki.ros.org/kinetic/Installation/Ubuntu)

####**添加软件源到sources.list**###

设置软件源的代码如下：

```
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
```

添加了正确的软件源，操作系统就知道下载程序的具体位置在哪，并根据命令自动安装软件。

####**设置密钥**###

设置密钥的代码如下：

```
sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key 0xB01FA116
```

####**安装**###

首先确认你的Debian的软件包索引是最新的。

确认你的Debian的软件包索引是最新的代码如下：

```
$ sudo apt-get update
```

在ROS中有许多不同的函数库和工具，建议是**完全安装**，也可以根据自己的要求分别安装。完全安装时的工具包括ROS、rqt、可视化环境rviz、通用机器人库robot-generic libraries、2D（如stage）和3D（如Gazebo）仿真环境2D/3D simulators、导航功能包集navigation and 2D/3D（移动、定位、地图绘制、机械臂控制）、感知库perception（如视觉、激光雷达、RGB-D摄像头等）。

完全安装的代码如下：

```
sudo apt-get install ros-jade-desktop-full
```

若是想自定义安装，下面是找到可用包的代码：

```
apt-cache search ros-jade
```

####**初始化rosdep**###

在使用ROS之前，需要先初始化 rosdep. rosdep，这样不仅能够使你更方便的安装一些系统依赖程序包，而且ROS的一些主要部件的运行也需要rosdep。

初始化rosdep的代码如下：
```
sudo rosdep init
rosdep update
```


####**设置环境**###

添加ROS的环境变量，这样，当你打开你新的shell时，你的bash会话中会自动添加环境变量。

```
echo "source /opt/ros/indigo/setup.bash" >> ~/.bashrc
# 使环境变量设置立即生效
source ~/.bashrc
```
####**安装rosinstall**###

rosinstall命令是一个使用较非常频繁的命令，使用这个命令可以轻松的下载许多ROS软件包。

安装rosinstall的代码如下：

```
sudo apt-get install python-rosinstall
```

####**配置你的ROS环境**###

注意：当你用像apt这样的软件包安装管理器安装ROS，那么这些软件包用户是没有权利的去编辑的，当创建一个ROS package和处理一个ROS package时，你应该始终选择一个你有权限工作的目录作为工作目录。

####**管理你的ROS环境**###

在安装ROS的时候，你会看到提示：source（命令）几个setup.*sh文件，或者甚至添加sourcing到你的shell启动脚本中。这是必须的，因为ROS依赖于结合使用shell环境的概念上。这使得开发依赖不同版本的ROS或者不同系列的package更加容易。

如果你在寻找或者使用你的ROS package上有问题，请确定的你的ROS环境变量设置好了，检查是否有ROS_ROOT和ROS_PACKAGE_PATH这些环境变量。

```
export | grep ROS
```

如果没有，你需要source一些setup.*sh文件。

环境设置文件是为你产生的，但是可以来自不同的地方：

 - 使用package管理器安装的ROS package提供setup.*sh文件；
 - rosbuild workspace使用像rosws这样的工具提供setup.*sh文件；
 - setup.*sh文件在编译和安装catkin package时作为副产品创建。
 
注意：rosbuild和catkin是两种组织和编译ROS代码的方式，前者简单易用，后者更加复杂但是提供更多灵活性尤其是对那些需要去集成外部代码或者想发布自己软件的人。

如果你在Ubuntu上使用apt工具安装ROS，那么你会在'/opt/ros/indigo/'目录中有setup.*sh文件，你可以这样source它们：

```
source /opt/ros/indigo/setup.bash
```

你每次打开新的shell都需要运行这个命令，如果你把source /opt/ros/indigo/setup.bash添加进.bashrc文件就不必要每次打开一个新的shell都运行这条命令才能使用ROS的命令了。

####**创建ROS环境**###

对于ROS Groovy和之后的版本可以参考以下方式建立catkin工作环境。在shell中运行：

```
mkdir -p ~/catkin_ws/src
cd ~/catkin_ws/src
catkin_init_workspace
```

可以看到在src文件夹中可以看到一个CMakeLists.txt的链接文件，即使这个工作空间是空的（在src中没有package），任然可以建立一个工作空间。

```
cd ~/catkin_ws/
catkin_make
```

catkin_make命令可以非常方便的建立一个catkin工作空间，在你的当前目录中可以看到有build和devel两个文件夹，在devel文件夹中可以看到许多个setup.*sh文件。启用这些文件都会覆盖你现在的环境变量，想了解更多，可以查看文档catkin。在继续下一步之前先启动你的新的setup.*sh 文件。

```
source devel/setup.bash
```

为了确认你的环境变量是否被setup脚本覆盖了，可以运行一下命令确认你的当前目录是否在环境变量中：

```
echo $ROS_PACKAGE_PATH
```
输出：

```
/home/youruser/catkin_ws/src:/opt/ros/indigo/share:
/opt/ros/indigo/stacks
```

这样，你的环境就已经建立好了。
下面是我安装ROS以及建立环境的结果：


![这里写图片描述](http://img.blog.csdn.net/20161102094333894)


文章详情是参照下面的博客：

[https://www.cnblogs.com/CZM-/p/5858180.html](https://www.cnblogs.com/CZM-/p/5858180.html)


