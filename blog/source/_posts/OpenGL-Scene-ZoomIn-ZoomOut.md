---
title: OpenGL Scene ZoomIn & ZoomOut
date: 2022-08-03 21:01:52
categories:
- Computer Graph
- works
- SCUU
tags:
- OpenGL
---

<!-- # `OpenGL Scene ZoomIn & ZoomOut` -->
## 实现思路
- 获取鼠标滚轮的变化(这个改变是带方向的 向上获取一个正值,向下获取的是一个负值)
- 将获取的这个值映射到和投影变换参数上
- 修改投影矩阵
- 重新绘制,你将获得缩放后的场景


