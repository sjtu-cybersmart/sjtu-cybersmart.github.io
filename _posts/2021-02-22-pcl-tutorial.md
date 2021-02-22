---
layout: post
title: 学习目录
categories: [blog]
description: 点云学习
keywords: Wiki
---

# 环境配置
- 后期作业有深度学习相关内容，建议从最开始就使用如下配置完成作业，这样从头学到尾，环境配置都不用来回折腾

- ### 基础配置
  - 操作系统：ubuntu16.04 / ubuntu18.04
    - 用windwows也可以，就是可能会遇到坑然后解决不了，因为大部分人还是用ubuntu进行学习和开发。
  - 编程语言：[Anaconda](https://docs.anaconda.com/anaconda/install/) + python3.7
    - anaconda是python运行环境，安装好后可以启动任意版本的python环境，所以只要安装了anaconda就等于安装好了python
    - 用C++也可以，就是代码写起来难度更大；后面搞深度学习还是要切回python；特别是大作业结合了深度学习和经典算法内容，如果是分别用C++和Python实现的，难以统一起来完成大作业，所以建议直接python从头写到底。
  - 必备python库：[Opend3D](https://github.com/intel-isl/Open3D)
    - 点上面的链接进入github仓库页面，按照下面的install操作进行安装。
- ### 深度学习配置
  - nvidia驱动安装（百度上有很多教程，注意ubuntu16.04和ubuntu18.04安装步骤可能不一样）
  - pytorch安装
  - cuda安装
    - 实际上，安装完驱动和pytorch之后，就可以通过pytorch编程用显卡训练自己写的简单一点的网络了。
    - 但是在跑PointRCNN这种开源网络的时候还是会提示OsError: CUDA_HOME environment variable is not set。
    - 这时候就需要安装cuda了。

## 注意 
- 环境配置一定要搞好，后面才能顺顺利利地完成作业。
- 可以先搞好基础配置，就可以完成前几章的作业了。
- 前几节课内容简单，作业做完后可以继续鼓捣深度学习的环境配置。

---

# 数据集下载
- [modelnet40_normal_resampled](https://shapenet.cs.stanford.edu/media/modelnet40_normal_resampled.zip)
- [KITTI depth dataset(Optional)](http://www.cvlibs.net/datasets/kitti/eval_depth_all.php)
- [KITTI 3D object detection](http://www.cvlibs.net/datasets/kitti/eval_object.php?obj_benchmark=3d)下载如下几个数据集
  - [Download left color images of object data set(12 GB)](http://www.cvlibs.net/download.php?file=data_object_image_2.zip)
  - [Download Velodyne point clouds, if you want to use laser information (29 GB)](http://www.cvlibs.net/download.php?file=data_object_velodyne.zip)
  - [Download camera calibration matrices of object data set (16 MB)](http://www.cvlibs.net/download.php?file=data_object_calib.zip)
  - [Download training labels of object data set (5 MB)](http://www.cvlibs.net/download.php?file=data_object_label_2.zip)
  - [Download object development kit (1 MB)](https://s3.eu-central-1.amazonaws.com/avg-kitti/devkit_object.zip)
- registration_dataset.zip(由深蓝学院提供)

## 各章节需要的数据集统计
- 第一章:modelnet40_normal_resampled, KITTI depth dataset(Optional)
- 第四章:KITTI 3D object detection
- 第五章:modelnet40_normal_resampled
- 第六章:KITTI 3D object detection
- 第七章:modelnet40_normal_resampled
- 第八章:modelnet40_normal_resampled
- 第九章:registration_dataset.zip
- Final Project:KITTI 3D object detection
