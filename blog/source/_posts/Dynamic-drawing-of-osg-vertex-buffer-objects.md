---
title: Dynamic drawing of osg vertex buffer objects
date: 2021-11-07 00:36:24
categories:
- works
- HJ
- OSG
tags:
- cpp
- osg
---

### `Dynamic drawing of osg vertex buffer objects`

- 使用`显示列表`绘制图形在速度上并没有`vertex buffer object`那么快，所以在更高的`osg`的版本中，推荐使用`vertex buffer object`

<!-- more -->

- 在使用`vertex buffer object`的情况下，动态更新绘制的数据

- 代码实现：
```C++
// 首先- 创建完成 osg::Geometry
osg::ref_ptr<osg::Geometry> geom = new osg::Geometry;
// 关闭显示列表 并使用vbo(vertex buffer object)
geom->setUseDisplayLists(false);
geom->setUseVertexBufferObject(true);
// 修改几何体的绘制数据的类型 
geom->setDataVariance(osg::Object::DataVariance::DYNAMIC);
//================================================================
// 更新操作 
// 先获取顶点数组 
auto varray = dynamic_cast<osg::Vec3Array>(geom->getVertexArray());
// 需改顶点数据
// ---------------------你可以想数组中插入顶点-----------------------------------
// 调用 dirty函数，告诉osg::Geometry你修改了顶点
varray->dirty();
// 修改 PrimitiveSet(OpenGL的DrawArray(图元，开始点，绘制个数))
// PrimitiveSet* osg::Geometry::getPrimitiveSet  ( unsigned int  pos ) 
auto pri_set = geom->getPrimitiveSet(0);
// void  setNumInstances (int n) 
pri_set->setNumInstances(修改后的顶点个数)；
pri_set->dirty();
// 更新几何体
geom->dirty();
```

