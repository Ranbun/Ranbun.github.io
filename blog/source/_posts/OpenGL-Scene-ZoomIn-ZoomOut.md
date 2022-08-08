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
<center> <h1>OpenGL的缩放</h1></center>

## About
### 1. 直接缩放场景
#### 1. 实现思路
- 通过模型矩阵修改场景

### 2. 修改投影矩阵
#### 1. 实现思路
- 获取鼠标滚轮的变化(这个改变是带方向的 向上获取一个正值,向下获取的是一个负值)
- 将获取的这个值映射到和投影变换参数上
- 修改投影矩阵
- 重新绘制,你将获得缩放后的场景

<p style="color:red">
在我的实现中采用的是第二种方式
</p>

## 缩放的实现
### 1. 获取滚轮旋转角度
<p>
    我是用的是<code>Qt</code>的<code>OpenGL</code>,可以直接重写窗口的一些事件，比起使用<code>glfw</code>等方式，比起它们注册回调的方式可能相对方便一些。
</p>

#### Qt的滚轮事件
##### `QWheelEvent`
- 在出发滚轮事件之后，我们通过一个`QWheelEvent`对象获取我们需要的关于滚轮的信息

<div>

<p>
    &ensp;&ensp;Wheel events are sent to the widget under the mouse cursor, but if that widget does not handle the event they are sent to the focus widget. Wheel events are generated for both mouse wheels and trackpad scroll gestures. There are two ways to read the wheel event delta: <code>angleDelta()</code> returns the deltas in wheel degrees. These values are always provided. <code>pixelDelta()</code> returns the deltas in screen pixels.
</p>

<p>
    &ensp;&ensp; 上述部分来自<code>Qt</code>官方文档,我们有两种方法可以获取鼠标滚轮的增量(正负代表方向)，<code>angleDelta()</code>，<code>pixelDelta()</code>.
</p>

</div>

<h6> <code>QPoint QWheelEvent::angleDelta() const</code> </h6>

<p>
&ensp;&ensp;Returns the relative amount that the wheel was rotated, in eighths of a degree. A positive value indicates that the wheel was rotated forwards away from the user; a negative value indicates that the wheel was rotated backwards toward the user. angleDelta().y() provides the angle through which the common vertical mouse wheel was rotated since the previous event. angleDelta().x() provides the angle through which the horizontal mouse wheel was rotated, if the mouse has a horizontal wheel; otherwise it stays at zero. Some mice allow the user to tilt the wheel to perform horizontal scrolling, and some touchpads support a horizontal scrolling gesture; that will also appear in angleDelta().x().

&ensp;&ensp;Most mouse types work in steps of 15 degrees, in which case the delta value is a multiple of 120; i.e., 120 units * 1/8 = 15 degrees.
</p>

- 正值表示滚轮向上,远离用户. 负值向下,靠近用户.
- 此函数将返回两个方向上的滚动
    - <font color=red>我们此处使用是垂直方向的滚动的角度 </font>
- 滚轮每次触发滚动的的角度是$15^\circ\left(delta\right)$，但是实际的返回值是 $delta \times 8  = 120 $
- 返回的值应以 $ 1/8 $ 为单位:
    - $angleDelta() / 8$ 得到鼠标实际滚动的度数
- <code style="color:red">通过上面步骤便可以获取滚轮实际旋转的角度</code>

### 2. 将滚轮旋转角度映射到投影矩阵
- $ ratio = delta / 360^ \circ $
- 使用这个值去修改投影矩阵的参数 
- 我们使用的是平行投影的方式 
  - `left`
  - `rght`
  - `bottom`
  - `top`


### 3. 更新投影矩阵
- 在此之前 请将矩阵设置为单位矩阵 然后重新计算投影矩阵
- 更新投影矩阵
- 调用强制重新绘制(`Qt`) - `repaint()`


