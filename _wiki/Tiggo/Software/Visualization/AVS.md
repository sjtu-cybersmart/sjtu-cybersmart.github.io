---
layout: wiki
title: AVS
categories: [wiki]
description: AVS
keywords: wiki, Tiggo, visualization, AVS
---

# 介绍
- AVS是一个新的用来描述和可视化自主车辆的感知，运动，和规划数据的标准。
- 它提供一个功能强大的基于Web的应用程序。
- 作为一个独立的、标准化的可视化层，AVS使开发人员不必为他们的自主车辆构建定制的可视化软件。通过AVS抽象可视化，开发人员可以专注于驱动系统、远程协助、映射和模拟的核心自主能力。
- 我们围绕两个关键部分构建了系统：
  - xviz提供数据（包括管理和规范)。
  - streetscape.gl是为Web应用程序提供动力的组件工具包。
- AVS的与众不同：
  - 它被设计用来将数据与任何底层平台分离。
  - 其有限的、小的规格使工具更容易开发。
  - 它的数据格式的要求促进了快速传输和处理。

---

## XVIZ
XVIZ就是把自主系统中生成的数据灌入到AVS中的工具（或者说规范、协议）

### XVIZ相关概念
- Datum: 一个数据对象
- Stream: 数据流，一系列带有时间戳的datum。一个stream就是一个遵循类似路径的语法的identifier（比如'/object/bounds'），用于存储同类数据。
  - Stream Name: 数据流的名字
  - Stream Type: datum的数据类型
  - Pose Stream: 一系列的位置信息，用来表示动作对象的位置包括和它相关的所有坐标系转换。
  - Geometry Types: 几何基本体
  - Variables: 存放数据的数组
  - Time series: Time series类型的值会随着时间改变而改变，适合用来描述速度这种瞬时值。
  - Tree Table: 分层数据结构，用于传输密集的记录类型数据。
  - Image Stream: 二进制格式图像数据。
- Source: 数据流的来源，一个source包含一个metadata和多个streams。可以是离线和在线数据。
- Metadata: XVIZ特有的一种数据类型，包含了对一个source和属于它的streams的描述。
- Primitive: 一个几何对象，如点、线、多边形等。它可以被贴上标签并赋予特殊的样式（颜色等）。
- Style: 对象属性的样式表，通过stream和class用来描述对象的属性。
- Object: 通过将identifiers、primitive、variables 、time series组合在一起得到。标识符允许跨流和时间片链接信息。
- Variable: 一次生成的值序列。比如一条计划好的道路上的期望行驶速度。每次运动更新，都会更新整个期望速度列表。
- Declarative UI: 将UI元素（如绘图、控件、表和视频面板）和流名称数据绑定进行映射的结构化数据架构。此数据与metadata一起发送，使其与数据源紧密耦合。
- Video: xviz可以与外部视频源同步，前提是它们已以适当的方式编码。
- Encoding: xviz协议规范没有规定任何给定的编码，但是Xviz库支持JSON中的编码和解析。

## 简单的教程Demo（KITTI数据转换为XVIZ）
- 下载kitti数据（每条数据都必须带有一个较为精确的时间戳）[链接](https://avs.auto/#/xviz/getting-started/converting-to-xviz/downloading-data)
- 将KITTI数据转换
  - ego car数据处理
    - 位置:
      1. 定义metadata(stream的名字、类型)：
      		const xb = xvizMetaBuilder;
		xb.stream('/vehicle_pose').category('pose');
      2. 每一帧都需要更新一次current pose：
		xvizBuilder
		 .pose('/vehicle_pose')
		 .timestamp(pose.timestamp)
		 .mapOrigin(pose.longitude, pose.latitude, pose.altitude)
		 .orientation(pose.roll, pose.pitch, pose.yaw)
		 .position(0, 0, 0);
      3. 注意这里，地图的原点（mapOrigin）一直在不断变化，地图的原点就是车辆的经纬度，所以position一直保持（0，0，0）。在有些情况下，地图的原点被固定了，position就应该根据当前车辆的经纬度去改变。
    - 加速度和速度：
      1. 定义metadata(stream的名字、类型)：
		const xb = xvizMetaBuilder;
		xb.stream('/vehicle_pose').category('pose');//localization
		
		 .stream(this.VEHICLE_ACCELERATION)//accelerate
		 .category('time_series')
		 .type('float')
		 .unit('m/s^2')
		 
		 .stream(this.VEHICLE_VELOCITY)//velocity
		 .category('time_series')
		 .type('float')
		 .unit('m/s')
        * 可以看到xvizMetaBuilder只定义了一个，然后将所有的streams信息一起注册好。
        * this. VEHICLE_VELOCITY= /vehicle/velocity。在类的初始化里面就定义好了。
        * unit()内部是一个字符数据，将来会显示在面板上。
      2. 同样，每一帧需要更新速度加速度：
		xvizBuilder
		 .timeSeries(this.VEHICLE_VELOCITY)
		 .timestamp(velocity.timestamp)
		 .value(velocity['velocity-forward']);
		
		xvizBuilder
		 .timeSeries(this.VEHICLE_ACCELERATION)
		 .timestamp(acceleration.timestamp)
		 .value(acceleration['acceleration-forward']);
  - Object数据处理(来自于激光雷达的检测结果)
    - 位置:
      1. 定义metadata:
		this.FIXTURE_TRANSFORM_POSE = {
		  x: 0.81,
		  y: -0.32,
		  z: 1.73
		};
		.stream(this.TRACKLETS_TRACKING_POINT)
		.category('primitive')
 		.type('circle')
		.streamStyle({
		  radius: 0.2,
		  fill_color: '#FFFF00'
		})
		.pose(this.FIXTURE_TRANSFORM_POSE)

        * 首先填写激光雷达和GPS中心的位置偏差FIXTURE_TRANSFORM_POSE，用来进行坐标转换，单位米。
      2. 用圆表示objects:
		xvizBuilder
  		  // ...
		  .primitive(this.TRACKLETS_TRACKING_POINT)
		  .circle([tracklet.x, tracklet.y, tracklet.z])
		  .id(tracklet.id);
- 后面的不详细介绍了，上面列举的代码只是帮助理解，实际使用的时候可以直接下载源码！[链接](https://github.com/uber/xviz)
      

