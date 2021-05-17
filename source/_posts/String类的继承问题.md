---
title: String类的继承问题
date: 2020-08-19 23:36:34
tags:
    - Java基础
    - String类
    - final修饰符
categories: Java面试
---

{% centerquote %} ^ _ ^ {% endcenterquote %}
<!-- more -->

# Q & A

> **String类能被继承吗，为什么?**

不能，String类是由final修饰的类，因此不可被继承。
String类被设计成由final修饰的类，原因主要有两个，一是设计，而是效率：
- 设计：final类中的所有方法都是final类型，这样将方法锁定，可以防止任何继承类修改它的意义和实现；
- 效率：final方法使用内嵌机制，可以提升效率。但这样的机制在方法体很大时可能会耗费更多时间反而使效率降低，所以现在使用final方法一般只取决于设计原因。


> **String类可以被更改吗，为什么？**

不能，String类中用于保存字符串值的成员变量是由final修饰的char型数组。数组被final修饰后，数组本身不可被改变，但其数组元素本身可以被改变，因此，char数组是不能被扩容的。由于这个char数组也被private修饰，且String没有提供修改这个数组成员的方法，因此程序员也不能操纵String的每个字符。
String类被设计为不可变类的原因主要有两个，一是效率，二是安全：
- 效率：String类有一个成员变量hashcode，用来判断两个字符串值是否相等。String值不变，hashcode值就不变，缓存hashcode值才有意义；
- 安全：String类常被作为网络连接，文件操作等参数类型，倘若改变，会出现意想不到的结果。


# final修饰的作用
- **修饰基本类型变量**：类型的值不可改变
- **修饰对象类型变量**：指向对象的引用不可改变指向，对象本身可以改变
- **修饰方法**：使方法不可被重写
- **修饰类**：类不可被继承


# 编程测试
```java
public void equalTest() {
    System.out.printf("\nequalTest\n");
    String s1 = "abc";
    String s2 = "abc";
    String s3 = new String("abc");
    System.out.println("s1 == s2? " + (s1==s2));    \\ true
    System.out.println("s1 == s3? " + (s1==s3));    \\ false
    System.out.println("s1 equals s2? " + s1.equals(s2));   \\true
    System.out.println("s1 equals s3? " + s1.equals(s3));   \\true
}
public void concatEqualTest() {
    System.out.printf("\nconcatEqualTest\n");
    String s1 = "Hello Word";
    String s2 = "Hello " + "Word";
    System.out.println("s1 == s2? " + (s1==s2));    \\true
    System.out.println("s1 equals s2? " + s1.equals(s2));   \\true
    String hello = "Hello ";
    String s3 = hello + "Word";
    System.out.println("s1 == s3? " + (s1==s3));    \\false
    System.out.println("s1 equals s3? " + s1.equals(s3));   \\true
}
```

