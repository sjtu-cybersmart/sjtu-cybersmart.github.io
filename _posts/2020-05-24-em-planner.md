---
layout: post
title: EM Planner
categories: [blog]
description: EM Planner
keywords: apollo, planning
---

# 简介
- EM Planner是Apollo面向L4的实时运动规划算法
- 该算法在Routing模块给出的参考线的Frenet坐标系下，进行车道级的路径和速度规划
- 主要通过Dynamic Programming和基于样条的Quadratic Programming实
- [论文地址](https://arxiv.org/abs/1807.08048)

# 整体框架
- ![pic1](https://img-blog.csdnimg.cn/20200525172503406.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM1NTAzOTcx,size_16,color_FFFFFF,t_70)

# 路径-速度迭代算法
- Frenet坐标下的轨迹规划实际上带约束的3D最优求解
  - [直接3D最优化求解和路径-速度解耦求解](http://ieeexplore.ieee.org/iel5/5967842/5979525/05980223.pdf)
  - [在SLT坐标系下使用轨迹采样或Lattice搜索](https://ieeexplore.ieee.org/document/5354448)
  - [路径-速度解耦规划](https://www.researchgate.net/publication/308692093_Tunable_and_Stable_Real-Time_Trajectory_Planning_for_Urban_Autonomous_Driving)

- EM Planner迭代地进行路径和速度最优求解
  - 是路径-速度解耦规划的一种
  - 用上一帧的轨迹预测和周围障碍物的交互时间和位置
  - 利用上面的交互时空的预测迭代生成一条新的path

# 决策和交通规则约束
- 交通规则直接影响决策模块
- 决策优先于规划，为规划带来更明确的意图，减少最优求解的搜索空间。

# 横向决策规划
- ## SL投影
  - 感知结果往往是UTM坐标系，为了方便处理，所有障碍物都从UTM投影到Frenet坐标系（x,y ->s,l）
  - ![pic2](https://img-blog.csdnimg.cn/20200525173216695.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM1NTAzOTcx,size_16,color_FFFFFF,t_70)
- ## 安全走廊生成
  - 根据障碍物的SL投影，一旦确定我们要从静态障碍物的左侧或右侧通过，就可以生成一条将障碍物排除在外的安全走廊
  ```
  vector<tuple<s, right_l, left_l>> boundary;
  ```
- ## 规划（QP优化）
  - ### 约束
    - 初始状态是ego当前在SL坐标系下的位置速度和加速度
    - 边界约束和动力学可行性。这些约束都会施加在每个s处，通过限制l来将车辆限制在车道内。
    - ![pic3](https://img-blog.csdnimg.cn/20200525173348667.png)
    - ![pic4](https://img-blog.csdnimg.cn/20200525173358933.png)
  - ### 目标
    - 其中$g(s)$为DP规划的路径，$f(s)$的一阶导表示朝向、二阶导表示曲率、三阶导表示曲率的导数。该函数描述了避让障碍物和曲线光滑性之间的权衡。
    - ![pic5](https://img-blog.csdnimg.cn/2020052517334096.png)

# 纵向决策规划
- ## 决策
  - 将障碍物的预测轨迹投影到ST图上，与上面的SL投影类似
  - 利用DP求解一条从(s=0,t=0)到(s = x,t=8)的最优speed profile（其实就是迪杰斯特拉算出来的最优path）
  - ![pic6](https://img-blog.csdnimg.cn/20200525173431186.png)
  - ![pic7](https://img-blog.csdnimg.cn/20200525173438832.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM1NTAzOTcx,size_16,color_FFFFFF,t_70)
  - 第一项是速度误差，g用来惩罚与$V_ref$的不同的误差。第二项、第三项用来描述曲线的光滑程度。最后一项用来描述障碍物代价，以到障碍物的距离来衡量。
- ## 规划
  - 该speed file已经决定了对每个障碍物采取yeild或者overtake的决策，通过这些决策，可以生成ST图中的安全走廊
  - 然后利用该安全走廊仿照path优化，求解一条精细的速度曲线



