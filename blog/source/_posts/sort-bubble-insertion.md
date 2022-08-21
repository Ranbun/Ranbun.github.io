---
title: 简单排序-冒泡&插入
date: 2021-10-19 23:41:14
categories:
- Algorithm
tags:
- cpp
- sort
---

<p>
&ensp;&ensp;简单排序算法...
</p>

<!-- more -->

## 简单排序:

- 冒泡排序
- 插入排序

上述两种排序是相对基础的简单排序,通过交换逆序对逐渐使序列有序。

在常规的排序算法中：我们默认的顺序是从小到大

#### 1、冒泡排序

&ensp;&ensp;原理： 冒泡排序是一个两重的`for`循环，外面一层每循环一次就代表有一个元素被放好，第二重循环做的事情是从没有排好序的元素中取出一个，然后依次和后面的元素作比较，将这些元素中最大的一个放到这些无序元素的最后一个元素的位置，标志着这个元素完成排序——变得有序。

- C++实现 -- 初始版本

```c++
void swap(int * a,int * b)；
void bubble_Sort(ElementTYpe A[],int N)  // A是待排序的数据 N代表的是数据的个数
{
    int p = 0,i = 0;
  	for(p = N - 1;p>=0;p--)
    {
        for(i = 0;i < p;i++) // 从无序的数据中抛出最大的
        {
            if(A[i] > A[i+1])  // 比较元素
            {
                swap(A+i,(A+(i+1))); // 交换元素 
            }
        }
    }
}

void swap(int * a,int * b)
{
    *a ^= *b;
    *b ^= *a;
    *a ^= *b;
}
```

- 优化版本

如是在某一次循环中，提前让数组有序，但是在上述的代码中，即使数据有序了，循环依旧会继续执行，我们需要一个标志，来知道数据已经有序：

```C++
void bubble_Sort(ElementTYpe A[],int N)  // A是待排序的数据 N代表的是数据的个数
{
    int p = 0,i = 0;
    bool flag = false;       // 标识数组是否有序
  	for(p = N - 1;p>=0;p--)
    {
        for(i = 0;i < p;i++) // 从无序的数据中抛出最大的
        {
            if(A[i] > A[i+1])  // 比较元素
            {
                swap(A+i,(A+(i+1))); // 交换元素
                flag = true;
            }
        }
        if(!flag)  // 满足退出条件 结束循环
        {
            break; 
        }
    }
}
```
- 结果展示
  - 输入与数据：`int a[7] = {3,1,2,98,30,8,90};`

![](https://raw.githubusercontent.com/CuntBoy/images/main/blog/sort/bubble_sort.png)

- 复杂度分析 - 优化版本

最好的情况：默认传输进来的元素是有序的，我们只需要，经历一次数据的遍历`flag`变量会触发循环结束，复杂度：

<center>T = O(N)</center>

最坏情况：元素逆序，需要遍历比较所有的元素

<center>T = O(N^2)</center>

#### 2、插入排序

插入排序：不发生元素的交换，但是会发生元素的移动，每次获取一个元素(P)，将这个P插入到一个有序的序列中，插入的过程是一个比较+覆盖的过程。

- C++实现 

  ```C++
  void Insertion_Sort(ElementType A[],int N)
  {
      int p,i，tmp;
      for(p = 1;p < N;p++)
      {
          tmp = A[p];  // 获取插入的元素 
          for(i = p;i > 0 && A[i - 1] > tmp;i--) // A[i-1]表示的是有序序列中的最大元素
          {
              A[i] = A[i-1];
          }
          // 比较结束后，当前 i - 1指向位置的元素是小于 tmp的，所以：
          A[i] = tmp;
      }
  }
  ```

- 结果展示

  - 输入与数据：`int a[7] = {3,1,1,98,30,8,90};`

![](https://raw.githubusercontent.com/CuntBoy/images/main/blog/sort/insertion_sort.png)

- 复杂度分析：

  最好情况，每次进来的元素不需要，比较。直接插入到相应的位置，此时时间复杂度：

  <center>T = O(N)</center>

  最坏情况，数据逆序，每一次的元素都需要遍历所有的有序元素，此时时间复杂度：

  <center>T = O(N^2)</center>

#### 补充说明：

- 时间复杂度下界

&ensp;&ensp;&ensp;&ensp;概念：对于下边`i<j`如果A[i] > A[j]，则称`(i,j)`是一对逆序对<font color=red>逆序对(inversion)</font>

- 在上述的排序中，相邻元素的交换其实是消掉逆序对的操作！<font color=red>如果序列基本有序，那么插入排序简单且高效</font>

<center>插入排序：T(N,I) = O(N+I)</center>

- 对于N个不同元素组成的序列，平均具有`N(N-1)/4`个逆序对。

- 对于仅以交换相邻元素来排序的算法，其平均时间复杂度为婐`Ω(N^2)`

基于上面的描述，我们要提高此类算法的效率，必须每次消去的逆序对数大于1。

### 下面将会介绍，希尔排序！

