---
title: Bit manipulation
date: 2022-07-29 17:24:35
categories:
- Coding
tags:
- cpp
---
### 将二进制数某一位置0，置1，取反

- 用位运算可以解决我们的问题,所以下面部分的代码就是一些位运算的简单应用

<!-- more -->


- 函数实现 


#### 1. 将不同进制数转为二进制(非必须) - 我们只是为了显示结果
- 模拟人的计算过程
    ```cpp
    static void showBinary(int d)
    {
        using std::cout;
        using std::endl;
        using std::array;

        // 存储具体对应的位的值 0 / 1
        array<int, 32> num;
        num.fill(0);
        int flag = 1; // 当前转换的Bit位
        for (int i = 31; i >= 0; i--)
        {
            if (d & flag)
            {
                num[i] = 1;
            }
            else
            {
                num[i] = 0;
            }

            flag = flag << 1;
        }

        // 查找第一个不是零的数 
        int index = 0;
        #if 1 
        // 此过程可以省略 做了个显示上的优化
            for (auto i = 0; i < num.size(); i++)
            {
                if (num[i] != 0)
                {

                    i == 0 ? index = i : index = i - 1;
                    break;
                }
            }
        #endif 

        // 输出转换结果
        for (auto i = index; i < num.size(); i++)
        {
            cout << num[i];

        }
        cout << endl;
    }

    ```

#### 某一位置为 0
- `&`运算的应用
    ```cpp
        /// 某一位设置为 0
        /// d 你要修改的数 
        /// bits 你要修改的位置
        template<typename T>
        T setBit2Zero(T d, int bits)
        {
            BitManipulate::showBinary(static_cast<int>(d));
            // 取反是一个只和取反位数以及之后所有位数相关的操作
            // 二进制的位数从0开始计算 所以需要 bits - 1 
            // 1 << (bits(3) - 1) = 4 = 0100 
            // ~4 = 1011
            // & 同为 1 则为 1 => 1011 & (任意数字) 第三位等于 0
            d = d & (~(1 << (bits - 1)));
            BitManipulate::showBinary(static_cast<int>(d));
            return d;
        }
    ```

#### 某一位置为 1
- `|` 运算的应用, 只要有一个是1 结果都是1
    ```cpp
        /// 某一位设置为 1
        /// d 你要修改的数 
        /// bits 你要修改的位置
        template<typename T>
        T setBit2One(T d, int bits)
        {
            BitManipulate::showBinary(static_cast<int>(d));
            // d(16) = 10000 bits = 3
            // 1 << bits - 1 == 4 = 0100
            // d | 0100 = 10000 | 0100  = 10100 
            d = d | (1 << (bits - 1));
            BitManipulate::showBinary(static_cast<int>(d));
            return d;
        }
    ```

#### 某一位置取反
- `^` 异或运算的运用
    - `^` 异或运算 1 ^ 0 == 1 / 0 ^ 1 == 1  其余的情况都是 0

    ```cpp
        /// 某一位取反
        /// d 你要修改的数 
        /// bits 你要修改的位置
        template<typename T>
        T setBit2Negate(T d, int bits)
        {
            BitManipulate::showBinary(static_cast<int>(d));
            // 转换过程
            // ^ 异或运算 1 ^ 0 == 1 / 0 ^ 1 == 1  其余的情况都是 0 
            // d(15) == 1111 bits = 3
            // 1 << bits - 1 == 0100
            // 1111
            // 0100 ^
            // 1011 
            d = d ^ (1 << (bits - 1));
            BitManipulate::showBinary(static_cast<int>(d));
            return d;
        }

    ```

## 参考
- [1] [Github](https://gthub.com/Ranbun/blogProjects/tree/main/BitManipulation "Github")