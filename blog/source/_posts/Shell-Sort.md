---
title: Shell-Sort
date: 2021-10-26 20:25:46
categories:
- Algorithm
tags:
- cpp
- sort
---

###  希尔排序

- 希尔排序：预先定义一系列间隔(增量序列)DK，按照间隔，进行间隔排序
  - `DK[i-1]`有序后，执行`DK[i]`的排序后，`DK[i-1]`依旧是有序的

#### 1、原始的希尔排序

- `DK[i-1] = 取整(N/2),DK[i] = 取整(DK[i-1]/2)`

```C++
void Shell_sort(ElementTypes A[], int N)
{
    int D;  // 当前次序的间隔
    int P,i;
    ElementType temp;
    for(P=N/2;D>0;D/=2) // 间隔的变换
    {
        // 插入排序 
        for(P = D; P<N; P++)
        {
        	temp = A[P];  // 当前要插入的元素 
            for(i = P; i >= D && A[i-D] > temp;i -= D)  // 和已经有序的元素进行比较
            // 前一个元素A[i-D]如果大于temp，temp放置的位置在i-D之前，i-D位置的元素向后移动
            {
                A[i] = A[i-D];
            }
            A[i] = temp;  // temp 放到正确位置
        }
    }
}

```

- 复杂度分析： 当所有大于的增量的序列的数据都是有序的时候，所有的交换操作都会在增量为1的时候执行，这回导致一个不怎么好的结果，时间复杂度会达到`O(N^2)`。

