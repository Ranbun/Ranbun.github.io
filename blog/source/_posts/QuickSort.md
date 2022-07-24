---
title: QuickSort
date: 2021-10-28 22:28:59
categories:
- Algorithm
tags:
- cpp
- sort
---

## Quick Sort

#### 1、快速排序概述

- 快速排序也是采用分治的思想实现的，首先你有一组无序的数据，然后通过某种方法，取一个这组数据的中间数（M），然后对这组数据进行分组，大于M放到M的右边，小于M放到M的左边。然后通过相同的方法对这两组数分别做相同的处理，直到数据有序。

- 伪码实现

```C++ 
void QuickSort(ElementType A[],int N)
{
    if(N < 2)
    {
        return ; //数据有序 不需要排序
    }
    // 计算主元 -- 将数据分组
    pivot = {A|A中的一个元素}
    将A（S）分成两个独立的子集{A[] \ pivot}；
    A1 = {a∈S|a < pivot};
    A2 = {a∈S|a > pivot};
    // 递归调用QuickSort
}
```

#### 2、选主元

- 选择一个合适的主元是快速排序算法实现的关键，不能草率的取一个数作为主元。

- 下面介绍一种主元的选取方法：

  - 取头，中，尾的中位数

  - 伪码描述：
  ```C++
  ElementType Media3(ElementType A[],int left,int right)
  {
      int center = (left + right) / 2;
      
      // 如果坐标的比中间的大
      if(A[left] > A[center])
      {
          Swap(&A[left],&A[center]);  // 交换 左中的值 
          //此后 A[center] > A[left]
      }
      // 交换后的左 依旧大于右边
      if(A[left] > A[right])
      {
           Swap(&A[left],&A[right]); 
      }
      if(A[center] > A[right])
      {
           Swap(&A[center],&A[right]); 
      }
      // A[right] > A[center] 此时我们将center的元素放到此位置，可以只考虑 left+1 -- right-2的元素
      swap(&A[center],&A[right - 1]);
      
      return A[rigjt -1 ];  // 返回我们得到的主元 
  }
  ```

#### 3、子集划分

- 每次完成子集划分，主元会放到一个在全局中都正确的位置，将不会在改变。

#### 4、算法实现

```C++ 
void QuickSort(ElementType A[], int left,int right)
{
    if(Cutoff <= right - left)
    {
        // 主元
        poivt = Media3(A,left,right);
        int i = left;        // i 当前指向第一个元素的位置 
        int j = right - 1;   // J 指向主元的位置 
        for(;;)
        {
            while(A[++i] < poivt){}
            while(A[--j] > poivt){}
            if(i < j)
            {
                swap(&A[i],A[j]);
            }
            else
            {
                break;
            }
        }
        swap(&A[i],&A[right - 1]);
        // 递归 
        QuickSort(A,left,i-1);
        QuickSort(A,i,right);
    }
    else
    {
        Insert_selection(A+left,right - left + 1);
    }
    
}

```

