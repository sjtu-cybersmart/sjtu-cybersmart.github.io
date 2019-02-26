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
  ---
  下面的是预先在协议中显示定义好的
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
- 下载kitti数据（每条数据都必须带有一个较为精确的时间戳）
- 将KITTI数据转换
