---
title: OBB与Triangle求交
date: 2022-09-08 17:49:59
mathjax: true
categories:
- Algorithm
- Computer Graphics
tags:
- OBB 
- Intersect

---

`OBB` 与三角形求交.


### 预备知识

#### 点到面的距离方程

##### 面的方程

- 定义平面`A`的法向 $\vec{n}$ `{a,b,c}`,
- 已知平面行一点$P_0 \lbrace x_0,y_0,z_0\rbrace$
- 任意点 $P$`{x,y,z}`
- 面方程:
  - $\overrightarrow{P - P_0} \cdot  \vec{n} = 0$

##### 点到面的距离

###### 法向量法

- $\vec{n}$  - 平面的法向量
- $d$ - 点到平面的距离
- $P$ - 平面外一点
- $A$ - 平面上一点
- $d = \vec{n} \cdot \vec{PA} $
  - PA在法向方向上的投影既是点$P$到平面的距离 

##### 射线的方程

- 射线方程
  - $O$ 为起点， 沿着方向 $\vec{d}$
  - 射线方向上任意一点$P_0$
  - $P_0 = O + \vec{d}$ 
  
 