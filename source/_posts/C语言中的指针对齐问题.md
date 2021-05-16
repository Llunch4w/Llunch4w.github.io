---
title: C语言中的指针对齐问题
date: 2020-09-13 09:06:41
tags: C语言
categories: C语言
---

{% centerquote %} ^ _ ^ {% endcenterquote %}

<!-- more -->

# 查看占用内存大小的方式
```
sizeof(类型/对象);
```

# 基础类型在内存中的大小
|类型|所占大小(字节)|
|---|---|
|int|4|
|long|4|
|short|2|
|char|1|
|指针|8|
|long long|8|
|float|4|
|double|8|


# 结构体中的内存对齐
**就近原则**
> 根据最近的下一个变量的大小进行对齐，最后一个变量则是根据它的上一个
```
例1：
char a[3];
int b;
因为a最邻近的b是int类型，因此a按照int类型的大小（4字节）进行对齐，a和它之前的所有成员变量所占内存之和应该为4的倍数

例2：
char a[3];
long long b;
因为a最邻近的b是int类型，因此a按照long long类型的大小（8字节）进行对齐，a和它之前的所有成员变量所占内存之和应该为8的倍数

例3：
char a[3];
char b[8];
因为a最邻近的b是char类型，虽然它是一个8字节的数组，a仍然是按照char类型的大小(1字节)进行对齐，a和它之前的所有成员变量所占内存之和应该为1的倍数

例4：
int a;
char b;
虽然a邻近的b是char类型，但由于a本身为int类型，自身类型大小大于b，因此仍然是按int类型大小(4字节)进行对齐
```

**练习**
```
struct Demo1{
    char c;
	long long b;
	char name[7];
	char id[3];	
	long long a;
};
// 占用内存大小为40，：8 + 8 + 7 + 3 + 6 + 8 = 40

struct Demo2{
    long long a;
    char c;
    char c1[2];
    char c2[14];
    int b;
};
// 所占内存大小为 32: 8 + 1 + 2 + 14 + 3 + 4 = 32

struct Demo3{
    char a[3];
    char b[8];
};
// 所占内存大小为 11：3 + 8 = 11;

struct Demo4{
    char a[3];
    int b;
    char c;
    long long d;
}
// 所占内存大小为24 ：3 + 1 + 4 + 1 + 7 + 8 = 24

struct Demo5{
    long long a;
    int b;
    char c;
};
// 所占内存大小为16 ：8 + 4 + 4 = 16
```

# 空结构体（类）
如果是一个空结构体或类，则其所占内存大小为1。

**测试**
```
class Empty{
	
};
```

# 虚函数
- 虚函数的原理是系统初始化一个虚函数表，在类中通过一个指向虚函数表的指针来定位虚函数，因此虽然普通成员函数不占用空间，但申明虚函数会占用一个指针的空间，即8字节；
- 但无论一个类中有多少个虚函数，这些虚函数都会存放在一个虚函数表中，即只有一个虚函数指针，因此总共只会占用8字节

**例子**
```
class Demo1{
    char c;
	long long b;
	char name[7];
    virtual void func1();
    virtual void func2();
};
// 占用内存大小为 32: 8 + 8 + 7 + 1 + 8 = 32
// 其中7变成8是根据虚函数指针的大小，而不是b大小，Demo2可以证明

class Demo2{
    char c;
	int b;
	char name[3];
    virtual void func1();
    virtual void func2();
};
// 占用内存大小为 24: 4 + 4 + 3 + 5 + 8 = 24
// 且无论虚函数写在什么位置，其进行内存计算的位置都是Demo2这样，Demo3可以证明

class Demo2{
    virtual void func1();
    virtual void func2();
    char c;
    int b;
    char name[3];   
};
```

# 继承
> 1. 继承父类，相当于继承了父类的所有成员，不管是否可见
> 2. 根据1，可知子类的大小为继承的父类大小之和，且会根据父类的最后一个元素进行对齐

**例子**
```
class Base{
	char arr[10];
	int b;
	virtual void func();
};
// Base：10 + 2 + 4 + 8 = 24
class A:Base{
	char a[3];
};
// A ：24 + 3 + 5 = 32，因为最后一个虚指针是8字节，因此a要跟它对齐
class B:Base{
	char b[6];
};
// B: 24 + 6 +2 = 32
class C:A,B{
	
};
// C：32 + 32 = 64
```