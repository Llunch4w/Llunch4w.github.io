---
title: 'String,StringBuilder,StringBuffer'
date: 2020-08-20 06:57:07
tags: 
    - Java基础
    - String类
categories: Java面试
---

{% centerquote %} ^ _ ^ {% endcenterquote %}
<!-- more -->

# Q & A
> **String，StringBuffer，StringBuilder的区别**

- **可变性**：String 一旦创建不可变，如果修改即创建新的对象，StringBuffer 和 StringBuilder 可变，修改之后引用不变；
- **拼接效率**：String 对象直接拼接效率高，但是如果执行的是间接拼接，效率很低，而 StringBuffer 和 StringBuilder 的效率更高，同时 StringBuilder 的效率高于 StringBuffer；
- **线程安全**：StringBuffer 的方法是线程安全的，StringBuilder 是线程不安全的，在考虑线程安全的情况下，应该使用 StringBuffer。


# 编程测试
**可变性测试**
```
String s = "Hello";
String s1 = s;
StringBuilder strBuilder = new StringBuilder("Hello");
StringBuffer strBuffer = new StringBuffer("Hello");
s += " World";
strBuilder.append(" Word");
strBuffer.append(" Word");
System.out.println("s = " + s); \\s = Hello World
System.out.println("s1 = " + s1); \\s1 = Hello
System.out.println("strBuilder = " + strBuilder); \\strBuilder = Hello Word
System.out.println("strBuffer = " + strBuffer); \\strBuffer = Hello Word
```
s和s1的值不相等说明String是不可变的。因为如果s += xxx是为s加上后缀的话，那么指向s存储地址的s1应该也是会改变的。而事实是，s += xxx中，虚拟机先会实例一个StringBuilder，将s中的字符和新添加的xxx按顺序加入StringBuilder，然后调用StringBuilder的toString方法将其转换为一个新的String实例对象，并将s（引用地址）指向这个新的对象。

**效率测试**
```
final int N = 5000;
String s = new String();
StringBuilder builder = new StringBuilder();
StringBuffer buffer = new StringBuffer();
long startTime,endTime;
// String用时统计
startTime = System.currentTimeMillis();
for(int i = 0;i < N;i++) {
    s += i;
}
endTime = System.currentTimeMillis();
System.out.printf("String%d次拼接操作耗时%d(ms)\n",N,endTime-startTime);
// StringBuilder用时统计
startTime = System.currentTimeMillis();
for(int i = 0;i < N;i++) {
    builder.append(i);
}
endTime = System.currentTimeMillis();
System.out.printf("StringBuilder%d次拼接操作耗时%d(ms)\n",N,endTime-startTime);
// StringBuffer用时统计
startTime = System.currentTimeMillis();
for(int i = 0;i < N;i++) {
    buffer.append(i);
}
endTime = System.currentTimeMillis();
System.out.printf("StringBuffer%d次拼接操作耗时%d(ms)\n",N,endTime-startTime);
```
结果：
String5000次拼接操作耗时51(ms)
StringBuilder5000次拼接操作耗时1(ms)
StringBuffer5000次拼接操作耗时2(ms)

**线程安全**
```
final int N = 500;
final int threadNum = 10;
//StringBuilder builder = new StringBuilder();
StringBuffer buffer = new StringBuffer();
for(int i = 0;i < threadNum;i++) {
    new Thread(new Runnable() {
        @Override
        public void run() {
            for(int i = 0;i < N;i++) {
				//builder.append('a');
                buffer.append('a');
            }
        }
    }).start();
}
try {
    Thread.sleep(1000);
	//System.out.println("builder_length = " + builder.length());
    System.out.println("buffer_length = " + buffer.length());
}catch(Exception e) {
    e.printStackTrace();
}
```
- 如果是用builder来运行的话，builder_length <= 5000 且可能出现`ArrayIndexOutOfBoundsException`异常。这是因为StringBuilder存在扩容机制，但append函数却没有用`synchronized`修饰，导致出现：多个线程访问StringBuilder的最后一个空闲名额时，都觉得不用扩容，但实际名额已经不够了；
- 如果是用buffer来运行的话，builder_length == 5000 是恒定的，StringBuffer通过给其涉及修改的成员函数增加`synchronized`修饰符来保证操作的互斥性。