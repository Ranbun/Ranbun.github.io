---
title: Heap
date: 2021-10-24 15:55:06
categories:
- Algorithm
tags:
- cpp
- sort
---

<p>
&ensp;&ensp;<code>Heap</code>数据结构介绍...
</p>

<!-- more -->

## 堆

#### 优先队列(`Pripority Queue`)
- 特殊的 "队列",取出元素的顺序是按照元素的优先级大小，而不是进入队列的先后顺序。
- 使用数组构建：
  - 插入：
   &ensp;&ensp;&ensp;&ensp;&ensp;总是插入数组的尾部`T = O(1)`
  - 删除：
	 &ensp;&ensp;&ensp;&ensp;&ensp;查找到最大的(最小的元素)：`T=O(N)`
	  &ensp;&ensp;&ensp;&ensp;&ensp;从数组中删除元素，需要将元素移动位置：`T=O(N)`
- 链表构建：
    - 插入：
   &ensp;&ensp;&ensp;&ensp;&ensp;总是插入链表的头部`T = O(1)`
  - 删除：
	 &ensp;&ensp;&ensp;&ensp;&ensp;查找到最大的(最小的元素)：`T=O(N)`
	  &ensp;&ensp;&ensp;&ensp;&ensp;删除元素：`T=O(1)`
- 有序数组：
    - 插入：
   &ensp;&ensp;&ensp;&ensp;找到合适的位置:`T = O(N) or O(log2(N))`
   &ensp;&ensp;&ensp;&ensp;移动元素并插入`T=O(N)`
  - 删除：
	 &ensp;&ensp;&ensp;&ensp;删除最后一个元素：`T=O(1)`
- 有序链表：
    - 插入：
   &ensp;&ensp;&ensp;&ensp;找到合适的位置:`T = O(N)`
   &ensp;&ensp;&ensp;&ensp;插入`T=O(1)`
  - 删除：
	 &ensp;&ensp;&ensp;&ensp;删除最后一个元素(或者首元素)：`T=O(1)`

#### 最大堆 - 完全二叉树（大顶堆）

- 根元素是当前树中最大的

##### 1、堆的创建 -- 创建空堆

```C++
typedef strut HeapStruct * MaxHeap;
struct HeadStruct{
    ElementType * Elements;   // 存储元素的数组 
    int Size;                 // 当前元素个数
    int Capacity;             // 堆的最大容量
}

// 创建堆
MaxHeap create(int MaxSize)
{
    MaxHeap H = malloc(sizeof(struct HeapStruct));
    H->Elements = malloc((MaxSize+1) * sizeof(ElementType));
    H->size = 0;
    H->Capactity = MaxSize;
    H->Elements[0] = MaxData;  // O号位置存储的是哨兵 不是个元素 
	return H;
}

```



##### 2，堆的插入

```C++ 
void Insert(MaxHeap H,ElementType item)
{
    int i;
    if(isFull(H))
    {
        std::cout<<"最大堆已经满了"<<std::endl;
    }
    i = ++H->Size;  // i等于 插入元素后的堆中的最后一个元素的下标
    // 交换节点的位置 
    for(;H->Elements[i/2] < item; i = i/2) // 如果I对应的那个插入元素 大于其父节点的数据 交换两个元素的值
    {
        H->Elements[i] = H->Elements[i/2];  // 覆盖结果
    }
    H->Elements[i] = item;
}

```

##### 3、堆的删除

```C++ 
// 取出根节点
// 将最后一个元素放到根节点(保留树的特性)
// 修改使其具有最大（最小）堆的特性

EleementType deleteMax(MaxHeap H)
{
    int parent,child;
    ElementType MaxChild,temp;
    if(IsEmpty(H))
    {
        std::cout<<"堆为空"<<std::endl;
        return ;
    }
    MaxItem = H->Elements[1]; // 取出根的最大值 
    // 取出堆中最后一个元素， 并将Size - 1
    temp = H->Elements[H->Size --];
    // 调整数据的大小关系 
    for(parent = 1; parent*2 < H->Size;parent = child)  // 是不是存在左孩子 
    {
        child = parent * 2;
        // 比较左右孩子的大小
        if((child != H->Size)) && (H->Elements[child] < H->Elements[child+1]))  // 当前child不是最后一个元素 
        {
            child++; 
        }
        // 不满足交换条件 - 结束循环
        if(temp > H->Elements[child])
        {
            break;
        }
        else
        {
            // 交换元素 
            H->Elements[parent] = H->Elements[child];
            
        }
    }
    // 循环结束代表找到temp元素的合适安置位置 -- 放到合适的位置即可
    H->Elements[parent] = temp;
    return MaxItem;
}

```

##### 4、最大堆的建立

&ensp;&ensp;建立最大堆：将已经存在的N个元素按最大堆的要求存放在一个一维数组中

&ensp;&ensp;1、通过插入操作，将N个元素一个一个的插入到空堆中去：`T = O(NlogN)`

&ensp;&ensp;2、线性时间复杂度下建立最大堆

​	&ensp;&ensp;1、将元素安顺序输入，先构建完全二叉树（下标为1开始）

&ensp;	&ensp;2、调整元素位置，使其满足最大堆

<a href=https://github.com/CuntBoy/Sort_Algorithm/tree/main/heap style="color:red">Heap C++ 实现  </a>

#### 最小堆- 完全二叉树（小顶堆）

- 可以参照最大堆写

