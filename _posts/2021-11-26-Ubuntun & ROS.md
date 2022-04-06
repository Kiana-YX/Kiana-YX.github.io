---
layout:     post
title:     Ubuntun
date:       2021-11-27
author:     Kiana
header-img: img/post-bg-coffee.jpg
catalog: true
tags:
    - 学习资料
---
# Ubuntun

## 双系统相关配置

1. 双系统安装

   [windows10安装ubuntun16.04双系统教程](https://www.cnblogs.com/masbay/p/10844857.html)

2. 默认启动系统

   机械革命电脑是开机按F2进入BIOS管理，按面板提示修改Prior即可

   按F10可以选择启动设备

2. 蓝牙键盘

   1. ubuntun系统下将蓝牙键盘与系统连接好

   2. 获取ubuntun下的蓝牙配对linkkey

      ![命令行](https://gitee.com/Kiana-yx/pictures/raw/master/img/image-20211115100413787.png)
      
   3. **管理员**打开cmd
      
      ![image-20211128163253551](https://gitee.com/Kiana-yx/pictures/raw/master/img/win%E9%85%8D%E7%BD%AE%E8%93%9D%E7%89%99%E9%94%AE%E7%9B%98.png)
      
   4. 修改
      
      找到`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\services\BTHPORT\Parameters\Keys\`，将ubuntu下的linkkey写入注册表
      
      参考链接：
      [win10 ubuntu16 双系统共用蓝牙鼠标](https://blog.csdn.net/10km/article/details/61201268?utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-3.control&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-3.control)
   
3. 启动项管理

   [Windows](https://blog.csdn.net/qq459080123/article/details/81392060)
   
   [Ubuntun](https://blog.csdn.net/qq_27278957/article/details/100099169?spm=1001.2101.3001.6650.2&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ELandingCtr%7Edefault-2.queryctrv2&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ELandingCtr%7Edefault-2.queryctrv2&utm_relevant_index=5) `gnome-session-properties`

4. 小技巧

   [解决pip下载过慢的问题](https://blog.csdn.net/SampsonTse/article/details/103997667)
   
5. ubuntun扩容

   [通过GParted](https://zhuanlan.zhihu.com/p/422981369)

   [Win10方案](https://www.diskgenius.cn/)
   
6. [QQ、微信](https://www.cnblogs.com/voidint07/p/14097092.html)

7. c

   ```
   $ sudo apt-get install ros-noetic-rqt
   $ sudo apt-get install ros-noetic-rqt-common-plugins
   ```
   


## LINUX 

1. 软件安装与卸载
	
	  ```
	  1.cd 到安装包的目录
	  2.sudo dpkg -i file.deb
	```
	
	​		  卸载：sudo apt-get remove 【软件名 】
	
2. vim常用功能

     https://zhuanlan.zhihu.com/p/68111471
     
3. 文件命令

     ```
     # 删除原本软连接，再新建一个
     rm /usr/bin/python3 
     ln -s /usr/local/python3/bin/python3 /usr/bin/python3
     ```

     ```
     # 通过命令行在文件管理器中打开一个文件夹
     xdg-open way
     nautilus way
     ```
4. [右键添加新建空白](https://blog.csdn.net/bingxinyang123/article/details/110543742)

### EasyConnect 安装

[大致流程](https://www.cnblogs.com/cocode/p/12890684.html)
[详细版本](https://blog.csdn.net/malloc_luo/article/details/108915451)
[查看架构](https://shliang.blog.csdn.net/article/details/109166131?spm=1001.2101.3001.6661.1&utm_medium=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-1.no_search_link&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-1.no_search_link&utm_relevant_index=1)——amd64

```
# 查找链接库，我的ldd不能直接用，暂未解决
ldd /usr/share/sangfor/EasyConnect/EasyConnect | grep pango
     
# 在文件夹中复制权限
sudo chmod -R 777 /usr/share/sangfor
```

最后访问 https://vpn.whu.edu.cn/

### 为AppImage创建快捷方式

[添加图标并添加到应用](https://zhuanlan.zhihu.com/p/215507075)

## ROS环境安装

[安装](https://blog.csdn.net/qq_44339029/article/details/120579608?utm_source=app&app_version=4.16.0)

## ROS 使用
### 从零开始

[教程参考-古月局ROS入门21讲](https://www.bilibili.com/video/BV1zt411G7Vn?from=search&seid=13971428836986102434&spm_id_from=333.337.0.0)

[代码参考--PCL_test](https://adamshan.blog.csdn.net/article/details/82901295)

1. 创建工作空间catkin_ws

   ```
   mkdir -p ~/catkin_lib/src
   cd ~/catkin_lib/src
   catkin_init_workspace
   ```

2. 编译工作空间

   ```
   cd ~/catkin_lib/
   catkin_make
   ```

3. 设置环境变量

   注意catkin_make clean 后必须要source一下

   ```
   source devel/setup.bash
   ```
   
4. 检查环境变量

   ```
   echo $ROS_PACKAGE_PATH
   输出：/home/kiana/catkin_lib/src:/home/kiana/catkin_ws/src:/opt/ros/noetic/share
   ```

   为了不用在小车的 Ubuntu 上每次都 `source` 自己的工作空间，把自己的 workspace 加到 `~/.bashrc` 文件末尾：添加source /home/kiana/catkin_ws/devel/setup.bash

   `source ~/.bashrc`
   
5. 创建功能包

   ![](https://gitee.com/Kiana-yx/pictures/raw/master/img/202202141058546.png)

   ```
   catkin_create_pkg tim_pkg std_msgs rospy roscpp
   ```

6. 。。
### 坐标系

为了让所有开发者围绕相同的坐标系开发软件，ros中制定了标准的坐标系定义方案。此时与激光雷达用户手册上有所不同

![](https://gitee.com/Kiana-yx/pictures/raw/master/img/202201032023757.png)

### ROS Nodelet 

ROS不同节点之间通过话题通信时，是通过网络实现的，且需要经过序列化与反序列化，延时比较高。ROS Nodelet将不同节点封装在一个进程中，进而以共享数据的方式进行通信，提高通信效率。

ros nodelet是ros为提高节点之间，通过话题通信效率而实现，常用于数据量比较大的场景，例如激光雷达和图像数据等。其实现机制是通过ros plugin技术，将节点封装为plugins，然后在加载到同一个进程中，实现零拷贝通信。

### bug

1. std_msgs写错成stdmsgs

   ```
   CMake Error at /opt/ros/noetic/share/catkin/cmake/catkinConfig.cmake:83 (find_package):
     Could not find a package configuration file provided by "stdmsgs" with any
     of the following names:
   
       stdmsgsConfig.cmake
       stdmsgs-config.cmake
   
     Add the installation prefix of "stdmsgs" to CMAKE_PREFIX_PATH or set
     "stdmsgs_DIR" to a directory containing one of the above files.  If
     "stdmsgs" provides a separate development package or SDK, be sure it has
     been installed.
   ```

   ```
   修改learning_topic中CMakeList
   .txt
   find_package(catkin REQUIRED COMPONENTS
     geometry_msgs
     roscpp
     rospy
     std_msgs
     turtlesim
   )
   ```

2. **{**catkin_LIBRARIES**}** rather than **(**catkin_LIBRARIES**)**

### roslaunch

1. 基本格式

   ```
   <launch>
       <node pkg="euclidean_cluster" type="euclidean_cluster_node" name="euclidean_cluster_node" output="screen"/>
       <node pkg="pcl_test" type="pcl_test_node" name="pcl_test_node" output="screen"/>
       <node pkg="rosbag" type="play" name="playe" output="screen" args="--clock /home/kiana/indoor.bag"/>
   </launch>
   ```

2. bag使用

   ```
   <node pkg="rosbag" type="record" 
           name="bag_record" 
           args="topicname1 topicname2 -o /home/xxx/bagfiles/aaa.bag"/>
   
   
   <node pkg="rosbag" type="play" name="playe" output="screen" args="--clock -l /home/kiana/car.bag"/>
   <!-- 注意这里bag文件的路径必须为绝对路径，-L循环播放-->
   ```

   

3. 。。

## 踩坑

1. ROS与anacoda不好共存，，，

   [解决编译](https://blog.csdn.net/qq_36013249/article/details/103311001)

2. 。。
