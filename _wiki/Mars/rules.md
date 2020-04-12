---
layout: wiki
title: Developing rules of Mars Plan
categories: [wiki]
description: Developing rules designed for Mars plan.
keywords: rules
---

## Rules
- #### Change of nodes or topics should be approved by the Mars Planning Committee. Any personal modification is invalid.
- #### Edition of cyber_msgs is owned by the Mars Planning Committee. All developers share with the same version of cyber_msgs.
- #### ROS messages should be used only when pub-sub or services are required. In another word, if a data structure is only used in one node, you can either create a class for it or used data structures provided by Opencv, PCL, Eigen and so on.
- #### Try to use standard msgs provided by ROS to fix communication requirements. Msgs outside of cyber_msgs should be defined in certain packages without creating another cyber_msgs package.

