---
layout: wiki
title: 规划控制进阶方案
categories: [wiki]
description: 如何快速学习无人驾驶的规划控制
keywords: wiki, learning guide, planning
---

------

# 规划控制进阶方案
## 自我审核
- 谷歌C++编程规范是否掌握？
- 谷歌Python编程规范是否掌握？
- Git基本操作是否熟练？
- Markdown基本语法是否掌握？
- ROS基础教程是否完整学习过？
### 以上审核内容若有一条没有完成，请谨慎开始开发实验室代码！

## 规划学习（学到这里问实验室要资料）
- ### 有限状态机
  - smach是较为经典的statemachine（状态机）实现方案，特别是它与ros完美融合，可视化的状态跳转界面非常清晰。实现一个简单的smach状态机例程，控制虚拟机器人在stage中依次运动到设定好的点，实现巡逻。
  - 了解boostchart库中的statemachine实现方案。尝试写一些demo。
  - 学习强化学习相关理论。

- ### 轨迹规划
  - 了解A*，D*全局路径规划原理（空间搜索规划）。
  - 了解并使用teb局部路径规划（时空优化规划）在stage中实现car-like机器人的运动。
  - 了解Frenet坐标系，尝试用Frenet坐标系进行多项式拟合路径规划。


