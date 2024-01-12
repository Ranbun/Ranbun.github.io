---
title: ColorBlend
date: 2024-01-12 15:33:25
categories:
  - Computer Graph
  - works
tags:
  
---

## 颜色混合

在传统的渲染管线中，如果场景中的所有物体都不透明，那么最终渲染的结果是根据`Z-buffer`算法比较深度，显示的片元的颜色是离相机的最近的片元的颜色。

如果场景中存在半透明物体(颜色的`alpha`值小于`1.0`)的时候，比如模拟渲染一个透过玻璃观察的场景，正确的场景是我们可以透过玻璃观察到玻璃后面的世界，要正确的渲染这个场景，需要将玻璃的颜色和玻璃后面的场景进行颜色的混合，混合既是混合多种颜色为一种，从而渲染我们认为正确的结果。

透明物体可以半透明也可以是全透明的，全透明下光线会完全穿透，是指这个透明物体的颜色不参与整个混合，也就是透明物体本身的颜色不对混合的结果有任何的贡献，除去全透明物体剩下的就是半透明物体，半透明物体本身的颜色参与最终颜色的合并，会对生成的结果产生一定的贡献，具体的数量要根据混合算法计算。

!["blending transparency"][blending_transparency]



在`OpenGL`中颜色混合一般都是使用`ADD`的方式,使用片元的`alpha`作为混合的因子。

- `ADD`既是取这两个需要合并的像素，根据因子分别计算一个值$C_{src},C_{dst}$，输出最终的颜色$C_{out} = C_{src} + C_{dst}$，可以分别为`RGB`通道和`alpha`通道分别设置混合因子

- `ADD`被称为混合混合方程，可以分别为`RGB`通道和`alpha`通道分别设置混合方程

因为在渲染过程中`画家&z-buffer`算法的实现方式，当我们有一个半透明的场景，并且我们想要正确显示场景中的物体的颜色，我们必须对场景中的物体进行排序,保证优先渲染最远的物体，此时我们通常可以得到一个正确的渲染结果。



[blending_transparency]: https://raw.githubusercontent.com/Ranbun/images/main/blog/computer%20graph/transparent/blending_transparency.png	"blending transparency"



