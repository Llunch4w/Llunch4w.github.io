---
title: Python菜鸟教程基础练习题
date: 2020-12-24 14:57:11
tags: 菜鸟教程 Python基础
categories: Python
---

{% centerquote %} ^ _ ^ {% endcenterquote %}
<!-- more -->

# 题目网址
菜鸟教程：[https://www.runoob.com/python3/python3-examples.html](https://www.runoob.com/python3/python3-examples.html)


# Hello World!
> 在屏幕输出 Hello World!

```python
# -*- coding: UTF-8 -*-

# Filename : helloworld.py
# author by : llunch4w

def helloWorld():
    print("Hello World!")

if __name__ == "__main__":
    helloWorld()
```

# 数字求和
> 通过用户输入两个数字，并计算两个数字之和

```python
def add():
    a = input()
    b = input()
    c = float(a) + float(b)
    print(f'{a} + {b} = {c}')
```

# 平方根
> 通过用户输入一个数字，并计算这个数字的平方根

```python
import cmath
def sqrt():
    a = input()
    res = cmath.sqrt(float(a))
    print(f'sqrt({a})= {res}')
```

# 二次方程
> 通过用户输入数字，并计算二次方程
> 二次方程式 ax**2 + bx + c = 0
> a、b、c 用户提供，为实数，a ≠ 0

```python
import cmath
def solv():
    a,b,c = [int(x) for x in input().split(' ')]
    d = cmath.sqrt(b**2 - 4*a*c)
    res1 = (-b + d)/(2*a)
    res2 = (-b - d)/(2*a)

    print(f'x1 = {res1},x2 = {res2}')    
```

# 计算三角形的面积
> 通过用户输入三角形三边长度，并计算三角形的面积，保留2位小数

```python
def solv():
    a,b,c = [float(x) for x in input().split(' ')]
    s = (a + b + c)/2
    area = (s*(s-a)*(s-b)*(s-c))**0.5
    print(f"area = {area:.2f}")
```

# 计算圆的面积
> 用户输入圆的半径，求圆的面积，保留6位小数

```python
def solv():
    r = float(input())
    area = cmath.pi*(r**2)
    print(f'area = {area:.6f}')
```

# 随机数生成
> 生成 0 ~ 9 之间的随机数

```python
def solv():
    while input():
        print(f'generated num = {random.randint(0,9)}')
```

# 摄氏温度转华氏温度
> 用户输入摄氏温度，计算华氏温度，保留1位小数

```python
def solv():
    cels = float(input())
    huas = (cels * 1.8) + 32
    print(f'摄氏温度 = {cels}时，华式温度约为{huas:.1f}')
```

# 交换变量
> 通过用户输入两个变量，并相互交换

```python
def solv():
    a,b = [int(x) for x in input().split(' ')]
    print(f'交换前：a = {a},b = {b}')
    a,b = b,a
    print(f'交换后：a = {a},b = {b}')
```

# if 语句
> 使用 if...elif...else 语句判断数字是正数、负数或零

```python
def solv():
    num = float(input())
    if num > 0 :
        print(f'{num} 是正数')
    elif num < 0:
        print(f'{num} 是负数')
    else:
        print(f'{num} 是零')
```

# 判断字符串是否为数字
> 通过创建自定义函数 is_number() 方法来判断字符串是否为数字

```python
def is_number(s):
    try:
        float(s)
        return True
    except Exception:
        pass

    try:
        import unicodedata
        unicodedata.numeric(s)
        return True
    except Exception:
        pass

    return False

def solv():
    while True:
        s = input()
        if is_number(s):
            print(f'{s} 是数字字符串')
        else:
            print(f'{s} 不是数字字符串')
```

> 注意：如果使用 str.isdigit() 和 str.isnumeric() 的话，是判断字符串中所有字符都是数字，而不能判断字符串的含义是否为数字。以上两种方式甚至不能判断小数和负数

# 判断奇数偶数
> 判断一个数字是否为奇数或偶数

```python
def solv():
    while True:
        s = input()
        if s.isdigit():
            num = int(s)
            print(f'{s} 是' + ('偶数' if num%2 == 0 else '奇数'))
        else:
            print(f'{s} 不是整数')
```

# 判断闰年
> 判断用户输入的年份是否为闰年
> 非整百年:能被4整除的为闰年
> 整百年:能被400整除的是闰年

```python
def solv():
    while True:
        year = int(input())
        if year % 100 == 0:
            if year % 400 == 0:
                print(f'{year}是闰年')
            else:
                print(f'{year}不是闰年')
        else:
            if year % 4 == 0:
                print(f'{year}是闰年')
            else:
                print(f'{year}不是闰年')
```

# 获取最大值函数
> 使用max()方法求最大值

```python
def solv():
    while True:
        numList = [float(x) for x in input().split(' ')]
        print(f'The max num of {numList} is {max(numList)}')
```

# 质数判断
> 一个大于1的自然数，除了1和它本身外，不能被其他自然数（质数）整除（2, 3, 5, 7等），换句话说就是该数除了1和它本身以外不再有其他的因数

```python
def isPrime(num):
    if num < 2:
        return False
    n = int(num**0.5)
    for i in range(2,n+1):
        if num % i == 0:
            print(f'{num//i} * {i} = {num}')
            return False
    return True

def solv():
    while True:
        num = int(input())
        if isPrime(num):
            print(f'{num}是质数')
        else:
            print(f'{num}不是质数')
```

# 输出指定范围内的素数
> 素数（prime number）又称质数，有无限个。除了1和它本身以外不再被其他的除数整除
> 用户输入范围，计算该范围内包含的所有素数

```python
# 利用筛法判断[0,highBound]之间的数是否为素数
def generate(highBound):
    resList = [True for i in range(highBound+1)]
    resList[0:2] = [False for i in range(2)]
    for i in range(2,int(highBound**0.5)+1):
        # 如果素数，采用筛法：筛去该数的所有倍数
        if resList[i]:
            num = 2 * i
            while num <= highBound:
                resList[num] = False
                num += i
        else:
            pass
    return resList
    
def solv():
    curHighBound = 1000
    primsTillHighBound = generate(curHighBound)
    while True:
        lowBound,highBound = [int(x) for x in input().split(' ')]
        if highBound > curHighBound:
            curHighBound = highBound
            primsTillHighBound = generate(curHighBound)
        res = [i for i in range(lowBound,highBound+1) if primsTillHighBound[i]]
        print(f'{lowBound}-{highBound}间的素数集合：{res}')
```

# 阶乘实例
> 整数的阶乘（英语：factorial）是所有小于及等于该数的正整数的积，0的阶乘为1。即：n!=1×2×3×...×n

```python
def solv():
    num = int(input())
    res = 1
    for i in range(2,num+1):
        res *= i
    print(f'{num}! = {res}')
```

# 九九乘法表
> 打印九九乘法表

```python
def solv():
    for j in range(1,10):
        for i in range(1,j+1):
            print(f'{i}x{j}={i*j}',end='\t')
        print()
```

# 斐波那契数列
> 斐波那契数列指的是这样一个数列 0, 1, 1, 2, 3, 5, 8, 13,特别指出：第0项是0，第1项是第一个1。从第三项开始，每一项都等于前两项之和
> 用户输入需要输出的斐波拉契数列项数，程序进行输出

```python
def solv():
    count = int(input())
    if count >= 1:
        print('0',end='')
    a,b = 0,1
    curCount = 1
    while curCount < count:
        print(f',{b}',end='')
        a,b = b,a+b
        curCount += 1
```

# 阿姆斯特朗数
> 如果一个n位正整数等于其各位数字的n次方之和,则称该数为阿姆斯特朗数。 例如1^3 + 5^3 + 3^3 = 153
> 编写代码检测用户输入的数字是否为阿姆斯特朗数

```python
def solv():
    while True:
        originNum = int(input())
        num = originNum
        n = len(str(num))
        res = 0
        while num != 0:
            res += pow(num%10,n)
            num //= 10
        if originNum == res:
            print(f'{originNum}是阿姆斯特朗数')
        else:
            print(f'{originNum}不是阿姆斯特朗数')
```

# 十进制转二进制、八进制、十六进制
> 实现十进制转二进制、八进制、十六进制

```python
def solv():
    while True:
        dec = int(input())
        print(f'十进制数：{dec}；二进制：{bin(dec)}；八进制：{oct(dec)}；十六进制：{hex(dec)}')
```

# ASCII码与字符相互转换
> 实现ASCII码与字符相互转换

```python
def solv():
    while True:
        print(f'1.字符转ASCII码\n2.ASCII码转字符')
        choice = int(input())
        if choice == 1:
            c = input()
            print(f'字符：{c} => ASCII码 = {ord(c)}')
        elif choice == 2:
            num = int(input())
            print(f'ASCII码：{num} => 字符 = {chr(num)}')
```

# 最大公约数
> 用户输入两个数，返回这两个数的最大公约数

```python
def gcd(a,b):
    return b if a%b == 0 else gcd(b,a%b)
    
def solv():
    while True:
        a,b = [int(x) for x in input().split(' ')]
        print(f'{a}和{b}的最大公约数为：{gcd(a,b)}')
```

# 最小公倍数算法
> 用户输入两个数，返回这两个数的最大公倍数

```python
def gcd(a,b):
    return b if a%b == 0 else gcd(b,a%b)
    
def solv():
    while True:
        a,b = [int(x) for x in input().split(' ')]
        print(f'{a}和{b}的最大公约数为：{a*b//gcd(a,b)}')
```

# 简单计算器实现
> 用户输入表达式，程序计算结果

```python
def solv():
    while True:
        expression = input().strip()
        charSta = []
        numSta = []
        i = 0
        while i < len(expression):
            if expression[i] in ['+','-','*','/','(',')']:
                if len(charSta) == 0:
                    pass
                elif charSta[-1] == ')':
                    charSta.pop()
                    while len(charSta) > 0:
                        token = charSta.pop()
                        if token == '(':
                            break
                        num1,num2 = numSta[-2:]
                        del numSta[-2:]
                        numSta.append(eval(str(num1) + token + str(num2)))
                elif charSta[-1] in ['*','/']:
                    while len(charSta) > 0 and charSta[-1] in ['*','/']:
                        token = charSta.pop()
                        num1,num2 = numSta[-2:]
                        del numSta[-2:]
                        numSta.append(eval(str(num1) + token + str(num2)))
                
                charSta.append(expression[i])
            elif expression[i].isdigit():
                num = 0
                frac = 0
                count = 0
                while i < len(expression) and expression[i].isdigit():
                    num = num*10 + int(expression[i])
                    i += 1
                if i < len(expression) and expression[i] == '.':
                    i += 1
                    while i < len(expression) and expression[i].isdigit():
                        frac = frac*10 + int(expression[i])
                        count += 1
                        i += 1
                num += frac/pow(10,count)
                numSta.append(num)
                i -= 1
            i += 1
        
        while len(charSta) != 0:
            if charSta[-1] == ')':
                charSta.pop()
                while len(charSta) > 0:
                    token = charSta.pop()
                    if token == '(':
                        break
                    num1,num2 = numSta[-2:]
                    del numSta[-2:]
                    numSta.append(eval(str(num1) + token + str(num2)))
            else:
                token = charSta.pop()
                num1,num2 = numSta[-2:]
                del numSta[-2:]
                numSta.append(eval(str(num1) + token + str(num2)))
        
        print(f'{expression} = {numSta[-1]}')
```

# 生成日历
> 生成指定日期（月份和年份）的日历

```python
import calendar
def solv():
    while True:
        yy = int(input())
        mm = int(input())
        print(calendar.month(yy,mm))
```

# 使用递归斐波那契数列
> 使用递归的方式来生成斐波那契数列

```python
def fib(n):
    if n <= 1:
        return n
    return fib(n-1) + fib(n-2)

def solv():
    count = int(input())
    for i in range(count):
        print(fib(i),end=' ')
```

# 文件 IO
> 演示Python基本的文件操作，包括 open，read，write

```python
def solv():
    file = input("请输入您要写入的文件名：")
    with open(file,'wt') as f:
        while True:
            s = input("请输入要写入的内容：")
            if len(s) > 0:
                f.write(s)
            else:
                break
        print('写入完成！')

    print('读取文件：')
    with open(file,'r') as f:
        for line in f.readlines():
            print(line)
```

# 约瑟夫生者死者小游戏
> 30 个人在一条船上，超载，需要 15 人下船。于是人们排成一队，排队的位置即为他们的编号。报数，从 1 开始，数到 9 的人下船。如此循环，直到船上仅剩 15 人为止，问都有哪些编号的人下船了呢？

```python
def solv():
    peopleNum = 30
    peoples = [True for i in range(peopleNum)]
    index = 0
    while peopleNum > 15:
        count = 0
        while True:
            if peoples[index]:
                count += 1
                if count == 9:break
            index = (index+1)%30
        peoples[index] = False
        print(f'{index+1}号下船了')
        peopleNum -= 1
```

# 五人分鱼
> A、B、C、D、E 五人在某天夜里合伙去捕鱼，到第二天凌晨时都疲惫不堪，于是各自找地方睡觉。
> 日上三杆，A 第一个醒来，他将鱼分为五份，把多余的一条鱼扔掉，拿走自己的一份。
> B 第二个醒来，也将鱼分为五份，把多余的一条鱼扔掉拿走自己的一份。 
> C、D、E依次醒来，也按同样的方法拿鱼。
> 问他们至少捕了多少条鱼?

```python
def solv():
    # finalFish是最后一个人分到的鱼数目
    finalFish,fish = 1,1
    flag = False
    while not flag:
        flag = True
        fish = finalFish
        for _ in range(5):
            if fish % 4 == 0:
                fish = fish // 4 * 5 + 1
            else:
                flag = False
                break
        finalFish += 1
    else:
        print(f'至少捕鱼{fish}条')
```

# 实现秒表功能
> 使用 time 模块来实现秒表功能

```python
import time
def solv():
    input()
    starttime = time.time()
    print("开始")
    try:
        while True:
            print(f"计时：{round(time.time()-starttime,0)}s")
            time.sleep(1)
    except KeyboardInterrupt:
        print("结束")
        print(f'总时间：{round(time.time()-starttime,2)}s')
```

# 数组翻转指定个数的元素
> 定义一个整型数组，并将指定个数的元素翻转到数组的尾部。
> 例如：(ar[], d, n) 将长度为 n 的 数组 arr 的前面 d 个元素翻转到数组尾部。

```python
def solv():
    n = int(input("请输入数组大小："))
    arr = list(range(1,n+1))
    print(f'已为您自动生成数组{arr}')
    d = int(input("请输入翻转个数："))
    arr.extend(arr[:d])
    del arr[:d]
    print(f'翻转后数组为：{arr}')
```

# 提取字符串中的 URL
> 给定一个字符串，里面包含 URL 地址，需要我们使用正则表达式来获取字符串的 URL

```python
import re
def solv():
    pattern = re.compile(r'https?://(?:[-./\w]|(?:%[\da-zA-Z]{2}))+')
    while True:
        s = input()
        urls = pattern.findall(s)
        print(f'urls:{urls}')
```

# 将字符串作为代码执行
> 给定一个字符串代码，然后使用 exec() 来执行字符串代码。
```txt
def factorial(num): 
    fact=1 
    for i in range(1,num+1): 
        fact = fact*i 
    return fact 
print(factorial(5)) 
```

```python
def solv():
    code = '''
def factorial(num): 
    fact=1 
    for i in range(1,num+1): 
        fact = fact*i 
    return fact 
print(factorial(5))
    '''
    exec(code)
```

# 按格式输出当前时间
> 按yyyy-mm-dd HH:MM:SS的格式输出当前时间

```python
import time
def solv():
    timeStamp = time.time()
    timeArray = time.localtime(timeStamp)
    timeStr = time.strftime("%Y-%m-%d %H:%M:%S",timeArray)
    print(f'当前时间为 {timeStr}')
```

# 获取几天前的时间
> 计算几天前并转换为指定格式

```python
import time
import datetime
def solv():
    threeDaysAgo = datetime.datetime.now() - datetime.timedelta(days=3)
    timeArray = threeDaysAgo.timetuple()
    timeStr = time.strftime("%Y-%m-%d %H:%M:%S",timeArray)
    print(f'当前时间为 {timeStr}')
```

# 转换时间显示格式
> 给定一个字符串的时间，让其以另一种时间显示格式显示

```python
import time
def solv():
    timeStr = "2019-5-10 23:40:00"
    timeArray = time.strptime(timeStr,"%Y-%m-%d %H:%M:%S")
    timeStr2 = time.strftime("%Y/%m/%d %H:%M:%S",timeArray)
    print(f'转换前时间字符串为：{timeStr}\n转换后时间字符串为：{timeStr2}')
```

# 将字符串时间转换为时间戳

```python
def solv():
    timeStr = "2019-5-10 23:40:00"
    timeArray = time.strptime(timeStr,"%Y-%m-%d %H:%M:%S")
    timeStamp = time.mktime(timeArray)
    print(f'转换前时间字符串为：{timeStr}\n转换为时间戳为：{timeStamp}')
```

# 对字典进行排序
> 根据key进行升序排列

```python
def solv():
    dictDemo = {1:9,2:-4,3:7,4:6,-3:9}
    print(f'排序前：dictDemo={dictDemo}')
    sortedDict = dict(sorted(dictDemo.items(),key=lambda kv:(kv[0],kv[1]),reverse=False))
    print(f'排序后：dictDemo={sortedDict}')
```

> 根据value进行降序排列

```python
def solv():
    dictDemo = {1:9,2:-4,3:7,4:6,-3:9}
    print(f'排序前：dictDemo={dictDemo}')
    sortedDict = dict(sorted(dictDemo.items(),key=lambda kv:(kv[1],kv[0]),reverse=True))
    print(f'排序后：dictDemo={sortedDict}')
```

> 字典列表排序

```python
def solv():
    lis = [{ "name" : "Taobao", "age" : 100},  
            { "name" : "Runoob", "age" : 7 }, 
            { "name" : "Google", "age" : 100 }, 
            { "name" : "Wiki" , "age" : 200 }] 
    # 先按 age 排序，再按 name 排序
    sortedList = sorted(lis,key=lambda d:(d['age'],d['name']))
    print(f'排序后的list：{sortedList}')
```

# 二分查找和顺序查找
> 生成随机序列，利用二分查找和顺序查找两种方式对元素进行查找

```python
# -*- coding: UTF-8 -*-

# Filename : search.py
# author by : llunch4w
import random
import time

def binarySearch(arr,left,right,key):
    while left <= right:
        mid = left + (right-left)//2
        if arr[mid] == key:
            return mid
        elif arr[mid] > key:
            right = mid - 1
        else:
            left = mid + 1
    return -1

def testBinarySearch():
    code = \
'''
nums = list(range(1000))
key = random.randint(0,1000)
binarySearch(nums,0,len(nums)-1,key)
'''
    return code

def seqSearch(arr,left,right,key):
    for i in range(left,right):
        if arr[i] == key:
            return i
    return -1

def testSeqSearch():
    code = \
'''
nums = list(range(1000))
random.shuffle(nums)
key = random.randint(0,1000)
seqSearch(nums,0,len(nums)-1,key)
'''
    return code

def test(code):
    # 将code执行1000次计算时间
    startTime = time.time()
    for _ in range(1000):
        exec(code)
    endTime = time.time()
    print(f'耗时：{(endTime-startTime)/1000}s')

def solv():
    # 生成0~999并将其打乱
    nums = list(range(1000))
    random.shuffle(nums)
    print(f'生成随机数组：{nums}')

    key = int(input("输入要查询的数："))

    # 二分搜索测试
    res = binarySearch(sorted(nums),0,len(nums)-1,key)
    print(f'找到元素{key}位于第{res+1}位') if res != -1 else \
    print(f'未找到元素{key}')

    # 顺序查找测试
    startTime = time.time()
    res = seqSearch(nums,0,len(nums)-1,key)
    endTime = time.time()
    print(f'找到元素{key}位于第{res+1}位') if res != -1 else \
    print(f'未找到元素{key}')

    # 耗时测试
    print('二分查找耗时测试：')
    test(testBinarySearch())
    print('顺序查找耗时测试：')
    test(testSeqSearch())

if __name__ == "__main__":
    solv()
```

# 排序
> 实现插入排序、快速排序、选择排序、冒泡排序、归并排序、堆排序、计数排序、希尔排序、拓扑排序9种排序方法

```python
# 插入排序
def insertSort(arr):
    for i in range(1,len(arr)):
        j = i - 1
        num = arr[i]
        while j >= 0 and arr[j] > num:
            arr[j+1] = arr[j]
            j -= 1
        arr[j+1] = num
```

```python
# 快速排序
def partion(arr,left,right):
    if right-left < 1:
        return
    i,j = left+1,right
    while i <= j:
        while i <= j and arr[j] > arr[left]:
            j -= 1
        if i > j:
            break
        while i <= j and arr[i] < arr[left]:
            i += 1
        if i > j:
            break
        arr[i],arr[j] = arr[j],arr[i]
    arr[left],arr[j] = arr[j],arr[left]
    partion(arr,left,j-1)
    partion(arr,j+1,right)

def quickSort(arr):
    partion(arr,0,len(arr)-1) 
```

```python
# 选择排序
def chooseSort(arr):
    for i in range(len(arr)):
        index = i
        for j in range(i+1,len(arr)):
            if arr[j] < arr[index]:
                index = j
        arr[i],arr[index] = arr[index],arr[i]
```

```python
# 冒泡排序
def bubbleSort(arr):
    flag = False
    count = 0
    while not flag:
        flag = True
        for i in range(0,len(arr)-1-count):
            if arr[i] > arr[i+1]:
                arr[i],arr[i+1] = arr[i+1],arr[i]
                flag = False
        count += 1
```

```python
# 归并排序
def merge(arr,left,right):
    if left == right:
        return
    res = []
    s1,s2 = left,left + (right-left)//2
    merge(arr,left,s2)
    merge(arr,s2+1,right)
    i,j = s1,s2+1
    while i <= s2 and j <= right:
        if arr[i] <= arr[j]:
            res.append(arr[i])
            i += 1
        else:
            res.append(arr[j])
            j += 1
    
    res.extend(arr[i:s2+1]) if i <= s2 else res.extend(arr[j:right+1])
    arr[left:right+1] = res[:]
```

```python
# 堆排序
def adjust(arr,i,n):
    while i < n:
        leftNode = 2 * i + 1
        rightNode = 2 * i + 2
        largestIndex = i
        if leftNode < n and arr[leftNode] > arr[largestIndex]:
            largestIndex = leftNode
        if rightNode < n and arr[rightNode] > arr[largestIndex]:
            largestIndex = rightNode

        if largestIndex != i:
            arr[i],arr[largestIndex] = arr[largestIndex],arr[i]
            # 继续向下调整
            i = largestIndex
        else:
            break           

def heapSort(arr):
    # 建立一个大根堆
    # unNodeIndex是第一个非叶子结点的下标
    unNodeIndex = (len(arr)-1)//2
    for i in range(unNodeIndex,-1,-1):
        adjust(arr,i,len(arr))

    for i in range(len(arr)-1,0,-1):
        # 将最大的元素（根元素）放入最后一个，然后调整剩余元素组成的堆，得到新的大根堆
        arr[i],arr[0] = arr[0],arr[i]
        adjust(arr,0,i)
```

```python
# 希尔排序
def shellSort(arr):
    n = len(arr)
    gap = n // 2
    while gap > 0:
        for i in range(gap,n):
            j = i - gap
            num = arr[i]
            while j >= 0 and arr[j] > num:
                arr[j+gap] = arr[j]
                j -= gap
            arr[j+gap] = num
        gap //= 2
```

```python
# 计数排序
def countSort(arr):
    s,e = min(arr),max(arr)
    countList = [0] * (e - s + 1)
    for item in arr:
        countList[item] += 1
    ans = []
    for i in range(len(countList)):
        while countList[i] > 0:
            ans.append(i)
            countList[i] -= 1
    arr[:] = ans[:]
```