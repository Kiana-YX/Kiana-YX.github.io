---
layout:     post
title:    C++
date:       2021-11-29
author:     Kiana
header-img: img/post-bg-coffee.jpg
catalog: true
tags:
    - 学习资料
---
# c++

## [vscode运行](https://zhuanlan.zhihu.com/p/394595507)

## 基础进阶

### const那些事

```c++
const char * a; //指向const对象的指针或者说指向常量的指针。
char const * a; //同上
char * const a; //指向类型对象的const指针。或者说常指针、const指针。
const char * const a; //指向const对象的const指针。
```

如果*const*位于`*`的左侧，则const就是用来修饰指针所指向的变量，即指针指向为常量；如果const位于`*`的右侧，*const*就是修饰指针本身，即指针本身是常量。

