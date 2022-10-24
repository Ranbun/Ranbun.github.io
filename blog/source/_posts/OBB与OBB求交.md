---
title: OBB与OBB求交
date: 2022-09-08 17:28:20
mathjax: true
categories:
- Algorithm
- Computer Graphics
tags:
- OBB
- Intersect
---

OBB 全称为 Oriented Bound Box，译为有朝向的包围盒。OBB 常与 AABB(Axis-Aligned Bound Box) 对比：AABB 的边与轴平行，而 OBB 的边则与物体的朝向有关。

<!--more-->

### OBB求交介绍

- 采用分离轴的方式计算
- 两个凸包多边形，当且仅当存在一条线，这两个多边形在这条线上的投影不相交，则这两个多边形也不相交.
- 这条线称为`Separating Axis`.垂直`Separating Axis`存在一条`Separating Line`将两个多边形分开。

#### OBB to AABB

- 我们采用一个稍微容易理解的方法完成这种计算 - 将`OBB`转换为`AABB`
- 然后使用`AABB`o 求交的计算方式去算`OBB`的相交

### `AABB`的定义

- `AABB`既是`Axis-aligned Bounding Box`

<center>
<img src=https://raw.githubusercontent.com/Ranbun/images/main/blog/intersection/define_aabb.png></img>
</center>

- 简单由定义两个最大最小点完成定义，一个`OBB`也可以描述一个`AABB`

### OBB的定义

- `m_center` 中点位置
- `size`     盒子的大小(尺寸)
- $\vec{u},\vec{v},\vec{w}$, 坐标系的轴向(与面的法相一样)

```Cpp
struct OBB
///< 右手系
{
    OBB(const vec3 & pos,const vec3 & size)
        : m_pos(pos)
        , m_size(size)
    {
    }

    OBB(const vec3& pos, const vec3& size, const vec3& u, const vec3& v, const vec3& w)
        : m_pos(pos)
        , m_size(size) 
        , m_u(u)
        , m_v(v)
        , m_w(w)
    {
    }

    vec3 m_pos{0,0,0};   ///< pos 
    vec3 m_size{ 0.5,0.5,0.5 };  ///< 长宽高 x = 长 y = 宽 z = 高
    vec3 m_u{1,0,0};     ///< x
    vec3 m_v{0,1,0};     ///< y 
    vec3 m_w{0,0,1};     ///< z 
};

```

### `AABB`求交

- 根据分离轴定理，对于`AABB`我们只需要计算最大最小的点在标准的${\vec{x},\vec{y},\vec{z}}$轴上的投影是否相交即可，实现起来比较简单

#### code

```C++
bool intersectObbWithObb(OBB &first, OBB &second)
{
    auto fRes  = computerOBBIntersecte(first,second);
    auto sRes = computerOBBIntersecte(second,first);
    return (fRes&sRes);
}

bool computerOBBIntersecte(OBB first, OBB second)
{
    /// first
    first.m_trans = glm::translate(glm::mat4(1.0f),-first.m_center);
    auto rotate = glm::mat4(glm::vec4(first.m_u, 0.0), glm::vec4(first.m_v, 0.0), glm::vec4(first.m_w, 0.0), glm::vec4(0, 0, 0, 1.0));
    rotate = glm::transpose(rotate);
    first.m_rotate = rotate;

    // 将第一个matrix变换到第一个变换矩阵描述的空间
    first.m_center = first.m_rotate * first.m_trans * glm::vec4(first.m_center,1.0);
    first.m_u = first.m_rotate * first.m_trans * glm::vec4(first.m_u,0.0);
    first.m_v = first.m_rotate * first.m_trans * glm::vec4(first.m_v,0.0);
    first.m_w = first.m_rotate * first.m_trans * glm::vec4(first.m_w,0.0);

    // 将 second 变换到对应的空间中
    auto trans2Origin = glm::translate(glm::mat4(1.0),-second.m_center);
    second.m_trans = trans2Origin;
    auto transRestore = glm::translate(glm::mat4(1.0),second.m_center);
    second.m_center = first.m_trans * transRestore * first.m_rotate * second.m_trans * glm::vec4(second.m_center,1.0);
    second.m_u = first.m_trans * transRestore * first.m_rotate * second.m_trans * glm::vec4(second.m_u,0.0);
    second.m_v = first.m_trans * transRestore * first.m_rotate * second.m_trans * glm::vec4(second.m_v,0.0);
    second.m_w = first.m_trans * transRestore * first.m_rotate * second.m_trans * glm::vec4(second.m_w,0.0);

    /// first Obb max & min position
    glm::vec3 fMinPos = first.m_center - first.m_size.x/2.0f * first.m_u - first.m_size.y / 2.0f * first.m_v - first.m_size.z/2.0f * first.m_w;
    glm::vec3 fMaxPos = first.m_center + first.m_size.x/2.0f * first.m_u + first.m_size.y / 2.0f * first.m_v + first.m_size.z/2.0f * first.m_w;

    /// 计算second OBB 的最大最小点
    std::array<glm::vec3,8> secondPos;
    {
        /// 右手系
        // m_center + vector * size
        secondPos[0] = second.m_center - second.m_u * second.m_size.x * 0.5f -
                second.m_v * second.m_size.y * 0.5f -  second.m_w * second.m_size.z * 0.5f;

        secondPos[1] = second.m_center - second.m_u * second.m_size.x * 0.5f -
                       second.m_v * second.m_size.y * 0.5f +  second.m_w * second.m_size.z * 0.5f;

        secondPos[2] = second.m_center - second.m_u * second.m_size.x * 0.5f +
                       second.m_v * second.m_size.y * 0.5f - second.m_w * second.m_size.z * 0.5f;

        secondPos[3] = second.m_center - second.m_u * second.m_size.x * 0.5f +
                       second.m_v * second.m_size.y * 0.5f +  second.m_w * second.m_size.z * 0.5f;

        secondPos[4] = second.m_center + second.m_u * second.m_size.x * 0.5f -
                       second.m_v * second.m_size.y * 0.5f -  second.m_w * second.m_size.z * 0.5f;

        secondPos[5] = second.m_center + second.m_u * second.m_size.x * 0.5f -
                       second.m_v * second.m_size.y * 0.5f +  second.m_w * second.m_size.z * 0.5f;

        secondPos[6] = second.m_center + second.m_u * second.m_size.x * 0.5f +
                       second.m_v * second.m_size.y * 0.5f - second.m_w * second.m_size.z * 0.5f;

        secondPos[7] = second.m_center + second.m_u * second.m_size.x * 0.5f +
                       second.m_v * second.m_size.y * 0.5f +  second.m_w * second.m_size.z * 0.5f;
    }

    glm::vec3 sMinPos{secondPos[0]};
    glm::vec3 sMaxPos{secondPos[0]};

    for(auto & it : secondPos)
    {
        sMinPos.x = sMinPos.x > it.x? it.x:sMinPos.x;
        sMinPos.y = sMinPos.y > it.y? it.y:sMinPos.y;
        sMinPos.z = sMinPos.z > it.z? it.z:sMinPos.z;

        sMaxPos.x = sMaxPos.x < it.x? it.x:sMaxPos.x;
        sMaxPos.y = sMaxPos.y < it.y? it.y:sMaxPos.y;
        sMaxPos.z = sMaxPos.z < it.z? it.z:sMaxPos.z;
    }

    /// 计算
    if(
        (fMinPos.x > sMaxPos.x || sMinPos.x > fMaxPos.x) ||
        (fMinPos.y > sMaxPos.y || sMinPos.y > fMaxPos.y) ||
        (fMinPos.z > sMaxPos.z || sMinPos.z > fMaxPos.z)
    )
    {
        return false;
    }

    return true;
}
```

- 当然也可以直接使用其他的方式计算`OBB`的求交，分离轴使用相对较多，可以直接投影`OBB`的最大最小的点到由`OBB`确定的投影轴上,包含15跟轴
- `OBB A`的三个轴向,`OBB B`的三个轴向 6 根。
- 两个`OBB`各自轴向的叉积 => 3*3 = 9。
