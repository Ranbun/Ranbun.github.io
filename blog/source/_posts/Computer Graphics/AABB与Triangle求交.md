---
title: AABB与Triangle求交
date: 2022-09-08 17:49:59
mathjax: true
categories:
- Algorithm
- Computer Graphics
tags:
- AABB
- Intersect

---

`AABB` 与三角形求交，采用分离轴的方式实现的一种比较优质的方法。
<!--more-->

## 0. 预备知识

### 点到面的距离方程

### 面的方程

- 定义平面`A`的法向 $\vec{n}$ `{a,b,c}`,
- 已知平面行一点$P_0 \lbrace x_0,y_0,z_0\rbrace$
- 任意点 $P$`{x,y,z}`
- 面方程:
  - $\overrightarrow{P - P_0} \cdot  \vec{n} = 0$

### 点到面的距离

#### 法向量法

- $\vec{n}$  - 平面的法向量
- $d$ - 点到平面的距离
- $P$ - 平面外一点
- $A$ - 平面上一点
- $d = \vec{n} \cdot \vec{PA} $
  - PA在法向方向上的投影既是点$P$到平面的距离

### 射线的方程

- 射线方程
  - $O$ 为起点， 沿着方向 $\vec{d}$
  - 射线方向上任意一点$P_0$
  - $P_0 = O + \vec{d}$

## 三角形与AABB求交

- 我们定义一个轴对齐包围盒
  - center $c$, `AABB`的中心点
  - a half vector $\vec{h}$, 记录盒子的各个轴的方向与在轴所在方向大小

- 一个三角形
  - $\Delta u_0u_1u_2$

<img id=AABB&Trangle src=https://raw.githubusercontent.com/Ranbun/images/main/blog/intersection/AABB_and_trangle_intersect.png></img>

### 1. 第一步 

- 移动`AABB`与三角形，使`AABB`与原点重合

### 2. 计算测试的轴

- 我们将在原点进行基于分离轴的相交测试，需要测试13根轴。

#### 2.1 `AABB`的面的法线

- $\vec{e_0}(1,0,0)$
- $\vec{e_1}(0,1,0)$
- $\vec{e_2}(0,0,1)$

#### 2.2 三角形$\Delta u_0u_1u_2$的法线

- $\vec{n}$
- $\vec{n} == \vec{f_0} \times \vec{f_1}$

#### 2.3 $a_{ij} = e_i \times f_j$

- $i,j \in \left ( 0,1,2 \right )$
- $\vec{f_0} = \vec{v_1} - \vec{v_0}$
- $\vec{f_1} = \vec{v_2} - \vec{v_1}$
- $\vec{f_0} = \vec{v_0} - \vec{v_2}$

### 3 分离轴计算

- 一旦找到分离轴算法就会立即停止并且返回一个不相交的结果
- 如果通过所有的测试并且没有找到分离轴，那么三角形与`AABB box`相交

- 将三角形的顶点投影到每一个分离轴上，然后计算`AABB`在分离轴上的范围,`AABB`被移动到原点，它的投影将会是一个$\left [ -r,+r\right ]$对称的
  - 如果她们在这个分离轴上重合，那么它们投影后的结果也是重合的
- 只有当所有的分离轴上的测试都通过的时候才能是相交的。

## Code 

### define AABB

```C++
struct AABB
{
    AABB() = default;

    explicit AABB(const glm::vec3 & center,const glm::vec3 & size)
        : m_center(center)
        , m_size(size)
    {
    }

    void updateTransfrom(const glm::mat4 & model)
    {
        m_center = glm::vec3( model * glm::vec4(m_center,1.0));
    }

    glm::vec3 m_center;
    glm::vec3 m_size;
};

```

### define Triangle

```C++
struct Triangle
{
    /// 注意三角形的顶点顺序为逆时针
    Triangle(const glm::vec3& p1, const glm::vec3& p2, const glm::vec3& p3, const glm::vec3& normal)
        : m_p1(p1)
        , m_p2(p2)
        , m_p3(p3)
        , m_normal(glm::normalize(normal))
    {


    }

    Triangle(const glm::vec3& p1, const glm::vec3& p2, const glm::vec3& p3)
        : m_p1(p1)
        , m_p2(p2)
        , m_p3(p3)
    {
        /// 计算三角形的法相

        /*
                p2

          p3           p1

        */

        auto v1 = p3 - p1;
        auto v2 = p2 - p1;
        m_normal = glm::normalize(glm::cross(v2, v1));

    }

    void updateTransform(glm::mat4 & model)
    {
        m_p1 = vec3(model * glm::vec4(m_p1,1.0));
        m_p2 = vec3(model * glm::vec4(m_p2,1.0));
        m_p3 = vec3(model * glm::vec4(m_p3,1.0));

        auto v1 = m_p3 - m_p1;
        auto v2 = m_p2 - m_p1;
        m_normal = glm::normalize(glm::cross(v2, v1));
    }

    glm::vec3 m_p1;
    glm::vec3 m_p2;
    glm::vec3 m_p3;
    glm::vec3 m_normal;

};

```

### 实现

```C++
// done 
bool IntersectAABBWithTriangle(const AABB &aabb, const Triangle &triangle)
{
    // move AABB ro  origin
    auto box = aabb;
    auto trans = glm::mat4(1.0);
    trans = glm::translate(trans,- box.m_center);
    box.updateTransfrom(trans);

    auto tri = triangle;
    tri.updateTransform(trans);

    std::array<glm::vec3,13> projectAxis;

    std::array<glm::vec3,3> aabbAxisVector;
    {
        aabbAxisVector[0] = glm::vec3(1.0f, 0.0, 0.0);
        aabbAxisVector[1] = glm::vec3(0.0f, 1.0, 0.0);
        aabbAxisVector[2] = glm::vec3(0.0f, 0.0, 1.0);
    }

    std::array<glm::vec3,3> triEdgeVector;
    {
        triEdgeVector[0] = tri.m_p2 - tri.m_p1;
        triEdgeVector[1] = tri.m_p3 - tri.m_p2;
        triEdgeVector[2] = tri.m_p1 - tri.m_p3;
    }

    auto triangleNormal = tri.m_normal;

    std::array<glm::vec3,3> aabbFaceNormal;
    {
        aabbFaceNormal[0] = glm::vec3(1.0f, 0.0, 0.0);
        aabbFaceNormal[1] = glm::vec3(0.0f, 1.0, 0.0);
        aabbFaceNormal[2] = glm::vec3(0.0f, 0.0, 1.0);
    }

    // 两个图形边的随机组合的叉积也作为一个分离轴
    for(const auto & i: {0,1,2})
    {  /// AABB
        for(const auto & j: {0,1,2})
        { /// triangle
            projectAxis[i*3 + j] = glm::normalize(glm::cross(aabbAxisVector[i], triEdgeVector[j]));
        }
    }

    // 三角形的法线
    projectAxis[9] = triangleNormal;
    for(const auto & index: {0,1,2})
    {
        projectAxis[10 + index] = aabbFaceNormal[index];
    }

    // 计算过程
    for(const auto & it: projectAxis)
    {
        // 投影三角形到分离轴上
        auto p_0 = glm::dot(it,tri.m_p1);
        auto p_1 = glm::dot(it,tri.m_p2);
        auto p_2 = glm::dot(it,tri.m_p3);

        // 计算AABB的Size
        const auto half = glm::vec3(0.5 * aabb.m_size.x,0.5 * aabb.m_size.y,0.5 * aabb.m_size.z);

        // 计算AABB的投影的结果的范围
        auto r = half.x * std::abs(dot(aabbAxisVector[0],it)) +
                half.y * std::abs(dot(aabbAxisVector[1],it)) +
                half.z * std::abs(dot(aabbAxisVector[2],it));

        // 计算三角形投影结果的范围
        auto min_p = std::min(std::min(p_0,p_1),p_2);
        auto max_p = std::max(std::max(p_0,p_1),p_2);

        // 判断是否相交
        if(min_p > r || max_p < -r)
            return false;
    }

    return true;
}
```
