---
title: OpenGL parallel  projection graphics do not scale with the window
date: 2022-08-04 10:05:58
categories:
- Computer Graph
- works
- SCUU
tags:
- OpenGL
---

<p>
&ensp;&ensp;<code>QtOpenGL</code>平行投影窗口缩放导致图形变形...
</p>

<!-- more -->

<center><h2>OpenGL parallel projection graphics do not scale with the window </h2> </center>

###  实现步骤 
- 重写窗口的`resize`事件 
  - 计算投影的范围
- 更新投影矩阵
- `drawcull`

#### `resizeGL`
- 我们最初计算一个比例： 
  - 关于窗口的`size`和投影的范围的
    - 使用`left, right`计算一个`spanx(right - left)`, `top, bottom`计算`spany(top - bottom)`, 分别与窗口的宽高比较
    ```C++
    auto ratioW = w / spanx;
    auto ratioH = h / spany; 
    // 取最小的一个 
    auto ratio = std::min(ratioW,ratioH);
    ```
  - 每次窗口缩放，我们就使用上述的范围计算新的`left,right,bottom,top`
    - 只要投影的范围没有发生改变我们便不计算新的`ratio`

- 计算新的`left,right,bottom,top`
```C++
void OpenGLWidget::resizeGL(int w, int h)
{
    glViewport(0,0,w,h);

    left = - w / ratio;
    right = abs(left);
    bottom = -h / ratio;
    top = abs(bottom);
}
```

#### 更新投影矩阵 
- update 
  - 关于`zNear & zFar`投影当二维平面时候，并未使用这两个参数
```c++
projMat.ortho(left,right,bottom,top,0.1f,100.0f);
```

#### 更新绘制的场景 


```C++

shaderProgram.bind(); 
shaderProgram->setUniformValue("projectionMatrix", projMat); 
glDrawArray()
......

```
















