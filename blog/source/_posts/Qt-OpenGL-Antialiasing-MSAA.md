---
title: Qt OpenGL Antialiasing-MSAA
date: 2022-08-02 17:16:16
categories:
 - works
 - SCUU
 - Computer Graph
tags:
- OpenGL
- Qt
---

<p>
&ensp;&ensp;<code>QtOpenGL</code>MSAA算法介绍...
</p>

<!-- more -->

## Qt OpenGL Antialiasing - MSAA 

### 锯齿是图形绘制中常见的问题
- 这是一个采样不足然后信号丢失导致的问题
- 经过各位前辈们的其扑后继的研究，终究是有了看起来不错的解决方案
- 本文中我们采用一种名为`MSAA`的抗锯齿的技术

### `MSAA`

<p>
&ensp;&ensp;超级采样抗锯齿（Super Sampling Anti-Aliasing）的原理是把当前分辨率成倍提高,然后再把画缩放到当前的显示器上。这样的做法实际上就是在显示尺寸不变的情况提高分辨率，让单个像素变得极小，这样就能够大幅减轻画面的锯齿感了。不过是由于对整个显示画面的放大，因此它消耗的显示资源也是非常大的。
</p>

### `Qt OpengGL` 的 `MSAA`

#### 走样展示

![走样](https://raw.githubusercontent.com/Ranbun/images/main/blog/OpenGL/OpenGLAntialiasing.png "线的走样")


#### 开启`QtOpenGLWidget`的`MSAA`

![MSAA](https://raw.githubusercontent.com/Ranbun/images/main/blog/OpenGL/OpenGLAntialiasing-MSAA.png "MSAA")

#### 如何开启`QtOpenGL`的`MSAA`

- `Qt`有他自己的关于`OpenGL`的封装，我们使用的`Qt`的关于`OpenGL`封装
- 在创建`QOpenGLWidget`窗口时，在构造函数中添加如下代码：

    ```cpp
    QSurfaceFormat format;
    format.setSamples(4);
    setFormat(format);
    ```

    - `setFormat`是窗口`QOpenGLWidget`的函数，在构造函数中使用，会给后面所有依赖的小部件设置一个默认的`format`，对于后面的小部件(窗口)，如果不做格式的修改，将会使用在构造函数中调用`setFormat`函数设置的格式作为默认格式

#### 失败的尝试
<p>
    &ensp;&ensp;我尝试，单独修改<code>QopenGLContext</code>的<code>QSurfaceFormat</code>,前面的部分是成功，我获取了当前窗口的<code>QopenGLContext</code>,修改了<code>QSurfaceFormat</code>，向其中添加了
    启用<code>MSAA</code>的代码.然后调用<code>QopenGLContext</code>的<code>create</code>函数，企图重新创建一个<code>QopenGLContext</code>，但是失败了，在初始化<code>QopenGLFunction</code>失败，<p style="color:red">暂时不知道原因,目测可能需要重新将当前窗口的绑定到重新创建后的上下文，以及调用这个上下文的<code>OpenGL</code>函数
    </p>
</p>






