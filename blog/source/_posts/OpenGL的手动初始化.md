---
title: OpenGL的手动初始化
date: 2022-11-03 23:35:17
tags:
---

## OpenGL初始化

- 在使用OpenGL前我们通常需要配置好OpenGL的环境 - 创建一个提交命令的客户端，我们需要完成的事情包括如下部分(此处我们介绍关于Windows的操作)：
  - 创建一个窗口(for windows)
  - 获取窗口的`Device Context`
  - 使用`Device Context(HDC)`创建 `OpenGL Render Contesxt(HRLRC)`
  - 绑定`HDC & HGLRC`到当前线程
- 完成上述的设置之后，便可以调用`OpenGL`进行渲染


### 1、 创建`OpenGL`可用的窗口 - `Win`

#### 1.1 注册窗口类
- `HINSTANCE`对象实例化
- 填充`WNDCLASS`结构体
- 注册窗口类


#### 1.2 创建窗口


#### 1.3 设置像素格式

<font color=red>loading 待续</font>