---
title: Selection_sort
date: 2021-10-25 22:25:09
categories:
- Algorithm
tags:
- cpp
- sort
---

## 选择排序  

- 选择排序，每次从无序的数据中选择一个`最大的`或者是`最小的`,放到合适的位置。

- 代码实现：

    ```C++ 
    // 这里为了使用最大堆-我们倒着排序 
    void Selection_Sort(ElementType A[], int N)
    {  
        for(auto i = N-1;i >= 0；i--)
        {
        	  max_postion = scanForMax(A,0,i); //获取最大元素的下标 
              swap(&A[i],&A[max_position]);    // 交换元素    
        }
    }
    ```

- `scanForMax`
  - 查找当前无序元素中最大的元素，并返回元素的下标
    - 常规下，直接对数组元素进行扫描，时间复杂度`O(N)`,整个算法的时间复杂度`T=O(N^2)`
    - 优化方案：如何快速找到最大值 -- 最大堆or最小堆。

