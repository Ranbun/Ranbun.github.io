---
title: OBB与Ray求交
date: 2022-09-08 17:49:42
mathjax: true
categories:
- Algorithm
- Computer Graphics
tags:
- OBB
- Intersect
---

光先追踪的加速方法就是引入层次结构数据结构，计算光线与层次结构的相交，从而加速光线与场景的求交。
<!--more-->

## OBB与Ray求交

- 我们在更之前的地方定义了一个`OBB`,往前翻一番也许就能找到
  - $center$   - 盒子的中心点
  - $\vec{u}$  - 盒子的某个法线 $u$
  - $\vec{v}$  - 盒子的某个法线 $v$
  - $\vec{w}$  - 盒子的某个法线 $w$
  - 盒子的尺寸 - $size\left(h_u,h_v,h_w \right )$

- 同样的 我们曾经定义过一条射线
  - $Ray \left( P\right ) = O + t \cdot \vec{u}$
  - `Ray(P)` 射线上某一点
  - `O` 射线的起点 
  - `t` 沿着方向$\vec{u}$前进的长度
  - $\vec{u}$ 射线的方向向量

## slab method

- 在OBB与光线的求交计算中，我们常用的方法是`slab method`
  - 将我们测量的这个盒子分成三组平行的板
  - 分别将光线与对应的板做相交计算，在射线的方向上，有一个进入这一组板的时间$t^{min}$，以及一个出板的时间$t^{max}$
  - 分别计算对于三个面板的$t^{min}_{i},t^{max}_{i},i \in \left(u,v,w\right)$
  - 如果射线与盒子相交，那么简单的说这条射线一定有一段时间是处于这三个$t^{min},t^{max}$之中的。

### 计算原理

![intersection](https://raw.githubusercontent.com/Ranbun/images/main/blog/intersection/Ray_and_Obb_intersect.png "Ray&OBB")

- $P_{min} = P + t_{min} \cdot \vec{u}$ - $1.0$
- $P_{max} = P + t_{max} \cdot \vec{u}$ - $1.1$
- $P_{i} = P + t \cdot \vec{u}$         - $1.2$

- $\left (P_{min} - C \right) \cdot \vec{o_u} = min $  - $1.3$
- $\left (P_{max} - C \right) \cdot \vec{o_u} = max $  - $1.4$

- $1.0$式 & $1.1$式 带入 $1.3$式 & $1.4$式得到：
$$
\left [ \left (P + t_i \cdot \vec{u} \right ) - C \right ] \cdot \vec{O_n} = O_{Size.j} / 2.0, i \in (min,max),j \in (x,y,z), n \in (u,v,w)
$$
=> 化简得:
$$
t_i = (O_{Size.i} / 2.0 - (C - P) \cdot \vec{O_n}) / (\vec{u} \cdot \vec{O_n}), i \in (min,max),j \in (x,y,z), n \in (u,v,w)
$$

### 求交实现

```C++
bool Intersect::IntersectObbWithRay(const OBB& obb, Ray & ray)
{
    auto delta = obb.m_center - ray.m_begin; // C- P
    float t1, t2;    // 当前面交点在射线上的位置
    double minT = DBL_MIN, maxT = DBL_MAX;

    {
        float e = glm::dot(delta, obb.m_u);
        float f = glm::dot(ray.m_dir, obb.m_u);
        if (std::abs(f) > 1e-20) // 判断当前面是否和射线平行 - 与法线垂直则会平行
        {
            t1 = (e + obb.m_size.x / 2.0) / f;
            t2 = (e - obb.m_size.x / 2.0) / f;
            if (t1 > t2)  // 交换 我们不知道当前的射线与目前检测的板的法相的方向是什么样的关系
            {
                auto temp = t1;
                t1 = t2;
                t2 = temp;
            }

            if (t1 > minT)
            {
                minT = t1;
            }
            if (t2 < maxT)
            {
                maxT = t2;
            }
            if (minT > maxT)   // 未发生相交
            {
                return false;
            }
            if (maxT < 0)  // 小于0表示不再正方向上，盒子在射线的后面
            {
                return false;
            }
        }
        // 此时盒子与光线平行，我们需要看看盒子和光线的关系，如果光线在盒子内部则相交
        else if (-e - obb.m_size.x / 2.0 > 0 || -e + obb.m_size.x / 2.0 < 0)
        {
            return false;
        }
    }

    {
        float e = glm::dot(delta, obb.m_v);
        float f = glm::dot(ray.m_dir, obb.m_v);
        if (std::abs(f) > 1e-20)
        {
            t1 = (e + obb.m_size.y / 2.0) / f;
            t2 = (e - obb.m_size.y / 2.0) / f;
            if (t1 > t2)
            {
                auto temp = t1;
                t1 = t2;
                t2 = temp;
            }

            if (t1 > minT)
            {
                minT = t1;
            }
            if (t2 < maxT)
            {
                maxT = t2;
            }
            if (minT > maxT)
            {
                return false;
            }
            if (maxT < 0)
            {
                return false;
            }
        }
        else if (-e - obb.m_size.y / 2.0 > 0 || -e + obb.m_size.y / 2.0 < 0)
        {
            return false;
        }
    }

    {
        float e = glm::dot(delta, obb.m_w);
        float f = glm::dot(ray.m_dir, obb.m_w);
        if (std::abs(f) > 1e-20)
        {
            t1 = (e + obb.m_size.z / 2.0) / f;
            t2 = (e - obb.m_size.z / 2.0) / f;
            if (t1 > t2)
            {
                auto temp = t1;
                t1 = t2;
                t2 = temp;
            }

            if (t1 > minT)
            {
                minT = t1;
            }
            if (t2 < maxT)
            {
                maxT = t2;
            }
            if (minT > maxT)
            {
                return false;
            }
            if (maxT < 0)
            {
                return false;
            }
        }
        else if (-e - obb.m_size.z / 2.0 > 0 || -e + obb.m_size.z / 2.0 < 0)
        {
            return false;
        }
    }

    return true;
}

```

