---
title: Cplusplus key words - extern
date: 2022-08-08 11:41:35
categories:
- cpp
tags:
- key words
---

<center><h2> <code>extern</code>关键字</h2></center>
## `Extern` 初解
- 关于`extern`关键字可以应用于全局变量、函数或模板声明。它指定符号具有外部链接性质。
  - 在非const 全局变量声明中， extern 指定变量或函数在另一个转换单元中定义。 extern必须在定义变量的所有文件中应用该变量。
  - const在变量声明中，它指定变量具有 external 链接。 extern必须应用于所有文件中的所有声明。 默认情况下， (全局 const 变量具有内部链接。)
  - extern "C" 指定函数在别处定义并使用 C 语言调用约定。 extern "C"修饰符也可能应用于块中的多个函数声明。
  - 在模板声明中， extern 指定模板已在其他位置实例化。 extern 告知编译器它可以重复使用另一个实例化，而不是在当前位置创建新实例。 有关此用法 extern的详细信息，请参阅 显式实例化。
  

## `Extern`的使用
- 小案例 
- `extern var` 


```C++ 
---
头文件
---
#ifndef EXTERN_TEST_H
#define EXTERN_TEST_H
/// extern 变量与函数
extern int extern_text_var;
extern int add(int x, int y);
#endif // EXTERN_TEST_H
```

```C++
---
cpp 文件
--- 
#include "KeyWordsExtern.h"
int extern_text_var = 100;
int add(int a,int b)
{
    return a + b;
}
```

```C++
---
使用导出变量
---
#include "KeyWordsExtern.h"
#include <iostream>

extern int extern_text_var;

void printExternVar()
{
    std::cout << "current: " << extern_text_var << std::endl;
    extern_text_var = 200;

    std::cout << "do update: " <<extern_text_var << std::endl;
}

```

```C++
---
main.cpp
---

#include <iostream>
#include "TestExtern.h"
#include "KeyWordsExtern.h"

using namespace std;

int main()
{
    printExternVar();
    std::cout<<"add result: "<<add(10, 30)<<std::endl;
    return 0;
}

```

```C++
---
输出
---
current: 100
do update: 200
add result: 40
```











