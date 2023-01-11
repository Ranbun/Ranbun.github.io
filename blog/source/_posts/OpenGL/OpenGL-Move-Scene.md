---
title: OpenGL Move Scene
date: 2022-08-03 21:05:59
categories:
  - Computer Graph
  - works
  - SCUU
tags:
  - OpenGL
---

<p>
&ensp;&ensp;<code>OpenGL</code>场景移动...
</p>

<!-- more -->

<center><h1> OpenGL Move Scene - 平面上移动 </h1></center>

- 我们考虑一个简单的平面的移动
  - 某个维度为 `0`,我们只需要在某个平面上移动 

## 1. 计算移动的方向
- 在此之前对于相机的概念，希望您是熟悉的
- 虽然在`OpenGL`中本身并没有相机的概念
  - `eye`    - `point`
  - `center` - `point`
  - `up`     - `vector`
 
### 计算相机的朝向 

- 右方向：$\vec{front} = center - eye$

### 计算相机的右方向
- $\vec{right} = \vec{front} \times \vec{up}$
- 记得将向量$\vec{right}$归一化处理

## 2. 鼠标的移动
- 规定使用鼠标右键平移场景
- 我们使用`Qt`的鼠标事件
  - 在`x`方向上，两次记录的位置相减携带了移动的方向
  - 在`y`方向上，`Qt`窗口的原点与`OpenGL`的窗口存在差别，需要做额外的处理 - 反向

### 1. 当右键按下我们记录当前的鼠标的坐标

### 2. 触发移动事件
- 此处每一次出发移动我们将计算一次场景的移动 
  - `X`方向 - `offsetX`
  - `Y`方向 - `offsetY`

- 更新我们记录的上一次的鼠标的位置

## 移动场景
- 将这个移动映射成为数据的比例，作为移动方向的系数，作用到`eye`&`center`, 重新计算视图矩阵，更新场景
### 计算新的 `eye` & `center`
- 计算沿着$\vec{right}$ & $\vec{up}$方向的移动的距离
- 获取窗口的宽高为 `w&h`
- 在投影矩阵中 $right - left = spanX$  &  $top - bottom = spanY$
```C++

offsetX / w * spanX = disX;
offsetY / h * spanY = disY;

eye += right * disX + up *  disY;
center += right * disX + up *  disY;
```

##  更新视图矩阵&`repaint`
- `ViewMat.lookAt(eye,center,up)`
- `repaint()`







