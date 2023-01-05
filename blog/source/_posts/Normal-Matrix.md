---
title: Normal Matrix
date: 2023-01-02 23:13:35
mathjax: true
categories:
  - Computer Graph
tags:
  - OpenGL
  - math
---


## 法线矩阵
- 在渲染管线的处理过程中，顶点处理阶段会通过模型变换将局部坐标变换到世界坐标,在模型变换的过程中，顶点会经过旋转、缩放、平移的过程.

- 法线是和方向相关的向量，所以平移对法向这个方向向量是没有意义的，所以我们考虑的是顶点的旋转与缩放对法线的影响，由于法线属于协变矢量(我们将在后面介绍这个概念)的一种，它不能和顶点使用相同的变换矩阵进行变换.

- 为了正确的变换法线，我们必须使用一个和顶点变换矩阵不同的矩阵.
  - 如果矩阵不包含缩放,无疑，它是合适的，可以安全的变换矩阵

    ![不包含缩放的变换](https://raw.githubusercontent.com/Ranbun/images/main/blog/computer%20graph/Normal_Matrix_1.png "不包含缩放的变换矩阵")

  - 如果矩阵包含均匀的缩放，它仍旧是安全的，依旧可以用来变换法线，唯一需要做的就是，就是变换完成后重新对法线进行归一化.

    ![均匀缩放](https://raw.githubusercontent.com/Ranbun/images/main/blog/computer%20graph/Normal_Matrix_2.png "均匀缩放矩阵")

  - 如果矩阵包含非均匀的缩放,我们需要一个新的矩阵处理变换后顶点的法相.

    ![非均匀缩放](https://raw.githubusercontent.com/Ranbun/images/main/blog/computer%20graph/Normal_Matrix_3.png "非均匀缩放矩阵")

### 1. 非均匀缩放的法线变换

- 通常情况下，我们在世界空间计算光照等

![Normal Matrix](https://raw.githubusercontent.com/Ranbun/images/main/blog/computer%20graph/Normal_Matrix_5.png
 "Normal Matrix")

- 推导：

- $\vec{T}$表示的是$\Delta_{ABC}$的切向量，由$A - C$计算得到.
  - $\vec{N} \bot \vec{T}$
- $\vec{T'}$表示的是$\Delta_{A'B'C'}$的切向量，由$A' - C'$计算得到.
  - 使用相同的变换矩阵之后，$\vec{N'}\cancel{\bot} \vec{T'}$

- 我们使用 $M$表示模型矩阵

  - $T = vec4(x,y,z,w)$

  $T \times M = (A - C) \times M$

  $T \times M = A \times M - C \times M$

  $T' = A' - C'$

  - $A' and C'$是三角形变换后的顶点，所以$T'$仍旧是三角形变换后的切线

- $M$不能用来变换法线，我们只能通过法线$\vec{N} \cdot \vec{T} = 0$的性质来完成下面的推导

  - 可得$\vec{N'} \cdot \vec{T'}$
  - 法线与切线垂直


- 法线是方向向量，平移对此没有意义，所以我们只考虑模型矩阵的左上角$3\times3$部分矩阵$M'$,并且变换后的法线 $\vec{N'}$ 满足$\vec{N'} \cdot \vec{T'} = 0$
- 在我们不知道一个合适的法线变换的矩阵的时候不妨设它$G$
- 此处我们不考虑平移

#### 推导：

​	$\vec{N'} \cdot \vec{T'} = (G_{3\times3} \times \vec{N}) \cdot ( M_{3\times3} \times \vec{T} ) = 0$

   $ (G_{3\times3} \times \vec{N}) \cdot ( M_{3\times3} \times \vec{T} ) = (G_{3\times3} \times \vec{N})^T \times (M_{3\times3} \times \vec{T}) $

   $\vec{N}^TG_{3\times3}^TM_{3\times3}\vec{T} = 0$ 

   $\vec{N}^T \times \vec{T} = \vec{N} \cdot \vec{T} = 0$

   $G_{3\times3}^TM_{3\times3} == I$ -- 单位矩阵

   $G_{3\times3}^T == I * M_{3\times3}^{-1}$

   $G_{3\times3} == I * (M_{3\times3}^{-1})^T = (M_{3\times3}^{-1})^T$

