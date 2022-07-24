---
title: Games202-ShadowMap
date: 2021-12-13 22:25:25
categories:
- Computer Graph
tags:
- Games202
- Shadow
---

## Shadow Map

- 记录一下关于`Games202`的学习

#### shadow map

- 实现：通过比较当前的`point`,在光源空间的的深度与深度图中记录的深度的大小，决定当前的`point`是否在阴影中

- 如何获取阴影图：假设相机位置在光源的位置，以此渲染一张纹理，最终得到的这个纹理，每个像素只存储了深度信息，如下：

  ![](https://raw.githubusercontent.com/CuntBoy/images/main/blog/computer_graph/games202/games202depth_map.png)

上图来自`Games202`的课件：

- 阴影图`shadow map`记录了相机在光照位置下的最近最近深度。

- 关于计算当前vertex point 是否在阴影中的问题，还需要将顶点变换到光照空间也需要一个PVM矩阵(相机位置在光源位置-`lightPVM`)，用于将顶点转换到光空间。

- 在`Games202`的作业1中，我们需要使用提供的matrix的接口处理矩阵。

  ```javascript
  mat.translate() // 设置矩阵的移动 
  mat4.scale      // 设置矩阵的缩放
  
  mat4.lookAt(); // 生成的视图矩阵
  mat4.ortho();  // 平行投影
  
  mat4.multiply(); // 矩阵的乘法
  ```

  [gl-matrix文档](https://www.icode9.com/content-4-956693.html)

- 深度比较：

  ```glsl
  sampler2D shadowMap;   // 你的深度图
  mat4 lightPVM;         // 光空间的变换矩阵
  vec4 vertexPosition;   // 渲染的顶点坐标
  
  // 1： 将顶点变换到光空间 
  vec4 vertex_light = lightPVM * vertexPosition;
  // 2: 透视除法将顶点变换到NDC坐标
  vec3 ndc_pos = vertex_light.xyz / vertex_light.w;
  // 3: 将深度变到 0-1
  ndc_pos = ndc_pos * 0.5 + 0.5；
  // 4：获取深度图中记录的深度
  // 5: 获取当前点在光空间下的实际深度
  // 6: 比较两个深度的大小    
  // 7：返回当前点是否在阴影中    
  ```

  

- 最终效果

![](https://raw.githubusercontent.com/CuntBoy/images/main/blog/computer_graph/games202/shadow_map_result.png)
