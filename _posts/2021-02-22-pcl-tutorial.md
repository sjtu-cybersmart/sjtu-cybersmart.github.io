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
  - 编程语言：[Anaconda](https://docs.anaconda.com/anaconda/install/) + python3.7
    - anaconda是python运行环境，安装好后可以启动任意版本的python环境，所以只要安装了anaconda就等于安装好了python
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
