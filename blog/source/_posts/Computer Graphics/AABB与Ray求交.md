---
title: AABB与Ray求交
date: 2022-09-14 15:49:17
mathjax: true
categories:
- Algorithm
- Computer Graphics
tags:
- AABB
- Intersect
---

光先追踪的加速方法就是引入层次结构数据结构，计算光线与层次结构的相交，从而加速光线与场景的求交。
<!--more-->

## AABB与Ray求交

- 在之前的部分我们说到了关于`OBB`与`Ray`求交的计算<a href="./../../08/OBB%E4%B8%8ERay%E6%B1%82%E4%BA%A4/">jump</a>,AABB算是比较特殊的`OBB`,计算`OBB`与`Ray`的方法可能不在适用于`AABB`与射线的求交计算，当然从原理上来说,这依旧是`Slabs Method`。

### 计算

#### 前置判断

- 考虑射线起点在盒子里面的情况
- 考虑射线和盒子某个面平行的时候

```C++
vec3 begin;// 射线的起点
vec3 u;    // 射线的方向
std::array<vec3,3> normals;  // 面的三个法线
vec3 min,max; // 盒子的边界点
// 计算是否平行
float del = 1e-6;
for(auto i = 0;i < 3;i++)
{
    if(u[i] < del)  // 近似为0  向量表示唯一 在这个方向上没有移动表示它和当前轴垂直
    {
        // 此时判断射线与盒子的关系
        // 在盒子内部必然相交
        if(begin[i] < min[i] || begin[i] > max[i])
        return false;
    }
}
return true;
```


#### 计算每个方向上对应的`slab`与射线的相交情况

- $P = P_0 + t \dots \vec{d}$
  - $\vec{d}$ - 射线的方向

```C++
float del = 1e-6;
vec3 begin;// 射线的起点
vec3 u;    // 射线的方向
std::array<vec3,3> normals;  // 面的三个法线
vec3 min,max; // 盒子的边界点

double tMin = DBL_MIN,tMax = DBL_MAX;  // 记录最后获取的结果


for(auto i = 0;i < 3;i++)
{
    const dis = 1.0 / u[i];
    float t_min;
    float t_max;
    if(dis >= del)
    {
        t_max = = (max[i] - begin[i]) * dis;
        t_min = = (min[i] - begin[i]) * dis;
    }
    else
    {
        t_max = = (min[i] - begin[i]) * dis;
        t_min = = (max[i] - begin[i]) * dis;
    }

    if(t_max < t_min)
    {
        swap(t_max,t_min);
    }
    if(tMin > t_min)
    {
        tMin = t_min;
    }
    if(tMax < t_max)
    {
        tMax = t_max;
    }
    if(tMin > tMax)
        return false;
}

return ture;

```

#### 判断

- 返回结果
