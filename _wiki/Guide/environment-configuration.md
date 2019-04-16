---
layout: wiki
title: 代码环境配置
categories: [wiki]
description: 依赖的安装步骤
keywords: wiki, environment configuration
---

## 安装glog
> sudo apt-get install libgoogle-glog-dev

## 安装gflag
> sudo apt-get install libgflags-dev

## 安装gtest
- 下载源码&编译
```
git clone https://github.com/google/googletest
cd googletest
mkdir build
cd build
make -j8
```
- 将生成的静态库拷贝到系统中
```
cd lib
sudo cp libgtest*.a /usr/lib
```
- 将源码中的include文件夹拷贝到系统中
```
cd ../../googletest/
sudo cp -a include/gtest/ /usr/include/
```
- 安装dev文件
> sudo apt-get install libgtest-dev

## ignore driver
- 在自己电脑上跑包的时候，是不需要用到底层驱动的，所以在catkin_make之前，进入到driver文件夹，添加CATKIN_IGNORE文件

## 安装ros包
- 这里列举的不全面，编译的时候根据提示安装缺少的包就行
```
sudo apt-get install ros-kinetic-driver-base
sudo apt-get install ros-kinetic-mrpt-bridge
sudo apt-get install ros-kinetic-mrpt-map
sudo apt-get install ros-kinetic-mrpt-localization
sudo apt-get install ros-kinetic-sbpl
sudo apt-get install ros-kinetic-ompl
sudo apt-get install ros-kinetic-jsk-recognition
sudo apt-get install ros-kinetic-jsk-rviz-plugins
```

# conda&ros
- 目前来说ros的python2.7和conda环境中的python3无法共存。
- 虽然网上有一些办法，但是在编译msg文件的时候，ros库文件中会执行import em操作。
- 在python3中，需要执行import empty，可能可行的方法是修改ros源码，把em改成empty。
- 这里建议直接把.bashrc中conda的引用注释掉，不要用conda来编译和运行ros。
- 除此之外，如果之前在conda环境下尝试编译过ros包，还需要把devel和build文件夹删掉，重新编译。

## 编译
- 编译srvs
> catkin_make --pkg tiggo_srvs
- 编译msgs
> catkin_make --pkg tiggo_msgs
- 编译
> catkin_make
